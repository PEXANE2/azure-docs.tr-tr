---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: matthewrobertson
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: marobert
ms.openlocfilehash: 4be8821a949527fefcc9005b1de7f4f7c438c568
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948156"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Python](https://www.python.org/downloads/) 2,7, 3,5 veya üzeri.
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../create-communication-resource.md).

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

1. Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin.

   ```console
   mkdir user-tokens-quickstart && cd user-tokens-quickstart
   ```

1. Proje kök dizininde **issue-Tokens.py** adlı bir dosya oluşturmak ve temel özel durum işleme dahil olmak üzere programın yapısını eklemek için bir metin düzenleyicisi kullanın. Bu hızlı başlangıç için tüm kaynak kodu aşağıdaki bölümlerde bu dosyaya ekleyeceksiniz.

   ```python
   import os
   from azure.communication.administration import CommunicationIdentityClient

   try:
      print('Azure Communication Services - User Access Tokens Quickstart')
      # Quickstart code goes here
   except Exception as ex:
      print('Exception:')
      print(ex)
   ```

### <a name="install-the-package"></a>Paketi yükler

Hala uygulama dizininde, komutunu kullanarak Python paketi için Azure Communication Services yönetim istemcisi kitaplığı ' nı da yükleyebilirsiniz `pip install` .

```console
pip install azure-communication-administration
```

[!INCLUDE [User Access Tokens Object Model](user-access-tokens-object-model.md)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`CommunicationIdentityClient`Bağlantı dizeniz ile bir örneğini oluşturun. Aşağıdaki kod, adlı bir ortam değişkeninden kaynak için bağlantı dizesini alır `COMMUNICATION_SERVICES_CONNECTION_STRING` . [Kaynak bağlantı dizesini yönetme](../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

Bu kodu bloğunun içine ekleyin `try` :

```python
# This code demonstrates how to fetch your connection string
# from an environment variable.
connection_string = os.environ['COMMUNICATION_SERVICES_CONNECTION_STRING']

# Instantiate the identity client
client = CommunicationIdentityClient.from_connection_string(connection_string)
```

## <a name="create-a-user"></a>Kullanıcı oluşturma

Azure Iletişim Hizmetleri, hafif bir kimlik dizini sağlar. `create_user`Dizinde benzersiz olan yeni bir giriş oluşturmak için yöntemini kullanın `Id` . Uygulamanızın kullanıcıları ve Iletişim Hizmetleri tarafından oluşturulan kimlikler (örneğin, uygulama sunucunuzun veritabanında depolayarak) arasında bir eşleme korumanız gerekir.

```python
user = client.create_user()
print("\nCreated a user with ID: " + user.identifier + ":")
```

## <a name="issue-user-access-tokens"></a>Kullanıcı erişim belirteçleri verme

`issue_token`Iletişim Hizmetleri kullanıcısına erişim belirteci vermek için yöntemini kullanın. İsteğe bağlı parametreyi sağlamazsanız, `user` Yeni bir Kullanıcı oluşturulur ve belirteçle döndürülür.

```python
# Issue an access token with the "voip" scope for a new user
token_result = client.issue_token(user, ["voip"])
expires_on = token_result.expires_on.strftime('%d/%m/%y %I:%M %S %p')
print("\nIssued a token with 'voip' scope that expires at " + expires_on + ":")
print(token_result.token)
```

Kullanıcı erişim belirteçleri, kullanıcılarınızın hizmet kesintilerini yaşmasını engellemek için yeniden verilmesini gerektiren kısa süreli kimlik bilgileridir. `expires_on`Response özelliği, belirtecin ömrünü gösterir.

## <a name="revoke-user-access-tokens"></a>Kullanıcı erişim belirteçlerini iptal et

Bazı durumlarda, örneğin, bir kullanıcı hizmetinize kimlik doğrulamak için kullandıkları parolayı değiştirdiğinde Kullanıcı erişim belirteçlerini açıkça iptal etmeniz gerekebilir. Bu işlev, Azure Iletişim Hizmetleri Yönetimi istemci kitaplığı aracılığıyla kullanılabilir.

```python  
client.revoke_tokens(user)
print("\nSuccessfully revoked all tokens for user with ID: " + user.identifier)
```

## <a name="delete-a-user"></a>Kullanıcı silme

Bir kimliği silmek tüm etkin belirteçleri iptal eder ve kimlikler için sonraki belirteçleri yayınlamaya engel olur. Ayrıca, kullanıcıyla ilişkili tüm kalıcı içeriği de kaldırır.

```python
client.delete_user(user)
print("\nDeleted the user with ID: " + user.identifier)
```

## <a name="run-the-code"></a>Kodu çalıştırma

Konsol isteminde *issue-Token.py* dosyasını içeren dizine gidin ve `python` uygulamayı çalıştırmak için aşağıdaki komutu yürütün.

```console
python ./issue-token.py
```
