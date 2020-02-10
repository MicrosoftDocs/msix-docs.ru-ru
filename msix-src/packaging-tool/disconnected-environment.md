---
title: Использование средства упаковки MSIX в отключенной среде
description: В этой статье описывается, как получить все ресурсы, необходимые для средства упаковки MSIX, если они находятся в отключенной среде.
ms.date: 02/05/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 588d6925194bc11cb9d475f229df2227705e751a
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073500"
---
# <a name="using-the-msix-packaging-tool-in-a-disconnected-environment"></a>Использование средства упаковки MSIX в отключенной среде

В то же время, чтобы пользователи могли получить средство упаковки MSIX с помощью Microsoft Store, мы понимаем, что не все используют магазин или подключенную среду, в которой требуется выполнить преобразование. Эта версия доступна только для наших общедоступных выпусков, а не для выпусков [программы предварительной оценки](insider-program.md) .

## <a name="get-the-msix-packaging-tool"></a>Скачать средство упаковки MSIX

Средство упаковки MSIX также можно скачать с [веб-портала](https://businessstore.microsoft.com/store) Microsoft Store для бизнеса, чтобы использовать на предприятии в автономном режиме. См. подробнее об [автономном распространении](https://docs.microsoft.com/microsoft-store/distribute-offline-apps).

Если возникают проблемы с автономной копией средства упаковки, убедитесь, что у вас есть [автономная копия лицензии](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app) для этого средства. 

После получения автономной версии приложения можно добавить на компьютер пакет приложения и лицензию с помощью [PowerShell](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps).

#### <a name="example-of-offline-installation"></a>Пример автономной установки
```
PS C:\> Add-AppxProvisionedPackage -Path C:\offline -PackagePath C:\MSIX\MyPackage.msix -LicensePath C:\MSIX\MyLicense.xml
```

## <a name="get-the-msix-packaging-tool-driver"></a>Получение драйвера средства упаковки MSIX

Драйвер средства упаковки MSIX поставляется как пакет по [запросу (FOD)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) от Центр обновления Windows и не может быть установлен, если служба центр обновления Windows отключена на компьютере или параметры кольца Microsoft Windows для рейсов не соответствуют сборке ОС компьютера.

Специалисты организаций, в которых используются службы Windows Server Update Services (WSUS), должны выполнить определенные действия для установки драйвера вручную:
- Скачайте файл FOD. cab для [Windows 10 версии 1809, x64](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) или [Windows 10, версия 1809, x86](https://download.microsoft.com/download/9/9/4/9948d09d-af25-45a5-b01f-cc4bcf05f5bf/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)
- Скачайте файл FOD. cab для [Windows 10, версия 1903, x64](https://download.microsoft.com/download/5/2/e/52ec35e9-3b50-47b2-879d-c815a93bc3fc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) или [Windows 10, версия 1903, Примечание x86](https://download.microsoft.com/download/2/c/3/2c3a78a2-4d64-426a-976d-dfe4805110cc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab) **. Это также будет работать для Windows 10, версия 1909**
- Полученные отдельно пакеты с компонентами по запросу можно установить с помощью [параметров командной строки системы DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). В окне PowerShell с повышенными привилегиями введите следующее: ```Dism /Online /add-package /packagepath:(path)```

ИТ-администраторы также могут создать [параллельное хранилище компонентов (общую папку)](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server), чтобы обеспечить доступ к компоненту по запросу с драйвером средства упаковки MSIX. Подробные сведения можно найти внизу этой [записи блога](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

В противном случае, если у вас есть доступ к каналам сбыта Enterprise или OEM, вы можете получить драйвер на носителе с компонентами Windows 10 по запросу из таких источников:

- [Служба корпоративного лицензирования Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): требуется доступ к корпоративной лицензии.
- [Портал поставщика вычислительной техники](https://www.microsoftoem.com): требуется доступ к изготовителю оборудования.
- [Скачать MSDN](https://my.visualstudio.com/Downloads/Featured): требуется подписка MSDN.

Полученные отдельно пакеты с компонентами по запросу можно установить с помощью параметров командной строки системы DISM.
