---
Description: В этой статье содержатся все сведения, необходимые для управления развертыванием приложений MSIX в среде для розничной торговли.  Эта статья предназначена для разработчиков.
title: Распространение пакета MSIX в потребительской среде
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, развертывание, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 3a27a4804786ce82485ba3e7de9c855a2bf2e23c
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073860"
---
# <a name="distribute-your-msix-in-a-consumer-environment"></a>Распространение пакета MSIX в потребительской среде

Если вы не являетесь корпоративным разработчиком, можете выполнять розничную реализацию MSIX через обычные розничные каналы.  Сюда относится даже размещение MSIX на обычной веб-странице.  

## <a name="microsoft-store"></a>Microsoft Store

Платформа [Microsoft App Store](https://www.microsoft.com/store/apps/windows) может стать отличным способом для распространения приложений.  Она стала доступной для потребителей с момента запуска Windows 8 в конце 2012 г. Пользователи могут здесь просматривать, искать и скачивать приложения или игры для Windows.

Дополнительные сведения о платформе Microsoft App Store и ее возможностях см. [здесь](https://docs.microsoft.com/windows/uwp/publish/). 

Чтобы приступить к публикации приложения на платформе Microsoft App Store и освоить ее, откройте [Центр партнеров](https://partner.microsoft.com/dashboard/home).

## <a name="app-center"></a>Центр приложений

[Центр приложений](https://appcenter.ms/) — это место, где можно автоматически выполнять сборку приложений, тестировать их на реальных устройствах и распространять для бета-тестирования.  Центр приложений позволяет поставлять приложения чаще, с более высоким качеством и лучшей защитой.  С помощью Центра приложений вы можете подключаться к своему репозиторию и за считаные минуты автоматизировать сборки, тестировать приложения на реальных устройствах в облаке, распространять их для бета-тестирования и отслеживать их реальное использование, получая данные о сбоях и аналитические данные. Все что нужно — под рукой.
Дополнительные сведения о Центре приложений см. [здесь](https://docs.microsoft.com/appcenter/).

## <a name="web-install"></a>Веб-установка

MSIX-файл может размещаться на любом сервере IIS.  Но вы можете значительно улучшить работу, включив прямую установку через Установщик приложений.  Прямая установка позволяет немедленно запустить Установщик приложений и выполнить установку с веб-сервера, без предварительного скачивания файлов.  Дополнительные сведения о веб-установке см. в статье [Installing Windows 10 apps from a web page](https://docs.microsoft.com/windows/msix/app-installer/installing-windows10-apps-web) (Установка приложений Windows 10 c веб-страницы).

 