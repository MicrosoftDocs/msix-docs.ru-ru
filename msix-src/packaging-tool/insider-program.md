---
title: Программа предварительной оценки средства упаковки MSIX
description: В этой статье описывается программа предварительной оценки для программы MSIX Packaging Tool, которая предоставляет выпуски с ранним доступом средства упаковки MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10, uwp, MSIX, MSIX Packaging Tool
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b55cb8b4622c3d72109129eca24d2d314437de9a
ms.sourcegitcommit: 789ba344260b0afd46206f94954de3120aefb91e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177673"
---
# <a name="msix-packaging-tool-insider-program"></a>Программа предварительной оценки средства упаковки MSIX

Программа предварительной оценки средства упаковки MSIX обеспечивает ранний доступ для ИТ-специалистов и разработчиков, которые заинтересованы в преобразовании существующих классических приложений в пакеты MSIX. Программа позволяет ознакомиться с новыми функциями до выпуска общих версий средства упаковки MSIX. Более того, вы можете оставить свой отзыв, помогая нам улучшить средство с учетом определенных бизнес-потребностей. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://aka.ms/MSIXPackagingPreviewProgram" data-linktype="external">Щелкните здесь, чтобы присоединиться</a></p></div>

## <a name="prerequisites"></a>Предварительные требования

- Windows 10, версия 1809 или более поздняя.
- Допустимая учетная запись Майкрософт для доступа к приложению из Microsoft Store.
- Права администратора на компьютере для запуска средства.

## <a name="install"></a>Установка

После регистрации для участия в программе вы получите сообщение электронной почты с подтверждением регистрации. 

Установите средство упаковки MSIX из [Microsoft Store](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf). Убедитесь, что вы вошли с учетной записью Майкрософт, которая использовалась для регистрации в Программе предварительной оценки средства упаковки MSIX. Затем перейдите на страницу с описанием продукта и щелкните значок **установки**, чтобы начать установку. Сборки для предварительной оценки недоступны для автономного распространения.

Если средство уже установлено на компьютере, проверьте установленную версию. Для этого запустите средство упаковки MSIX, щелкните значок шестеренки в правом верхнем углу и откройте вкладку со **сведениями о программе**. Версия приложения должна совпадать с текущей предварительной сборкой Insider Preview из раздела [ниже](#current-insider-preview-build).

## <a name="current-insider-preview-build"></a>Текущая предварительная сборка Insider Preview

### <a name="version-1202012190---public-version"></a>Версия 1.2020.1219.0 — общедоступная версия
- Удалена поддержка подписи Device Guard версии 1. См. документацию по использованию [версии 2](../package/signing-package-device-guard-signing.md). Если у вас возникнут вопросы, обратитесь в службу поддержки подписывания для Device Guard. DGSSMigration@microsoft.com
- Исправлена проблема, при которой нажатие в той же строке в файлах пакета не привело к выделению элемента.
- Улучшения UX для страницы "Выбор установщика"

См. [полный журнал заметок о выпуске средства упаковки MSIX](release-notes/history.md).

## <a name="share-your-feedback"></a>Оставьте отзыв

Если возникли проблемы при использовании приложения, нажмите клавиши **Windows+F**, чтобы открыть **Центр отзывов**. Предоставьте как можно больше сведений, чтобы мы могли выполнить диагностику и устранить проблему. Используйте программу Category **Applications**  >  **MSIX Packaging Tool** , чтобы получить обратную связь непосредственно с нами.

Вы также можете поделиться обратной связью в приложении. Щелкните "Параметры" (значок шестеренки) на начальном экране и откройте **Отзывы**, а затем выберите кнопку, которая наилучшим образом характеризует проблему. Откроется **Центр отзывов** с автоматически указанной информацией о категории. 

**Центр отзывов** также позволяет обмениваться идеями и предложениями, связанными с новыми функциями, которые вы хотели бы видеть в приложении.  

## <a name="faqs"></a>Часто задаваемые вопросы

1. Мне не пришло сообщение электронной почты с подтверждением моей регистрации в Программе предварительной оценки. 
    - Попробуйте [присоединиться к программе](https://aka.ms/MSIXPackagingPreviewProgram) еще раз.  

2. После регистрации в Программе предварительной оценки у меня нет средства упаковки MSIX версии Insider Preview на компьютере. 
    - Обновления из Microsoft Store отправляются пользователям по всему миру постепенно, поэтому с получением автоматического обновления могут возникать задержки. Вы можете активировать проверку обновлений на [странице средства](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf) в Microsoft Store. 
3. Я хочу отказаться от участия в Программе предварительной оценки средства упаковки MSIX. 
    - Сожалеем, что вы покидаете программу. Заполните [эту форму](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-NSOqDz219PqoOqk5qxQEZUMlEwNVNKMDhNUVlKOVpTRTlVWFhMMThLQy4u), чтобы отказаться от участия в программе. 
