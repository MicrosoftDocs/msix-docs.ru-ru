---
title: Поддержка MSIX на сервере 2019
description: В этой статье содержатся сведения о том, как MSIX support на сервере 2019.
ms.date: 04/12/2019
ms.topic: article
author: dianmsft
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 423c58ab10a3e30558b9076c0d2a6c2e1ded9107
ms.sourcegitcommit: 0bd5ebc32feba8a4f4669f232129a8953d5cf773
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030180"
---
# <a name="msix-support-on-server-2019"></a>Поддержка MSIX на сервере 2019

MSIX теперь поддерживается в выпуске Windows Server 2019 (LTSC) с возможностями рабочего стола. Windows Server 2019 основан на Windows 10 1809, где доступны большинство возможностей MSIX, и вы сможете использовать их преимущества.
 
Поддержка позволяет распространять одни и те же пакеты MSIX в рамках предприятия на номера SKU клиента и сервера, установку через **PowerShell** или установку непосредственно через **API диспетчера пакетов**. 
 
## <a name="considerations"></a>Рекомендации
1. Microsoft Store и Microsoft Store для бизнеса не поддерживаются в Windows Server, поэтому все приложения потребуется установить с помощью **PowerShell**.
2. Так как Windows Server 2019 основана на Windows 10 1809, она поддерживает приложения с минимальной целевой версией **10.0.17763** или ниже.
3. Кроме того, в номере SKU сервера недоступны следующие API и платформы:
- Платформа Microsoft Advertising
- Географическое расположение, Поиск Кортаны
- Покупка из приложения

4. Следующие платформы не установлены на SKU сервера, поэтому при необходимости убедитесь, что они установлены вместе с приложением: 
- Среда выполнения .NET
- .NET Framework
- Вклибс.

5. Следующие расширения MSIX не поддерживаются: 
- Windows. Фиреваллрулес
- Windows. Аппексекутионалиас
- Windows. Комсервер
- Windows. Коминтерфаце
- Windows. Поспайментконнектор
- Windows. для защиты.

> [!NOTE]
> Так как Windows Server 2019 с возможностями рабочего стола не поддерживает все функции SKU клиента Windows 10, рекомендуется протестировать приложения на сервере перед развертыванием в рабочей среде.
