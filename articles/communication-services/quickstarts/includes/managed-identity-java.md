---
ms.openlocfilehash: f2e4bf603fa4cfb93c7ca51f64029ccaedcff727
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103021125"
---
## <a name="add-managed-identity-to-your-communication-services-solution-java"></a>Iletişim Hizmetleri çözümünüze yönetilen kimlik ekleme (Java)

### <a name="install-the-client-library-packages"></a>İstemci kitaplığı paketlerini yükler
pom.xml dosyasında, aşağıdaki bağımlılık öğelerini bağımlılıklar grubuna ekleyin.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0-beta.6</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0-beta.4</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.3</version>
</dependency>
```

### <a name="use-the-client-library-packages"></a>İstemci kitaplığı paketlerini kullanma

`import`Azure kimliği ve Azure iletişim istemci kitaplıklarını kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```java
import com.azure.communication.common.*;
import com.azure.communication.identity.*;
import com.azure.communication.identity.models.*;
import com.azure.communication.sms.*;
import com.azure.core.credential.*;
import com.azure.core.http.*;
import com.azure.core.http.netty.*;
import com.azure.identity.*;

import java.io.IOException;
import java.util.*;
```

Aşağıdaki örneklerde [DefaultAzureCredential](/java/api/azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

`AZURE_CLIENT_SECRET``AZURE_CLIENT_ID`ve `AZURE_TENANT_ID` bir nesne oluşturmak için ortam değişkenleri gereklidir `DefaultAzureCredential` . Geliştirme ortamında kayıtlı bir uygulama oluşturmak ve ortam değişkenlerini ayarlamak için bkz. [yönetilen kimlik ile erişim yetkisi verme](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Yönetilen kimlikle bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulacağını gösterir.
Ardından, yeni bir kullanıcı için bir belirteç vermek üzere istemcisini kullanın:

```java
     public AccessToken createIdentityAndGetTokenAsync() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          CommunicationUserIdentifier user = communicationIdentityClient.createUser();
          AccessToken userToken = communicationIdentityClient.getToken(user, new ArrayList<>(Arrays.asList(CommunicationTokenScope.CHAT)));
          return userToken;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Yönetilen kimliğe sahip bir SMS gönderin

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```java
     public SendSmsResponse sendSms() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          SmsClient smsClient = new SmsClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          // Send the message and check the response for a message id
          SmsSendResult response = smsClient.send(
               "<from-phone-number>",
               "<to-phone-number>",
               "your message"
          );

          return response;
    }
```

