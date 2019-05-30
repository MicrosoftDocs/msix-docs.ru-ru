---
title: С помощью пакета SDK MSIX распространять пакет MSIX на платформах Windows 10
description: Руководство для разработчиков, использующих пакет SDK для пакетов MSIX пакета для использования на нескольких платформах
author: mcleanbyron
ms.author: mcleans
ms.date: 12/13/2018
ms.topic: article
ms.custom: RS5
ms.openlocfilehash: 64a780bbc7ef4edc6e2d0c829c7304fdf39762f7
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795229"
---
# <a name="use-the-msix-sdk-to-distribute-an-msix-package-on-non-windows-10-platforms"></a>С помощью пакета SDK MSIX распространять пакет MSIX на платформах Windows 10

Пакет SDK MSIX предлагает разработчикам универсальный способ распространения содержимого пакета для клиентских устройств, независимо от платформы операционной системы на клиентском устройстве. Это позволяет разработчикам упаковать их содержимое приложения один раз вместо того, чтобы пакет для каждой платформы.

Чтобы воспользоваться преимуществами пакета SDK MSIX и возможность распределения содержимого пакета на множество платформ, мы предоставляем способ указания целевых платформ, место пакетов для извлечения. Это означает, что гарантирует, что содержимое пакета извлекаются из пакета только в том случае, при желании.

В следующей таблице показаны семейств устройств целевой для объявления в манифесте.

<table class="tg">
  <tr>
    <th class="tg-yw4l">Платформа</th>
    <th class="tg-yw4l">Семья</th>
    <th class="tg-yw4l" colspan="3">Семейство объекта устройства</th>
    <th class="tg-yw4l">Примечания</th>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="6">Windows 10</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-031e" rowspan="24"><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>Platform.All<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br></td>
    <td class="tg-baqh" rowspan="6">Зависимости Windows.Universal</td>
    <td class="tg-yw4l">Windows.Mobile</td>
    <td class="tg-yw4l">Мобильные устройства</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Настольный компьютер</td>
    <td class="tg-yw4l">Windows.Desktop</td>
    <td class="tg-yw4l">Компьютер</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xbox</td>
    <td class="tg-yw4l">Windows.Xbox</td>
    <td class="tg-yw4l">Консоль Xbox</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Surface Hub</td>
    <td class="tg-yw4l">Windows.Team</td>
    <td class="tg-yw4l">Устройства на большом экране Windows 10</td>
  </tr>
  <tr>
    <td class="tg-yw4l">HoloLens</td>
    <td class="tg-yw4l">Windows.Holographic</td>
    <td class="tg-yw4l">Гарнитура VR/AR</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Интернет вещей</td>
    <td class="tg-yw4l">Windows.IoT</td>
    <td class="tg-yw4l">Устройства Интернета вещей</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="4">iOS</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-yw4l" rowspan="4">Apple.Ios.All</td>
    <td class="tg-yw4l">Apple.Ios.Phone</td>
    <td class="tg-yw4l">iPhone, сенсорного ввода</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Планшет</td>
    <td class="tg-yw4l">Apple.Ios.Tablet</td>
    <td class="tg-yw4l">iPad mini, iPad, iPad Pro</td>
  </tr>
  <tr>
    <td class="tg-yw4l">TV</td>
    <td class="tg-yw4l">Apple.Ios.TV</td>
    <td class="tg-yw4l">Apple TV</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Посмотрите</td>
    <td class="tg-yw4l">Apple.Ios.Watch</td>
    <td class="tg-yw4l">iWatch</td>
  </tr>
  <tr>
    <td class="tg-yw4l">macOS</td>
    <td class="tg-yw4l">Настольный компьютер</td>
    <td class="tg-baqh" colspan="2">Apple.MacOS.All</td>
    <td class="tg-yw4l">MacBook Pro, iMac, MacBook Air, Mac Mini</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="5">Android</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-yw4l" rowspan="5">Google.Android.All</td>
    <td class="tg-yw4l">Google.Android.Phone</td>
    <td class="tg-yw4l">Мобильных устройств, предназначенных для любой разновидности Android</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Планшет</td>
    <td class="tg-yw4l">Google.Android.Tablet</td>
    <td class="tg-yw4l">Планшетов с Android</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Настольный компьютер</td>
    <td class="tg-yw4l">Google.Android.Desktop</td>
    <td class="tg-yw4l">Ли Chromebooks</td>
  </tr>
  <tr>
    <td class="tg-yw4l">TV</td>
    <td class="tg-yw4l">Google.Android.TV</td>
    <td class="tg-yw4l">Устройства Android большом экране</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Посмотрите</td>
    <td class="tg-yw4l">Google.Android.Watch</td>
    <td class="tg-yw4l">Устройства Google шестеренки</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="2">Windows</td>
    <td class="tg-yw4l">7</td>
    <td class="tg-baqh" colspan="2">Windows7.Desktop</td>
    <td class="tg-yw4l">Устройства Windows 7</td>
  </tr>
  <tr>
    <td class="tg-yw4l">8</td>
    <td class="tg-baqh" colspan="2">Windows8.Desktop</td>
    <td class="tg-yw4l">Устройства Windows 8 и 8.1</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="5">Интернет</td>
    <td class="tg-yw4l">Майкрософт</td>
    <td class="tg-yw4l" rowspan="5">Web.All</td>
    <td class="tg-yw4l">Web.Edge.All</td>
    <td class="tg-yw4l">Edge веб-ядра приложения</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Android</td>
    <td class="tg-yw4l">Web.Blink.All</td>
    <td class="tg-yw4l">BLINK ядра веб-приложений</td>
  </tr>
    <tr>
    <td class="tg-yw4l">Хром</td>
    <td class="tg-yw4l">Web.Chromium.All</td>
    <td class="tg-yw4l">Chrome веб-ядра приложения</td>
  </tr>
  <tr>
    <td class="tg-yw4l">iOS</td>
    <td class="tg-yw4l">Web.Webkit.All</td>
    <td class="tg-yw4l">Webkit веб-ядра приложения</td>
  </tr>
  <tr>
    <td class="tg-yw4l">macOS</td>
    <td class="tg-yw4l">Web.Safari.All</td>
    <td class="tg-yw4l">Safari веб-ядра приложения</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Linux</td>
    <td class="tg-yw4l">Any/All</td>
    <td class="tg-baqh" colspan="2">Linux.All</td>
    <td class="tg-yw4l">Все дистрибутивы Linux</td>
  </tr>
</table>

В файле манифеста приложения пакета необходимо будет включать семейства устройств соответствующему целевому объекту в том случае, если вам нравится быть извлечено только на определенных платформах и устройствах содержимого пакета. Если вам нравится bulid пакет таким образом, что он поддерживается для всех типов платформы и устройства, выберите **Platform.All** как семейства целевых устройств. Аналогично, если вам нравится пакета поддерживаться только в веб-приложений, выберите **Web.All**.

## <a name="sample-manifest-file-appxmanifestxml"></a>Пример файла манифеста (AppxManifest.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
         IgnorableNamespaces="mp uap uap3">

  <Identity Name="BestAppExtension"
            Publisher="CN=awesomepublisher"
            Version="1.0.0.0" />

  <mp:PhoneIdentity PhoneProductId="56a6ecda-c215-4864-b097-447edd1f49fe" PhonePublisherId="00000000-0000-0000-0000-000000000000"/>

  <Properties>
    <DisplayName>Best App Extension</DisplayName>
    <PublisherDisplayName>Awesome Publisher</PublisherDisplayName>
    <Description>This is an extension package to my app</Description>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>

  <Resources>
    <Resource Language="x-generate"/>
  </Resources>

  <Dependencies>
    <TargetDeviceFamily Name="Platform.All" MinVersion="0.0.0.0" MaxVersionTested="0.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="App">
      <uap:VisualElements
          DisplayName="Best App Extension"
          Description="This is the best app extension"
          BackgroundColor="white"
          Square150x150Logo="images\squareTile-sdk.png"
          Square44x44Logo="images\smallTile-sdk.png"
          AppListEntry="none">
      </uap:VisualElements>

      <Extensions>
        <uap3:Extension Category="Windows.appExtension">
          <uap3:AppExtension Name="add-in-contract" Id="add-in" PublicFolder="Public" DisplayName="Sample Add-in" Description="This is a sample add-in">
            <uap3:Properties>
               <!--Free form space-->
            </uap3:Properties>
          </uap3:AppExtension>
        </uap3:Extension>
      </Extensions>

    </Application>
  </Applications>
</Package>
```

## <a name="platform-version"></a>Версия платформы
В выше пример файла манифеста, а также имя платформы, также доступны параметры, указывающие **MinVersion** и **maxversiontested укажите** эти параметры используются на платформах Windows 10. В Windows 10 пакет будет развернут только в версиях ОС Windows 10, больше, чем MinVersion. На других платформах Windows 10 MinVersion и maxversiontested укажите параметры не используются для объявления того, можно ли извлечь содержимое пакета.

Если вы хотите использовать пакет для всех платформ (Windows 10 и Windows 10), рекомендуется использовать MinVersion и maxversiontested укажите параметры для указания версий ОС Windows 10, место работы вашего приложения. Поэтому в манифест **зависимости** раздел будет выглядеть следующим образом:
```xml
  <Dependencies>
    <TargetDeviceFamily Name="Platform.All" MinVersion="0.0.0.0" MaxVersionTested="0.0.0.0"/>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.14393.0" MaxVersionTested="10.0.16294.0"/>
  </Dependencies>
```

**MinVersion** и **maxversiontested укажите** поля являются обязательными в манифесте, и они должны соответствовать четырьмя notation(#.#.#.#). Если только вы используете MSIX пакетов SDK для платформ, отличных от Windows 10, можно просто использовать "0.0.0.0» как **MinVersion** и **maxversiontested укажите** как версии.

## <a name="how-to-effectively-use-the-same-package-on-all-platforms-windows-10-and-non-windows-10"></a>Инструкции по эффективному использованию тот же пакет на всех платформах (Windows 10 и Windows 10)

Чтобы использовать все преимущества упаковки MSIX пакета SDK, необходимо будет выполнить сборку пакета способом, который будет развертываться как пакет приложения в Windows 10 и в то же время, поддерживаемые на других платформах. В Windows 10, можно создать пакет в качестве *расширение приложения*. Дополнительные сведения о расширения приложений и как они могут помочь сделать расширяемого приложения, см. в разделе [введение расширения приложения](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/) записи блога.

В примере файла манифеста, показанном ранее в этой статье, можно заметить **свойства** сервисном **AppExtension** элемент. Отсутствует проверка выполняется в этом разделе файла манифеста. Это позволяет разработчикам указывать нужные метаданные между расширения и узла или клиента приложения.
