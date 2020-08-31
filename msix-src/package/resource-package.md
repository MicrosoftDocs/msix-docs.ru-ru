---
title: Пакеты ресурсов
description: В этой статье описываются пакеты ресурсов и способы их использования разработчиками в коде.
ms.date: 02/06/2020
author: dianmsft
ms.topic: article
ms.author: diahar
keywords: Windows 10, msix, UWP, необязательные пакеты, связанный набор, расширение пакета, Visual Studio
ms.localizationpriority: medium
ms.openlocfilehash: d85cbd0dcca860cf46f2641af0f3fe852aff8855
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091082"
---
# <a name="resource-packages"></a>Пакеты ресурсов 
Пакеты ресурсов предлагают отличный способ сократить объем дискового пространства, используемого сегментированным языком или масштабируемым ресурсом, на отдельные пакеты, которые автоматически загружаются Windows в зависимости от конфигурации компьютера пользователя. Если пользователь добавляет новый язык в список языков ОС в **регионе & языковых** параметров или изменяет конфигурацию экрана в автоматическом обновлении хранилища, ОС выберет применимые пакеты ресурсов для всех установленных приложений на устройстве.

В Windows SDK 10.0.17095.0 [АДДРЕСАУРЦЕПАККАЖЕАСИНК API](/uwp/api/Windows.ApplicationModel.PackageCatalog) позволяет разработчикам устанавливать пакет ресурсов для приложения по требованию. 


## <a name="how-to-use-the-addresourcepackageasync-api"></a>Как использовать API Аддресаурцепаккажеасинк 
- Аддресаурцепаккажеасинк принимает **паккажефамилинаме** приложения и идентификатор ресурса, который однозначно определяет пакет ресурсов, который пытается загрузить. Идентификатор ресурса соответствует элементу **ResourceId** в **AppxManifest.xml** пакета ресурсов.

- Приложение должно перезапуститься, чтобы получить объединенное представление всех ресурсов, доступных для приложения. API предлагает параметр **форцетаржетаппликатионшутдовн** , который может передавать для перезапуска приложения.

- Если доступно обновление для приложения при скачивании пакета ресурсов, API завершится ошибкой, так как старая версия приложения больше не будет доступна. Передавая флаг **апплюпдатеифаваилабле** , API обновляет приложение, а также получает запрошенный пакет ресурсов в рамках той же загрузки. 

- API возвращает сведения о ходе загрузки, которые могут отображаться в приложении. Кроме того, обратите внимание, что для приложений, опубликованных в магазине, получение пакета ресурсов отображается на странице **загрузки и обновления** Microsoft Store и отображается как обновление приложения. Конечный пользователь может приостановить или отменить загрузку пакета ресурсов из Microsoft Store. 

Пример 
```
 private async void DownloadGermanResourcePackage(object sender, RoutedEventArgs e)
{            
    // ResourceId that is unique to the resource package
    string resourceId = "split.language-de";
    // Warn user that application will need to restart.
    // To take update if available pass in the ApplyUpdateIfAvailable flag
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    PackageCatalogAddResourcePackageResult result = await packageCatalog.AddResourcePackageAsync("29270depappf.CaffeMacchiato_gah1vdar1nn7a", resourceId, 
        AddResourcePackageOptions.ApplyUpdateIfAvailable | AddResourcePackageOptions.ForceTargetApplicationShutdown)
        .AsTask<PackageCatalogAddResourcePackageResult, PackageInstallProgress>(new Progress
        (progress =>;
        {
                // Draw progress
        }));

        if (result.ExtendedError != null)
        {
                //Display error or retry if needed
        }
}
```
## <a name="validation"></a>Проверка
 Для локальной проверки разработчики могут создать файл. msixbundle или. appxbundle и установить его с локального диска, сетевой папки или веб Server. Когда приложение вызывает API Аддресаурцепаккажеасинк, Windows получит пакет ресурсов из расположения, в котором было установлено исходное приложение. Это делает проверку простой. После этого приложение будет готово к развертыванию. 

## <a name="removing-resource-packages"></a>Удаление пакетов ресурсов 
Приложение может выбрать удаление, чтобы удалить пакеты ресурсов, загруженные с помощью [API ремовересаурцепаккажеасинк](/uwp/api/Windows.ApplicationModel.PackageCatalog). Однако пакеты ресурсов, применимые для пользователя или устройства в целом, не могут быть удалены. 

Пример 
```
 private async void RemoveResourcePackage(object sender, RoutedEventArgs e)
{            
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    List resourcePackagesToRemove = new List();
    resourcePackagesToRemove.Add("split.language-de");
    //Warn user that application will be restarted
    var removePackageResult = await packageCatalog.RemoveResourcePackagesAsync(resourcePackagesToRemove);
    if (removePackageResult.ExtendedError != null)
    {
        // display error
    }
}
```