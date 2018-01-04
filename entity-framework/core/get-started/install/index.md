---
title: "Установка EF Core"
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="installing-ef-core"></a><span data-ttu-id="98a12-102">Установка EF Core</span><span class="sxs-lookup"><span data-stu-id="98a12-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98a12-103">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="98a12-103">Prerequisites</span></span>

<span data-ttu-id="98a12-104">Для разработки приложений .NET Core 2.0 (включая приложения ASP.NET Core 2.0, предназначенные для .NET Core) нужно скачать и установить подходящую версию [пакета SDK .NET Core 2.0](https://www.microsoft.com/net/download/core) для используемой платформы.</span><span class="sxs-lookup"><span data-stu-id="98a12-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="98a12-105">**Это справедливо, даже если вы установили Visual Studio 2017 версии 15.3.**</span><span class="sxs-lookup"><span data-stu-id="98a12-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="98a12-106">Чтобы использовать EF Core 2.0 или любую другую библиотеку .NET Standard 2.0 с платформами .NET, кроме .NET Core 2.0 (например, с .NET Framework 4.6.1 или более поздней версии), вам потребуется версия NuGet, которая поддерживает .NET Standard 2.0 и ее совместимые платформы.</span><span class="sxs-lookup"><span data-stu-id="98a12-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="98a12-107">Ниже приведено несколько способов, позволяющих получить ее.</span><span class="sxs-lookup"><span data-stu-id="98a12-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="98a12-108">Установите Visual Studio 2017 версии 15.3.</span><span class="sxs-lookup"><span data-stu-id="98a12-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="98a12-109">Если вы используете Visual Studio 2015, [скачайте и обновите клиент NuGet до версии 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="98a12-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="98a12-110">Чтобы обеспечить совместимость проектов, созданных в предыдущих версиях Visual Studio и предназначенных для .NET Framework, с библиотеками NET Standard 2.0, в эти проекты может потребоваться внести некоторые изменения.</span><span class="sxs-lookup"><span data-stu-id="98a12-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="98a12-111">Измените файл проекта и включите в группу начальной свойств следующую запись.</span><span class="sxs-lookup"><span data-stu-id="98a12-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="98a12-112">Для тестовых проектов также включите следующую запись.</span><span class="sxs-lookup"><span data-stu-id="98a12-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="98a12-113">Получение битов</span><span class="sxs-lookup"><span data-stu-id="98a12-113">Getting the bits</span></span>
<span data-ttu-id="98a12-114">Для добавления библиотек времени выполнения EF Core в приложение рекомендуется установить поставщик базы данных EF Core из NuGet.</span><span class="sxs-lookup"><span data-stu-id="98a12-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="98a12-115">Кроме библиотек времени выполнения, можно установить средства, облегчающие выполнение нескольких задач, связанных с EF Core, во время разработки, таких как создание и применение миграций, а также создание модели на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="98a12-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="98a12-116">Если требуется обновить приложение, использующее сторонний поставщик базы данных, всегда ищите обновление поставщика, совместимое с нужной вам версией EF Core.</span><span class="sxs-lookup"><span data-stu-id="98a12-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="98a12-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="98a12-117">E.g.</span></span> <span data-ttu-id="98a12-118">поставщики баз данных для предыдущих версий не совместимы с версией 2.0 среды выполнения EF Core.</span><span class="sxs-lookup"><span data-stu-id="98a12-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="98a12-119">Приложения, предназначенные для ASP.NET Core 2.0, могут использовать EF Core 2.0 без дополнительных зависимостей, кроме сторонних поставщиков базы данных.</span><span class="sxs-lookup"><span data-stu-id="98a12-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="98a12-120">Приложения, предназначенные для предыдущих версий ASP.NET Core, нужно обновить до ASP.NET Core 2.0, чтобы они могли использовать EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="98a12-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="98a12-121">Кроссплатформенная разработка с помощью интерфейса командной строки (CLI) .NET Core</span><span class="sxs-lookup"><span data-stu-id="98a12-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="98a12-122">Для разработки приложений, предназначенных для [.NET Core](https://www.microsoft.com/net/download/core), можно использовать [команды CLI `dotnet`](https://docs.microsoft.com/dotnet/core/tools/) в сочетании с вашим любимым текстовым редактором или интегрированную среду разработки (IDE), такую как Visual Studio, Visual Studio для Mac или Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="98a12-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="98a12-123">Приложениям, предназначенным для .NET Core, требуются определенные версии Visual Studio, например, для разработки .NET Core 1.x требуется Visual Studio 2017, а для разработки .NET Core 2.0 — Visual Studio 2017 версии 15.3.</span><span class="sxs-lookup"><span data-stu-id="98a12-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="98a12-124">Чтобы установить или обновить поставщик SQL Server в кроссплатформенном приложении .NET Core, перейдите в каталог приложения и выполните следующую команду в командной строке.</span><span class="sxs-lookup"><span data-stu-id="98a12-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="98a12-125">Вы можете указать в команде `dotnet add package` установку конкретной версии, используя модификатор `-v`.</span><span class="sxs-lookup"><span data-stu-id="98a12-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="98a12-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="98a12-126">E.g.</span></span> <span data-ttu-id="98a12-127">чтобы установить пакеты EF Core 2.0, добавьте `-v 2.0.0` в команду.</span><span class="sxs-lookup"><span data-stu-id="98a12-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="98a12-128">EF Core содержит набор [дополнительных команд для CLI `dotnet`](../../miscellaneous/cli/dotnet.md), начиная с `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="98a12-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="98a12-129">Чтобы использовать команды CLI `dotnet ef`, файл `.csproj` вашего приложения должен содержать следующую запись.</span><span class="sxs-lookup"><span data-stu-id="98a12-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="98a12-130">Инструментам CLI .NET Core CLI для EF Core также требуется отдельный пакет — Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="98a12-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="98a12-131">Вы можете просто добавить его в проект с помощью команды.</span><span class="sxs-lookup"><span data-stu-id="98a12-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="98a12-132">Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="98a12-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="98a12-133">Разработка в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98a12-133">Visual Studio development</span></span>

<span data-ttu-id="98a12-134">С помощью Visual Studio можно разрабатывать множество разных типов приложений, предназначенных для .NET Core, .NET Framework или других платформ, поддерживаемых EF Core.</span><span class="sxs-lookup"><span data-stu-id="98a12-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="98a12-135">Установить поставщик базы данных EF Core в своем приложении из Visual Studio можно двумя способами.</span><span class="sxs-lookup"><span data-stu-id="98a12-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="98a12-136">Использование [пользовательского интерфейса диспетчера пакетов](https://docs.microsoft.com/nuget/tools/package-manager-ui) NuGet</span><span class="sxs-lookup"><span data-stu-id="98a12-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="98a12-137">Выберите в меню пункты **Проект > Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="98a12-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="98a12-138">Нажмите кнопку **Обзор** или откройте вкладку **Обновления**.</span><span class="sxs-lookup"><span data-stu-id="98a12-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="98a12-139">Выберите пакет `Microsoft.EntityFrameworkCore.SqlServer` и нужную версию, а затем подтвердите операцию.</span><span class="sxs-lookup"><span data-stu-id="98a12-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="98a12-140">Использование [консоли диспетчера пакетов (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) NuGet</span><span class="sxs-lookup"><span data-stu-id="98a12-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="98a12-141">В меню выберите пункты **"Сервис" > "Диспетчер пакетов NuGet" > "Консоль диспетчера пакетов"**.</span><span class="sxs-lookup"><span data-stu-id="98a12-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="98a12-142">Введите и запустите следующую команду в PMC.</span><span class="sxs-lookup"><span data-stu-id="98a12-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="98a12-143">Вместо этого можно использовать команду `Update-Package`, чтобы обновить уже установленный пакет до более новой версии.</span><span class="sxs-lookup"><span data-stu-id="98a12-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="98a12-144">Чтобы указать конкретную версию, можно использовать модификатор `-Version`. Например, чтобы установить пакеты EF Core 2.0, добавьте `-Version 2.0.0` в команды.</span><span class="sxs-lookup"><span data-stu-id="98a12-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="98a12-145">Инструменты</span><span class="sxs-lookup"><span data-stu-id="98a12-145">Tools</span></span>

<span data-ttu-id="98a12-146">Кроме того, в Visual Studio существуют версии PowerShell для [команд EF Core, выполняемых внутри PMC,](../../miscellaneous/cli/powershell.md) возможности которых аналогичны командам `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="98a12-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="98a12-147">Чтобы использовать их, установите пакет `Microsoft.EntityFrameworkCore.Tools` с помощью пользовательского интерфейса диспетчера пакетов или консоли PMC.</span><span class="sxs-lookup"><span data-stu-id="98a12-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="98a12-148">Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="98a12-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="98a12-149">Хотя команды `dotnet ef` можно использовать из PMC в Visual Studio, гораздо удобнее использовать их версии PowerShell.</span><span class="sxs-lookup"><span data-stu-id="98a12-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="98a12-150">Они автоматически работают для выбранного в PMC проекта без ручного переключения каталогов.</span><span class="sxs-lookup"><span data-stu-id="98a12-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="98a12-151">Они автоматически открывают файлы, созданные командами в Visual Studio, после завершения соответствующей команды.</span><span class="sxs-lookup"><span data-stu-id="98a12-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="98a12-152">**Нерекомендуемые пакеты в EF Core 2.0:** при обновлении существующего приложения до EF Core 2.0 некоторые ссылки на более старые пакеты EF Core может потребоваться удалить вручную.</span><span class="sxs-lookup"><span data-stu-id="98a12-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="98a12-153">В частности, пакеты поставщиков баз данных во время разработки, например `Microsoft.EntityFrameworkCore.SqlServer.Design`, в EF Core 2.0 не требуется и не поддерживается, но они не удаляется автоматически при обновлении других пакетов.</span><span class="sxs-lookup"><span data-stu-id="98a12-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>