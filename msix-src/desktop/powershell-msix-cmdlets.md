---
description: В этой статье содержатся все сведения, необходимые для управления развертыванием приложений MSIX в корпоративной среде.  Эта статья предназначена для ИТ-разработчиков предприятий.
title: Управление MSIX с помощью PowerShell
ms.date: 12/4/2020
ms.topic: article
keywords: windows 10, развертывание, msix, PowerShell, PSH, PS, PoSh, командлеты
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: e90acc3aa30f689943d16c014db87322af3e8812
ms.sourcegitcommit: de0c711ce28851ef6976a71dd1317291f278b4d1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2020
ms.locfileid: "96612112"
---
# <a name="managing-msix-with-powershell"></a>Управление MSIX с помощью PowerShell
В этой статье описываются командлеты PowerShell, которые используются для управления пакетами .appx и .msix.

## <a name="msix-powershell-cmdlets"></a>Командлеты PowerShell для MSIX

| Командлеты PowerShell | Описание |
|-------------------|-------------|
| [Add-AppPackage](/powershell/module/appx/add-appxpackage) | С помощью этого командлета можно добавить подписанный пакет приложения (*.msix, *.appx) на устройство. Командлет Add-AppPackage также можно использовать при добавлении приложения MSIX, которое имеет связь с другим приложением MSIX, например: внешние пакеты, [дополнительные пакеты](../package/optional-packages.md)и [семейство пакетов](../package/optional-packages.md). |
| [Remove-AppPackage](/powershell/module/appx/remove-appxpackage) | Этот командлет используется для удаления подписанного пакета приложения (*.msix, *.appx) с устройства. При удалении приложения также удаляется содержимое папки, в которую было установлено подписанное приложение, а также все ссылки на удаленное приложение на компьютере. |
| [Get-AppPackage](/powershell/module/appx/get-appxpackage) | Этот командлет предоставит список всех установленных пакетов подписанного приложения (*.msix, *.appx) на компьютере. Для фильтрации результатов можно указать значение. Для создания отфильтрованного возвращаемого значения задайте полную или частичную строку в параметре **-Name** с подставленным знаком "*". |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume) | Этот командлет предоставит том по умолчанию, используемый подписанными пакетами приложений (*.msix, *.appx) на компьютере. Том по умолчанию является целевым объектом для всех операций развертывания или установки на компьютере. Этот том невозможно удалить из списка томов. |
| [Get-AppPackageManifest](/powershell/module/appx/get-appxpackagemanifest) | Этот командлет вернет xml-объект манифеста пакета подписанного приложения (*.msix, *.appx) для указанного подписанного приложения с полным именем пакета. |
| [Reset-AppxPackage](/powershell/module/appx/reset-appxpackage) | Этот командлет сбрасывает установленное приложение до исходных параметров. |
| [Get-AppxVolume](/powershell/module/appx/get-appxvolume) | Этот командлет возвращает список объектов AppxVolume, известных компьютеру. |
| [Add-AppxVolume](/powershell/module/appx/add-appxvolume) | Этот командлет добавляет новый том AppxVolume, объявляемый диспетчером пакетов. |
| [Remove-AppxVolume](/powershell/module/appx/remove-appxvolume) | Этот командлет удаляет существующий том AppxVolume с устройства. |
| [Mount-AppxVolume](/powershell/module/appx/mount-appxvolume) | Этот командлет подключает том AppxVolume, предоставляя доступ ко всем приложениям, которые развернуты на целевом томе. |
| [Dismount-AppxVolume](/powershell/module/appx/dismount-appxvolume) | Этот командлет отключает том AppxVolume, блокируя доступ к приложениям, которые развернуты на целевом томе. |
| [Move-AppxPackage](/powershell/module/appx/move-appxpackage) | Этот командлет перемещает пакет приложения Windows из текущего расположения в другой подключенный том AppxVolume. |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume) | Этот командлет получает том AppxVolume по умолчанию, используемый в качестве целевого для всех операций развертывания на устройстве. |
| [Set-AppxDefaultVolume](/powershell/module/appx/set-appxdefaultvolume) | Этот командлет задает дополнительный подключенный том AppxVolume в качестве нового целевого тома по умолчанию для всех операций развертывания на устройстве. |
| [Invoke-CommandInDesktopPackage](/powershell/module/appx/invoke-commandindesktoppackage) | Этот командлет разрешает выполнять команды в контексте пакета приложения Windows. |
