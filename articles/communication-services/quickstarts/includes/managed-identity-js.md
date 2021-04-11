---
ms.openlocfilehash: a3771a21914831f249696fc3d733c5ea935c2c7e
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2021
ms.locfileid: "107251318"
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

Aşağıdaki örneklerde [DefaultAzureCredential](/javascript/api/@azure/identity/defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

Yönetilen kimlik doğrulaması kullanarak kolayca geçebileceğiniz kolay bir yol için bkz. [yönetilen kimlikle erişimi yetkilendirme](../managed-identity-from-cli.md)

DefaultAzureCredential nesnesinin nasıl çalıştığı ve bu hızlı başlangıçta belirtilmeyen yollarla nasıl kullanabileceğiniz hakkında daha ayrıntılı bir bakış için bkz. [JavaScript Için Azure Identity Client Library](https://docs.microsoft.com/javascript/api/overview/azure/identity-readme)

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