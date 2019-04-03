---
author: c-don
title: Как пакеты MSIX наборов
description: Объединение пакетов MSIX различных архитектур
ms.author: cdon
ms.date: 10/25/2018
ms.topic: article
keywords: Windows 10, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 54e6f063aa05c1a297725e6c1c7a091ebffbb043
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900166"
---
# <a name="bundle-msix-packages"></a>Пакеты MSIX наборов 

В этой статье описывается процесс создания пакета после преобразования x86 и x64 64 из собственных установщиков Windows, с помощью средства упаковки MSIX. 

Путем объединения нескольких версий архитектура установщика в одну сущность, только пакет, необходимо передать Store или другое расположение распространения. Платформа развертывания Windows 10 поддерживает тип пакета .msixbundle и загрузит только файлы, которые можно использовать для архитектуры вашего устройства. Имейте в виду, что если принято решение для распространения .msixbundle для конкретного приложения, вы не может вернуться к распространения пакет MSIX. 

Ниже представлены поэтапный подход для создания .msixbundle. Предполагается, что вы уже [преобразовать существующий x86 и x64 64](https://docs.microsoft.com/windows/msix/mpt-best-practices) MSIX пакетов установщика Windows. 

### <a name="setup"></a>Установка
Вам потребуются следующие настройки для успешного создания комплект MSIX:
- [Пакет SDK для Windows 10](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) (версии 1809 или более поздней версии)
- MSIX x64 и x86 преобразованных пакетов 

## <a name="step-1-find-makeappxexe"></a>Шаг 1. Найти MakeAppx.exe
[MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) — это средство, доступных в Windows 10 SDK, позволяющий для упаковки и объединение MSIX пакетов. Объединить два пакета MSIX, будет использовать это средство. 

MakeAppx.exe можно использовать для извлечения содержимого файла пакета приложения Windows 10 или пакета. Она также шифрует и расшифровывает пакеты приложений и пакетов.

После установки пакета SDK для Windows 10 MakeAppx.exe обычно можно найти здесь: 
- [x86] — C:\Program Files (x86) \Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64] — C:\Program Files (x86) \Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>Шаг 2. Объединение пакетов

Для формирования пакета пакетов с помощью MakeApp.exe проще всего добавить все пакеты, которые вы хотите объединить в одной папке. Каталог должны оставаться свободными от всего остального, за исключением пакетов, было необходимо объединить в пакет. 

Переместите пакеты приложений, которые вы хотите объединить в одном каталоге, как показано на следующем снимке экрана.

![Пакеты наборов в каталоге](images/bundle-pic1.png)

>[!NOTE] 
> MakeAppx.exe объединяет только пакеты, которые имеют одинаковое удостоверение, это означает, что идентификатор приложения, издатель, версия должна быть одинаковыми. Только архитектуру процессора пакета для пакета приложения могут быть разными. 

MakeAppx.exe имеет следующий синтаксис командной строки.

''' Командной строке C:\> «C:\Program файлы (x86) \Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe» объединить /d input_directorypath /p <filepath>.msixbundle
```

Here is an example command.

```
C:\> «C:\Program файлы (x86) \Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe» объединить /d c:\AppPackages\ /p c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

After running the command, an unsigned .msixbundle will be created in the path specified. Packages do not need to be signed before bundling.  

## Step 3: Sign the bundle

After you create the bundle, you must sign the package before you can distribute the app to your users or install it. 

To sign a package, you will need a general code signing certificate and use SignTool.exe from the Windows 10 SDK. 

We strongly recommend that you use a trusted cert from certificate authority as that allows for the package to be distributed and deployed on your end users devices seamlessly. Once you have access to the private certificate (.pfx file), you can sign the package as shown below.

>[!NOTE]
> SignTool.exe is available in the same directory as MakeAppx.exe in the Windows 10 SDK. 

SignTool.exe has the following command line syntax.

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd <Hash Algorithm> /a 
/f <Path to Certificate>.pfx /p <Your Password> <File path>.msixbundle
```

Ниже приведен пример команды.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

Дополнительные сведения о подписи пакетов приложений с помощью SignTool.exe см. в разделе [в этой статье](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool). 

После успешного входа пакета, вы готовы разместить его в общей сетевой папке или в любой сети распространения содержимого, а затем распространить его среди пользователей. 

