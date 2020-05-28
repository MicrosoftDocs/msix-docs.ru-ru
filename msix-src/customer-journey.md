---
title: Пути клиентов
description: В этой статье описывается MSIXное путешествие.
author: dianmsft
ms.date: 05/12/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: aa87e92294a970f82f1653cb1510c7eddf371dc2
ms.sourcegitcommit: 8b02a0d376ab16873607ed84d1560b1c69c9e915
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83632616"
---
# <a name="customer-journeys-with-msix"></a>Пути взаимодействия клиентов с MSIX

Многие клиенты успешно используют MSIX для улучшения развертывания приложений Windows. В этой статье рассматриваются некоторые пути взаимодействия клиентов с MSIX.

* [Узнайте, как база данных систел позволяет гибко развертывать приложения Windows.](customer-journey.md#db-systel)
* [Узнайте, как образовательное окружение Онтарио (ЕКНО) улучшает доставку приложений для настольных компьютеров и процесс установки для учащихся Онтарио](customer-journey.md#ecno)
* [Узнайте, как Schneiderные электрические технологии разрабатывают и развертывают классические приложения WPF](customer-journey.md#schneider-electric)

## <a name="db-systel"></a>Систел базы данных

![Логотип систел базы данных](images/DB_logo_red_outlined_200px_rgb.png)

База данных систел GmbH, находящийся в головном офисе Франкфурт am, — это полностью принадлежащий дочерний элемент базы данных AG и цифровой партнер для всех групповых компаний. Подразделение Бахн AG — это вторая крупнейший транспортная компания в мире, которая является крупнейшим оператором раилвай и владельцем инфраструктуры в Европе. Он работает с большими частями немецкого раилвай и несет примерно 2 000 000 000 пассажиров ежегодно.

В базе данных систел сотрудники, имеющие около 4 600 человек, работают с 600 бизнес-приложений, 100 000 рабочих станций ПК, 93 000 VoIP УАТС и 200 000 Mobile и т. д. Они обрабатывали всю ИТ – инфраструктуру компании, от традиционных ИТ служб до разработки всех внутренних приложений, используемых для управления всеми аспектами системы раилвай. 

Для баз данных систел классические приложения являются важнейшим компонентом инфраструктуры. Они являются основным интерфейсом для многих критически важных задач, от управления сотрудниками до обеспечения правильной работы системы раилвай. База данных систел разработка, сопровождение и развертывание всего 600 клиентских приложений с клиентскими системами FAT и 200 приложений Java.

Когда речь идет о настольных приложениях, они столкнулись с проблемами в основном в следующих разделах:

* Многие из своих серверных приложений создаются, тестируются и предоставляются через конвейеры сборки с помощью высокоавтоматизированных процессов — несколько раз в день (DevOps). Однако текущие технологии развертывания стали невозможными для достижения той же цели в классических приложениях Windows.
* Многие команды участвуют в процессе разработки и развертывания, который задерживается в течение нескольких дней, прежде чем пользователи смогут получить последние версии программного обеспечения.
* Старый процесс развертывания программного обеспечения был очень трудоемким, длинным и дорогостоящим.
* Многие бизнес-приложения основаны на технологии веб-запуска Java, которая была прекращена.

В результате этих трудностей в базе данных систел была возможность предоставлять краткосрочные обновления с большим количеством усилий. Это стало критически важной проблемой, так как многие из своих приложений полагаются на конкретную версию программного обеспечения в серверной части. Важно, чтобы клиентское программное обеспечение для пользователя обновлялось непосредственно после обновления программного обеспечения в серверной части. Если это не так, то возможность пользователя работать с рассматриваемым программным обеспечением больше не гарантируется и может привести к нарушению работы служб салазок.

Систел базы данных в первый момент слышал о MSIX, когда они начали изучать, как заменить технологию веб-запуска Java. MSIX был в расчете, поскольку он позволит им создавать автономные приложения, которые не зависят от устанавливаемого среда выполнения Java. Это позволит сократить время координации и синхронизации команд, а также привести к более стабильной работе. Когда они начали экспериментировать с MSIX, они быстро понимают, что это правильная технология, не предназначенная для поддержки миграции Java, но также для решения основных проблем, связанных с упаковкой и распространением.

MSIX базы данных с поддержкой систел для:

* Упростите традиционную упаковку и развертывание программных пакетов.
* Разрешите разработчикам программного обеспечения выполнять весь комплексный процесс создания и развертывания программного обеспечения вместо делегирования процессов упаковки и распространения специальным группам.
* Автоматизируйте существующие процессы, выполняемые вручную, благодаря конвейерам.
* Включите скорость и простоту в развертывании классических приложений Windows, что приведет к значительному снижению затрат благодаря новому подходу к самообслуживанию.

*"В прошлом у нас было бы много групп, вовлеченных в процесс, и пришло время до достижения точки, в которой наши диспетчеры приложений могут использовать и обновлять наше программное обеспечение. Следовательно, мы смогли распределить выпуски (обновления) нашим клиентам с большой наработкой. После очень информативного и плодотворный MSIXного семинара вместе с экспертами Майкрософт мы уверены, что мы можем переделать процесс подготовки программного обеспечения в базе данных систел с помощью MSIX Self-Service. MSIX предлагает большие преимущества в качестве формата контейнера с точки зрения скорости и простоты. Сами руководители приложений могут упаковывать программное обеспечение с помощью MSIX и предоставлять свое программное обеспечение через магазин ".*
-Маркус Соманн, консультант по программному обеспечению в современной группе развертывания в базе данных

Система DB интегрирует MSIX в процесс сборки как формат контейнера. Большинство приложений, включая множество критически важных приложений, будут перенесены в формат MSIX. Это сделает процесс подготовки программного обеспечения более простым, быстрым и дешевле. Благодаря MSIX и современной группе развертывания руководители приложений теперь могут предоставлять обновления программного обеспечения для конечных пользователей напрямую и много раз в день.

*"Технология MSIX позволяет нам применять подход DevOps, даже если мы предоставляем клиентское программное обеспечение, а не облачное программное обеспечение. Это стало неудовлетворительным до тех пор, пока не будет очень недавно ".* -Маркус Соманн, консультант по программному обеспечению в современной группе развертывания в базе данных

## <a name="ecno"></a>екно

![Логотип ЕКНО](images/ECNO_masterlogo.png)

Сетевая вычислительная сеть Онтарио (ЕКНО) — это организация, которая совместно находит и выполняет эффективные ИТ-решения для Онтарио 72 K-12 учебных карт (2 000 000 учащихся). ЕКНО работает с новейшими вычислительными технологиями, так как они относятся к целям обучения Онтарио. Они позволяют ИТ и поделиться рекомендациями ИТ в инфраструктуре с учебными досками.

Школьные доски находятся в автономном режиме и могут использовать собственные стратегии ИТ для того, что лучше подходит для них. Например, одна школьная доска использует Configuration Manager исключительно для распространения приложений, а другая использует ее только для создания образов машин и небольшой доли приложений.

В рамках ЕКНОа группа проекта общих технологических служб оценивает, тестирует и упаковывает Каталог приложений, в 450 сторонних приложений, для спектра потребностей в течение K – 12 учащихся и персонала.

На сегодняшний день группа разработчиков ЕКНО STS преобразовала большую часть своих приложений с помощью средства упаковки MSIX и средства преобразования, которое входит в область элементов MSIX.

ЕКНО в настоящее время содержит около 170 приложений в центре партнеров, с десятью школьными досками, связанными с учетной записью LOB. В настоящее время два из них разрешают доступ на уровне самообслуживания к нашим приложениям в своем магазине. Третья из них уже Configuration Manager связан с магазином, и им требуется добавить доступ к нашим приложениям. Шесть дополнительных задач переходят в Управление программным обеспечением и их назначение через Intune и запрашивают примеры приложений для работы. И другая доска школы завершает проект, чтобы переместить свои iPad в Intune в качестве средства MDM, а затем переместит устройства Windows в Intune (где они ищут использование MSIX и магазина в качестве модели распространения программного обеспечения). Они предполагают, что дополнительные доски начинают переключаться по мере перехода к использованию Intune/Configuration Manager.

*"Это push-уведомление в правильном направлении для того, чтобы мы доставляли программного обеспечения нашим пользователям, которые не могут находиться в сети учебного заведения на данный момент во время КОВИД-19. Пакеты AppV были boonы в течение года, и эта следующая итерация, использующая MSIX, позволит нам обслуживать наших сотрудников и учащихся с помощью программного обеспечения, которое им требуется, используя систему доставки, которая является современной и легкой из любого Интернет-подключения в мире ".*
-Джейсон Дэвид Фланнери, системный инженер, Симкое округ район учебного заведения

Вы можете легко настроить и легко выполнить развертывание после регистрации устройств в Windows Intune с помощью автопилота.

Какие преимущества получает ЕКНО благодаря использованию MSIX?

* В качестве лидера в развертывании программного обеспечения для школьных систем в Онтарио MSIX предоставляет простой переход от наших процессов App-V.
* Позволяет нашим доскам учебных заведений оставаться в курсе современных средств развертывания программного обеспечения и управления конечными точками.
* Для нашей группы проекта была увеличена производительность, что обеспечивает упрощенное развертывание и поддержку для наших школьных систем.

*"По мере того, как наша группа ИТ стремится стать более гибкой, особенно теперь с обучением по расстоянию, мы будем полагаться на облако более и многое другое.  Следующим логическим развитием нашей Организации является управление облаком. Мы хотим, чтобы наши ИТ-специалисты по ИТ были сосредоточены на успешной реализации технологии, а не на развертывании и устранении неполадок. По мере того как мы работаем над развертыванием наших устройств и приложений, мы будем тесно работать с группой STS ЕКНО, чтобы интегрировать приложения, которые мы используем в своем классе, в наш облачный подход. "*
-Иван Хеучерт, руководитель информации, Петербораугх Виктория Нортумберленде Кларингтон Касолик округ учебная доска

## <a name="schneider-electric"></a>Schneider Электрический

![Schneider Электрический](images/Logo_SE_Green_RGB-Screen.png)

Schneider Электрический разрабатывает GIS программное обеспечение для поддержки небольших и крупных служебных программ (Электрический, газа, воды, волокон, коаксиального) в США и международных.

Работая с Майкрософт, мы хотели бы перенести наши приложения WPF в новейшую технологию установки, MSIX. У нас есть конкретный сценарий использования на основе клиента; нам нужно часто выпускать и развертывать их в качестве программной компании с учетом специфики разработки, но каждая служебная компания хочет управлять изменениями и по отдельности определять, какая версия приложения будет выпущена для пользователей и когда. Мы тесно работали с корпорацией Майкрософт, чтобы удовлетворить эти требования клиентов. После завершения нашего обновления до MSIX мы сможем удалить некоторые сторонние зависимости и выполнить шаг ближе к обновлению наших приложений до .NET Core 3,0.

Используя MSIX, мы упростили наш стек разработки. Мы будем использовать технологии Майкрософт из кода (C#/ВПФ) для сборки (Azure dev Ops) в установщик (MSIX). Одним из основных преимуществ MSIX является то, что разработчики могут легко разрабатывать, отлаживать и тестировать установщик непосредственно из Visual Studio. Это значительно упрощает устранение неполадок при установке и позволило нам тесно работать с корпорацией Майкрософт.

*"Поддержка Майкрософт, которую мы перейдем в MSIX, была неценной. У нас есть уникальный вариант использования, и корпорация Майкрософт помогает нам перемещаться по всем препятствиям, которые мы обнаружили при обновлении наших приложений. Разработчики очень рады переместить наши приложения в MSIX ".* – Дарлене Раулеау от Schneider Электрический