---
title: Обновление до платформы Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 2e2dacfe67238bdb7fd1f31f784319049f0f2cb0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490952"
---
# <a name="upgrading-to-entity-framework-6"></a>Обновление до платформы Entity Framework 6

В предыдущих версиях EF код был разделить между основными библиотеками (в основном System.Data.Entity.dll), в состав платформы .NET Framework и библиотеки (OOB)-каналу (в основном EntityFramework.dll) поставляется в виде пакета NuGet. EF6 принимает код из библиотеки ядра и включает его в библиотеках OOB. Это потребовалось, чтобы разрешить EF сделать открытым исходным кодом и для того, чтобы иметь возможность развиваться в разных темпе с .NET Framework. Вследствие этого является то, что приложения должны быть перестроено на перемещенные типы.

Это должно быть простым для приложений, которые используют DbContext как поставляемый в EF 4.1 и более поздних версий. Немного больше работы требуется для приложения, использующие класса ObjectContext, но он по-прежнему не трудно.

Ниже приведен контрольный список задач, которые необходимо выполнить для обновления существующего приложения в EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Установите пакет EF6 NuGet

Необходимо обновить до новой среды выполнения Entity Framework 6.

1. Щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet...**  
2. В разделе **Online** вкладке выберите **EntityFramework** и нажмите кнопку **установки**  
   > [!NOTE]
   > Если предыдущая версия EntityFramework NuGet, пакет установлен это обновит ее к EF6.

Также можно запустить следующую команду из консоли диспетчера пакетов:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Убедитесь, что удалены ссылки на сборки для System.Data.Entity.dll

Установка пакета EF6 NuGet автоматически следует удалить все ссылки на System.Data.Entity из проекта для вас.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Замените любые модели EF конструктора (EDMX), создания кода EF 6.x

При наличии модели, созданные в конструкторе EF, будет необходимо обновить шаблоны создания кода, чтобы создать совместимый код EF6.

> [!NOTE]
> Доступны в настоящее время только EF 6.x DbContext генератор шаблонов для Visual Studio 2012 и 2013.

1. Удалите существующие шаблоны создания кода. Эти файлы обычно называется  **\<edmx_file_name\>.tt** и  **\<edmx_file_name\>. Context.tt** и быть вложен в узел файла edmx в обозревателе решений. Можно выбрать шаблоны, в обозревателе решений и нажмите клавишу **Del** ключа для их удаления.  
   > [!NOTE]
   > В проектах веб-сайт шаблоны будут не вложен в файле edmx, но в списке с его в обозревателе решений.  

   > [!NOTE]
   > В проектах VB.NET необходимо включить «Показать все файлы» иметь возможность см. в файлах вложенный шаблон.
2. Добавьте соответствующий шаблон генерации кода EF 6.x. Откройте модель в конструкторе EF, щелкните правой кнопкой мыши область конструктора и выберите **добавить элемент формирования кода...**
    - Если вы используете API DbContext (рекомендуется). затем **EF 6.x генератор DbContext** будут доступны в разделе **данных** вкладки.  
      > [!NOTE]
      > Если вы используете Visual Studio 2012, необходимо установить инструменты EF 6, чтобы этот шаблон. См. в разделе [получение Entity Framework](~/ef6/fundamentals/install.md) подробные сведения.  

    - Если вы используете ObjectContext API, а затем будет необходимо выбрать **Online** вкладку и выполните поиск **EF 6.x EntityObject Generator**.  
3. Если вы применили все настройки, шаблоны создания кода, вам потребуется повторно применить их к в обновленных шаблонах.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Обновление пространства имен для использования типов EF core

Пространства имен для типов DbContext и Code First не изменились. Это означает, что для многих приложений, использующих EF 4.1 или более поздней версии не нужно ничего менять.

Типы как ObjectContext, которые ранее находились в System.Data.Entity.dll были перемещены в новые пространства имен. Это означает, что может потребоваться обновить ваш *с помощью* или *импорта* директивы для сборки с EF6.

Общее правило для изменения пространства имен является то, что любой тип в System.Data.* перемещается в System.Data.Entity.Core.*. Другими словами, просто вставьте **Entity.Core.** После System.Data. Пример:

- System.Data.EntityException = > System.Data. **Entity.Core.** EntityException  
- System.Data.Objects.ObjectContext = > System.Data. **Entity.Core.** Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core.** Objects.DataClasses.RelationshipManager  

Эти типы находятся в *Core* пространства имен, так как они не используются непосредственно для большинства приложений на основе DbContext. Некоторые типы, которые были частью System.Data.Entity.dll по-прежнему используются часто, так и непосредственно для приложений на основе DbContext и поэтому не были перемещены в *Core* пространства имен. Эти особые значения приведены ниже.

- System.Data.EntityState = > System.Data. **Сущности.** EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Этот класс был переименован; класс со старым именем все еще существует и работает, но он теперь помечен как устаревший.  
- System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**  
  > [!NOTE]
  > Этот класс был переименован; класс со старым именем все еще существует и работает, но теперь помечены как устаревшие.)  
- Пространственные классов (например, DbGeography, DbGeometry) перешли из System.Data.Spatial = > System.Data. **Сущности.** Пространственных

> [!NOTE]
> Некоторые типы в пространстве имен System.Data находятся в System.Data.dll, которая не является сборкой EF. Эти типы не были перемещены и поэтому их пространства имен остаются неизменными.
