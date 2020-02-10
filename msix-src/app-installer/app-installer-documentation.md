---
title: Документация по файлу установщика связанных приложений
description: В этой статье содержатся ссылки на документацию по схеме файлов установщика приложений и связанных с ними API-интерфейсов, предоставляемых Windows SDK.
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, UWP, установщик приложений, AppInstaller, загружать неопубликованные, API, XML, схема
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 39715076139c502b21074b2d180370745c7194f5
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072633"
---
# <a name="app-installer-file-apis"></a>API файлов установщика приложений

Классы [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) и [Package](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) в Windows SDK предоставляют методы, которые можно использовать для добавления или изменения пакетов через файлы установщика приложений, а также для получения сведений о приложениях с помощью Ассоциации установщика приложения.

|  Метод  |  Описание | Минимальный поддерживаемый выпуск |
|----------|--------------|-------------------|
|  [PackageManager. Аддпаккажебяппинсталлерфилеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | Обеспечивает установку одного или нескольких пакетов приложения с файлом APPINSTALLER. | Обновление для дизайнеров Windows 10 (версия 1709, сборка 16299)   |
|  [PackageManager. Рекуестаддпаккажебяппинсталлерфилеасинк](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | Обеспечивает установку одного или нескольких пакетов приложения с файлом APPINSTALLER. После этого будет выполнен фильтр SmartScreen и проверка пользователя перед установкой пакетов приложений. | Обновление для дизайнеров Windows 10 (версия 1709, сборка 16299)       |
|  [Package. Жетаппинсталлеринфо](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | Возвращает расположение XML-файла appinstaller. Это позволяет разработчикам приложений извлекать расположение XML-файла appinstaller при необходимости в приложении. | Windows 10, версия 1809 (сборка 17763) |
|  [Package. Чеккупдатеаваилабилитясинк](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | Проверяет наличие обновлений для основного пакета приложения, указанного в файле. appinstaller. Он позволяет разработчику определить, требуются ли обновления из-за политики. appinstaller. В настоящее время этот метод работает только для приложений, установленных с помощью файлов. appinstaller. | Windows 10, версия 1809 (сборка 17763) |

## <a name="app-installer-file-schema"></a>Схема файла Установщика приложений

Дополнительные сведения о форматировании файла установщика приложения вручную см. в разделе [Справочник по схеме файлов установщика приложений](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).
