---
title: Настройка удаленного преобразования в средстве упаковки MSIX
description: В этой статье описывается настройка и подключение к удаленному компьютеру для выполнения преобразований приложений с помощью средства упаковки MSIX.
ms.date: 02/26/2019
ms.topic: article
keywords: MSIX, MPT, MSIX Packaging Tool, remote IP
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: a13b2a2725cc8c3750cbd22d4238ae41f5406fdc
ms.sourcegitcommit: 4d912f89e385268757e87bf8fd9ca1828b99e109
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2020
ms.locfileid: "77544765"
---
# <a name="setup-instructions-for-remote-machine-conversions"></a>Инструкции по настройке преобразования на удаленном компьютере

Подключение к удаленному компьютеру — один из вариантов, чтобы убедиться в соблюдении рекомендаций по [использованию](prepare-your-environment.md) среды преобразования, так как это может быть среда очистки, отличная от локального компьютера. Прежде чем начать удаленное преобразование, необходимо выполнить несколько шагов.  

На удаленном компьютере нужно включить удаленное взаимодействие PowerShell, чтобы обеспечить безопасный доступ. Кроме того, необходимо иметь учетную запись администратора для удаленного компьютера.  Чтобы подключиться через IP-адрес, выполните инструкции по подключению к удаленным компьютерам, не присоединенным к домену.

## <a name="connecting-to-a-remote-machine-in-a-trusted-domain"></a>Подключение к удаленному компьютеру в доверенном домене

Чтобы включить удаленное взаимодействие PowerShell, выполните следующую команду на удаленном компьютере из окна PowerShell с правами **администратора**: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck
```

Подключаться к присоединенному к домену компьютеру нужно через доменную, а не локальную учетную запись. В противном случае следуйте инструкциям по настройке для компьютера, не присоединенного к домену.

### <a name="port-configuration"></a>Конфигурация порта

Если удаленный компьютер входит в группу безопасности (например, Azure), необходимо настроить правила безопасности для доступа к серверу средства упаковки MSIX.  

#### <a name="azure"></a>Azure

1. На портале Azure выберите **Сети** > **Добавить входящий порт**.
2. Щелкните **Основной**.
3. В поле "Служба" должно остаться значение **Custom** (Пользовательская).
4. Задайте номер порта **1599** (стандартный номер порта для средства упаковки MSIX; его можно изменить в настройках средства) и укажите имя правила (например, AllowMPTServerInBound).

#### <a name="other-infrastructure"></a>Другая инфраструктура

Конфигурация порта сервера должна соответствовать значению порта для средства упаковки MSIX (стандартный номер порта для средства упаковки MSIX — 1599; его можно изменить в настройках средства).

## <a name="connecting-to-a-non-domain-joined-remote-machineincludes-ip-addresses"></a>Подключение к удаленному компьютеру, не присоединенному к домену (с использованием IP-адресов)

Для компьютеров, не присоединенных к домену, необходим сертификат для подключения через протокол HTTPS.

1. Включите удаленное взаимодействие PowerShell и соответствующие правила брандмауэра, выполнив следующую команду на удаленном компьютере в окне PowerShell с **правами администратора**:

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck  

New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled  True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
```
 
2. Создайте самозаверяющий сертификат, задайте конфигурацию WinRM HTTPS и экспортируйте сертификат.

``` PowerShell
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My -KeyExportPolicy NonExportable).Thumbprint

$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername"";CertificateThumbprint=""$thumbprint""}"

cmd.exe /C $command

Export-Certificate -Cert Cert:\LocalMachine\My\$thumbprint -FilePath <path_to_cer_file>
```

3. На локальном компьютере скопируйте экспортированный сертификат и установите его в хранилище доверенных корневых сертификатов.

``` PowerShell
Import-Certificate -FilePath <path> -CertStoreLocation Cert:\LocalMachine\Root
```

### <a name="port-configuration"></a>Конфигурация порта 

Если удаленный компьютер входит в группу безопасности (например, Azure), необходимо настроить правила безопасности для доступа к серверу средства упаковки MSIX.  

#### <a name="azure"></a>Azure

Выполните инструкции по [добавлению пользовательского порта](#azure) для средства упаковки MSIX и добавлению сетевого правила безопасности для WinRM HTTPS.

1. На портале Azure выберите **Сети** > **Добавить входящий порт**.
2. Щелкните **Основной**.
3. Задайте для поля "Служба" значение **WinRM**.

#### <a name="other-infrastructure"></a>Другая инфраструктура 

Конфигурация порта сервера должна соответствовать значению порта для средства упаковки MSIX (стандартный номер порта для средства упаковки MSIX — 1599; его можно изменить в настройках средства).
