---
Description: В этой статье содержатся все сведения, необходимые для управления развертыванием приложений MSIX в корпоративной среде.  Эта статья предназначена для ИТ-разработчиков предприятий.
title: Управление MSIX с помощью PowerShell
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, развертывание, msix, PowerShell, PSH, PS, PoSh, командлеты
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: b5c4366ac7e22fc67fb3314737c9f59f4a057aca
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074050"
---
# <a name="managing-msix-with-powershell"></a>Управление MSIX с помощью PowerShell
В этой статье описываются командлеты PowerShell, которые используются для управления пакетами .appx и .msix.

## <a name="msix-powershell-cmdlets"></a>Командлеты PowerShell для MSIX

| Командлеты PowerShell | Описание |
|-------------------|-------------|
| [Add-AppPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) | С помощью этого командлета можно добавить подписанный пакет приложения (*.msix, *.appx) на устройство. Командлет Add-AppPackage также можно использовать при добавлении приложения MSIX, которое имеет связь с другим приложением MSIX, например: внешние пакеты, [дополнительные пакеты](https://docs.microsoft.com/windows/msix/package/optional-packages)и [семейство пакетов](https://docs.microsoft.com/windows/msix/package/optional-packages). |
| [Remove-AppPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps) | Этот командлет используется для удаления подписанного пакета приложения (*.msix, *.appx) с устройства. При удалении приложения также удаляется содержимое папки, в которую было установлено подписанное приложение, а также все ссылки на удаленное приложение на компьютере. |
| [Get-AppPackage](https://docs.microsoft.com/powershell/module/appx/get-appxpackage?view=win10-ps) | Этот командлет предоставит список всех установленных пакетов подписанного приложения (*.msix, *.appx) на компьютере. Для фильтрации результатов можно указать значение. Для создания отфильтрованного возвращаемого значения задайте полную или частичную строку в параметре **-Name** с подставленным знаком "*". |
| [Get-AppxDefaultVolume](https://docs.microsoft.com/powershell/module/appx/get-appxdefaultvolume?view=win10-ps) | Этот командлет предоставит том по умолчанию, используемый подписанными пакетами приложений (*.msix, *.appx) на компьютере. Том по умолчанию является целевым объектом для всех операций развертывания или установки на компьютере. Этот том невозможно удалить из списка томов. |
| [Get-AppPackageManifest](https://docs.microsoft.com/powershell/module/appx/get-appxpackagemanifest?view=win10-ps) | Этот командлет вернет xml-объект манифеста пакета подписанного приложения (*.msix, *.appx) для указанного подписанного приложения с полным именем пакета. |
