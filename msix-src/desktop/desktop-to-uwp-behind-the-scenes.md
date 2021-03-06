---
description: В этой статье подробно рассматривается внутренняя работа моста для классических приложений.
title: Как работает Desktop Bridge
ms.date: 01/30/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: 6e7641f095ef7a74210c796d52f62c778d8b040d
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090082"
---
# <a name="understanding-how-packaged-desktop-apps-run-on-windows"></a>Основные сведения о работе упакованных классических приложений в Windows

Эта статья содержит более подробные сведения о том, что происходит с файлами и записями реестра при создании пакета приложения Windows для классического приложения.

Главная задача современного пакета — максимально отделить состояние приложения от состояния системы, в то же время не нарушая совместимость с другими приложениями. Для этого Windows 10 помещает приложение в пакет MSIX, а затем определяет и перенаправляет некоторые изменения, вносимые приложением в файловую систему и реестр во время выполнения.

Пакеты, создаваемые вами для классических приложений, представляют собой пригодные для запуска только на компьютере приложения с полным доверием, не виртуализированные и не изолированные. Это позволяет им взаимодействовать с другими приложениями точно так же, как это делают классические приложения.

## <a name="installation"></a>Установка

Пакеты приложения устанавливаются в папку *C:\Program Files\WindowsApps\имя_пакета*; исполняемый файл имеет имя вида *имя_приложение.exe*. Каждая папка пакета содержит манифест (с именем AppxManifest.xml), содержащий специальное пространство имен XML для упакованных приложений. Внутри этого файла манифеста находится элемент ```<EntryPoint>```, который указывает на приложение с полным доверием. При запуске этого приложения оно выполняется не в контейнере приложения, а как обычно, т. е. как пользователь.

После развертывания пакета файлы помечаются как доступные только для чтения и блокируются операционной системой. Windows не позволяет запускать приложения, если в эти файлы внесены несанкционированные изменения.

## <a name="file-system"></a>Файловая система

ОС поддерживает различные уровни операций с файловой системой для упакованных классических приложений в зависимости от расположения папок.

### <a name="appdata-operations-on-windows-10-version-1903-and-later"></a>Операции AppData в Windows 10 версии 1903 и более поздних версий

Все недавно созданные файлы и папки в папке AppData пользователя (например, *C:\Users\user_name\AppData*) записываются в частном расположении для каждого пользователя, но во время выполнения они появляются в папке AppData. Это позволяет в некоторой степени разделять состояния для артефактов, которые используются только самим приложением, и дает возможность системе очищать эти файлы при удалении приложения. Изменения в существующих файлах в пользовательской папке AppData позволяют обеспечить более высокую степень совместимости и взаимодействия между приложениями и операционной системой. Это уменьшает "деградацию" файловой системы, потому что операционной системе будет известно о каждом файле или изменении каталога, сделанном приложением. Разделение состояний также позволяет упакованным классическим приложениям получать информацию о том, где осталась неупакованная версия того же самого приложения. Обратите внимание, что операционная система не поддерживает папку виртуальной файловой системы (VFS) для пользовательской папки AppData.

### <a name="appdata-operations-on-windows-10-version-1809-and-earlier"></a>Операции AppData в Windows 10 версии 1809 и более поздних версий

Все операции записи в папку AppData пользователя (например, *C:\Users\имя_пользователя\AppData*), включая создание, удаление и обновление, при записи копируются в закрытое расположение, отдельное для каждого пользователя и каждого приложения. При этом создается иллюзия того, что упакованное приложение редактирует настоящую папку AppData, хотя на самом деле изменения вносятся в закрытую копию. Перенаправляя операции записи таким образом, система может отслеживать все изменения, вносимые приложением в файлы. Это позволяет системе очистить эти файлы при удалении приложения, тем самым уменьшая "деградацию" системы и делая процедуру удаления приложения более комфортной для пользователя.

### <a name="other-folders"></a>Другие папки

Кроме перенаправления AppData, для известных папок Windows (System32, Program Files (x86), и т. д.) выполняется динамическое объединение с соответствующими каталогами в пакете приложения. В корне каждого пакета содержится папка с именем "VFS". Все операции чтения каталогов или файлов в каталоге VFS во время выполнения объединяются с их системными эквивалентами. Например, приложение может содержать файл *C:\Program Files\WindowsApps\имя_пакета\VFS\SystemX86\vc10.dll* в составе пакета приложения, но будет казаться, что этот файл установлен в папку *C:\Windows\System32\vc10.dll*.  Это обеспечивает совместимость с классическими приложениями, которые могут ожидать, что файлы будут находиться не в пакете.

Операции записи в файлы и папки в пакете приложения не допускаются. Операции записи в файлы и папки, которые не входят в пакет, игнорируются и допускаются ОС при условии, что у пользователя есть на них разрешение.

### <a name="common-operations"></a>Типичные операции

В этой краткой справочной таблице приведены типичные операции с файловой системой с указанием того, как они обрабатываются ОС.

| Операция | Результат | Пример |
| --- | --- | --- |
| Чтение или перечисление известного файла или папки Windows | Динамическое объединение *C:\Program Files\имя_пакета\VFS\известная_папка* с локальным системным эквивалентом. | Операция чтения *C:\Windows\System32* возвращает содержимое *C:\Windows\System32* плюс содержимое *C:\Program Files\WindowsApps\имя_пакета\VFS\SystemX86*. |
| Запись в папке AppData | **Windows 10 версии 1903 и более поздних версий.** Новые файлы и папки, созданные в следующих каталогах, перенаправляются в частное расположение для каждого пользователя и пакета:<ul><li>Локальная</li><li>Local\Microsoft</li><li>Роуминг</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Menu\Programs</li></ul>В ответ на команду открытия файла ОС сначала открывает файл из расположения для каждого пользователя и пакета. Если это расположение не существует, ОС попытается открыть файл из настоящей папки AppData. Если файл открыт из настоящего расположения AppData, виртуализация для этого файла не выполняется. Удаление файлов в AppData разрешено при наличии у пользователя на это прав.<p/><p/>**Windows 10 версии 1809 и более ранних версий.** Копирование при записи в расположение, отдельное для каждого пользователя и каждого приложения. | Папка AppData — это обычно *C:\Users\имя_пользователя\AppData*. |
| Запись внутри пакета | Не допускается. Пакет доступен только для чтения. | Операции записи внутри папки *C:\Program Files\WindowsApps\имя_пакета* не допускаются. |
| Запись за пределами пакета | Допускается, если у пользователя есть разрешения. | Операция записи в *C:\Windows\System32\foo.dll* допускается, если пакет не содержит файла *C:\Program Files\WindowsApps\имя_пакета\VFS\SystemX86\foo.dll* и у пользователя есть разрешения. |

### <a name="packaged-vfs-locations"></a>Расположения, соответствующие расположениям в каталоге VFS пакета

В следующей таблице показано, где в системе для приложения создаются оверлеи файлов, поставляемых в составе пакета. Приложение будет считать, что эти файлы находятся в указанных системных расположениях, но на самом деле они будут находиться в перенаправленных расположениях внутри каталога *C:\Program Files\WindowsApps\имя_пакета\VFS*. Расположения FOLDERID происходят от констант [**KNOWNFOLDERID**](/windows/win32/shell/knownfolderid).

Системное расположение | Перенаправленное расположение (в [Корневой каталог пакета]\VFS\) | Действительно для следующих архитектур
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | Common AppData | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>Реестр

Пакеты приложений содержат файл registry.dat, который выступает в качестве логического эквивалента куста *HKLM\Software* в реальном реестре. Во время выполнения этот виртуальный реестр объединяет содержимое этого куста с собственным кустом системы, чтобы обеспечить их единообразное представление. Например, если registry.dat содержит только раздел «Foo», при чтении куста *HKLM\Software* во время выполнения создастся впечатление, что этот куст также содержит раздел «Foo» (в дополнение ко всем собственным разделам системы).

В состав пакета входят только разделы внутри куста *HKLM\Software*; разделы внутри куста *HKCU* или другие составляющие реестра к пакету не относятся. Операции записи в разделы или значения в пакете не допускаются. Операции записи в разделы или значения, которые не входят в пакет, допускаются, если у пользователя есть на них разрешение.

Все операции записи внутри куста HKCU копируются при записи в расположение, отдельное для каждого пользователя и каждого приложения. Как правило, при удалении приложения не удается очистить раздел *HKEY_CURRENT_USER*, потому что данные реестра для вышедших из системы пользователей размонтированы и недоступны.

Результаты всех операций записи сохраняются при обновлении пакета и удаляются только при полном удалении приложения.

### <a name="common-operations"></a>Типичные операции

В этой краткой справочной таблице приведены типичные операции с реестром с указанием того, как они обрабатываются ОС.

Операция | Результат | Пример
:--- | :--- | :---
Чтение или перечисление куста *HKLM\Software* | Динамическое объединение куста пакета с локальным системным эквивалентом. | Например, если registry.dat содержит только раздел «Foo», операция чтения куста *HKLM\Software* во время выполнения возвратит содержимое и *HKLM\Software*, и *HKLM\Software\Foo*.
Запись внутри куста HKCU | Копирование при записи в закрытое расположение, отдельное для каждого пользователя и каждого приложения. | (Аналогично AppData для файлов.)
Запись внутри пакета | Не допускается. Пакет доступен только для чтения. | Операции записи внутри куста *HKLM\Software* не допускаются, если в кусте пакета существует соответствующая пара «раздел-значение».
Запись за пределами пакета | Игнорируется ОС. Допускается, если у пользователя есть разрешения. | Операции записи внутри куста *HKLM\Software* допускаются при условии, что в кусте пакета нет соответствующей пары «раздел-значение», и что у пользователя есть соответствующие разрешения на доступ.

## <a name="uninstallation"></a>Удаление

При удалении пакета пользователем все файлы и папки, расположенные в папке *C:\Program Files\WindowsApps\имя_пакета*, удаляются вместе с результатами всех перенаправленных операций записи в папку AppData или реестр, записанных в процессе упаковки.
