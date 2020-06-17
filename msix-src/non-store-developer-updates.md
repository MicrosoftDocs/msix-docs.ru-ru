---
title: Обновление приложений, опубликованных не в Store, с помощью кода
description: Описывает, как пакеты MSIX, поставляющиеся за пределы хранилища, могут обновляться разработчиками в коде.
author: Huios
ms.date: 06/12/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: e68269c0a17254fd92fb00fad6bcbc1d7a16e588
ms.sourcegitcommit: 6243b7aca6f52f007f4571c835f580f433c31769
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/16/2020
ms.locfileid: "84812779"
---
# <a name="update-non-store-published-app-packages-from-your-code"></a>Обновление опубликованных пакетов приложений без сохранения из кода

При отправке приложения в качестве MSIX можно программно запустить обновление приложения. Если приложение развертывается за пределами магазина, необходимо проверить сервер на наличие новой версии приложения и установить новую версию. Способ применения обновления зависит от того, выполняется ли развертывание пакета приложения с помощью файла установщика приложения. Чтобы применить обновления из кода, пакет приложения должен объявить `packageManagement` возможность.

В этой статье приведены примеры, демонстрирующие способы объявления `packageManagement` возможностей в манифесте пакета и применения обновления из кода. В первом разделе рассматривается, как это сделать, если вы используете файл установщика приложений, а во втором разделе объясняется, как это сделать, когда файл установщика приложения **не** используется. В последнем разделе рассматривается, как убедиться, что приложение перезапускается после применения обновления.

## <a name="add-the-packagemanagement-capability-to-your-package-manifest"></a>Добавление функции PackageManagement в манифест пакета

Чтобы использовать `PackageManager` API, приложение должно объявить `packageManagement` [ограниченную возможность](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) в [манифесте пакета](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest).

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

## <a name="updating-packages-deployed-using-an-app-installer-file"></a>Обновление пакетов, развернутых с помощью файла установщика приложения

При развертывании приложения с помощью файла установщика приложения все выполняемые обновления, управляемые кодом, должны использовать [API файлов установщика приложений](https://docs.microsoft.com/windows/msix/app-installer/app-installer-documentation#app-installer-file-apis). Это гарантирует, что ваши обновления файлов установщика приложения будут продолжать работать. Для интиатинг обновления на основе установщика приложений из кода можно использовать [PackageManager. аддпаккажебяппинсталлерфилеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync?view=winrt-19041) или [PackageManager. рекуестаддпаккажебяппинсталлерфилеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync?view=winrt-19041). Проверить, доступно ли обновление, можно с помощью API [Package. чеккупдатеаваилабилитясинк](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync?view=winrt-19041) . Ниже приведен пример кода.

```csharp
using Windows.Management.Deployment;

public async void CheckForAppInstallerUpdatesAndLaunchAsync(string targetPackageFullName, PackageVolume packageVolume)
{
    // Get the current app's package for the current user.
    PackageManager pm = new PackageManager();
    Package package = pm.FindPackageForUser(string.Empty, targetPackageFullName);

    PackageUpdateAvailabilityResult result = await package.CheckUpdateAvailabilityAsync();
    switch (result.Availability)
    {
        case PackageUpdateAvailability.Available:
        case PackageUpdateAvailability.Required:
            //Queue up the update and close the current instance
            await pm.AddPackageByAppInstallerFileAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.appinstaller"),
            AddPackageByAppInstallerOptions.ForceApplicationShutdown,
            packageVolume);
            break;
        case PackageUpdateAvailability.NoUpdates:
            // Close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
        case PackageUpdateAvailability.Unknown:
        default:
            // Log and ignore error.
            Logger.Log($"No update information associated with app {targetPackageFullName}");
            // Launch target app and close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
    }
}
```

## <a name="updating-packages-deployed-without-an-app-installer-file"></a>Обновление пакетов, развернутых без файла установщика приложения


### <a name="check-for-updates-on-your-server"></a>Проверка наличия обновлений на сервере

Если вы **не** используете файл установщика приложения для развертывания пакета приложения, сначала необходимо проверить доступность новой версии приложения. В следующем примере проверяется, чтобы версия пакета на сервере была больше текущей версии приложения (в этом примере для демонстрационных целей используется тестовый сервер).

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

### <a name="apply-the-update"></a>Применение обновления 

После того как вы определили, что обновление доступно, вы можете поставить его в очередь для скачивания и установки с помощью API [аддпаккажеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackageasync?view=winrt-19041) . Обновление будет применено при следующем завершении работы приложения. После перезапуска приложения новая версия будет доступна пользователю. Ниже приведен пример кода.

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
            AddPackageOptions.ForceApplicationShutdown
        );
    }
}
```
## <a name="automatically-restarting-your-app-after-an-update"></a>Автоматический перезапуск приложения после обновления

Если приложение является приложением UWP, передача в Аддпаккажебяппинсталлероптионс. Форцеаппликатионшутдовн или Аддпаккажеоптионс. Форцетаржетаппшутдовн при применении обновления должна запланировать перезапуск приложения после завершения работы и обновления. Для приложений, не являющихся универсальными UWP, необходимо вызвать [регистераппликатионрестарт](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions#updates) перед применением обновления.

Необходимо вызвать Регистераппликатионрестарт, прежде чем приложение начнет работу. Ниже приведен пример использования служб взаимодействия для вызова собственного метода в C#:

```csharp
 // Register the active instance of an application for restart in your Update method
 uint res = RelaunchHelper.RegisterApplicationRestart(null, RelaunchHelper.RestartFlags.NONE);
```

Пример вспомогательного класса для вызова собственного метода Регистераппликатионрестарт в C#:

```csharp
using System;
using System.Runtime.InteropServices;

namespace MyEmployees.Helpers
{
    class RelaunchHelper
    {
        #region Restart Manager Methods
        /// <summary>
        /// Registers the active instance of an application for restart.
        /// </summary>
        /// <param name="pwzCommandLine">
        /// A pointer to a Unicode string that specifies the command-line arguments for the application when it is restarted.
        /// The maximum size of the command line that you can specify is RESTART_MAX_CMD_LINE characters. Do not include the name of the executable
        /// in the command line; this function adds it for you.
        /// If this parameter is NULL or an empty string, the previously registered command line is removed. If the argument contains spaces,
        /// use quotes around the argument.
        /// </param>
        /// <param name="dwFlags">One of the options specified in RestartFlags</param>
        /// <returns>
        /// This function returns S_OK on success or one of the following error codes:
        /// E_FAIL for internal error.
        /// E_INVALIDARG if rhe specified command line is too long.
        /// </returns>
        [DllImport("kernel32.dll", CharSet = CharSet.Unicode)]
        internal static extern uint RegisterApplicationRestart(string pwzCommandLine, RestartFlags dwFlags);
        #endregion Restart Manager Methods

        #region Restart Manager Enums
        /// <summary>
        /// Flags for the RegisterApplicationRestart function
        /// </summary>
        [Flags]
        internal enum RestartFlags
        {
            /// <summary>None of the options below.</summary>
            NONE = 0,

            /// <summary>Do not restart the process if it terminates due to an unhandled exception.</summary>
            RESTART_NO_CRASH = 1,
            /// <summary>Do not restart the process if it terminates due to the application not responding.</summary>
            RESTART_NO_HANG = 2,
            /// <summary>Do not restart the process if it terminates due to the installation of an update.</summary>
            RESTART_NO_PATCH = 4,
            /// <summary>Do not restart the process if the computer is restarted as the result of an update.</summary>
            RESTART_NO_REBOOT = 8
        }
        #endregion Restart Manager Enums

    }
}
```
