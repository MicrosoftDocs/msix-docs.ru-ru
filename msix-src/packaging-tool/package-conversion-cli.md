---
title: Создание пакета с помощью интерфейса командной строки
description: В этой статье описывается, как создать пакет MSIX с помощью интерфейса командной строки для средства упаковки MSIX.
ms.date: 02/11/2019
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4ebdb73779821c1ba0dc5fbae844b02df4d204cb
ms.sourcegitcommit: 71c49de79d061909fb1ab632ec7550227d2287bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754822"
---
# <a name="conversion-with-command-line-interface-cli"></a>Преобразование с помощью интерфейса командной строки (CLI)

## <a name="automate-conversion-of-windows-installers-to-msix-packages-using-scripts"></a>Автоматизация преобразования установщиков Windows в пакеты MSIX с помощью сценариев

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Получить средство упаковки MSIX</a></p></div>

Это средство упаковки MSIX поддерживает интерфейс командной строки для создания пакетов приложений MSIX. Это позволяет автоматизировать процесс повторной упаковки установщиков приложений и выполнение групповых преобразований.

Примеры скриптов Bash и PowerShell, демонстрирующих способы автоматизации процесса упаковки, подписывания, администрирования и распространения пакетов MSIX см. в папке [scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) набора средств MSIX.

## <a name="use-the-command-line-interface-cli-with-the-command-prompt"></a>Использование интерфейса командной строки (CLI) в командной строке

Чтобы создать новый пакет MSIX для приложения, выполните команду `MsixPackagingTool.exe create-package` в окне командной строки администратора.

Ниже приведены параметры, которые можно передать в качестве аргументов командной строки:

|**Параметр** |    **Описание**|
|---------|---------|
|-? --help  |Отображает справочные сведения.|
|--template | [Обязательно] Путь к XML-файлу с шаблоном преобразования, который содержит сведения о пакете и настройки преобразования.|
|--virtualMachinePassword   | [Необязательно] Пароль виртуальной машины, которая будет применяться в среде преобразования. Примечания. файл шаблона должен содержать элемент VirtualMachine, а атрибуту Settings:: Алловпромптфорпассворд не должно быть присвоено значение true.|
|-v --verbose   |[Необязательно] Вывод подробных журналов в консоль.|

Примеры.

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> Преобразование App-V 5.x сейчас можно выполнять с помощью командной строки. При этом поддерживаются некоторые возможности. 

**Файл шаблона преобразования**

``` xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018">

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
    EnforceMicrosoftStoreVersioningRequirements="false">
        
    <!--Note: Exclusion items are optional and if declared take precedence over the default tool exclusion items
        <ExclusionItems>
            <FileExclusion ExcludePath="[{CryptoKeys}]" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Crypto" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Search\Data" />
            <FileExclusion ExcludePath="[{Cookies}]" />
            <FileExclusion ExcludePath="[{History}]" />
            <FileExclusion ExcludePath="[{Cache}]" />
            <FileExclusion ExcludePath="[{Personal}]" />
            <FileExclusion ExcludePath="[{Profile}]\Local Settings" />
            <FileExclusion ExcludePath="[{Profile}]\NTUSER.DAT.LOG1" />
            <FileExclusion ExcludePath="[{Profile}]\ NTUSER.DAT.LOG2" />
            <FileExclusion ExcludePath="[{Recent}]" />
            <FileExclusion ExcludePath="[{Windows}]\debug" />
            <FileExclusion ExcludePath="[{Windows}]\Logs\CBS" />
            <FileExclusion ExcludePath="[{Windows}]\Temp" />
            <FileExclusion ExcludePath="[{Windows}]\WinSxS\ManifestCache" />
            <FileExclusion ExcludePath="[{Windows}]\WindowsUpdate.log" />
        <FileExclusion ExcludePath="[{Windows}]\Installer" />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\$Recycle.Bin " />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\System Volume Information" />
        <FileExclusion ExcludePath="[{AppVPackageDrive}]\Config.Msi" />
            <FileExclusion ExcludePath="[{AppData}]\Microsoft\AppV" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Antimalware" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Windows Defender" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Windows Defender" />
        <FileExclusion ExcludePath="[{ProgramFiles}]\WindowsApps" />
            <FileExclusion ExcludePath="[{Local AppData}]\Temp" />
        <FileExclusion ExcludePath="[{Local AppData}]\Microsoft\Windows" />
        <FileExclusion ExcludePath="[{Local AppData}]\Packages" />

            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware Setup" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Security Client" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\AppV" />
        </ExclusionItems>
    -->
        
    </Settings>

    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute and is optional
    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService ="true"/>
    -->

    <SaveLocation
    PackagePath="C:\users\user\Desktop\MyPackage.msix" 
    TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />
    
    
    <!--NOTE: This section specifies that the conversion will be run on a local Virtual Machine.
    <VirtualMachine Name="vmname" Username="vmusername" />
    -->

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">
        
    <!--NOTE: This ID will be used if the Application entry detected matches the specified ExecutableName
        <Applications>
            <Application
                Id="MyApp1"
                Description="MyApp"
                DisplayName="My App"
                ExecutableName="MyApp.exe"/>
        </Applications>
    -->

    <!--NOTE: This is optional as “runFullTrust” capability is added by default during conversion
        <Capabilities>
            <Capability Name="runFullTrust" />
        </Capabilities>
    -->
        
    </PackageInformation>
</MsixPackagingToolTemplate>
```

**Справка по параметрам шаблонов преобразования**

Ниже приведен полный список параметров, которые можно использовать в файле шаблона преобразования.

|**Параметры преобразования** |   **Описание** |
|---------|---------|
|Параметры:: Алловтелеметри |        [Необязательно] Включает ведение журнала телеметрии для этого вызова средства.|
|Параметры:: Аппляллпрепарекомпутерфиксес     |  [Необязательно] Применяет все рекомендуемые исправления в ходе подготовки компьютера. Не может быть задан, если используются другие атрибуты.|
|Параметры:: Женератекоммандлинефиле  |  [Необязательно] Копирует входные данные из файла шаблона в каталог SaveLocation для последующего использования.|
|Параметры:: Алловпромптфорпассворд |        [Необязательно] Указывает средству выводить пользователю запрос на ввод паролей для виртуальной машины и на подписывание сертификата (если это обязательно, но не задано).|
|Параметры:: Енфорцемикрософтстореверсионингрекуирементс |       [Необязательно] Указывает средству применять схему управления версиями, которая обязательна при развертывании из Microsoft Store и Microsoft Store для бизнеса.|
|ExclusionItems |       [Необязательно] 0 или более элементов FileExclusion или RegistryExclusion. Все элементы FileExclusion должны быть указаны до элементов RegistryExclusion.|
|ExclusionItems::FileExclusion |        [Необязательно] Файл, который будет исключен при создании пакета.|
|ExclusionItems::FileExclusion::ExcludePath |       Путь к файлу, который будет исключен при создании пакета.|
|ExclusionItems::RegistryExclusion |        [Необязательно] Раздел реестра, который будет исключен при создании пакета.|
|Ексклусионитемс:: Регистрексклусион:: Ексклудепас |      Путь к реестру, который будет исключен при создании пакета.|
|PrepareComputer::DisableDefragService |        [Необязательно] Отключает программу дефрагментации Windows на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|Препарекомпутер:: Дисаблевиндовссеарчсервице |        [Необязательно] Отключает Windows Search на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|Препарекомпутер:: Дисаблесмшостсервице     |  [Необязательно] Отключает службу узла агента SMS на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|Препарекомпутер:: Дисаблевиндовсупдатесервице   |  [Необязательно] Отключает клиентский компонент Центра обновления Windows на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|SaveLocation     |[Необязательно] Элемент для указания расположения, в котором средство сохранит пакет. Если значение не указано, пакет будет сохранен в папке на рабочем столе.         |
|SaveLocation::PackagePath     |[Необязательно] Путь к файлу или папке с сохраненным пакетом MSIX.         |
|SaveLocation::TemplatePath    |[Необязательно] Путь к файлу или папке с сохраненным шаблоном CLI.    |
|Installer::Path |      Путь к установщику приложения.|
|Installer::Arguments |     [Необязательно] Аргументы, передаваемые установщику.  Средство автоматически и без участия пользователя запустит установщики MSI с аргументом "/qn /norestart INSTALLSTARTMENUSHORTCUTS=1 DISABLEADVTSHORTCUTS=1". Примечание. необходимо передать аргументы, чтобы принудительно запустить установщик при использовании установщиков. exe.|
|Installer::InstallLocation |       используемых Полный путь к корневой папке приложения для установленных файлов, если они были установлены (например, "C:\Program Files (x86) \Мяппинсталллокатион").|
|VirtualMachine |       [Необязательно] Элемент, который указывает, что преобразование будет выполнено на локальной виртуальной машине.|
|VrtualMachine::Name     |  Имя виртуальной машины, которая будет использоваться в среде преобразования.|
|VirtualMachine::Username |     [Необязательно] Имя пользователя виртуальной машины, которая будет применяться в среде преобразования.|
|PackageInformation::PackageName |      Имя пакета MSIX.|
|PackageInformation::PackageDisplayName |       Отображаемое имя пакета MSIX.|
|PackageInformation::PublisherName |        Издатель пакета MSIX.|
|PackageInformation::PublisherDisplayName |     Отображаемое имя издателя пакета MSIX.|
|PackageInformation::Version |      Номер версии пакета MSIX.|
|Паккажеинформатион:: Маинпаккаженамеформодификатионпаккаже |       [Необязательно] Имя удостоверения пакета для имени основного пакета. Используется при создании пакета изменений, который принимает зависимость основного (родительского) приложения.|
|Приложения |     [Необязательно] 0 или более элементов приложения для настройки записей приложения в пакете MSIX.|
|Application::Id |      Идентификатор приложения MSIX. Этот идентификатор будет использоваться для обнаруженной записи приложения, которая соответствует указанному параметру ExecutableName. Вы можете указать несколько значений идентификаторов приложений для исполняемых файлов в пакете.<br/><br/>Это значение является уникальным идентификатором приложения в пакете. Оно также иногда называется связанным с пакетом идентификатором приложения (PRAID). Идентификатор должен быть уникальным в пределах пакета (в одном пакете нельзя более одного раза использовать один и тот же идентификатор). Но он не должен быть глобально уникальным. Другой пакет в системе может использовать такой же идентификатор.<br/><br/>Эта строка содержит буквенно-цифровые поля, разделенные точками. Каждое поле должно начинаться с буквенного символа ASCII. Их нельзя использовать в качестве значений полей: CON, PRN, AUX, NUL "," COM1 "," COM2 "," COM3 "," COM4 "," COM5 "," COM6 "," COM7 "," COM8 "," COM9 "," LPT1 "," LPT2 "," LPT3 "," LPT4 "," LPT5 "," LPT6 "," LPT7 "," LPT8 "и" LPT9 ".|
|Application::ExecutableName |      Имя исполняемого файла для приложения MSIX, которое будет добавлено в манифест пакета. Соответствующая запись приложения будет игнорироваться, если приложение с таким именем не обнаружено.|
|Application::Description |     [Необязательно] Описание приложения MSIX. Если этот параметр не указан, будет использоваться отображаемое имя приложения. Это описание будет использоваться для обнаруженной записи приложения, которая соответствует указанному параметру ExecutableName.|
|Application::DisplayName    |  Отображаемое имя приложения дял пакета MSIX. Это отображаемое имя будет использоваться для обнаруженной записи приложения, которая соответствует указанному параметру ExecutableName.|
|Характеристики |     [Необязательно] 0 или более элементов Capability для добавления пользовательских функций в пакет MSIX. Элемент Capability "runFullTrust" добавляется по умолчанию в процессе преобразования.|
|Capability::Name | Элемент Capability, который нужно добавить в пакет MSIX.

