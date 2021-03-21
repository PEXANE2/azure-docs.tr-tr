---
ms.openlocfilehash: c1b74b43c6ef884c68282dcaaae8dfc9a5541453
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103622328"
---
## <a name="add-managed-identity-to-your-communication-services-solution-net"></a>Iletişim Hizmetleri çözümünüze yönetilen kimlik ekleme (.NET)

### <a name="install-the-client-library-packages"></a>İstemci kitaplığı paketlerini yükler

```console
dotnet add package Azure.Communication.Identity  --version 1.0.0-beta.5
dotnet add package Azure.Communication.Sms  --version 1.0.0-beta.4
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>İstemci kitaplığı paketlerini kullanma

`using`Azure kimlik ve Azure depolama istemci kitaplıklarını kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Sms;
using Azure.Core;
```

Aşağıdaki örneklerde [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

`AZURE_CLIENT_SECRET``AZURE_CLIENT_ID`ve `AZURE_TENANT_ID` bir nesne oluşturmak için ortam değişkenleri gereklidir `DefaultAzureCredential` . Geliştirme ortamında kayıtlı bir uygulama oluşturmak ve ortam değişkenlerini ayarlamak için bkz. [yönetilen kimlik ile erişim yetkisi verme](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Yönetilen kimlikle bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, Azure Active Directory belirteçleriyle bir hizmet istemci nesnesinin nasıl oluşturulacağını gösterir.

Ardından, yeni bir kullanıcı için bir belirteç vermek üzere istemcisini kullanın:

```csharp
     public Response<AccessToken> CreateIdentityAndGetTokenAsync(Uri resourceEndpoint)
     {
          TokenCredential credential = new DefaultAzureCredential();

          // You can find your endpoint and access key from your resource in the Azure portal
          // "https://<RESOURCE_NAME>.communication.azure.com";

          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = client.CreateUser();
          var identity = identityResponse.Value;

          var tokenResponse = client.GetToken(identity, scopes: new[] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Yönetilen kimliğe sahip bir SMS gönderin

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir SMS hizmeti istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```csharp
     public SmsSendResult SendSms(Uri resourceEndpoint, string from, string to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          // "https://<RESOURCE_NAME>.communication.azure.com";

          SmsClient smsClient = new SmsClient(resourceEndpoint, credential);
          SmsSendResult sendResult = smsClient.Send(
               from: from,
               to: to,
               message: message,
               new SmsSendOptions(enableDeliveryReport: true) // optional
          );

          return sendResult;
      }
```

