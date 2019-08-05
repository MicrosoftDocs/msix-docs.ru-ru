---
title: Создание сертификата для подписания пакета
description: Создавайте и экспортируйте сертификат для пакета приложения, подписывая его с использованием инструментов PowerShell.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: d4ee3cae57fc5e344edd8a7daf48a24b49baeb80
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689723"
---
# <a name="create-a-certificate-for-package-signing"></a>Создание сертификата для подписания пакета

В этой статье объясняется, как создать и экспортировать сертификат для подписывания пакета приложений с помощью инструментов PowerShell. Мы рекомендуем использовать Visual Studio для [упаковки приложений UWP](packaging-uwp-apps.md) и [упаковки настольных приложений](../desktop/desktop-to-uwp-packaging-dot-net.md), но вы по-прежнему можете упаковать приложение вручную, если вы не используете Visual Studio для разработки приложения.

## <a name="prerequisites"></a>предварительные требования

- **Упакованное или неупакованное приложение**  
Приложение, содержащее файл AppxManifest.xml. При создании сертификата, который будет использоваться для подписывания окончательного пакета приложений, потребуется сослаться на этот файл манифеста. Сведения о том, как вручную упаковать приложение, см. в разделе [Создание пакета приложений с помощью инструмента MakeAppx.exe](create-app-package-with-makeappx-tool.md).

- **Командлеты инфраструктуры открытых ключей (PKI)**  
Вам потребуются командлеты PKI для создания и экспорта сертификата для подписи. Дополнительные сведения см. в разделе [Командлеты инфраструктуры открытых ключей](https://docs.microsoft.com/powershell/module/pkiclient/).

## <a name="create-a-self-signed-certificate"></a>Создание самозаверяющего сертификата

Самозаверяющий сертификат полезен для тестирования приложения, прежде чем вы будете готовы опубликовать его в магазине. Выполните действия, описанные в этом разделе, чтобы создать самозаверяющий сертификат.

> [!NOTE]
> Самозаверяющие сертификаты строго подписаны для тестирования. Когда вы будете готовы опубликовать свое приложение либо в магазине, либо с другого места проведения, переключите сертификат на надежные источники. Невозможность сделать это может привести к невозможности установки приложения клиентами.

### <a name="determine-the-subject-of-your-packaged-app"></a>Определение субъекта вашего упакованного приложения  

Чтобы использовать сертификат для подписывания пакета приложений, значение в поле сертификата "Субъект" **должно** совпадать с данными в разделе "Издатель" манифеста вашего приложения.

Например, раздел "Идентификатор" файла AppxManifest.xml вашего приложения должен выглядеть примерно следующим образом:

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

"Издатель" в данном случае — "CN=Contoso Software, O=Contoso Corporation, C=US", и именно его следует использовать для создания сертификата.

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Использование командлета **New-SelfSignedCertificate** для создания сертификата

Используйте командлет PowerShell **New-SelfSignedCertificate** для создания самозаверяющего сертификата. Командлет **New-SelfSignedCertificate** имеет несколько параметров для настройки, однако в этой статье мы сконцентрируемся на создании простого сертификата, который будет работать вместе с **SignTool**. Дополнительные примеры и варианты использования этого командлета см. в статье [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate).

Основываясь на файле AppxManifest.xml из предыдущего примера, используйте следующий синтаксис для создания сертификата. В строке с повышенными привилегиями PowerShell:

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

Обратите внимание на следующие сведения о некоторых параметрах.

- **Кэйусаже**: Этот параметр определяет, для чего может использоваться сертификат. Для самозаверяющего сертификата этот параметр должен иметь значение **дигиталсигнатуре**.

- **Текстекстенсион**: Этот параметр включает параметры для следующих расширений:

  - Расширенное использование ключа (EKU): Это расширение указывает дополнительные цели, для которых может использоваться сертифицированный открытый ключ. Для самозаверяющего сертификата этот параметр должен включать строку расширения **"2.5.29.37 = {text} 1.3.6.1.5.5.7.3.3"** , которая указывает, что сертификат должен использоваться для подписывания кода.

  - Основные ограничения: Это расширение указывает, является ли сертификат центром сертификации (ЦС). Для самозаверяющего сертификата этот параметр должен включать строку расширения **"2.5.29.19 = {Text}"** , которая указывает на то, что сертификат является конечной сущностью (а не центром сертификации).

После выполнения этой команды сертификат будет добавлен в локальное хранилище сертификатов, как указано в параметре "-CertStoreLocation". Результат выполнения команды также будет выдавать отпечаток сертификата.  

Сертификат можно просмотреть в окне PowerShell, выполнив следующие команды:

```powershell
Set-Location Cert:\CurrentUser\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

Это позволит отобразить все сертификаты в вашем локальном хранилище.

## <a name="export-a-certificate"></a>Экспорт сертификата 

Чтобы экспортировать сертификат из локального хранилища в файл обмена личной информацией (PFX), используйте командлет **Export-PfxCertificate**.

При использовании командлета **Export-PfxCertificate** следует либо создать и использовать пароль, либо с помощью параметра "-ProtectTo" указать, какие пользователи или группы могут осуществлять доступ к файлу без пароля. Обратите внимание, что если вы не используете параметр "-Password" или "-ProtectTo", отобразится ошибка.

### <a name="password-usage"></a>Использование пароля

```powershell
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

### <a name="protectto-usage"></a>Использование ProtectTo

```powershell
Export-PfxCertificate -cert Cert:\CurrentUser\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

После создания и экспорт сертификата вы готовы подписать пакет приложений с помощью **SignTool**. Следующий шаг в процессе упаковки вручную описан в разделе [Подпись пакета приложения с использованием инструмента SignTool](sign-app-package-using-signtool.md).

## <a name="security-considerations"></a>Замечания по безопасности

Добавив сертификат в [хранилища сертификатов локальной машины](https://docs.microsoft.com/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores), вы меняете доверие сертификатов всех пользователей на компьютере. Рекомендуется удалить эти сертификаты, когда они больше не требуется, чтобы их невозможно было использовать для нарушения доверия системы.