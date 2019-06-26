---
title: Автоматизация преобразования файлов установщика Windows в пакеты MSIX
description: Автоматизация преобразования существующих файлов установщика Windows в пакеты MSIX с помощью интерфейса командной строки
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4408381a0ebbcc7fbdad7c517c1b64bbec2254da
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/14/2019
ms.locfileid: "58900176"
---
# <a name="automate-conversion-of-windows-installers-to-msix-packages"></a>Автоматизация преобразования файлов установщика Windows в пакеты MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Получить средство упаковки MSIX</a></p></div>

Средство упаковки MSIX поддерживает интерфейс командной строки для создания MSIX-пакетов приложений, который позволяет автоматизировать распаковку и выполнять массовое преобразование с помощью простого скрипта PowerShell.

Ниже приведен простой скрипт PowerShell, который с помощью примера шаблона преобразования создает соответствующие шаблоны для каждого установщика в пути к папке и передает эти шаблоны средству упаковки MSIX для создания пакетов MSIX.


```ps1
$root = "C:\Installers\"
$ConversionTemplate = $root + "\SampleTemplate.xml"
$MSIXSaveLocation = "C:\MSIX\Converted"

get-childitem $root -recurse | where {$_.extension -eq ".msi"} | % {
  
    $Installerpath = $_.FullName
    $filename = $_.BaseName
    $XML_Path = $root + $filename + "ConversionTemplate.xml"

    [xml]$XmlDocument = Get-Content $ConversionTemplate
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Path = $Installerpath
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Arguments = "/qb"
    $XmlDocument.MSIXPackagingToolTemplate.Installer.InstallLocation = "C:\Program Files (x86)\"

    $XmlDocument.MSIXPackagingToolTemplate.SaveLocation.Path = $MSIXSaveLocation 
    
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherName = "CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" 
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ExecutableName = $filename +".exe"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.DisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.Description = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ID = $filename +"1"

    $xmldocument.Save($XML_Path)

    MsixPackagingTool.exe create-package --template $XML_Path

}
```

