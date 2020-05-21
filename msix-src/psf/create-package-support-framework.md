---
Description: Устранение проблем, препятствующих запуску настольного приложения в контейнере MSIX
title: Устранение проблем, препятствующих запуску настольного приложения в контейнере MSIX
ms.date: 05/13/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0af2ab1de7c43ed6060048d68e148afefbc1e10f
ms.sourcegitcommit: 8d6bc53d5f5ae80d9ce191fe81660407e9f11e0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/15/2020
ms.locfileid: "83430436"
---
# <a name="create-a-package-support-framework-fixup"></a>Создание привязки для платформы поддержки пакетов 

Если для проблемы не существует исправления среды выполнения, можно создать новое исправление среды выполнения, записав функции замены и включив все данные конфигурации, имеющие смысл. Рассмотрим каждую часть.

### <a name="replacement-functions"></a>Функции замены

Сначала выясните, какие вызовы функций завершаются ошибкой при выполнении приложения в контейнере MSIX. Затем вам нужно создать альтернативные функции, которые должен вызвать диспетчер среды выполнения. Это позволит вам заменить реализацию функции, чтобы ее поведение соответствовало правилам работы с современными средами выполнения.

Объявите ``FIXUP_DEFINE_EXPORTS`` макрос, а затем добавьте оператор include для элемента в `fixup_framework.h` верхней части каждого из них. CPP файл, в который вы собираетесь добавить функции исправления среды выполнения.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Убедитесь, что `FIXUP_DEFINE_EXPORTS` макрос появился перед оператором include.

Создайте функцию, которая имеет ту же сигнатуру функции, что и поведение, которую необходимо изменить. Ниже приведен пример функции, которая заменяет `MessageBoxW` функцию.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

Вызов метода `DECLARE_FIXUP` сопоставляет `MessageBoxW` функцию с новой замещающей функцией. Когда приложение пытается вызвать `MessageBoxW` функцию, вместо этого будет вызываться функция замены.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Защита от рекурсивных вызовов функций в исправлениях среды выполнения

`reentrancy_guard`Тип можно добавить в функции, чтобы защитить их от рекурсивных вызовов функций.

Например, вы можете создать функцию замены для `CreateFile` функции. Ваша реализация может вызвать `CopyFile` функцию, но реализация `CopyFile` функции может вызвать `CreateFile` функцию. Это может привести к бесконечному рекурсивному циклу вызовов `CreateFile` функции.

Дополнительные сведения см. в `reentrancy_guard` разделе [Authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Данные конфигурации

Если вы хотите добавить данные конфигурации в исправление среды выполнения, рассмотрите возможность его добавления в ``config.json`` . Таким образом, можно использовать `FixupQueryCurrentDllConfig` для простого анализа этих данных. В этом примере анализируется логическое и строковое значения из этого файла конфигурации.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

### <a name="fixup-metadata"></a>Адресная привязка метаданных

Каждая адресная привязка и приложение запуска ПСФ имеют XML-файл метаданных, содержащий следующие сведения:

* Версия. версия ПСФ является основной. Дополнительный. Формат исправления в соответствии с [SEM версии 2](https://semver.org/).
* Минимальная платформа Windows: Минимальная версия Windows, необходимая для программы запуска исправления или ПСФ.
* Описание: краткое описание исправления.
* Вхентаусе: эвристика, когда необходимо применить исправление.

Пример см. в файле метаданных [филередиректионфиксупметадата. XML](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/fixups/FileRedirectionFixup/FileRedirectionFixupMetadata.xml) для исправления перенаправления. Схема метаданных доступна [здесь](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/MetadataSchema.xsd).