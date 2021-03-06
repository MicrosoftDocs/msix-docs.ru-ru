---
title: Подписывание пакетов с помощью Azure Key Vault
description: В статье показано, как подписать пакет приложения с помощью сертификата из Azure Key Vault.
ms.date: 05/07/2020
ms.topic: article
keywords: Windows 10, MSIX, UWP, Azure Key Vault, Visual Studio
ms.localizationpriority: medium
ms.openlocfilehash: ac69ae105dbd1fff5d64c20f50af644f9d27504f
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090432"
---
# <a name="sign-packages-with-azure-key-vault"></a>Подписывание пакетов с помощью Azure Key Vault

В Visual Studio 2019 16.6 (предварительная версия 3) и последующих версий пакеты UWP и классических приложений можно подписывать с помощью сертификата, хранящегося в Azure Key Vault, в сценариях разработки и тестирования. Средство извлекает открытые и закрытые ключи из Azure Key Vault и загружает их в хранилище сертификатов на компьютере разработки, чтобы подписать пакет с помощью SignTool.exe.

> [!IMPORTANT]
> Описанный в этой статье процесс относится только к сценариям разработки и тестирования. Его не рекомендуется использовать для закрытых ключей, предназначенных для распространения. Чтобы обеспечить высокий уровень безопасности, закрытые ключи для распространения следует обрабатывать только теми инструментами, которые рекомендуются для используемой вами платформы непрерывной интеграции и непрерывного развертывания (CI/CD).

## <a name="prerequisites"></a>Предварительные условия

- Учетная запись Azure. Если у вас еще нет учетной записи, зарегистрируйте ее [здесь](https://azure.microsoft.com/free/).
- Azure Key Vault. Дополнительные сведения см. в разделе [Создание хранилища](/azure/key-vault/secrets/quick-create-portal#create-a-vault).
- Действительный сертификат для подписывания пакета, импортированный в Azure Key Vault. Сертификат, который создается в Azure Key Vault по умолчанию, не подходит для этого. Дополнительные сведения см. в статье [Создание сертификата для подписывания пакета](../package/create-certificate-package-signing.md).

## <a name="import-a-certificate-to-your-key-vault"></a>Импорт сертификата в Key Vault

Добавить сертификат в Key Vault очень просто. В этом примере мы добавим действительный сертификат для подписывания кода UWP с именем **UwpSigningCert.pfx**.

1. На страницах свойств Key Vault выберите **Сертификаты**.
2. Нажмите **Generate/Import** (Создать или импортировать).
3. На экране **Создание сертификата** выберите следующие значения:
    - **Метод создания сертификата**: Импорт
    - **Имя сертификата**: UwpSigningCert.
    - **Отправка файла сертификата**: UwpSigningCert.pfx.
    - **Decrypt Certificate** (Расшифровка сертификата): если сертификат защищен паролем, укажите его в поле **Пароль**.
4. Нажмите кнопку **Create** (Создать).

> [!NOTE]
> Чтобы этот самозаверяющий сертификат стал доверенным для Windows, импортировать и сделать его доверенным следует администратору. Обеспечьте безопасность всех сертификатов, включая самозаверяющие.

## <a name="configure-the-access-policies-for-your-key-vault"></a>Настройка политик доступа для Key Vault

Вы можете управлять доступом к содержимому Key Vault с помощью **политик доступа**. Политики доступа Key Vault предоставляют разрешения отдельно для ключей, секретов и сертификатов. Вы можете предоставить пользователю доступ только к ключам, но не к секретам. Разрешения на доступ к ключам, секретам и сертификатам управляются на уровне хранилища. Дополнительные сведения см. в статье [Безопасность Azure Key Vault](/azure/key-vault/general/overview-security#identity-and-access-management).

> [!NOTE]
> При создании Key Vault в подписке Azure решение автоматически связывается с клиентом Azure Active Directory этой подписки. Любому пользователю, пытающемуся управлять содержимым в Key Vault или извлекать его из Key Vault, следует пройти аутентификацию в Azure AD.

1. На страницах свойств Key Vault выберите **Политики доступа**.
2. Щелкните элемент **+ Добавить политику доступа**.
3. Разверните раскрывающийся список **Разрешения ключей** и в разделе **Операции управления ключами** установите флажки **Получить** и **Список**.
4. Щелкните элемент **Выбор субъекта**. Найдите пользователя, которому предоставляется доступ, и нажмите кнопку **Выбрать**.
5. Нажмите кнопку **Добавить**.
6. Обязательно нажмите кнопку **Сохранить**, чтобы сохранить изменения.

> [!NOTE]
> **Не рекомендуется** предоставлять пользователям непосредственный доступ к хранилищу ключей. Оптимальное решение — добавить пользователей в группу Azure AD, которой предоставлен доступ к хранилищу ключей.

## <a name="select-a-certificate-from-your-key-vault-in-visual-studio"></a>Выбор сертификата из Key Vault в Visual Studio

Доступный в Visual Studio мастер **Создание пакетов приложения** позволяет выбрать сертификат, который будет использоваться для подписывания пакета приложения. Сертификат для подписывания пакета можно выбрать с помощью Azure Key Vault. Необходимо указать универсальный код ресурса (URI) для Key Vault, где содержится сертификат. Кроме того, учетная запись Майкрософт, прошедшая проверку подлинности в Visual Studio, должна предоставлять соответствующие разрешения на доступ к хранилищу ключей.

1. В Visual Studio откройте проект **приложения UWP** или **проект упаковки приложений Windows** для классического приложения.
2. Выберите элементы **Опубликовать** -> **Пакет** -> **Создать пакеты приложения…** , чтобы открыть мастер **Создание пакетов приложения**.
3. На странице **Выбрать метод распределения** выберите элемент **Загрузка неопубликованного приложения**.
4. На странице **Выбор метода подписывания** щелкните элемент **Select from Azure Key Vault…** (Выбрать из Azure Key Vault…).
5. Когда откроется диалоговое окно **Select a certificate from Azure Key Vault** (Выбор сертификат из Azure Key Vault), используйте средство выбора учетной записи, чтобы выбрать ту, для которой настроена политика доступа.
6. Введите универсальный код ресурса (URI) для Key Vault. Его можно найти на странице **Обзор** для Key Vault в графе **DNS-имя**.
7. Нажмите кнопку **Просмотр метаданных**.
8. После загрузки сертификатов выберите нужный из списка (например, **UwpSigningCert**).
9. Нажмите кнопку **ОК**.

> [!NOTE]
> Сертификат будет импортирован в локальное хранилище сертификатов, где будет использоваться для подписывания пакета.

## <a name="decrypt-your-certificate-with-a-password-from-azure-key-vault"></a>Расшифровка сертификата с помощью пароля из Azure Key Vault

Если для подписывания пакета приложения используется локальный сертификат, защищенный паролем (PFX), управление паролем для расшифровки такого сертификата может оказаться сложной задачей. При импорте сертификата в мастере **Создание пакетов приложения** вам будет предложено ввести пароль вручную. Кроме того, пароль можно выбрать из Azure Key Vault.

1. В Visual Studio откройте проект **приложения UWP** или **проект упаковки приложений Windows** для классического приложения.
2. Выберите элементы **Опубликовать** -> **Пакет** -> **Создать пакеты приложения…** , чтобы открыть мастер **Создание пакетов приложения**.
3. На странице **Выбрать метод распределения** выберите элемент **Загрузка неопубликованного приложения**.
4. На странице **Выбор метода подписывания** щелкните команду **Выбрать из файла…**
5. Когда откроется диалоговое окно **Certificate is password protected** (Сертификат защищен паролем), щелкните элемент **Select Password From Key Vault** (Выбрать пароль из Key Vault).
6. Когда откроется диалоговое окно **Select a password from Azure Key Vault** (Выбор пароля из Azure Key Vault), используйте средство выбора учетной записи, чтобы выбрать ту, для которой настроена политика доступа.
7. Введите универсальный код ресурса (URI) для Key Vault. Его можно найти на странице **Обзор** для Key Vault в графе **DNS-имя**.
8. Нажмите кнопку **Просмотр метаданных**.
9. После загрузки паролей выберите нужный из списка (например, **UwpSigningCertPassword**).
10. Нажмите кнопку **ОК**.