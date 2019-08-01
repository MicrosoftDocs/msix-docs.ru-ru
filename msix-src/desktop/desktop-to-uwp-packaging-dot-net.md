---
Description: В этом руководство объясняется, как настроить решение Visual Studio для редактирования, отладки и упаковки классических приложений.
title: Упаковка классического приложения с помощью исходного кода в Visual Studio
ms.date: 07/18/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 8effa64d5b06739d1251423fc0776e3e010b73e2
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68685421"
---
# <a name="package-a-desktop-app-from-source-code-using-visual-studio"></a>Упаковка классического приложения с помощью исходного кода в Visual Studio

Вы можете использовать проект **упаковки приложений Windows** в Visual Studio для создания пакета для классического приложения. Затем можно опубликовать этот пакет на Microsoft Store или загружать неопубликованные его на один или несколько компьютеров.

Проект **упаковки приложений Windows** доступен в следующих версиях Visual Studio. Для оптимальной работы рекомендуется использовать последний выпуск.

* Visual Studio 2019
* Visual Studio 2017 15,5 и более поздние версии

> [!IMPORTANT]
> Проект **упаковки приложений Windows** в Visual Studio поддерживается в Windows 10, версии 1607 и более поздних версиях. Его можно использовать только в проектах, предназначенных для годовщины обновления Windows 10 (10,0; Сборка 14393) или более поздней версии.

## <a name="prepare-your-application"></a>Подготовка приложения

Прежде чем приступить к созданию пакета для приложения, ознакомьтесь с этим руководством. [Подготовка к упаковке классического приложения](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Создание пакета

1. В Visual Studio откройте решение, содержащее проект вашего классического приложения.

2. Добавьте **Проект упаковки приложения для Windows** в свое решение.

   Вам не потребуется добавлять в него какой-либо код. Он используется только для создания пакета. Мы будем называть этот проект "проектом упаковки".

   ![Проект упаковки](images/packaging-project.png)

3. Задайте **целевую версию** проекта (любую), но не забудьте указать для параметра **Минимальная версия** значение **Юбилейное обновление Windows 10**.

   ![Диалоговое окно выбора версии упаковки](images/packaging-version.png)

4. В обозреватель решений щелкните правой кнопкой мыши папку **приложения** в проекте упаковки и выберите команду **Добавить ссылку**.

   ![Добавление ссылки на проект](images/add-project-reference.png)

5. Выберите пакет классического приложения и нажмите кнопку **ОК** .

   ![Проект классического приложения](images/reference-project.png)

   Можно включить несколько классических приложений в пакет, но когда пользователи выбирают плитку с вашим приложением, запустить можно только одно из них. В узле **Приложения** щелкните правой кнопкой мыши приложение, которое пользователи должны запускать, выбирая плитку приложения, и выберите **Задать в качестве точки входа**.

   ![Задание точки входа](images/entry-point-set.png)

6. Если упакованное приложение предназначено для .NET Core 3, выполните следующие действия, чтобы добавить новый целевой объект сборки в файл проекта. Это необходимо только для приложений, предназначенных для .NET Core 3.  

    1. В обозреватель решений щелкните правой кнопкой мыши узел проект упаковки и выберите пункт **изменить файл проекта**.

    2. Добавьте следующий XML-код в файл проекта непосредственно перед закрывающим `</Project>` элементом.

        ``` xml
        <!-- Stomp the path to application executable. This task will copy the main exe to the appx root folder. -->
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
          <ItemGroup>
            <!-- Stomp all "SourceProject" values for all incoming dependencies to flatten the package. -->
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
              <!-- Blank the SourceProject here to vend all files into the root of the package. -->
              <SourceProject></SourceProject>
            </_FilteredNonWapProjProjectOutput>
          </ItemGroup>
        </Target>
        ```

    3. Сохраните файл проекта и закройте его.

7. Соберите проект упаковки, чтобы убедиться, что ошибок нет. При возникновении ошибок откройте **Configuration Manager** и убедитесь, что проекты предназначены для той же платформы.

   ![Диспетчер конфигурации](images/config-manager.png)

8. Используйте мастер [создания пакетов приложений](../package/packaging-uwp-apps.md) , чтобы создать файл. мсиксуплоад/. appxupload.

   Этот файл можно передать непосредственно в хранилище.

**Видео**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Следующие шаги

**Поиск ответов на вопросы**

Есть вопросы? Задайте их на Stack Overflow. Наша команда следит за этими [тегами](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Вы также можете задать нам вопросы [здесь](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Отправьте отзыв или получите предложения по функциям**

См. раздел [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Запуск, отладка и тестирование приложения для настольных систем**

См. раздел [Запуск, отладка и тестирование приложения в упакованном классическом приложении](desktop-to-uwp-debug.md) .

**Расширение классического приложения путем добавления API UWP**

См. [Улучшение классического приложения для Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**Расширение классического приложения путем добавления проектов UWP и компонентов среда выполнения Windows**

См. [Расширение классического приложения с помощью современных компонентов UWP](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)

**Распространение приложения**

См. раздел [распространение упакованного](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute) классического приложения.
