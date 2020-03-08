---
description: В этой статье приводятся рекомендации по регистрации макета пакета из сетевой папки.
title: Регистрация макета пакета из сетевой папки
ms.date: 02/06/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 08a21a15d5e9929d7f7b01e0865ee90da45f8a7c
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074190"
---
# <a name="registering-a-package-layout-from-a-network-share"></a>Регистрация макета пакета из сетевой папки

Совместная работа и разработка нескольких устройств могут занять много времени из-за необходимости копировать пакеты, репозитории, папки сборки и т. д. При разработке в Windows 10 версии 1709 и более поздних версиях вы можете воспользоваться преимуществами функции для создания макета пакета в сетевой папке, а затем зарегистрировать макет на удаленном устройстве непосредственно из сети.

Несколько человек могут внести свой вклад в компоновку одного пакета приложений в сетевой папке, и изменения будут видны другим сотрудникам и пользователям, зарегистрировавшим приложение. Если вы создаете приложение для нескольких устройств, вы можете воспользоваться преимуществами этой функции и не копировать приложение на каждое устройство для тестирования.

## <a name="prerequisites"></a>Предварительные условия

1. Устройства должны иметь сборку 14965 для программы предварительной оценки Windows 10 Creators или более поздней версии.

2. Вам потребуется включить режим разработчика и обнаружение устройств на всех устройствах.

3. Участники совместной работы должны иметь доступ к папке сборки для чтения и записи.

4. А пользователи — только доступ для чтения.

## <a name="in-visual-studio"></a>В Visual Studio

При разработке в Visual Studio можно выполнить действия, описанные [здесь](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps?redirectedfrom=MSDN#advanced-remote-deployment-options).

## <a name="from-the-command-line"></a>В командной строке

При разработке вне Visual Studio и использовании средства командной строки можно использовать [WinDeployAppCmd](https://docs.microsoft.com/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool). Ниже приведен пример выполнения в окне командной строки:

```
WinAppDeployCmd.exe registerfiles -remotedeploydir <network path> -ip <IP Address> -pin <target machine PIN>
```
- Сетевой путь — путь к свободным файлам приложения.

- IP-адрес — введите здесь IP-адрес конечного компьютера.

- PIN-код — если он необходим для установления подключения с целевым устройством. (Вам будет предложено повторить попытку с параметром -pin, если потребуется проверка подлинности.) Щелкните здесь, чтобы узнать, как получить PIN-код.
 

Вы можете также создать полностью упакованное приложение, которое обращается к файлам в сетевой папке во время тестирования и проверки. Пример см. [здесь](https://github.com/AppInstaller/Windows-appsample-marble-maze).