---
Description: Это руководство содержит инструкции по созданию и развертыванию приложения MSIX с помощью Microsoft Endpoint Configuration Manager.
title: Microsoft Endpoint Configuration Manager
ms.date: 1/31/2020
ms.topic: article
keywords: Windows 10, UWP, mecm, ConfigMgr, приложение, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: eb136e71cec242efebe9129d2c6c8bee5fa62a35
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074180"
---
# <a name="microsoft-endpoint-configuration-manager"></a>Microsoft Endpoint Configuration Manager
[Microsoft Endpoint Configuration Manager](https://docs.microsoft.com/configmgr/) можно использовать для создания пакета `Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)`, который может быть доставлен на клиентские устройства. Для развертывания приложения MSIX должны выполняться следующие условия:
1) Доступ к развертываемому пакету MSIX.
2) Коллекция устройств или пользователей, которая будет целевым объектом установки.

## <a name="deploying-msix-application"></a>Развертывание приложения MSIX
При развертывании пакета доставляемого приложения Windows с помощью консоли Microsoft Endpoint Configuration Manager, прежде всего необходимо указать путь в формате UNC к пакету MSIX. Сведения о создаваемом приложении извлекаются из метаданных, сохраненных в пакете приложения MSIX, а затем автоматически добавляются в свойства пакета приложения Windows.

|||
|-----|------|
| 1. Запись пакета MSIX из установщика или получение существующего приложения MSIX | [Создание пакета MSIX из классического установщика](../packaging-tool/create-app-package-msi-vm.md)  |
| 2. Запуск консоли Microsoft Endpoint Configuration Manager | [Консоль Microsoft Endpoint Configuration Manager](https://devicemanagement.microsoft.com) |
| 3. Создание бизнес-приложения | [Создание пакета приложения для Windows](https://docs.microsoft.com/configmgr/apps/get-started/creating-windows-applications) |
| 4. Создание коллекции устройств или пользователей | [Создание коллекций](https://docs.microsoft.com/configmgr/core/clients/manage/collections/create-collections) |
| 5. Развертывание бизнес-приложения | [Развертывание приложений с помощью ConfigMgr](https://docs.microsoft.com/configmgr/apps/deploy-use/deploy-applications) |
