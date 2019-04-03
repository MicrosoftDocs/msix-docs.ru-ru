---
title: Заметки о выпуске MSIX SDK версии 1.5
description: Заметки о выпуске для пакета SDK MSIX версии 1.5
author: c-don
ms.author: cdon
ms.date: 02/06/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: f6b6350c6d55b0d330d73c4588d4c9c50fb151e1
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900496"
---
# <a name="msix-sdk-15-update"></a>Обновление MSIX SDK 1.5

С выпуском пакета SDK (1,5) основной инвестициях был при уменьшении размера двоичного пакета SDK. Один из ключей отзыв о нашего пакета SDK был размер и особенно для мобильных приложений, зависимые двоичный, обойдется примерно 5 МБ не категории "Удовлетворительно". 

Чтобы устранить эту проблему, мы заменили статический двоичные зависимости для синтаксического анализа XML с двоичными файлами папки "Входящие" на Android и iOS, Mac OS. Как часть уменьшения размера двоичный мы также добавили параметры для других функциональных возможностей в сценарии сборки, на которые входит в состав пакета SDK. Только добавляя функциональность, необходимую вашему приложению в двоичный файл, размер двоичных MSIX SDK дальнейшем может быть уменьшен. 

Вы можете получить последнюю версию пакета SDK на сайте GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.5" data-linktype="external">Пакет SDK MSIX на GitHub</a></p></div>

