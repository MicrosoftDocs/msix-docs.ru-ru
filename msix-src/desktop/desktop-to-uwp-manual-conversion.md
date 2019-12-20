---
Description: Процедура ручной упаковки классического приложения для Windows (например, Win32, WPF и Windows Forms) для Windows 10.
title: Упаковка приложения вручную (мост для настольных компьютеров)
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 04b92d8b8665fb75f2484e5e0e1bebcf01e93d22
ms.sourcegitcommit: 0412ba69187ce791c16313d0109a5d896141d44c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303300"
---
# <a name="package-a-desktop-app-manually"></a>Упаковка классического приложения вручную

В этой статье показано, как упаковать приложение без использования таких средств, как Visual Studio или средство упаковки MSIX.

Чтобы вручную упаковать приложение, создайте файл манифеста пакета, а затем запустите программу командной строки **программе makeappx. exe** , чтобы создать пакет приложения Windows.

Если приложение устанавливается с помощью команды XCOPY или вы знакомы с изменениями, внесенными установщиком приложения в систему и требующими более детального контроля над процессом, рассмотрите возможность ручного упаковки.

Если вы не знаете точно, какие изменения вносит в систему ваш установщик, или если предпочитаете использовать автоматические средства для создания пакета манифеста, попробуйте воспользоваться любым из [этих](desktop-to-uwp-root.md#convert) параметров.

> [!IMPORTANT]
> Возможность создания пакета приложения Windows для настольного приложения (также называемого мостом рабочего стола) появилась в Windows 10 версии 1607, и ее можно использовать только в проектах, предназначенных для годовщины обновления Windows 10 (10,0; Сборка 14393) или более поздняя версия в Visual Studio.

## <a name="first-prepare-your-application"></a>Сначала подготовьте свое приложение

Прежде чем приступить к созданию пакета для приложения, ознакомьтесь с этим руководством. [Подготовка к упаковке](desktop-to-uwp-prepare.md)классического приложения.

## <a name="create-a-package-manifest"></a>Создание пакета манифеста

Создайте файл, назовите его **appxmanifest.xml**, а затем добавьте в него XML-код, представленный ниже.

Это базовый шаблон, который содержит элементы и атрибуты, требуемые для вашего пакета. Мы добавим для них значения в следующем разделе.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Заполнение элементов уровня пакета для файла

Заполните этот шаблон сведениями о вашем пакете.

### <a name="identity-information"></a>Сведения об удостоверении

Вот пример элемента **Identity** с замещающим текстом для атрибутов. Вы можете задать атрибуту ``ProcessorArchitecture`` значение ``x64`` или ``x86``.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Если вы зарезервированы имя приложения в Microsoft Store, вы можете получить имя и издателя с помощью [центра партнеров](https://partner.microsoft.com/dashboard). Если вы планируете загружать неопубликованные приложение на другие системы, вы можете указать собственные имена, пока имя издателя совпадает с именем сертификата, используемого для подписания приложения.

### <a name="properties"></a>"Свойства"

Элемент [Properties](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) имеет три обязательных дочерних элемента. Вот пример узла **Properties** с замещающим текстом для элементов. **DisplayName** — это имя приложения, которое вы резервируете в хранилище, для приложений, которые передаются в хранилище.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>Ресурсы

Вот пример узла [Resources](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources).

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>Зависимости

Для классических приложений, для которых создается пакет, всегда присвойте атрибуту ``Name`` значение ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Возможности
Для классических приложений, для которых создается пакет, необходимо добавить функцию ``runFullTrust``.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Заполните элементы уровня приложения

Заполните этот шаблон сведениями о вашем приложении.

### <a name="application-element"></a>Элемент приложения

Для классических приложений, для которых создается пакет, атрибут ``EntryPoint`` элемента Application всегда ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>Визуальные элементы

Вот пример узла [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>Добавление специальных ресурсов без покрытия (необязательно)

Специальные ресурсы предназначены для значков и плиток, которые отображаются на панели задач Windows, в представлении задач, в окне, вызываемом сочетанием клавиш ALT+TAB, в Snap Assist и в правом нижнем углу плиток начального экрана. Подробнее об этом см. [здесь](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos#unplated-assets).

1. Получите правильные изображения 44x44 и скопируйте их в папку с изображениями (например, "Ресурсы").

2. Создайте копию каждого изображения 44x44 в той же папке и добавьте **.targetsize-44_altform-unplated** к имени файла. Вам потребуются две копии каждого значка, каждый из которых должен быть назван особым образом. Например, после завершения процесса папка ресурсов может содержать файлы **MYAPP_44x44.png** и **MYAPP_44x44.targetsize 44_altform unplated.png**.

   > [!NOTE]
   > В этом примере значок **MYAPP_44x44.png** — это значок, который вы укажете в атрибуте логотипа ``Square44x44Logo`` вашего пакета приложения для Windows.

3.  В пакете приложения для Windows задайте ``BackgroundColor`` для каждого значка, который должен быть прозрачным.

4. Перейдите к следующему подразделу для создания нового файла индекса ресурсов пакета.

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>Создание файла индекса ресурсов пакета (PRI)

Если вы создаете ресурсы на основе целевого объекта, как описано в приведенном выше разделе, или изменяете любой из визуальных ресурсов приложения после создания пакета, необходимо создать новый PRI-файл.

1.  Откройте **командную строку разработчика для VS 2017**.

    ![командная строка разработчика](images/developer-command-prompt.png)

2.  Смените каталог на корневую папку пакета и создайте файл priconfig.xml с помощью команды ``makepri createconfig /cf priconfig.xml /dq en-US``.

5.  Создайте файлы resources.pri с помощью команды ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Например, команда для приложения может выглядеть следующим образом: ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  Запакуйте приложение для Windows, руководствуясь инструкциями в следующем шаге.

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>Создание пакета приложений для Windows

Используйте **MakeAppx.exe**, чтобы создать пакет приложения для Windows для вашего проекта. Он входит в пакет Windows 10 SDK, и если у вас установлена Visual Studio, доступ к нему можно получить через командную строку разработчика для вашей версии Visual Studio.

См. раздел [Создание пакета приложения с помощью средства MakeAppx.exe](../package/create-app-package-with-makeappx-tool.md)

## <a name="run-the-packaged-app"></a>Запуск упакованного приложения

Приложение можно запустить для локального тестирования без получения сертификата и его подписания. Просто запустите этот командлет PowerShell:

```Add-AppxPackage –Register AppxManifest.xml```

Чтобы обновить файлы .exe или .dll приложения, замените существующие файлы в пакете новыми, повысьте номер версии в AppxManifest.xml, а затем выполните указанную выше команду повторно.

> [!NOTE]
> Упакованное приложение всегда запускается как интерактивный пользователь, а любой диск, на котором устанавливается упакованное приложение, должен быть отформатирован в формате NTFS.

## <a name="next-steps"></a>Дальнейшие действия

**Поиск ответов на вопросы**

Есть вопросы? Задайте их на Stack Overflow. Наша команда отслеживает эти [теги](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Вы также можете задать нам вопрос [здесь](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Пошаговое прохождение кода, Поиск и устранение проблем**

См. раздел [Запуск, отладка и тестирование приложения в упакованном классическом приложении](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-debug) .

**Подпишите приложение, а затем распространите его**

См. раздел [распространение упакованного](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute) классического приложения.
