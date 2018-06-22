---
title: Последовательность — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052714"
---
# <a name="sequences"></a><span data-ttu-id="4ebb1-102">Последовательности</span><span class="sxs-lookup"><span data-stu-id="4ebb1-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="4ebb1-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4ebb1-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="4ebb1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4ebb1-105">Последовательность формирует последовательный числовых значений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="4ebb1-106">Последовательности не связаны с определенной таблицей.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="4ebb1-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="4ebb1-107">Conventions</span></span>

<span data-ttu-id="4ebb1-108">По соглашению последовательности не представленные в модели.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4ebb1-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="4ebb1-109">Data Annotations</span></span>

<span data-ttu-id="4ebb1-110">Вы можете настроить не последовательности с использованием заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4ebb1-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="4ebb1-111">Fluent API</span></span>

<span data-ttu-id="4ebb1-112">Fluent API можно использовать для создания последовательности в модели.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="4ebb1-113">Можно также настроить дополнительные аспектом последовательности, например ее схему, начальное значение и приращение.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="4ebb1-114">Будучи однажды представленным последовательности, его можно использовать для формирования значений для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="4ebb1-115">Например, можно использовать [значения по умолчанию](default-values.md) для вставки следующего значения из последовательности.</span><span class="sxs-lookup"><span data-stu-id="4ebb1-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
