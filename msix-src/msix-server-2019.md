---
title: Поддержка MSIX в Windows Server 2019
description: В этой статье содержатся сведения о поддержке MSIX в Windows Server 2019
ms.date: 01/16/2020
ms.topic: article
author: dianmsft
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, MSIX, MSIX Core, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d5da083602173560a9fa481f138e108e851d84fc
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2020
ms.locfileid: "76248426"
---
# <a name="msix-support-on-windows-server-2019"></a>Поддержка MSIX в Windows Server 2019

MSIX теперь поддерживается в Windows Server 2019 (LTSC) с возможностями рабочего стола. Эта поддержка позволяет распространять одни и те же пакеты MSIX в рамках предприятия на номера SKU клиента и сервера, а также устанавливать такие пакеты с помощью **PowerShell** или непосредственно через [API диспетчера пакетов](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager).

## <a name="considerations"></a>Рекомендации

* Windows Server 2019 не поддерживает Microsoft Store и Microsoft Store для бизнеса. Все приложения следует устанавливать с помощью **PowerShell**.
* Решение Windows Server 2019 разработано на основе Windows 10 версии 1809. Поэтому оно поддерживает приложения минимальной целевой версии **10.0.17763** или ниже.
* В Windows Server 2019 недоступны следующие API и платформы:
  * Microsoft Advertising
  * Геолокация
  * поиск с помощью Кортаны;
  * покупка из приложения.
* В Windows Server 2019 не установлены перечисленные ниже платформы. Вам следует убедиться, что при необходимости они устанавливаются вместе с приложением:
  * среда выполнения .NET;
  * Платформа .NET Framework
  * VCLibs.
* Windows Server 2019 не поддерживает следующие расширения пакета:
  * windows.firewallRules;
  * windows.appExecutionAlias;
  * windows.comServer;
  * windows.comInterface;
  * windows.posPaymentConnector;
  * windows.dataProtection.

> [!NOTE]
> Windows Server 2019 с возможностями рабочего стола поддерживает не все функции номеров SKU клиента Windows 10. Поэтому мы рекомендуем протестировать приложения в Windows Server 2019 перед развертыванием в рабочей среде.
