---
title: "Типы запросов - EF Core"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a><span data-ttu-id="a58a6-102">Типы запросов</span><span class="sxs-lookup"><span data-stu-id="a58a6-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="a58a6-103">Этот компонент является новые возможности EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="a58a6-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="a58a6-104">Типы запросов являются типы результатов запросов только для чтения, которые могут добавляться EF базовой модели.</span><span class="sxs-lookup"><span data-stu-id="a58a6-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="a58a6-105">Типы запросов включить нерегламентированный запрос (например, анонимные типы), но являются более гибкими, из-за наличия сопоставление параметров.</span><span class="sxs-lookup"><span data-stu-id="a58a6-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="a58a6-106">Они похожи на типы сущностей в том, что:</span><span class="sxs-lookup"><span data-stu-id="a58a6-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="a58a6-107">Они являются типами POCO C#, которые добавляются в модель, либо в ```OnModelCreating``` с помощью ```ModelBuilder.Query``` метод, или через свойство DbContext «набор» (для запроса типов такого свойства типизируется как ```DbQuery<T>``` вместо ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="a58a6-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather than ```DbSet<T>```).</span></span>
- <span data-ttu-id="a58a6-108">Они поддерживают большую часть возможностей же сопоставления как типы обычной сущности.</span><span class="sxs-lookup"><span data-stu-id="a58a6-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="a58a6-109">Например, сопоставление наследования, переходы, (см. ниже limitiations) и в реляционных хранилищах, возможность настройки объекты схемы целевой базы данных через ```ToTable```, ```HasColumn``` методов fluent api (или заметок к данным).</span><span class="sxs-lookup"><span data-stu-id="a58a6-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="a58a6-110">Типы запросов отличаются от сущности типов в том, что:</span><span class="sxs-lookup"><span data-stu-id="a58a6-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="a58a6-111">Ключ определен не требуется.</span><span class="sxs-lookup"><span data-stu-id="a58a6-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="a58a6-112">Никогда не находятся под контролем отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="a58a6-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="a58a6-113">Никогда не обнаруживаются по соглашению.</span><span class="sxs-lookup"><span data-stu-id="a58a6-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="a58a6-114">Только поддерживают подмножество возможности навигации сопоставление — в частности, никогда не может выступать в роли основной элемент связи.</span><span class="sxs-lookup"><span data-stu-id="a58a6-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="a58a6-115">Может быть сопоставлен _определение запроса_ -определение это дополнительный запрос, используемый источником данных для типа запроса.</span><span class="sxs-lookup"><span data-stu-id="a58a6-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="a58a6-116">Ниже приведены некоторые из сценариев использования основных типов запросов.</span><span class="sxs-lookup"><span data-stu-id="a58a6-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="a58a6-117">Сопоставление для представления базы данных.</span><span class="sxs-lookup"><span data-stu-id="a58a6-117">Mapping to database views.</span></span>
- <span data-ttu-id="a58a6-118">Сопоставление таблиц, у которых нет первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="a58a6-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="a58a6-119">Для нерегламентированных выступают в качестве возвращаемого типа для ```FromSql()``` запросов.</span><span class="sxs-lookup"><span data-stu-id="a58a6-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="a58a6-120">Сопоставление запросов, определенных в модели.</span><span class="sxs-lookup"><span data-stu-id="a58a6-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="a58a6-121">Сопоставление типа запроса представления базы данных достигается с помощью ```ToTable``` fluent API.</span><span class="sxs-lookup"><span data-stu-id="a58a6-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="a58a6-122">Пример</span><span class="sxs-lookup"><span data-stu-id="a58a6-122">Example</span></span>

<span data-ttu-id="a58a6-123">В следующем примере показано, как используется тип запроса для запроса представления базы данных.</span><span class="sxs-lookup"><span data-stu-id="a58a6-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="a58a6-124">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="a58a6-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="a58a6-125">Во-первых определим простой модели блога и Post.</span><span class="sxs-lookup"><span data-stu-id="a58a6-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="a58a6-126">Затем определяется представление простой базы данных, мы будем запрашивать количество записей, связанных с каждого блога:</span><span class="sxs-lookup"><span data-stu-id="a58a6-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="a58a6-127">Затем определяется класс, содержащий результат из представления базы данных:</span><span class="sxs-lookup"><span data-stu-id="a58a6-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="a58a6-128">Далее мы настраиваем тип запроса в _OnModelCreating_ с помощью ```modelBuilder.Query<T>``` API.</span><span class="sxs-lookup"><span data-stu-id="a58a6-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="a58a6-129">Мы используем стандартной конфигурации fluent API-интерфейсы для настройки сопоставления для типа запроса:</span><span class="sxs-lookup"><span data-stu-id="a58a6-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="a58a6-130">Наконец можно запросить представления базы данных обычным образом:</span><span class="sxs-lookup"><span data-stu-id="a58a6-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="a58a6-131">Обратите внимание, что мы определили свойство контекста запроса уровня (DbQuery) в качестве корневой для запросов этого типа.</span><span class="sxs-lookup"><span data-stu-id="a58a6-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
