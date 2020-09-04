---
Description: В этой статье содержатся все сведения, необходимые для управления развертыванием приложений MSIX в корпоративной среде.  Эта статья предназначена для ИТ-специалистов предприятий.
title: Управление развертыванием MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, развертывание, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 2ecd18c4bf7cc48bca8f9cea39dedd4a042e41fc
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090462"
---
# <a name="msix-validation-and-troubleshooting"></a>Проверка пакета MSIX и устранение неполадок
Несмотря на то, что установка MSIX завершается успешно в 99 % случаев, иногда может возникнуть необходимость в устранении проблем с установкой.

## <a name="know-the-application"></a>Узнайте принцип работы приложения
Понимание устанавливаемого приложения и того, как оно работает, может в значительной степени способствовать устранению неполадок в работе пользователей.  Например, имеет ли приложение определенные ограничения, которые могут вызвать проблемы?  Имеет ли пользователь доступ к ресурсам, необходимым для работы приложения?  Были ли у приложения зависимости, которые не были удовлетворены текущей операционной системой?

## <a name="test-your-application"></a>Тестирование приложения
Перед развертыванием приложения убедитесь, что оно протестировано.  Windows SDK предоставляет инструмент "Комплект сертификации приложений для Windows", который может определить общие проблемы до публикации приложения.  
Для установки новейшего пакета Windows SDK, перейдите по ссылке [здесь.](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
Дополнительные сведения см. в статье [Windows App Certification Kit](/windows/uwp/debug-test-perf/windows-app-certification-kit) (Комплект сертификации приложений для Windows).

## <a name="flight-your-application"></a>Тестирование приложения
Еще одним отличным способом обнаружения проблем на ранней стадии является тестирование приложения.  При установке приложения через Microsoft Store или Microsoft Store для бизнеса тестовый пакет можно использовать для развертывания приложения для подмножества частных лиц, чтобы получить дополнительное тестирование в реальных условиях.  
Подробнее о фокус-тестировании см. в статье[Package flighting.](/windows/uwp/publish/package-flights?context=%252fwindows%252fmsix%252frender) (Тестовые пакеты)

## <a name="device-portal-and-debugging"></a>Портал устройств и отладка
Иногда, чтобы адекватно разобраться в проблеме, необходимо взаимодействовать с приложением в пользовательской среде.  Windows предоставляет мощный инструмент [Device Portal Desktop](/windows/uwp/debug-test-perf/device-portal-desktop) (Рабочий стол портала устройств), который позволит подключиться к устройству и взаимодействовать с приложением удаленно.  
Дополнительные сведения о портале устройств см. в статье [Device Portal for Windows Desktop](/windows/uwp/debug-test-perf/device-portal-desktop) (Портал устройств для компьютеров на Windows).
Подробнее об отладке пакетов MSIX см. в статье [Запуск, отладка и тестирование упакованного классического приложения](./desktop-to-uwp-debug.md)

## <a name="installation-issues"></a>Проблемы, связанные с установкой
Когда возникают проблемы с установкой, вы можете исследовать артефакты, предоставляемые AppInstaller.  Во-первых, AppInstaller выдает сбои в коде ошибок.  Если кода ошибки любого сбоя недостаточно для определения причины, AppInstaller также записывает все взаимодействия в журнал "Просмотр событий".  Журналы можно найти здесь: Журналы приложений и служб Logs->Microsoft->Windows->AppxDeployment-Server.

Для доступа к этим событиям можно использовать Просмотр событий или Powershell. Подробнее о том, как просмотреть события, смотрите статью [Troubleshooting packaging, deployment, and query of Windows apps](/windows/win32/appxpkg/troubleshooting) (Устранение неполадок при упаковке, развертывании и запросе в приложениях Windows)

Подробнее об устранении неполадок AppInstaller см. в статье [Troubleshoot installation issues with the App Installer file](../app-installer/troubleshoot-appinstaller-issues.md) (Устранение проблем файла установщика приложений, связанных с установкой)


## <a name="next-steps"></a>Дальнейшие действия

Есть вопросы? Задайте их на Stack Overflow. Наша команда следит за этими [тегами](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Вы также можете задать нам вопросы [здесь](https://social.msdn.microsoft.com/Forums//home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

