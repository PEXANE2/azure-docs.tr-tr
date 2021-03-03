---
ms.openlocfilehash: 0fd97fe0eebb23130513409698b75d2ee7b98a6f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657664"
---
## <a name="add-managed-identity-to-your-communication-services-solution-js"></a>Iletişim Hizmetleri çözümünüze (JS) yönetilen kimlik ekleme

### <a name="install-the-client-library-packages"></a>İstemci kitaplığı paketlerini yükler

```console
npm install @azure/communication-identity
npm install @azure/communication-common
npm install @azure/communication-sms
npm install @azure/identity
```

### <a name="use-the-client-library-packages"></a>İstemci kitaplığı paketlerini kullanma

`import`Azure kimlik ve Azure depolama istemci kitaplıklarını kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```typescript
import { DefaultAzureCredential } from "@azure/identity";
import { CommunicationIdentityClient } from "@azure/communication-identity";
import { SmsClient } from "@azure/communication-sms";
```

Aşağıdaki örneklerde [DefaultAzureCredential](/javascript/api/azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

Uygulamayı geliştirme ortamında kaydetmek ve ortam değişkenlerini ayarlamak için bkz. [yönetilen kimlikle erişimi yetkilendirme](../managed-identity-from-cli.md)  

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Yönetilen kimlikle bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından yeni bir kullanıcı için bir belirteç vermek üzere istemciyi kullanır:

```JavaScript
export async function createIdentityAndIssueToken(resourceEndpoint: string): Promise<CommunicationUserToken> {
     let credential = new DefaultAzureCredential();
     const client = new CommunicationIdentityClient(resourceEndpoint, credential);
     return await client.createUserWithToken(["chat"]);
}
```

### <a name="send-an-sms-with-managed-identity"></a>Yönetilen kimliğe sahip bir SMS gönderin

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```JavaScript
export async function sendSms(resourceEndpoint: string, fromNumber: any, toNumber: any, message: string) {
     let credential = new DefaultAzureCredential();
     const smsClient = new SmsClient(resourceEndpoint, credential);
     const sendRequest: SendRequest = { 
          from: fromNumber, 
          to: [toNumber], 
          message: message 
     };

     const response = await smsClient.send(
          sendRequest, 
          {} //Optional SendOptions
          );
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kimlik doğrulaması hakkında bilgi edinin](../concepts/authentication.md)

Ayrıca şunları yapmak isteyebilirsiniz:

- [Azure rol tabanlı erişim denetimi hakkında daha fazla bilgi edinin](../../../../articles/role-based-access-control/index.yml)
- [JS için Azure kimlik Kitaplığı hakkında daha fazla bilgi edinin](/javascript/api/overview/azure/identity-readme)
- [Kullanıcı erişim belirteçleri oluşturma](../../quickstarts/access-tokens.md)
- [SMS iletisi gönderme](../../quickstarts/telephony-sms/send.md)
- [SMS hakkında daha fazla bilgi](../../concepts/telephony-sms/concepts.md)

