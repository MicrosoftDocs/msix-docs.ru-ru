---
Description: В этом руководстве описывается настройка решения Visual Studio для редактирования, отладки и упаковки классического приложения.
title: Упаковка классического приложения с помощью исходного кода в Visual Studio
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: bf157014fd43a8fdfe3162a8b42e1f4b241d7938
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828927"
---
# <a name="package-a-desktop-app-from-source-code-using-visual-studio"></a>Упаковка классического приложения с помощью исходного кода в Visual Studio

Можно использовать **проект упаковки приложений Windows** проект в Visual Studio для создания пакета для вашего приложения. После этого вы публикуете, его в пакет для Microsoft Store или загрузки неопубликованных приложений на один или несколько компьютеров.

**Проект упаковки приложений Windows** проект доступен в следующих версиях Visual Studio. Для получения наилучших результатов мы рекомендуем использовать последний выпуск.

* Visual Studio 2019
* Visual Studio 2017 15.5 и более поздних версий

> [!IMPORTANT]
> **Проект упаковки приложений Windows** проект в Visual Studio поддерживается в Windows 10 версии 1607 и более поздних версий. Может использоваться только в проектах, предназначенных для Windows 10 Anniversary Update (10.0; Сборка 14393) или более поздней версии.

## <a name="first-prepare-your-application"></a>Сначала подготовьте свое приложение

Ознакомьтесь с этим руководством прежде чем начать создание пакета для приложения: [Подготовка для упаковки классического приложения](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Создание пакета

1. В Visual Studio откройте решение, содержащее проект вашего классического приложения.

2. Добавьте **Проект упаковки приложения для Windows** в свое решение.

   Вам не потребуется добавлять в него какой-либо код. Он используется только для создания пакета. Мы будем называть этот проект "проектом упаковки".

   ![Проект упаковки](images/packaging-project.png)

3. Задайте **целевую версию** проекта (любую), но не забудьте указать для параметра **Минимальная версия** значение **Юбилейное обновление Windows 10**.

   ![Диалоговое окно выбора версии упаковки](images/packaging-version.png)

4. В проекте упаковки щелкните правой кнопкой мыши папку **Приложения** и выберите **Добавить ссылку**.

   ![Добавление ссылки на проект](images/add-project-reference.png)

5. Выберите пакет классического приложения и нажмите кнопку **ОК** .

   ![Проект классического приложения](images/reference-project.png)

   Можно включить несколько классических приложений в пакет, но когда пользователи выбирают плитку с вашим приложением, запустить можно только одно из них. В узле **Приложения** щелкните правой кнопкой мыши приложение, которое пользователи должны запускать, выбирая плитку приложения, и выберите **Задать в качестве точки входа**.

   ![Задание точки входа](images/entry-point-set.png)

6. Соберите проект упаковки, чтобы убедиться, что ошибок нет.  Если возникнут ошибки, откройте **Configuration Manager** и убедитесь, что ваши проекты предназначены для той же платформе.

   ![Configuration manager](images/config-manager.png)

7. Используйте мастер [Создание пакетов приложений](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps), чтобы создать файл appxupload.

   Этот файл можно отправить непосредственно к Store.

**Видео**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Следующие шаги

**Найдите ответы на ваши вопросы**

Есть вопросы? Задайте их на Stack Overflow. Наша команда следит за этими [тегами](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Вы также можете задать нам вопросы [здесь](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Отправить отзыв или предложения по функциям**

См. раздел [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Запустите, отладку или тестирование своего настольного приложения**

См. в разделе [запуска, отладки и тестирования упакованные приложения рабочего стола](desktop-to-uwp-debug.md)

**Улучшить своего настольного приложения, добавив API-интерфейсов универсальной платформы Windows**

См. [Улучшение классического приложения для Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**Расширение своего настольного приложения путем добавления проектов универсальной платформы Windows и компонентов среды выполнения Windows**

См. [Расширение классического приложения с помощью современных компонентов UWP](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)

**Распространение приложения**

См. в разделе [распределить упакованные настольного приложения](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
