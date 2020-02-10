---
title: Создание пакета с помощью интерфейса командной строки
description: В этой статье описывается, как создать пакет MSIX с помощью интерфейса командной строки для средства упаковки MSIX.
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 726d743106932f7febac35ad70256b867146101d
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073720"
---
# <a name="conversion-with-the-command-line"></a>Преобразование с помощью командной строки

## <a name="automate-conversion-of-windows-installers-to-msix-packages-using-scripts"></a>Автоматизация преобразования установщиков Windows в пакеты MSIX с помощью сценариев

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Получить средство упаковки MSIX</a></p></div>

Средство упаковки MSIX поддерживает интерфейс командной строки для создания пакетов приложений MSIX. Это позволяет автоматизировать процесс повторной упаковки установщиков приложений и выполнение групповых преобразований.

Примеры скриптов Bash и PowerShell, демонстрирующих способы автоматизации процесса упаковки, подписывания, администрирования и распространения пакетов MSIX см. в папке [scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) набора средств MSIX.

## <a name="use-the-command-line-with-the-command-prompt"></a>Использование командной строки с командной строкой

Чтобы создать новый пакет MSIX для приложения, выполните команду `MsixPackagingTool.exe create-package` в окне командной строки администратора.

Ниже приведены параметры, которые можно передать в качестве аргументов командной строки:

|**Параметр** |    **Описание**|
|---------|---------|
|-? --help  |Отображает справочные сведения.|
|--template | [Обязательно] Путь к XML-файлу с шаблоном преобразования, который содержит сведения о пакете и настройки преобразования.|
|--virtualMachinePassword   | [Необязательно] Пароль виртуальной машины, которая будет применяться в среде преобразования. Примечания. файл шаблона должен содержать элемент VirtualMachine, а атрибуту Settings:: Алловпромптфорпассворд не должно быть присвоено значение true.|
|-v --verbose   |[Необязательно] Вывод подробных журналов в консоль.|

Примеры

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> Преобразование App-V 5.x сейчас можно выполнять с помощью командной строки. При этом поддерживаются некоторые возможности.

Вы можете [создать файл шаблона командной строки](generate-template-file.md) с помощью средства упаковки MSIX, прополнив процесс преобразования с помощью приложения или создав его на основе нашего примера шаблона.
