---
title: Создание файла Установщика приложений вручную
description: В этой статье описывается, как установить связанный набор с помощью установщика приложений, включая создание файла *. appinstaller, определяющего связанный набор.
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, установщик приложений, AppInstaller, загрузка неопубликованных приложений, связанный набор, дополнительные пакеты
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 2e7e3752fbdf7ba16a6cd6ba0b6fae3aee41eacd
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328298"
---
# <a name="create-an-app-installer-file-manually"></a>Создание файла Установщика приложений вручную

В этой статье показано, как вручную создать файл установщика приложения, который определяет [связанный набор](install-related-set.md). Связанный набор — это не один объект, а сочетание основного и дополнительных пакетов. 

Чтобы получить возможность установить связанный набор как один объект, необходимо указать основной и дополнительные пакеты как один объект. Для этого необходимо создать XML-файл с расширением **appinstaller** , чтобы определить связанный набор. Установщик приложения использует файл **appinstaller** и позволяет пользователю установить все определенные пакеты одним щелчком. 

## <a name="app-installer-file-example"></a>Пример файла установщика приложения

Прежде чем мы перейдем к более подробной информации, ниже приведен полный пример файла msixbundle *. appinstaller:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix"
            ProcessorArchitecture="x64" />

    </OptionalPackages>
    
    <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0"/>   
  </UpdateSettings>

</AppInstaller>
```

При развертывании файл Установщика приложений сверяется с пакетами приложений, на которые ссылается элемент `Uri`. таким образом, значения `Name`, `Publisher` и `Version` должны соответствовать элементу [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) в манифесте пакета приложения.

## <a name="how-to-create-an-app-installer-file"></a>Инструкции по созданию файла Установщика приложений.

Для распределения связанного набора как одного объекта необходимо создать файл Установщика приложений, содержащий элементы, которые требуются соответствующей [схеме Установщика приложений](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

### <a name="step-1-create-the-appinstaller-file"></a>Шаг 1. Создание файла *.appinstaller
Используя текстовый редактор, создайте файл (который будет содержать XML) и назовите его &lt;имя_файла&gt;.appinstaller

### <a name="step-2-add-the-basic-template"></a>Шаг 2. Добавление базового шаблона
Базовый шаблон включает в себя сведения о файле Установщика приложений.
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>Шаг 3. Добавление сведений об основном пакете

Если основным пакетом приложения является файл. msix или. appx, используйте `<MainPackage>`, как показано ниже. Обязательно включите ProcessorArchitecture, так как он обязателен для пакетов, не относящихся к пакету.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainPackage
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        ProcessorArchitecture="x64"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msix" />

</AppInstaller>
```

Если основным пакетом приложения является msixbundle или appxbundle или файл, используйте `<MainBundle>` вместо `<MainPackage>`, как показано ниже. Для пакетов ProcessorArchitecture не требуется.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

</AppInstaller>
```

Сведения в атрибуте `<MainBundle>` или `<MainPackage>` должны совпадать с элементом [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) в манифесте пакета приложений или пакета приложения соответственно.

### <a name="step-4-add-the-optional-packages"></a>Шаг 4. Добавление дополнительных пакетов
Как и атрибут основного пакета приложений, если дополнительный пакет может являться пакетом приложения или пакетом приложений, дочерний элемент с атрибутом `<OptionalPackages>` должен являться `<Package>` или `<Bundle>`. Сведения о пакете в дочерних элементах должны соответствовать элементу идентификатора в манифеста пакета приложений или пакета приложения.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x64"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>Шаг 5. Добавление зависимостей
В элементе зависимостей можно указать требуемые пакеты платформы для основного пакета или дополнительных пакетов.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>Шаг 6. Добавление параметра обновления

В файле Установщика приложений можно также указать параметр обновления, чтобы связанные наборы автоматически обновлялись при публикации нового файла Установщика приложений. **<UpdateSettings>** является необязательным элементом. В **<UpdateSettings>** параметр OnLaunch указывает, что проверку обновлений следует выполнять при запуске приложения, а значение HoursBetweenUpdateChecks="12" указывает, что проверять обновления нужно каждые 12 часов. Если параметр HoursBetweenUpdateChecks не задан, интервал проверки обновлений по умолчанию — 24 часа. Дополнительные типы обновлений, например фоновые обновления, можно найти в [схеме](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings)параметров обновления. Дополнительные типы обновлений для запуска, такие как обновления с запросом, можно найти в [схеме](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-onlaunch) OnLaunch.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

Полные сведения о схеме XML см. в разделе [Справочник по файлу Установщика приложений](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> Тип файла установщика приложений является новым в Windows 10, версия 1709 (Windows 10 Creators Update). Развертывание приложений Windows 10 с помощью файла установщика приложения в предыдущих версиях Windows 10 не поддерживается. Элемент **хаурсбетвинупдатечеккс** доступен начиная с Windows 10, версия 1803.
