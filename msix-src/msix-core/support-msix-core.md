---
title: Обновление существующего пакета MSIX для поддержки MSIX Core
description: В этой статье представлен обзор MSIX Core, в котором поддерживается MSIX поддержка Windows 7 с пакетом обновления 1 (SP1), Windows 8.1, поддерживаемых в настоящее время Windows Server (с возможностями рабочего стола) и версий Windows 10 до 1709 (годовщина обновления).
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 3fb4d333c0ed84a4d54522e17b7e4ab54c6e28be
ms.sourcegitcommit: f6cee51b46fc36a57b5cf9c8cb3fd24a40ae858a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391625"
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

## <a name="update-manifest-using-the-msix-packaging-tool"></a>Обновление манифеста с помощью средства упаковки MSIX 
Если у вас есть пакет MSIX, можно использовать средство пакета MSIX, чтобы обновить пакет для поддержки MSIX Core. Ниже приведены действия. 
1. Открыть приложение **инструмента упаковки MSIX**
2. Выбор **редактора пакетов** 
3. Щелкните " **Обзор...** ", чтобы найти пакет
4. Щелкните **Открыть пакет** .
5. В разделе **файл манифеста**щелкните **Открыть файл** .
6. Вы просматриваете манифест пакета. В разделе **зависимость** добавьте MSIX Core в качестве целевого семейства устройств (см. выше).
7. Сохранение и закрытие манифеста 
8. Повторно подписать пакет 
9. Нажмите кнопку **сохранить** и укажите, нужно ли увеличивать пакет. 

## <a name="windows-versions-supported-by-msix-core"></a>Версии Windows, поддерживаемые MSIX Core

| Имя | Версия |
|------|---------|
| Windows 7, SP 1| 6.1.7601.0|
| Windows 8.1 (обновление 1) |6.3.9600.0|
| Windows 10 2015 LTSB (1507)|10.0.10240.0|
| Windows 10 2016 LTSB (1607)|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
