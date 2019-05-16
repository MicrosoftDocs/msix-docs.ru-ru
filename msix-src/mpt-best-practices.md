---
title: Советы и рекомендации для средства упаковки MSIX
description: Советы и рекомендации для средства упаковки MSIX
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 419ea7ab48d9199629570d58efc2bc5ff85ebc05
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900436"
---
# <a name="best-practices-for-msix-packaging-tool"></a>Советы и рекомендации для средства упаковки MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Получить средство упаковки MSIX</a></p></div>

В этой статье приведены рекомендации по повторной упаковке приложения, чтобы MSIX и с помощью средства упаковки MSIX.

## <a name="best-practices-during-setup"></a>Рекомендации по во время установки
 
Начнем с того необходимо иметь версию средства упаковки MSIX для Windows 10 октября 2018 с обновлением (также известный как версия 1809). Для процесса преобразования существует ряд других моментов, которые мы рекомендуем вам перед началом. 

- Мы понимаем, что не каждый является в Windows 10 октября 2018 с обновлением или даже Windows 10. Таким образом рекомендуется создать предварительно настроенного минимальной версии поддержки MSIX чистой виртуальной Машине. 

- Еще одна причина, что это рекомендация — что во время интерактивного преобразования графического пользовательского интерфейса с помощью средства упаковки MSIX, мы будет вести прослушивание на все, что на устройстве и она поможет предотвратить лишние данные в пакете. 

- Его также полезно знать, какие зависимости, у вас, чтобы понять, какие из них следует запускать с вашим приложением, а какие должны быть упакованы как пакет изменения. Например при наличии зависимости среды выполнения, рекомендуется включать в главное приложение. При наличии подключаемому модулю, должны упаковать как пакет связанные изменения. 

---
## Best practices during repackaging 
'When you are using the MSIX Packaging Tool, there are a few things that we also recommend you do as best practice':
  - 'When packaging ClickOnce installers, it is necessary to send a shortcut to desktop if the installer is not doing so already. In general, it is good practice to always remember to send a shortcut to desktop for the main app executable.'
  - 'When creating modification packages, you need to declare the package Name (identity name) of the parent application in the tool UI so that the tool sets the correct package dependency in the manifest of the modification package.'
  - Declaring an installation location field in the **Package information** page is optional but recommended. Make sure that this path matches the installation location of application installer.
  - Performing the preparation steps in the **Prepare computer** page is optional but highly recommended.
ms.custom: RS5
---

## <a name="best-practices-while-bundling-msix-packages"></a>Рекомендации при объединении MSIX пакетов

Мы рекомендуем внедрить MSIX пакетов при наличии разных установщики для различных процессорных архитектурах, например x86, x64 и ARM для вашего приложения. Путем объединения установщиков друг с другом, вы можете разрешить ОС Windows 10, чтобы определить применимость соответствующего установщика вместо пользователю не придется сделать правильный выбор. 

При объединении MSIX пакетов, необходимо будет уже преобразования собственных установщиков Win32 в MSIX пакетов. 

- Средство упаковки MSIX предполагает архитектуру процессора в версии ОС Windows 10, что преобразование выполняется. Например, если вы являетесь в x64, версия Windows 10 и при преобразовании x86 версию программы установки, полученный пакет MSIX будет x64. 
- Если вы планируете пакеты MSIX наборов, необходимо будет для преобразования собственных установщиков в той же среде, где вы планируете их развертывание. Таким образом, средства упаковки MSIX создаст соответствующий пакет MSIX для этой среды. 


