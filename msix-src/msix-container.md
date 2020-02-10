---
title: Контейнер MSIX
description: В этой статье описывается контейнер MSIX
ms.date: 02/04/2020
ms.topic: article
author: dianmsft
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 760491c0cbfe31bb47e5deb201e43d885101b631
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073490"
---
# <a name="msix-container"></a>Контейнер MSIX

Приложения, Упакованные с помощью MSIX, выполняются в контейнере упрощенных приложений. Процесс приложения MSIX и его дочерние процессы выполняются внутри контейнера и изолируются с помощью файловой системы и виртуализации реестра. Все приложения MSIX могут читать глобальный реестр. Приложение MSIX записывает в свой собственный виртуальный реестр и папку данных приложения, и эти данные будут удалены при удалении или сбросе приложения. Другие приложения не имеют доступа к виртуальному реестру или виртуальной файловой системе приложения MSIX.

Дополнительные сведения см. в статьях [понимание того, как работают пакеты MSIX в Windows](desktop/desktop-to-uwp-behind-the-scenes.md). 
