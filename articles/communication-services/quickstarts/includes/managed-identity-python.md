---
ms.openlocfilehash: 9da0cbc3777359994fac3778dcc126c844754861
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657671"
---
## <a name="add-managed-identity-to-your-communication-services-solution"></a>Iletişim Hizmetleri çözümünüze yönetilen kimlik ekleme

### <a name="install-the-client-library-packages"></a>İstemci kitaplığı paketlerini yükler

```console
pip install azure-identity
pip install azure-communication-identity
pip install azure-communication-sms
```

### <a name="use-the-client-library-packages"></a>İstemci kitaplığı paketlerini kullanma

`import`Azure kimliğini kullanmak için kodunuza aşağıdakileri ekleyin.

```python
from azure.identity import DefaultAzureCredential
```

Aşağıdaki örneklerde [DefaultAzureCredential](/python/api/azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

Uygulamayı geliştirme ortamında kaydetmek ve ortam değişkenlerini ayarlamak için bkz. [yönetilen kimlikle erişimi yetkilendirme](../managed-identity-from-cli.md)

### <a name="create-an-identity-and-issue-a-token"></a>Bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından yeni bir kullanıcı için bir belirteç vermek üzere istemciyi kullanır:

```python
import azure.communication.identity 

def create_identity_and_get_token(resource_endpoint):
     credential = DefaultAzureCredential()
     client = CommunicationIdentityClient(endpoint, credential)

     user = client.create_user()
     token_response = client.get_token(user, scopes=["voip"])
     
     return token_response
```

### <a name="send-an-sms-with-azure-managed-identity"></a>Azure yönetilen kimliği ile SMS gönderin

Aşağıdaki kod örneği, Azure yönetilen kimliği ile bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```python
from azure.communication.sms import (
    PhoneNumberIdentifier,
    SendSmsOptions,
    SmsClient
)

def send_sms(resource_endpoint, from_phone_number, to_phone_number, message_content):
     credential = DefaultAzureCredential()
     sms_client = SmsClient(resource_endpoint, credential)

     sms_client.send(
          from_phone_number=PhoneNumberIdentitifier(from_phone_number),
          to_phone_numbers=[PhoneNumberIdentifier(to_phone_number)],
          message=message_content,
          send_sms_options=SendSmsOptions(enable_delivery_report=True))  # optional property
     )
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kimlik doğrulaması hakkında bilgi edinin](../../concepts/authentication.md)

Ayrıca şunları yapmak isteyebilirsiniz:

- [Azure rol tabanlı erişim denetimi hakkında daha fazla bilgi edinin](../../../../articles/role-based-access-control/index.yml)
- [Python için Azure kimlik Kitaplığı hakkında daha fazla bilgi edinin](/net/api/overview/azure/identity-readme)
- [Kullanıcı erişim belirteçleri oluşturma](../../quickstarts/access-tokens.md)
- [SMS iletisi gönderme](../../quickstarts/telephony-sms/send.md)
- [SMS hakkında daha fazla bilgi](../../concepts/telephony-sms/concepts.md)