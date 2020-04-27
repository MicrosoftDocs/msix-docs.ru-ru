---
Description: В этом руководстве объясняется, как настроить решение Visual Studio для изменения, отладки и упаковки классических приложений.
title: Упаковка классического приложения с помощью исходного кода в Visual Studio
ms.date: 02/02/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 2c34ec830981e9d9907dba9ec4ea124f800cbc02
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2020
ms.locfileid: "80108438"
---
# <a name="set-up-your-desktop-application-for-msix-packaging-in-visual-studio"></a>Настройка классического приложения для упаковки с помощью MSIX в Visual Studio

Для создания пакета для классического приложения в Visual Studio можно использовать **Проект упаковки приложений Windows**. Затем вы можете опубликовать этот пакет в Microsoft Store или установить его на один или несколько компьютеров без публикации.

**Проект упаковки приложений Windows** доступен в приведенных ниже версиях Visual Studio. Для оптимальной работы рекомендуется использовать последнюю версию.

* Visual Studio 2019
* Visual Studio 2017 15.5 и более поздних версий

> [!IMPORTANT]
> **Проект упаковки приложений Windows** в Visual Studio поддерживается в Windows 10 версии 1607 и более поздних версиях. Его можно использовать только в проектах, предназначенных для юбилейного обновления Windows 10 (10.0; сборка 14393) или более поздней версии.

Ниже приведены некоторые другие действия, которые можно выполнить в проекте упаковки приложений Visual Studio.

:heavy_check_mark: Автоматическое создание визуальных ресурсов

:heavy_check_mark: Внесение изменений в манифест с помощью визуального конструктора

:heavy_check_mark: Создание пакета или набора с использованием мастера

:heavy_check_mark: (При публикации в Microsoft Store) Простое назначение приложению идентификатора на основе имени, уже зарезервированного в [Центре партнеров](https://partner.microsoft.com/dashboard).


## <a name="prepare-your-application"></a>Подготовьте свое приложение

Прежде чем приступить к созданию пакета для приложения, ознакомьтесь с этим руководством. [Prepare to package a desktop application](desktop-to-uwp-prepare.md) (Подготовка к упаковке классического приложения).

<a id="new-packaging-project"/>

## <a name="setup-the-windows-application-packaging-project-in-your-solution"></a>Установите Проект упаковки приложения Windows в свое решение.

1. В Visual Studio откройте решение, содержащее проект вашего классического приложения.

2. Добавьте **Проект упаковки приложения Windows** в свое решение.

   Вам не потребуется добавлять в него какой-либо код. Он используется только для создания пакета. Мы будем называть этот проект "проектом упаковки".

   ![Проект упаковки](images/packaging-project.png)

3. Задайте **целевую версию** проекта (любую), но не забудьте указать для параметра **Минимальная версия** значение **Юбилейное обновление Windows 10**.

   ![Диалоговое окно выбора версии упаковки](images/packaging-version.png)

4. В Обозревателе решений щелкните правой кнопкой мыши **Приложения** в проекте упаковки и выберите команду **Добавить ссылку**.

   ![Добавление ссылки на проект](images/add-project-reference.png)

5. Выберите пакет классического приложения и нажмите кнопку **ОК**.

   ![Проект классического приложения](images/reference-project.png)

   В пакет можно включить несколько классических приложений, однако, когда пользователи выбирают плитку с вашим приложением, запустить можно только одно из них. В узле **Приложения** щелкните правой кнопкой мыши приложение, которое пользователи должны запускать, выбирая плитку приложения, и выберите **Задать как точку входа**.

   ![Задать как точку входа](images/entry-point-set.png)

6. Соберите проект упаковки, чтобы убедиться, что ошибок нет. При возникновении ошибок откройте **Диспетчер конфигурации** и убедитесь, что проекты предназначены для той же платформы.

   ![Диспетчер конфигурации](images/config-manager.png)

7. Используйте мастер [Создание пакетов приложения](../package/packaging-uwp-apps.md), чтобы создать пакет или набор MSIX или файл .msixupload/.appxupload (для публикации в Store).


## <a name="next-steps"></a>Дальнейшие действия

**Упаковка классического приложения в Visual Studio**

См. статью [Package a desktop or UWP app in Visual Studio](../package/packaging-uwp-apps.md) (Упаковка классического приложения или приложения UWP в Visual Studio)

**Запуск, отладка и тестирование классических приложений**

См. [Запуск, отладка и тестирование упакованного классического приложения](desktop-to-uwp-debug.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

**Видео**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

**Call UWP APIs in desktop apps** (Вызов API UWP в классических приложениях)

См. [Улучшение классического приложения для Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**Улучшение классического приложения путем добавления проектов UWP и компоненты среды выполнения Windows**

См. в статье [Extend your desktop app with modern UWP components](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend) (Улучшение классических приложений с помощью современных компонентов UWP).

**Распространение приложения**

См. статью [Distribute your packaged desktop app](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute) (Распространение упакованного классического приложения)
