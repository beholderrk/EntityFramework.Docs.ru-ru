---
title: Перенос приложений из EF6 в основные EF – перенос модель на основе кода
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052954"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Перенос EF6 модели на основе кода EF ядра

Если вы прочитали все предупреждения, и вы готовы к порту, Вот некоторые рекомендации, которые помогут приступить к работе.

## <a name="install-ef-core-nuget-packages"></a>Установить пакеты EF Core NuGet

Для использования основных EF, установите пакет NuGet для поставщика базы данных, которые вы хотите использовать. Например, если для различных версий SQL Server, необходимо установить `Microsoft.EntityFrameworkCore.SqlServer`. В разделе [поставщиков базы данных](../../core/providers/index.md) подробные сведения.

Если вы планируете использовать миграции, то необходимо также установить `Microsoft.EntityFrameworkCore.Tools` пакет.

Это нормально оставить пакет EF6 NuGet (EntityFramework) установлен, как ядро EF и EF6 может быть используется side-by-side в одном приложении. Тем не менее если вы не собираетесь использовать EF6 в любой области приложения, затем при удалении пакета поможет выдают ошибки компиляции на элементы кода, требующие внимания.

## <a name="swap-namespaces"></a>Поменять местами пространства имен

Большинство интерфейсов API, которые используются в EF6 находятся в `System.Data.Entity` пространства имен (и связанные вложенные пространства имен). Первое изменение кода — для замены для `Microsoft.EntityFrameworkCore` пространства имен. Следует начинать с помощью файла кода в контексте производной и затем составить оттуда адресации ошибки компиляции, как только они происходят.

## <a name="context-configuration-connection-etc"></a>Контекст конфигурации (подключение и т. д.)

Как описано в [обеспечить EF основная будет работа для вашего приложения](ensure-requirements.md), ядро EF имеет меньше magic вокруг обнаружение для подключения к базе данных. Необходимо переопределить `OnConfiguring` метод в производном контекста и установить подключение к базе данных с помощью конкретного API поставщика базы данных.

Большинство приложений EF6 Сохранение строки подключения в приложениях `App/Web.config` файла. В ядре EF, чтение этой строки подключения с помощью `ConfigurationManager` API. Может потребоваться добавить ссылку на `System.Configuration` framework сборки, чтобы иметь возможность использовать этот API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>Обновите ваш код

На этом этапе это вопрос устранения ошибок компиляции и проверки кода, чтобы просмотреть, если вы влияют изменения в работе.

## <a name="existing-migrations"></a>Существующие миграции

Действительно не подходящий способ переноса существующих EF6 миграций EF ядра.

Если это возможно лучше предположить, что все предыдущие миграцию с EF6 были применены к базе данных, а затем с помощью EF Core миграции схемы, из начала. Чтобы сделать это, используйте `Add-Migration` команду, чтобы добавить к миграции после модели, перенесена в EF Core. Затем нужно удалить весь код из `Up` и `Down` методы переноса формирования шаблонов. Последующей миграции будет сравнить модели при этой первоначальной миграции был формирования шаблонов.

## <a name="test-the-port"></a>Проверить порт

Только потому, что компилирует приложения, не означает, что он успешно перенесена на EF Core. Необходимо для проверки всех областях приложения, чтобы убедиться, что никакие изменения в поведении отрицательно повлиять приложения.
