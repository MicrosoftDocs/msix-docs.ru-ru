---
title: Изменение скрипта издателя пакета
description: В этой статье содержатся сведения о скрипте изменения издателя пакета в MSIX Тулит.
ms.date: 1/23/2020
ms.topic: article
keywords: Windows 10, msix, msix Toolkit
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b54d6d0a6d1f1d971a7ece99104fdac0db3c49d3
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073580"
---
# <a name="modify-package-publisher-script"></a>Изменение скрипта издателя пакета

[Сценарий изменения издателя пакета](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/ModifyPackagePublisher) в наборе средств MSIX можно использовать для обновления издателя в манифесте перед повторной подписью пакета на основе нового сертификата. В настоящее время этот скрипт ограничен MSIX приложениями, а не MSIX пакетами.

## <a name="syntax"></a>Синтаксис

```powershell
.\modify-package-publisher.ps1 -directory <String> -redist <String> -certPath <String> [[-pfxPath] <String>] [[-Password] <String>] [[-forceContinue]<Switch>]
```

## <a name="examples"></a>Примеры

### <a name="update-the-publisher-based-on-the-certificate"></a>Обновление издателя на основе сертификата

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer"
```

Эта команда рекурсивно ищет содержимое К:\МСИКС для всех пакетов MSIX и обновляет издатель приложения MSIX в соответствии с издателем сертификата, расположенного по адресу К:\церт\мицерт.цер.

### <a name="update-the-publisher-and-sign-the-msix-app"></a>Обновление издателя и подписание приложения MSIX

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx"
```

Эта команда рекурсивно ищет содержимое К:\МСИКС для всех пакетов MSIX и обновляет издатель приложения MSIX в соответствии с издателем сертификата, расположенного по адресу К:\церт\мицерт.цер. Затем команда повторно подписывает идентифицированные пакеты MSIX с помощью сертификата, расположенного по адресу К:\церт\церткэй.пфкс.

### <a name="update-the-publisher-and-sign-the-msix-app-with-a-password-protected-pfx-certificate"></a>Обновление издателя и подписание приложения MSIX с помощью сертификата PFX, защищенного паролем

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -password "aaabbbccc"
```

Эта команда рекурсивно ищет содержимое К:\МСИКС для всех пакетов MSIX и обновляет издатель приложения MSIX в соответствии с издателем сертификата, расположенного по адресу К:\церт\мицерт.цер. Затем команда повторно подписывает идентифицированные пакеты MSIX с помощью сертификата, расположенного по адресу К:\церт\церткэй.пфкс, используя *ааабббккк* Password для разблокировки сертификата, защищенного паролем.

### <a name="update-the-publisher-sign-the-msix-app-and-force-continue-to-next-msix-app"></a>Обновление издателя, подписание приложения MSIX и принудительное выполнение следующего приложения MSIX

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -forceContinue -pfxPath "C:\cert\CertKey.pfx"
```

Эта команда рекурсивно ищет содержимое К:\МСИКС для всех пакетов MSIX и обновляет издатель приложения MSIX в соответствии с издателем сертификата, расположенного по адресу К:\церт\мицерт.цер. Затем команда повторно подписывает идентифицированные пакеты MSIX с помощью сертификата, расположенного по адресу К:\церт\церткэй.пфкс. Если при обработке пакета MSIX возникли ошибки, скрипт продолжит обновление издателя и повторно подписывать идентифицированные пакеты MSIX.

## <a name="parameters"></a>Параметры

### <a name="-directory"></a>-Каталог

Предоставляет корневой каталог, содержащий приложения MSIX. В этом каталоге рекурсивно выполняется поиск всех пакетов MSIX.

* **Тип:** Строка
* **Требуется:** Да
* **Расположение:** С именем
* **Значение по умолчанию:** None

### <a name="-certpath"></a>-Цертпас

Предоставляет полный путь к файлу сертификата (CER), используемому для обнаружения новых или обновленных сведений об издателе приложения.

* **Тип:** Строка
* **Требуется:** Да
* **Расположение:** С именем
* **Значение по умолчанию:** None

### <a name="-redist"></a>-Redist

Путь к распространяемому файлу, полученному из [набора средств MSIX](https://aka.ms/msixtoolkit). Этот файл используется для повторного упаковки приложения в формат пакета MSIX. Должен указывать на распространяемый пакет архитектуры 32-bit или 64-bit.

* **Тип:** Строка
* **Требуется:** Да
* **Расположение:** С именем
* **Значение по умолчанию:** None

### <a name="-pfxpath"></a>-Пфкспас

Путь к сертификату подписи кода (PFX), который будет использоваться для подписания пакета MSIX после обновления издателя приложения.

* **Тип:** Строка
* **Требуется:** Нет
* **Расположение:** С именем
* **Значение по умолчанию:** None

### <a name="-password"></a>-password

Пароль, необходимый для сертификата подписи кода (PFX).

* **Тип:** Строка
* **Требуется:** Нет
* **Расположение:** С именем
* **Значение по умолчанию:** None

### <a name="-forcecontinue"></a>-forceContinue

Если этот параметр указан, скрипт будет пропускать ошибки и пытаться обновить сведения об издателе для всех приложений.

* **Тип:** Строка
* **Требуется:** Нет
* **Расположение:** С именем
* **Значение по умолчанию:** None
