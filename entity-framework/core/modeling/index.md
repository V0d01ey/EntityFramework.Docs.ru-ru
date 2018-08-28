---
title: Создание модели — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 9f702d5833b88e6eb77c0afefdae0ed3bc162ec8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993937"
---
# <a name="creating-a-model"></a><span data-ttu-id="131fe-102">Создание модели</span><span class="sxs-lookup"><span data-stu-id="131fe-102">Creating a Model</span></span>

<span data-ttu-id="131fe-103">Entity Framework использует набор соглашений для создания модели на основе формы классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="131fe-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="131fe-104">Вы можете указать дополнительную конфигурацию, чтобы дополнить и (или) переопределить данные, обнаруженные соглашением.</span><span class="sxs-lookup"><span data-stu-id="131fe-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="131fe-105">Эта статья описывает конфигурацию, которую можно применить к модели, ориентированной на любое хранилище данных, а также при ориентации на любую реляционную базу данных.</span><span class="sxs-lookup"><span data-stu-id="131fe-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="131fe-106">Поставщики также могут включить конфигурацию, предназначенную для конкретного хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="131fe-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="131fe-107">Документацию по конфигурации для конкретного поставщика см. в разделе [Поставщики баз данных](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="131fe-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="131fe-108">Для этой статьи вы можете просмотреть [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) в репозитории GitHub.</span><span class="sxs-lookup"><span data-stu-id="131fe-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="131fe-109">Методы настройки</span><span class="sxs-lookup"><span data-stu-id="131fe-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="131fe-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="131fe-110">Fluent API</span></span>

<span data-ttu-id="131fe-111">Вы можете переопределить метод `OnModelCreating` в производном контексте и использовать `ModelBuilder API` для настройки модели.</span><span class="sxs-lookup"><span data-stu-id="131fe-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="131fe-112">Это наиболее эффективный метод настройки, который позволяет задать конфигурацию без изменения ваших классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="131fe-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="131fe-113">Конфигурация текучего API имеет самый высокий приоритет и переопределяет соглашения и заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="131fe-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a><span data-ttu-id="131fe-114">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="131fe-114">Data Annotations</span></span>

<span data-ttu-id="131fe-115">Вы также можете применять для классов и свойств атрибуты (называемые заметками к данным).</span><span class="sxs-lookup"><span data-stu-id="131fe-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="131fe-116">Заметки к данным переопределяют соглашения, но перезаписываются конфигурацией текучего API.</span><span class="sxs-lookup"><span data-stu-id="131fe-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
