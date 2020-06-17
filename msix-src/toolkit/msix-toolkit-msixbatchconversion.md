---
title: MSIX скрипты для полного преобразования
description: В этой статье содержатся сведения о сценариях пакетного преобразования в наборе средств MSIX.
ms.date: 1/23/2020
ms.topic: article
keywords: Windows 10, msix, мсикстулкит, msix Toolkit, набор средств, пакет, преобразование, пакетное преобразование
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b2af803cc540d393703e68569d8da1a78d478f2d
ms.sourcegitcommit: 6243b7aca6f52f007f4571c835f580f433c31769
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/16/2020
ms.locfileid: "84812793"
---
# <a name="msix-bulk-conversion-scripts"></a>MSIX скрипты для полного преобразования

[Сценарии с массовым преобразованием](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion) в наборе средств MSIX можно использовать для автоматизации преобразования приложений Windows в формат пакета MSIX. Список приложений и их сведения приведены в сценарии **entry.ps1** .

## <a name="prepare-machines-for-conversion"></a>Подготовка компьютеров к преобразованию
Перед запуском сценария многомерного преобразования MSIX Toolkit для автоматизации преобразования приложения в формат упаковки MSIX устройства, которые вы будете использовать (виртуальная или удаленная), должны быть настроены для удаленного взаимодействия и установлен инструмент упаковки MSIX.

| Термин            | Описание                                                        |
|-----------------|--------------------------------------------------------------------|
| Хост компьютера    | Это устройство, выполняющее скрипты пакетного преобразования.          |
| Виртуальная машина | Это устройство, существующее в Hyper-V, размещенное на размещающем компьютере.  |
| Удаленный компьютер  | Это физическая или виртуальная машина, доступная по сети. |

### <a name="host-machine"></a>Хост компьютера
Компьютер размещения должен соответствовать следующим требованиям.
* Должен быть установлен [инструмент упаковки MSIX](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) .
* Если используются виртуальные машины, необходимо установить Hyper-V.
* Если используются удаленные компьютеры:
    * Устройство существует в том же домене, что и удаленные компьютеры:
        * Включение удаленного взаимодействия PowerShell 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            ```
    * Устройство существует в Рабочей группе или в альтернативном домене в качестве удаленных компьютеров:
        * Включить удаленное взаимодействие PowerShell 
        * Доверенный узел WinRM должен содержать имя или IP-адрес устройства удаленного компьютера 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            Set-Item WSMan:\localhost\Client\TrustedHosts -Value <RemoteMachineName>,[<RemoteMachineName>,...]
            ```

### <a name="remote-machine"></a>Удаленный компьютер
Удаленный компьютер должен соответствовать следующим требованиям.
* Должен быть установлен [инструмент упаковки MSIX](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) .
* Если устройство существует в том же домене, что и главный компьютер:
    * Включение удаленного взаимодействия PowerShell 
    * Необходимо включить WinRM 
    * Разрешить протокол ICMPv4 через брандмауэр клиента
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        ```

* Если устройство существует в Рабочей группе или в альтернативном домене в качестве главного компьютера:
    * Включение удаленного взаимодействия PowerShell 
    * Доверенный узел WinRM должен содержать имя устройства или IP-адрес главного компьютера
    * Разрешить протокол ICMPv4 через брандмауэр клиента
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        Set-Item WSMan:\localhost\Client\TrustedHosts -Value <HostMachineName>
        
        ```

### <a name="virtual-machine"></a>Виртуальная машина
Рекомендуется использовать образ "быстрое создание" средства создания пакетов MSIX для Hyper-V, так как он предварительно настроен для удовлетворения всех требований. Виртуальная машина должна быть размещена на главном компьютере и запущена в Microsoft Hyper-V.

Виртуальная машина должна соответствовать следующим требованиям.
* Должен быть установлен [инструмент упаковки MSIX](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) .

## <a name="syntax"></a>Синтаксис

```powershell
entry.ps1
```

## <a name="description"></a>Описание

Это набор сценариев PowerShell, предоставляющий возможность пакетной обработки приложений в формате пакета MSIX. Эти сценарии будут подключаться к локальной виртуальной машине или удаленному компьютеру, который будет использоваться для упаковки каждого приложения.

Приложения, Упакованные в формат приложения MSIX, будут преобразованы в том порядке, в котором они были указаны в сценарии **entry.ps1** . Удаленные компьютеры, перечисленные в скрипте **entry.ps1** , будут использоваться для упаковки приложений в формат MSIX, который будет использоваться в единственном числе. Виртуальные машины можно использовать несколько раз для упаковки различных приложений в формат приложения MSIX.

Перед выполнением скрипта необходимо сначала добавить приложения, которые нужно преобразовать в `conversionsParameters` переменную в скрипте. В переменную можно добавить несколько приложений. Сценарий использует приложение и удаленные виртуальные машины, чтобы создать XML-файл в соответствии с требованиями [средства упаковки MSIX](..\packaging-tool\mpt-overview.md) (MsixPackagingTool.exe). После создания XML-файла сценарий **run_job.ps1** выполняется в новом процессе PowerShell, который выполняет MsixPackagingTool.exe на целевом устройстве, чтобы преобразовать приложение и поместить его в папку **.\аут** , расположенную в папке выполнения скрипта.

## <a name="example"></a>Пример

```powershell
PS C:\> entry.ps1
```

В примере этого выполняется скрипт **entry.ps1** . Этот скрипт преобразует приложения, указанные в `conversionsParameters` переменной, в пакеты MSIX. Приложения преобразуются с помощью виртуальных машин или удаленных компьютеров, указанных в переменных *virtualMachines* и *ремотемачинес* .

## <a name="parameters"></a>Параметры

### <a name="virtualmachines"></a>virtualMachines

`virtualMachines`Параметр представляет собой массив, содержащий имя и учетные данные виртуальных машин для подключения и доступа при упаковке приложения в формат MSIX.

* **Тип:** InArray
* **Обязательный:** нет

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

Указанная виртуальная машина будет использоваться для упаковки приложений в формат MSIX. Эта виртуальная машина будет подключена к, используя учетные данные, указанные при появлении запроса (запрос появляется сразу после выполнения сценария **entry.ps1** ). Перед упаковкой приложения в формат упаковки MSIX сценарий создаст моментальный снимок виртуальной машины Hyper-V, а затем восстановился до этого снимка после упаковки приложения.

### <a name="remotemachines"></a>ремотемачинес

`remoteMachines`Параметр представляет собой массив, содержащий имя и учетные данные удаленных компьютеров для подключения и доступа при упаковке приложения в формат MSIX. Указанные удаленные компьютеры будут однозначными устройствами, используемыми для упаковки одного приложения.

Удаленные компьютеры должны быть доступны и обнаружены в сети.

* **Тип:** InArray
* **Обязательный:** нет

```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

Указанная удаленная машина будет использоваться для упаковки одного приложения в формат MSIX. Этот удаленный компьютер будет подключен к, используя учетные данные, указанные при появлении запроса (запрос отображается непосредственно после выполнения сценария **entry.ps1** ).

Убедитесь, что полное доменное имя или внешний псевдоним устройства разрешается до выполнения скрипта **entry.ps1** .

### <a name="signingcertificate"></a>сигнингцертификате
`signingCertificate`Параметр — это массив, содержащий сведения, связанные с сертификатом подписи кода, который будет использоваться для подписи упакованного приложения MSIX. Этот сертификат должен иметь уровень шифрования как минимум SHA256.

* **Тип:** InArray
* **Обязательный:** нет

```powershell
$SigningCertificate = @{
    Password = "Password"; 
    Path = "C:\Temp\ContosoLab.pfx"
}
```
### <a name="conversionsparameters"></a>конверсионспараметерс

`conversionsParameters`Параметр представляет собой массив, содержащий сведения о приложениях, которые необходимо преобразовать в формат MSIX. Каждое приложение в массиве будет анализироваться по отдельности и выполняться с помощью преобразования пакета MSIX на удаленном компьютере или виртуальной машине. Приложения будут преобразованы в том порядке, в котором они отображаются в скрипте. Если преобразование в формат MSIX завершается неудачей, сценарий не будет повторно пытаться преобразовать приложение на другой удаленный компьютер или виртуальную машину.

* **Тип:** InArray
* **Обязательный:** да

```powershell
$conversionsParameters = @(
    ## Use for MSI applications:
    @{
        InstallerPath = "C:\Path\To\YourInstaller.msi";    # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0"                         # MSIX Application version (must contain 4 octets).
    },
    ## Use for EXE or other applications:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement"   # Arguements required by the installer to provide a silent installation of the application.
    },
    ## Creating the Packaged app and Template file in a specific folder path:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement";  # Arguements required by the installer to provide a silent installation of the application.
        SavePackagePath = "Custom\folder\Path";            # Specifies a custom folder path where the MSIX app will be created.
        SaveTemplatePath = "Custom\folder\Path"            # Specifies a custom folder path where the MSIX Template XML will be created.
    }
)
```

Сведения о приложении, предоставленные в `conversionsParameters` переменной, будут использоваться для создания XML-файла со всеми необходимыми сведениями о приложении. После создания XML-файла скрипт передает XML-файл [средству упаковки MSIX](..\packaging-tool\mpt-overview.md) (MsixPackagingTool.exe) для упаковки.

## <a name="logging"></a>Ведение журнала

Скрипт создаст файл журнала, который описывает, что произошло во время выполнения скрипта. В файле журнала содержатся сведения, относящиеся к упаковке приложений в формат пакета MSIX, а также сведения, связанные с ходом выполнения сценариев. Журналы можно считывать из любой текстовой служебной программы, но были настроены для чтения с помощью средства чтения журнала Trace32. Ошибки в выполнении скрипта будут выделены красным цветом, а предупреждения — желтыми. Дополнительные сведения о средстве чтения журнала трассировки 32 см. на странице [CMTrace](https://docs.microsoft.com/mem/configmgr/core/support/cmtrace) on документация Майкрософт.

Файл журнала создается в каталоге скрипта `.\logs\BulkConversion.log` .
