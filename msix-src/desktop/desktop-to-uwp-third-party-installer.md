---
Description: В этом руководстве список продуктов независимых производителей и установщики классических приложениях пакета.
title: Упаковка классического приложения с помощью сторонних установщиков
ms.date: 06/17/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 586314c33e3b33bdcfdd15ad95d415233355c99e
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555549"
---
# <a name="package-a-desktop-app-using-third-party-installers"></a>Упаковка классического приложения с помощью сторонних установщиков

Ниже приведен список популярных продуктов независимых производителей и установщики, которые поддерживают возможность упаковки классического приложения. Их можно использовать для создания установщиков MSI или пакетов упакованного приложения в несколько щелчков мышью. Поскольку мы не создаем документацию по использованию этих средств, посетите их веб-сайты, чтобы получить дополнительные сведения.

## <a name="advanced-installer"></a>Advanced Installer

Компания Caphyon предоставляет средство упаковки классических приложений, которое поможет вам создать пакет приложения для Windows для вашего приложения всего в несколько щелчков мышью. Данное средство имеет графический интерфейс и распространяется бесплатно. Его можно использовать любой установщик; даже те, которые выполняются в автоматическом режиме и выполняет проверку проверьте, подходит ли приложение для упаковки. Desktop App Converter также интегрируется с Hyper-V и [VMware](https://www.vmware.com/). Это означает, что можно использовать собственные виртуальные машины, не скачивая соответствующий образ [Docker](https://docs.docker.com/), который может иметь размер более 3 ГБ.

<img width="20%" src="images/Advanced_Installer_Vertical.png">

Вы можете использовать [Advanced Installer](https://www.advancedinstaller.com/) для создания MSI и [пакетов приложений для Windows](https://www.advancedinstaller.com/uwp-app-package.html) из существующих проектов. Advanced Installer также можно использовать для импорта пакетов приложений для Windows, которые созданы с помощью Desktop App Converter (Microsoft). После импорта их можно обслуживать с помощью визуальных средств, разработанных специально для приложений UWP.

Advanced Installer также предоставляет расширение для Visual Studio 2017 и 2015, которое можно использовать для [сборки и отладки приложений, перенесенных с помощью моста для классических приложений](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Краткий обзор см. в этом [видео](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be).

> [!TIP]
> Ознакомьтесь с недавно выпущенным выпуском [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

## <a name="cloudhouse-compatibility-containers"></a>Контейнеры совместимости Cloudhouse

Для корпоративных клиентов, имеющих линейку бизнес-приложений, не совместимых с Windows 10 и Windows 10 S, контейнеры совместимости Cloudhouse обеспечивают возможность работы приложений для Windows XP и Windows 7 в Windows 10, а также позволяют преобразовать их для работы на универсальной платформе Windows (UWP) для доставки через Microsoft Store для бизнеса или Microsoft InTune без изменения исходного кода. Зарегистрируйтесь для получения [бесплатной пробной версии](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse предоставляет автоматическое упаковщика для упаковки ряда бизнес-приложений в [контейнеры совместимости](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) в операционных системах, которые в настоящее время на приложения (например: Windows XP), а затем [подготовить его для преобразования](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) для универсальной платформы Windows. Затем контейнер преобразовывается в новый формат пакета приложения для Windows путем интеграции со средством Desktop App Converter корпорации Microsoft.

Средство автоматической упаковки использует анализ во время выполнения и установку/захват, чтобы создать контейнер для приложения, включающий файлы приложения и реестра, среды выполнения, зависимости, а также механизм совместимости и перенаправления, который позволяет приложению работать в Windows 10. Контейнер обеспечивает изоляцию приложения и его сред выполнения, так что они не влияют и не конфликтуют с другими приложениями, работающими на устройстве пользователя.

Дополнительные сведения о способах доставки бизнес-приложений через Microsoft Store для бизнеса можно найти в нашем [Блоге по выпуску](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

## <a name="firegiant"></a>FireGiant

[MSIX FireGiant расширение](https://www.firegiant.com/products/wix-expansion-pack/msix) позволяет одновременно создавать пакеты приложений Windows и пакеты MSI из одного исходного кода WiX. При каждой сборке, можно работать с Windows 10 с помощью пакета приложения Windows и более ранних версиях Windows с помощью MSI.

<img width="20%" src="images/FG3rdPartyLogo.png">

Расширение MSIX FireGiant использует статический анализ и интеллектуальная эмуляции проекты WiX для создания пакетов приложений Windows избавит дискового пространства и среды выполнения контейнеров или виртуальных машин.

Так как расширение MSIX FireGiant не преобразует установщиком, запустив его, можно поддерживать установщик WiX без необходимости повторно преобразовать его в пакеты приложений Windows. Все ваши пользователи с различными версиями Windows получают последние нововведения в приложении, и вам не нужно беспокоиться о том, что MSI и пакеты приложений для Windows будут отличаться.

Ознакомьтесь с этим [видео](https://www.youtube.com/watch?v=AFBpdBiAYQE) и увидеть, как в несколько строк кода FireGiant генеральный Директор Роба Меншинга создается Appx (пакет приложений Windows) версия средства сжатия 7-Zip и популярные открытым исходным кодом, а затем как он улучшает оба приложения Windows и Пакеты MSI с изменениями в тот же исходный код WiX.

## <a name="installaware"></a>InstallAware

InstallAware, с помощью [финансовое](https://www.installaware.com/press-room.htm) быстро поддерживать инновации корпорации Майкрософт, создает [пакеты приложений Windows (мост для классических приложений)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), MSI (установщик Windows), и EXE-файла (машинный код) пакеты из одного источника.

<img width="20%" src="images/installaware.png">

InstallAware предоставляет бесплатные расширения InstallAware для версий Visual Studio 2012 – 2017. Их можно использовать для создания пакетов приложений для Windows одним щелчком мыши непосредственно из [панели инструментов Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).

Можно также импортировать какой-либо настройки, даже если у вас нет исходного кода для установки, с помощью PackageAware (фиксирует установка без моментальных снимков), или мастера импорта базы данных (для всех установщиков MSI и MSM модули слияния). Для поддержания базы импортированных данных и ее улучшения либо в графическом виде, либо с помощью скриптов можно использовать [средства с графическим интерфейсом](https://www.installaware.com/scripting-two-way-integrated-ide.htm).

[Дополнительные параметры создания APPX](https://www.installaware.com/mhtml5/desktop/appx.htm) помогают вам нацеливать отправки в Microsoft Store и создавать двоичные файлы пакета приложения для Windows для распространения неопубликованных приложений среди конечных пользователей. Можно даже создать пакеты установщика WSA (приложения Windows Server), предназначенных для развертывания на **Nano Server** из одного источника, а также с полной поддержкой [автоматизации командной строки](https://www.installaware.com/scripting-automation-interface.htm), кроме для графического пользовательского интерфейса.

InstallAware также [открытый](https://www.installaware.com/gnu.asp) **библиотеки построитель APPX**вместе с приложения командной строки пример лицензии GNU Affero GPL. Они предназначены для использования на платформах с открытым исходным кодом, таких как WiX.

## <a name="installshield"></a>InstallShield

InstallShield предоставляет единое решение для разработки установщиков MSI и EXE, создания пакетов для универсальной платформы Windows (UWP) и приложений для Windows Server (WSA), а также виртуализации приложений с минимальным объемом сценариев, кода и переработки.

<img width="20%" src="images/InstallShield-logo.jpg">

Сканирование проекта InstallShield позволяет вам сэкономить многие часы работы путем автоматического выявления потенциальных проблем совместимости вашего приложения и пакетов UWP и WSA.

Подготовка для Microsoft Store и упрощение процесса установки программного обеспечения в Windows 10 путем создания пакетов приложений UWP из существующих проектов InstallShield. Одновременная сборка установщика Windows и пакетов приложений UWP для поддержки всех сценариев развертывания, требуемых вашим клиентам. Поддержка развертываний Nano Server и Windows Server 2016 путем сборки пакетов WSA из существующих проектов InstallShield.

Разработка установки по модулям для облегчения развертывания и обслуживания и дальнейшее объединение компонентов и зависимостей во время сборки в один пакет приложения UWP для Microsoft Store. Для прямого распространения за пределами Store объединить пакеты приложений универсальной платформы Windows и другие зависимости, вместе с установщика Suite/Дополнительно пользовательского интерфейса.

Подробные сведения см. в этой [электронной книге](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

## <a name="pace-suite"></a>PACE Suite

[PACE Suite](https://pacesuite.com/) — это средство упаковки приложений, которое позволяет преобразовывать классические приложения в приложения для универсальной платформы Windows.

<img width="20%" src="images/PACE.png">

Благодаря PACE Suite не нужно готовить специальные среды упаковки или устанавливать дополнительные компоненты пакета Windows SDK. PACE Suite может независимо создавать пакеты приложений для Windows в стандартной среде упаковки в Windows 10 или Windows Server 2016. Ознакомьтесь с этим [иллюстрированным примером](https://pacesuite.com/convert-exe-to-appx/) чтобы узнать, какой подход используется в PACE Suite для перепаковки установщика в пакет приложения для Windows.

Помимо создания пакетов приложений для Windows PACE Suite можно использовать для создания пакетов установщика Windows (MSI), исправлений (MSP), преобразований (MST) и пакетов App-V. Когда речь идет о создании MSI, PACE Suite помогает управлять обновлениями, параметрами разрешений, настраиваемыми действиями, сценариями и т д. Приложения также можно публиковать напрямую в System Center Configuration Manager.

Все возможности упаковки приложений можно просмотреть в разделе [Возможности PACE Suite](https://pacesuite.com/features/).

## <a name="rad-studio"></a>RAD Studio

См. [RAD Studio от Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="raypack-studio"></a>RayPack Studio

Raynet за упаковочный решения, [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), поддерживает создание пакетов приложений для настольных компьютеров, как один из нескольких возможных результатов преобразования эффективно и легко настроить и распаковка framework.

<img width="20%" src="images/RaynetLogo_v3.png">

Существующие виртуальные среды (VMware Workstation, Hyper-V) могут быть использованы для выполнения автоматического/группового преобразования без длительной настройки среды. Компонент студии ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) выполняет предварительные проверки и тесты на совместимость, чтобы подтвердить пригодность программного обеспечения для преобразования. Кроме того, пользователи теперь могут выполнять полные проверки на совместимость и наличие конфликтов для различных выпусков Windows 10, включая юбилейное обновление и обновление Creators.

Помимо программных пакетов в формате APPX/UWP для Windows 10, RayPack Studio также позволяет создавать классические пакеты установщика Windows (MSI), исправления (MSP), преобразования (MST) и пакеты App-V. Кроме того, данное решение включает набор программных продуктов и компонентов для упаковки профессионального программного обеспечения предприятий. RayPack Studio позволяет выполнять не только упаковку и виртуализацию программного обеспечения, но и все связанные с упаковкой задачи: проверку приложений и пакетов на совместимость и наличие конфликтов ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), оценку программного обеспечения ([RayEval](https://raynet.de/Raynet-Products/RayEval)), контроль качества ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

С помощью системы корпоративных рабочих процессов [RayFlow](https://raynet.de/Raynet-Products/RayFlow) от Raynet пользователи могут эффективно работать с программным обеспечением на каждом этапе жизненного цикла корпоративного приложения, включая заказ пакетов, оценку, анализ, упаковку, контроль качества, тесты на приемлемость для пользователя и разработку. Все пакеты и форматы можно хранить и развертывать непосредственно в SCCM или с помощью других решений. Прохождение приложением всего жизненного цикла отслеживается и контролируется через систему RayFlow. Можно также интегрировать любые системы заказов, например ServiceNow. Используя свои инструменты для поставщиков услуг, Raynet создает фабрики по упаковке программного обеспечения по всему миру.

Убедитесь во всем сами, получив [лицензию на бесплатную пробную версию](https://raynet.de/contact?init=license) RayPack Studio и RayFlow от Raynet. Подробнее см. на сайте [www.raynet.de](https://raynet.de/home).

Ссылки по теме:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Лицензия на бесплатную пробную версию: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)
