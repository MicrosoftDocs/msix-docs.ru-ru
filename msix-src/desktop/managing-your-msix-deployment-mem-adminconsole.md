---
Description: Это руководство содержит инструкции по созданию и развертыванию приложения MSIX с помощью консоли администрирования Microsoft Endpoint Manager
title: Консоль администрирования Microsoft Endpoint Manager
ms.date: 06/17/2019
ms.topic: article
keywords: Windows 10, UWP, MEM, MEM AC, консоль администрирования MEM, приложение
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f0ed835c3eb94dda358fbf88fe587bcb44277af1
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2020
ms.locfileid: "77073920"
---
# <a name="microsoft-endpoint-manager-admin-console"></a>Консоль администрирования Microsoft Endpoint Manager
[Консоль администрирования Microsoft Endpoint Manager](https://devicemanagement.microsoft.com) можно использовать для развертывания пакета MSIX на клиентских устройствах. Чтобы развернуть приложение MSIX с помощью консоли администрирования Microsoft Endpoint Configuration Manager, вам потребуется само приложение MSIX для развертывания и группа Azure Active Directory в качестве целевого объекта для установки.

## <a name="deploying-msix-application"></a>Развертывание приложения MSIX
При развертывании распространяемого приложения с помощью консоли администрирования Microsoft Endpoint Manager прежде всего необходимо указать локальный или сетевой путь к пакету MSIX. Когда создается приложение в консоли администрирования Microsoft Endpoint Manager, сначала извлекаются метаданные из пакета приложения MSIX, а затем эти данные автоматически загружаются в свойства приложения.

|||
|-----|------|
| 1. Запись пакета MSIX из установщика или получение существующего приложения MSIX | [Создание пакета MSIX из классического установщика](../packaging-tool/create-app-package-msi-vm.md)  |
| 2. Запуск консоли администрирования Microsoft Endpoint Manager | [Консоль администрирования Microsoft Endpoint Manager](https://devicemanagement.microsoft.com) |
| 3. Создание и развертывание бизнес-приложения | [Создание бизнес-приложения](https://docs.microsoft.com/intune/apps/lob-apps-windows) |
