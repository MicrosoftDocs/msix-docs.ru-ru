---
title: Преобразование установщика с помощью служб
description: В этой статье объясняется, как преобразовать существующий установщик со службами в MSIX с помощью средства упаковки MSIX.
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, MSIX, средство упаковки MSIX, службы
ms.localizationpriority: medium
ms.openlocfilehash: f5306be1c4be846c5e3d0be6fe55e83a61c3220c
ms.sourcegitcommit: 1c4e671172104bba39eebd513d849cfbbb689539
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100630855"
---
# <a name="convert-an-installer-that-includes-services"></a>Преобразование установщика, который содержит службы

Windows 10, версия 2004, предоставляет поддержку для запуска пакета MSIX, который включает в себя службы. С помощью средства упаковки MSIX можно получить существующий установщик со службами и преобразовать его в MSIX. Эта поддержка относится к выпуску [MSIXing Tool](tool-overview.md)(1.2019.1220.0) за январь 2020. Получив пакет MSIX со службой, ему потребуются права администратора для установки на компьютере.

## <a name="instructions"></a>Инструкции

Чтобы преобразовать установщик, включающий службы, используйте средство упаковки MSIX, как и для любого [пакета приложения](create-app-package.md). Выберите установщик со службами, и вы увидите страницу отчета **службы** перед последним шагом для создания пакета MSIX.

На странице отчета **службы** перечислены службы, обнаруженные в установщике во время преобразования. Службы, которые имеют всю необходимую информацию и поддерживаются, будут показаны в **прилагаемой** таблице. В **исключенной** таблице будут показаны службы, для которых требуется дополнительная информация, требуется исправление или не поддерживается.

Чтобы исправить службу или просмотреть дополнительные данные о службе, дважды щелкните запись службы в таблице, чтобы просмотреть всплывающее окно с дополнительными сведениями о службе. Некоторые из этих сведений можно изменить при необходимости.

- **Имя ключа:** Имя службы. Это недоступно для редактирования.
- **Описание:** Описание записи службы.
- **Отображаемое имя:** Отображаемое имя службы.
- **Путь к изображению:** Расположение исполняемого файла службы. Это недоступно для редактирования.
- **Начальная учетная запись:** Начальная учетная запись для службы.
- **Тип запуска:** Тип запуска службы. Поддерживает **Автоматические**, **Ручные**, **Отключенные** и **отложенные**.
- **Аргументы:** Аргументы, которые будут запускаться при запуске службы.
- **Зависимости:** Зависимости для службы.

После того как служба будет исправлена, ее можно переместить в **включенную** таблицу или оставить в **исключенной** таблице, если ее не нужно использовать в окончательном пакете. Затем можно перейти к последнему шагу, чтобы создать пакет MSIX.

## <a name="known-limitations"></a>Известные ограничения

Путь к исполняемому файлу служб (также называемый путем к изображению) в настоящее время недоступен для редактирования. Чтобы устранить проблемы с путем, необходимо вручную изменить путь к исполняемому файлу службы перед преобразованием установщика. Кроме того, после преобразования можно изменить манифест вручную с помощью [редактора пакетов](package-editor.md) в средстве упаковки MSIX.

В настоящее время отчет о службах недоступен в **редакторе пакетов**. Необходимо вручную изменить манифест, чтобы внести изменения в службы, входящие в пакет MSIX.

В настоящее время не поддерживаются службы с зависимостями вне пакета.

## <a name="add-a-service-manually-using-your-manifest"></a>Добавление службы вручную с помощью манифеста

При добавлении службы в приложение вручную необходимо [Добавить службу](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-service) в манифест приложения. Для этого требуется [ограниченная возможность](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) добавления в приложение.
