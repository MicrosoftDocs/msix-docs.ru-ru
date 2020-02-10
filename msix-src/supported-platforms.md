---
title: Поддерживаемые платформы
description: В этой статье описывается поддерживаемая платформа для MSIX.
author: dianmsft
ms.date: 12/02/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7bc8d140d0d62e0e844ca2f51f407349bd85baf7
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073640"
---
# <a name="supported-platforms"></a>Поддерживаемые платформы

В настоящее время MSIX поддерживается в следующих версиях Windows:

* Windows 10, версия 1709 и более поздние версии.
* Windows Server 2019 LTSC и более поздние версии.
* Windows Enterprise 2019 LTSC и более поздние версии.

В этой статье описывается, как основные функции MSIX поддерживаются в этих версиях Windows.

> [!NOTE]
> Для Windows Server 2019 LTSC и Windows Enterprise 2019 LTSC необходимо установить приложение **установщика приложений** для поддержки функций MSIX, таких как установка MSIX,. msixbundle,. appx или. appxbundle. Дополнительные сведения о Windows Server 2019 LTSC см. в [этой статье](msix-server-2019.md).

> [!NOTE]
> Для версий Windows, предшествующих Windows 10 версии 1709, используйте [MSIX Core](msix-core/msixcore.md) для установки пакетов MSIX.

## <a name="msix-feature-support"></a>Поддержка функций MSIX

В следующей таблице показано, какие функции и сценарии MSIX поддерживаются в разных версиях Windows.

> [!div class="mx-tableFixed"]
| Компоненты | 1709 | 1803 | 1809 | 1903 | 1909 | 2004| Windows Server 2019 LTSC | Windows Enterprise 2019 LTSC|
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Установка и удаление собственного MSIX | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :x: | :x: |
| [Поддержка файлов установщика приложений](app-installer/installing-windows10-apps-web.md)| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Удостоверение для упакованных классических приложений | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Разрешить повышение прав | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Пакеты с модификациями | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Принудительное обновление с любой предыдущей версии |  :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Платформа поддержки пакетов (ПСФ) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:|  :heavy_check_mark: | :heavy_check_mark:|  
| Службы Windows | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x:| :x: | 
| Отложить флаг регистрации |  :x: | :x: | :x: | :x:| :heavy_check_mark: | :heavy_check_mark:| :x: | :x: |
| Принудительная подготовка |  :x: | :x: | :x: | :x:| :heavy_check_mark: | :heavy_check_mark:| :x: | :x: |

## <a name="package-format-support"></a>Поддержка формата пакета

В следующей таблице показано, какие форматы пакетов поддерживаются в разных версиях Windows 10.

| Формат пакета | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .msixbundle| :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:|
| APPX | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .appxbundle | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## <a name="microsoft-store"></a>Microsoft Store

В следующей таблице показано, какие функции Microsoft Store поддерживаются в разных версиях Windows 10.

| Компоненты | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| публикация             | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Обновить уведомление| :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Потоковая установка | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| 
| Разностные обновления | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

> [!NOTE]
> . appx или. appxbundle будет работать для всех версий Windows 10, перечисленных выше. Таблица отражает только поведение msix или msixbundle.

### <a name="microsoft-store-submissions"></a>Microsoft Store отправки

Минимальная версия операционной системы, поддерживаемая пакетом MSIX, указана в файле манифеста пакета как `MinVersion` в элементе `TargetDeviceFamily`. Например, пакет MSIX может `MinVersion="10.0.17701.0"` в качестве минимальной поддерживаемой версии, что означает, что пакет MSIX может работать в этой и более поздних версиях ОС.

В Windows 10 версий 1709, 1803 и 1809 поддерживаются основные сценарии корпоративного развертывания. К ним относятся установка с помощью Microsoft Endpoint Configuration Manager, Microsoft Intune, PowerShell или двойной щелчок по установке.

В настоящее время MSIX установка с помощью Microsoft Store и Microsoft Store для бизнеса требует Windows 10, 1809 и более поздних версий.
