---
title: Поддержка MSIX в Windows Server 2019
description: В этой статье содержатся сведения о поддержке MSIX в Windows Server 2019
ms.date: 01/16/2020
ms.topic: article
author: dianmsft
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, MSIX, MSIX Core, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: e897cd77785c345863c0e2e17bc4dbf8d73b854f
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091192"
---
# <a name="msix-support-on-windows-server-2019"></a>Поддержка MSIX в Windows Server 2019

MSIX теперь поддерживается в Windows Server 2019 (LTSC) с возможностями рабочего стола. Эта поддержка позволяет распространять одни и те же пакеты MSIX в рамках предприятия на номера SKU клиента и сервера, а также устанавливать такие пакеты с помощью **PowerShell** или непосредственно через [API диспетчера пакетов](/uwp/api/Windows.Management.Deployment.PackageManager).

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