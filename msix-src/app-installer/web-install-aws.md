---
title: Распространение приложения для Windows 10 из веб-службы AWS
description: Руководство по настройке веб-сервера AWS для проверки установки приложения с помощью приложения установщика приложений
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, установщик приложений, AppInstaller, загружать неопубликованные, связанный набор, дополнительные пакеты, AWS
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 4e999f4939432c6490da380c722c31ae87d669d8
ms.sourcegitcommit: f5936c95c0f5b6f080e51b8d47a7cd62ccf6a600
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2020
ms.locfileid: "80241953"
---
# <a name="distribute-a-windows-10-app-from-an-aws-web-service"></a>Распространение приложения для Windows 10 из веб-службы AWS

Приложение "Установщик приложений" позволяет разработчикам и ИТ-специалистам распространять приложения Windows 10 путем их размещения в собственной сети доставки содержимого (CDN). Это полезно для предприятий, которым не требуется публиковать свои приложения в Microsoft Store, но они все же хотят воспользоваться преимуществами платформы упаковки и развертывания Windows 10.

В этом разделе описаны шаги по настройке веб-сайта Amazon Web Services (AWS) для размещения пакетов приложений Windows 10 и использованию приложения установщика приложений для установки пакетов приложений.

## <a name="setup"></a>Установка

Для успешного выполнения действий в этом руководстве необходимо следующее:
 
1. Подписка AWS 
2. Веб-страница
3. Пакет приложения Windows 10 — пакет приложения, который будет распространяться

Необязательно: [Стартовый проект](https://github.com/AppInstaller/MySampleWebApp) на GitHub. Это полезно в том случае, если у вас нет пакета приложения или веб-страницы для работы, но вы все равно хотите узнать, как использовать эту функцию.

В этом руководстве описано, как настроить веб-страницу и разместить пакеты на AWS. Для этого потребуется подписка AWS. В зависимости от масштаба операции можно использовать свое бесплатное членство в этом руководстве. 

## <a name="step-1---aws-membership"></a>Шаг 1. членство в AWS
Чтобы получить членство в AWS, перейдите на [страницу сведений об учетной записи AWS](https://aws.amazon.com/free/). В рамках этого учебника можно использовать бесплатное членство.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Шаг 2. Создание контейнера Amazon S3

Amazon Simple Storage Service (S3) — это AWS предложение для сбора, хранения и анализа данных. Контейнеры S3 — это удобный способ размещения пакетов приложений Windows 10 и веб-страниц для распространения. 

После входа в AWS с вашими учетными данными в разделе `Services` найти `S3`. 

Выберите **создать контейнер**и введите **имя контейнера** для веб-сайта. Следуйте указаниям диалогового окна, чтобы задать свойства и разрешения. Чтобы обеспечить возможность распространения приложения Windows 10 с веб-сайта, включите разрешения на **Чтение** и **запись** для контейнера и выберите **предоставить общий доступ на чтение этому контейнеру**.

![Задание разрешений для контейнера Amazon S3](images/aws-permissions.png) 

Проверьте сводку, чтобы убедиться, что выбранные параметры отражены. Щелкните **создать контейнер** , чтобы завершить этот шаг. 

## <a name="step-3---upload-windows-10-app-package-and-web-pages-to-an-s3-bucket"></a>Шаг 3. Передача пакета приложения Windows 10 и веб-страниц в контейнер S3

Вы создали контейнер Amazon S3, вы сможете увидеть его в представлении Amazon S3. Вот пример того, как выглядит наш демонстрационный контейнер:

![Снимок экрана: представление контейнера Amazon S3](images/aws-post-create.png)

Теперь все готово для отправки пакетов приложений и веб-страниц, которые нужно разместить в контейнере Amazon S3. 

Щелкните только что созданный контейнер для отправки содержимого. Контейнер в настоящее время пуст, так как еще не отправлен. Нажмите кнопку **Upload (отправить** ) и выберите пакеты приложений и файлы веб-страниц, которые вы хотите отправить.

> [!NOTE]
> Если у вас нет пакета приложений, вы можете использовать пакет приложений, являющийся частью репозитория [Стартовый проект](https://github.com/AppInstaller/MySampleWebApp) на GitHub. Сертификат (MySampleApp.cer), с помощью которого был подписан пакет, также входит в состав примера на GitHub. Перед установкой приложения на устройстве необходимо установить сертификат.

![Снимок экрана: Отправка пакета взаимодействия с пакетом приложения](images/aws-upload-package.png)

Аналогично разрешениям для создания сегмента Amazon S3, содержимое в контейнере также должно иметь разрешения на **Чтение**, **запись**и **предоставление общего доступа на чтение для этих объектов** .

Если вы хотите протестировать отправку веб-страницы, но у вас ее нет, можно использовать образец HTML-страницы (Default. HTML) из [начального проекта](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Перед отправкой веб-страницы убедитесь, что на веб-странице указана правильная ссылка на пакет приложения. 

Чтобы получить ссылку на пакет приложения, передайте сначала пакет приложения и скопируйте URL-адрес пакета приложения. Измените веб-страницу HTML, чтобы она отражала правильный путь к пакету приложения. Дополнительные сведения см. в примере кода. 

Выберите отправленный файл пакета приложения, чтобы получить ссылку на пакет приложения. 

**Скопируйте** ссылку на пакет приложения и добавьте ссылку на веб-страницу. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.msixbundle"> Install My Sample App</a>
    </body>
</html>
```
Отправьте HTML-файл в контейнер Amazon S3. Не забудьте установить разрешения на доступ для **чтения** и **записи** .

## <a name="step-4---test"></a>Шаг 4. тестирование

После отправки веб-страницы в контейнер Amazon S3 перейдите по ссылке на веб-страницу, выбрав загруженный HTML-файл.

Используйте ссылку, чтобы открыть веб-страницу. Так как мы устанавливаем разрешения на предоставление общего доступа к пакету приложения и веб-странице, любой пользователь со ссылкой на веб-страницу сможет получить к нему доступ и установить пакеты приложений Windows 10 с помощью установщика приложений. Обратите внимание, что установщик приложения является частью платформы Windows 10. Разработчику не нужно добавлять дополнительный код или функции в приложение, чтобы разрешить использование установщика приложений. 

## <a name="troubleshooting"></a>Диагностика

### <a name="app-installer-fails-to-install"></a>Не удается установить установщик приложения 

Установка приложения завершится ошибкой, если сертификат, на котором подписан пакет приложения, не установлен на устройстве. Чтобы устранить эту проблему, необходимо установить сертификат перед установкой приложения. Если вы размещаете пакет приложения для общедоступного распространения, рекомендуется подписать пакет приложения с помощью сертификата из центра сертификации. 

