---
title: Стратегия развития Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: fd9086c9911cdb0890117d44c2787780aad9a7cb
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821365"
---
# <a name="entity-framework-core-roadmap"></a>Стратегия развития Entity Framework Core

> [!IMPORTANT]
> Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.

## <a name="last-release-ef-core-21"></a>Последний выпуск: EF Core 2.1

Стабильная версия EF Core 2.1 была выпущена 30 мая 2018 г. Дополнительные сведения об этом выпуске см. в статье [Новые возможности в EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

## <a name="future-releases"></a>Будущие выпуски

### <a name="ef-core-22"></a>EF Core 2.2

Этот выпуск включает множество исправлений ошибок и сравнительно небольшой набор новых функций. Сведения об этом выпуске включены в [объявление о стратегическом плане по EF Core 2.2](https://github.com/aspnet/Announcements/issues/308). 

### <a name="ef-core-30"></a>EF Core 3.0

Хотя [процесс планирования выпуска](#release-planning-process), следующего после 2.2, не закончен, сейчас мы планируем опубликовать основной выпуск вместе с .NET Core 3.0 и ASP.NET 3.0. 

Рабочие элементы, предварительно назначенные для будущего выпуска, можно просмотреть в [этом запросе в средстве отслеживания проблем](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="schedule"></a>Расписание

Расписание выпусков EF Core синхронизируется с [расписанием .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) и [расписанием ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Невыполненная работа

Мы используем [контрольные точки невыполненной работы](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) при отслеживании проблем для ведения подробного списка проблем и возможностей. Клиенты могут комментировать и голосовать за них.

Как правило, мы не закрываем проблемы, которые мы рассчитываем когда-нибудь решить, или с которыми может справиться кто-то из сообщества, но это не означает намерения разрешить их за конкретный период времени до тех пор, пока мы не добавим их к определенной контрольной точке как часть [процесса планирования выпусков](#release-planning-process).

Если мы не планируем когда-либо реализовать возможность, проблема, скорее всего, будет закрыта. Закрытие проблемы может быть пересмотрено позднее, если у нас появятся новые сведения о ней.

Другими словами, у нас недостаточно сведений о будущем, чтобы иметь возможность сказать, что возможность X будет добавлена за время или в версии Y. Как и в случае с любым программным обеспечением, приоритеты, расписания выпусков и доступные ресурсы могут измениться в любой момент.

## <a name="release-planning-process"></a>Процесс планирования выпусков

Мы часто получаем вопросы о том, как мы выбираем возможности, которые будут добавлены в конкретный выпуск. Наш список невыполненной работы не становится автоматически планом выпусков. Наличие возможности в EF6 также не означает автоматически, что она должна быть реализована в EF Core.

Сложно подробно описать весь процесс планирования выпуска, отчасти от того,что большую часть занимает обсуждение конкретных функций, возможностей и приоритетов, отчасти от того, что сам процесс обычно эволюционирует от выпуска к выпуску. Тем не менее можно относительно легко привести список наиболее распространенных вопросов, на которые мы пытаемся найти ответ при принятии решения о том, над чем мы будем работать на следующем этапе:

1. **Сколько разработчиков, по нашему мнению, будет использовать новую возможность, насколько она улучшит их приложения и облегчит процесс разработки?** Для этого мы собираем отзывы из множества источников, комментарии и голосования за проблемы — один из таких источников.

2. **Какие существуют обходные пути для использования этой возможности, пока мы ее не реализовали?** Например, многие разработчики используют сопоставление соединяемой таблицы для обхода отсутствия встроенной поддержки связи многие ко многим. Очевидно, что не все разработчики могут использовать эту возможность, но многие сумеют, и это является фактором, который влияет на принятие решения.

3. **Будет ли реализация этой возможности способствовать развитию EF Core, то есть приблизит ли это реализацию других возможностей?** Как правило, мы отдаем предпочтение возможностям, выступающим в качестве строительных блоков для других функций. Например, разделение таблицы для собственных типов продвигает нас в реализации поддержки подхода TPT (одна таблица на тип).

4. **Является ли возможность точкой расширяемости?** Как правило, мы отдаем предпочтение точкам расширяемости, так как они позволяют разработчикам легко реализовывать собственные алгоритмы и использовать некоторые из нереализованных возможностей. Мы планируем реализовать кое-что из этого в начале работы над отложенной загрузкой.

5. **Каков синергетический эффект от использования этой возможности в сочетании с другими продуктами?** Как правило, мы отдаем предпочтение возможностям, позволяющим использовать EF Core с другими продуктами или значительно улучшить процесс использования других продуктов, таких как .NET Core, последняя версия Visual Studio, Microsoft Azure и т. д.

6. **Какова квалификация людей, доступных для работы над этой возможностью, и как лучше всего распорядиться этими ресурсами?** Все члены команды EF и даже участники сообщества обладают разным уровнем опыта в различных областях, и при планировании мы должны это учитывать. Даже если бы мы захотели бросить все силы на работу над конкретной возможностью, такой как преобразование оператора GroupBy или связи многие ко многим, это было бы непрактично.

Как упоминалось ранее, этот процесс развивается от выпуска к выпуску, и в будущем мы хотим добавить больше возможностей членам сообщества разработчиков для участия в разработке плана выпуска, например облегчая рецензирование предложенных проектов возможностей и самого плана выпуска.
