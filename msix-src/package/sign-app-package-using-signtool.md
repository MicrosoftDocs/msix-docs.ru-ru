---
title: Подписание пакета приложения с помощью SignTool
description: Используйте SignTool для ручного подписывания пакета приложения сертификатом.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: 1ece6feed1b012bd2f51e4211b8719fbb48e6d63
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689983"
---
# <a name="sign-an-app-package-using-signtool"></a>Подписание пакета приложения с помощью SignTool

**SignTool** — это средство командной строки, используемое для цифровой подписи пакета приложения или пакета приложений с помощью сертификата. Сертификат может быть создан пользователем (для тестирования) или выдан компанией (для распространения). Подписывание пакета приложения дает пользователю средство проверки отсутствия изменений в данных приложения после его подписи, при этом также подтверждается подлинность пользователя или компании, подписавшего пакет. **SignTool** может подписать зашифрованные и незашифрованные пакеты приложения и пакеты приложений.

> [!IMPORTANT] 
> Если для разработки приложения использовали Visual Studio, рекомендуется применять мастер Visual Studio для создания и подписывания пакета приложения. Дополнительные сведения см. в статьях [Упаковка приложения UWP с помощью Visual Studio](packaging-uwp-apps.md) и [упаковка классического приложения из исходного кода с помощью Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

Дополнительные сведения о подписи кода и сертификатах в целом см. в разделе [Знакомство с процессом подписания кода](https://docs.microsoft.com/windows/desktop/SecCrypto/cryptography-tools).

## <a name="prerequisites"></a>предварительные требования

- **Упакованное приложение**  
    Подробнее о ручном создании пакета приложения, [создания пакета приложения с помощью средства MakeAppx.exe](create-app-package-with-makeappx-tool.md).

- **Допустимый сертификат для подписи**  
    Дополнительные сведения о создании или импорте действительного сертификата подписи см. в разделе [Создание или импорт сертификата для подписания пакета](create-certificate-package-signing.md).

- **SignTool. exe**  
    В зависимости от пути установки пакета SDK **SignTool** может находиться в следующих расположениях на компьютере с Windows 10:
    - x86: C:\Program Files (x86) \Windows Kits\10\bin\x86\SignTool.exe
    - х C:\Program Files (x86) \Windows Kits\10\bin\x64\SignTool.exe

## <a name="using-signtool"></a>Применение SignTool

**SignTool** может использоваться для подписывания файлов, проверки подписей и меток времени, удаления подписей и другого. Так как нас интересует подписывание пакета приложения, мы рассмотрим команду **sign**. Подробные сведения об инструменте **SignTool** см. на справочной странице [SignTool](https://docs.microsoft.com/windows/desktop/SecCrypto/signtool).

### <a name="determine-the-hash-algorithm"></a>Определение хэш-алгоритма

При использовании **SignTool** для подписи пакета приложения или пакета приложений, хэш-алгоритм, применяемый в **SignTool**, должен совпадать с алгоритмом, использованном для упаковки приложения. Например, если вы применили **MakeAppx.exe** для создания пакета приложения с параметрами по умолчанию, то необходимо выбрать SHA256 в **SignTool**, поскольку это алгоритм, по умолчанию используемый в **MakeAppx.exe**.

Чтобы узнать, какой алгоритм хэширования применялся при упаковке приложения, извлеките содержимое пакета и изучите файл AppxBlockMap.xml. Инструкции по распаковке/извлечению пакета приложения см. в разделе [Извлечение файлов из пакета приложения или пакета приложений](create-app-package-with-makeappx-tool.md). Хэш-метод указан в элементе BlockMap и имеет следующий формат:

```xml
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap"
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

В этой таблице показан каждое значение HashMethod и соответствующий хэш-алгоритм:


| Значение HashMethod                              | Хэш-алгоритм |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> Так как в **SignTool** по умолчанию применяется алгоритм SHA1 (которого нет в **MakeAppx.exe**), вам всегда необходимо указывать хэш-алгоритм при использовании **SignTool**.

### <a name="sign-the-app-package"></a>Подпись пакета приложения

Если у вас есть все необходимые компоненты и вы определили, какой хэш-алгоритм применялся для упаковки приложения, то вы готовы подписать его. 

Общий синтаксис командной строки при подписи пакета с помощью **SignTool** таков:

```syntax
SignTool sign [options] <filename(s)>
```

Сертификат, используемый для подписания приложения, должен быть либо PFX-файлом либо установлен в хранилище сертификатов.

Чтобы подписать пакет приложения с помощью сертификата из PFX-файла, примените следующий синтаксис:

```syntax
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```

Обратите внимание, что параметр `/a` позволяет **SignTool** автоматически выбрать наиболее подходящий сертификат.

Если ваш сертификат не является PFX-файлом, используйте следующий синтаксис:

```syntax
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

Кроме того, вы можете указать хэш SHA1 нужного сертификата вместо &lt;Имя сертификата&gt;, с помощью следующего синтаксиса:

```syntax
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

Обратите внимание, что с некоторыми сертификатами пароль не используется. Если для вашего сертификата не требуется пароль, опустите параметр "/p &lt;Ваш пароль&gt;" в примерах команд.

Подписав пакет приложения с помощью действительного сертификата, вы сможете отправить пакет в Store. Дополнительные рекомендации по загрузке и отправке приложений в Store см. в разделе [Отправка приложений](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

## <a name="common-errors-and-troubleshooting"></a>Распространенные ошибки и их устранение
Наиболее распространенные типы ошибок при использовании **SignTool** являются внутренними и обычно выглядят примерно так:

```syntax
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

Если код ошибки начинается с 0x8008, например 0x80080206 (APPX_E_CORRUPT_CONTENT), подписываемый пакет не является допустимым. При появлении такого типа ошибки, необходимо перестроить пакет и запустить **SignTool** еще раз.

В инструменте **SignTool** есть параметр отладки для показа ошибок с сертификатом и их фильтрации. Чтобы использовать функцию отладки, поместите параметр `/debug` сразу после `sign`, а затем полную команду **SignTool**.

```syntax
SignTool sign /debug [options]
``` 

Наиболее распространенная ошибка — 0x8007000B. О подобном типе ошибки можно найти дополнительные сведения в журнале событий.
 
Для получения дополнительных сведений в журнале событий:
- Запустите Eventvwr.msc
- Откройте журнал событий: Просмотр событий (локальный) — журналы приложений и служб > — > Microsoft-> Windows-> AppxPackagingOM-> Microsoft-Windows-Аппкспаккагинг/эксплуатация
- Поиск последнего события об ошибке

Внутренняя ошибка 0x8007000B обычно соответствует одному из следующих значений:

| **ИД события** | **Пример строки события** | **Предложен** |
|--------------|--------------------------|----------------|
| 150          | Ошибка 0x8007000B: Имя издателя манифеста приложения (CN = Contoso) должно соответствовать имени субъекта сертификата для подписи (CN = Contoso, C = US). | Имя издателя манифеста приложения должно точно соответствовать имени субъекта подписи.               |
| 151          | Ошибка 0x8007000B: Указанный метод хэширования подписи (SHA512) должен соответствовать методу хэширования, используемому в сопоставлении блоков пакетов приложений (SHA256).     | В параметре /fd указан неправильный HashAlgorithm. Повторно запустите **SignTool**, указав hashAlgorithm, совпадающий со схемой блоков пакета приложения (который использовался при создании пакета приложения)  |
| 152          | Ошибка 0x8007000B: Содержимое пакета приложения должно проверяться на соответствие с его картой блоков.                                                           | Пакет приложения поврежден и его необходимо перестроить для формирования новой схемы блока. Подробнее о ручном создании пакета приложения, см. [Создания пакета приложения с помощью средства MakeAppx.exe](create-app-package-with-makeappx-tool.md). |