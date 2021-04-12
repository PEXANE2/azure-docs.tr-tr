---
ms.openlocfilehash: 1c4aab27eb72afa473f95f1c0956b5e3d66c3940
ms.sourcegitcommit: d40ffda6ef9463bb75835754cabe84e3da24aab5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2021
ms.locfileid: "107073452"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install) sürüm 8 veya üzeri.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Dağıtılan bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../create-communication-resource.md).

## <a name="add-managed-identity-to-your-communication-services-solution-java"></a>Iletişim Hizmetleri çözümünüze yönetilen kimlik ekleme (Java)

### <a name="install-the-sdk-packages"></a>SDK paketlerini yükler
pom.xml dosyasında, aşağıdaki bağımlılık öğelerini bağımlılıklar grubuna ekleyin.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.3</version>
</dependency>
```

### <a name="use-the-sdk-packages"></a>SDK paketlerini kullanma

`import`Azure kimliği ve Azure Iletişim SDK 'ları kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```java
import com.azure.communication.common.*;
import com.azure.communication.identity.*;
import com.azure.communication.identity.models.*;
import com.azure.communication.sms.*;
import com.azure.core.credential.*;
import com.azure.identity.*;

import java.io.IOException;
import java.util.*;
```

Aşağıdaki örneklerde [DefaultAzureCredential](/java/api/com.azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

Yönetilen kimlik doğrulaması kullanarak kolayca geçebileceğiniz kolay bir yol için bkz. [yönetilen kimlikle erişimi yetkilendirme](../managed-identity-from-cli.md)

DefaultAzureCredential nesnesinin nasıl çalıştığı ve bu hızlı başlangıçta belirtilmeyen yollarla nasıl kullanabileceğiniz hakkında daha ayrıntılı bir bakış için bkz. [Java Için Azure Identity Client Library](https://docs.microsoft.com/java/api/overview/azure/identity-readme)

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Yönetilen kimlikle bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, yönetilen kimliğe sahip bir hizmet istemci nesnesinin nasıl oluşturulacağını gösterir.
Ardından, yeni bir kullanıcı için bir belirteç vermek üzere istemcisini kullanın:

```java
     public AccessToken createIdentityAndGetTokenAsync() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
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

          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          SmsClient smsClient = new SmsClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
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

