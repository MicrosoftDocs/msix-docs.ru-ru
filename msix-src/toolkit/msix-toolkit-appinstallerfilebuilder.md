---
title: Средство AppInstaller File Builder
description: В этой статье содержатся сведения о средстве AppInstaller File Builder в наборе средств MSIX.
ms.date: 1/23/2020
ms.topic: article
keywords: Windows 10, msix, msix Toolkit
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: eaf2234423ccc20cb2bb5f1024a7f8144722f1c1
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073560"
---
# <a name="appinstaller-file-builder-tool"></a>Средство AppInstaller File Builder

[Средство AppInstaller File Builder](https://github.com/microsoft/MSIX-Toolkit/tree/master/AppInstallerFileBuilder) в наборе MSIX Toolkit можно использовать для настройки приложения MSIX Package для возврата в центральный источник для обновлений. Это средство помогает в создании файла AppInstaller (. AppInstaller). Файл AppInstaller можно использовать с различными [вариантами развертывания](../desktop/managing-your-msix-deployment-overview.md).

## <a name="create-an-appinstaller-file"></a>Создание файла AppInstaller

1. Скачать [AppInstaller File Builder](https://github.com/microsoft/MSIX-Toolkit/releases/download/1.3.3/AppInstallerFileBuilder_1.2019.1001.0.msix)
2. Установите и запустите приложение **AppInstaller File Builder** .
3. Нажмите кнопку **получить сведения о пакете** и перейдите к приложению MSIX, которое вы хотите установить с помощью файла AppInstaller.
4. Укажите параметры обновления, которые должен иметь пакет приложения.
    > [!Note]
    > Параметры могут различаться в зависимости от выбранной минимальной версии операционной системы. Для просмотра или настройки этих значений необходимо включить **проверку наличия обновлений для запуска приложения** .
5. Продолжайте работу с мастером, добавляя [Дополнительные пакеты](../package/optional-packages.md), [пакеты изменений](//modification-packages.md), [связанные пакеты](../package/optional-packages.md) и зависимости по мере необходимости.
6. Нажмите кнопку **создать** после просмотра сводных данных, чтобы создать файл AppInstaller.
