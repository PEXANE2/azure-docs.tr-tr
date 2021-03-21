---
ms.openlocfilehash: 50707b46445803ee27118ee72b90a237a3e76200
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103021838"
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
from azure.communication.identity import CommunicationIdentityClient 

def create_identity_and_get_token(resource_endpoint):
     credential = DefaultAzureCredential()
     client = CommunicationIdentityClient(resource_endpoint, credential)

     user = client.create_user()
     token_response = client.get_token(user, scopes=["voip"])
     
     return token_response
```

### <a name="send-an-sms-with-azure-managed-identity"></a>Azure yönetilen kimliği ile SMS gönderin

Aşağıdaki kod örneği, Azure yönetilen kimliği ile bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```python
from azure.communication.sms import SmsClient

def send_sms(resource_endpoint, from_phone_number, to_phone_number, message_content):
     credential = DefaultAzureCredential()
     sms_client = SmsClient(resource_endpoint, credential)

     sms_client.send(
          from_=from_phone_number,
          to_=[to_phone_number],
          message=message_content,
          enable_delivery_report=True  # optional property
     )
```
