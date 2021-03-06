---
title: Глобальные фильтры запросов — EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053904"
---
# <a name="global-query-filters"></a>Глобальные фильтры запросов

Глобальные фильтры запросов являются предикатами запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ *Where*), которые применяются непосредственно в типах сущностей в модели метаданных (обычно в *OnModelCreating*). Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации. Ниже приведены некоторые типичные способы применения этой функции.

* **Обратимое удаление**. Тип сущности определяет свойство *IsDeleted*.
* **Мультитенантность**. Тип сущности определяет свойство *TenantId*.

## <a name="example"></a>Пример

В следующем примере показано, как использовать глобальные фильтры запросов для реализации такого поведения запроса, как обратимое удаление и мультитенантность, в простой модели ведения блогов.

> [!TIP]
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) из репозитория GitHub.

Сначала определите сущности:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Запишите объявление поля __tenantId_ в сущности _Blog_. Оно будет использоваться для связывания каждого экземпляра блога с конкретным клиентом. Также определено свойство _IsDeleted_ в типе сущности _Post_. Оно используется, чтобы проверять, был ли экземпляр _Post_ удален "обратимо". То есть, экземпляр помечен как удаленный без физического удаления базовых данных.

Затем настройте фильтры запросов в _OnModelCreating_, используя API ```HasQueryFilter```.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Выражения предиката, поступающие в вызовы _HasQueryFilter_, будут автоматически применяться ко всем запросам LINQ для этих типов.

> [!TIP]
> Обратите внимание на использование поля уровня экземпляра DbContext. ```_tenantId``` используется для установки текущего клиента. Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста. То есть, экземпляра, который выполняет этот запрос.

## <a name="disabling-filters"></a>Отключение фильтров

Фильтры можно отключить для отдельных запросов LINQ, используя оператор ```IgnoreQueryFilters()```.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Ограничения

Глобальные фильтры запросов имеют следующие ограничения:

* Фильтры не должны содержать ссылки на свойства навигации.
* Фильтры можно определить только для корневого типа сущности в иерархии наследования.
