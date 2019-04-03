---
title: Параметры удаленного преобразования в инструменте упаковки MSIX
description: Инструкции по установке для удаленной преобразования
author: c-don
ms.author: cdon
ms.date: 02/26/2019
ms.topic: article
keywords: Инструмент упаковки MSIX MSIX, MPT, удаленный IP-адрес
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 19a6c4741293835e1494e2796e2ef84f63b63079
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900366"
---
# <a name="setup-instructions-for-remote-machine-conversions"></a>Инструкции по настройке для выполнения преобразований, удаленный компьютер 

> [!NOTE]
> Удаленный компьютер преобразования — это новая функция, которая в данный момент доступна только в предварительной версии сборки программы предварительной оценки. 
>
> Вы можете присоединиться к [MSIX упаковки средство Insider Preview Program](insider-program.md) для получения доступа к этой функции. 

В этом предварительном выпуске из [средство упаковки MSIX](insider-program.md#current-insider-preview-build), мы добавили возможность подключиться к удаленному компьютеру под управлением преобразования. Существуют несколько шагов, которые необходимо выполнить, прежде чем Приступая к работе с преобразованиями удаленного.  

Необходимо включить удаленное взаимодействие PowerShell на удаленном компьютере для безопасного доступа. Необходимо также настроить учетную запись администратора для удаленного компьютера.  Если вы хотите подключиться, используя IP-адрес, выполните инструкции по подключению к не присоединен к домену удаленного компьютера. 

## <a name="connecting-to-a-remote-machine-in-a-trusted-domain"></a>Подключение к удаленному компьютеру в доверенном домене 

Чтобы включить удаленное взаимодействие PowerShell, выполните на удаленном компьютере из **администратора** окне PowerShell: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck 
```

Убедитесь, что вход на компьютер, присоединенных к домену, с помощью учетной записи домена, а не локальной учетной записи, или вы должны следовать инструкциям по установке для не-присоединения к домену. 

### <a name="port-configuration"></a>Конфигурация порта 

Если удаленный компьютер входит в группу безопасности (например, Azure), необходимо настроить правила безопасности сети к серверу MSIX средство упаковки.  

#### <a name="azure"></a>Azure 

1. На портале Azure перейдите к **сети** > **добавить входящий порт** 
2. Нажмите кнопку **базовый**
3. Служба поле должно остаться задайте **Custom**
4. Номер порта **1599** (значение порта по умолчанию средство упаковки MSIX – это можно изменить в параметрах программы) и введите имя (например AllowMPTServerInBound) правила 

#### <a name="other-infrastructure"></a>Другой инфраструктуры 

Убедитесь, что конфигурацию порта сервера выравнивается по средство упаковки MSIX значение порта (порт по умолчанию средство упаковки MSIX 1599 — это можно изменить в параметрах программы) 

## <a name="connecting-to-a-non-domain-joined-remote-machineincludes-ip-addresses"></a>Подключение к домену присоединен удаленный компьютер (включая IP-адреса) 

Для компьютер присоединен к домену необходимо настроить с помощью сертификата для подключения по протоколу HTTPS. 

1. Включите удаленное взаимодействие PowerShell и соответствующие правила брандмауэра, выполнив следующее на удаленном компьютере в **администратора** окне PowerShell: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck  

New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled  True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP 
```
 
2. Создать самозаверяющий сертификат, установите WinRM HTTPS, конфигурации и Экспорт сертификата 

``` PowerShell
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My -KeyExportPolicy NonExportable).Thumbprint 

$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername"";CertificateThumbprint=""$thumbprint""}" 

cmd.exe /C $command 

Export-Certificate -Cert Cert:\LocalMachine\My\$thumbprint -FilePath <path_to_cer_file> 
```

3. На локальном компьютере скопируйте экспортированного сертификата и установите его в доверенное корневое хранилище 

``` PowerShell
Import-Certificate -FilePath <path> -CertStoreLocation Cert:\LocalMachine\Root 
``` 

### <a name="port-configuration"></a>Конфигурация порта 

Если удаленный компьютер входит в группу безопасности (например, Azure), необходимо настроить правила безопасности сети к серверу MSIX средство упаковки.  

#### <a name="azure"></a>Azure 

Следуйте инструкциям, чтобы [добавить пользовательский порт](#azure) для средства упаковки MSIX, а также Добавление правила безопасности сети для WinRM HTTPS 

1. На портале Azure перейдите к **сети** > **добавить входящий порт** 
2. Нажмите кнопку **базовый** 
3. Значение поля службы **WinRM**

#### <a name="other-infrastructure"></a>Другой инфраструктуры 

Убедитесь, что конфигурацию порта сервера выравнивается по средство упаковки MSIX значение порта (порт по умолчанию средство упаковки MSIX 1599 — это можно изменить в параметрах программы) 
