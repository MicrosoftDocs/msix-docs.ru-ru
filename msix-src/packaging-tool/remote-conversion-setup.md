---
title: Настройка удаленного преобразования в средстве упаковки MSIX
description: Инструкции по настройке удаленного преобразования
ms.date: 02/26/2019
ms.topic: article
keywords: MSIX, MPT, MSIX Packaging Tool, remote IP
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 444b4a98d183f2473543c0ca2472f0fddf50052e
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829140"
---
# <a name="setup-instructions-for-remote-machine-conversions"></a>Инструкции по настройке преобразования на удаленном компьютере 

Теперь вы можете подключаться к удаленному компьютеру, чтобы выполнять преобразование. Прежде чем начать удаленное преобразование, необходимо выполнить несколько шагов.  

На удаленном компьютере нужно включить удаленное взаимодействие PowerShell, чтобы обеспечить безопасный доступ. Также на удаленном компьютере у вас должна быть учетная запись администратора.  Чтобы подключиться через IP-адрес, выполните инструкции по подключению к удаленным компьютерам, не присоединенным к домену. 

## <a name="connecting-to-a-remote-machine-in-a-trusted-domain"></a>Подключение к удаленному компьютеру в доверенном домене 

Чтобы включить удаленное взаимодействие PowerShell, на удаленном компьютере из окна **admin** в PowerShell выполните следующую команду: 

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

1. Включите удаленное взаимодействие PowerShell и соответствующие правила брандмауэра, выполнив следующую команду в окне **admin** в PowerShell на удаленном компьютере: 

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
