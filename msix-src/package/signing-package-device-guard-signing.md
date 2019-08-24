---
Description: В этой статье описывается, как подписываться с помощью подписывания Device Guard.
title: Подписывание пакета MSIX с помощью подписи Device Guard
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: f373b5a67e5af6eb0c88ae45c0a2b8f6e092ad39
ms.sourcegitcommit: 2eb663be861f2eb29f4882a15a4913ced9da833a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/24/2019
ms.locfileid: "70014866"
---
# <a name="sign-an-msix-package-with-device-guard-signing"></a>Подписывание пакета MSIX с помощью подписи Device Guard

[Подписывание Device Guard](https://docs.microsoft.com/microsoft-store/device-guard-signing-portal) — это функция Device Guard, доступная в Microsoft Store для бизнеса и образовательных учреждений. Она позволяет компаниям гарантировать, что каждое приложение поступает из надежного источника. Начиная с Windows 10 Insider Preview Build 18945 можно использовать средство SignTool в Windows SDK для подписи приложений MSIX с подписыванием Device Guard. Эта поддержка позволяет легко включить вход в пакет MSIX для создания и подписи рабочих процессов.

Для подписи Device Guard требуются разрешения в Microsoft Store для бизнеса и используется проверка подлинности Azure Active Directory (AD). Чтобы подписать пакет MSIX с помощью подписи Device Guard, выполните следующие действия.

1. Если вы еще не сделали этого, [Зарегистрируйтесь для Microsoft Store для бизнеса или Microsoft Store для образовательных учреждений](https://docs.microsoft.com/microsoft-store/sign-up-microsoft-store-for-business).
    > [!NOTE]
    > Этот портал необходимо использовать только для настройки разрешений для подписи Device Guard.
2. В Microsoft Store для бизнеса (или Microsoft Store для образования) назначьте себе роль с разрешениями, необходимыми для подписывания Device Guard.
3. Зарегистрируйте приложение в [портал Azure](https://portal.azure.com/) с соответствующими параметрами, чтобы можно было использовать проверку подлинности Azure AD с Microsoft Store для бизнеса.
4. Получите маркер доступа Azure AD в формате JSON.
5. Запустите средство SignTool, чтобы подписать пакет MSIX с помощью подписи Device Guard, и передайте маркер доступа Azure AD, полученный на предыдущем шаге.

В следующих разделах эти действия описаны более подробно.

## <a name="configure-permissions-for-device-guard-signing"></a>Настройка разрешений для подписи Device Guard

Чтобы использовать подписывание Device Guard в Microsoft Store для бизнеса или Microsoft Store для образования, требуется роль " **подписавший" Device Guard** . Это роль минимальных привилегий, которая может подписываться. Также можно подписывать другие роли, такие как **глобальный администратор** и **Владелец учетной записи выставления счетов** .

Чтобы подтвердить или переназначить роли:

1. Войдите в [Microsoft Store для бизнеса](https://businessstore.microsoft.com/).
2. Выберите **Управление** , а затем щелкните **разрешения**.
3. Просмотр **ролей**.

Подробнее: [Роли и разрешения в Microsoft Store для бизнеса и образования](https://docs.microsoft.com/microsoft-store/roles-and-permissions-microsoft-store-for-business).

## <a name="register-your-app-in-the-azure-portal"></a>Регистрация приложения на портале Azure

Чтобы зарегистрировать приложение с соответствующими параметрами, чтобы можно было использовать аутентификацию Azure AD с Microsoft Store для бизнеса:

1. Войдите в [портал Azure](https://portal.azure.com/) и следуйте инструкциям в [кратком руководстве: Зарегистрируйте приложение на платформе](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app) удостоверений Майкрософт, чтобы зарегистрировать приложение, которое будет использовать подписывание Device Guard.

    > [!NOTE]
    > В разделе **URI перенаправления** рекомендуется выбрать общедоступный **клиент (мобильный & Рабочий стол)** . В противном случае, если выбран вариант " **Интернет** " для типа приложения, необходимо указать [секрет клиента](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-credentials-to-your-web-application) при получении маркера доступа Azure AD позже в этом процессе.

2. После регистрации приложения на главной странице приложения в портал Azure щелкните **разрешения API** и добавьте разрешение для **API магазина Windows для бизнеса**.

3. Затем выберите **делегированные разрешения** и щелкните **user_impersonation**.

## <a name="get-an-azure-ad-access-token"></a>Получение маркера доступа Azure AD

Затем получите маркер доступа Azure AD для приложения Azure AD в формате JSON. Это можно сделать с помощью различных языков программирования и сценариев. Дополнительные сведения об этом процессе см. [в разделе Авторизация доступа к Azure Active Directory веб-приложениям с помощью потока предоставления кода OAuth 2,0](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Рекомендуется получить [маркер обновления](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code#refreshing-the-access-tokens) вместе с маркером доступа, так как срок действия маркера доступа истечет через один час.

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

## <a name="sign-your-package"></a>Подписать пакет

После получения маркера доступа Azure AD вы можете использовать средство SignTool для подписания пакета с помощью подписи Device Guard. Дополнительные сведения об использовании средства SignTool для подписания пакетов см. в статье [Подписывание пакета приложения с помощью средства SignTool](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#prerequisites).

В следующем примере командной строки показано, как подписать пакет с помощью подписи Device Guard.

```cmd
signtool sign /fd sha256 /dlib DgssLib.dll /dmdf <Azure AAD in .json format> /t <timestamp-service-url> <your .msix package>
```

> [!NOTE]
> * При подписывании пакета рекомендуется использовать один из параметров метки времени. Если вы не применяете [метку времени](signing-package-overview.md#timestamping), срок действия подписи истечет через один год, и приложение потребуется повторно подписать.
> * Убедитесь, что имя издателя в манифесте пакета совпадает с сертификатом, который используется для подписания пакета. Эта функция будет представлять собой конечный сертификат. Например, если конечным сертификатом является **CompanyName**, имя издателя в манифесте должно быть **CN = CompanyName**. В противном случае операция подписывания завершится ошибкой.
> * Поддерживается только алгоритм SHA256.
> * При подписывании пакета с помощью подписи Device Guard пакет не отправляется через Интернет.

## <a name="test"></a>Тест

Чтобы проверить подпись Device Guard, скачайте корневой сертификат организацию с портала Microsoft Store для бизнеса.

1. Войдите в [Microsoft Store для бизнеса](https://businessstore.microsoft.com/).
2. Выберите **Управление** , а затем щелкните **Параметры**.
3. Просмотр **устройств**.
4. Просмотр **Скачайте корневой сертификат организации для использования с Device Guard**
5. Нажмите кнопку **скачать** .

Разверните этот сертификат на устройстве. Установите новое подписанное приложение, чтобы убедиться, что приложение успешно подписано с помощью подписи Device Guard.

## <a name="common-errors"></a>Распространенные ошибки

Ниже приведены распространенные ошибки, которые могут возникнуть.

* 0x800700d: Эта распространенная ошибка означает, что формат JSON-файла Azure AD недопустим.
