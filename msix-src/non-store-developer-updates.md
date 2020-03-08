---
title: Обновление приложений, опубликованных не в Store, с помощью кода
description: Описывает, как пакеты MSIX, поставляющиеся за пределы хранилища, могут обновляться разработчиками в коде.
author: Huios
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: aefddfdaa50c2ffabb05c2e75a44de792db49c7b
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073730"
---
# <a name="update-non-store-published-apps-from-your-code"></a>Обновление приложений, опубликованных не в Store, с помощью кода

При отправке приложения в качестве MSIX можно программно запустить обновление приложения. Если приложение развертывается за пределами магазина, необходимо проверить сервер на наличие новой версии приложения и установить новую версию. Можно обновить до новой версии, используя метод [PackageManager. аддпаккажеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackageasync) . Это требует, чтобы приложение объявило `packageManagement` возможность.

В этой статье приведены примеры, демонстрирующие объявление возможности `packageManagement` в манифесте пакета и использование метода [PackageManager. аддпаккажеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackageasync) для проверки и установки обновления.

## <a name="add-the-packagemanagement-capability-to-your-package-manifest"></a>Добавление функции PackageManagement в манифест пакета

Чтобы использовать `PackageManager` API, приложение должно объявить [ограниченную возможность](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) `packageManagement` в [манифесте пакета](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest).

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

## <a name="check-for-updates-on-your-server"></a>Проверка наличия обновлений на сервере

Первым шагом является проверка доступности новой версии приложения. В следующем примере проверяется, чтобы версия пакета на сервере была больше текущей версии приложения (в этом примере для демонстрационных целей используется тестовый сервер).

```csharp
using Windows.Management.Deployment;

//check for an update on my server
private async void CheckUpdate(object sender, TappedRoutedEventArgs e)
{
    WebClient client = new WebClient();
    Stream stream = client.OpenRead("https://trial3.azurewebsites.net/HRApp/Version.txt");
    StreamReader reader = new StreamReader(stream);
    var newVersion = new Version(await reader.ReadToEndAsync());
    Package package = Package.Current;
    PackageVersion packageVersion = package.Id.Version;
    var currentVersion = new Version(string.Format("{0}.{1}.{2}.{3}", packageVersion.Major, packageVersion.Minor, packageVersion.Build, packageVersion.Revision));

    //compare package versions
    if (newVersion.CompareTo(currentVersion) > 0)
    {
        var messageDialog = new MessageDialog("Found an update.");
        messageDialog.Commands.Add(new UICommand(
            "Update",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.Commands.Add(new UICommand(
            "Close",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.DefaultCommandIndex = 0;
        messageDialog.CancelCommandIndex = 1;
        await messageDialog.ShowAsync();
    } else
    {
        var messageDialog = new MessageDialog("Did not find an update.");
        await messageDialog.ShowAsync();
    }
}
```

## <a name="download-an-update"></a>Скачать обновление

После того как вы определили, что обновление доступно, вы можете поставить его в очередь для скачивания и установки. Обновление будет применено при следующем завершении работы приложения. После перезапуска приложения новая версия будет доступна пользователю.

```csharp

// Queue up the update and close the current app instance.
private async void CommandInvokedHandler(IUICommand command)
{
    if (command.Label == "Update")
    {
        PackageManager packagemanager = new PackageManager();
        await packagemanager.AddPackageAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.msix"),
            null,
            DeploymentOptions.ForceApplicationShutdown
        );
    }
}
```