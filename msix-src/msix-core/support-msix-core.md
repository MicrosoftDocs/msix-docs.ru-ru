---
title: Обновление существующих MSIX для поддержки MSIX Core
description: В этой статье представлен обзор MSIX Core, в котором поддерживается MSIX поддержка Windows 7 с пакетом обновления 1 (SP1), Windows 8.1, поддерживаемых в настоящее время Windows Server (с возможностями рабочего стола) и версий Windows 10 до 1709 (годовщина обновления).
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 5f269d933f3b513c7984000e0ee855e0ffa9a63f
ms.sourcegitcommit: 0412ba69187ce791c16313d0109a5d896141d44c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303384"
---
# <a name="update-your-existing-msix-package-to-support-msix-core"></a>Обновление существующего пакета MSIX для поддержки MSIX Core

Перед развертыванием пакета MSIX с помощью [MSIX Core](msixcore.md)необходимо сначала обновить манифест пакета MSIX.

Приложения, Упакованные в MSIX, должны быть совместимы с операционной системой, в которой они развертываются. Манифест пакета MSIX должен содержать правильный **TargetDeviceFamily** с именем **мсикскоре. Desktop** и **MinVersion** , соответствующей номеру сборки операционной системы. Также обязательно включите соответствующую запись Windows 10, версии 1709 и более поздних версий, чтобы приложение было правильно развернуто в операционных системах, которые изначально поддерживают MSIX.

В следующем примере указывается минимальная версия Windows 7 с пакетом обновления 1 (SP1):

```xml
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

Все приложения **мсикскоре. Desktop** развертываются в Windows Server с операционными системами на основе рабочих столов с одинаковым номером сборки. Если приложение предназначено только для серверной операционной системы, укажите **TargetDeviceFamily** с именем **мсикскоре. Server**. Развертывание в Windows Server Core не поддерживается.

## <a name="windows-versions-supported-by-msix-core"></a>Версии Windows, поддерживаемые MSIX Core

| Имя | Версия |
|------|---------|
| Windows 7, SP 1| 6.1.7601.0|
| Windows 8.1 (обновление 1) |6.3.9600.0|
| Windows 10 2015 LTSB (1507)|10.0.10240.0|
| Windows 10 2016 LTSB (1607)|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
