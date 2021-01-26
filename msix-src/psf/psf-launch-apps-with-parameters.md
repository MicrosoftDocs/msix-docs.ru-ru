---
description: Содержит рекомендации по применению исправлений среды выполнения с платформой поддержки пакетов.
title: Платформа поддержки пакетов — запуск приложений Windows с параметрами
ms.date: 12/16/2020
ms.topic: article
keywords: Windows 10, UWP, ПСФ, платформа поддержки пакетов, аргументы, средство запуска приложений, параметры, ярлык, msix
ms.localizationpriority: medium
ms.openlocfilehash: 4eaee3a4b514527500f715ec3390fbad136790c7
ms.sourcegitcommit: 059f215a0804adeeefeaaa09b376684caa4382eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98768852"
---
# <a name="launching-windows-app-with-parameters"></a>Запуск приложения Windows с параметрами

## <a name="investigation"></a>Исследование

Некоторые средства запуска приложений Windows в меню "Пуск" должны использовать параметры для передачи исполняемому файлу при запуске приложения Windows. Для этого сначала необходимо найти средство запуска, требующее параметра, перед интеграцией приложения Windows с помощью платформы поддержки пакетов.

#### <a name="identifying-windows-app-launcher-parameter-requirement"></a>Определение требования к параметру средства запуска приложений Windows

1. Установите приложение Windows на тестовом компьютере.
1. Откройте **меню "Пуск" Windows**.
1. В меню "Пуск" выберите средство запуска приложений Windows.
1. Если приложение запускается, у вас нет каких-либо проблем (проверьте все связанные средства запуска приложений Windows в меню "Пуск").
1. Удалите приложение Windows с тестового компьютера.
1. С помощью установочного носителя Win32 установите приложение на тестовый компьютер.
1. Откройте **меню "Пуск" Windows**.
1. Перейдите в меню "Пуск" и щелкните правой кнопкой мыши приложение Windows.
1.   >>  В раскрывающемся меню выберите пункт **открыть расположение файла** .
1. Щелкните правой кнопкой мыши первый связанный с ним ярлык приложения (повторите следующие три шага для всех связанных с ним ярлыков приложений).
1. В раскрывающемся меню выберите пункт **Свойства** .
1. Проверьте значение в текстовом поле справа от **целевого объекта**. Если после пути к файлу приложения в списке присутствует параметр, в этом приложении в :::image type="content" source="images/contosoapp-fileproperties-target-parameter.png" alt-text="качестве примера используется окно свойств файла с параметром в Target"::: .

1. Запишите значение параметра для использования в будущем.


## <a name="resolution"></a>Решение

Приложения Windows будут перенаправлять определенные каталоги, связанные с приложением, в папку контейнера приложений Windows. Если приложение пытается выполнить запись в контейнер приложения Windows, будет вызвана ошибка, и запись не будет выполнена. 

Чтобы устранить проблему, связанную с невозможностью записи приложения Windows в контейнер приложения Windows, необходимо выполнить следующие четыре шага:

1. [Размещение приложения Windows в локальном каталоге](#stage-the-windows-app)
1. [Создание Config.jsи вставка необходимых файлов ПСФ](#create-and-inject-required-psf-files)
1. [Обновление файла AppxManifest приложения Windows](#update-appxmanifest)
1. [Переупаковка и Подписывание приложения Windows](#re-package-the-application)

В приведенных выше шагах приведены инструкции по извлечению содержимого приложения Windows в локальный промежуточный каталог, внедрению файлов адресной привязки ПСФ в промежуточный каталог приложений Windows, настройке средства запуска приложений для указания на средство запуска ПСФ, а также настройке ПСФ config.jsв файле для перенаправления средства запуска ПСФ в приложение, в котором указан рабочий каталог.

### <a name="download-and-install-required-tools"></a>Скачивание и установка необходимых средств
Этот процесс поможет вам получить сведения об использовании следующих средств:
- Клиентский инструмент NuGet
- Платформа поддержки пакетов
- Пакет SDK для Windows 10 (последняя версия)
- Монитор процессов SysInternals

Ниже приведены пошаговые инструкции по скачиванию и установке необходимых средств.

1. Скачайте последнюю версию [средства клиента NuGet](https://www.nuget.org/downloads)(не для предварительного просмотра) и сохраните **nuget.exe** в `C:\PSF\nuget` папке.
1. Скачайте платформу поддержки пакетов с помощью [NuGet](/nuget/install-nuget-client-tools#nugetexe-cli) , выполнив следующую команду в окне администрирования PowerShell:
    ```PowerShell
    Set-Location "C:\PSF"
    .\nuget\nuget.exe install Microsoft.PackageSupportFramework
    ``` 
    
1. Скачайте и установите [набор средств разработки программного обеспечения Windows 10 (пакет SDK для Win 10)](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).
    1. Скачайте [пакет SDK для Win 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).
    1. Запустите **winsdksetup.exe** , скачанный на предыдущем шаге.
    1. Нажмите кнопку **Далее**.
    1. Выберите только следующие три функции для установки:
        - Средства подписывания Windows SDK для классических приложений;
        - Windows SDK для приложений UWP C++;
        - Локализация пакета SDK признана для приложений UWP
    1. Нажмите кнопку **Установить**.
    1. Нажмите кнопку **ОК** .

### <a name="stage-the-windows-app"></a>Размещение приложения Windows
При промежуточном хранении приложения Windows мы будем извлекать и распаковывать содержимое приложения Windows в локальный каталог. После распаковки приложения Windows в промежуточное расположение файлы адресной привязки ПСФ могут быть добавлены к исправлению нежелательных возможностей.

1. Откройте окно администрирования PowerShell.
1. Задайте следующие переменные, предназначенные для конкретного файла приложения, и версию пакета SDK для Windows 10:
    ```PowerShell
    $AppPath          = "C:\PSF\SourceApp\PSFSampleApp.msix"         ## Path to the MSIX App Installer
    $StagingFolder    = "C:\PSF\Staging\PSFSampleApp"                ## Path to where the MSIX App will be staged
    $OSArchitecture   = "x$((gwmi Win32_Processor).AddressWidth)"    ## Operating System Architecture
    $Win10SDKVersion  = "10.0.19041.0"                               ## Latest version of the Win10 SDK
    ```

1. Распакуйте приложение Windows в промежуточную папку, выполнив следующий командлет PowerShell:
    ```PowerShell
    ## Sets the directory to the Windows 10 SDK
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    
    ## Unpackages the Windows App to the staging folder
    .\makeappx.exe unpack /p "$AppPath" /d "$StagingFolder"
    ```


### <a name="create-and-inject-required-psf-files"></a>Создание и вставка необходимых файлов ПСФ
Чтобы применить корректирующие действия к приложению Windows, необходимо создать **config.jsдля** файла и предоставить сведения о сбое средства запуска приложений Windows. Если возникают проблемы с несколькими запусками приложений Windows, **config.jsв** файле можно обновить с помощью нескольких записей.

После обновления **config.js** в файле **config.jsв** файле и вспомогательные файлы адресной привязки ПСФ должны быть перемещены в корневую папку пакета приложения Windows. 


1. Откройте Visual Studio Code (VS Code) или любой другой текстовый редактор.
1. Создайте новый файл, выбрав меню **файл** в верхней части VS Code и выбрав в раскрывающемся меню пункт **создать файл** .
1. Сохраните файл как **config.js**. в меню **файл** в верхней части окна VS Code выберите пункт **сохранить** в раскрывающемся меню. В окне Сохранить как перейдите в каталог промежуточного хранения приложения Windows (**к:\псф\стагинг\псфсамплеапп**) и задайте **имя файла** в формате `config.json` . Нажмите кнопку **Сохранить**.
1. Скопируйте следующий код в созданный **config.js** файла.
    ```JSON
    {
        "applications": [
            {
                "id": "",
                "executable": "",
                "arguments": ""
            }
        ]
    }
    ```

1. Откройте промежуточный файл **appxmanifest** приложения Windows, расположенный в папке размещения приложений windows (**C:\PSF\Staging\PSFSampleApp\AppxManifest.xml**) с помощью VS Code или другого текстового редактора.
    ```xml
    <Applications>
        <Application Id="PSFSAMPLE" Executable="VFS\ProgramFilesX64\PS Sample App\PSFSample.exe" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements BackgroundColor="transparent" DisplayName="PSFSample" Square150x150Logo="Assets\StoreLogo.png" Square44x44Logo="Assets\StoreLogo.png" Description="PSFSample">
            <uap:DefaultTile Wide310x150Logo="Assets\StoreLogo.png" Square310x310Logo="Assets\StoreLogo.png" Square71x71Logo="Assets\StoreLogo.png" />
        </uap:VisualElements>
        </Application>
    </Applications>
    ```

1. Задайте `applications.id` для параметра *config.js* значение, совпадающее со значением, которое содержится в поле **Applications.Application.ID** файла *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-id.png" alt-text="Image изогнутыми расположение идентификатора в файле AppxManifest.":::

1. Задайте `applications.executable` значение в *config.js* , чтобы указать относительный путь к приложению, расположенному в поле **Applications.Application.Exeкутабле** файла *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-executable.png" alt-text="Image изогнутыми расположение исполняемого файла в файле AppxManifest.":::

1. Задайте `applications.arguments` значение в *config.js* в для соответствия аргументу, используемому для запуска приложения. См. записанное значение из последнего шага в руководстве по [требованию к параметрам средства запуска приложений Windows](#identifying-windows-app-launcher-parameter-requirement) .

1. Задайте `applications.workingdirectory` значение в *config.js* , чтобы указать относительный путь к папке, найденный в поле **Applications.Application.Exeкутабле** файла *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Image изогнутыми расположение рабочего каталога в файле AppxManifest.":::

1. Сохраните обновленный **config.jsв** файле.
    ```json
    {
        "applications": [
            {
            "id": "PSFSample",
            "executable": "VFS/ProgramFilesX64/PS Sample App/PSFSample.exe",
            "arguments": "/bootfromsettingshortcut"
            }
        ]
    }
    ```

1. Скопируйте следующие четыре файла из платформы поддержки пакетов, основанной на исполняемой архитектуре приложения, в корневую папку промежуточного приложения Windows. Следующие файлы находятся в папке **.\микрософт.паккажесуппортфрамеворк. <Version> \bin**.

    | Приложение (x64)          | Приложение (x86)          | 
    |----------------------------|----------------------------|
    | PSFLauncher64.exe          | PSFLauncher32.exe          |
    | PSFRuntime64.dll           | PSFRuntime32.dll           |
    | PSFRunDll64.exe            | PSFRunDll32.exe            |


### <a name="update-appxmanifest"></a>Обновление AppxManifest
После создания и обновления **config.jsв** файле необходимо обновить **AppxManifest.xml** приложения Windows для каждого средства запуска приложений Windows, которое было включено в **config.js**. Теперь приложения **appxmanifest** должны ориентироваться на **PSFLauncher.exe** , связанные с архитектурой приложений.


1. Откройте проводник и перейдите к папке промежуточного MSIX приложения (**к:\псф\стагинг\псфсамплеапп**).
1. Щелкните правой кнопкой мыши **AppxManifest.xml** и выберите в раскрывающемся меню пункт **Открыть с кодом** (при необходимости можно открыть другой текстовый редактор).

1. Обновите файл AppxManifest.xml следующими сведениями:
    ```xml
    <Package ...>
    ...
    <Applications>
        <Application Id="PSFSample"
                    Executable="PSFLauncher32.exe"
                    EntryPoint="Windows.FullTrustApplication">
        ...
        </Application>
    </Applications>
    </Package>
    ```


### <a name="re-package-the-application"></a>Повторно упаковать приложение
Все исправления применены, теперь приложение Windows можно повторно упаковать в MSIX и подписать с помощью сертификата подписи кода.

1. Откройте окно администрирования PowerShell.
1. Задайте следующие переменные:
    ```PowerShell
    $AppPath          = "C:\PSF\SourceApp\PSFSampleApp_Updated.msix" ## Path to the MSIX App Installer
    $CodeSigningCert  = "C:\PSF\Cert\CodeSigningCertificate.pfx"     ## Path to your code signing certificate
    $CodeSigningPass  = "<Password>"                                 ## Password used by the code signing certificate
    $StagingFolder    = "C:\PSF\Staging\PSFSampleApp"                ## Path to where the MSIX App will be staged
    $OSArchitecture   = "x$((gwmi Win32_Processor).AddressWidth)"    ## Operating System Architecture
    $Win10SDKVersion  = "10.0.19041.0"                               ## Latest version of the Win10 SDK
    ```

1. Выполните переупаковку приложения Windows из промежуточной папки, выполнив следующий командлет PowerShell:
    ```PowerShell
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    .\makeappx.exe pack /p "$AppPath" /d "$StagingFolder"
    ```

1. Подпишите приложение Windows, выполнив следующий командлет PowerShell:
    ```PowerShell
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    .\signtool.exe sign /v /fd sha256 /f $CodeSigningCert /p $CodeSigningPass $AppPath
    ```