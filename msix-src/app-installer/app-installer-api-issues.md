---
author: joshusto
title: Проблемы с установщиком приложения API файлов
description: Перечисляет известные проблемы с файлом установщика приложений API-интерфейсы.
ms.author: joshusto
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, универсальной платформы Windows, установщик приложения, AppInstaller, неопубликованные API
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: be13529ecbb1845447719c7951b77ca66287ffc0
ms.sourcegitcommit: 9bbb116d1984082123f694130b4d6cc078fa8510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983346"
---
# <a name="app-installer-file-api-issues"></a>Проблемы с установщиком приложения API файлов

## <a name="javascript-support-for-app-installer-file-apis"></a>Поддержка JavaScript для API-интерфейсов файлового установщика приложений

[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) и [пакета](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) классы в пакете SDK для Windows содержат методы, которые можно использовать для добавления или изменения пакетов с помощью файлов установщика приложений или для получения сведений о приложениях с установщиком приложения связь. Дополнительные сведения см. в разделе [связанной документации](app-installer-documentation.md).

Из этих методов [PackageManager.AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync), [PackageManager.RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync), и [ Package.CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync) не поддерживаются в JavaScript. Тем не менее, можно создать [компонента среды выполнения Windows](https://docs.microsoft.com/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript) , вызывает эти методы, а затем вызвать этот компонент из приложения JavaScript UWP.

Ниже приведен пример.

```csharp
namespace CSRuntimeComponent
{
    public sealed class UpdateAvailabilityChecker
    {
        public static IAsyncOperation<PackageUpdateAvailability> CheckForUpdatesAsync()
        {
            return AsyncInfo.Run<PackageUpdateAvailability>((result) => Task.Run<PackageUpdateAvailability>(async () =>
            {
                PackageManager pm = new PackageManager();
                Package currentPackage = pm.FindPackageForUser(string.Empty, Package.Current.Id.FullName);
                PackageUpdateAvailabilityResult apiResult = await currentPackage.CheckUpdateAvailabilityAsync();

                if (apiResult.Availability == PackageUpdateAvailability.Error)
                {
                    Logger.Error($"Error occurred, extended code: {apiResult.ExtendedError}");
                }

                return apiResult.Availability;
            }));
        }
    }
}
```

```javascript
window.onload = function () {
    document.getElementById('mainButton').onclick = function (evt) {
        CSRuntimeComponent.UpdateAvailabilityChecker.checkForUpdatesAsync().done(function (result) {
            document.getElementById("resultLabel").innerHTML = "Update availability result:" + result;
        });
    }
}
```
