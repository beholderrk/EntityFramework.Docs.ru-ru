---
title: EF6 Database First.
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
caps.latest.revision: 3
ms.openlocfilehash: 17bba5fe9883a1bee0f8b9624dfa35da889e6005
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121926"
---
# <a name="database-first"></a>Сначала базы данных
В этом пошаговом руководстве видео и пошаговые познакомят вас с первой базы данных разработки, использующий Entity Framework. Во-первых, базы данных дает возможность Реконструировать модель из существующей базы данных. Модель хранится в EDMX-файла (расширение EDMX) и их можно просмотреть и изменить в конструкторе Entity Framework. Классы, которые взаимодействуют с в приложении автоматически создаются из файла EDMX.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео предоставляет общие сведения о первой базы данных разработки, использующий Entity Framework. Во-первых, базы данных дает возможность Реконструировать модель из существующей базы данных. Модель хранится в EDMX-файла (расширение EDMX) и их можно просмотреть и изменить в конструкторе Entity Framework. Классы, которые взаимодействуют с в приложении автоматически создаются из файла EDMX.

**Представляет**: [Роуэн Миллер (Rowan Miller)](http://romiller.com/)

**Видео**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения этого пошагового руководства необходимо иметь по крайней мере Visual Studio 2010 или Visual Studio 2012.

Если вы используете Visual Studio 2010, также необходимо будет иметь [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) установлен.

 

## <a name="1-create-an-existing-database"></a>1. Создание базы данных

Обычно при ориентировании существующей базы данных, он будет уже создан, но для этого пошагового руководства необходимо создать базу данных для доступа к.

Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:

-   Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.
-   Если вы используете Visual Studio 2012, а затем вы создадите [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) базы данных.

 

Перейдем дальше и создать базу данных.

-   Открытие Visual Studio
-   **Представление —&gt; обозревателя серверов**
-   Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**
-   Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам потребуется выбрать в качестве источника данных Microsoft SQL Server

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   Подключение к LocalDB или SQL Express, в зависимости от того, какой из них вы установили и введите **DatabaseFirst.Blogging** имя базы данных

    ![SqlExpressConnectionDF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDBConnectionDF](~/ef6/media/localdbconnectiondf.png)

-   Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   Новой базы данных будут отображаться в обозревателе сервера щелкните его правой кнопкой мыши и выберите **новый запрос**
-   Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a>2. Создание приложения

Для простоты мы создадим простое консольное приложение, использующее первой базы данных для выполнения доступа к данным:

-   Открытие Visual Studio
-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Windows** в меню слева и **консольного приложения**
-   Введите **DatabaseFirstSample** как имя
-   Нажмите кнопку **ОК**

 

## <a name="3-reverse-engineer-model"></a>3. Реконструирование модели

Мы собираемся использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.

-   **Проект -&gt; добавить новый элемент...**
-   Выберите **данных** в меню слева и затем **модель EDM ADO.NET**
-   Введите **BloggingModel** имя и нажмите кнопку **ОК**
-   Это откроет **мастер моделей EDM**
-   Выберите **создать из базы данных** и нажмите кнопку **Далее**

    ![WizardStep1](~/ef6/media/wizardstep1.png)

-   Выберите соединение с базой данных, созданной в первом разделе, введите **BloggingContext** как имя строки подключения и нажмите кнопку **Далее**

    ![WizardStep2](~/ef6/media/wizardstep2.png)

-   Установите флажок рядом с «Таблицы», чтобы импортировать все таблицы и нажмите кнопку «Готово»

    ![WizardStep3](~/ef6/media/wizardstep3.png)

 

После завершения процесса реконструирования новой модели добавлен в проект и открывается для просмотра в конструкторе Entity Framework. Файл App.config также был добавлен в проект со сведениями о подключении для базы данных.

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Дополнительные действия в Visual Studio 2010

Если вы работаете в Visual Studio 2010, существуют некоторые дополнительные действия, которые необходимо выполнить для обновления до последней версии платформы Entity Framework. Обновление важно, так как он предоставляет вам доступ к Улучшенная поверхность API, то есть гораздо проще в использовании, а также последние исправления ошибок.

Во-первых, нам нужно получить последнюю версию Entity Framework из NuGet.

-   **Проект —&gt; управление пакетами NuGet... ** 
     *При отсутствии **управление пакетами NuGet... ** следует установить параметр [последнюю версию NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Выберите **Online** вкладку
-   Выберите **EntityFramework** пакета
-   Нажмите кнопку **установки**

Далее нам нужно переключить нашей модели, для которого создается код, который использует API DbContext, который появился в более поздних версиях Entity Framework.

-   Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите **добавить элемент формирования кода...**
-   Выберите **шаблоны в Интернете** из меню слева и выполните поиск **DbContext**
-   Выберите EF **5.x генератор DbContext для C\#**, введите **BloggingModel** имя и нажмите кнопку **добавить**

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Чтение и запись данных

Теперь, когда у нас есть модель, настала пора использовать ее для доступа к некоторые данные. Классы мы будем использовать для доступа к данным автоматически создаются зависимости в EDMX-файл.

*Этот снимок экрана из Visual Studio 2012, если вы используете Visual Studio 2010 BloggingModel.tt и BloggingModel.Context.tt файлов будет непосредственно в проекте, а не вложен в узел EDMX-файла.*

![GeneratedClassesDF](~/ef6/media/generatedclassesdf.png)

 

Реализуйте метод Main в файле Program.cs, как показано ниже. Этот код создает новый экземпляр класса наш контекст, а затем использует его для вставки нового блога. Затем он использует запрос LINQ для извлечения из базы данных, представлены в алфавитном порядке по названию все блоги.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Теперь можно запустить приложение и протестировать его.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. Изменения базы данных

Теперь пора внести некоторые изменения в схему нашей базы данных, когда мы внести эти изменения, необходимо также обновить нашей модели, чтобы отразить эти изменения.

Первым делом для внесения некоторых изменений в схему базы данных. Мы собираемся добавить таблицу пользователей в схему.

-   Щелкните правой кнопкой мыши **DatabaseFirst.Blogging** базы данных в обозревателе сервера и выберите **новый запрос**
-   Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Теперь, когда схема обновляется, пришло время для обновления модели с помощью этих изменений.

-   Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите «Обновить модель из базы данных...», это приведет к запуску мастера обновления
-   На вкладке "Добавить" мастер обновления проверки рядом с таблицами это означает, что мы хотим добавить новые таблицы из схемы.
    *На вкладке обновления отображается все имеющиеся таблицы в модели, которая будет выполнена проверка изменений во время обновления. На вкладках Delete отображаются все таблицы, которые были удалены из схемы, а также будут удалены из модели как часть обновления. Сведения об этих двух вкладках обнаруживается автоматически и предоставляется только в ознакомительных целях нельзя изменить какие-либо параметры.*

    ![RefreshWizard](~/ef6/media/refreshwizard.png)

-   Щелкните "Готово" в мастере обновления

 

Теперь модель обновляется для включения нового Пользовательская сущность, которая сопоставляется с таблицей пользователей, которые мы добавили в базу данных.

![ModelUpdated](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели Database First разработки, которое позволяет создать модель в конструкторе, EF, основываясь на существующую базу данных. Затем мы использовали эту модель для чтения и записи некоторых данных из базы данных. Наконец мы обновили модель для отражения изменений, внесенных в схему базы данных.
