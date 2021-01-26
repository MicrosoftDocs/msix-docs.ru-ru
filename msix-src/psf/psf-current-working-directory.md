---
description: Содержит инструкции по применению исправления рабочего каталога среды выполнения с помощью платформы поддержки пакетов.
title: Платформа поддержки пакетов — адресная привязка рабочего каталога
ms.date: 12/16/2020
ms.topic: article
keywords: Windows 10, UWP, ПСФ, платформа поддержки пакетов, Рабочий каталог, WorkingDirectory, msix
ms.localizationpriority: medium
ms.openlocfilehash: f1a3fcceb9502b2dd819e825c40a0443dfff440d
ms.sourcegitcommit: 059f215a0804adeeefeaaa09b376684caa4382eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98768880"
---
# <a name="package-support-framework---working-directory-fixup"></a>Платформа поддержки пакетов — адресная привязка рабочего каталога

## <a name="investigation"></a>Исследование

Приложения Windows будут перенаправлять определенные каталоги, связанные с приложением, в папку контейнера приложений Windows. Если приложение создает вложенную папку ( `C:\Program Files\Vendor\subfolder` ) в составе установки и позже вызывает эту вложенную папку, не удается найти каталог, так как он не существует.

С помощью платформы поддержки пакетов (ПСФ) для решения этой проблемы можно внести улучшения в пакет приложения Windows. Сначала необходимо задать сбой и пути к каталогам, запрашиваемые приложением.

#### <a name="capture-the-windows-app-failure"></a>Запись ошибки приложения Windows

Фильтрация результатов — это необязательный шаг, позволяющий упростить просмотр ошибок, связанных с приложениями. Для этого будут созданы два правила фильтрации. Первый фильтр включения для имени процесса приложения, второй — включение всех неудачных результатов.

1. Скачайте и извлеките [Монитор процессов Sysinternals](/sysinternals/downloads/procmon) в каталог **к:\псф\процессмонитор** .
1. Откройте проводник Windows и перейдите к папке извлеченных мониторов процессов SysInternals
1. Дважды щелкните файл "монитор процессов SysInternals" (procmon.exe), чтобы запустить приложение.
1. При появлении запроса UAC нажмите кнопку **Да** .
1. В окне Обработка фильтра монитора выберите первое раскрывающееся меню с именем **архитектура**.
1. В раскрывающемся меню выберите пункт **имя процесса** .
1. В следующем раскрывающемся меню убедитесь, что для него задано значение **равно**.
1. В текстовом поле введите имя процесса приложения (например: PSFSample.exe).
    :::image type="content" source="images/procmon-filterdialog-processname-psfsample.png" alt-text="Пример окна &quot;обработка фильтра&quot; монитора с именем приложения":::
1. Нажмите кнопку **Добавить**.
1. В окне Обработка фильтра монитора выберите первое раскрывающееся меню **имя процесса**.
1. Выберите **результат** в раскрывающемся меню.
1. В следующем раскрывающемся меню выберите его и выберите пункт **не** из раскрывающегося меню.
1. В текстовом поле введите: **Success**.
    :::image type="content" source="images/procmon-filterdialog-result-success.png" alt-text="Пример окон фильтра монитора процессов с результатами":::
1. Нажмите кнопку **Добавить**.
1. Нажмите кнопку **ОК** .
1. Запустите приложение Windows, активируйте ошибку и закройте приложение Windows.

#### <a name="review-the-windows-app-failure-logs"></a>Проверка журналов сбоев приложений Windows

После захвата процессов приложений Windows необходимо исследовать результаты, чтобы определить, связана ли ошибка с рабочим каталогом.

1. Проверьте результаты в мониторе процесса SysInternals, выполнив поиск ошибок, описанных в приведенной выше таблице.
1. Если в результатах отображается результат **"имя не найдено"** , отображаются сведения **"требуемый доступ:..."** для конкретного приложения, предназначенного для каталога за пределами **"C:\Program филес\виндовсаппс \\ ... \\ "** (как показано на рисунке ниже), вы успешно определили сбой, связанный с рабочим каталогом, используйте статью [ПСФ support-FileSystem для получения]() инструкций по применению исправления ПСФ к приложению.
:::image type="content" source="images/procmon-psfsampleapp-readfailure.png" alt-text="Отображает сообщение об ошибке, следящий в мониторе процессов SysInternals, для сбоя записи в каталог.":::

## <a name="resolution"></a>Решение

Приложения Windows будут перенаправлять определенные каталоги, связанные с приложением, в папку контейнера приложений Windows. Если приложение создает вложенную папку ( `C:\Program Files\Vendor\subfolder` ) в составе установки и позже вызывает эту вложенную папку, не удается найти каталог, так как он не существует.

Чтобы устранить проблему, связанную с тем, что приложение Windows ссылается на неверный рабочий каталог, необходимо выполнить следующие четыре шага:

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
                "workingDirectory": ""
            }
        ],
        "processes": [
            {
                "executable": ""
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

1. Скопируйте значение в поле **идентификатор** , расположенное в файле **AppxManifest.xml** , расположенном в, в `Package.Applications.Application` поле Application ID (идентификатор приложения) в файле **config.js** .
    :::image type="content" source="images/appxmanifest-application-id.png" alt-text="Image изогнутыми расположение идентификатора в файле AppxManifest.":::

1. Скопируйте путь, относительный от пакета, из поля **исполняемый** файл, расположенного в файле **AppxManifest.xml** , расположенном в, в `Package.Applications.Application` поле **исполняемый** файл приложения в **config.js** .
    :::image type="content" source="images/appxmanifest-application-executable.png" alt-text="Image изогнутыми расположение исполняемого файла в файле AppxManifest.":::

1. Скопируйте связанный с пакетом родительский путь из поля **исполняемого** файла, расположенного в файле **AppxManifest.xml** , расположенном в, в `Package.Applications.Application` поле Application **WorkingDirectory** (приложения) в файле **config.js** .
    :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Image изогнутыми расположение рабочего каталога в файле AppxManifest.":::

1. Скопируйте имя исполняемого файла из поля **исполняемого** файла, расположенного в файле **AppxManifest.xml** , расположенном в, в `Package.Applications.Application` поле процессы **исполняемого** файла в **config.js** .
    :::image type="content" source="images/appxmanifest-application-processexecutable.png" alt-text="Image изогнутыми расположение исполняемого файла процесса в файле AppxManifest.":::

1. Сохраните обновленный **config.jsв** файле.
    ```json
    {
        "applications": [
            {
            "id": "PSFSample",
            "executable": "VFS/ProgramFilesX64/PS Sample App/PSFSample.exe",
            "workingDirectory": "VFS/ProgramFilesX64/PS Sample App/"
            }
        ],
        "processes": [
            {
            "executable": "PSFSample"
            }
        ]
    }
    ```

1. Скопируйте следующие три файла из платформы поддержки пакетов, основанной на исполняемой архитектуре приложения, в корневую папку промежуточного приложения Windows. Следующие файлы находятся в папке **.\микрософт.паккажесуппортфрамеворк. <Version> \bin**.

    | Приложение (x64) | Приложение (x86) | 
    |-------------------|-------------------|
    | PSFLauncher64.exe | PSFLauncher32.exe |
    | PSFRuntime64.dll  | PSFRuntime32.dll  |
    | PSFRunDll64.exe   | PSFRunDll32.exe   |


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