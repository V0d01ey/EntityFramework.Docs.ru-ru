---
title: Обновление с EF Core 1.0 RC2 для RTM-версии — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949061"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="fc785-102">Обновление с EF Core 1.0 RC2 до RTM</span><span class="sxs-lookup"><span data-stu-id="fc785-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="fc785-103">Эта статья содержит руководство по переносу приложения, созданного с помощью пакетов 1.0.0, версия-кандидат 2 RTM.</span><span class="sxs-lookup"><span data-stu-id="fc785-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="fc785-104">Версии пакетов</span><span class="sxs-lookup"><span data-stu-id="fc785-104">Package Versions</span></span>

<span data-ttu-id="fc785-105">Имена пакетов верхнего уровня, которые требуется установить в приложение обычно не изменился между RC2 и RTM.</span><span class="sxs-lookup"><span data-stu-id="fc785-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="fc785-106">**Вам потребуется обновить установленные пакеты до RTM-версии.**</span><span class="sxs-lookup"><span data-stu-id="fc785-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="fc785-107">Пакеты среды выполнения (например, `Microsoft.EntityFrameworkCore.SqlServer`) изменилось с `1.0.0-rc2-final` для `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="fc785-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="fc785-108">`Microsoft.EntityFrameworkCore.Tools` Пакета изменен с `1.0.0-preview1-final` для `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="fc785-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="fc785-109">Обратите внимание, что оборудование по-прежнему предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="fc785-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="fc785-110">Существующие миграции может потребоваться добавить maxLength</span><span class="sxs-lookup"><span data-stu-id="fc785-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="fc785-111">В версии-кандидата 2, определению столбца в миграции выглядело как `table.Column<string>(nullable: true)` и длину столбца было искать в некоторые метаданные, мы сохраняем в коде для миграции.</span><span class="sxs-lookup"><span data-stu-id="fc785-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="fc785-112">В версии RTM, длина теперь входит в шаблонном коде `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="fc785-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="fc785-113">Все существующие миграции, которые были скаффолдинга перед использованием RTM не будет иметь `maxLength` указан аргумент.</span><span class="sxs-lookup"><span data-stu-id="fc785-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="fc785-114">Это означает, что будет использоваться максимальную длину, поддерживаемую в базе данных (`nvarchar(max)` на сервере SQL Server).</span><span class="sxs-lookup"><span data-stu-id="fc785-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="fc785-115">Это может быть нормально для некоторых столбцов, но столбцы, являющиеся частью ключа, внешний ключ или индекс должен обновляться по максимальной длиной.</span><span class="sxs-lookup"><span data-stu-id="fc785-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="fc785-116">По соглашению 450 — Максимальная длина для ключи, внешние ключи и индексированных столбцов.</span><span class="sxs-lookup"><span data-stu-id="fc785-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="fc785-117">Если вы явным образом настроили длиной в модели, затем следует использовать этой длины, вместо этого.</span><span class="sxs-lookup"><span data-stu-id="fc785-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="fc785-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="fc785-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="fc785-119">Это изменение влияет на проекты, использующие ASP.NET Identity и были созданы из подготовительной-RTM шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="fc785-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="fc785-120">Шаблон проекта включает миграции, используемый для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="fc785-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="fc785-121">Этот вид миграции необходимо отредактировать для указания максимальной длиной `256` для следующих столбцов.</span><span class="sxs-lookup"><span data-stu-id="fc785-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="fc785-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="fc785-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="fc785-123">name</span><span class="sxs-lookup"><span data-stu-id="fc785-123">Name</span></span>

    * <span data-ttu-id="fc785-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="fc785-124">NormalizedName</span></span>

*  <span data-ttu-id="fc785-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="fc785-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="fc785-126">Адрес эл. почты</span><span class="sxs-lookup"><span data-stu-id="fc785-126">Email</span></span>

   * <span data-ttu-id="fc785-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="fc785-127">NormalizedEmail</span></span>

   * <span data-ttu-id="fc785-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="fc785-128">NormalizedUserName</span></span>

   * <span data-ttu-id="fc785-129">UserName</span><span class="sxs-lookup"><span data-stu-id="fc785-129">UserName</span></span>

<span data-ttu-id="fc785-130">Не удалось внести это изменение приведет к следующее исключение при первоначальной миграции применяется к базе данных.</span><span class="sxs-lookup"><span data-stu-id="fc785-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="fc785-131">.NET core: Удаление «import» в project.json</span><span class="sxs-lookup"><span data-stu-id="fc785-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="fc785-132">Если вы для .NET Core с помощью версии-кандидата 2, необходимо добавить `imports` в файл project.json в качестве временного решения для некоторых зависимостей EF Core не поддерживает .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="fc785-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="fc785-133">Они могут быть удален.</span><span class="sxs-lookup"><span data-stu-id="fc785-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="fc785-134">Начиная с версии 1.0 RTM, [пакет SDK для .NET Core](https://www.microsoft.com/net/download/core) больше не поддерживает `project.json` или разработки приложений .NET Core с помощью Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="fc785-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="fc785-135">Корпорация Майкрософт рекомендует [выполнить миграцию project.json в формат csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="fc785-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="fc785-136">Если вы используете Visual Studio, мы рекомендуем обновить до [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="fc785-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="fc785-137">UWP: Добавить переадресации привязок</span><span class="sxs-lookup"><span data-stu-id="fc785-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="fc785-138">При попытке выполнения команд EF на результатов проектов универсальной платформы Windows (UWP) следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="fc785-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="fc785-139">Необходимо вручную добавить перенаправления привязки в проект универсальной платформы Windows.</span><span class="sxs-lookup"><span data-stu-id="fc785-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="fc785-140">Создайте файл с именем `App.config` в проекте корневую папку и добавьте перенаправления правильные версии сборок.</span><span class="sxs-lookup"><span data-stu-id="fc785-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
