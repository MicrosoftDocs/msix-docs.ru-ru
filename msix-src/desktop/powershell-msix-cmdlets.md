---
Description: В этой статье содержатся все сведения, необходимые для управления развертыванием приложений MSIX в корпоративной среде.  Эта статья предназначена для ИТ-разработчиков предприятий.
title: Управление MSIX с помощью PowerShell
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, развертывание, msix, PowerShell, PSH, PS, PoSh, командлеты
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 7141207e336347f092f7bb23ae35fbd75464887b
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090442"
---
# <a name="managing-msix-with-powershell"></a>Управление MSIX с помощью PowerShell
В этой статье описываются командлеты PowerShell, которые используются для управления пакетами .appx и .msix.

## <a name="msix-powershell-cmdlets"></a>Командлеты PowerShell для MSIX

| Командлеты PowerShell | Описание |
|-------------------|-------------|
| [Add-AppPackage](/powershell/module/appx/add-appxpackage?view=win10-ps) | С помощью этого командлета можно добавить подписанный пакет приложения (*.msix, *.appx) на устройство. Командлет Add-AppPackage также можно использовать при добавлении приложения MSIX, которое имеет связь с другим приложением MSIX, например: внешние пакеты, [дополнительные пакеты](../package/optional-packages.md)и [семейство пакетов](../package/optional-packages.md). |
| [Remove-AppPackage](/powershell/module/appx/remove-appxpackage?view=win10-ps) | Этот командлет используется для удаления подписанного пакета приложения (*.msix, *.appx) с устройства. При удалении приложения также удаляется содержимое папки, в которую было установлено подписанное приложение, а также все ссылки на удаленное приложение на компьютере. |
| [Get-AppPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) | Этот командлет предоставит список всех установленных пакетов подписанного приложения (*.msix, *.appx) на компьютере. Для фильтрации результатов можно указать значение. Для создания отфильтрованного возвращаемого значения задайте полную или частичную строку в параметре **-Name** с подставленным знаком "*". |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume?view=win10-ps) | Этот командлет предоставит том по умолчанию, используемый подписанными пакетами приложений (*.msix, *.appx) на компьютере. Том по умолчанию является целевым объектом для всех операций развертывания или установки на компьютере. Этот том невозможно удалить из списка томов. |
| [Get-AppPackageManifest](/powershell/module/appx/get-appxpackagemanifest?view=win10-ps) | Этот командлет вернет xml-объект манифеста пакета подписанного приложения (*.msix, *.appx) для указанного подписанного приложения с полным именем пакета. |