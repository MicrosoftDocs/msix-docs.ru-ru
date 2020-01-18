---
title: Поддержка MSIX на сервере 2019
description: В этой статье содержатся сведения о том, как MSIX support на сервере 2019.
ms.date: 01/16/2020
ms.topic: article
author: dianmsft
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d5da083602173560a9fa481f138e108e851d84fc
ms.sourcegitcommit: e1da85c80d639646f9c9dd7ab26e7e4bffd1c9dd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76248426"
---
# <a name="msix-support-on-windows-server-2019"></a>Поддержка MSIX в Windows Server 2019

MSIX теперь поддерживается в Windows Server 2019 (LTSC) с возможностями рабочего стола. Эта поддержка позволяет распространять одни и те же пакеты MSIX в рамках предприятия на номера SKU клиента и сервера, установку через **PowerShell** или установку непосредственно через [API диспетчера пакетов](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager).

## <a name="considerations"></a>Рекомендации

* Microsoft Store и Microsoft Store для бизнеса не поддерживаются в Windows Server 2019. Все приложения должны быть установлены с помощью **PowerShell**.
* Так как Windows Server 2019 основана на Windows 10 версии 1809, она поддерживает приложения с минимальной целевой версией **10.0.17763** или ниже.
* Следующие API и платформы недоступны в Windows Server 2019:
  * Microsoft Advertising
  * Геолокация
  * Поиск кортаны
  * Покупка из приложения
* Следующие платформы не установлены в Windows Server 2019. Вы обязаны убедиться, что они установлены вместе с приложением при необходимости.
  * Среда выполнения .NET
  * Платформа .NET Framework
  * вклибс
* Следующие расширения пакета не поддерживаются в Windows Server 2019:
  * Windows. Фиреваллрулес
  * Windows. Аппексекутионалиас
  * Windows. Комсервер
  * Windows. Коминтерфаце
  * Windows. Поспайментконнектор
  * Windows.

> [!NOTE]
> Поскольку Windows Server 2019 с возможностями рабочего стола не поддерживает все функции SKU клиента Windows 10, мы рекомендуем протестировать приложения на Windows Server 2019 перед развертыванием в рабочей среде.
