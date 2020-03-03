---
Description: В этом руководстве приведен список сторонних продуктов и установщиков для упаковки классических приложений.
title: Упаковка классического приложения с помощью сторонних установщиков
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 010e14fa34852656dc0a117d84dae2b73e594e7d
ms.sourcegitcommit: 4d912f89e385268757e87bf8fd9ca1828b99e109
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2020
ms.locfileid: "77544777"
---
# <a name="package-a-desktop-app-using-third-party-installers"></a>Упаковка классического приложения с помощью сторонних установщиков

Ниже приведен список популярных сторонних продуктов и установщиков, которые поддерживают возможность упаковки классического приложения. Их можно использовать для создания установщиков MSI или пакетов приложения в несколько щелчков мышью. Поскольку мы не создаем документацию по использованию этих средств, посетите их веб-сайты, чтобы получить дополнительные сведения.

## <a name="advanced-installer"></a>Advanced Installer

Компания Caphyon предоставляет бесплатное средство упаковки классических приложений с графическим интерфейсом, которое поможет вам создать пакет приложения для Windows для вашего приложения всего в несколько щелчков мышью. Оно может использовать любые установщики, даже те, которые работают в автоматическом режиме, и проверяет, подходит ли приложение для упаковки. Desktop App Converter также интегрируется с Hyper-V и [VMware](https://www.vmware.com/). Это означает, что вы можете использовать собственные виртуальные машины, не скачивая соответствующий образ [Docker](https://docs.docker.com/), который может иметь размер более 3 ГБ.

<img width="20%" src="images/Advanced_Installer_Vertical.png">

Вы можете использовать [Advanced Installer](https://www.advancedinstaller.com/) для создания MSI и [пакетов приложений для Windows](https://www.advancedinstaller.com/uwp-app-package.html) из существующих проектов. Advanced Installer также можно использовать для импорта пакетов приложений для Windows, созданных с помощью Desktop App Converter (Майкрософт). После импорта их можно обслуживать с помощью визуальных средств, разработанных специально для приложений UWP.

Advanced Installer также предоставляет расширение для Visual Studio 2017 и 2015, которое можно использовать для [сборки и отладки приложений моста для классических приложений](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Краткий обзор см. в этом [видео](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be).

> [!TIP]
> Ознакомьтесь с недавно выпущенным выпуском [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

## <a name="cloudhouse-compatibility-containers"></a>Контейнеры совместимости Cloudhouse

Для корпоративных клиентов, имеющих линейку бизнес-приложений, не совместимых с Windows 10 и Windows 10 S, контейнеры совместимости Cloudhouse обеспечивают возможность работы приложений для Windows XP и Windows 7 в Windows 10, а также позволяют преобразовать их для работы на универсальной платформе Windows (UWP) для доставки через Microsoft Store для бизнеса или Microsoft InTune без изменения исходного кода. Зарегистрируйтесь, чтобы получить [бесплатную пробную версию](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse предоставляет автоматический упаковщик для упаковки бизнес-приложений в [контейнеры совместимости](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) в той операционной системе, в которой они работают сейчас (например, Windows XP) и последующей [подготовки для преобразования](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) в UWP. Затем контейнер преобразовывается в новый формат пакета приложения для Windows путем интеграции со средством Desktop App Converter корпорации Майкрософт.

Средство автоматической упаковки использует анализ во время выполнения и установку/захват, чтобы создать контейнер для приложения, включающий файлы приложения и реестра, среды выполнения, зависимости, а также механизм совместимости и перенаправления, который позволяет приложению работать в Windows 10. Контейнер обеспечивает изоляцию приложения и его сред выполнения, чтобы не было конфликтов с другими приложениями, работающими на устройстве пользователя, и не влияли на них.

Дополнительные сведения о способах доставки бизнес-приложений через Microsoft Store для бизнеса можно найти в нашем [блоге, посвященном выпуску](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

## <a name="firegiant"></a>FireGiant

[Расширение FireGiant MSIX](https://www.firegiant.com/products/wix-expansion-pack/msix) позволяет одновременно создавать пакеты приложений для Windows и MSI-пакеты из того же исходного кода WiX. При каждом построении вы можете выбрать Windows 10 с пакетом приложения для Windows и более ранние версии Windows с использованием MSI.

<img width="20%" src="images/FG3rdPartyLogo.png">

Расширение FireGiant MSIX использует статический анализ и интеллектуальную эмуляцию ваших проектов WiX для создания пакетов приложений для Windows без избыточных требований по дисковому пространству и среде выполнения, характерных для контейнеров и виртуальных машин.

Поскольку расширение FireGiant MSIX не преобразует ваш установщик путем его выполнения, вы можете сохранить свой установщик WiX без необходимости постоянно преобразовывать его в пакеты приложений для Windows. Все ваши пользователи с различными версиями Windows получают последние улучшения, поэтому вам не нужно беспокоиться о том, что пакеты приложений для Windows и MSI будут отличаться.

Посмотрите этот [видеоролик](https://www.youtube.com/watch?v=AFBpdBiAYQE) и узнайте, как с помощью нескольких строк кода генеральный директор FireGiant Роб Меншинг (Rob Mensching) создает версию Appx (пакет приложения для Windows) для популярного архиватора с открытым исходным кодом 7-Zip и затем вносит улучшения и в приложение для Windows, и в пакеты MSI путем внесения изменений в одном и том же исходном коде WiX.

## <a name="installshield"></a>InstallShield

InstallShield предоставляет единое решение для разработки установщиков MSI и EXE, создания пакетов для универсальной платформы Windows (UWP) и приложений для Windows Server (WSA), а также виртуализации приложений с минимальным объемом сценариев, кода и переработки.

<img width="20%" src="images/InstallShield-logo.jpg">

Сканирование проекта InstallShield позволяет вам сэкономить многие часы работы путем автоматического выявления потенциальных проблем совместимости вашего приложения и пакетов UWP и WSA.

Подготовка для Microsoft Store и упрощение процесса установки программного обеспечения в Windows 10 путем создания пакетов приложений UWP из существующих проектов InstallShield. Одновременная сборка установщика Windows и пакетов приложений UWP для поддержки всех сценариев развертывания, необходимых вашим клиентам. Поддержка развертываний Nano Server и Windows Server 2016 путем сборки пакетов WSA из существующих проектов InstallShield.

Разработка установки по модулям для облегчения развертывания и обслуживания и дальнейшее объединение компонентов и зависимостей во время сборки в один пакет приложения UWP для Microsoft Store. Для прямого распространения без использования Store пакеты приложений UWP и другие зависимости можно сгруппировать с использованием установщика Suite/Advanced UI.

Подробные сведения см. в этой [электронной книге](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

## <a name="pace-suite"></a>PACE Suite

[PACE Suite](https://pacesuite.com/) — это средство упаковки приложений, которое позволяет преобразовывать классические приложения в приложения для универсальной платформы Windows.

<img width="20%" src="images/PACE.png">

Благодаря PACE Suite не нужно создавать специальные среды упаковки или устанавливать дополнительные компоненты Windows SDK. PACE Suite может независимо создавать пакеты приложений для Windows в стандартной среде упаковки в Windows 10 или Windows Server 2016. Ознакомьтесь с этим [иллюстрированным примером](https://pacesuite.com/convert-exe-to-appx/) чтобы узнать, какой подход используется в PACE Suite для перепаковки установщика в пакет приложения для Windows.

Помимо создания пакетов приложений для Windows, PACE Suite можно использовать для создания пакетов установщика Windows (MSI), исправлений (MSP), преобразований (MST) и пакетов App-V. Когда речь идет о создании MSI, PACE Suite помогает управлять обновлениями, параметрами разрешений, настраиваемыми действиями, сценариями и т д. Приложения также можно публиковать напрямую в System Center Configuration Manager.

Все возможности упаковки приложений можно просмотреть в статье [PACE Suite features](https://pacesuite.com/features/) (Компоненты PACE Suite).

## <a name="rad-studio"></a>RAD Studio

См. [RAD Studio от Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="raypack-studio"></a>RayPack Studio

Решение для упаковки [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio) от Raynet поддерживает создание пакетов для классических приложений, как один из нескольких возможных результатов применения эффективной, быстро настраиваемой платформы преобразования и повторной упаковки.

<img width="20%" src="images/RaynetLogo_v3.png">

Существующие виртуальные среды (VMware Workstation, Hyper-V) можно использовать для выполнения автоматического/группового преобразования без длительной настройки среды. Компонент studio ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) выполняет предварительные проверки и тесты на совместимость, чтобы подтвердить пригодность программного обеспечения для преобразования. Кроме того, пользователи теперь могут выполнять полные проверки на совместимость и наличие конфликтов для различных выпусков Windows 10, включая юбилейное обновление и обновление Creators.

Помимо программных пакетов в формате APPX/UWP для Windows 10, RayPack Studio также позволяет создавать классические пакеты установщика Windows (MSI), исправления (MSP), преобразования (MST) и пакеты App-V. Кроме того, данное решение включает набор программных продуктов и компонентов для упаковки профессионального программного обеспечения предприятий. RayPack Studio позволяет выполнять не только упаковку и виртуализацию программного обеспечения, но и все связанные с упаковкой задачи: проверку приложений и пакетов на совместимость и наличие конфликтов ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), оценку программного обеспечения ([RayEval](https://raynet.de/Raynet-Products/RayEval)), контроль качества ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

С помощью системы корпоративных рабочих процессов [RayFlow](https://raynet.de/Raynet-Products/RayFlow) от Raynet пользователи могут эффективно работать с программным обеспечением на каждом этапе жизненного цикла корпоративного приложения, включая заказ пакетов, оценку, анализ, упаковку, контроль качества, тесты на приемлемость для пользователя и разработку. Все пакеты и форматы можно хранить и развертывать непосредственно в SCCM или с помощью других решений. Весь процесс жизненного цикла приложения контролируется и управляется через систему RayFlow. Можно также интегрировать любые системы заказов, например ServiceNow. Используя свои инструменты для поставщиков услуг, Raynet создает фабрики по упаковке программного обеспечения по всему миру.

Убедитесь во всем сами, получив [лицензию на бесплатную пробную версию](https://raynet.de/contact?init=license) RayPack Studio и RayFlow от Raynet. Подробнее см. на сайте [www.raynet.de](https://raynet.de/home).

Связанные ссылки:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Лицензия на бесплатную пробную версию: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)
