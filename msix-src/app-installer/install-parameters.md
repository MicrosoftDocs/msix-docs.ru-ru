---
title: Передача параметров установки в приложение с помощью установщика приложений
description: Описывает, как передавать параметры установки в приложение через установщик приложения и активацию протокола.
author: Huios
ms.date: 12/17/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: f6b60e925d3df5bf784d8df19295f88fd5e50b15
ms.sourcegitcommit: 77e8b10706d4ad2fa97b2c71d82947528555398b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691837"
---
# <a name="passing-installation-parameters-to-your-app-via-app-installer"></a>Передача параметров установки в приложение с помощью установщика приложений

При распространении приложения с помощью MSIX можно настроить приложение таким, чтобы параметры строки запроса, определяемые в URI скачивания или установки, передавались приложению при запуске, после того как пользователь щелкнет URI скачивания или установки. Это работает, когда пользователь впервые устанавливает приложение или если приложение было установлено ранее. В этой статье показано, как настроить упакованное приложение MSIX и его URI для скачивания и установки, чтобы воспользоваться преимуществами этой функции. Это может быть полезно, если требуется относить или обрабатывать различные установки на основе источника, загрузки типа и т. д., а также для загрузки веб-страниц и других случаев, когда пользователь щелкает URI, например из кампании по электронной почте. Дополнительные сведения см. в этой [записи блога](https://techcommunity.microsoft.com/t5/windows-dev-appconsult/passing-installation-parameters-to-a-windows-application-with/ba-p/1719829).

## <a name="configure-your-application-for-protocol-activation"></a>Настройка приложения для активации протокола

Первое, что нужно сделать, — зарегистрировать приложение для запуска с помощью [настраиваемого протокола](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-in-different-ways) , который вы определили. При вызове этого протокола приложение запускается и все праметерс, указанные в URI, передаются в аргументы события активации приложения при его запуске. Вы можете зарегистрировать протокол, добавив запись расширения протокола в узел расширения приложения в файле appxmanifest.xml MSIX:

```xml
<Application>
...
   <Extensions>
     <uap:Extension Category="windows.protocol">
        <uap:Protocol Name="my-custom-protocol"/>
     </uap:Extension>
   </Extensions>
  
...
</Application>
```

Если вы используете [проект упаковки Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), можно также определить пользовательский протокол с помощью редактора манифестов по умолчанию, дважды щелкнув файл _Package. appxmanifest_ , перейдя на вкладку _объявления_ и выбрав _протокол_ в списке _Доступные объявления_:

![Объявление протокола в Package. appxmanifest](images/custom-protocol.PNG)

##  <a name="write-code-to-handle-parameters-when-your-app-is-launched-after-installation"></a>Написание кода для управления параметрами при запуске приложения после установки

Необходимо реализовать код в приложении, чтобы обрабатывать параметры установки, которые будут переданы в приложение при его запуске. В приведенном ниже примере кода используется метод [аппинстанце. жетактиватедевентаргс](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs?view=winrt-19041) для определения типа активации, используемого для создания экземпляра приложения (можно также выполнять обработку параметров с помощью другого метода). Когда приложение запускается или активируется с помощью параметров проверочного запросов из URI установки (см. Описание определения в следующем разделе), тип активации будет активироваться протоколом, как определено настраиваемым протоколом, объявленным в appxmanifest.xml и скачать/установить URI. Аргументы события активации будут иметь тип [протоколактиватедевентаргс](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs?view=winrt-19041) , который используется в коде, приведенном ниже.

```csharp

using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;

public static void Main(string[] cmdArgs)
{
            
    var activationArgs = AppInstance.GetActivatedEventArgs();
    switch (activationArgs.Kind)
    {
        //Install parameters will be passed in during a protocol activation
        case ActivationKind.Protocol:
        HandleProtocolActivation(activationArgs as ProtocolActivatedEventArgs);
        break;
        case ActivationKind.Launch:
        //Regular launch activation type
        HandleLaunch(activationArgs as LaunchActivatedEventArgs);
        break;
        default:
        break;
     }       
    

     static void HandleProtocolActivation(ProtocolActivatedEventArgs args)
     {

         if (args.Uri != null)
        {
            //Handle the installation parameters in the protocol uri
            handleInstallParameter(args.Uri.ToString());

        }
            
}
```

## <a name="add-your-custom-activation-protocol-and-parameters-to-the-installation-uri"></a>Добавление пользовательского протокола активации и параметров в URI установки

После того как приложение настроено для работы с параметрами установки, можно настроить URI скачивания и установки приложения, чтобы он содержал уникальные параметры, которые будут переданы в приложение при запуске, после того, как пользователь щелкнет URI. URI должен содержать:

1. Протокол [MS-appinstaller](https://docs.microsoft.com/windows/msix/app-installer/installing-windows10-apps-web#protocol-activation-scheme) , который вызывает установщик приложения.
2. Уникальный параметр **активатионури** , указывающий на пользовательский протокол приложения и параметры установки, которые необходимо передать в приложение при его запуске.
3. Настраиваемый протокол приложения, а также параметр и его значение.

В приведенных ниже примерах универсальных кодов ресурса (URI) я определил настраиваемый протокол _My-Custom-Protocol_, параметр _My-Parameter_ и присвоить ему значение _My-PARAM-value_. Когда приложение запускается после того, как пользователь щелкнет универсальный код ресурса (URI), он получит часть строки запроса в URI после **активатионури**, в этом случае это будет _мой пользовательский параметр — протокол:? My-Parameter = My-PARAM-value_.

```html
ms-appinstaller:?source=https://contoso.com/myapp.appinstaller&activationUri=my-custom-protocol:?my-parameter=my-param-value
```
```html
ms-appinstaller:?source=https://contos.com/myapp.msix&activationUri=my-custom-protocol:?my-parameter=my-param-value
```
