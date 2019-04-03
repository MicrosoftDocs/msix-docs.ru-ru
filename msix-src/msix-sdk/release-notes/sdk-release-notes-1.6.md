---
title: Заметки о выпуске пакета SDK 1.6 MSIX выпуске
description: Заметки о выпуске для пакета SDK MSIX выпуска 1.6
author: c-don
ms.author: cdon
ms.date: 02/06/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b964c8942d3da85d85cbff87f78ef8c9399ab142
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900326"
---
# <a name="msix-sdk-16-update"></a>Обновление пакета SDK 1.6 MSIX

С выпуском пакета SDK (1.6) мы слышали отзывы от наших партнеров и добавлены дополнительные API-интерфейсы для предоставления разработчикам дополнительные возможности и гибкость в обработке MSIX пакетов. 

## <a name="utf8-api-variants"></a>UTF8 Варианты API

В этом выпуске пакета SDK мы добавим около 14 новых вариантов UTF8 API для существующих вызовов API. При включении этих новых интерфейсов API разработчики могут выбрать использовать вариант Utf8 для обработки в соответствии с их среды и платформ. С помощью API-интерфейсы AppxPackaging, вызывающий объект несет ответственность за освобождение памяти, используемой LPSTR * выходные параметры.

Ниже приведены новые интерфейсы UTF8.
- IAppxBlockMapFileUtf8
- IAppxBlockMapReaderUtf8
- IAppxBundleManifestPackageInfoUtf8
- IAppxBundleReaderUtf8
- IAppxFactoryUtf8
- IAppxFileUtf8
- IAppxManifestApplicationUtf8
- IAppxManifestPackageDependencyUtf8
- IAppxManifestPackageIdUtf8
- IAppxManifestPropertiesUtf8
- IAppxManifestQualifiedResourceUtf8
- IAppxManifestResourcesEnumeratorUtf8
- IAppxManifestTargetDeviceFamilyUtf8
- IAppxPackageReaderUtf8


## <a name="override-language-selection"></a>Переопределить выбор языка 

По умолчанию, при обработке наборов приложений пакет SDK MSIX Возвращает языковой пакет, который применяется, выбрав язык, который также устанавливается по умолчанию в системе. Этот API позволяет приложению для перечисления пакетов, которые доступны и переопределить языковой пакет, который будет возвращаться при обработке наборов приложений. 

## <a name="other-updates-and-improvements"></a>Другие обновления и улучшения

В этом обновлении 
- Обновление зависимости lib OpenSSL для 1.0.2q
- Исправлена, как мы обрабатываем международные символы 

Вы можете получить последнюю версию пакета SDK на сайте GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.6" data-linktype="external">Пакет SDK MSIX на GitHub</a></p></div>

