---
title: Пакет создается с помощью интерфейса командной строки
description: Узнайте, как создать пакет MSIX, с помощью интерфейса командной строки.
author: mcleanbyron
ms.author: mcleans
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7dcd522a1c1b79fe59a5b6e1fd04c7aec19ed419
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566508"
---
# <a name="conversion-with-command-line-interface-cli"></a>Преобразование с помощью командной строки (CLI)

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Получить средство упаковки MSIX</a></p></div>
      
Чтобы создать новый пакет MSIX для вашего приложения, выполните команду создания пакета MsixPackagingTool.exe в окне командной строки администратора. 

Ниже приведены параметры, которые могут быть переданы как аргументы командной строки.

|**Параметр** |    **Описание**|
|---------|---------|
|-? --справки  |Показать справочные сведения|
|--шаблон | [обязательно] путь к XML-файле шаблона преобразования, содержащий сведения о пакете и параметры для этого преобразования|
|--virtualMachinePassword   | [необязательно] Пароль для виртуальной машины для использования среды преобразования. Примечания. Файл шаблона должен содержать элемент VirtualMachine и Settings::AllowPromptForPassword атрибут не должен быть установлен в значение true.|
|-v--verbose   |[необязательно] Печать подробных журналов в консоль.|

Примеры

''' командной строки

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```
> [!NOTE]
> App-V 5.x conversion is currently supported to be converted throught the command line. This includes capabilities. 

**Conversion template file**

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

**Ссылка на параметр шаблона преобразования**

Ниже приведен полный список параметров, которые можно использовать в файле преобразования шаблона.

|**ConversionSettings** |   **Описание** |
|---------|---------|
|Параметры:: AllowTelemetry |        [необязательно] Включение регистрации данных телеметрии для вызова инструмента.|
|Параметры:: ApplyAllPrepareComputerFixes     |  [необязательно] Применяет все рекомендуется подготовить компьютер исправления. Невозможно установить, если используются другие атрибуты.|
|Параметры:: GenerateCommandLineFile  |  [необязательно] Копирует входные файлы шаблона в каталог SaveLocation для использования в будущем.|
|Параметры:: AllowPromptForPassword |        [необязательно] Указывает средству предлагать пользователю вводить пароли для виртуальной машины, а также для сертификата для подписи, если он требуется, но не указан.|
|Параметры:: EnforceMicrosoftStoreVersioningRequirements |       [необязательно] Указывает, что инструмент для принудительного применения схемы управления версиями пакетов, необходимых для развертывания из Microsoft Store и Microsoft Store для бизнеса.|
|ExclusionItems |       [необязательно] 0 или более элементов FileExclusion или RegistryExclusion. Все элементы FileExclusion должны находиться перед всеми элементами RegistryExclusion.|
|ExclusionItems::FileExclusion |        [необязательно] Файл, чтобы исключить для упаковки.|
|ExclusionItems::FileExclusion::ExcludePath |       Путь к файлу, чтобы исключить для упаковки.|
|ExclusionItems::RegistryExclusion |        [необязательно] Раздел реестра, чтобы исключить для упаковки.|
|ExclusionItems::RegistryExclusion:: ExcludePath |      Путь реестра для исключения для упаковки.|
|PrepareComputer::DisableDefragService |        [необязательно] Отключает дефрагментации Windows во время преобразования приложения. Если значение false, переопределяет ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsSearchService |        [необязательно] Отключает Windows Search, во время преобразования приложения. Если значение false, переопределяет ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableSmsHostService     |  [необязательно] Отключает узла SMS, во время преобразования приложения. Если значение false, переопределяет ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsUpdateService   |  [необязательно] Отключает обновления Windows во время преобразования приложения. Если значение false, переопределяет ApplyAllPrepareComputerFixes.|
|SaveLocation     |[необязательно] Элемент для указания сохранения место средства. Если не указано, пакет будет сохранен в папке рабочего стола.         |
|SaveLocation::PackagePath     |[необязательно] Путь к файлу или папке, где сохранен полученный пакет MSIX.         |
|SaveLocation::TemplatePath    |[необязательно] Путь к файлу или папке, где сохранен полученный шаблон интерфейса командной строки.    |
|Installer::path |      Путь установщика приложения.|
|Installer::Arguments |     [необязательно] Аргументы, передаваемые в установщик.  Средство автоматически запустится автоматически, используя аргумент MSI-установщики «/qn/norestart / INSTALLSTARTMENUSHORTCUTS = 1 DISABLEADVTSHORTCUTS = 1». ПРИМЕЧАНИЕ. Необходимо передать аргументы для принудительного установщик, чтобы запускаться, если вы используете установщики .exe.|
|Installer::InstallLocation |       [необязательно] Полный путь к корневой папке приложения для установленных файлов, если оно было установлено (например) «C:\Program файлы (x86) \MyAppInstalllocation»).|
|VirtualMachine |       [необязательно] Элемент, чтобы указать, что преобразование будет выполняться на локальной виртуальной машине.|
|VrtualMachine::Name     |  Имя виртуальной машины, используемый для преобразования среды.|
|VirtualMachine::Username |     [необязательно] Имя пользователя для виртуальной машины для использования среды преобразования.|
|PackageInformation::PackageName |      Имя пакета для пакета MSIX.|
|PackageInformation::PackageDisplayName |       Отображаемое имя пакета для пакета MSIX.|
|PackageInformation::PublisherName |        Издатель для MSIX пакета.|
|PackageInformation::PublisherDisplayName |     Отображаемое имя издателя для своего пакета MSIX.|
|PackageInformation::Version |      Номер версии для пакета MSIX.|
|Пакетинформационные:: MainPackageNameForModificationPackage |       [необязательно] Имя удостоверения пакета имя главного пакета. Используется при создании пакета изменения, зависящий от приложения главную (родительскую).|
|Приложения |     [необязательно] 0 или более элементов приложения для настройки записи приложения в пакете MSIX.|
|Application::ID |      Идентификатор приложения для вашего приложения MSIX. Этот идентификатор будет использоваться для входа приложения обнаружена, соответствующий указанным ExecutableName. Может иметь несколько значений идентификатора приложения для исполняемых файлов в пакете.<br/><br/>Это значение — это уникальный идентификатор приложения в пределах пакета. Это значение, иногда называется идентификатором зависящий от пакета приложения (PRAID). Идентификатор должен быть уникальным в пределах пакета (тот же идентификатор не может использоваться более одного раза в том же пакете). Тем не менее идентификатор не являться уникальным глобально. Возможно, другой пакет в системе, которая использует тот же идентификатор.<br/><br/>Эта строка содержит буквенно цифровой полей, разделенных точками. Каждое поле должно начинаться с алфавитного символа ASCII. Нельзя использовать их в качестве значения полей: «CON», «PRN», «AUX», «NUL», «COM1», «COM2», «COM3», «COM4», «COM5», «COM6», «COM7», «COM8», «COM9», «LPT1», «LPT2», «LPT3», «LPT4», «LPT5», «LPT6», «LPT7», «LPT8» и «LPT9».|
|Application::ExecutableName |      Имя исполняемого файла для приложения MSIX, который будет добавлен в манифест пакета. Соответствующая запись приложения будет игнорироваться, если приложение с таким именем не обнаружено.|
|Application::Description |     [необязательно] Описание приложения для вашего приложения MSIX. Если не используется, будет использоваться параметр DisplayName приложения. Это описание будет использоваться для входа приложения обнаружена, соответствующий указанным ExecutableName|
|Application::DisplayName    |  Отображаемое имя приложения для пакета MSIX. Это отображаемое имя будет использоваться для входа приложения обнаружена, соответствующий указанным ExecutableName|
|Возможности |     [необязательно] 0 или дополнительные возможности элементов для добавления пользовательских возможностей MSIX пакета. возможность «runFullTrust» добавляется по умолчанию во время преобразования.|
|Capability::Name | Возможность добавления в пакет MSIX.

