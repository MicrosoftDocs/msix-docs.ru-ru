---
title: Развертывание пакета MSIX с помощью MSIX Core
description: Описывается, как развернуть пакет MSIX с MSIX Core с помощью средства мсиксмгр. exe.
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 98ca9fce99139608da8e59108d68756717c605ae
ms.sourcegitcommit: 0412ba69187ce791c16313d0109a5d896141d44c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303355"
---
# <a name="deploy-an-msix-package-with-msix-core"></a>Развертывание пакета MSIX с помощью MSIX Core

[MSIX Core](msixcore.md) предоставляет развертывание MSIX для выбора предыдущих версий Windows. Чтобы начать работу, сначала убедитесь, что на целевом устройстве установлен MSIX Core.

## <a name="msi-installation"></a>Установка MSI

Мы рекомендуем использовать предоставленные установщики MSI для установки MSIX Core, так как они автоматически добавляют мсиксмгр. exe в путь поиска и связывают расширение MSIX с установщиком.

Вы можете скачать следующие установщики MSI, зависящие от архитектуры, из раздела " **активы** " на нашей [странице выпуска](https://github.com/microsoft/msix-packaging/releases):

* **msixmgrSetup-x64. msi**
* **msixmgrSetup-86. msi**

> [!NOTE]
> убедитесь, что выбран правильный установщик для архитектуры устройства. Это повлияет на то, где в установщике будут храниться важные файлы. Имя файла может измениться в зависимости от версии установщика.

## <a name="installing-your-certificate"></a>Установка сертификата

Пакеты MSIX должны быть подписаны. Перед установкой каких бы то ни было пакетов MSIX убедитесь, что установлен сертификат, который использовался для подписи пакетов. Это можно сделать с помощью обычных рабочих процессов для установки сертификата из средства управления.

Если вы хотите вручную установить сертификат, эту команду можно выполнить из командной строки с повышенными привилегиями:

```PowerShell
certutil -addstore root <insert certificate.cert>
```

> [!NOTE]
> следует добавить доверенный сертификат в доверенный корневой центр сертификации во всех сценариях.

## <a name="using-the-command-line"></a>Использование командной строки

После установки средства мсиксмгр. exe его можно использовать для управления пакетами MSIX на этом компьютере путем поиска, установки и удаления. Служебная программа командной строки мсиксмгр. exe предназначена для системных администраторов. Он наиболее удобен при запуске из командной строки администратора. Не все команды при запуске из обычной командной строки выводятся на консоль. Ниже приведены дополнительные сведения.

### <a name="install"></a>"Установить"

С помощью командной строки или PowerShell перейдите в каталог, содержащий мсиксмгр. exe, и выполните следующую команду, чтобы установить пакет MSIX. Параметр `-quietUX` также можно добавить в конце команды, чтобы пользователи не могли видеть пользовательский интерфейс установщика.

```PowerShell
msixmgr.exe -AddPackage C:\NotePadPlus\notepadplus.msix -quietUX
```

> [!NOTE]
> этом и в следующих примерах используется **нотепадплус. msix**. Это один из наших [примеров пакетов](https://github.com/microsoft/msix-packaging/tree/master/MsixCore/Tests).

### <a name="querying-for-a-specific-msix-package"></a>Запрос определенного пакета MSIX

Поиск конкретного пакета возможен с помощью **полноеимяпакета**, **паккажефамилинаме** и (или) также с использованием подстановочных знаков. Поддерживаются подстановочные знаки * (совпадение с любым символом) и? (совпадение с одиночным символом).

```PowerShell
msixmgr.exe -FindPackage notepadplus_0.0.0.1_???__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *epadplus_8wekyb3d8bbw?
```

### <a name="uninstall"></a>Удалить

Чтобы удалить, используйте следующую команду:

```PowerShell
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe -quietUX
```