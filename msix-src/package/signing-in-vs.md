---
description: В этой статье описывается, как подписать пакет MSIX в Visual Studio.
title: Подписывание пакета MSIX в Visual Studio
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 7904f6de962804a9776338860485c98d04360f02
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073680"
---
# <a name="sign-an-msix-package-in-visual-studio"></a>Подписывание пакета MSIX в Visual Studio

Проект **упаковки приложений Windows** в Visual Studio включает файл формата файла обмена личной информацией, защищенный паролем по умолчанию (PFX), который, возможно, потребуется заменить собственным. Если ваша организация не предоставляет вам сертификат подписи кода, вы можете приобрести его в доверенном центре сертификации или создать самозаверяющий сертификат. В Visual Studio имеется параметр "создать тестовый сертификат" и мастер импорта, который можно найти, открыв файл Package. appxmanifest в конструкторе манифестов приложений по умолчанию и просмотрев вкладку "Упаковка". Если вы не находитесь в мастерах и диалоговых окнах, вы можете создать сертификат с помощью командлета PowerShell New-SelfSignedCertificate:

```
 > New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=MyCompany,
  O=MyCompany, L=Stockholm, S=N/A, C=Sweden" -KeyUsage DigitalSignature
    -FriendlyName MyCertificate -CertStoreLocation "Cert:\LocalMachine\My"
      -TextExtension @('2.5.29.37={text}1.3.6.1.5.5.7.3.3',
        '2.5.29.19={text}Subject Type:End Entity')
```

Командлет выводит отпечаток (например, A27... D9F здесь), которые можно передать другому командлету, Move-Item, чтобы переместить сертификат в доверенное корневое хранилище сертификации:

```
>Move-Item Cert:\LocalMachine\My\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F
  -Destination Cert:\LocalMachine\Root
```
Опять же, необходимо установить сертификат в это хранилище на всех компьютерах, где планируется установить и запустить упакованное приложение. Также необходимо включить загрузку неопубликованных приложений на этих устройствах. На неуправляемом компьютере это можно сделать с помощью Update & Security | Для разработчиков в приложении "Параметры". На устройстве под управлением Организации можно включить загрузку неопубликованных приложений, отправив политику с помощью поставщика управления мобильными устройствами (MDM).

Отпечаток также можно использовать для экспорта сертификата в новый PFX-файл с помощью командлета Export PfxCertificate:

```
>$pwd = ConvertTo-SecureString -String secret -Force -AsPlainText
>Export-PfxCertificate -cert
  "Cert:\LocalMachine\Root\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F"
    -FilePath "c:/<SolutionFolder>/Msix/certificate.pfx" -Password $pwd
```
Не забывайте, что Visual Studio будет использовать созданный PFX-файл для подписывания пакета MSIX, выбрав его на вкладке Упаковка в конструкторе или вручную отредактировав файл проекта wapproj и заменив значения элементов <PackageCertificateKeyFile> и <PackageCertificateThumbprint>.
