---
ms.openlocfilehash: 2c816f005dea452c3ffa2889aa7d2742a5762759
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657672"
---
## <a name="add-managed-identity-to-your-communication-services-solution-net"></a>Iletişim Hizmetleri çözümünüze yönetilen kimlik ekleme (.net)

### <a name="install-the-client-library-packages"></a>İstemci kitaplığı paketlerini yükler

```console
dotnet add package Azure.Communication.Identity
dotnet add package Azure.Communication.Sms
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>İstemci kitaplığı paketlerini kullanma

`using`Azure kimlik ve Azure depolama istemci kitaplıklarını kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Sms;
```

Aşağıdaki örneklerde [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

`AZURE_CLIENT_SECRET``AZURE_CLIENT_ID`ve `AZURE_TENANT_ID` bir nesne oluşturmak için ortam değişkenleri gereklidir `DefaultAzureCredential` . Geliştirme ortamında kayıtlı bir uygulama oluşturmak ve ortam değişkenlerini ayarlamak için bkz. [yönetilen kimlik ile erişim yetkisi verme](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Yönetilen kimlikle bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, Azure Active Directory belirteçleriyle bir hizmet istemci nesnesinin nasıl oluşturulacağını gösterir.

Ardından, yeni bir kullanıcı için bir belirteç vermek üzere istemcisini kullanın:

```csharp
     public async Task<Response<CommunicationUserToken>> CreateIdentityAndGetTokenAsync(Uri resourceEdnpoint)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = await client.CreateUserAsync();

          var tokenResponse = await client.GetTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Yönetilen kimliğe sahip bir SMS gönderin

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir SMS hizmeti istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```csharp
     public async Task SendSms(Uri resourceEndpoint, PhoneNumber from, PhoneNumber to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          SmsClient smsClient = new SmsClient(resourceEndpoint, credential);
          smsClient.Send(
               from: from,
               to: to,
               message: message,
               new SendSmsOptions { EnableDeliveryReport = true } // optional
          );
     }
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kimlik doğrulaması hakkında bilgi edinin](../concepts/authentication.md)

Ayrıca şunları yapmak isteyebilirsiniz:

- [Azure rol tabanlı erişim denetimi hakkında daha fazla bilgi edinin](../../../../articles/role-based-access-control/index.yml)
- [.NET için Azure kimlik Kitaplığı hakkında daha fazla bilgi edinin](/dotnet/api/overview/azure/identity-readme)
- [Kullanıcı erişim belirteçleri oluşturma](../../quickstarts/access-tokens.md)
- [SMS iletisi gönderme](../telephony-sms/send.md)
- [SMS hakkında daha fazla bilgi](../../concepts/telephony-sms/concepts.md)
