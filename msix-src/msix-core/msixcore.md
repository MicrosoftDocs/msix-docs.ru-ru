---
title: MSIX Core
description: В этой статье представлен обзор MSIX Core, в котором поддерживается MSIX поддержка Windows 7 с пакетом обновления 1 (SP1), Windows 8.1, поддерживаемых в настоящее время Windows Server (с возможностями рабочего стола) и версий Windows 10 до 1709 (годовщина обновления).
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: f12efb71fb004f7354ef50193ef2127173e82ded
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090262"
---
# <a name="msix-core"></a>MSIX Core

MSIX Core обеспечивает поддержку MSIX для более ранних версий Windows, чем Windows 10, версии 1709. MSIX Core — это [проект с открытым исходным кодом](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore) в GitHub, который позволяет этим предыдущим версиям Windows устанавливать пакеты MSIX. Вы можете начать с загрузки [последней версии](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release) или [предварительных сборок](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-preview).

С MSIX Core разработчики и ИТ-специалисты, которым требуется поддержка пользователей в предыдущих версиях Windows, теперь могут использовать преимущества MSIX.

## <a name="what-is-msix-core"></a>Что такое MSIX Core?

MSIX Core позволяет устанавливать приложения MSIX в предыдущих версиях Windows при условии, что приложения уже созданы для работы с этими версиями Windows. MSIX Core построена для следующих версий Windows, которые в настоящее время не поддерживают MSIX:

* Windows 7 с пакетом обновления 1 (SP1);
* Windows 8.1
* Поддерживаемые в настоящее время Windows Server (с возможностями рабочего стола)
* Версии Windows 10 до 1709

MSIX Core разработана как для разработчиков, так и для ИТ-специалистов. Разработчики могут использовать библиотеку ядра MSIX, чтобы позволить существующим установщикам устанавливать свои Упакованные приложения MSIX в предыдущих версиях Windows, чтобы они могли создать только один пакет MSIX для всех пользователей Windows. ИТ-специалисты могут загрузить установщик MSIX Core.  Установщик MSIX Core позволяет установить в командной строке MSIX, а также возможность пользователям устанавливать пакеты MSIX простым щелчком.

## <a name="considerations-of-msix-core"></a>Рекомендации по MSIX Core

Цель MSIX Core заключается в том, чтобы включить установку, запрос и удаление упакованных приложений MSIX (которые уже работают с этими версиями Windows) и предоставить как можно более чистую установку. MSIX Core предоставляет подмножество функций собственного MSIX, функционирующих аналогично существующим типам установщика Win32.

* MSIX Core не предоставляет преимущества контейнеров собственных MSIX, а также не позволяет приложению, которое использует функции Windows 10 для работы в предыдущих версиях Windows.
* При использовании MSIX Core в ОС нижнего уровня [псевдонимы выполнения приложений](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-an-alias) будут работать только из **Win + R** , а не из командной строки или PowerShell.
* MSIX Core не поддерживает интеграцию Microsoft Store. Разработчикам, желающим публиковать свои приложения в магазине, можно воспользоваться документацией [здесь](/windows/uwp/publish/).

## <a name="get-started"></a>Начало работы

Чтобы развернуть пакет MSIX с помощью MSIX Core, необходимо сначала [обновить существующий манифест MSIX](support-msix-core.md). Затем можно [развернуть пакет MSIX с помощью MSIX Core](deploy-with-msix-core.md) (если имеется только пакет) или [создать пакет MSIX с MSIX Core из исходного кода](msixcore-clickonce-solution.md) (если у вас есть исходный код).