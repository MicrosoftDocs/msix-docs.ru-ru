---
Description: В этой статье содержатся сведения о вариантах обновления приложения MSIX.
title: Разностные обновления пакетов приложений MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, развертывание, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 80258b0b63b16ed02ac12a0b103ac1a4c8949c8e
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091242"
---
# <a name="differential-updates-for-msix-app-packages"></a>Разностные обновления для пакетов приложений MSIX

## <a name="understanding-msix-app-package-updates"></a>Общие сведения об обновлении пакета приложения MSIX
При создании пакета приложения MSIX создается файл манифеста с подробными сведениями о файлах, включенных в этот пакет. При создании пакета создается часть метаданных и сохраняется в пакете с расширением MSIX или MSIXBUNDLE, что позволяет Windows однозначно идентифицировать части пакета. Затем при обновлении Windows может использовать файл метаданных для сравнения старого пакета c новым и определения частей, которые нужно скачать на устройство. Так как метаданные позволяют однозначно идентифицировать части пакета, это означает, что механизм выборочного обновления с любой версии пакета до любой другой версии пакета полностью работает (предполагается, что у исходного пакета более ранняя версия, чем у целевого).

Все заключается в файле AppxBlockMap.xml (упомянутые выше метаданные). Файл AppxBlockMap.xml представляет собой документ XML, содержащий двумерный список сведений о файлах в пакете. В первом измерении размещаются общие сведения о файле (например, имя и размер), а во втором — хэш-код SHA2-256 каждого среза (размером 64 КБ) этого файла ("блок").

Первый хэш-код представляет первый блок (64 КБ) файла, а второй хэш-код — остальные 35 КБ с учетом того, что размер этого файла составляет 101188 байта.

Если при обновлении вносятся изменения во второй блок этого файла, хэш также будет обновлен соответствующим образом. Компонент скачивания распознает изменения, извлекает второй блок и повторно использует первый блок без изменений из старого пакета.

Кроме того, если не изменился весь файл (что определяется по отсутствию изменений во всем наборе блоков), то этот файл можно и далее использовать из существующего пакета, что позволяет значительно сэкономить пользователям Windows 10.

### <a name="upgrading-to-newer-versions"></a>Обновление до более новых версий
При установке более новой версии пакета приложения MSIX происходит сравнение файла манифеста и определение измененных блоков файла. При обновлении пакета приложения MSIX до более новой версии извлекаются только изменяемые файлы, чтобы уменьшить потребление пропускной способности, если обновляемые приложения находятся в общей сетевой папке или вне сети организации.

### <a name="upgrading-to-older-versions"></a>Переход на более старые версии
При установке более старой версии пакета приложения MSIX происходит сравнение файла манифеста и определение измененных блоков файлов. При переводе пакета приложения MSIX на более старую версию извлекаются только изменяемые файлы, чтобы уменьшить потребление пропускной способности, если обновляемые приложения находятся в общей сетевой папке или вне сети организации.

## <a name="optimizing-upgrade-experiences"></a>Оптимизация обновления
Процесс установки пакета приложения MSIX на устройстве можно настроить, чтобы улучшить работу для пользователя. При развертывании приложения на устройстве установщик можно настроить обновлять приложение, когда его закроет пользователь, или принудительно закрывать и обновлять приложение.

### <a name="powershell"></a>PowerShell
При установке пакета приложения MSIX на устройстве с помощью PowerShell используется командлет [add-appxpackage](/powershell-msix-cmdlets.md). Этот командлет содержит следующие параметры, которые изменяют процесс установки или обновления пакета приложения MSIX.

|||
|-|-|
| -DeferRegistrationWhenPackagesAreInUse | Указывает, что этот командлет будет предотвращать обновление пакета приложения MSIX, пока пользователь не закроет приложение. |
| -ForceApplicationShutdown | Указывает, что этот командлет принудительно завершает работу всех активных процессов, связанных с пакетом или его зависимостями. |
| -ForceUpdateFromAnyVersion | Указывает, что пакет приложения MSIX принудительно сохранит в промежуточном хранилище или зарегистрирует определенную версию пакета, независимо от того, размещена или зарегистрирована ли уже более поздняя версия. |
| -InstallAllResources | Указывает, что командлет принудительно выполняет развертывание всех пакетов ресурсов, указанных в аргументе объединенного пакета. Этот параметр переопределяет проверку применимости ресурсов для подсистемы развертывания и принудительно сохраняет в промежуточном хранилище все пакеты ресурсов. |
| -RetainFilesOnFailure | Если для этого параметра задано значение true, в случае сбоя развертывания файлы, созданные на целевом компьютере во время установки, не удаляются. |
| -Update | Указывает, что добавляемый пакет является обновлением пакета зависимостей. Пакет зависимостей удаляется при удалении родительского приложения. Если этот параметр не указан, пакет не будет удален при удалении родительского приложения. |

Полный список параметров, доступных для этого командлета, см. в статье для PowerShell [add-appxpackage](/powershell/module/appx/add-appxpackage?view=win10-ps).