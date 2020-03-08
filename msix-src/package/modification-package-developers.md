---
title: Создание приложения с помощью пакета изменений
description: В этой статье описывается, как пакеты изменений будут работать со своими приложениями, если они упакованы в формат MSIX.
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 91eacb13777804c63c62a0b3eedd9c39d1e67530
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073990"
---
# <a name="building-an-app-with-a-modification-package"></a>Создание приложения с помощью пакета изменений 
Пакеты изменений позволяют ИТ-специалистам настраивать приложения MSIX без повторной упаковки приложения. Таким образом, когда ИТ-специалист развернет основное приложение и пакет модификации, а пользователь запустит это приложение, сохраненные в пакете изменения будут наложены поверх основного приложения так, что пользователь будет воспринимать их как единое приложение. Это связано с тем, что пакеты изменений выполняются внутри контейнера MSIX основного приложения и доступны только через основное приложение. Роль пакетов изменений могут выполнять любые подключаемые модули, надстройки, файлы и разделы реестра. Это следует помнить при разработке приложений, особенно для предприятий. 

Используя модель основного приложения и пакетов изменений, разработчик может предоставить корпоративному клиенту последние и лучшие версии сразу после обновления, без повторной упаковки приложения. Это становится возможным, поскольку все изменения хранятся в отдельном пакете, который развертывается и обслуживается независимо от приложения. 
