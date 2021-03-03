---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: tomaschladek
manager: nmurav
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: tchladek
ms.openlocfilehash: 381cba23175086a4d9bac7cc0ba71807bc248056
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750575"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/) Etkin LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../create-communication-resource.md).

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin.

```console
mkdir access-tokens-quickstart && cd access-tokens-quickstart
```

`npm init -y`Dosya **üzerinde** varsayılan ayarlarla birpackage.jsoluşturmak için öğesini çalıştırın.

```console
npm init -y
```

### <a name="install-the-package"></a>Paketi yükler

`npm install`JavaScript Için Azure Iletişim Hizmetleri kimlik istemci kitaplığını yüklemek için komutunu kullanın.

```console

npm install @azure/communication-identity --save

```

`--save`Seçeneği, kitaplığı dosyadaki **package.js** bir bağımlılık olarak listeler.

## <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Kod Düzenleyicinizde yeni bir metin dosyası açın
1. `require`Yüklemek için bir çağrı ekleyin`CommunicationIdentityClient`
1. Temel özel durum işleme dahil olmak üzere programın yapısını oluşturma

Başlamak için aşağıdaki kodu kullanın:

```javascript
const { CommunicationIdentityClient } = require('@azure/communication-identity');

const main = async () => {
  console.log("Azure Communication Services - Access Tokens Quickstart")

  // Quickstart code goes here
};

main().catch((error) => {
  console.log("Encountered an error");
  console.log(error);
})
```

1. Yeni dosyayı, *erişim-belirteçleri-hızlı başlangıç* dizinine **issue-access-token.js** olarak kaydedin.

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`CommunicationIdentityClient`Bağlantı dizeniz ile bir örneğini oluşturun. Aşağıdaki kod, adlı bir ortam değişkeninden kaynak için bağlantı dizesini alır `COMMUNICATION_SERVICES_CONNECTION_STRING` . [Kaynak bağlantı dizesini yönetme](../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

`main` yöntemine aşağıdaki kodu ekleyin:

```javascript
// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the identity client
const identityClient = new CommunicationIdentityClient(connectionString);
```

## <a name="create-an-identity"></a>Kimlik oluşturma

Azure Iletişim Hizmetleri, hafif bir kimlik dizini sağlar. `createUser`Dizinde benzersiz olan yeni bir giriş oluşturmak için yöntemini kullanın `Id` . Uygulamanın kullanıcılarına eşleme ile alınan kimliği depola. Örneğin, bunları uygulama sunucunuzun veritabanında depolayarak. Daha sonra erişim belirteçleri vermek için kimlik gereklidir.

```javascript
let identityResponse = await identityClient.createUser();
console.log(`\nCreated an identity with ID: ${identityResponse.communicationUserId}`);
```

## <a name="issue-access-tokens"></a>Erişim belirteçleri verme

`issueToken`Zaten var olan bir Iletişim Hizmetleri kimliği için erişim belirteci vermek üzere metodunu kullanın. Parametresi `scopes` , bu erişim belirtecini yetkilendirecek temel öğeler kümesini tanımlar. [Desteklenen eylemlerin listesine](../../concepts/authentication.md)bakın. Parametresinin yeni örneği, `communicationUser` Azure Iletişim hizmeti kimliğinin dize gösterimine göre oluşturulabilir.

```javascript
// Issue an access token with the "voip" scope for an identity
let tokenResponse = await identityClient.issueToken(identityResponse, ["voip"]);
const { token, expiresOn } = tokenResponse;
console.log(`\nIssued an access token with 'voip' scope that expires at ${expiresOn}:`);
console.log(token);
```

Erişim belirteçleri yeniden verilmesini gerektiren kısa ömürlü kimlik bilgileridir. Bunu yapmamak, uygulamanızın kullanıcı deneyiminin kesintiye uğramasına neden olabilir. `expiresOn`Response özelliği, erişim belirtecinin ömrünü gösterir.

## <a name="create-an-identity-and-issue-an-access-token-within-the-same-request"></a>Aynı istek içinde bir kimlik oluşturma ve erişim belirteci verme

`createUserWithToken`Bir Iletişim Hizmetleri kimliği oluşturmak ve bunun için bir erişim belirteci vermek için yöntemini kullanın. Parametresi `scopes` , bu erişim belirtecini yetkilendirecek temel öğeler kümesini tanımlar. [Desteklenen eylemlerin listesine](../../concepts/authentication.md)bakın.

```javascript
// Issue an identity and an access token with the "voip" scope for the new identity
let identityTokenResponse = await this.client.createUserWithToken(["voip"]);
const { token, expiresOn, user } = identityTokenResponse;
console.log(`\nCreated an identity with ID: ${user.communicationUserId}`);
console.log(`\nIssued an access token with 'voip' scope that expires at ${expiresOn}:`);
console.log(token);
```

## <a name="refresh-access-tokens"></a>Erişim belirteçlerini yenileme

Erişim belirteçlerini yenilemek, `issueToken` belirteçleri vermek için kullanılan kimlik ile çağırmak kadar kolaydır. Ayrıca, yenilenen belirteçleri de sağlamanız gerekir `scopes` . 

```javascript
// // Value of identityResponse represents the Azure Communication Services identity stored during identity creation and then used to issue the tokens being refreshed
let refreshedTokenResponse = await identityClient.issueToken(identityResponse, ["voip"]);
```


## <a name="revoke-access-tokens"></a>Erişim belirteçlerini iptal et

Bazı durumlarda, erişim belirteçlerini açıkça iptal edebilirsiniz. Örneğin, bir uygulamanın kullanıcısı hizmetinize kimlik doğrulaması yapmak için kullandıkları parolayı değiştirdiğinde. Yöntem `revokeTokens` , kimliğe verilen tüm etkin erişim belirteçlerini geçersiz kılar.

```javascript  
await identityClient.revokeTokens(identityResponse);
console.log(`\nSuccessfully revoked all access tokens for identity with ID: ${identityResponse.communicationUserId}`);
```

## <a name="delete-an-identity"></a>Kimlik silme

Bir kimlik silindiğinde tüm etkin erişim belirteçleri iptal olur ve kimlik için erişim belirteçleri yayınınızdan sonra. Ayrıca, kimlik ile ilişkili tüm kalıcı içeriği de kaldırır.

```javascript
await identityClient.deleteUser(identityResponse);
console.log(`\nDeleted the identity with ID: ${identityResponse.communicationUserId}`);
```

## <a name="run-the-code"></a>Kodu çalıştırma

Konsol isteminde *issue-access-token.js* dosyasını içeren dizine gidin, ardından `node` uygulamayı çalıştırmak için aşağıdaki komutu yürütün.

```console
node ./issue-access-token.js
```
