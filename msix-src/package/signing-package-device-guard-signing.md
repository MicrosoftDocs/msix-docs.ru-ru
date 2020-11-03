---
description: В этой статье описывается, как подписать пакет MSIX с помощью подписи Device Guard, что позволяет предприятиям гарантировать, что приложения поступают из надежного источника.
title: Подписывание пакета MSIX с помощью подписи Device Guard
ms.date: 10/26/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 687d2c0ab59b8a02c0c08f1f7bade7c1301666be
ms.sourcegitcommit: b907ca46c847bfff6278fdcf2af84efb6e23c3e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/02/2020
ms.locfileid: "93191508"
---
# <a name="sign-an-msix-package-with-device-guard-signing"></a>Подписывание пакета MSIX с помощью подписи Device Guard

> [!IMPORTANT]
> Теперь доступна [Служба подписывания Device Guard версии 2](https://docs.microsoft.com/microsoft-store/device-guard-signing-portal) . Как было объявлено ранее, у вас будет до конца декабря 2020 переходить на ДГСС v2. В конце декабря 2020 года существующие веб-механизмы для текущей версии службы DGSS будут удалены и больше не будут доступны для использования. Прежде чем Декабрь 2020, необходимо создать план для перехода на новую версию службы. Для получения дополнительных сведений свяжитесь с DGSSMigration@Microsoft.com. 

[Подписывание Device Guard](/microsoft-store/device-guard-signing-portal) — это функция Device Guard, доступная в Microsoft Store для бизнеса и образовательных учреждений. Она позволяет компаниям гарантировать, что каждое приложение поступает из надежного источника. Начиная с Windows 10 Insider Preview Build 18945 можно использовать средство SignTool в Windows SDK для подписи приложений MSIX с подписыванием Device Guard. Эта поддержка позволяет легко включить вход в пакет MSIX для создания и подписи рабочих процессов.

Для подписи Device Guard требуются разрешения в Microsoft Store для бизнеса и используется проверка подлинности Azure Active Directory (AD). Чтобы подписать пакет MSIX с помощью подписи Device Guard, выполните следующие действия.

1. Если вы еще не сделали этого, [Зарегистрируйтесь для Microsoft Store для бизнеса или Microsoft Store для образовательных учреждений](/microsoft-store/sign-up-microsoft-store-for-business).
    > [!NOTE]
    > Этот портал необходимо использовать только для настройки разрешений для подписи Device Guard.
2. В Microsoft Store для бизнеса (или Microsoft Store для образования) назначьте себе роль с разрешениями, необходимыми для подписывания Device Guard.
3. Зарегистрируйте приложение в [портал Azure](https://portal.azure.com/) с соответствующими параметрами, чтобы можно было использовать проверку подлинности Azure AD с Microsoft Store для бизнеса.
4. Получите маркер доступа Azure AD в формате JSON.
5. Запустите средство SignTool, чтобы подписать пакет MSIX с помощью подписи Device Guard, и передайте маркер доступа Azure AD, полученный на предыдущем шаге.

В следующих разделах эти действия описаны более подробно.

## <a name="configure-permissions-for-device-guard-signing"></a>Настройка разрешений для подписи Device Guard

Чтобы использовать подписывание Device Guard в Microsoft Store для бизнеса или Microsoft Store для образования, требуется роль " **подписавший" Device Guard** . Это роль минимальных привилегий, которая может подписываться. Также можно подписывать другие роли, такие как **глобальный администратор** и **Владелец учетной записи выставления счетов** . 

 > [!NOTE]
 > Роль подписи Device Guard используется при подписывании в качестве приложения. Владелец учетной записи глобального администратора и счета выставления счетов используется при входе в систему в качестве пользователя, выполнившего вход.

Чтобы подтвердить или переназначить роли:

1. Войдите в [Microsoft Store для бизнеса](https://businessstore.microsoft.com/).
2. Выберите **Управление** , а затем щелкните **разрешения** .
3. Просмотр **ролей** .

Подробнее: [Роли и разрешения в Microsoft Store для бизнеса и образования](/microsoft-store/roles-and-permissions-microsoft-store-for-business).

## <a name="register-your-app-in-the-azure-portal"></a>Регистрация приложения на портале Azure

Чтобы зарегистрировать приложение с соответствующими параметрами, чтобы можно было использовать аутентификацию Azure AD с Microsoft Store для бизнеса:

1. Войдите в [портал Azure](https://portal.azure.com/) и следуйте инструкциям в [кратком руководстве по регистрации приложения на платформе Microsoft Identity](/azure/active-directory/develop/quickstart-register-app) , чтобы зарегистрировать приложение, которое будет использовать подписывание Device Guard.

    > [!NOTE]
    > В разделе **URI перенаправления** рекомендуется выбрать **общедоступный клиент (мобильный & Рабочий стол)** . В противном случае, если выбран вариант " **Интернет** " для типа приложения, необходимо указать [секрет клиента](/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-credentials-to-your-web-application) при получении маркера доступа Azure AD позже в этом процессе.

2. После регистрации приложения на главной странице приложения в портал Azure щелкните **разрешения API** , в разделе **API-интерфейсы, используемые моей организацией** , а затем добавьте разрешение для **API магазина Windows для бизнеса** .

3. Затем выберите **делегированные разрешения** и щелкните **user_impersonation** .

## <a name="get-an-azure-ad-access-token"></a>Получение токена доступа Azure AD

Затем получите маркер доступа Azure AD для приложения Azure AD в формате JSON. Это можно сделать с помощью различных языков программирования и сценариев. Дополнительные сведения об этом процессе см. [в разделе Авторизация доступа к Azure Active Directory веб-приложениям с помощью потока предоставления кода OAuth 2,0](/azure/active-directory/develop/v1-protocols-oauth-code). Рекомендуется получить [маркер обновления](/azure/active-directory/develop/v1-protocols-oauth-code#refreshing-the-access-tokens) вместе с маркером доступа, так как срок действия маркера доступа истечет через один час.

> [!NOTE]
> Если приложение зарегистрировано как **веб-** приложение в портал Azure, необходимо указать секрет клиента при запросе токена. Дополнительные сведения см. в предыдущем разделе.

В следующем примере PowerShell показано, как запросить маркер доступа.

```powershell
function GetToken()
{

    $c = Get-Credential -Credential $user
    
    $Credentials = New-Object System.Management.Automation.PSCredential -ArgumentList $c.UserName, $c.password
    $user = $Credentials.UserName
    $password = $Credentials.GetNetworkCredential().Password
    
    $tokenCache = "outfile.json"

    #replace <application-id> and <client_secret-id> with the Application ID from your Azure AD application registration
    $Body = @{
      'grant_type' = 'password'
      'client_id'= '<application-id>'
      'client_secret' = '<client_secret>'
      'resource' = 'https://onestore.microsoft.com'
      'username' = $user
      'password' = $password
    }

    $webpage = Invoke-WebRequest 'https://login.microsoftonline.com/common/oauth2/token' -Method 'POST'  -Body $Body -UseBasicParsing
    $webpage.Content | Out-File $tokenCache -Encoding ascii
}
```

> [!NOTE]
> Мы выполним команду, которая сохраняет файл JSON для последующего использования.

## <a name="obtain-the-device-guard-signing-version-2-dll"></a>Получение библиотеки DLL для подписания Device Guard версии 2
Чтобы подписать подписывание Device Guard версии 2, получите **Microsoft.Acs.Dlib.dll** , загрузив [пакет NuGet](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client/) , который будет использоваться для подписания пакета. Это также необходимо для получения корневого сертификата. 

## <a name="sign-your-package"></a>Подписать пакет

После получения маркера доступа Azure AD вы можете использовать средство SignTool для подписания пакета с помощью подписи Device Guard. Дополнительные сведения об использовании средства SignTool для подписания пакетов см. в статье [Подписывание пакета приложения с помощью средства SignTool](/windows/uwp/packaging/sign-app-package-using-signtool?context=%252fwindows%252fmsix%252frender#prerequisites).

В следующем примере командной строки показано, как подписать пакет с помощью подписывания Device Guard версии 2.

```cmd
signtool sign /fd sha256 /dlib Microsoft.Acs.Dlib.dll /dmdf <Azure AAD in .json format> /t <timestamp-service-url> <your .msix package>
```

В следующем примере командной строки показано, как подписать с помощью подписывания Device Guard версии 1. Обратите внимание, что это будет считаться устаревшим до окончания декабря 2020.
```cmd
signtool sign /fd sha256 /dlib DgssLib.dll /dmdf <Azure AAD in .json format> /t <timestamp-service-url> <your .msix package>
```

> [!NOTE]
> * При подписывании пакета рекомендуется использовать один из параметров метки времени. Если вы не применяете [метку времени](signing-package-overview.md#timestamping), срок действия подписи истечет через один год, и приложение потребуется повторно подписать.
> * Убедитесь, что имя издателя в манифесте пакета совпадает с сертификатом, который используется для подписания пакета. Эта функция будет представлять собой конечный сертификат. Например, если конечным сертификатом является **CompanyName** , имя издателя в манифесте должно быть **CN = CompanyName** . В противном случае операция подписывания завершится ошибкой.
> * Поддерживается только алгоритм SHA256.
> * При подписывании пакета с помощью подписи Device Guard пакет не отправляется через Интернет.

## <a name="test"></a>Тест

Чтобы проверить, скачайте корневой сертификат, загрузив [пакет NuGet](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client/) и получая его с помощью команды:
```cmd
Get-RootCertificate
```

Установите корневой сертификат в **список доверенных корневых центров сертификации** на устройстве. Установите новое подписанное приложение, чтобы убедиться, что приложение успешно подписано с помощью подписи Device Guard. 

> [!NOTE]
> Рекомендуется использовать политику CI для дальнейшей изоляции. Обязательно ознакомьтесь с документацией по readme_cmdlets и переносом из DGSSv1 в документацию по DGSSv2, которая включена в пакет NuGet. 

## <a name="common-errors"></a>Распространенные ошибки

Ниже приведены распространенные ошибки, которые могут возникнуть.

* 0x800700d: Эта распространенная ошибка означает, что формат JSON-файла Azure AD недопустим.
* Перед скачиванием корневого сертификата подписывания Device Guard может потребоваться принять условия Microsoft Store для бизнеса. Это можно сделать, приобретя бесплатное приложение на портале.
