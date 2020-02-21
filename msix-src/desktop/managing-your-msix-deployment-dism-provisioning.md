---
Description: В этой статье описываются различные способы установки приложения MSIX для всех пользователей.
title: Установка приложения для всех пользователей
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e20d16fb5c25bc76ecccab9dd6a2b59cc1dc39a6
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073980"
---
# <a name="install-your-app-for-all-users-with-dism"></a>Установка приложения для всех пользователей с помощью DISM

Существует ряд средств, которые можно использовать для установки приложения MSIX для всех пользователей. [DISM (DISM.exe)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism---deployment-image-servicing-and-management-technical-reference-for-windows) — это программа командной строки, которую можно использовать для обслуживания и подготовки образов Windows, включая используемые для Windows PE, среды восстановления Windows (Windows RE) и программы установки Windows. DISM можно использовать для обслуживания образа Windows (.wim) или виртуального жесткого диска (.vhd или .vhdx).

Подготовка Windows помогает ИТ-профессионалам настраивать устройства пользователей, не создавая образы. Используя подготовку Windows, ИТ-профессионал может легко указать желаемые конфигурацию и параметры, необходимые для регистрации устройства в системе управления, а затем применить эту конфигурацию к целевым устройствам за считаные минуты. Например, при установке приложения для всех пользователей на устройстве. 

Начиная с версии Windows 1809, ИТ-профессионалы могут предварительно установить приложение с помощью подготовки.   Подготовленные приложения будут установлены в центральное расположение — %ProgramFiles%\WindowsApps и будут сразу же доступны зарегистрированным пользователям. Для пользователей, не имеющих зарегистрированного в учетной записи MSIX, доступ будет запрещен.  
Дополнительные сведения о подготовке пакетов см. в статье [Provisioning packages for Windows 10](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-packages). (Пакеты подготовки для Windows 10)

## <a name="user-preferences-when-provisioning"></a>Настройки пользователя при подготовке

Подготовка учитывает настройки пользователей, и поэтому, когда пользователь удаляет подготовленное приложение, единственный способ вернуть такое приложение — это переустановить его. В Windows 10 версии 2004, приложение с установленными настройками будет автоматически переопределять пользовательские настройки и переустанавливать приложение при повторном создании.

Вы также должны быть внимательны, чтобы предотвратить установку и обновление пользователями различных версий пакета MSIX, предоставляемого поставщиком. Если пользователь модернизирует или ухудшает качество приложения, устанавливая тот же самый пакет из магазина, вы можете получить неожиданные результаты.
Дополнительные сведения о настройке MSIX, смотрите статью [Create Windows applications in Configuration Manager](https://docs.microsoft.com/configmgr/apps/get-started/creating-windows-applications) (Создание приложений Windows в диспетчере конфигураций).

## <a name="removing-apps-for-all-users"></a>Удаление приложений для всех пользователей

Помимо установки приложений для всех пользователей, вы также можете деинсталлировать приложение для всех пользователей. Для этого используется API диспетчера пакетов, [PackageManager.RemovePackageAsync Method](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.removepackageasync).

В качестве альтернативы можно использовать метод **AppxRemovePackageForAllUsers(PCWSTR packageFullName, BOOL* reserved)()* *, который можно найти в **AppxDeploymentClient.dll**, который был представлен в Windows 10 версии 1703. Однако мы рекомендуем использовать вместо этого **PackageManager.RemovePackageAsync**.
