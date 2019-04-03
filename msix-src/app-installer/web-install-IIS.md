---
author: mcleanbyron
title: Установка приложения для Windows 10 с сервера IIS | Документация Майкрософт
description: Этом руководстве показано, как настроить сервер IIS, убедитесь, что веб-приложения может разместить пакеты приложения и вызвать и эффективного использования установщика приложений.
ms.author: mcleans
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, универсальной платформы Windows, установщик приложения, AppInstaller, загрузки неопубликованных приложений, связанных с пакетов установлен, необязательно, сервера IIS
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 58415ff867d322f953ddb2a9d91d9593dd17d589
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900606"
---
# <a name="install-a-windows-10-app-from-an-iis-server"></a>Установка приложения для Windows 10 с сервера IIS

Этом руководстве показано, как настроить сервер IIS, убедитесь, что веб-приложения может разместить пакеты приложения и вызвать и эффективного использования установщика приложений.

Приложение "Установщик приложений" позволяет разработчикам и ИТ-специалистам распространять приложения Windows 10 путем их размещения в собственной сети доставки содержимого (CDN). Это полезно для предприятий, которым не требуется публиковать свои приложения в Microsoft Store, но они все же хотят воспользоваться преимуществами платформы упаковки и развертывания Windows 10. 

## <a name="setup"></a>Установка

Чтобы успешно пройти с этим руководством, необходимо следующее:

1. Visual Studio 2017  
2. Средства веб-разработки и IIS 
3. Пакет приложения Windows 10 — пакет приложения, который будет распространяться

Дополнительно [Начальный проект](https://github.com/AppInstaller/MySampleWebApp) на сайте GitHub. Это полезно в том случае, если у вас нет пакетов приложений для работы с, но по-прежнему хотите узнать, как использовать эту функцию.

## <a name="step-1---install-iis-and-aspnet"></a>Шаг 1 - Установка служб IIS и ASP.NET 

[Internet Information Services](https://www.iis.net/) является компонентом Windows, которая может устанавливаться через меню "Пуск". В **меню "Пуск"** поиск **или отключение компонентов Windows включить**.

Найдите и выберите **Internet Information Services** Установка служб IIS.

> [!NOTE]
> Установите все флажки в службах IIS не требуется. Только выбранные при проверке **Internet Information Services** достаточно.

Также необходимо будет установить ASP.NET 4.5 или более поздней. Чтобы установить его, найдите **Internet Information Services "->" World Wide Web Services -> компоненты разработки приложений**. Выберите версию ASP.NET, которое больше или равно для ASP.NET 4.5.

![Снимок экрана: Установка компонента ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Шаг 2 - установить Visual Studio 2017 и средства разработки веб-приложений 

[Установите Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) Если вы еще не установили его. Если у вас уже есть Visual Studio 2017, убедитесь, что установлены следующие рабочие нагрузки. Если рабочие нагрузки не существуют в среде установки, выполните его с использованием Visual Studio Installer (найдено из меню "Пуск").  

Во время установки, выберите **ASP.NET и веб-разработка** и других рабочих нагрузок, которые вас интересуют. 

После завершения установки запустите Visual Studio и создайте новый проект (**файл** -> **новый проект**).

## <a name="step-3---build-a-web-app"></a>Шаг 3 — Создание веб-приложения

Запустите Visual Studio 2017 как **администратора** и создайте новый **Visual C# веб-приложение** проект с **пустой** шаблона проекта. 

![Снимок экрана создания нового веб-проекта](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Шаг 4 — Настройка IIS с нашего веб-приложения 

В обозревателе решений щелкните правой кнопкой мыши щелкните корневой проект и выберите **свойства**.

В свойства веб-приложения, выберите **Web** вкладки. В **серверы** выберите **локальный сервер IIS** из раскрывающегося меню и выберите пункт **создать виртуальный каталог**. 

![Снимок экрана вкладки web в свойствах проекта](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Шаг 5: Добавление пакета приложения в веб-приложение 

Добавление пакета приложения, предназначенные для распространения в веб-приложение. Можно использовать пакет приложения, который является частью предоставленный [пакеты начального проекта](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) на сайте GitHub, если у вас нет доступных пакета приложения. Сертификат (MySampleApp.cer), с помощью которого был подписан пакет, также входит в состав примера на GitHub. Необходимо иметь сертификат для устройства до установки приложения (шаг 9).

Начальный проект веб-приложения, новую папку был добавлен в веб-приложение, которое вызывается **пакетов** , содержащий пакеты для распространения приложений. Чтобы создать папку в Visual Studio, щелкните правой кнопкой мыши узел проекта в обозревателе решений выберите **добавить** -> **новую папку** и назовите его **пакетов**. Чтобы добавить пакеты приложений к папке, щелкните правой кнопкой мыши **пакетов** папку и выберите **добавить** -> **существующий элемент...**  и перейдите к расположению пакета приложения. 

![Снимок экрана: Добавление пакета](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Шаг 6 — создание веб-страницы

Этот пример веб-приложения использует простой HTML. Вы можете свободно создавать веб-приложения, как требуется в соответствии с вашими потребностями. 

Щелкнуть правой кнопкой мыши корневой проект в обозревателе решений выберите **добавить** -> **новый элемент**и добавьте новую **HTML-страницу** из **Web**раздел.

После создания HTML-страницу, щелкните правой кнопкой мыши на HTML-страницу в обозревателе решений и выберите **задать в качестве начальной страницы**.  

Дважды щелкните файл HTML, чтобы открыть его в окне редактора кода. В этом руководстве будет использоваться только те элементы, которые в обязательном на веб-странице для вызова приложения установщика приложения успешно, чтобы установить приложение Windows 10. 

Включить следующий код HTML в веб-страницы. Ключ для успешного вызова установщика приложений является использование настраиваемой схемой, регистрируемая установщика приложений с операционной Системой: `ms-appinstaller:?source=`. См. в примере кода ниже для получения дополнительных сведений.

> [!NOTE]
> Убедитесь, что путь URL-адреса, указанные после пользовательской совпадает с URL-адрес проекта на вкладке веб-решения VS.
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Шаг 7 - Настройка веб-приложения для типов MIME пакета приложений

Откройте **Web.config** файл в обозревателе решений и добавьте следующие строки в пределах `<configuration>` элемент. 

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
</system.webServer>
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Шаг 8 - Добавление исключение замыкания на себя для установщика приложений

Из-за сетевой изоляции приложений Windows 10 как установщика приложений, ограничены использовать IP-адреса замыкания на себя как http://localhost/. При использовании локального сервера IIS, установщик приложения необходимо добавить в список исключенных замыкания на себя. 

Чтобы сделать это, откройте **командной строки** как **администратора** и введите следующее: ''' CheckNetIsolation.exe LoopbackExempt командной строки --n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Вы должны найти `microsoft.desktopappinstaller_8wekyb3d8bbwe` в списке.

По завершении локальная проверка установки приложения с помощью установщика приложения можно удалить исключение замыкания на себя, добавленного при изучении этого шага по:

''' Командной строки CheckNetIsolation.exe LoopbackExempt -d-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![Screenshot of running web app in Visual Studio](images/run.png)

A web page will open in your browser:

![Screenshot of installing app from web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
