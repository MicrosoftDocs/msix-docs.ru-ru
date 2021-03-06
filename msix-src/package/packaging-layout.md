---
title: Создание пакета с использованием макета упаковки
description: Макет упаковки — это отдельный документ, описывающий структуру упаковки приложения, например пакеты приложения (первичные и необязательные).
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, упаковка, макет пакета, пакет активов
ms.localizationpriority: medium
ms.openlocfilehash: 92e14f8d7b8e1b3a7508f673b81a7ea6bb14ea42
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328776"
---
# <a name="package-creation-with-the-packaging-layout"></a>Создание пакета с использованием макета упаковки  

С появлением пакетов активов у разработчиков появились инструменты для создания большего числа пакетов более разнообразных типов. По мере того как приложение становится все более крупным и сложным, в его состав часто входит все больше пакетов, растет сложность управления этими пакетами (особенно если сборка происходит за пределами Visual Studio или с использованием файлов сопоставления). Чтобы упростить управление структурой упаковки приложения, можно использовать макет упаковки, который поддерживается MakeAppx.exe. 

Макет упаковки — это отдельный документ XML, описывающий структуру упаковки приложения. Он задает пакеты приложений (основной и дополнительный), пакеты приложения в пакетах приложений и файлы в пакетах приложения. Можно выбирать файлы из разных папок, сетевых расположений и с разных дисков. Для выбора или исключения файлов можно использовать подстановочные знаки.

После настройки структуры упаковки для приложения она используется вместе с файлом MakeAppx.exe, чтобы создать все пакеты для приложения одним вызовом командной строки. Макет упаковки можно редактировать, чтобы изменить структуру пакета в соответствии с вашими потребностями развертывания. 


## <a name="simple-packaging-layout-example"></a>Пример простой структуры упаковки

Вот пример простой структуры упаковки:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

Разделим пример на несколько частей, чтобы понять, как все функционирует.

### <a name="packagefamily"></a>PackageFamily

В этом макете упаковки будет создан один плоский файл набора приложений с пакетом архитектуры x64 и пакетом ресурсов "Media". 

Элемент **PackageFamily** используется для определения пакета приложений. Необходимо использовать атрибут **ManifestPath**, чтобы предоставить элемент **AppxManifest** для пакета. Манифест **AppxManifest** должен соответствовать манифесту **AppxManifest** для пакета архитектуры пакета. Необходимо также указать атрибут **ID**. Он используется вместе с файлом MakeAppx.exe при создании пакета, чтобы при желании можно было создать только этот пакет. Это же будет именем файла получившегося пакета. Атрибут **FlatBundle** служит для описания типа пакета, который требуется создать: **true** — плоский пакет (см. подробную информацию здесь); **false** — классический пакет. Атрибут **ResourceManager** указывает, будут ли пакеты ресурсов внутри этого пакета использовать MRT, чтобы получить доступ к файлам. По умолчанию этот параметр имеет значение **true**, но с Windows 10 версии 1803 эта возможность еще не готова, поэтому атрибуту необходимо присваивать значение **false**.

### <a name="package-and-assetpackage"></a>Элементы Package и AssetPackage

В **PackageFamily** определяются пакеты приложения, которые содержит пакет приложений, или ссылки. Здесь определяется пакет архитектуры (основной пакет) с элементом **Package** и пакет активов с элементом **AssetPackage**. Пакет архитектуры должен указывать, для какой архитектуры предназначен пакет: x64, x86, ARM или нейтральная. Кроме того, можно (при необходимости) напрямую предоставить манифест **AppxManifest** специально для этого пакета, используя атрибут **ManifestPath** еще раз. Манифест **AppxManifest** не предоставляется, он будет создан автоматически из манифеста **AppxManifest**, предоставляемого для **PackageFamily**. 

По умолчанию **AppxManifest** будет создан для каждого пакета приложения внутри пакета приложений. Для пакета активов можно также задать атрибут **AllowExecution**. Если задать значение **false** (по умолчанию), это поможет уменьшить время публикации для вашего приложения, поскольку пакеты, выполнять которые не требуются, не заблокируют своим сканированием на вирусы процесс публикации (подробнее об этом см. в разделе [Вводные сведения о пакетах активов](asset-packages.md)). 

### <a name="files"></a>Файлы

В каждом определении пакета можно использовать элемент **File**, чтобы выбрать файлы для включения в пакет. Атрибут **SourcePath** указывает на локальное расположение файлов. Можно выбрать файлы из разных папок (указав относительные пути), с разных дисков (указав абсолютные пути) или даже сетевых папок (указав что-то вроде `\\myshare\myapp\*`). **DestinationPath** — это место, где в конце концов окажутся файлы в пакете (относительно корневого каталога пакета). **ExcludePath** можно использовать (вместо двух других атрибутов), чтобы выбрать файлы, которые необходимо исключить из файлов, выбранных атрибутом **SourcePath** других элементов **File** в том же пакете.

Каждый элемент **File** можно использовать, чтобы выбрать несколько файлов с помощью подстановочных знаков. Как правило, отдельный подстановочный знак (`*`) можно использовать в любом месте пути неограниченное число раз. Однако один подстановочный знак будет соответствовать только файлам в папке, но не вложенным папкам. Например, `C:\MyGame\*\*` можно использовать в **SourcePath**, чтобы выбрать файлы `C:\MyGame\Audios\UI.mp3` и `C:\MyGame\Videos\intro.mp4`, однако выбрать `C:\MyGame\Audios\Level1\warp.mp3` невозможно. Двойной подстановочный знак (`**`) также можно использовать вместо имен папок или файлов, чтобы установить рекурсивное соответствие элементов (использовать рядом с неполными именами его, однако, нельзя). Например, `C:\MyGame\**\Level1\**` может выбрать `C:\MyGame\Audios\Level1\warp.mp3` и `C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4`. Подстановочные знаки можно также использовать для непосредственного изменения имен файлов в процессе упаковки, если подстановочные знаки используются в разных местах источника и целевого назначения. Например, если задано `C:\MyGame\Audios\*` для **SourcePath** и `Sound\copy_*` для **DestinationPath**, можно выбрать `C:\MyGame\Audios\UI.mp3` и отображать его в пакете как `Sound\copy_UI.mp3`. Как правило, число отдельных и двойных подстановочных знаков должно быть одинаковым для **SourcePath** и **DestinationPath** отдельного элемента **File**.

## <a name="advanced-packaging-layout-example"></a>Пример сложной структуры упаковки

Вот пример более сложной структуры упаковки:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

Этот пример отличается от простого примера добавлением элементов **ResourcePackage** и **Optional**.

Пакеты ресурсов можно указать с помощью элемента **ResourcePackage**. В **ResourcePackage** элемент **Resources** необходимо использовать, чтобы задать квалификаторы ресурсов для пакета ресурсов. Квалификаторы ресурсов — это ресурсы, которые поддерживаются пакетом ресурсов. Здесь мы видим, что определено два пакета ресурсов и каждый содержит файлы для английского и французского языков. Пакет ресурсов может иметь несколько квалификаторов. Для этого достаточно добавить другой элемент **Resource** в **Resources**. Ресурс по умолчанию для измерения ресурсов также необходимо указать, если измерение существует (измерения: language, scale, dxfl). Здесь английский указан в качестве языка по умолчанию, то есть пользователи, у которых в качестве системного языка не задан французский, перейдут на загрузку пакета ресурсов для английского языка, и интерфейс для них будет отображаться на английском языке.

У каждого дополнительного пакета есть свое определенное имя семейства пакетов, его необходимо определить с помощью элементов **PackageFamily**, задав для атрибута **Optional** значение **true**. Атрибут **RelatedSet** используется, чтобы указать, находится ли дополнительный пакет внутри связанного набора (по умолчанию это так) и нужно ли обновлять дополнительный пакет вместе с основным.

Элемент **пребуилтпаккаже** используется для добавления пакетов, которые не определены в макете упаковки для включения в создаваемые файлы набора приложений или ссылки на них. В этом случае сюда добавляется другой дополнительный пакет DLC, чтобы основной файл пакета приложений мог ссылаться на него и был частью связанного набора.

## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>Создание пакетов приложений с использованием структуры упаковки и файла MakeAppx.exe

Определившись со структурой упаковки для своего приложения, можно приступать к использованию файла MakeAppx.exe для создания пакетов вашего приложения. Чтобы создать все пакеты, определенные в структуре упаковки, воспользуйтесь следующей командой:

```cmd
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

Однако если вы обновляете свое приложение и некоторые пакеты не содержат никаких измененных файлов, можно создать только пакеты, которые изменились. Если использовать пример простой структуры упаковки на этой странице и создать пакет с архитектурой x64, команда будет выглядеть следующим образом:

```cmd
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

Флаг `/id` можно использовать, чтобы выбрать пакеты, которые необходимо создать из структуры упаковки, чтобы они соответствовали атрибуту **ID** в структуре. `/ip` указывает, где в этом случае находится предыдущая версия пакетов. Должна быть указана Предыдущая версия, так как файл пакета приложений по-прежнему должен ссылаться на предыдущую версию пакет **мультимедиа** . Флаг `/iv` используется для автоматического увеличения номера версии создаваемых пакетов (вместо изменения версии в **AppxManifest**). Кроме того, коммутаторы `/pv` и `/bv` можно использовать для непосредственного предоставления версии пакета приложения (для всех создаваемых пакетов приложения) и версии пакетов приложений (для всех создаваемых пакетов приложений) соответственно.
Если используется пример сложной схемы упаковки на этой странице и требуется только создать дополнительный пакет приложений **Themes** и пакет приложения **Themes.main**, на который он ссылается, следует воспользоваться следующей командой:

```cmd
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

Флаг `/bc` указывает, что необходимо также создать дочерние объекты пакета приложений **Themes** (в этом случае будет создан **Themes.main**). Флаг `/nbp` указывает, что родительский пакет приложений **Themes** создавать не требуется. Родительский пакет пакета **Themes** (дополнительного пакета приложений) является основным пакетом приложений: **MyGame** . Обычно для дополнительного пакета в соответствующем наборе нужно также создать основной пакет приложений, чтобы можно было установить дополнительный пакет, поскольку основной пакет приложений также ссылается на дополнительный пакет, когда находится в соответствующем наборе (чтобы гарантировать эффективное управление версиями между основными и дополнительными пакетами). Взаимоотношения "родительский/дочерний" между пакетами проиллюстрированы на следующей диаграмме:

![Схема структуры пакета](images/packaging-layout.png)
