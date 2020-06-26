---
Description: Процедура ручной упаковки классического приложения Windows (например, Win32, WPF и Windows Forms) для Windows 10.
title: Упаковка приложения вручную (мост для классических приложений)
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0345e15ad014afb9461eccc6ed112550a3c0c7a8
ms.sourcegitcommit: e3a06eccd3322053b8b498cb6343fb6f711a7a0b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724546"
---
# <a name="generating-msix-package-components"></a>Создание компонентов пакета MSIX

В этой статье показано, как создавать компоненты пакета MSIX для упаковки приложения с помощью программ командной строки (без использования Visual Studio или средства формирования пакетов MSIX).

Чтобы упаковать приложение вручную, необходимо создать файл манифеста пакета, добавить компоненты пакета, а затем запустить программу командной строки **MakeAppx.exe** для создания пакета MSIX.

## <a name="first-prepare-to-package"></a>Сначала подготовьте приложения к упаковке

Если вы еще не сделали этого, ознакомьтесь со статьей [What to know before packaging your desktop application](../desktop/before-packaging-overview.md) (Что необходимо знать перед упаковкой классического приложения).

## <a name="create-a-package-manifest"></a>Создайте манифест пакета

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

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Заполните элементы уровня пакета для файла

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
> Если вы зарезервировали имя приложения в Microsoft Store, значения для "Name" (Имя) и "Publisher" (Издатель) можно получить с помощью [Центра партнеров](https://partner.microsoft.com/dashboard). Если планируется, что приложение будет загружено на другие системы в неопубликованном виде, вы можете указать для этих атрибутов собственные имена при условии, что имя издателя совпадает с именем, указанным в сертификате, который используется для подписания приложения.

### <a name="properties"></a>Свойства

Элемент [Properties](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) имеет три обязательных дочерних элемента. Вот пример узла **Properties** с замещающим текстом для элементов. **DisplayName** — это имя приложения, которое вы резервируете в Store для отправляемых в этот Store приложений.

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

Для классических приложений, для которых вы создаете пакет, всегда задавайте для атрибута ``Name`` значение ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Характеристики
Для классических приложений, для которых вы создаете пакет, необходимо добавить возможность ``runFullTrust``.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Заполните элементы уровня приложения

Заполните этот шаблон сведениями о вашем приложении.

### <a name="application-element"></a>Элемент приложения

Для классических приложений, для которых вы создаете пакет, атрибут ``EntryPoint`` элемента "Application" всегда имеет значение ``Windows.FullTrustApplication``.

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
<a id="target-based-assets"></a>

## <a name="optional-add-target-based-unplated-assets"></a>Добавление специальных ресурсов без покрытия (необязательно)

Специальные ресурсы предназначены для значков и плиток, которые отображаются на панели задач Windows, в представлении задач, в окне, вызываемом сочетанием клавиш ALT+TAB, в Snap Assist и в правом нижнем углу плиток начального экрана. Подробнее об этом см. [здесь](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos#unplated-assets).

1. Получите правильные изображения 44x44 и скопируйте их в папку с изображениями (например, "Ресурсы").

2. Создайте копию каждого изображения 44x44 в той же папке и добавьте **.targetsize-44_altform-unplated** к имени файла. Вам потребуются две копии каждого значка, каждый из которых должен быть назван особым образом. Например, после завершения процесса папка ресурсов может содержать файлы **MYAPP_44x44.png** и **MYAPP_44x44.targetsize 44_altform unplated.png**.

   > [!NOTE]
   > В этом примере значок **MYAPP_44x44.png** — это значок, который вы укажете в атрибуте логотипа ``Square44x44Logo`` вашего пакета MSIX.

3. В файле манифеста задайте ``BackgroundColor`` для каждого значка, который должен быть прозрачным.

4. Перейдите к следующему подразделу для создания нового файла индекса ресурсов пакета.

<a id="make-pri"></a>

### <a name="generate-a-package-resource-index-pri-file-using-makepri"></a>Создание файла индекса ресурсов пакета (PRI) с помощью MakePri

Для создания специальных ресурсов, как описано в приведенном выше разделе, или изменения каких-либо визуальных ресурсов приложения после создания пакета, необходимо создать новый файл PRI.

В зависимости от пути установки пакета SDK **MakePri.exe** может находиться в следующих расположениях на компьютере с Windows 10:
- x86: C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x86\makepri.exe
- X64: C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x64\makepri.exe

Версия ARM этого средства отсутствует.

1.  Откройте командную строку или окно PowerShell.

2.  Смените каталог на корневую папку пакета и создайте файл priconfig.xml с помощью команды ``<path>\makepri.exe createconfig /cf priconfig.xml /dq en-US``.

5.  Создайте файлы resources.pri с помощью команды ``<path>\makepri.exe new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Например, команда для вашего приложения может выглядеть так: ``<path>\makepri.exe new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  Упакуйте приложение, руководствуясь инструкциями в следующем шаге.

<a id="make-appx"></a>

## <a name="test-your-application-before-packaging"></a>Протестируйте приложение перед упаковкой

Вы можете развернуть неупакованное приложение и протестировать его перед упаковкой или подписанием. Для этого запустите нижеприведенную команду из окна PowerShell. Обязательно передайте файл манифеста вашего приложения, расположенный в корне каталога вашего пакета, со всеми другими вашими компонентами пакета.

```Add-AppxPackage –Register AppxManifest.xml```

Как только это будет выполнено, ваше приложение необходимо развернуть в системе. Перед упаковкой вы можете протестировать его, чтобы удостовериться, что все работает. Чтобы обновить файлы .exe или .dll приложения, замените существующие файлы в пакете новыми, повысьте номер версии в AppxManifest.xml, а затем выполните указанную выше команду повторно.

## <a name="package-your-components-into-an-msix"></a>Упакуйте ваши компоненты в MSIX

Следующий шаг. Используйте **MakeAppx.exe** для создания MSIX-пакета для вашего приложения. Makeappx.exe входит в пакет Windows 10 SDK, и если у вас установлена Visual Studio, доступ к нему можно получить через командную строку разработчика Visual Studio.

См. [Create an MSIX package or bundle with MakeAppx.exe](../package/create-app-package-with-makeappx-tool.md) (Создание пакета MSIX или набора с помощью программы MakeAppx.exe)



> [!NOTE]
> Упакованное приложение всегда выполняется от лица текущего пользователя, а диск, на который производится установка приложения, должен быть отформатирован в NTFS.


