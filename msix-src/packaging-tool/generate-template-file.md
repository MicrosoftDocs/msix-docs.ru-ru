---
title: Создание файла шаблона для преобразований из командной строки
description: В этой статье описывается создание файла шаблона для преобразований из командной строки.
ms.date: 01/28/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 00c7997673f6c0ed3fc79f425c90e283b7a1f25c
ms.sourcegitcommit: 4d912f89e385268757e87bf8fd9ca1828b99e109
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2020
ms.locfileid: "77544615"
---
# <a name="how-to-generate-a-template-file-for-command-line-conversions"></a>Создание файла шаблона для преобразований из командной строки

С помощью средства упаковки MSIX можно выполнить преобразование двумя способами: через интерактивный пользовательский интерфейс или с помощью нашего параметра командной строки. При использовании командной строки необходимо предоставить файл шаблона, чтобы преобразование работало с конкретными параметрами и потребностями. Эта статья поможет вам в процессе создания файла шаблона, который подходит для вас.

Существует два способа получить файл шаблона, который подходит для вас:

* Вы можете использовать пользовательский интерфейс средства упаковки MSIX. В параметрах этого средства можно указать, что вы хотите создать файл шаблона преобразования с каждым создаваемым пакетом MSIX.
* Можно взять [Пример шаблона](#sample-conversion-template-file) и вручную ввести конфигурации, необходимые для каждого преобразования.

## <a name="generate-a-conversion-template-file-from-the-msix-packaging-tool"></a>Создание файла шаблона преобразования из средства упаковки MSIX

1. Запустите средство упаковки MSIX.
2. Перейдите к параметрам в правом верхнем углу приложения.
3. Убедитесь, что выбран параметр "создать файл командной строки с каждым пакетом".
4. Внесите любые другие изменения или изменения в нужные параметры (например, элементы исключения, коды выхода).
5. Сохранение параметров
6. Пройдите по рабочему процессу пакета приложения с помощью установщика.
    - Если вы не выберете установщик, вы не сможете создать файл шаблона преобразования.
    - При использовании EXE-файла необходимо передать в установщик флаг Silent, чтобы создать файл шаблона преобразования.
7. По завершении преобразования вы получите файл шаблона, настроенный на основе выбранного установщика, и текущие параметры, которые теперь можно повторно использовать для будущих преобразований.
    - По умолчанию файл шаблона преобразования будет сохранен в том же расположении, что и пакет MSIX, но можно указать отдельное расположение для сохранения файла шаблона на странице Создание пакета.
    - Вам по-прежнему потребуется внести некоторые изменения в зависимости от того, какие MSIX вы хотите выводить в конце каждого преобразования.

## <a name="manually-edit-the-conversion-template-file"></a>Изменение файла шаблона преобразования вручную

Можно вручную изменить параметры шаблона для файла шаблона преобразования, чтобы создать файл шаблона, который подходит для вас. При создании файла с шаблоном преобразования обратите внимание на то, какие функции добавляются в файл шаблона, так как для работы некоторых может потребоваться дополнительная ссылка на схему.

### <a name="conversion-template-parameter-reference"></a>Справочник по параметрам шаблона преобразования

Ниже приведен полный список параметров, которые можно использовать в файле шаблона преобразования.

|**Параметры преобразования** |   **Описание** |
|---------|---------|
|Параметры:: Алловтелеметри | [Необязательно] Включает ведение журнала телеметрии для этого вызова средства.|
|Параметры:: Аппляллпрепарекомпутерфиксес | [Необязательно] Применяет все рекомендуемые исправления в ходе подготовки компьютера. Не может быть задан, если используются другие атрибуты.|
|Параметры:: Женератекоммандлинефиле | [Необязательно] Копирует входные данные из файла шаблона в каталог SaveLocation для последующего использования.|
|Параметры:: Алловпромптфорпассворд | [Необязательно] Указывает средству выводить пользователю запрос на ввод паролей для виртуальной машины и на подписывание сертификата (если это обязательно, но не задано).|
|Параметры:: Енфорцемикрософтстореверсионингрекуирементс | [Необязательно] Указывает средству применять схему управления версиями, которая обязательна при развертывании из Microsoft Store и Microsoft Store для бизнеса.|
|Параметры:: Серверпортнумбер| используемых Используется при подключении к удаленному компьютеру. Требуется v2 схемы шаблона.|
|Параметры:: Аддпаккажеинтегрити| используемых Добавляет целостность пакетов во все созданные MSIX. Требуется версия 5 схемы шаблона. |
|валидинсталлерекситкодес | [необязательно] 0 или более элементов Валидинсталлерекситкоде. Требуется v2 схемы шаблона. |
|Валидинсталлерекситкодес:: Валидинсталлерекситкоде | используемых Укажите коды выхода установщика, которые не могут быть знакомы с этим средством или для которых требуется перезагрузка. Требуется v2 схемы шаблона. |
|Валидинсталлерекситкодес:: Валидинсталлерекситкоде:: reboot | используемых Укажите, должен ли код выхода активировать перезагрузку во время преобразования. Требуется v3 схемы шаблона. |
|ExclusionItems | [Необязательно] 0 или более элементов FileExclusion или RegistryExclusion. Все элементы FileExclusion должны быть указаны до элементов RegistryExclusion.|
|ExclusionItems::FileExclusion | [Необязательно] Файл, который будет исключен при создании пакета.|
|ExclusionItems::FileExclusion::ExcludePath | Путь к файлу, который будет исключен при создании пакета.|
|ExclusionItems::RegistryExclusion | [Необязательно] Раздел реестра, который будет исключен при создании пакета.|
|Ексклусионитемс:: Регистрексклусион:: Ексклудепас | Путь к реестру, который будет исключен при создании пакета.|
|PrepareComputer::DisableDefragService | [Необязательно] Отключает программу дефрагментации Windows на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|Препарекомпутер:: Дисаблевиндовссеарчсервице | [Необязательно] Отключает Windows Search на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|Препарекомпутер:: Дисаблесмшостсервице | [Необязательно] Отключает службу узла агента SMS на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|Препарекомпутер:: Дисаблевиндовсупдатесервице | [Необязательно] Отключает клиентский компонент Центра обновления Windows на время преобразования приложения. Если задано значение false, переопределяет параметр ApplyAllPrepareComputerFixes.|
|SaveLocation | [Необязательно] Элемент для указания расположения, в котором средство сохранит пакет. Если значение не указано, пакет будет сохранен в папке на рабочем столе. |
|SaveLocation::PackagePath | [Необязательно] Путь к файлу или папке с сохраненным пакетом MSIX. |
|SaveLocation::TemplatePath | используемых Путь к файлу или папке, в котором сохранен шаблон результирующей командной строки. |
|Installer::Path | Путь к установщику приложения.|
|Installer::Arguments | [Необязательно] Аргументы, передаваемые установщику.  Средство автоматически и без участия пользователя запустит установщики MSI с аргументом "/qn /norestart INSTALLSTARTMENUSHORTCUTS=1 DISABLEADVTSHORTCUTS=1". Примечание. необходимо передать аргументы, чтобы принудительно запустить установщик при использовании установщиков. exe.|
|Installer::InstallLocation | используемых Полный путь к корневой папке приложения для установленных файлов, если они были установлены (например, "C:\Program Files (x86) \Мяппинсталллокатион").|
|VirtualMachine | [Необязательно] Элемент, который указывает, что преобразование будет выполнено на локальной виртуальной машине.|
|VirtualMachine:: Name | Имя виртуальной машины, используемой для среды преобразования.|
|VirtualMachine::Username | Имя пользователя виртуальной машины, используемой для среды преобразования.|
|RemoteMachine | используемых Элемент, указывающий, что преобразование будет выполняться на удаленном компьютере. Требуется v2 схемы шаблона. |
|RemoteMachine:: ComputerName | Имя удаленного компьютера, используемого для среды преобразования. Требуется v2 схемы шаблона. |
|RemoteMachine:: username | Имя пользователя для удаленного компьютера, используемого для среды преобразования. Требуется v2 схемы шаблона. |
|RemoteMachine:: Енаблеаутологон | используемых При выполнении преобразования, требующего перезапуска на удаленном компьютере, будет автоматически выполнен вход, чтобы преобразование выполнялось без проблем. Требуется v3 схемы шаблона. |
|PackageInformation::PackageName | Имя пакета MSIX.|
|PackageInformation::PackageDisplayName | Отображаемое имя пакета MSIX.|
|PackageInformation::PublisherName | Издатель пакета MSIX.|
|PackageInformation::PublisherDisplayName | Отображаемое имя издателя пакета MSIX.|
|PackageInformation::Version | Номер версии пакета MSIX.|
|Паккажеинформатион::P Аккажедескриптион | используемых Описание пакета MSIX. Требуется v4 схемы шаблона. |
|Паккажеинформатион:: Маинпаккаженамеформодификатионпаккаже | [Необязательно] Имя удостоверения пакета для имени основного пакета. Используется при создании пакета изменений, который принимает зависимость основного (родительского) приложения.|
|сигнингинформатион| используемых Элемент, указывающий сведения о подписывании для подписи Device Guard. Требуется v4 схемы шаблона. |
|Сигнингинформатион:: Девицегуардсигнинг | используемых Элемент для указания сведений о подписывании Device Guard. Требуется v4 схемы шаблона. |
|Девицегуардсигнинг:: Токенфиле | [Маркер доступа Azure AD, необходимый](../package/signing-package-device-guard-signing.md#get-an-azure-ad-access-token) для подписи Device Guard в формате JSON. Требуется схема шаблона v4. |
|Девицегуардсигнинг:: Тиместампурл | используемых Предоставляет отметку времени во время подписания с помощью Device Guard, чтобы гарантировать, что приложение будет устанавливаться за время существования сертификата. Требуется v4 схемы шаблона. |
|Приложения | [Необязательно] 0 или более элементов приложения для настройки записей приложения в пакете MSIX.|
|Application::Id | Идентификатор приложения MSIX. Этот идентификатор будет использоваться для обнаруженной записи приложения, которая соответствует указанному параметру ExecutableName. Вы можете указать несколько значений идентификаторов приложений для исполняемых файлов в пакете.<br/><br/>Это значение является уникальным идентификатором приложения в пакете. Оно также иногда называется связанным с пакетом идентификатором приложения (PRAID). Идентификатор должен быть уникальным в пределах пакета (в одном пакете нельзя более одного раза использовать один и тот же идентификатор). Но он не должен быть глобально уникальным. Другой пакет в системе может использовать такой же идентификатор.<br/><br/>Эта строка содержит буквенно-цифровые поля, разделенные точками. Каждое поле должно начинаться с буквенного символа ASCII. Их нельзя использовать в качестве значений полей: CON, PRN, AUX, NUL "," COM1 "," COM2 "," COM3 "," COM4 "," COM5 "," COM6 "," COM7 "," COM8 "," COM9 "," LPT1 "," LPT2 "," LPT3 "," LPT4 "," LPT5 "," LPT6 "," LPT7 "," LPT8 "и" LPT9 ".|
|Application::DisplayName | Отображаемое имя приложения дял пакета MSIX. Это отображаемое имя будет использоваться для обнаруженной записи приложения, которая соответствует указанному параметру ExecutableName.|
|Application::ExecutableName | Имя исполняемого файла для приложения MSIX, которое будет добавлено в манифест пакета. Соответствующая запись приложения будет игнорироваться, если приложение с таким именем не обнаружено.|
|Application::Description | [Необязательно] Описание приложения MSIX. Если этот параметр не указан, будет использоваться отображаемое имя приложения. Это описание будет использоваться для обнаруженной записи приложения, которая соответствует указанному параметру ExecutableName.|
|Возможности | [Необязательно] 0 или более элементов Capability для добавления пользовательских функций в пакет MSIX. Элемент Capability "runFullTrust" добавляется по умолчанию в процессе преобразования.|
|Capability::Name | Элемент Capability, который нужно добавить в пакет MSIX. |

### <a name="sample-conversion-template-file"></a>Пример файла преобразования шаблона

```xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018"
    xmlns:V2="http://schemas.microsoft.com/msix/msixpackagingtool/template/1904"
    xmlns:V3="http://schemas.microsoft.com/msix/msixpackagingtool/template/1907"
    xmlns:V4="http://schemas.microsoft.com/msix/msixpackagingtool/template/1910"
    xmlns:V5="http://schemas.microsoft.com/msix/msixpackagingtool/template/2001">
<!--Note: You only need to include xmlns:v2 - xmlns:v5 if you are using one of the features that use those schemas -->

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
        EnforceMicrosoftStoreVersioningRequirements="false"
        v2:ServerPortNumber="1599"
        v5:AddPackageIntegrity="true">    

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
    
    <!--Note: Specifying an installer exit code will allow you to automatically trigger a reboot during your conversion
      <v2:ValidInstallerExitCodes>
        <V2:ValidInstallerExitCode ExitCode="3010" V3:Reboot="true"/>
        <V2:ValidInstallerExitCode ExitCode="1641"/>
      </v2:ValidInstallerExitCodes>
    -->
        
    </Settings>

    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute and is optional
    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService="true"/>
    -->

    <SaveLocation
        PackagePath="C:\users\user\Desktop\MyPackage.msix" 
        TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />
    
    <!--NOTE: This section specifies that the conversion will be run on a local Virtual Machine. This is optional if you want to change your conversion environment from the default local machine.
    <VirtualMachine Name="vmname" Username="vmusername"/>
    -->

    <!--NOTE: This section specifies that the conversion will be run on a remote machine.This is optional if you want to change your conversion environment from the default local machine.
    <v2:RemoteMachine ComputerName="vmname" Username="vmusername" v3:EnableAutoLogon="true"/>
    -->

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">

    <!--Note: This is optional, if you want to sign your package with Device Guard signing
        <v4:SigningInformation>
            <v4:DeviceGuardSigning
                Tokenfile="tokenfile.json"
                TimestampUrl="https://mytimestamp.com"/>
        </v4:SigningInformation>
    -->
        
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
