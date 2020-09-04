---
description: Это руководство содержит инструкции по созданию и развертыванию приложения MSIX с помощью Microsoft Endpoint Configuration Manager.
title: Microsoft Endpoint Configuration Manager
ms.date: 1/31/2020
ms.topic: article
keywords: Windows 10, UWP, mecm, ConfigMgr, приложение, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 51b681af964ad55822bb27768a17372ed3aebadf
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090612"
---
# <a name="microsoft-endpoint-configuration-manager"></a>Microsoft Endpoint Configuration Manager
[Microsoft Endpoint Configuration Manager](/configmgr/) можно использовать для создания пакета `Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)`, который может быть доставлен на клиентские устройства. Для развертывания приложения MSIX должны выполняться следующие условия:
1) Доступ к развертываемому пакету MSIX.
2) Коллекция устройств или пользователей, которая будет целевым объектом установки.

## <a name="deploying-msix-application"></a>Развертывание приложения MSIX
При развертывании пакета доставляемого приложения Windows с помощью консоли Microsoft Endpoint Configuration Manager, прежде всего необходимо указать путь в формате UNC к пакету MSIX. Сведения о создаваемом приложении извлекаются из метаданных, сохраненных в пакете приложения MSIX, а затем автоматически добавляются в свойства пакета приложения Windows.

|||
|-----|------|
| 1. Запись пакета MSIX из установщика или получение существующего приложения MSIX | [Создание пакета MSIX из классического установщика](../packaging-tool/create-app-package.md)  |
| 2. Запуск консоли Microsoft Endpoint Configuration Manager | [Консоль Microsoft Endpoint Configuration Manager](https://devicemanagement.microsoft.com) |
| 3. Создание бизнес-приложения | [Создание пакета приложения для Windows](/configmgr/apps/get-started/creating-windows-applications) |
| 4. Создание коллекции устройств или пользователей | [Создание коллекций](/configmgr/core/clients/manage/collections/create-collections) |
| 5. Развертывание бизнес-приложения | [Развертывание приложений с помощью ConfigMgr](/configmgr/apps/deploy-use/deploy-applications) |
