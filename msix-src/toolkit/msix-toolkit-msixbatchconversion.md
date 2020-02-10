---
title: Скрипты преобразования пакетной службы MSIX
description: В этой статье содержатся сведения о скриптах преобразования пакетов в набор средств MSIX Toolkit.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0b0329df39d55fff04b6bdf33a5ec3865a823b99
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073570"
---
# <a name="msix-batch-conversion-scripts"></a>Скрипты преобразования пакетной службы MSIX

[Скрипты преобразования пакетов](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion) в наборе средств MSIX можно использовать для автоматизации преобразования приложений Windows в формат пакета MSIX. Список приложений и их сведения приведены в скрипте **entry. ps1** .

## <a name="syntax"></a>Синтаксис

```powershell
entry.ps1
```

## <a name="description"></a>Описание

Это набор сценариев PowerShell, предоставляющий возможность пакетной обработки приложений в формате пакета MSIX. Эти сценарии будут подключаться к локальной виртуальной машине или удаленному компьютеру, который будет использоваться для упаковки каждого приложения.

Приложения, Упакованные в формат приложения MSIX, будут преобразованы в том порядке, в котором они были введены в скрипте **entry. ps1** . Удаленные компьютеры, перечисленные в сценарии **entry. ps1** , будут использоваться для упаковки приложений в формат MSIX, который будет использоваться в единственном числе. Виртуальные машины можно использовать несколько раз для упаковки различных приложений в формат приложения MSIX.

Перед выполнением скрипта необходимо сначала добавить приложения, которые необходимо преобразовать в переменную `conversionsParameters` в скрипте. В переменную можно добавить несколько приложений. Сценарий использует приложение и удаленные виртуальные машины, чтобы создать XML-файл в соответствии с требованиями [средства упаковки MSIX](..\packaging-tool\mpt-overview.md) (мсикспаккагингтул. exe). После создания XML-файла скрипт **run_job. ps1** выполняется в новом процессе PowerShell, который выполняет мсикспаккагингтул. exe на целевом устройстве для преобразования приложения и помещает его в папку **.\аут** , расположенную в папке выполнения скрипта.

## <a name="example"></a>Пример

```powershell
PS C:\> entry.ps1
```

В примере этого выполняется скрипт **entry. ps1** . Этот скрипт преобразует приложения, указанные в переменной `conversionsParameters`, в пакеты MSIX. Приложения преобразуются с помощью виртуальных машин или удаленных компьютеров, указанных в переменных *virtualMachines* и *ремотемачинес* .

## <a name="parameters"></a>Параметры

### <a name="virtualmachines"></a>virtualMachines

Параметр `virtualMachines` — это массив, содержащий имя и учетные данные виртуальных машин для подключения и доступа при упаковке приложения в формат MSIX.

* **Тип:** InArray
* **Требуется:** Нет

```

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

Указанная виртуальная машина будет использоваться для упаковки приложений в формат MSIX. Эта виртуальная машина будет подключена к с использованием учетных данных, введенных при выводе запроса (запрос отображается непосредственно после выполнения сценария **entry. ps1** ).

### <a name="remotemachines"></a>ремотемачинес

Параметр `remoteMachines` — это массив, содержащий имя и учетные данные удаленных компьютеров для подключения и доступа при упаковке приложения в формат MSIX. Указанные удаленные компьютеры будут однозначными устройствами, используемыми для упаковки одного приложения.

Удаленные компьютеры должны быть доступны и обнаружены в сети.

* **Тип:** InArray
* **Требуется:** Нет

```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

Указанная удаленная машина будет использоваться для упаковки одного приложения в формат MSIX. Этот удаленный компьютер будет подключен к с использованием учетных данных, введенных при выводе запроса (запрос отображается непосредственно после выполнения сценария **entry. ps1** ).

Убедитесь, что полное доменное имя или внешний псевдоним устройства разрешается до выполнения скрипта **entry. ps1** .

### <a name="conversionsparameters"></a>конверсионспараметерс

Параметр `conversionsParameters` — это массив, содержащий сведения о приложениях, которые необходимо преобразовать в формат MSIX. Каждое приложение в массиве будет анализироваться по отдельности и выполняться с помощью преобразования пакета MSIX на удаленном компьютере или виртуальной машине. Приложения будут преобразованы в том порядке, в котором они отображаются в скрипте. Если преобразование в формат MSIX завершается неудачей, сценарий не будет повторно пытаться преобразовать приложение на другой удаленный компьютер или виртуальную машину.

* **Тип:** InArray
* **Требуется:** Да

```powershell
$conversionsParameters = @(
    @{
        InstallerPath = "Path\To\Your\Installer\YourInstaller.msi"; # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                                    # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                            # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                           # Certificate Publisher information
        PublisherDisplayName = "YourCompany";                       # Application Publisher name
        PackageVersion = "1.0.0.0"                                  # MSIX Application version (must contain 4 octets).
    }
)
```

Сведения о приложении, предоставленные в переменной `conversionsParameters`, будут использоваться для создания XML-файла со всеми необходимыми сведениями о приложении. После создания XML-файла сценарий передает XML-файл [средству упаковки MSIX](..\packaging-tool\mpt-overview.md) (мсикспаккагингтул. exe) для упаковки.
