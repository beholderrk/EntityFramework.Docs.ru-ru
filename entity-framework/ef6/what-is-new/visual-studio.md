---
title: Выпуски Visual Studio - EF6
author: divega
ms.date: 2018-07-05
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
caps.latest.revision: 3
ms.openlocfilehash: 7bd08a46b1d6acc5a565952e834f01546a5262c8
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121846"
---
# <a name="visual-studio-releases"></a>Выпуски Visual Studio

Мы рекомендуем всегда использовать последнюю версию Visual Studio, так как она содержит новейшие средства для .NET, NuGet и Entity Framework.
На самом деле различные примеры и пошаговые руководства по документации по Entity Framework предполагается, что вы используете последнюю версию Visual Studio.

Это возможно, однако для использования с различными версиями платформы Entity Framework более старых версиях Visual Studio до тех пор, пока вы учетной записи некоторые различия:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 и более поздних версий

- Эта версия Visual Studio включает в себя последнюю версию средства платформы Entity Framework и средой выполнения EF 6.2 и не требует дополнительных шагов настройки.
См. в разделе [новые](~/ef6/what-is-new/index.md) Дополнительные сведения об этих выпусках.
- Добавление платформы Entity Framework к новым проектам, со средствами EF автоматически добавляет пакет EF 6.2 NuGet.
Можно вручную установить или обновить до любой пакет EF NuGet, доступный через Интернет.
- По умолчанию экземпляр SQL Server, доступные в этой версии Visual Studio — это экземпляр LocalDB, вызывается MSSQLLocalDB.
В разделе сервера следует использовать строки подключения «(localdb)\\MSSQLLocalDB».
Не забывайте использовать буквальная строка с префиксом `@` или двойной обратной косой черты "\\\\" при указании строку подключения в коде C#.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 до Visual Studio 2017 версии 15.6

- Эти версии Visual Studio включают средства платформы Entity Framework и среды выполнения 6.1.3.
См. в разделе [последние выпуски](~/ef6/what-is-new/past-releases.md#ef-613) Дополнительные сведения об этих выпусках.
- Добавление платформы Entity Framework к новым проектам, со средствами EF автоматически добавит EF 6.1.3 пакет NuGet.
Можно вручную установить или обновить до любой пакет EF NuGet, доступный через Интернет.
- По умолчанию экземпляр SQL Server, доступные в этой версии Visual Studio — это экземпляр LocalDB, вызывается MSSQLLocalDB.
В разделе сервера следует использовать строки подключения «(localdb)\\MSSQLLocalDB».
Не забывайте использовать буквальная строка с префиксом `@` или двойной обратной косой черты "\\\\" при указании строку подключения в коде C#.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Эта версия Visual Studio включает и более старую версию средства платформы Entity Framework и среды выполнения.
Рекомендуется обновить средства платформы Entity Framework 6.1.3, с помощью [установщик](https://www.microsoft.com/en-us/download/details.aspx?id=40762) доступны в центре загрузки Майкрософт.
См. в разделе [последние выпуски](~/ef6/what-is-new/past-releases.md#ef-613) Дополнительные сведения об этих выпусках.
- Добавление новых проектов с помощью обновленных средств EF Entity Framework автоматически добавит EF 6.1.3 пакет NuGet.
Можно вручную установить или обновить до любой пакет EF NuGet, доступный через Интернет.
- По умолчанию экземпляр SQL Server, доступные в этой версии Visual Studio — это экземпляр LocalDB, вызывается MSSQLLocalDB.
В разделе сервера следует использовать строки подключения «(localdb)\\MSSQLLocalDB».
Не забывайте использовать буквальная строка с префиксом `@` или двойной обратной косой черты "\\\\" при указании строку подключения в коде C#.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Эта версия Visual Studio включает и более старую версию средства платформы Entity Framework и среды выполнения.
Рекомендуется обновить средства платформы Entity Framework 6.1.3, с помощью [установщик](https://www.microsoft.com/en-us/download/details.aspx?id=40762) доступны в центре загрузки Майкрософт.
См. в разделе [последние выпуски](~/ef6/what-is-new/past-releases.md#ef-613) Дополнительные сведения об этих выпусках.
- Добавление новых проектов с помощью обновленных средств EF Entity Framework автоматически добавит EF 6.1.3 пакет NuGet.
Можно вручную установить или обновить до любой пакет EF NuGet, доступный через Интернет.
- По умолчанию экземпляр SQL Server, доступные в этой версии Visual Studio — это экземпляр LocalDB, вызывается v11.0.
В разделе сервера следует использовать строки подключения «(localdb)\\v11.0».
Не забывайте использовать буквальная строка с префиксом `@` или двойной обратной косой черты "\\\\" при указании строку подключения в коде C#.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Версии доступны средства платформы Entity Framework с данной версией Visual Studio несовместим со средой выполнения Entity Framework 6 и не могут быть обновлены.
- По умолчанию средства платформы Entity Framework будет добавлять в проекты Entity Framework 4.0.
Чтобы создать приложения, использующие более новые версии EF, необходимо сначала установить [расширение диспетчера пакетов NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- По умолчанию все создание кода в версии инструментов EF основан на EntityObject и Entity Framework 4.
Мы рекомендуем переключиться формирования кода должен быть основан на DbContext и Entity Framework 5, установив шаблоны создания кода DbContext для [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) или [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- После установки расширения диспетчера пакетов NuGet, можно вручную установить или обновить до любой пакет EF NuGet, доступный через Интернет и использовать EF6 в режиме Code First, которая не требует конструктора.
- По умолчанию экземпляр SQL Server, доступные в этой версии Visual Studio — SQL Server Express с именем SQLEXPRESS.
В разделе сервера следует использовать строки подключения «. \\SQLEXPRESS».
Не забывайте использовать буквальная строка с префиксом `@` или двойной обратной косой черты "\\\\" при указании строку подключения в коде C#.
