---
title: Обновление приложений, опубликованных в магазине, из кода
description: Описывает, как разработчики могут обновлять пакеты MSIX в коде.
author: Huios
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 1d387789c072782199b85d52c79b57c1c94e8312
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726634"
---
# <a name="update-store-published-apps-from-your-code"></a>Обновление приложений, опубликованных в магазине, из кода

Начиная с Windows 10 версии 1607 (сборка 14393), Windows 10 позволяет разработчикам обеспечить более надежную гарантию обновлений приложений из магазина. Для этого требуется несколько простых интерфейсов API, создает единообразное и предсказуемое взаимодействие с пользователем и позволяет разработчикам сосредоточиться на том, что они делают лучше, позволяя Windows выполнять тяжелую работу.

Существует два фундаментальных способа управления обновлениями приложений. В обоих случаях общий результат для этих методов одинаков — обновление применяется. Однако в одном случае можно разрешить системе выполнять всю работу, в то время как в другом случае может потребоваться более глубокий контроль над работой пользователя.

## <a name="simple-updates"></a>Простые обновления

Первым и самым простым вызовом API является то, что система проверяет наличие обновлений, загружает их, а затем запрашивает у пользователя разрешение на их установку. Начните с использования класса [стореконтекст](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) для получения объектов [сторепаккажеупдате](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) , скачайте и установите их.

```csharp
using Windows.Services.Store;

private async void GetEasyUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation = 
            updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);
        StorePackageUpdateResult result = await downloadOperation.AsTask();
    }
}
```

На этом этапе пользователь может выбрать один из двух вариантов: применить обновление сейчас или отложить обновление. Любой вариант, который пользователь делает, возвращается через объект `StorePackageUpdateResult`, что позволяет разработчикам выполнять дальнейшие действия, такие как закрытие приложения, если обновление необходимо для продолжения, или просто повторить попытку позже.

## <a name="fine-controlled-updates"></a>Детально контролируемые обновления

Для разработчиков, которым требуется полностью настроенный интерфейс, предоставляются дополнительные API, которые обеспечивают больший контроль над процессом обновления. Платформа позволяет выполнять следующие действия:

* Получение событий хода выполнения по отдельному пакету загрузки или по всему обновлению.
* Применяйте обновления на удобство пользователя и приложения, а не на одном или другом.

Разработчики могут скачивать обновления в фоновом режиме (в то время как приложение используется), а затем запрашивать установку обновлений, если они отклоняются, но вы можете просто отключить возможности, затронутые обновлением, если вы решили.

### <a name="download-updates"></a>Скачать обновления

```csharp
private async void DownloadUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
            updateManager.RequestDownloadStorePackageUpdatesAsync(updates);

        downloadOperation.Progress = async (asyncInfo, progress) =>
        {
            // Show progress UI
        };

        StorePackageUpdateResult result = await downloadOperation.AsTask();
        if (result.OverallState == StorePackageUpdateState.Completed)
        {
            // Update was downloaded, add logic to request install
        }
    }
}
```

### <a name="install-updates"></a>Установка обновлений

```csharp
private async void InstallUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();    

    // Save app state here

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    StorePackageUpdateResult result = await installOperation.AsTask();

    // Under normal circumstances, app will terminate here

    // Handle error cases here using StorePackageUpdateResult from above
}
```

## <a name="making-updates-mandatory"></a>Обеспечение обязательных обновлений

В некоторых случаях может быть желательно иметь обновление, которое должно быть установлено на устройстве пользователя, делая его действительно обязательным (например, критическое исправление для приложения, которое не может ждать). В таких случаях есть дополнительные меры, которые можно выполнить, чтобы сделать обновление обязательным.

1. Реализуйте в коде приложения обязательную логику обновления (необходимо выполнить до самого обязательного обновления).
2. Во время отправки в центр разработки убедитесь, что выбран флажок **сделать это обновление обязательным** .

### <a name="implementing-app-code"></a>Реализация кода приложения

Чтобы воспользоваться всеми преимуществами обязательных обновлений, необходимо внести некоторые небольшие изменения в приведенный выше код. Чтобы определить, является ли обновление обязательным, необходимо использовать [объект сторепаккажеупдате](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) .

```csharp
 private async bool CheckForMandatoryUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        foreach (StorePackageUpdate u in updates)
        {
            if (u.Mandatory)
                return true;
        }
    }
    return false;
}
```

Затем необходимо создать настраиваемое диалоговое окно приложения, чтобы уведомить пользователя о наличии обязательного обновления и установить его, чтобы продолжить полное использование приложения. Если пользователь отклоняет обновление, приложение может либо снизить функциональность (например, запретить доступ к сети), либо полностью завершить работу (например, только в онлайн-игры).

### <a name="partner-center"></a>Центр партнеров

Чтобы убедиться, что в [сторепаккажеупдате](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) указано значение true для обязательного обновления, необходимо пометить обновление как обязательное в центре партнеров на странице **пакеты** .

Обратите внимание на несколько моментов.

* Если устройство возвращается в режим «в сети» после замены обязательного обновления другим необязательным обновлением, необязательное обновление по-прежнему будет отображаться на устройстве как обязательное до его обязательного обновления.
* Обновления, контролируемые разработчиком, и обязательные обновления в настоящее время ограничены магазином.
