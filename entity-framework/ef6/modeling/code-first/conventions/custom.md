---
title: Пользовательский код соглашения First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
caps.latest.revision: 3
ms.openlocfilehash: 24d6f1bd5eb2ff8be59b9eedd1c4156709fa42fb
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122609"
---
# <a name="custom-code-first-conventions"></a>Первый соглашения о написании пользовательского кода
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

При использовании Code First модели вычисляется на основе классов с помощью набора соглашений. Значение по умолчанию [первый соглашения о коде](~/ef6/modeling/code-first/conventions/built-in.md) определить, как и в которых свойство становится первичный ключ сущности, имя таблицы, сопоставляет сущности и какие точность и масштаб столбца decimal имеет по умолчанию.

Иногда эти соглашения по умолчанию не идеально подходят для вашей модели, и вам нужно решить их, настроив множество отдельных сущностей, с помощью заметок к данным или Fluent API. Пользовательские соглашения о коде первый позволяют определять собственные соглашения, которые предоставляют значения конфигурации по умолчанию для модели. В этом пошаговом руководстве мы рассмотрим различные типы соглашения об именовании и создание каждого из них.


## <a name="model-based-conventions"></a>Правила на основе моделей

В этой статье описываются API-интерфейса DbModelBuilder для соглашения об именовании. Этот API должно быть достаточно для создания большинства соглашения об именовании. Тем не менее есть также возможность создавать модели с использованием соглашений - соглашения, которые управляют окончательная версия модели, после ее создания — для обработки более сложных сценариев. Дополнительные сведения см. в разделе [модели с использованием соглашений](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Наша модель

Начнем с определения простой модели, который можно использовать с помощью наших соглашений. Добавьте следующие классы в проект.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Знакомство с соглашения об именовании

Давайте напишем соглашение, которое настраивает любое свойство с именем ключа в качестве первичного ключа для своего типа сущности.

Соглашения о включены построитель модели, к которому можно получить путем переопределения OnModelCreating в контексте. Обновите класс ProductContext следующим образом:

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

Теперь, любое свойство в нашей модели, с именем ключа будет настроен в качестве первичного ключа сущности, независимо от его частью.

Можно также сделать соглашения более точным, фильтруя данные по тип свойства, который будет использоваться для настройки:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Будут настроены все свойства, называемые ключ основного ключа их сущности, но только в том случае, если это целое число.

Интересной особенностью метод IsKey является его аддитивный характер. То есть, если вы вызываете IsKey на несколько свойств, и все они станут частью составного ключа. Единственным условием для этого является то, что при указании нескольких свойств для ключа также необходимо указать, заказ для этих свойств. Это можно сделать, вызвав HasColumnOrder, аналогичный метод:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Этот код будет настроить типы в нашей модели иметь составной ключ, состоящий из столбца Key int и строкового имени столбца. Если просмотреть модель в конструкторе оно выглядело следующим образом:

![compositeKey](~/ef6/media/compositekey.png)

Другой пример соглашений свойство — настроить все свойства даты и времени в моей модели для сопоставления с типом datetime2 в SQL Server вместо даты и времени. Это можно сделать с помощью следующих:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Соглашение о классах

Определение соглашения еще один способ — использовать соглашение о класс для инкапсуляции соглашению об. При использовании класса соглашение о создании типа, наследуемого от класса соглашение в пространстве имен System.Data.Entity.ModelConfiguration.Conventions.

Мы можем создать класс соглашение об с datetime2 соглашение, которое мы продемонстрировали ранее, сделав следующее:

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Чтобы сообщить EF, чтобы использовать это соглашение, добавить его в коллекцию соглашений в OnModelCreating, что если вы выполняли с пошаговым руководством, будет выглядеть следующим образом:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Как вы видите, мы добавим экземпляр нашей соглашение в коллекцию соглашений. Наследование от соглашение предоставляет удобный способ группировки и совместное использование соглашения несколькими командами или проектов. Например, можно создать общий набор соглашений, что все организации проекты, использующие библиотеки классов.

 

## <a name="custom-attributes"></a>Настраиваемые атрибуты

Чтобы включить новые атрибуты для использования при настройке модели — еще одно отличное применение соглашений. Чтобы проиллюстрировать это, давайте создадим атрибут, который можно использовать для пометки свойств строки как Юникод.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Теперь давайте создадим соглашение о этот атрибут применяется к нашей модели:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Данное соглашение мы можно добавить атрибут NonUnicode к любому из наших свойства строки, что означает столбец в базе данных будет храниться как varchar, а не nvarchar.

Следует отметить о это соглашение о том, что если поместить атрибут NonUnicode на ничего, кроме строковое свойство, а затем он вызовет исключение. Это связано с IsUnicode невозможно настроить для любого типа, кроме строки. В этом случае после этого можно выполнять соглашению об более точным, позволяя отфильтровывает все, что не является строкой.

Хотя выше соглашение работает для определения настраиваемых атрибутов нет другого API, который может быть гораздо проще в использовании, особенно если вы хотите использовать свойства класса атрибутов.

В этом примере мы собираемся обновить наш атрибут и измените его атрибут IsUnicode, чтобы он выглядел следующим образом:

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Получив это, можно установить логическое значение для наших атрибут, чтобы понять в соответствии с соглашением, ли свойство должно быть Юникода. Это можно делать в соглашение, которое уже имеется, обратившись к ClrProperty класса конфигурации следующим образом:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Это достаточно просто, но есть более краткий способ сделать это с помощью Having метод соглашений API. Having, метод имеет параметр типа Func&lt;PropertyInfo, T&gt; , принимающий класс PropertyInfo так же, как Where метод, но должен вернуть объект. Если возвращаемый объект имеет значение null, то свойство не будет настроен, что может сделать возможной фильтрацию свойств с ним так же, как Where, но он отличается тем, что он также помещается возвращаемый объект и передать его в метод Configure. Это работает следующим образом:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Пользовательские атрибуты не единственная причина для использования Having метод, полезно в любом месте, необходимо делать выводы о том, что фильтрации по при настройке типами или свойствами.

 

## <a name="configuring-types"></a>Настройка типов

До сих всех соглашений были для свойств, но существует еще одна область API соглашений для настройки типов в модели. Процесс выполняется аналогично соглашения об именах, который мы видели в данный момент, но параметры настройки внутри будет объекты вместо свойства уровня.

Среди прочего, соглашения об уровне типов может быть очень полезно при изменении таблицы соглашение об именовании, сопоставляемый существующую схему, отличную от EF по умолчанию или создать новую базу данных с различных соглашения об именовании. Для этого необходимо сначала метод, который может принимать TypeInfo типа в нашей модели и возвращать имя таблицы для этого типа, которое должно быть:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Этот метод принимает значение типа и возвращает строку, которая используется нижний регистр с символами подчеркивания, а не CamelCase. В нашей модели это означает, что таблица с именем продукта будет сопоставлено класс ProductCategory\_категории вместо ProductCategories.

После получения этого метода можно обращаться к нему в соглашении следующим образом:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Это соглашение настраивает каждый тип в нашей модели, которое будет сопоставлено имя таблицы, которое возвращается из нашего метода GetTableName. Это соглашение эквивалентен вызову totable-метод для каждой сущности в модели с помощью Fluent API.

Следует заметить, об этом, что при вызове totable-EF будет иметь строку, указанных в качестве точное имя таблицы, без каких-либо преобразования во множественную форму, что обычно делается при определении имен таблиц. Вот почему имя таблицы из наших соглашение — продукт\_категории вместо продукта\_категории. Чтобы устранить, в нашем соглашении, делая вызов к службе преобразования во множественную форму, сами.

В следующем коде мы будем использовать [разрешение зависимостей](~/ef6/fundamentals/configuring/dependency-resolution.md) компонент, добавленный в EF6, обращение к службе преобразования во множественную форму, приходилось использовать EF и pluralize наших имя таблицы.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> Универсальная версия GetService является методом расширения в пространстве имен System.Data.Entity.Infrastructure.DependencyResolution, необходимо добавить с помощью инструкции к контексту, чтобы их использовать.

### <a name="totable-and-inheritance"></a>Totable- и наследование

Еще один важный аспект ToTable том, что если вы явным образом сопоставить тип для данной таблицы, а затем его можно изменить стратегию сопоставления, который будет использовать EF. Если вы вызываете метод ToTable для каждого типа в иерархии наследования, передавая имя типа как имя таблицы, как это делалось выше, затем мы изменим стратегию сопоставления по умолчанию таблица на иерархию (TPH) таблица на тип (TPT). Лучший способ описания — whith конкретный пример:

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

По умолчанию сотрудника и руководителя сопоставляются с той же таблицей (сотрудники) в базе данных. Таблица будет содержать сотрудники и менеджеры со столбцом дискриминатора, который сообщит, какой тип экземпляра хранится в каждой строке. Такое сопоставление TPH, так как для одной таблицы для иерархии. Тем не менее если вызвать метод ToTable в обоих classe затем каждого типа будет вместо этого можно сопоставить с собственную таблицу, а также называется TPT, так как каждый тип имеет собственную таблицу.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Приведенный выше код будет сопоставлен структуру таблицы, которая выглядит следующим образом:

![tptExample](~/ef6/media/tptexample.jpg)

Можно избежать этого и поддерживать TPH сопоставление по умолчанию, несколькими способами:

1.  Вызовите метод ToTable с тем же именем таблицы, для каждого типа в иерархии.
2.  Вызовите метод ToTable только на базовый класс для иерархии, в нашем примере, которая была бы сотрудника.

 

## <a name="execution-order"></a>Порядок выполнения

Соглашения о работают в виде последнего wins, так же, как Fluent API. Это означает, что при написании два соглашения, настройте один и тот же параметр этого свойства, а затем последнее из них для выполнения wins. Например в приведенном ниже коде Максимальная длина для всех строк имеет значение 500, но мы затем настройте все свойства «Name» в модели, чтобы максимальная длина 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Так как соглашение об установке максимальной длины до 250 после того, который задает все строки до 500, все свойства «Name» в нашей модели будет иметь MaxLength 250 при других строк, например, описания, составит 500. Подобное использование соглашения означает, что может предоставить общие соглашения для типов или свойств в модели и затем переопределить их подмножества, которые отличаются.

Fluent API и заметки к данным может также использоваться для переопределения соглашения в конкретных случаях. В примере выше если бы мы использовали Fluent API для задания Максимальная длина свойства затем было бы поместить ее до или после соглашение, так как более конкретный Fluent API бьет соглашение по дополнительной конфигурации.

 

## <a name="built-in-conventions"></a>Встроенные соглашения

Так как соглашения об именовании может повлиять на соглашения Code First по умолчанию, может быть полезно добавить соглашения для выполнения до или после другим соглашением. Для этого можно использовать методы AddBefore и AddAfter коллекции соглашения в вашем производном DbContext. Добавить приведенный ниже класс соглашение, созданную ранее будет выполняться перед встроенные в соглашении обнаруживать ключ.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Это будет наиболее полезны при добавлении соглашения, которые должны выполняться до или после встроенных правил, список встроенных соглашений можно найти здесь: [пространства имен System.Data.Entity.ModelConfiguration.Conventions](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

Можно также удалить соглашения, которые необходимо применить к модели. Чтобы удалить соглашение, используйте метод Remove. Ниже приведен пример удаления PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
