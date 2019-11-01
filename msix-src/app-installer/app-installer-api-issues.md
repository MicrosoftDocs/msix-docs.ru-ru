---
title: Проблемы API файла установщика приложений
description: В этой статье перечислены известные проблемы с интерфейсами API PackageManager и Package для управления файлами установщика приложений.
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, UWP, установщик приложений, AppInstaller, загружать неопубликованные, API
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ca759a80d443daeb58dd66913be2b2acd9e17eef
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328265"
---
# <a name="app-installer-file-api-issues"></a>Проблемы API файла установщика приложений

## <a name="javascript-support-for-app-installer-file-apis"></a>Поддержка JavaScript для файловых API установщика приложений

Классы [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) и [Package](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) в Windows SDK предоставляют методы, которые можно использовать для добавления или изменения пакетов через файлы установщика приложений или для получения сведений о приложениях с помощью Ассоциации установщика приложения. См. подробнее в [документации по Установщику приложений](app-installer-documentation.md).

Из этих методов [PackageManager. аддпаккажебяппинсталлерфилеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync), [PackageManager. рекуестаддпаккажебяппинсталлерфилеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)и [Package. Чеккупдатеаваилабилитясинк](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync) не поддерживаются в JavaScript. Однако можно создать [Среда выполнения Windows компонент](https://docs.microsoft.com/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript) , вызывающий эти методы, а затем вызвать этот компонент из приложения UWP для JavaScript.

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
