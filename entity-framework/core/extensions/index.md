---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.author: divega
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/extensions/index
ms.openlocfilehash: 6c8cb3e0d8552f274118e4020b7e2e8009af7e11
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769443"
---
# <a name="ef-core-tools--extensions"></a>Инструменты и расширения EF Core

Инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core.

> [!IMPORTANT]  
> Расширения создаются с помощью различных источников и не разрабатываются в рамках проекта Entity Framework Core. Выбирая стороннее расширение, обязательно оцените качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.

## <a name="tools"></a>Инструменты

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core. Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.

[веб-сайт](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL. Используйте подходы Model-First (сначала модель) и Database-First (сначала база данных) для разработки модели ORM и создания для нее кода C# или Visual Basic .NET. В нем используются новые подходы для разработки моделей ORM, повышения производительности и упрощения разработки приложений баз данных.

[веб-сайт](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Расширение Visual Studio 2017+. Вы можете реконструировать классы DbContext и POCO из существующей базы данных или проекта базы данных SQL Server и визуализировать и проверить DbContext различными способами.

[Вики-сайт GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a>Расширения

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки автоматической записи истории изменения данных.

[Репозиторий GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Расширения динамического Linq для Microsoft.EntityFrameworkCore, которые добавляют поддержку асинхронных запросов

 [Репозиторий GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Попытка объединить некоторые хорошие примеры использования в один API, поддерживающий тестирование, включая небольшую платформу для проверки на наличие запросов N+1.

[Репозиторий GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Кэширующая библиотека второго уровня. Кэширование второго уровня — это кэширование запросов. Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз.

[Репозиторий GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Загружает и сохраняет полные диаграммы отсоединенных сущностей (сущность со своими дочерними сущностями и списками). Создан под впечатлением от расширения [GraphDiff](https://github.com/refactorthis/GraphDiff/). Идея также заключается в добавлении подключаемых модулей для упрощения некоторых повторяющихся задач, таких как аудит и разбиение на страницы.

[Репозиторий GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Получает из любой сущности первичный ключ (включая составные ключи) как словарь.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Оболочки реактивных расширений для критически важных наблюдаемых сущностей Entity Framework.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Добавление триггеров для сущностей с реагированием на события insert, update и delete. Существует три события для каждой операции: до, после и при сбое.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Получите типизированный доступ к OriginalValue свойств сущности. Поддерживаются простые и сложные свойства, навигации и коллекции не поддерживаются.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco предоставляет генератор реконструированной модели с поддержкой преобразования во множественную или единичную форму и настраиваемых шаблонов на основе интерполированных строк C# 6.0, выполняющихся в .NET Core. Он также предоставляет генератор сценариев начальных значений со сценариями слияния SQL и запускателем сценариев.

[Репозиторий GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore — бесплатный набор расширений для опытных пользователей LINQ to SQL и EntityFrameworkCore. С поддержкой Include(...) и IDbAsync.

[Репозиторий GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore предоставляет полезные расширения для использования поставщиков LINQ, таких как Entity Framework, поддерживающие только небольшое подмножество функций .NET, повторно используемые функции, перезапись запросов (даже переделку их в безопасные для обработки NULL) и построение динамических запросов с помощью транслируемых предикатов и селекторов.

[Репозиторий GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.

[Репозиторий GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Отложенная загрузка для EF Core 1.1

[Репозиторий GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Расширения EntityFrameworkCore для массовых операций (Insert, Update, Delete).

[Репозиторий GitHub](https://github.com/borisdj/EFCore.BulkExtensions)
