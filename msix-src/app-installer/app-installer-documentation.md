---
title: Сопутствующая документация файл установщика приложений
description: Выводит сведения о схеме файла установщика приложений и API-интерфейсы.
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, универсальной платформы Windows, установщик приложения, AppInstaller, неопубликованные, API, XML, схемы
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 48ca61da7e4e568e9ae9381c353a658be6d80ae9
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828656"
---
# <a name="related-app-installer-file-documentation"></a>Сопутствующая документация файл установщика приложений

## <a name="app-installer-file-apis"></a>API-интерфейсов файлового установщика приложений

[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) и [пакета](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) классы в пакете SDK для Windows содержат методы, которые можно использовать для добавления или изменения пакетов с помощью файлов установщика приложений или для получения сведений о приложениях с установщиком приложения связь.

|  Метод  |  Описание | Минимальный поддерживаемый выпуск |
|----------|--------------|-------------------|
|  [PackageManager.AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | Разрешает установку с файлом .appinstaller приложения одного или нескольких пакетов. | Windows 10 Fall Creators Update (версия 1709, сборка 16299)   |
|  [PackageManager.RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | Разрешает установку с файлом .appinstaller приложения одного или нескольких пакетов. Это приведет к выполнению SmartScreen filter и пользователя проверки перед установкой пакетов приложений. | Windows 10 Fall Creators Update (версия 1709, сборка 16299)       |
|  [Package.GetAppInstallerInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | Возвращает расположение .appinstaller XML-файла. Это позволяет разработчикам приложений получить расположение .appinstaller XML-файла, если это требуется для их приложений. | Windows 10 версии 1809 (сборка 17763) |
|  [Package.CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | Проверяет наличие обновлений для пакета основного приложения, перечисленные в файле .appinstaller. Он позволяет разработчику определить необходимость обновления из-за политики .appinstaller. Этот метод в настоящее время работает только для приложений, установленных с помощью .appinstaller файлов. | Windows 10 версии 1809 (сборка 17763) |

## <a name="app-installer-file-schema"></a>Схема файла Установщика приложений

Дополнительные сведения о том, как вручную отформатировать файл установщика приложения, см. в разделе [по схеме файла установщика приложения](https://docs.microsoft.com/en-us/uwp/schemas/appinstallerschema/app-installer-file).
