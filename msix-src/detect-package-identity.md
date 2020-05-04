---
title: Определение идентификатора пакета и контекста среды выполнения
description: Эта статья описывает, как определить, поставляется ли приложение в виде MSIX-пакета на Win 1709 или более поздней версии.
author: Huios
ms.date: 01/23/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 0fe0a721368b10f68ba30665883e9070692dbb04
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2020
ms.locfileid: "76726594"
---
# <a name="detect-package-identity-and-runtime-context"></a>Определение идентификатора пакета и контекста среды выполнения

Возможно, у вас есть некоторые версии приложения, которые не были распространены в MSIX-пакете. Во время выполнения приложение можно определить, было ли оно развернуто как пакет MSIX с помощью API диспетчера пакетов Windows или собственного пользовательского установщика. Вам может потребоваться изменить поведение приложения, например параметры обновления, или воспользоваться преимуществами функций, доступных только для пакетов MSIX.

Чтобы определить, запущено ли ваше приложение в виде MSIX-пакета на версии Windows, поддерживающей полный набор возможностей MSIX, можно использовать собственную функцию [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) в kernel32.dll. Если классическое приложение выполняется как неупакованное приложение без удостоверения пакета, эта функция возвращает ошибку, которая может помочь определить контекст, в котором выполняется приложение.

Если функция выполнена, это означает:

* Приложение упаковывается в пакет MSIX.
* Ваше приложение работает в Windows 10, версии 1709 (сборка 16299) или более поздней версии с полной поддержкой MSIX.

## <a name="use-getcurrentpackagefullname-in-native-code"></a>Использование GetCurrentPackageFullName в машинном коде

В следующем примере кода показано, как использовать [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) для определения контекста приложения.

```cpp
#define _UNICODE 1
#define UNICODE 1

#include <Windows.h>
#include <appmodel.h>
#include <malloc.h>
#include <stdio.h>

int __cdecl wmain()
{
    UINT32 length = 0;
    LONG rc = GetCurrentPackageFullName(&length, NULL);
    if (rc != ERROR_INSUFFICIENT_BUFFER)
    {
        if (rc == APPMODEL_ERROR_NO_PACKAGE)
            wprintf(L"Process has no package identity\n");
        else
            wprintf(L"Error %d in GetCurrentPackageFullName\n", rc);
        return 1;
    }

    PWSTR fullName = (PWSTR) malloc(length * sizeof(*fullName));
    if (fullName == NULL)
    {
        wprintf(L"Error allocating memory\n");
        return 2;
    }

    rc = GetCurrentPackageFullName(&length, fullName);
    if (rc != ERROR_SUCCESS)
    {
        wprintf(L"Error %d retrieving PackageFullName\n", rc);
        return 3;
    }
    wprintf(L"%s\n", fullName);

    free(fullName);

    return 0;
}
```

## <a name="use-getcurrentpackagefullname-function-in-managed-code"></a>Использование функции GetCurrentPackageFullName в управляемом коде

Чтобы вызвать [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) в управляемом приложении .NET Framework, необходимо использовать [Вызов неуправляемого кода (P/Invoke)](https://docs.microsoft.com/dotnet/standard/native-interop/pinvoke) или другую форму взаимодействия.

Чтобы упростить этот процесс, можно использовать библиотеку [DesktopBridgeHelpers](https://github.com/qmatteoq/DesktopBridgeHelpers/). Эта библиотека поддерживает .NET Framework 4 и более поздних версий и использует P/Invoke внутри для предоставления вспомогательного класса, который определяет, выполняется ли приложение в версии Windows, поддерживающей полный набор функций MSIX. Эта библиотека также доступна в качестве [пакета NuGet](https://www.nuget.org/packages/DesktopBridge.Helpers/).

После установки пакета в проекте можно создать новый экземпляр класса `DesktopBridge.Helpers` и вызвать метод `IsRunningAsUwp`. Этот метод возвращает значение true, если приложение выполняется как пакет MSIX в Windows 10, версии 1709 (сборка 16299) или более поздней, и false, если это не так. В следующем примере показан вызов этого метода.

```csharp
private bool IsRunningAsUwp()
{
   UwpHelpers helpers = new UwpHelpers();
   return helpers.IsRunningAsUwp();
}

private void Form1_Load(object sender, EventArgs e)
{
   if (IsRunningAsUwp())
   {
       txtUwp.Text = "I'm running as MSIX";
   }
   else
   {
       txtUwp.Text = "I'm running as a native desktop app";
   }
}
```
