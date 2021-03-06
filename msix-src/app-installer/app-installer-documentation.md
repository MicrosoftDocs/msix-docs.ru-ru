---
title: Документация по файлу установщика связанных приложений
description: В этой статье содержатся ссылки на документацию по схеме файлов установщика приложений и связанных с ними API-интерфейсов, предоставляемых Windows SDK.
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, UWP, установщик приложений, AppInstaller, загружать неопубликованные, API, XML, схема
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: e38df2e69e6eda70228b08c0c54233ae34d25063
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89089812"
---
# <a name="app-installer-file-apis"></a>API для файлов Установщика приложений

Классы [PackageManager](/uwp/api/windows.management.deployment.packagemanager) и [Package](/uwp/api/windows.applicationmodel.package) в Windows SDK предоставляют методы, которые можно использовать для добавления или изменения пакетов через файлы установщика приложений, а также для получения сведений о приложениях с помощью Ассоциации установщика приложения.

|  Метод  |  Описание | Минимальный поддерживаемый выпуск |
|----------|--------------|-------------------|
|  [PackageManager. Аддпаккажебяппинсталлерфилеасинк](/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | Позволяет устанавливать один или несколько пакетов приложений с помощью файла. appinstaller. | Обновление для дизайнеров Windows 10 (версия 1709, сборка 16299)   |
|  [PackageManager. Рекуестаддпаккажебяппинсталлерфилеасинк](/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | Позволяет устанавливать один или несколько пакетов приложений с помощью файла. appinstaller. После этого будет выполнен фильтр SmartScreen и проверка пользователя перед установкой пакетов приложений. | Обновление для дизайнеров Windows 10 (версия 1709, сборка 16299)       |
|  [Package.GetAppInstallerInfo](/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | Возвращает расположение XML-файла appinstaller. Это позволяет разработчикам приложений извлекать расположение XML-файла appinstaller при необходимости в приложении. | Windows 10, версия 1809 (сборка 17763) |
|  [Package.CheckUpdateAvailabilityAsync](/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | Проверяет наличие обновлений для основного пакета приложения, указанного в файле. appinstaller. Он позволяет разработчику определить, требуются ли обновления из-за политики. appinstaller. В настоящее время этот метод работает только для приложений, установленных с помощью файлов. appinstaller. | Windows 10, версия 1809 (сборка 17763) |

## <a name="app-installer-file-schema"></a>Схема файла Установщика приложений

Дополнительные сведения о форматировании файла установщика приложения вручную см. в разделе [Справочник по схеме файлов установщика приложений](/uwp/schemas/appinstallerschema/app-installer-file).