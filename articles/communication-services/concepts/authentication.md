---
title: Azure Iletişim hizmetlerinde kimlik doğrulama
titleSuffix: An Azure Communication Services concept document
description: Bir uygulamanın veya hizmetin Iletişim hizmetlerinde kimlik doğrulaması yapabileceği çeşitli yollar hakkında bilgi edinin.
author: matthewrobertson
manager: jken
services: azure-communication-services
ms.author: marobert
ms.date: 07/24/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 928737608ae3e3e44b352724713a284ff9a45da9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941751"
---
# <a name="authenticate-to-azure-communication-services"></a>Azure Iletişim hizmetlerinde kimlik doğrulama

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Bu makalede, *erişim anahtarları* ve *Kullanıcı erişimi belirteçleri*kullanılarak Azure iletişim hizmetleri ile istemcilerin kimlik doğrulaması hakkında bilgi sağlanır. Azure Iletişim Hizmetleri ile her istemci etkileşiminin kimliğinin doğrulanması gerekir.

Aşağıdaki tabloda, Azure Iletişim Hizmetleri istemci kitaplıkları tarafından desteklenen kimlik doğrulama seçenekleri açıklanmaktadır:

| İstemci kitaplığı | Erişim anahtarı    | Kullanıcı erişim belirteçleri |
| -------------- | ------------- | ------------------ |
| Yönetim | Desteklenir     | Desteklenmiyor      |
| SMS            | Desteklenir     | Desteklenmiyor      |
| Sohbet           | Desteklenmiyor | Desteklenir          |
| Events        | Desteklenmiyor | Desteklenir          |

Her yetkilendirme seçeneği kısaca aşağıda açıklanmıştır:

- SMS ve yönetim işlemleri için anahtar kimlik doğrulamasına **erişin** . Erişim anahtarı kimlik doğrulaması, güvenilir bir hizmet ortamında çalışan uygulamalar için uygundur. Bir erişim anahtarı ile kimlik doğrulamak için, bir istemci, [karma tabanlı bir yöntem kimlik doğrulama kodu (HMAC)](https://en.wikipedia.org/wiki/HMAC) oluşturur ve `Authorization` her http isteğinin üstbilgisine dahil eder. Daha fazla bilgi için bkz. [erişim anahtarı Ile kimlik doğrulama](#authenticate-with-an-access-key).
- Sohbet için **Kullanıcı erişim belirteci** kimlik doğrulaması ve çağırma. Kullanıcı erişim belirteçleri, istemci uygulamalarınızın doğrudan Azure Iletişim hizmetlerinde kimlik doğrulamasını sağlar. Bu belirteçler, oluşturduğunuz sunucu tarafı belirteç sağlama hizmetinde oluşturulur. Daha sonra, sohbeti başlatmak ve istemci kitaplıklarını çağırmak için belirteci kullanan istemci cihazlara sağlanırlar. Daha fazla bilgi için bkz. [Kullanıcı erişim belirteci Ile kimlik doğrulama](#authenticate-with-a-user-access-token).

## <a name="authenticate-with-an-access-key"></a>Erişim anahtarı ile kimlik doğrulama

Erişim anahtarı kimlik doğrulaması, SHA256 algoritması kullanılarak hesaplanan her HTTP isteği için HMAC oluşturmak üzere paylaşılan bir gizli anahtar kullanır ve `Authorization` düzeni kullanarak üst bilgiye gönderir `HMAC-SHA256` .

```
Authorization: "HMAC-SHA256 SignedHeaders=date;host;x-ms-content-sha256&Signature=<hmac-sha256-signature>"
```

Erişim anahtarı kimlik doğrulaması kullanan Azure Iletişim Hizmetleri istemci kitaplıkları, kaynağınızın bağlantı dizesiyle başlatılmalıdır. İstemci kitaplığı kullanmıyorsanız, kaynak erişim anahtarınızı kullanarak program aracılığıyla HMAC 'ler oluşturabilirsiniz. Bağlantı dizeleri hakkında daha fazla bilgi edinmek için [kaynak sağlama hızlı](../quickstarts/create-communication-resource.md)başlangıcı ' nı ziyaret edin.

### <a name="sign-an-http-request"></a>HTTP isteğini imzala

Azure Iletişim Hizmetleri REST API 'Lerine HTTP istekleri yapmak için bir istemci kitaplığı kullanmıyorsanız, her HTTP isteği için programlı olarak HMAC 'ler oluşturmanız gerekir. Aşağıdaki adımlarda yetkilendirme üst bilgisinin nasıl oluşturulacağı açıklanır:

1. İstek için Eşgüdümlü Evrensel Saat (UTC) zaman damgasını ya da `x-ms-date` Standart HTTP `Date` üst bilgisinde belirtin. Hizmet, yeniden yürütme saldırıları dahil olmak üzere belirli güvenlik saldırılarına karşı koruma sağlamak için bunu doğrular.
1. SHA256 algoritmasını kullanarak HTTP istek gövdesini karma hale gelir ve sonra, istek ile, üst bilgisi aracılığıyla geçirin `x-ms-content-sha256` .
1. HTTP fiilini (ör. `GET` veya `PUT` ), http istek yolunu ve `Date` `Host` ve `x-ms-content-sha256` http üst bilgilerinin değerlerini aşağıdaki biçimde birleştirerek imzalanacak dizeyi oluşturun:
    ```
    VERB + "\n"
    URLPathAndQuery + "\n"
    DateHeaderValue + ";" + HostHeaderValue + ";" + ContentHashHeaderValue
    ```
1. Önceki adımda oluşturduğunuz UTF-8 kodlu dize için HMAC-256 imzası oluşturun. Sonra, sonuçlarınızı Base64 olarak kodlayın. Ayrıca, depolama hesabı anahtarınızın Base64 kodunu çözmelisiniz. Aşağıdaki biçimi kullanın (sözde kod olarak gösterilir):
    ```
    Signature=Base64(HMAC-SHA256(UTF8(StringToSign), Base64.decode(<your_azure_storage_account_shared_key>)))
    ```
1. Yetkilendirme üstbilgisini aşağıdaki gibi belirtin:
    ```
    Authorization="HMAC-SHA256 SignedHeaders=date;host;x-ms-content-sha256&Signature=<hmac-sha256-signature>"  
    ```
    Bu, `<hmac-sha256-signature>` önceki ADıMDA HMAC 'nin hesaplandığı yerdir.

## <a name="authenticate-with-a-user-access-token"></a>Kullanıcı erişim belirteciyle kimlik doğrulama

Kullanıcı erişim belirteçleri, istemci uygulamalarınızın doğrudan Azure Iletişim hizmetlerinde kimlik doğrulamasını sağlar. Bunu başarmak için, uygulama kullanıcılarınızın kimliğini doğrulayan ve yönetim istemci kitaplığı ile Kullanıcı erişim belirteçleri veren bir güvenilen hizmet ayarlamanız gerekir. Mimari önemli konuları hakkında daha fazla bilgi edinmek için [istemci ve sunucu mimarisi](./client-and-server-architecture.md) kavramsal belgelerini ziyaret edin.

`CommunicationClientCredential`Sınıfı, istemci kitaplıklarına Kullanıcı erişim belirteci kimlik bilgileri sağlamaya ve yaşam döngüsünü yönetmeye yönelik mantığı içerir.

### <a name="initialize-the-client-libraries"></a>İstemci kitaplıklarını başlatma

Kullanıcı erişim belirteci kimlik doğrulaması gerektiren Azure Iletişim Hizmetleri istemci kitaplıklarını başlatmak için önce sınıfın bir örneğini oluşturun `CommunicationClientCredential` ve ardından API istemcisini başlatmak için bunu kullanın.

Aşağıdaki kod parçacıkları, bir Kullanıcı erişim belirteci ile sohbet istemci kitaplığını nasıl başlatakullanacağınızı gösterir:

#### <a name="c"></a>[C#](#tab/csharp)

```csharp
// user access tokens should be created by a trusted service using the Administration client library
var token = "<valid-user-access-token>";

// create a CommunicationUserCredential instance
var userCredential = new CommunicationUserCredential(token);

// initialize the chat client library with the credential
var chatClient = new ChatClient(ENDPOINT_URL, userCredential);
```

#### <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
// user access tokens should be created by a trusted service using the Administration client library
const token = "<valid-user-access-token>";

// create a CommunicationUserCredential instance with the AzureCommunicationUserCredential class
const userCredential = new AzureCommunicationUserCredential(token);

// initialize the chat client library with the credential
let chatClient = new ChatClient(ENDPOINT_URL, userCredential);
```

#### <a name="swift"></a>[Swift](#tab/swift)

```swift
// user access tokens should be created by a trusted service using the Administration client library
let token = "<valid-user-access-token>";

// create a CommunicationUserCredential instance
let userCredential = try CommunicationUserCredential(token: token)

// initialize the chat client library with the credential
let chatClient = try CommunicationChatClient(credential: userCredential, endpoint: ENDPOINT_URL)
```

#### <a name="java"></a>[Java](#tab/java)

```java
// user access tokens should be created by a trusted service using the Administration client library
String token = "<valid-user-access-token>";

// create a CommunicationUserCredential instance
CommunicationUserCredential userCredential = new CommunicationUserCredential(token);

// Initialize the chat client
final ChatClientBuilder builder = new ChatClientBuilder();
builder.endpoint(ENDPOINT_URL)
    .credential(userCredential)
    .httpClient(HTTP_CLIENT);
ChatClient chatClient = builder.buildClient();
```

---

### <a name="refreshing-user-access-tokens"></a>Kullanıcı erişim belirteçleri yenileniyor

Kullanıcı erişim belirteçleri, kullanıcılarınızın hizmet kesintilerini yaşmasını engellemek için yeniden kullanılması gereken kısa ömürlü kimlik bilgileridir. `CommunicationUserCredential`Oluşturucu, Kullanıcı erişim belirteçlerini kullanım süreleri dolmadan önce güncelleştirmenizi sağlayan bir yenileme geri çağırma işlevini kabul eder. Güvenilen hizmetinizden yeni bir Kullanıcı erişim belirteci getirmek için bu geri aramayı kullanmanız gerekir.

#### <a name="c"></a>[C#](#tab/csharp)

```csharp
var userCredential = new CommunicationUserCredential(
    initialToken: token,
    refreshProactively: true,
    tokenRefresher: cancellationToken => fetchNewTokenForCurrentUser(cancellationToken)
);
```

#### <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const userCredential = new AzureCommunicationUserCredential({
  tokenRefresher: async () => fetchNewTokenForCurrentUser(),
  refreshProactively: true,
  initialToken: token
});
```

#### <a name="swift"></a>[Swift](#tab/swift)

```swift
 let userCredential = try CommunicationUserCredential(initialToken: token, refreshProactively: true) { |completionHandler|
   let updatedToken = fetchTokenForCurrentUser()
   completionHandler(updatedToken, nil)
 }
```

#### <a name="java"></a>[Java](#tab/java)

```java
TokenRefresher tokenRefresher = new TokenRefresher() {
    @Override
    Future<String> getFetchTokenFuture() {
        return fetchNewTokenForCurrentUser();
    }
}

CommunicationUserCredential credential = new CommunicationUserCredential(tokenRefresher, token, true);
```
---

`refreshProactively`Seçeneği, belirteç yaşam döngüsünü nasıl yöneteceğine karar vermenizi sağlar. Varsayılan olarak, bir belirteç eski olduğunda, geri çağırma API isteklerini engeller ve yenileme girişiminde bulunur. `refreshProactively`, Geri çağırma için ayarlandığında, `true` belirtecin süresi dolmadan önce zaman uyumsuz olarak yürütülür.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanıcı erişim belirteçleri oluşturma](../quickstarts/access-tokens.md)

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [İstemci ve sunucu mimarisi hakkında bilgi edinin](../concepts/client-and-server-architecture.md)
