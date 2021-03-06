---
title: Начало работы в .NET Framework — существующая база данных — EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614419"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Начало работы с EF Core в .NET Framework с существующей базой данных

В этом руководстве вы создадите консольное приложение, которое реализует простейший доступ к базе данных Microsoft SQL Server с помощью Entity Framework. Для создания модели Entity Framework используется реконструирование существующей базы данных.

[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Предварительные требования

* [Visual Studio 2017 версии 15.7 или выше](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Создание базы данных Blogging

В этом руководстве в качестве существующей базы данных используется база **Blogging** для ведения блогов, размещенная на локальном экземпляре LocalDb. Если вы уже создали базу данных **Blogging**, работая с другим руководством, пропустите эти шаги.

* Открытие Visual Studio

* **"Сервис" > "Подключение к базе данных"…**

* Выберите **Microsoft SQL Server** и щелкните **Продолжить**.

* Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.

* Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.

* Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.

* Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.

* Скопируйте представленный ниже скрипт и вставьте его в редактор запросов

* Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Создание нового проекта

* Откройте Visual Studio 2017.

* **"Файл" > "Создать" > "Проект"…**

* В меню слева выберите **"Установленные" > Visual C# > Классическое приложение Windows**

* Выберите шаблон проекта **Консольное приложение (.NET Framework)**.

* Задайте **.NET Framework 4.6.1** или более позднюю версию в качестве целевой платформы проекта

* Назовите проект *ConsoleApp.ExistingDb* и нажмите кнопку **ОК**

## <a name="install-entity-framework"></a>Установка Entity Framework

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом руководстве используется SQL Server. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

На следующем этапе вы воспользуетесь рядом инструментов Entity Framework Tools для реконструирования базы данных. Поэтому также установите пакет инструментов.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-the-model"></a>Реконструирование модели

Теперь пора создать модель EF на основе существующей базы данных.

* Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.

* Выполните следующую команду, чтобы создать модель на основе существующей базы данных.

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> Вы можете выбрать, для каких таблиц создавать сущности, указав в команде выше аргумент `-Tables`. Например, `-Tables Blog,Post`.

Процесс реконструирования создает классы сущностей (`Blog` и `Post`) и производный контекст (`BloggingContext`) на основе схемы существующей базы данных.

Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения. Ниже приведены классы сущностей `Blog` и `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Чтобы включить отложенную загрузку, можно присвоить свойствам навигации (Blog.Post и Post.Blog) значение `virtual`.

Контекст представляет сеанс работы с базой данных. Он содержит методы, которые можно использовать для запроса и сохранения экземпляров классов сущностей.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Использование модели

Теперь вы можете использовать созданную модель для доступа к данным.

* Откройте файл *Program.cs*.

* Замените все содержимое этого файла следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Выберите "Отладка" -> "Запустить без отладки".

  Вы увидите, как один блог сохраняется в базе данных, а затем сведения обо всех блогах выводятся в консоль.

  ![изображение](_static/output-existing-db.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [EF Core в .NET Framework с новой базой данных](xref:core/get-started/full-dotnet/new-db)
* [EF Core в .NET Core с новой базой данных — SQLite](xref:core/get-started/netcore/new-db-sqlite). Руководство по кроссплатформенной консоли EF.
