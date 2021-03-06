---
description: Устранение проблем, препятствующих запуску настольного приложения в контейнере MSIX
title: Устранение проблем, препятствующих запуску настольного приложения в контейнере MSIX
ms.date: 05/14/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c6526587d94af34b6c5c7df9702f0ab19e64e095
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091262"
---
# <a name="get-started-with-package-support-framework"></a>Начало работы с платформой поддержки пакетов 

[Платформа поддержки пакетов](package-support-framework-overview.md) — это набор с открытым исходным кодом, который помогает применять исправления к существующему классическому приложению (без изменения кода), чтобы его можно было запускать в контейнере MSIX. Платформа поддержки пакетов помогает настроить приложения в соответствии с требованиями современных сред выполнения.

Эта статья содержит подробные сведения о каждом компоненте платформы поддержки пакетов и пошаговое руководство по его использованию.

## <a name="understand-what-is-inside-a-package-support-framework"></a>Узнайте, что входит в состав платформы поддержки пакета

Платформа поддержки пакетов содержит исполняемый файл, библиотеку DLL диспетчера среды выполнения и набор исправлений для среды выполнения.

![Платформа поддержки пакетов](images/package-support-framework.png)

Ниже приведен процесс. 
1. Создайте файл конфигурации, в котором будут указаны исправления, которые необходимо применить к приложению. 
1. Измените пакет, чтобы он указывал на исполняемый файл средства запуска для платформы поддержки пакетов (ПСФ).

Когда пользователи запускают приложение, запускается средство запуска платформы поддержки пакетов. это первый исполняемый файл. Он считывает файл конфигурации и внедряет исправления среды выполнения и библиотеку DLL диспетчера среды выполнения в процесс приложения. Диспетчер среды выполнения применяет исправление, когда это требуется для выполнения приложения в контейнере MSIX.

![Внедрение библиотеки DLL платформы поддержки пакетов](images/package-support-framework-2.png)

## <a name="step-1-identify-packaged-application-compatibility-issues"></a>Шаг 1. выявление проблем совместимости упакованных приложений

Сначала создайте пакет для своего приложения. Затем установите его, запустите его и следите за его поведением. Могут появиться сообщения об ошибке, которые помогут выявить проблемы с совместимостью. Вы также можете использовать [Монитор процесса](/sysinternals/downloads/procmon) для выявления проблем.  Распространенные проблемы связаны с допущениями приложений о правах доступа к рабочему каталогу и пути программы.

### <a name="using-process-monitor-to-identify-an-issue"></a>Использование монитора процессов для обнаружения проблемы

[Монитор процессов](/sysinternals/downloads/procmon) — это мощная служебная программа для наблюдения за файлами и операциями реестра приложения, а также их результатами.  Это может помочь в понимании проблем совместимости приложений.  После открытия монитора процессов добавьте фильтр (фильтр > фильтр...), чтобы включить только события из исполняемого файла приложения.

![Фильтр приложений ProcMon](images/procmon_app_filter.png)

Появится список событий. Для многих из этих событий слово **Success** будет отображаться в столбце **result** .

![События ProcMon](images/procmon_events.png)

При необходимости можно отфильтровать события, чтобы отображались только ошибки.

![ProcMon исключить успешное выполнение](images/procmon_exclude_success.png)

Если вы считаете, что произошла ошибка доступа к файловой системе, выполните поиск событий со сбоем, которые находятся в папке System32/SysWOW64 или в пути к файлу пакета. Фильтры также могут помочь здесь. Начало в нижней части этого списка и прокрутка вверх. Ошибки, которые появляются в нижней части этого списка, были недавно выполнены. Обратите особое внимание на ошибки, содержащие такие строки, как "отказано в доступе" и "путь/имя не найдено", и пропускайте те вещи, которые не выглядят как подозрительные. У [псфсампле](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) есть две проблемы. Эти проблемы можно увидеть в списке, который отображается на следующем рисунке.

![ProcMon Config.txt](images/procmon_config_txt.png)

В первой выпуске, которая появляется на этом изображении, приложению не удается прочитать файл "Config.txt", расположенный в пути "C:\Windows\SysWOW64". Маловероятно, что приложение пытается напрямую ссылаться на этот путь. Скорее всего, он пытается выполнить чтение из этого файла с помощью относительного пути, а по умолчанию system32/SysWOW64 — рабочий каталог приложения. Это предполагает, что приложение ожидает, что его текущий рабочий каталог устанавливается в любое место в пакете. Глядя на приложение appx, можно увидеть, что файл существует в том же каталоге, что и исполняемый объект.

![Config.txt приложений](images/psfsampleapp_config_txt.png)

Вторая ошибка отображается на следующем рисунке.

![Файл журнала ProcMon](images/procmon_logfile.png)

В этой ситуации приложению не удается записать файл журнала в путь пакета. Это предложит, что может помочь адресная привязка перенаправления файлов.

<a id="find"></a>

## <a name="step-2-find-a-runtime-fix"></a>Шаг 2. Поиск исправления среды выполнения

ПСФ содержит исправления среды выполнения, которые можно использовать прямо сейчас, например адресную привязку перенаправления файлов.

### <a name="file-redirection-fixup"></a>Адресная привязка перенаправления файлов

С помощью [адресной привязки перенаправления файлов](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) можно перенаправить попытки записи или чтения данных в каталоге, который недоступен из приложения, работающего в контейнере MSIX.

Например, если приложение записывает в файл журнала, который находится в том же каталоге, что и исполняемые приложения, можно использовать [адресную привязку перенаправления файлов](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) , чтобы создать этот файл журнала в другом расположении, например в хранилище данных локального приложения.

### <a name="runtime-fixes-from-the-community"></a>Исправления среды выполнения из сообщества

Обязательно ознакомьтесь с вкладами сообщества на нашу страницу [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) . Возможно, другие разработчики разрешили проблему, похожую на вашу, и предоставила исправление среды выполнения.

## <a name="step-3-apply-a-runtime-fix"></a>Шаг 3. Применение исправления среды выполнения

Можно применить существующее исправление среды выполнения с помощью нескольких простых средств из Windows SDK и выполнить следующие действия.

> [!div class="checklist"]
> * Создание папки макета пакета
> * Получение файлов платформы поддержки пакетов
> * Добавьте их в пакет
> * Изменение манифеста пакета
> * Создание файла конфигурации

Давайте проверим каждую задачу.

### <a name="create-the-package-layout-folder"></a>Создание папки макета пакета

Если у вас уже есть файл. msix (или. appx), его содержимое можно распаковать в папку макета, которая будет служить промежуточной областью для пакета. Это можно сделать из командной строки с помощью средства программе makeappx, основываясь на пути установки пакета SDK. здесь вы найдете makeappx.exe средство на компьютере с Windows 10: x86: C:\Program Files (x86) \Windows Kits\10\bin\x86\makeappx.exe x64: C:\Program Files (x86) \Windows Kits\10\bin\x64\makeappx.exe

```powershell
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Это позволит получить примерно следующий вид.

![Макет пакета](images/package_contents.png)

Если у вас нет файла. msix (или. appx), то можно создать папку пакета и файлы с нуля.

### <a name="get-the-package-support-framework-files"></a>Получение файлов платформы поддержки пакетов

Пакет NuGet для ПСФ можно получить с помощью автономного средства командной строки NuGet или с помощью Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Получение пакета с помощью программы командной строки

Установите программу командной строки NuGet из этого расположения: https://www.nuget.org/downloads . Затем в командной строке NuGet выполните следующую команду: 

```powershell
nuget install Microsoft.PackageSupportFramework
```

Кроме того, можно переименовать расширение пакета в ZIP-файл и распаковать его. Все необходимые файлы будут находиться в папке "переносить".

#### <a name="get-the-package-by-using-visual-studio"></a>Получение пакета с помощью Visual Studio

В Visual Studio щелкните правой кнопкой мыши узел решения или проекта и выберите одну из команд Управление пакетами NuGet.  Найдите **Microsoft. паккажесуппортфрамеворк** или **ПСФ** , чтобы найти пакет на NuGet.org. Затем установите его.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Добавление файлов платформы поддержки пакетов в пакет

Добавьте необходимые 32-разрядные и 64-битные ПСФ DLL и исполняемые файлы в каталог пакета. Руководствуйтесь следующей таблицей. Также необходимо включить все необходимые исправления среды выполнения. В нашем примере нам требуется исправление среды выполнения для перенаправления файлов.

| Исполняемый файл приложения — x64 | Исполняемый файл приложения — x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

Теперь содержимое пакета должно выглядеть примерно так:

![Двоичные файлы пакета](images/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Изменение манифеста пакета

Откройте манифест пакета в текстовом редакторе, а затем присвойте `Executable` атрибуту `Application` элемента имя исполняемого файла средства запуска ПСФ.  Если вы знакомы с архитектурой целевого приложения, выберите соответствующую версию, PSFLauncher32.exe или PSFLauncher64.exe.  В противном случае PSFLauncher32.exe будет работать во всех случаях.  Рассмотрим пример.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Создание файла конфигурации

Создайте имя файла ``config.json`` и сохраните его в корневую папку пакета. Измените объявленный идентификатор приложения config.jsв файле, чтобы он указывал на только что замененный исполняемый файл. С помощью набора знаний, полученного с помощью монитора процессов, можно также задать рабочий каталог, а также использовать адресную привязку перенаправления файлов для перенаправления операций чтения и записи в файлы журнала в папке с относительным пакетом "Псфсамплеапп".

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```

Ниже приведено руководство по config.jsсхеме.

| Array | ключ | Значение |
|-------|-----------|-------|
| веб-масштабированием; | идентификатор |  Используйте значение `Id` атрибута `Application` элемента в манифесте пакета. |
| веб-масштабированием; | исполняемый файл | Относительный путь пакета к исполняемому файлу, который необходимо запустить. В большинстве случаев это значение можно получить из файла манифеста пакета, прежде чем изменять его. Это значение `Executable` атрибута `Application` элемента. |
| веб-масштабированием; | workingDirectory | Используемых Путь относительно пакета, используемый в качестве рабочего каталога запускаемого приложения. Если это значение не задано, операционная система использует каталог в `System32` качестве рабочего каталога приложения. |
| процессы | исполняемый файл | В большинстве случаев это будет имя, указанное `executable` выше, с удаленным путем и расширением файла. |
| исправлений | Файл DLL. | Относительный путь пакета к адресной привязке,. msix/. appx для загрузки. |
| исправлений | config | Используемых Определяет, как ведет себя библиотека DLL адресной привязки. Точный формат этого значения зависит от способа исправления, так как каждая адресная привязка может интерпретировать этот «большой двоичный объект». |

`applications`Ключи, `processes` и `fixups` являются массивами. Это означает, что вы можете использовать config.jsв файле, чтобы указать более одного DLL приложения, процесса и адресной привязки.

### <a name="package-and-test-the-app"></a>Упаковка и тестирование приложения

Затем создайте пакет.

```powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Затем подпишите его.

```powershell
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Дополнительные сведения см. в статьях [Создание сертификата для подписи пакета](/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) и [Подписывание пакета с помощью средства SignTool](/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool) .

С помощью PowerShell установите пакет.

>[!NOTE]
> Не забудьте сначала удалить пакет.

```powershell
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

Запустите приложение и обратите внимание на поведение с применением исправления среды выполнения.  При необходимости повторите шаги диагностики и упаковки.

### <a name="check-whether-the-package-support-framework-is-running"></a>Проверьте, запущена ли платформа поддержки пакетов. 

Вы можете проверить, выполняется ли исправление среды выполнения. Для этого откройте **Диспетчер задач** и щелкните **Дополнительные сведения**. Найдите приложение, к которому применена платформа поддержки пакетов, и разверните сведения о приложении, чтобы веив более подробную информацию. Вы сможете увидеть, что платформа поддержки пакетов запущена. 

### <a name="use-the-trace-fixup"></a>Использование исправления трассировки

Альтернативный способ диагностики проблем совместимости упакованных приложений заключается в использовании исправления трассировки. Эта библиотека DLL входит в состав ПСФ и предоставляет подробное диагностическое представление о поведении приложения, аналогичное монитору процессов.  Он специально разработан для выявления проблем совместимости приложений.  Чтобы использовать исправление трассировки, добавьте в пакет библиотеку DLL, добавьте следующий фрагмент в config.js, а затем упакуйте и установите приложение.

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

По умолчанию исправление трассировки отфильтровывает сбои, которые могут считаться ожидаемыми.  Например, приложения могут попытаться безусловно удалить файл без проверки его существования, игнорируя результат. Это имеет следствие, что некоторые непредвиденные сбои могут быть отфильтрованы, поэтому в приведенном выше примере мы будем выбирать получение всех сбоев из функций файловой системы. Это делается потому, что перед попыткой чтения из файла Config.txt происходит сбой с сообщением «файл не найден». Это сбой, который часто наблюдается и обычно не предполагается непредвиденным. На практике лучше всего начать фильтрацию только до непредвиденных сбоев, а затем вернуться ко всем сбоям, если есть проблема, которую по-прежнему невозможно определить.

По умолчанию выходные данные исправления трассировки отправляются в подключенный отладчик. В этом примере мы не будем подключать отладчик и используем программу [DebugView](/sysinternals/downloads/debugview) из Sysinternals для просмотра ее выходных данных. После запуска приложения можно увидеть те же сбои, что и раньше, что указывает нам на те же исправления среды выполнения.

![Файл Трацешим не найден](images/traceshim_filenotfound.png)

![Отказано в доступе к Трацешим](images/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Отладка, расширение или создание исправления среды выполнения

Visual Studio можно использовать для отладки исправления среды выполнения, расширения среды выполнения или создания одной из них с нуля. Для успешного выполнения этих действий необходимо выполнить следующие действия.

> [!div class="checklist"]
> * Добавление проекта упаковки
> * Добавление проекта для исправления среды выполнения
> * Добавление проекта, запускающего исполняемый файл запуска ПСФ
> * Настройка проекта упаковки

По завершении решение будет выглядеть примерно так.

![Завершенное решение](images/runtime-fix-project-structure.png)

Рассмотрим каждый проект в этом примере.

| Project | Цель |
|-------|-----------|
| десктопаппликатионпаккаже | Этот проект основан на [проекте упаковки приложений Windows](../desktop/desktop-to-uwp-packaging-dot-net.md) и выводит пакет MSIX. |
| рунтимефикс | Это проект библиотеки C++ с динамической присоединением, который содержит одну или несколько функций замены, которые служат исправлением среды выполнения. |
| псфлаунчер | Это пустой проект на языке C++. Этот проект является местом для получения распространяемых файлов среды выполнения платформы поддержки пакетов. Он выводит исполняемый файл. Этот исполняемый файл является первым, который запускается при запуске решения. |
| винформсдесктопаппликатион | Этот проект содержит исходный код для классического приложения. |

Полный пример, содержащий все типы проектов, см. в разделе [псфсампле](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Давайте рассмотрим шаги по созданию и настройке каждого из этих проектов в решении.

### <a name="create-a-package-solution"></a>Создание решения пакета

Если у вас еще нет решения для классического приложения, создайте новое **пустое решение** в Visual Studio.

![Пустое решение](images/blank-solution.png)

Вам также может потребоваться добавить любые проекты приложений, которые у вас есть.

### <a name="add-a-packaging-project"></a>Добавление проекта упаковки

Если у вас еще нет **проекта упаковки приложений Windows**, создайте его и добавьте в решение.

![Шаблон проекта пакета](images/package-project-template.png)

Дополнительные сведения о проекте упаковки приложений Windows см. в статье [Упаковка приложения с помощью Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

В **Обозреватель решений**щелкните проект упаковки правой кнопкой мыши, выберите **изменить**, а затем добавьте в конец файла проекта:

```xml
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>Добавление проекта для исправления среды выполнения

Добавьте в решение проект **библиотеки динамической компоновки (DLL)** C++.

![Библиотека исправления среды выполнения](images/runtime-fix-library.png)

Щелкните правой кнопкой мыши этот проект и выберите пункт **Свойства**.

На страницах свойств найдите поле **стандарт языка C++** , а затем в раскрывающемся списке рядом с этим полем Выберите параметр **ISO C++ 17 Standard (/std: c++ 17)** .

![Параметр ISO 17](images/iso-option.png)

Щелкните правой кнопкой мыши этот проект, а затем в контекстном меню выберите пункт **Управление пакетами NuGet** . Убедитесь, что для параметра **источник пакета** задано значение **все** или **NuGet.org**.

Щелкните значок параметров рядом с этим полем.

Найдите пакет NuGet *ПСФ**, а затем установите его для этого проекта.

![пакет nuget](images/psf-package.png)

Если требуется выполнить отладку или расширение существующего исправления среды выполнения, добавьте файлы исправления среды выполнения, полученные с помощью инструкций, описанных в разделе [Поиск исправления среды выполнения](#find) этого руководства.

Если вы собираетесь создать новое исправление, не добавляйте его в проект еще раз. Мы поможем вам добавить нужные файлы в этот проект позже. Сейчас мы продолжим настройку решения.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Добавление проекта, запускающего исполняемый файл запуска ПСФ

Добавьте в решение **пустой проект проекта** C++.

![Пустой проект](images/blank-app.png)

Добавьте пакет NuGet **ПСФ** в этот проект с помощью тех же руководств, которые описаны в предыдущем разделе.

Откройте страницы свойств для проекта и на странице **Общие** параметры задайте для свойства **имя целевого объекта** значение ``PSFLauncher32`` или в ``PSFLauncher64`` зависимости от архитектуры приложения.

![Справочник по средству запуска ПСФ](images/shim-exe-reference.png)

Добавьте ссылку проекта в проект исправления среды выполнения в решении.

![Справочник по исправлению среды выполнения](images/reference-fix.png)

Щелкните ссылку правой кнопкой мыши, а затем в окне **Свойства** примените эти значения.

| Property (Свойство) | Значение |
|-------|-----------|
| Копировать локально | Верно |
| Копировать локальные вспомогательные сборки | Верно |
| Выходные данные ссылочной сборки | Верно |
| Компоновать зависимости библиотек | Нет |
| Связывание входных данных зависимостей библиотеки | Нет |

### <a name="configure-the-packaging-project"></a>Настройка проекта упаковки

В проекте упаковки щелкните правой кнопкой мыши папку **Приложения** и выберите **Добавить ссылку**.

![Добавление ссылки на проект](images/add-reference-packaging-project.png)

Выберите проект средства запуска ПСФ и проект приложения рабочего стола, а затем нажмите кнопку **ОК** .

![Проект классического приложения](images/package-project-references.png)

>[!NOTE]
> Если у вас нет исходного кода приложения, просто выберите проект ПСФ Launcher. Мы покажем, как ссылаться на исполняемый файл при создании файла конфигурации.

В узле **приложения** щелкните правой кнопкой мыши приложение ПСФ Launcher и выберите пункт **задать в качестве точки входа**.

![Задать как точку входа](images/set-startup-project.png)

Добавьте файл с именем ``config.json`` в проект упаковки, а затем скопируйте и вставьте следующий текст JSON в файл. Задайте для свойства **действие пакета** значение **содержимое**.

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "fixups": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```

Укажите значение для каждого ключа. Используйте эту таблицу в качестве справочника.

| Array | ключ | Значение |
|-------|-----------|-------|
| веб-масштабированием; | идентификатор |  Используйте значение `Id` атрибута `Application` элемента в манифесте пакета. |
| веб-масштабированием; | исполняемый файл | Относительный путь пакета к исполняемому файлу, который необходимо запустить. В большинстве случаев это значение можно получить из файла манифеста пакета, прежде чем изменять его. Это значение `Executable` атрибута `Application` элемента. |
| веб-масштабированием; | workingDirectory | Используемых Путь относительно пакета, используемый в качестве рабочего каталога запускаемого приложения. Если это значение не задано, операционная система использует каталог в `System32` качестве рабочего каталога приложения. |
| процессы | исполняемый файл | В большинстве случаев это будет имя, указанное `executable` выше, с удаленным путем и расширением файла. |
| исправлений | Файл DLL. | Относительный путь пакета к библиотеке DLL адресной привязки для загрузки. |
| исправлений | config | Используемых Определяет, как ведет себя библиотека DLL адресной привязки. Точный формат этого значения зависит от способа исправления, так как каждая адресная привязка может интерпретировать этот «большой двоичный объект». |

По завершении ``config.json`` файл будет выглядеть примерно так.

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`Ключи, `processes` и `fixups` являются массивами. Это означает, что вы можете использовать config.jsв файле, чтобы указать более одного DLL приложения, процесса и адресной привязки.

### <a name="debug-a-runtime-fix"></a>Отладка исправления среды выполнения

В Visual Studio нажмите клавишу F5, чтобы запустить отладчик.  Первое, что начинается, — это приложение ПСФ Launcher, которое, в свою очередь, запускает целевое классическое приложение.  Чтобы выполнить отладку целевого настольного приложения, необходимо вручную присоединиться к процессу настольного приложения, выбрав **отладка >присоединить к процессу**, а затем выбрав процесс приложения. Чтобы разрешить отладку приложения .NET с помощью встроенной библиотеки DLL с исправлением среды выполнения, выберите управляемые и машинные типы кода (Отладка в смешанном режиме).  

После настройки можно задать точки останова рядом с строками кода в коде приложения для настольных систем и в проекте исправления среды выполнения. Если у вас нет исходного кода для приложения, вы сможете устанавливать точки останова только рядом с строками кода в проекте исправления среды выполнения.

Поскольку отладка F5 запускает приложение, развертывая свободные файлы из пути к папке макета пакета, а не устанавливая из пакета. msix/. appx, папка макета обычно не имеет тех же ограничений безопасности, что и папка установленного пакета. В результате может быть невозможно воспроизвести ошибки отказа при доступе к пути пакета перед применением исправления среды выполнения.

Чтобы устранить эту ошибку, используйте развертывание пакета. msix/. appx вместо нажатия клавиши F5 для недостаточного развертывания файлов.  Чтобы создать файл пакета msix/. appx, используйте служебную программу [программе makeappx](/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) из Windows SDK, как описано выше. Или в среде Visual Studio щелкните правой кнопкой мыши узел проекта приложения и выберите пункт **магазин — > создать пакеты приложений**.

Другой проблемой в Visual Studio является отсутствие встроенной поддержки присоединения к любым дочерним процессам, запущенным отладчиком.   Это затрудняет отладку логики в пути запуска целевого приложения, который должен быть вручную присоединен Visual Studio после запуска.

Чтобы устранить эту ошибку, используйте отладчик, который поддерживает присоединение дочернего процесса.  Обратите внимание, что обычно невозможно присоединить JIT-отладчик к целевому приложению.  Это связано с тем, что большинство методов JIT подразумевает запуск отладчика вместо целевого приложения с помощью раздела реестра Имажефиликсекутионоптионс.  Это нарушает механизм дедемонстрации, используемый PSFLauncher.exe для внедрения FixupRuntime.dll в целевое приложение.  WinDbg, который входит в [средства отладки для Windows](/windows-hardware/drivers/debugger/index)и получен из [Windows SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk), поддерживает присоединение дочернего процесса.  Теперь он также поддерживает непосредственное [Запуск и отладку приложения UWP](/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Для отладки запуска целевого приложения в качестве дочернего процесса запустите ``WinDbg`` .

```powershell
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

В ``WinDbg`` командной строке включите дочернюю отладку и установите соответствующие точки останова.

```powershell
.childdbg 1
g
```

(выполнение до запуска и переноса целевого приложения в отладчик)

```powershell
sxe ld fixup.dll
g
```

(выполнить до загрузки библиотеки DLL адресной привязки)

```powershell
bp ...
```

>[!NOTE]
> [Плмдебуг](/windows-hardware/drivers/debugger/plmdebug) можно также использовать для подключения отладчика к приложению при запуске и также включен в [средства отладки для Windows](/windows-hardware/drivers/debugger/index).  Однако использование более сложно, чем прямая поддержка, предоставляемая WinDbg.

## <a name="support"></a>Поддержка

Есть вопросы? Попросите нас обратиться в конференцию по [платформе поддержки пакетов](https://techcommunity.microsoft.com/t5/Package-Support-Framework/bd-p/Package-Support) на сайте технического сообщества MSIX.
