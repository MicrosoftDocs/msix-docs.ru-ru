---
Description: С помощью Azure Pipelines можно создавать автоматизированные сборки для проекта MSIX.
title: Настройка конвейера CI/CD для автоматизации сборок MSIX и их развертывания
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 42f2486f0575cb1d7d15614ce65765388eaaa940
ms.sourcegitcommit: e650c86433c731d62557b31248c7e36fd90b381d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/02/2020
ms.locfileid: "82726443"
---
# <a name="set-up-a-cicd-pipeline-to-automate-your-msix-builds-and-deployments"></a>Настройка конвейера CI/CD для автоматизации сборок MSIX и их развертывания

С помощью Azure Pipelines можно создавать автоматизированные сборки для проекта MSIX. В этой статье рассматривается, как это сделать в Azure DevOps. Мы также покажем, как выполнить эти задачи с помощью командной строки, чтобы взаимодействовать с другими системами сборки.

## <a name="create-a-new-azure-pipeline"></a>Создание конвейера Azure

[Зарегистрируйтесь в Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up), если вы этого еще не сделали.

Затем создайте конвейер, который можно использовать для сборки исходного кода. Учебник по созданию конвейера для создания репозитория GitHub см. в статье [Create your first pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml) (Создание первого конвейера). Предложение Azure Pipelines поддерживает типы репозиториев, перечисленные [в этой статье](https://docs.microsoft.com/azure/devops/pipelines/repos).

Чтобы настроить фактический конвейер сборки, перейдите на портал Azure DevOps по адресу dev.azure.com/<organization> и создайте проект. Если у вас нет учетной записи, ее можно создать бесплатно. После входа и создания проекта можно отправить исходный код в репозиторий GIT, который будет настроен по адресу https://<organization>@dev.azure.com/\<organization>/<project>/_git/<project>, или использовать любой другой поставщик, например GitHub. Вы можете выбрать расположение для своего репозитория при создании конвейера на портале, нажав сначала кнопку "Конвейеры", а затем "Создать конвейер".

На экране настройки, который появляется далее, выберите параметр "Существующий файл YAML Azure Pipelines", а затем выберите путь к зарегистрированному файлу YAML в репозитории.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Добавление сертификата проекта в библиотеку защищенных файлов

> [!NOTE]
>Не следует отправлять сертификаты в репозиторий, если это возможно. По умолчанию GIT игнорирует их. Для управления безопасной обработкой конфиденциальных файлов, таких как сертификаты, Azure DevOps поддерживает функцию [защищенных файлов](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops).

Чтобы отправить сертификат для автоматизированной сборки, выполните следующие действия.

1. В Azure Pipelines разверните узел **Конвейеры** в области навигации и щелкните элемент **Библиотека**.
2. Перейдите на вкладку **Защищенные файлы** и щелкните **+ Secure file** (+ Защищенный файл).

  

3. Перейдите к файлу сертификата и нажмите кнопку **ОК**.
4. После отправки сертификата выберите его и просмотрите его свойства. В разделе **Разрешения конвейера** включите переключатель **Авторизовать использование во всех конвейерах**.

5. Если у закрытого ключа в сертификате есть пароль, рекомендуется сохранить пароль в [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates), а затем связать пароль с [группой переменных](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Для доступа к паролю из конвейера можно использовать переменную. Обратите внимание, что пароль поддерживается только для закрытого ключа. Использование файла сертификата, защищенного паролем, сейчас не поддерживается.

> [!NOTE]
> Начиная с Visual Studio 2019, временный сертификат больше не создается в проектах MSIX. Чтобы создать или экспортировать сертификаты, используйте командлеты PowerShell, описанные в [этой статье](/windows/msix/package/create-certificate-package-signing).


## <a name="configure-the-build-in-your-yaml-file"></a>Настройка сборки в файле YAML

В таблице ниже перечислены различные аргументы MSBuild, которые можно определить для настройки конвейера сборки.

|**Аргумент MSBuild**|**Значение**|**Описание**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Определяет папку для хранения созданных артефактов. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Позволяет указать платформы, в которые будет включен пакет. |
| AppxBundle | Всегда | Создает объединенный пакет MSIXBUNDLE или APPXBUNDLE с файлами MSIX или APPX для указанной платформы. |
| UapAppxPackageBuildMode | StoreUpload | Создает файл MSIXBUNDLE или APPXBUNDLE и папку **_Test** для загрузки неопубликованных приложений. |
| UapAppxPackageBuildMode | CI | Создает только файл MSIXBUNDLE или APPXBUNDLE. |
| UapAppxPackageBuildMode | SideloadOnly | Создает папку **_Test** только для загрузки неопубликованных приложений. |
| AppxPackageSigningEnabled | верно | Включает подписывание пакетов. |
| PackageCertificateThumbprint | Отпечаток сертификата | Это значение **должно** соответствовать отпечатку в сертификате для подписи, или строка должна быть пустой. |
| PackageCertificateKeyFile | Путь | Путь к файлу сертификата. Это значение извлекается из метаданных защищенного файла. |
| PackageCertificatePassword | Пароль | Пароль для закрытого ключа в сертификате. Рекомендуется сохранить пароль в [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) и связать пароль с [группой переменных](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Можно передать переменную в этот аргумент. |



Прежде чем создавать проект упаковки так же, как в мастере Visual Studio (с помощью командной строки MSBuild), процесс сборки может назначить версию создаваемого пакета MSIX, изменив атрибут Version элемента Package в файле Package.appxmanifest. В Azure Pipelines это можно сделать с помощью выражения, указав переменную счетчика, значение которой будет увеличиваться для каждой сборки, и скрипта PowerShell, который использует класс System.Xml.Linq.XDocument в .NET, чтобы изменить значения атрибута. 

Пример файла YAML, определяющего конвейер сборки MSIX

```yml
pool: 
  vmImage: windows-2019
  
variables:
  buildPlatform: 'x86'
  buildConfiguration: 'release'
  major: 1
  minor: 0
  build: 0
  revision: $[counter('rev', 0)]
  
steps:
 - powershell: |
     # Update appxmanifest. This must be done before the build.
     [xml]$manifest= get-content ".\Msix\Package.appxmanifest"
     $manifest.Package.Identity.Version = "$(major).$(minor).$(build).$(revision)"    
     $manifest.save("Msix/Package.appxmanifest")
  displayName: 'Version Package Manifest'
  
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp
     /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:AppxPackageOutput=$(Build.ArtifactStagingDirectory)\MsixDesktopApp.msix /p:AppxPackageSigningEnabled=false'
  displayName: 'Package the App'
  
- task: DownloadSecureFile@1
  inputs:
    secureFile: 'certificate.pfx'
  displayName: 'Download Secure PFX File'
  
- script: '"C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\signtool"
    sign /fd SHA256 /f $(Agent.TempDirectory)/certificate.pfx /p secret $(
    Build.ArtifactStagingDirectory)/MsixDesktopApp.msix'
  displayName: 'Sign MSIX Package'
  
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
```




Ниже приведена разбивка различных задач сборки, определенных в файле YAML.

### <a name="configure-package-generation-properties"></a>Настройка свойств создания пакета

Определение ниже задает каталог компонентов сборки, платформу и указывает, следует ли выполнять сборку пакета. 

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\"
/p:UapAppxPackageBuildMode=SideLoadOnly
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Never
```

### <a name="configure-package-signing"></a>Настройка подписывания пакета

Чтобы подписать пакет MSIX (или APPX), конвейеру необходимо получить сертификат для подписи. Для этого добавьте задачу DownloadSecureFile перед задачей VSBuild.
Это обеспечит доступ к сертификату для подписи с помощью ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Затем обновите задачу MSBuild, чтобы она ссылалась на сертификат для подписи:

```yml
- task: MSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Never 
                  p:UapAppxPackageBuildMode=SideLoadOnly 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> В качестве меры предосторожности для аргумента PackageCertificateThumbprint намеренно задана пустая строка. Если отпечаток задан в проекте, но не соответствует сертификату для подписи, сборка завершится ошибкой: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Проверка параметров

Параметры, определенные с использованием синтаксиса `$()` — это переменные, заданные в определении сборки, которые меняются в других системах сборки.


Все предопределенные переменные см. в статье [Use predefined variables](https://docs.microsoft.com/azure/devops/pipelines/build/variables) (Предварительно заданные переменные сборки).

## <a name="configure-the-publish-build-artifacts-task"></a>Настройка задачи публикации артефактов сборки

Конвейер MSIX по умолчанию не сохраняет созданные артефакты. Чтобы добавить возможность публикации в определение YAML, добавьте следующие задачи.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

Созданные артефакты можно просмотреть в параметре **Артефакты** на странице результатов сборки.


## <a name="appinstaller-file-for-non-store-distribution"></a>Файл Установщика приложений для распространения по сторонним каналам


Если вы распространяете приложение за пределами Магазина, для установки и обновления пакета можно воспользоваться файлом Установщика приложений.

Файл APPINSTALLER будет искать обновленные файлы в репозитории \\server\foo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller xmlns="http://schemas.microsoft.com/appx/appinstaller/2018"
              Version="1.0.0.0"
              Uri="\\server\foo\MsixDesktopApp.appinstaller">
  <MainPackage Name="MyCompany.MySampleApp"
               Publisher="CN=MyCompany, O=MyCompany, L=Stockholm, S=N/A, C=Sweden"
               Version="1.0.0.0"
               Uri="\\server\foo\MsixDesktopApp.msix"
               ProcessorArchitecture="x86"/>
  <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0" />
  </UpdateSettings>
</AppInstaller>
```

Элемент UpdateSettings сообщает системе периодичность проверки наличия обновлений, а также о необходимости принудительного обновления пользователем. Полный справочник по схемам, в котором также содержится список поддерживаемых пространств имен для каждой версии Windows 10, можно найти в документах по адресу bit.ly/2TGWnCR.

Если добавить файл APPINSTALLER в проект упаковки и задать для свойства "Действие пакета" значение "Содержимое", а для свойства "Копировать в выходной каталог" — "Копировать более позднюю версию", то можно добавить в файл YAML еще одну задачу PowerShell, которая обновляет атрибуты версии корневого элемента и элемента MainPackage и сохраняет обновленный файл в промежуточном каталоге:

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $doc = [System.Xml.Linq.XDocument]::Load(
    "$(Build.SourcesDirectory)/Msix/Package.appinstaller")
  $version = "$(major).$(minor).$(build).$(revision)"
  $doc.Root.Attribute("Version").Value = $version;
  $xName =
    [System.Xml.Linq.XName]
      "{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Version").Value = $version;
  $doc.Save("$(Build.ArtifactStagingDirectory)/MsixDesktopApp.appinstaller")
displayName: 'Version App Installer File'
```

Затем необходимо распространить файл APPINSTALLER среди пользователей, чтобы затем они могли дважды щелкнуть его вместо файла MSIX для установки упакованного приложения.

## <a name="continuous-deployment"></a>Непрерывное развертывание

Файл Установщика приложений является неоткомпилированным файлом XML, который можно изменить после сборки, если это необходимо. Это упрощает использование при развертывании программного обеспечения в нескольких средах и при необходимости отделения конвейера сборки от процесса выпуска.

Если вы создадите конвейер выпуска на портале Azure с помощью шаблона "Пустое задание" и используете недавно настроенный конвейер сборки в качестве источника артефакта, можно добавить задачу PowerShell на этапе выпуска, чтобы значения двух атрибутов кода URI в файле APPINSTALLER динамически изменялись, указывая расположение публикации приложения.

Ниже приведена задача конвейера выпуска, которая изменяет коды URI в файле APPINSTALLER:

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $fileShare = "\\filesharestorageccount.file.core.windows.net\myfileshare\"
  $localFilePath =
    "$(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop\MsixDesktopApp.appinstaller"
  $doc = [System.Xml.Linq.XDocument]::Load("$localFilePath")
  $doc.Root.Attribute("Uri").Value = [string]::Format('{0}{1}', $fileShare,
    'MsixDesktopApp.appinstaller')
  $xName =
    [System.Xml.Linq.XName]"{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Uri").Value = [string]::Format('{0}{1}',
    $fileShare, 'MsixDesktopApp.appx')
  $doc.Save("$localFilePath")
displayName: 'Modify URIs in App Installer File'
```

В задаче выше в качестве кода URI указан UNC-путь к общей папке Azure. В этом случае операционная система будет искать пакет MSIX при установке и обновлении приложения, поэтому в конвейер выпуска также добавлен еще один скрипт командной строки, который сначала сопоставляет общую папку в облаке с локальным диском Z:\ в агенте сборки, а затем использует команду xcopy, чтобы копировать на этот диск файлы APPINSTALLER и MSIX:

```yml
- script: |
  net use Z: \\filesharestorageccount.file.core.windows.net\myfileshare
    /u:AZURE\filesharestorageccount
    3PTYC+ociHIwNgCnyg7zsWoKBxRmkEc4Aew4FMzbpUl/
    dydo/3HVnl71XPe0uWxQcLddEUuq0fN8Ltcpc0LYeg==
  xcopy $(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop Z:\ /Y
  displayName: 'Publish App Installer File and MSIX package'
```

Если у вас есть собственный локальный сервер Azure DevOps Server, можно опубликовать файлы во внутренней сетевой папке.

В случае публикации на веб-сервере с помощью MSBuild можно создать файл APPINSTALLER с контролем версий и страницу HTML, содержащую ссылку для скачивания, а также некоторые сведения об упакованном приложении, указав несколько дополнительных аргументов в файле YAML:

```yml
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:GenerateAppInstallerFile=True
/p:AppInstallerUri=http://yourwebsite.com/packages/ /p:AppInstallerCheckForUpdateFrequency=OnApplicationRun /p:AppInstallerUpdateFrequency=1 /p:AppxPackageDir=$(Build.ArtifactStagingDirectory)/'
  displayName: 'Package the App'
  ```
  
Созданный HTML-файл будет содержать гиперссылку с префиксом схемы активации протокола ms-appinstaller, независящей от браузера:

```html
<a href="ms-appinstaller:?source=
  http://yourwebsite.com/packages/Msix_x86.appinstaller ">Install App</a>
```

Если вы настроили конвейер выпуска, который публикует содержимое папки сброса в интрасети или на любом другом веб-сайте, а веб-сервер поддерживает запросы байтового диапазона и правильно настроен, пользователи смогут использовать эту ссылку для прямой установки приложения без предварительного скачивания пакета MSIX.


