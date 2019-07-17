---
layout: LandingPage
title: Документация по MSIX
description: Документация по MSIX — обновленному безопасному формату упаковки, сочетающему в себе технологии установки с использованием MSI-файлов, APPX-файлов, App-V и ClickOnce.
ms.date: 09/07/2018
ms.topic: landing-page
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 4e31f0cf5af1ec9a637d0c59ed54f26c3ed80d31
ms.sourcegitcommit: 70036a054d1a5da24f535ddd4ea0fae78c30d469
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238952"
---
# <a name="msix-documentation"></a>Документация по MSIX
MSIX — это безопасный и надежный формат упаковки, созданный на основе сочетания технологий установки с помощью MSI-файлов, APPX-файлов, App-V и ClickOnce. 

 > [!TIP]
 > Посетите страницу [технического сообщества MSIX](https://aka.ms/msixcommunity), чтобы просматривать обсуждения и следить за новостями.
 
:::row:::
    :::column:::
        ### Package existing Windows apps
Средство упаковки MSIX позволяет обновить существующие пакеты приложений Win32 в формате MSIX. [Подробнее](mpt-overview.md)
<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Получить средство</a></p></div>
    :::column-end:::
    :::column:::
        ### Use MSIX anywhere Разработчики, работающие на разных платформах, могут использовать пакет MSIX SDK для обработки пакетов, предназначенных для распространения через Microsoft Store или в других сетях распространения содержимого. [Подробнее...](msix-sdk/sdk-overview.md)
    :::column-end:::
:::row-end:::
:::row:::
        :::column:::
        ### Установка пакетов приложений MSIX Установщик приложений позволяет устанавливать и обновлять пакеты приложений MSIX, размещенные как локально, так и в сетях распространения содержимого. [Подробнее...](app-installer/app-installer-root.md)
    :::column-end:::
    :::column:::
    ### Платформа поддержки пакетов Платформа поддержки пакетов позволяет приложению обходить некоторые ограничения современных сред выполнения, гарантируя надлежащую работу приложения без необходимости изменения его исходного кода. [Подробнее](psf/package-support-framework-overview.md)
    :::column-end:::
:::row-end:::

<br>

<a name="get-started"></a>
<h2>ИТ-специалистам</h2>
<hr />
<ul class="panelContent cardsF">
<li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Get started icon" src="/media/common/i_get-started.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Начало работы</h3>                                
                <p>
                                    <a href="/en-us/windows/msix/packaging-tool/create-app-package-msi-vm" data-linktype="absolute-path">Обновление существующих установщиков до MSIX</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
            <li class="x-hidden-focus">
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Design icon" src="/media/common/i_management.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Упаковка</h3>
                                <p>
                                    <a href="/en-us/windows/msix/mpt-overview" data-linktype="absolute-path">Использование средства упаковки MSIX</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/msix/packaging-tool/package-conversion-cli" data-linktype="absolute-path">Использование командной строки</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render" data-linktype="absolute-path">Подписывание пакетов</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
            <li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Develop icon" src="/media/common/i_code-edit.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Проверить</h3>
                                <p>
                                    <a href="/en-us/windows/uwp/publish/package-flights?context=/windows/msix/render" data-linktype="absolute-path">Фокус-тестирование в Microsoft Store</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/porting/desktop-to-uwp-test-windows-s?context=/windows/msix/render#first-download-the-policies-and-then-choose-one" data-linktype="absolute-path">Тестирование для Windows 10 в S-режиме</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
            <li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Develop Games icon" src="/media/common/i_build.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Распространение</h3>
                                <p>
                                    <a href="/en-us/windows/uwp/publish/app-submissions?context=/windows/msix/render" data-linktype="absolute-path">Microsoft Store</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/publish/distribute-lob-apps-to-enterprises?context=/windows/msix/render" data-linktype="absolute-path">Microsoft Store для бизнеса</a>
                                </p>
                                <p>
                                    <a href="/en-us/sccm/apps/understand/introduction-to-application-management?context=/windows/msix/render" data-linktype="absolute-path">System Center Configuration Manager</a>
                                </p>
                                <p>
                                    <a href="/en-us/intune/introduction-intune?context=/windows/msix/render" data-linktype="absolute-path">Microsoft Intune</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/msix/app-installer/app-installer-file-overview" data-linktype="absolute-path">Распространение по другим каналам</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
</ul>

<h2>Разработчик</h2>
<hr />

<ul class="panelContent cardsF">
<li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Get started icon" src="/media/common/i_get-started.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Начало работы</h3>
                                <p>
                                    <a href="/en-us/windows/msix/overview">Сведения об MSIX</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/msix/app-package-updates?context=/windows/msix/render">Обновления приложений MSIX</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
    <li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Design icon" src="/media/common/i_management.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Сборка</h3>
                                <p>
                                    <a href="/en-us/windows/uwp/packaging/packaging-uwp-apps?context=/windows/msix/render" data-linktype="absolute-path">Упаковка приложения с помощью Visual Studio</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/packaging/manual-packaging-root?context=/windows/msix/render" data-linktype="absolute-path">Средства упаковки вручную</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render" data-linktype="absolute-path">Подписывание пакета приложения</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
    <li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Develop icon" src="/media/common/i_code-edit.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Проверить</h3>
                                <p>
                                    <a href="/en-us/windows/uwp/debug-test-perf/windows-app-certification-kit?context=/windows/msix/render" data-linktype="absolute-path">Комплект сертификации приложений для Windows</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/debug-test-perf/device-portal?context=/windows/msix/render" data-linktype="absolute-path">Портал устройств Windows</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/publish/package-flights?context=/windows/msix/render" data-linktype="absolute-path">Фокус-тестирование пакетов</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/porting/desktop-to-uwp-test-windows-s?context=/windows/msix/render" data-linktype="absolute-path">Тестирование для Windows 10 в S-режиме</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
    <li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="Develop Games icon" src="/media/common/i_build.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Распространение</h3>
                                <p>
                                    <a href="/en-us/windows/uwp/publish/?context=/windows/msix/render" data-linktype="absolute-path">Microsoft Store</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/publish/distribute-lob-apps-to-enterprises?context=/windows/msix/render" data-linktype="absolute-path">Microsoft Store для бизнеса</a>
                                </p>
                                <p>
                                    <a href="/en-us/windows/uwp/packaging/create-appinstallerfile-vs?context=/windows/msix/render" data-linktype="absolute-path">Распространение по другим каналам</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
    <li>
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter">
                                <div class="cardImage">
                                    <img alt="API Ref icon" src="/media/common/i_api-reference.svg?branch=master" data-linktype="absolute-path">
                                </div>
                            </div>
                            <div class="cardText">
                                <h3>Справочник по API и схемам</h3>
                                <p>
                                    <a href="/uwp/api/windows.management.deployment?context=/windows/msix/render" data-linktype="absolute-path">API диспетчера пакетов</a>
                                </p>
                                <p>
                                    <a href="/uwp/schemas/appxpackage/appx-package-manifest?context=/windows/msix/render" data-linktype="absolute-path">Схема манифеста приложения</a>
                                </p>
                                <p>
                                    <a href="/uwp/schemas/appinstallerschema/schema-root?context=/windows/msix/render" data-linktype="absolute-path">Схема файла Установщика приложений</a>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
</ul>

<br>

## <a name="msix-training-videos"></a>Учебные видеоматериалы о MSIX
:::row:::
    :::column:::
    >[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3ig2l]
#### <a name="msix-overview"></a>Общие сведения об MSIX
MSIX предоставляет множество преимуществ, связанных с управлением жизненным циклом приложений. Узнайте, как получить максимум пользы от использования MSIX в компании, а также обеспечить преимущества для разработчиков, ИТ-специалистов и пользователей.
    :::column-end:::
    :::column:::
    >[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3i5DH]
#### <a name="msix-for-developers"></a>MSIX для разработчиков
Узнайте, как разработчики могут воспользоваться преимуществами MSIX.
    :::column-end:::
    :::column:::
    >[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3iiD5]
#### <a name="evolving-and-enhancing-desktop-apps-with-msix"></a>Доработка и оптимизация классических приложений с помощью MSIX
MSIX позволяет дорабатывать и улучшать классические приложения. Узнайте, как использовать новые API, новые элементы управления и другие возможности. 
    :::column-end:::
:::row-end:::


