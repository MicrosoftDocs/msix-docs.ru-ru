---
title: MSIX Core
description: В этой статье представлен обзор MSIX Core, в котором поддерживается MSIX поддержка Windows 7 с пакетом обновления 1 (SP1), Windows 8.1, поддерживаемых в настоящее время Windows Server (с возможностями рабочего стола) и версий Windows 10 до 1709 (годовщина обновления).
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, мсикскоре, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 38285c6fd3cd5e47a2f7570163ef85302d8b9fb7
ms.sourcegitcommit: 0412ba69187ce791c16313d0109a5d896141d44c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303394"
---
# <a name="msix-core"></a>MSIX Core

MSIX Core обеспечивает поддержку MSIX для более ранних версий Windows, чем Windows 10, версии 1709. MSIX Core — это [проект с открытым исходным кодом](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore) в GitHub, который позволяет этим предыдущим версиям Windows устанавливать пакеты MSIX. Вы можете начать с загрузки [последней версии](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release) или [предварительных сборок](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-preview).

С MSIX Core разработчики и ИТ-специалисты, которым требуется поддержка пользователей в предыдущих версиях Windows, теперь могут использовать преимущества MSIX.

## <a name="what-is-msix-core"></a>Что такое MSIX Core?

MSIX Core позволяет устанавливать приложения MSIX в предыдущих версиях Windows при условии, что приложения уже созданы для работы с этими версиями Windows. MSIX Core построена для следующих версий Windows, которые в настоящее время не поддерживают MSIX:

* Windows 7 с пакетом обновления 1 (SP1)
* Windows 8.1
* Поддерживаемые в настоящее время Windows Server (с возможностями рабочего стола)
* Версии Windows 10 до 1709

MSIX Core разработана как для разработчиков, так и для ИТ-специалистов. Разработчики могут использовать библиотеку ядра MSIX, чтобы позволить существующим установщикам устанавливать свои Упакованные приложения MSIX в предыдущих версиях Windows, чтобы они могли создать только один пакет MSIX для всех пользователей Windows. ИТ-специалисты могут загрузить установщик MSIX Core.  Установщик MSIX Core позволяет установить в командной строке MSIX, а также возможность пользователям устанавливать пакеты MSIX простым щелчком.

## <a name="considerations-of-msix-core"></a>Рекомендации по MSIX Core

Цель MSIX Core заключается в том, чтобы включить установку, запрос и удаление упакованных приложений MSIX (которые уже работают с этими версиями Windows) и предоставить как можно более чистую установку. MSIX Core предоставляет подмножество функций собственного MSIX, функционирующих аналогично существующим типам установщика Win32. MSIX Core не предоставляет преимущества контейнеров собственных MSIX, а также не позволяет приложению, которое использует функции Windows 10 для работы в предыдущих версиях Windows.

MSIX Core также не будет поддерживать интеграцию Microsoft Store. Разработчики, желающие опубликовать свои приложения на Microsoft Store, могут следовать [этой документации.](https://docs.microsoft.com/windows/uwp/publish/)

## <a name="get-started"></a>Начало работы

Чтобы развернуть пакет MSIX с помощью MSIX Core, необходимо сначала [обновить существующий манифест MSIX](https://docs.microsoft.com/windows/msix/msix-core/support-msix-core). Затем можно [развернуть пакет MSIX с помощью MSIX Core](https://docs.microsoft.com/windows/msix/msix-core/deploy-with-msix-core) (если имеется только пакет) или [создать пакет MSIX с MSIX Core из исходного кода](https://docs.microsoft.com/windows/msix/msix-core/msixcore-clickonce-solution) (если у вас есть исходный код).