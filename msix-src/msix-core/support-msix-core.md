---
title: Обновление существующих MSIX для поддержки MSIX Core
description: В этой статье представлен обзор MSIX Core, в котором поддерживается MSIX поддержка Windows 7 с пакетом обновления 1 (SP1), Windows 8.1, поддерживаемых в настоящее время Windows Server (с возможностями рабочего стола) и версий Windows 10 до 1709 (годовщина обновления).
ms.date: 04/12/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: a662ac31dce888c7dc373e546c7a70e1c62e4653
ms.sourcegitcommit: 8aafaa9ac4087ef2e95030343add8fe2ee1cccc9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2019
ms.locfileid: "75186790"
---
# <a name="update-your-existing-msix-to-support-msix-core"></a>Обновление существующих MSIX для поддержки MSIX Core 
Чтобы использовать существующие пакеты MSIX в Windows 7 с пакетом обновления 1 (SP1), Windows 8.1, поддерживаемые в настоящее время Windows Server (с возможностями рабочего стола) и версии Windows 10 до 1709 (обновление годовщины), необходимо обновить манифест пакета MSIX. Приложения, Упакованные в MSIX, должны быть совместимы с операционной системой, в которой они развертываются.  Манифест пакета MSIX должен содержать правильный **TargetDeviceFamily** с именем Мсикскоре. Desktop и MinVersion, соответствующей номеру сборки операционной системы.  Также обязательно включите соответствующую запись Windows 10 1709 и более поздних версий, чтобы приложение было правильно развернуто в операционных системах, которые изначально поддерживают MSIX.

Пример для Windows 7 с пакетом обновления 1 (SP1) в качестве минимальной версии:

```
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

Все приложения Мсикскоре. Desktop развертываются в Windows Server с операционными системами на основе рабочих столов с одинаковым номером сборки.  Если приложение предназначено только для серверной операционной системы, используйте TargetDeviceFamily Мсикскоре. Server.  Развертывание в Windows Server Core не поддерживается.

## <a name="supported-versions-of-windows-with-msix-core"></a>Поддерживаемые версии Windows с MSIX Core 

| Имя | Версия |
|------|---------|
| Windows 7, пакет обновления 1 (SP1)| 6.1.7601.0|
| Windows 8.1 (обновление 1) |6.3.9600.0|
| Windows 10 2015 LTSB (1507)|10.0.10240.0|
| Windows 10 2016 LTSB (1607)|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
