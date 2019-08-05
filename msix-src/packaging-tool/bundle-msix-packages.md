---
title: Объединение пакетов MSIX
description: Объединение пакетов MSIX с разными архитектурами
ms.date: 10/25/2018
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 7742ca5e52ed5ee435ee3c8765fa45cfbdc903dc
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68685435"
---
# <a name="bundle-msix-packages"></a>Объединение пакетов MSIX 

В этой статье описан процесс создания набора пакетов после преобразования версий x86 и x64 установщиков Windows с помощью средства упаковки MSIX. 

Объединив несколько версий установщика с разными архитектурами в один набор, вы можете выложить в Microsoft Store или на другую платформу распространения только этот набор. Платформа развертывания Windows 10 учитывает тип пакета MSIXBUNDLE и скачивает только те файлы, которые соответствуют архитектуре вашего устройства. Помните, что если вы решите распространять для определенного приложения набор MSIXBUNDLE, вы не сможете извлечь из него обратно только пакет MSIX для распространения. 

В следующем разделе приведены пошаговые инструкции по созданию набора пакетов MSIXBUNDLE. Предполагается, что вы уже [преобразовали существующие версии x86 и x64](https://docs.microsoft.com/windows/msix/mpt-best-practices) установщика Windows в пакеты MSIX. 

### <a name="setup"></a>Установка
Для успешного создания набора пакетов MSIX необходимо следующее:
- [пакет SDK для Windows 10](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) (версия 1809 или более новая);
- преобразованные пакеты MSIX x64 и x86. 

## <a name="step-1-find-makeappxexe"></a>Шаг 1. Открытие MakeAppx.exe
[MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) — это средство из пакета SDK для Windows 10, которое позволяет создавать и объединять пакеты MSIX. С его помощью вы объедините два пакета MSIX. 

Средство MakeAppx.exe можно использовать для извлечения файлов из пакета приложения или набора пакетов Windows 10. Оно также позволяет шифровать и расшифровывать пакеты приложений и наборы пакетов.

После установки пакета SDK для Windows 10 средство MakeAppx.exe обычно размещено здесь: 
- [x86] — C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64] — C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>Шаг 2. Объединение пакетов

Самый простой способ объединения пакетов с помощью MakeApp.exe — добавить все нужные пакеты в одну папку. В каталоге должны быть только пакеты, которые нужно объединить, и больше никаких лишних файлов. 

Переместите нужные пакеты приложений в один каталог, как показано на снимке экрана ниже.

![Пакеты для объединения в каталоге](images/bundle-pic1.png)

>[!NOTE] 
> MakeAppx.exe объединяет только пакеты с одинаковыми идентификаторами, то есть у них должен быть один идентификатор приложения, издатель и версия. Отличаться может только архитектура процессора пакета. 

В MakeAppx.exe используется следующий синтаксис командной строки.

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d input_directorypath 
/p <filepath>.msixbundle
```

Ниже приведен пример команды.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d c:\AppPackages\ 
/p c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

После выполнения команды в указанном расположении будет создан неподписанный пакет .msixbundle. Перед объединением пакетов их не нужно подписывать.  

## <a name="step-3-sign-the-bundle"></a>Шаг 3. Подписание пакета

После создания пакета его необходимо подписать, чтобы распространить приложение для пользователей или установить его. 

Чтобы подписать пакет, вам потребуется общий сертификат подписи кода и файл SignTool.exe из пакета SDK для Windows 10. 

Настоятельно рекомендуем использовать доверенный сертификат из центра сертификации, так как он позволяет беспрепятственно распространить и развернуть пакет на устройствах пользователей. Получив доступ к частному сертификату (PFX-файл), вы сможете подписать пакет, как показано ниже.

>[!NOTE]
> Файл SignTool.exe находится в том же каталоге в пакете SDK для Windows 10, что и MakeAppx.exe. 

В SignTool.exe используется следующий синтаксис командной строки.

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd <Hash Algorithm> /a 
/f <Path to Certificate>.pfx /p <Your Password> <File path>.msixbundle
```

Ниже приведен пример команды.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

Дополнительные сведения о подписании пакетов приложений с помощью SignTool.exe см. в [этой статье](../package/sign-app-package-using-signtool.md). 

Подписав набор пакетов, вы можете разместить его в сетевой общей папке или в любой сети распространения содержимого для доступа пользователей. 

