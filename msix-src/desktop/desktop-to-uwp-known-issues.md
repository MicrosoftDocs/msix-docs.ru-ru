---
Description: В этой статье описываются известные проблемы, касающиеся моста для классических приложений.
title: Известные проблемы с упакованных приложений рабочего стола
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: 4f8ac1b2eee520c4a1a3facbaba6aca248e346e8
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400785"
---
# <a name="known-issues-with-packaged-desktop-apps"></a>Известные проблемы с упакованных приложений рабочего стола

Эта статья содержит известные проблемы, которые могут возникнуть при создании пакета MSIX для вашего приложения.

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Сообщение об ошибке MSB4018, неожиданный сбой задачи GenerateResource

Это может произойти при попытке преобразования вспомогательных сборок в файлы индекса ресурсов пакетов (PRI).

Мы осведомлены об этой проблеме и работаем над ее окончательным решением. В качестве временного решения можно отключить генератор ресурсов, добавив следующую строку XML в первый элемент PropertyGroup в файле вышестоящего проекта:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Синий экран с кодом ошибки 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

После установки или запуска некоторых приложений из Microsoft Store, ваш компьютер может быть неожиданно перезагружен с ошибкой: **0x139 (ЯДРА\_безопасности\_ПРОВЕРЬТЕ\_ СБОЯ)** .

Известно, что эта проблема затрагивает приложения Kodi, JT2Go, Ear Trumpet, Teslagrad и др.

27.10.2016 г. было выпущено [обновление Windows (версия 14393.351 — KB3197954)](https://support.microsoft.com/kb/3197954), в которое вошли важные исправления, направленные на решение этой проблемы. Если вы столкнулись с этой проблемой, обновите свой компьютер. Если вам не удается обновить компьютер, потому что он перезапускается до того, как вам удается войти в систему, восстановите систему до точки более ранней, чем установка одного из затрагиваемых проблемой приложений. О том, как восстановить систему, см. в статье [Параметры восстановления в Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Если обновление не устраняет проблему или вы не знаете, как восстановить компьютер, обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/contactus/).

Если вы разработчик, вы можете запретить установку своих упакованных приложений в версиях Windows, в которых нет этого обновления. Обратите внимание, что это приложение не будет доступен пользователям, которые еще не установили обновление. Чтобы ограничить доступность для пользователей, на которых установлено это обновление, измените файл AppxManifest.xml следующим образом:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Подробнее о Центре обновления Windows можно прочитать по следующим ссылкам:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Распространенные ошибки, которые могут появиться при входе в приложение

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>Издатель и cert несоответствие приводит к ошибке Signtool «ошибка: Не удалось SignerSign()» (-2147024885/0x8007000b.)

Запись издателя в манифесте пакета приложения для Windows должна соответствовать субъекту сертификата, с помощью которого выполняется подписывание.  Вы можете использовать любой из следующих методов для просмотра субъекта сертификата.

**Вариант 1. PowerShell**

Выполните следующую команду PowerShell: В качестве файла сертификата можно использовать как .cer, так и .pfx, так как они содержат идентичные сведения об издателе.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Вариант 2. Проводник**

Дважды щелкните сертификат в проводнике, выберите вкладку *Сведения*, а затем выберите поле *Субъект* в списке. Скопируйте его содержимое.

**Вариант 3. CertUtil**

Запустите **certutil** из командной строки на PFX-файл и скопируйте *субъекта* поле из выходных данных.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>Недействительный сертификат PE (0x800700C1)

Это может произойти, если пакет содержит двоичный файл, имеет сертификат поврежден. Вот некоторые из причин, почему это может произойти:

* Начало сертификата не в конце изображения.  

* Размер сертификата не является положительным.

* Начало сертификата не после `IMAGE_NT_HEADERS32` структуру для 32-разрядный исполняемый файл или после `IMAGE_NT_HEADERS64` структуру для 64-разрядный исполняемый файл.

* Указатель сертификата не были правильно выровнены для WIN_CERTIFICATE структуры.

Чтобы найти файлы, содержащие неправильный сертификат PE, откройте **командной**и задайте переменную среды с именем `APPXSIP_LOG` в значение 1.

```
set APPXSIP_LOG=1
```

Затем из **командной**, подписать приложение еще раз. Пример:

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

Сведения о файлах, которые содержат недопустимый PE сертификат будет отображаться в **окно консоли**. Пример:

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>Следующие шаги

**Найдите ответы на ваши вопросы**

Есть вопросы? Задайте их на Stack Overflow. Наша команда следит за этими [тегами](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Вы также можете задать нам вопросы [здесь](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Отправить отзыв или предложения по функциям**

См. раздел [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)
