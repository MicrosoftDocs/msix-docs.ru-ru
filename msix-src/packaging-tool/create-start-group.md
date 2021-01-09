---
title: Группировка приложений в папке в меню "Пуск"
description: Описывается, как включить несколько упакованных MSIX приложений, сгруппированных в одну папку в меню "Пуск".
ms.date: 12/02/2020
ms.topic: article
keywords: Windows 10, UWP, msix, Appx
ms.localizationpriority: medium
ms.openlocfilehash: eb7202b5cef5a50f8dbe52c61be4bb581d8a8cce
ms.sourcegitcommit: 3a9aae783331833bbf244091544c48848768137e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98043311"
---
# <a name="group-applications-under-a-folder-in-the-start-menu"></a>Группировка приложений в папке в меню "Пуск"

> [!IMPORTANT]
> В настоящее время эта функция доступна в предварительных сборках Windows 10, распространяемых через круг разработчиков [программы предварительной оценки Windows](https://insider.windows.com/en/). Для включения этой функции потребуется по крайней мере сборка 20257.

Манифест упакованного приложения MSIX содержит одну или несколько `<Application>` записей, которые являются доступными точками входа. Каждый из них станет значком в меню "Пуск".

Пакет MSIX может содержать несколько приложений. Кроме того, компания может создавать несколько приложений, которые упаковываются как отдельные пакеты MSIX, но все они принадлежат одному и тому же набору.
В обоих случаях может потребоваться сгруппировать все записи в меню Пуск в одной папке, чтобы пользователю было проще найти все приложения в одном месте.

Эта цель может быть достигнута с помощью `VisualGroup` свойства `VisualElements` элемента.
Ниже приведены действия по реализации этого изменения.

1) Откройте файл манифеста приложения в текстовом редакторе по выбору. Кроме того, если вы используете средство упаковки MSIX, можно нажать кнопку *открыть манифест* в редакторе пакетов.
2) Убедитесь, что `uap3` пространство имен объявлено в `<Package>` узле манифеста:

    ```xml
    <Package ...
         xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"  
         IgnorableNamespaces="... uap3">
        ...
   </Package>
    ```

3) Найдите раздел `Applications`. Внутри вы найдете одну или несколько `Application` записей, по одной для каждого значка, который будет создан в меню "Пуск". Вот как это будет выглядеть:

    ```xml
      <Applications>
          <Application>
              <VisualElements DisplayName="App1" 
                              Square150x150Logo="images/150x150.png"
                              Square44x44Logo="images/44x44.png"
                              Description="App1"
                              BackgroundColor="#777777"
                              AppListEntry="default">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

4) Добавьте `uap3` префикс в `VisualElements` раздел. Не забудьте добавить его в открывающий и закрывающий теги:

    ```xml
      <Applications>
          <Application>
              <uap3:VisualElements DisplayName="App1"
                                   Square150x150Logo="images/150x150.png"
                                   Square44x44Logo="images/44x44.png"
                                   Description="App1"
                                   BackgroundColor="#777777"
                                   AppListEntry="default">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </uap3:VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

5) Наконец, добавьте `VisualGroup` атрибут к `VisualElements` элементу. В качестве значения задайте имя папки, которая будет создана в меню "Пуск".

    ```xml
      <Applications>
          <Application>
              <uap3:VisualElements DisplayName="App1"
                                   Square150x150Logo="images/150x150.png"
                                   Square44x44Logo="images/44x44.png"
                                   Description="App1"
                                   BackgroundColor="#777777"
                                   AppListEntry="default"
                                   VisualGroup="MyFolder">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </uap3:VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

Теперь можно повторить процесс для всех остальных `<Application>` записей, которые необходимо включить в одну и ту же папку. При необходимости можно также сделать то же самое с другими приложениями, просто отредактировав файл манифеста, включенный в пакет MSIX, таким же образом и используя то же значение `VisualGroup` атрибута.
