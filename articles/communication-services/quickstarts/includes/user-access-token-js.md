---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: matthewrobertson
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: marobert
ms.openlocfilehash: 22cfe369561eab1ca334c7ff2450162dfae3e761
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92346935"
---
## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/) Etkin LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../create-communication-resource.md).

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin.

```console
mkdir user-tokens-quickstart && cd user-tokens-quickstart
```

`npm init -y`Dosya **üzerinde** varsayılan ayarlarla birpackage.jsoluşturmak için öğesini çalıştırın.

```console
npm init -y
```

### <a name="install-the-package"></a>Paketi yükler

`npm install`JavaScript Için Azure Communication Services yönetim istemcisi kitaplığını yüklemek için komutunu kullanın.

```console

npm install @azure/communication-administration --save

```

`--save`Seçeneği, kitaplığı dosyadaki **package.js** bir bağımlılık olarak listeler.

## <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Kod Düzenleyicinizde yeni bir metin dosyası açın
1. `require`Yüklemek için bir çağrı ekleyin`CommunicationIdentityClient`
1. Temel özel durum işleme dahil olmak üzere programın yapısını oluşturma

Başlamak için aşağıdaki kodu kullanın:

```javascript
const { CommunicationIdentityClient } = require('@azure/communication-administration');

const main = async () => {
  console.log("Azure Communication Services - User Access Tokens Quickstart")

  // Quickstart code goes here
};

main().catch((error) => {
  console.log("Encountered and error");
  console.log(error);
})
```

1. Yeni dosyayı, *Kullanıcı belirteçleri-hızlı başlangıç* dizinine **issue-token.js** olarak kaydedin.

[!INCLUDE [User Access Tokens Object Model](user-access-tokens-object-model.md)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`CommunicationIdentityClient`Bağlantı dizeniz ile bir örneğini oluşturun. Aşağıdaki kod, adlı bir ortam değişkeninden kaynak için bağlantı dizesini alır `COMMUNICATION_SERVICES_CONNECTION_STRING` . [Kaynak bağlantı dizesini yönetme](../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

`main` yöntemine aşağıdaki kodu ekleyin:

```javascript
// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the user token client
const identityClient = new CommunicationIdentityClient(connectionString);
```

## <a name="create-a-user"></a>Kullanıcı oluşturma

Azure Iletişim Hizmetleri, hafif bir kimlik dizini sağlar. `createUser`Dizinde benzersiz olan yeni bir giriş oluşturmak için yöntemini kullanın `Id` . Uygulamanızın kullanıcıları ve Iletişim Hizmetleri tarafından oluşturulan kimlikler (örneğin, uygulama sunucunuzun veritabanında depolayarak) arasında bir eşleme korumanız gerekir.

```javascript
let userResponse = await identityClient.createUser();
console.log(`\nCreated a user with ID: ${userResponse.communicationUserId}`);
```

## <a name="issue-user-access-tokens"></a>Kullanıcı erişim belirteçleri verme

`issueToken`Iletişim Hizmetleri kullanıcısına erişim belirteci vermek için yöntemini kullanın. İsteğe bağlı parametreyi sağlamazsanız, `user` Yeni bir Kullanıcı oluşturulur ve belirteçle döndürülür.

```javascript
// Issue an access token with the "voip" scope for a new user
let tokenResponse = await identityClient.issueToken(userResponse, ["voip"]);
const { token, expiresOn } = tokenResponse;
console.log(`\nIssued a token with 'voip' scope that expires at ${expiresOn}:`);
console.log(token);
```

Kullanıcı erişim belirteçleri, kullanıcılarınızın hizmet kesintilerini yaşmasını engellemek için yeniden verilmesini gerektiren kısa süreli kimlik bilgileridir. `expiresOn`Response özelliği, belirtecin ömrünü gösterir.

## <a name="revoke-user-access-tokens"></a>Kullanıcı erişim belirteçlerini iptal et

Bazı durumlarda, örneğin, bir kullanıcı hizmetinize kimlik doğrulamak için kullandıkları parolayı değiştirdiğinde Kullanıcı erişim belirteçlerini açıkça iptal etmeniz gerekebilir. Bu, `revokeTokens` kullanıcının tüm erişim belirteçlerini geçersiz kılmak için yöntemini kullanır.

```javascript  
await identityClient.revokeTokens(userResponse);
console.log(`\nSuccessfully revoked all tokens for user with Id: ${userResponse.communicationUserId}`);
```

## <a name="refresh-user-access-tokens"></a>Kullanıcı erişim belirteçlerini Yenile

Bir belirteci yenilemek için, `CommunicationUser` yeniden vermek üzere nesnesini kullanın:

```javascript  
let userResponse = new CommunicationUser(existingUserId);
let tokenResponse = await identityClient.issueToken(userResponse, ["voip"]);
```

## <a name="delete-a-user"></a>Kullanıcı silme

Bir kullanıcının silinmesi, tüm etkin belirteçleri iptal eder ve kimlikler için sonraki belirteçleri yayınlamaya engel olur. Ayrıca, kullanıcıyla ilişkili tüm kalıcı içeriği de kaldırır.

```javascript
await identityClient.deleteUser(userResponse);
console.log(`\nDeleted the user with Id: ${userResponse.communicationUserId}`);
```

## <a name="run-the-code"></a>Kodu çalıştırma

Konsol isteminde *issue-token.js* dosyasını içeren dizine gidin, ardından `node` uygulamayı çalıştırmak için aşağıdaki komutu yürütün.

```console
node ./issue-token.js
```
