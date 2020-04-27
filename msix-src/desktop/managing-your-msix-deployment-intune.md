---
Description: Это руководство содержит инструкции по созданию и развертыванию приложения MSIX с помощью Microsoft Intune.
title: Microsoft Intune
ms.date: 06/17/2019
ms.topic: article
keywords: Windows 10, UWP, MEM, MEM AC, консоль администрирования MEM, приложение, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cf2fe754de71cd5242833c3bfd5b74f3c95b1a37
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2020
ms.locfileid: "77073930"
---
# <a name="microsoft-intune"></a>Microsoft Intune
Вы можете использовать [консоль Microsoft Intune](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) для создания бизнес-приложения, которое можно доставить на клиентские устройства. Чтобы развернуть приложение MSIX с помощью Microsoft Intune, вам потребуется само приложение MSIX для развертывания и группа Azure Active Directory в качестве целевого объекта для установки.

## <a name="deploying-an-msix-app"></a>Развертывание приложения MSIX
При развертывании клиентского приложения с помощью Microsoft Intune, прежде всего необходимо указать локальный или сетевой путь к приложению MSIX. При создании приложения в Microsoft Intune сначала извлекаются метаданные из пакета приложения MSIX, а затем эти данные автоматически загружаются в свойства приложения.

|||
|-----|------|
| 1. Запись пакета MSIX из установщика или получение существующего приложения MSIX | [Создание пакета MSIX из классического установщика](../packaging-tool/create-app-package.md) |
| 2. Откройте консоль Intune | [Консоль Microsoft Intune](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) |
| 3. Создание и развертывание бизнес-приложения | [Создание бизнес-приложения](https://docs.microsoft.com/intune/apps/lob-apps-windows) |
