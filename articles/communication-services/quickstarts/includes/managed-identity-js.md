---
ms.openlocfilehash: 3ec650f82b95cdfabc923e92325959c63f2846de
ms.sourcegitcommit: bed20f85722deec33050e0d8881e465f94c79ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2021
ms.locfileid: "105110776"
---
## <a name="add-managed-identity-to-your-communication-services-solution-js"></a>Iletişim Hizmetleri çözümünüze (JS) yönetilen kimlik ekleme

### <a name="install-the-sdk-packages"></a>SDK paketlerini yükler

```console
npm install @azure/communication-identity
npm install @azure/communication-common
npm install @azure/communication-sms
npm install @azure/identity
```

### <a name="use-the-sdk-packages"></a>SDK paketlerini kullanma

`import`Azure kimliğini ve Azure Storage SDK 'larını kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```typescript
import { DefaultAzureCredential } from "@azure/identity";
import { CommunicationIdentityClient, CommunicationUserToken } from "@azure/communication-identity";
import { SmsClient, SmsSendRequest } from "@azure/communication-sms";
```

Aşağıdaki örneklerde [DefaultAzureCredential](/javascript/api/azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

Uygulamayı geliştirme ortamında kaydetmek ve ortam değişkenlerini ayarlamak için bkz. [yönetilen kimlikle erişimi yetkilendirme](../managed-identity-from-cli.md)  

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Yönetilen kimlikle bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından yeni bir kullanıcı için bir belirteç vermek üzere istemciyi kullanır:

```JavaScript
export async function createIdentityAndIssueToken(resourceEndpoint: string): Promise<CommunicationUserToken> {
     let credential = new DefaultAzureCredential();
     const client = new CommunicationIdentityClient(resourceEndpoint, credential);
     return await client.createUserAndToken(["chat"]);
}
```

### <a name="send-an-sms-with-managed-identity"></a>Yönetilen kimliğe sahip bir SMS gönderin

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```JavaScript
export async function sendSms(resourceEndpoint: string, fromNumber: any, toNumber: any, message: string) {
     let credential = new DefaultAzureCredential();
     const smsClient = new SmsClient(resourceEndpoint, credential);
     const sendRequest: SmsSendRequest = { 
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

