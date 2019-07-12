---
Description: В этом руководстве объясняется, как устранять неполадки среды выполнения в контейнере MSIX.
title: Устранение проблем во время выполнения в контейнере MSIX
ms.date: 07/11/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: fdfaedf51333f93edd7e12f1c2ad8ea879bfc8ae
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829385"
---
# <a name="troubleshoot-runtime-issues-in-an-msix-container"></a>Устранение проблем во время выполнения в контейнере MSIX 

В этой статье мы рассмотрим способы устранения проблем во время выполнения, выполняемых в контейнере MSIX. Контейнеры MSIX сами по себе являются относительно простыми и понятными. Поскольку дополнительные приложения выполняются в один и тот же идентификатор пакета с помощью изменения пакетов, виртуальный реестр и виртуальную файловую систему будут layed over, в том порядке, в котором приложения устанавливаются. 

Может существовать случаи, где порядок, в котором установлены эти приложения может привести к проблемам вероятна, где могут быть перезаписаны разделы реестра ожидаемым и ожидаемые файлы могут быть заменены. 

Поможет выявить такие проблемы [Invoke CommandInDesktopPackage](https://docs.microsoft.com/en-us/powershell/module/appx/invoke-commandindesktoppackage?view=win10-ps) командлет PowerShell, который может использоваться для запуска приложения в контейнере MSIX. Это позволяет пользователям запустить командную строку, редактор реестра, PowerShell, внутри контейнера MSIX и получите представление о объединенных файловой системы и объединенные реестра. 

 > [!IMPORTANT]
 > Вызвать CommandInDesktopPackage требуется устройство должно находиться в режиме разработчика. 


## <a name="view-the-merged-file-system"></a>Просмотреть объединенный файловой системы

Чтобы просмотреть в файловой системе, наблюдаемых приложений, работающих под управлением внутри контейнера, используйте следующую команду PowerShell:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "29270sandstorm.AppPackage1_gah1vdar1nn7a" -Command "cmd.exe" -PreventBreakaway
```

Приведенная выше команда запускает экземпляр cmd.exe в *29270sandstorm. AppPackage1_gah1vdar1nn7a* контейнер пакета. Как вы используете командную строку из внутри контейнера, можно просматривать через файловую систему и просмотреть объединенные файлы. 

## <a name="view-the-merged-registry-hive"></a>Куст реестра объединенные представления

Чтобы просмотреть полный устройства куст реестра, наблюдаемых приложений, работающих под управлением insider контейнера, используйте следующую команду PowerShell:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "29270sandstorm.AppPackage1_gah1vdar1nn7a" -Command "regedit.exe" -PreventBreakaway
```

Приведенная выше команда запускает редактор реестра, в контексте *29270sandstorm. AppPackage1_gah1vdar1nn7a* контейнер пакета. Здесь можно просматривать через локальный компьютер и реестра текущего пользователя, ключи и определить возможные виновника, которое является причиной проблемы. 

 >[!TIP]
 > Используйте "-PreventBreakaway" флаг при использовании Invoke-CommandInDesktopPackage, если вы хотите запустить последующие процессы в одном контейнере. В противном случае все последующие запуска приведет к разрыву за пределы контейнера. 

 >[!NOTE]
 > Не все приложения могут запускаться в контейнере. К примеру в explorer.exe будет breakout контейнера.
