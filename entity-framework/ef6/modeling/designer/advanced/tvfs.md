---
title: Возвращающие табличные значения функции (TVF) - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122622"
---
# <a name="table-valued-functions-tvfs"></a>Возвращающие табличные значения функции (TVF)
> [!NOTE]
> **EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

Видео и пошаговые руководства показано, как сопоставить возвращающих табличные значения функции (TVF), с помощью Entity Framework Designer. Он также демонстрирует вызов функции с табличным значением из запроса LINQ.

Возвращающие табличное значение функции в настоящий момент поддерживается только в базе данных первого рабочего процесса.

Возвращающая табличное значение Функция поддержки появилась в версии 5 платформы Entity Framework. Обратите внимание, что для использования новых возможностей, таких как функции, возвращающие табличные значения, перечислимые типы и Пространственные типы .NET Framework 4.5 необходимо ориентироваться. Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.

Возвращающие табличное значение функции очень похожи на хранимые процедуры с одним основным отличием: составляема результатом функции с табличным значением. Это означает, что результаты из функции с табличным значением может использоваться в запросе LINQ, хотя результаты хранимой процедуры нельзя.

## <a name="watch-the-video"></a>Просмотреть видео

**Представленный**: Юлия Корнич

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения этого пошагового руководства, необходимо:

- Установка [базы данных School](~/ef6/resources/school-database.md).

- У последняя версия Visual Studio

## <a name="set-up-the-project"></a>Настройка проекта

1.  Открытие Visual Studio
2.  На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**
3.  В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона
4.  Введите **возвращающей табличное значение функции** как имя проекта и нажмите кнопку **ОК**

## <a name="add-a-tvf-to-the-database"></a>Добавление функции с табличным значением в базу данных

-   Выберите **представление —&gt; обозреватель объектов SQL Server**
-   Если LocalDB отсутствует в списке серверов: щелкните правой кнопкой мыши **SQL Server** и выберите **добавить SQL Server** используйте значение по умолчанию **проверки подлинности Windows** для подключения к серверу LocalDB
-   Разверните узел LocalDB
-   В узле базы данных, щелкните правой кнопкой мыши узел базы данных School и выберите **новый запрос...**
-   В редакторе T-SQL, вставьте следующее определение функции с табличным значением

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   Щелкните правой кнопкой мыши в редакторе T-SQL и выберите **Execute**
-   Функция GetStudentGradesForCourse будет добавлена в базу данных School

 

## <a name="create-a-model"></a>Создание модели

1.  Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**
2.  Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в **шаблоны** области
3.  Введите **TVFModel.edmx** имя файла, а затем нажмите кнопку **добавить**
4.  В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее**
5.  Нажмите кнопку **новое подключение** ввод **(localdb)\\mssqllocaldb** в текст имени сервера поле Ввод **School** для базы данных имя щелкните **ОК**
6.  В поле выберите ваши объекты базы данных диалогового **таблиц** выберите **Person**, **StudentGrade**, и **курс** таблиц
7.  Выберите **GetStudentGradesForCourse** функции, расположенный в **хранимые процедуры и функции** узла Обратите внимание, что, начиная с Visual Studio 2012 г. конструктор сущностей позволяет пакетный Импорт Хранимые процедуры и функции
8.  Нажмите кнопку **Готово**
9.  Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается. Все объекты, которые выбраны в **Choose Your Database Objects** диалоговое окно добавляются в модель.
10. По умолчанию результирующая форма каждый импортированный хранимой процедуры или функции автоматически становится новый сложный тип в модели сущности. Но нам нужно сопоставить с сущностью StudentGrade результаты функции GetStudentGradesForCourse: щелкните правой кнопкой мыши область конструктора и выберите **браузер моделей** в браузере моделей выберите **импортируемые функции**, а затем дважды щелкните **GetStudentGradesForCourse** функции в изменение импорта функции установите флажок **сущностей** и выберите **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Сохранения и извлечения данных

Откройте файл, в котором определен метод Main. Добавьте следующий код в функцию Main.

Следующий код демонстрирует построение запроса, использующего Table-valued Function. Результаты запроса проецируются в анонимный тип, содержащий связанные название курса и связанных слушателям оценку больше или равно 3.5.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Скомпилируйте и запустите приложение. Программа выдает следующие результаты.

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели способ сопоставления возвращающих табличные значения функции (TVF), с помощью Entity Framework Designer. Кроме того, вы узнали, как для вызова функции с табличным значением из запроса LINQ.
