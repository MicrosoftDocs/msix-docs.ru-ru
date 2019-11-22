---
Description: Вы можете выполнять сценарии с помощью платформы поддержки пакетов, чтобы настроить классическое приложение для пользовательской среды.
title: Выполнение сценариев с помощью платформы поддержки пакетов
ms.date: 09/20/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 39a4731daece164b89f2c2df1df1965c2c0543e3
ms.sourcegitcommit: 073a228653f004914851c3461b9ad6eef343f915
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2019
ms.locfileid: "74309009"
---
# <a name="run-scripts-with-the-package-support-framework"></a>Выполнение сценариев с помощью платформы поддержки пакетов

Сценарии позволяют ИТ-специалистам динамически настраивать приложение в среде пользователя после его упаковки с помощью MSIX. Например, можно использовать сценарии для настройки базы данных, настройки VPN, подключения общего диска или проверки лицензии динамически. Сценарии обеспечивают большую гибкость. Они могут изменять разделы реестра или выполнять изменения файлов на основе конфигурации компьютера или сервера.

Можно использовать платформу поддержки пакетов (ПСФ) для запуска одного скрипта PowerShell перед запуском исполняемого файла приложения, а также один сценарий PowerShell после запуска исполняемого файла приложения для очистки. Каждый исполняемый файл приложения, определенный в манифесте приложения, может иметь собственные скрипты. Можно настроить выполнение скрипта только один раз при запуске первого приложения и без отображения окна PowerShell, чтобы пользователи не запускали сценарий преждевременно по ошибке. Существуют и другие варианты настройки способа выполнения сценариев, показанные ниже.

## <a name="prerequisites"></a>Предварительные условия

Чтобы включить скрипты для выполнения, необходимо задать для политики выполнения PowerShell значение `RemoteSigned`. Это можно сделать, выполнив следующую команду:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

Необходимо задать политику выполнения для исполняемого файла PowerShell 64-bit и исполняемого файла PowerShell 32-bit. Обязательно Откройте каждую версию PowerShell и выполните одну из приведенных выше команд.

Ниже приведены расположения каждого исполняемого файла.

* 64-разрядный компьютер:
  * 64-разрядный исполняемый файл:%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
  * 32-разрядный исполняемый файл:%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
* 32-разрядный компьютер:
  * 32-разрядный исполняемый файл:%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe

Дополнительные сведения о политиках выполнения PowerShell см. в [этой статье](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6).

Убедитесь, что в пакет включен следующий "Стартингскриптвраппер. ps1", и укажите его в той же папке, где находится исполняемый файл. Его можно скопировать из пакета NuGet платформы поддержки пакетов (https://www.nuget.org/packages/Microsoft.PackageSupportFramework/).

## <a name="enable-scripts"></a>Включить скрипты

Чтобы указать, какие скрипты будут выполняться для каждого исполняемого файла упакованного приложения, необходимо изменить [файл config. JSON](package-support-framework.md#create-a-configuration-file). Чтобы указать ПСФ выполнить сценарий перед выполнением упакованного приложения, добавьте элемент конфигурации с именем `startScript`. Чтобы сообщить ПСФ о выполнении сценария после завершения работы упакованного приложения, добавьте элемент конфигурации с именем `endScript`.

### <a name="script-configuration-items"></a>Создать скрипт для элементов конфигурации

Ниже приведены элементы конфигурации, доступные для скриптов. Конечный скрипт игнорирует элементы конфигурации `waitForScriptToFinish` и `stopOnScriptError`.

| Имя ключа                | Тип значения | Требуется? | По умолчанию  | Описание
|-------------------------|------------|-----------|----------|---------|
| `scriptPath`              | Строка     | Да       | Нет данных      | Путь к сценарию, включая имя и расширение. Путь начинается с корневого каталога приложения.
| `scriptArguments`         | Строка     | Нет        | пусто    | Список аргументов, разделенных пробелами. Формат для вызова скрипта PowerShell одинаков. Эта строка добавляется к `scriptPath` для создания допустимого вызова PowerShell. exe.
| `runInVirtualEnvironment` | boolean    | Нет        | true     | Указывает, должен ли сценарий запускаться в той же виртуальной среде, в которой выполняется Пакетное приложение.
| `runOnce`                 | boolean    | Нет        | true     | Указывает, должен ли сценарий запускаться один раз для каждого пользователя в каждой версии.
| `showWindow`              | boolean    | Нет        | false    | Указывает, отображается ли окно PowerShell.
| `stopOnScriptError`       | boolean    | Нет        | false    | Указывает, следует ли выходить из приложения при сбое сценария запуска.
| `waitForScriptToFinish`   | boolean    | Нет        | true     | Указывает, должно ли упакованное приложение ожидать завершения начального скрипта перед запуском.
| `timeout`                 | DWORD      | Нет        | БОЛЬШОГО | Время, в течение которого сценарий будет разрешен для выполнения. По истечении этого времени сценарий будет остановлен.

> [!NOTE]
> Установка `stopOnScriptError: true` и `waitForScriptToFinish: false` для примера приложения не поддерживается. Если задать оба этих элемента конфигурации, ПСФ возвратит ошибку ERROR_BAD_CONFIGURATION.


## <a name="sample-configuration"></a>Пример конфигурации

Ниже приведен пример конфигурации с использованием двух различных исполняемых файлов приложения.

```json
{
  "applications": [
    {
      "id": "Sample",
      "executable": "Sample.exe",
      "workingDirectory": "",
      "stopOnScriptError": false,
      "startScript":
      {
        "scriptPath": "RunMePlease.ps1",
        "scriptArguments": "\\\"First argument\\\" secondArgument",
        "runInVirtualEnvironment": true,
        "showWindow": true,
        "waitForScriptToFinish": false
      },
      "endScript":
      {
        "scriptPath": "RunMeAfter.ps1",
        "scriptArguments": "ThisIsMe.txt"
      }
    },
    {
      "id": "CPPSample",
      "executable": "CPPSample.exe",
      "workingDirectory": "",
      "startScript":
      {
        "scriptPath": "CPPStart.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runInVirtualEnvironment": true
      },
      "endScript":
      {
        "scriptPath": "CPPEnd.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runOnce": false
      }
    }
  ],
  "processes": [
    ...(taken out for brevity)
  ]
}
```
