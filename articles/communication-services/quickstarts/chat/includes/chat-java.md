---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 146053ffd72b24216bfa86577787727257da2516
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103495431"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/java/azure/jdk/) sürüm 8 veya üzeri.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Dağıtılan bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- Bir [Kullanıcı erişim belirteci](../../access-tokens.md). Kapsamı "sohbet" olarak ayarladığınızdan emin olun ve belirteç dizesinin yanı sıra Kullanıcı kimliği dizesini de unutmayın.


## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-java-application"></a>Yeni bir Java uygulaması oluşturma

Terminal veya komut pencerenizi açın ve Java uygulamanızı oluşturmak istediğiniz dizine gidin. Maven-,-hızlı başlangıç şablonundan Java projesi oluşturmak için aşağıdaki komutu çalıştırın.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

' Oluştur ' hedefinin, ArtifactId ile aynı ada sahip bir dizin oluşturduğunu fark edeceksiniz. Bu dizin altında, `src/main/java directory` Proje kaynak kodunu içerir, `src/test/java` Dizin test kaynağını içerir ve pom.xml dosya projenin proje nesne modeli veya [Pod](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)olur.

Uygulamanızın Pod dosyasını Java 8 veya üstünü kullanacak şekilde güncelleştirin:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

### <a name="add-the-package-references-for-the-chat-client-library"></a>Sohbet istemci kitaplığı için paket başvurularını ekleme

Pod dosyanızda, `azure-communication-chat` sohbet API 'leriyle pakete başvurun:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-chat</artifactId>
    <version>1.0.0-beta.4</version> 
</dependency>
```

Kimlik doğrulaması için, istemcinizin pakete başvurması gerekir `azure-communication-common` :

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-common</artifactId>
    <version>1.0.0</version> 
</dependency>
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Java için Azure Iletişim Hizmetleri sohbeti istemci kitaplığı 'nın bazı önemli özelliklerinden bazılarını işler.

| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Bu sınıf, sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| ChatAsyncClient | Bu sınıf, zaman uyumsuz sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| ChatThreadClient | Bu sınıf, sohbet Iş parçacığı işlevselliği için gereklidir. ChatClient aracılığıyla bir örnek edinirsiniz ve bu örneği kullanarak ileti gönderebilir/alabilir/güncelleştirebilir/silebilirsiniz, kullanıcıları ekleyin/kaldırın/alın, yazma bildirimleri gönderin ve okundu bilgilerini okuyun. |
| ChatThreadAsyncClient | Bu sınıf, zaman uyumsuz sohbet Iş parçacığı işlevselliği için gereklidir. ChatAsyncClient aracılığıyla bir örnek edinirsiniz ve bu örneği kullanarak ileti gönderebilir/alabilir/güncelleştirebilir/silebilirsiniz, kullanıcıları ekleyin/kaldırın/alın, yazma bildirimleri gönderin ve okundu bilgilerini okuyun. |

## <a name="create-a-chat-client"></a>Sohbet istemcisi oluşturma
Bir sohbet istemcisi oluşturmak için, Iletişim hizmeti uç noktasını ve önkoşul adımlarının bir parçası olarak oluşturulan erişim belirtecini kullanacaksınız. Kullanıcı erişimi belirteçleri, Azure Iletişim hizmetlerinde doğrudan kimlik doğrulayan istemci uygulamaları oluşturmanızı sağlar. Bu belirteçleri sunucunuzda oluşturduktan sonra bunları bir istemci cihazına geri geçirin. Belirteci sohbet istemcinize geçirmek için ortak istemci kitaplığındaki CommunicationTokenCredential sınıfını kullanmanız gerekir. 

[Sohbet mimarisi](../../../concepts/chat/concepts.md) hakkında daha fazla bilgi edinin

İçeri aktarma deyimlerini eklerken, com. Azure. Communication. chat. Implementation ad alanından değil, com. Azure. Communication. chat ve com. Azure. Communication. chat. model ad alanlarından yalnızca içeri aktarmalar eklediğinizden emin olun. Maven aracılığıyla oluşturulan app. Java dosyasında, şununla başlamak için aşağıdaki kodu kullanabilirsiniz:

```Java
import com.azure.communication.chat.*;
import com.azure.communication.chat.models.*;
import com.azure.communication.common.*;
import com.azure.core.http.HttpClient;

import java.io.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
        System.out.println("Azure Communication Services - Chat Quickstart");
        
        // Your unique Azure Communication service endpoint
        String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

        // Create an HttpClient builder of your choice and customize it
        // Use com.azure.core.http.netty.NettyAsyncHttpClientBuilder if that suits your needs
        NettyAsyncHttpClientBuilder yourHttpClientBuilder = new NettyAsyncHttpClientBuilder();
        HttpClient httpClient = yourHttpClientBuilder.build();

        // User access token fetched from your trusted service
        String userAccessToken = "<USER_ACCESS_TOKEN>";

        // Create a CommunicationTokenCredential with the given access token, which is only valid until the token is valid
        CommunicationTokenCredential userCredential = new CommunicationTokenCredential(userAccessToken);

        // Initialize the chat client
        final ChatClientBuilder builder = new ChatClientBuilder();
        builder.endpoint(endpoint)
            .credential(userCredential)
            .httpClient(httpClient);
        ChatClient chatClient = builder.buildClient();
    }
}
```


## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

`createChatThread`Bir sohbet iş parçacığı oluşturmak için yöntemini kullanın.
`createChatThreadOptions` iş parçacığı isteğini tanımlamakta kullanılır.

- `topic`Bu sohbete bir konu vermek için kullanın; Konu, işlevi kullanılarak sohbet iş parçacığı oluşturulduktan sonra güncelleştirilemeyebilir `UpdateThread` .
- `participants`İş parçacığına eklenecek iş parçacığı katılımcılarını listelemek için kullanın. `ChatParticipant`[Kullanıcı erişim belirteci](../../access-tokens.md) hızlı başlangıç bölümünde oluşturduğunuz kullanıcıyı alır.

Yanıt, `chatThreadClient` oluşturulan sohbet iş parçacığı üzerinde işlem gerçekleştirmek için kullanılır: sohbet iş parçacığına katılımcılar ekleme, ileti gönderme, ileti silme vb. `chatThreadId` Sohbet iş parçacığının BENZERSIZ kimliği olan bir özelliği içerir. Özelliğine public method. getChatThreadId () tarafından erişilebilir.

```Java
List<ChatParticipant> participants = new ArrayList<ChatParticipant>();

ChatParticipant firstThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(firstUser)
    .setDisplayName("Participant Display Name 1");
    
ChatParticipant secondThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(secondUser)
    .setDisplayName("Participant Display Name 2");

participants.add(firstThreadParticipant);
participants.add(secondThreadParticipant);

CreateChatThreadOptions createChatThreadOptions = new CreateChatThreadOptions()
    .setTopic("Topic")
    .setParticipants(participants);
ChatThreadClient chatThreadClient = chatClient.createChatThread(createChatThreadOptions);
String chatThreadId = chatThreadClient.getChatThreadId();
```

## <a name="send-a-message-to-a-chat-thread"></a>Sohbet iş parçacığına ileti gönderin

`sendMessage`ChatThreadId tarafından tanımlanan, yeni oluşturduğunuz iş parçacığına bir ileti göndermek için yöntemini kullanın.
`sendChatMessageOptions` Sohbet iletisi isteğini tanımlamakta kullanılır.

- `content`Sohbet iletisi içeriğini sağlamak için kullanın.
- `type`Sohbet iletisi içerik türünü, metnini veya HTML 'yi belirtmek için kullanın.
- `senderDisplayName`Gönderenin görünen adını belirtmek için kullanın.

Yanıt, `sendChatMessageResult` `id` ILETININ benzersiz kimliği olan bir içerir.

```Java
SendChatMessageOptions sendChatMessageOptions = new SendChatMessageOptions()
    .setContent("Message content")
    .setType(ChatMessageType.TEXT)
    .setSenderDisplayName("Sender Display Name");

SendChatMessageResult sendChatMessageResult = chatThreadClient.sendMessage(sendChatMessageOptions);
String chatMessageId = sendChatMessageResult.getId();
```


## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al

`getChatThreadClient`Yöntemi, zaten var olan bir iş parçacığı için bir iş parçacığı istemcisi döndürür. Oluşturulan iş parçacığında işlem gerçekleştirmek için kullanılabilir: katılımcı ekleme, ileti gönderme vb. `chatThreadId` , mevcut sohbet iş parçacığının benzersiz KIMLIĞIDIR.

```Java
String chatThreadId = "Id";
ChatThread chatThread = chatClient.getChatThread(chatThreadId);
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Sohbet iş parçacığından sohbet iletileri alma

Sohbet `listMessages` iş parçacığı istemcisinde belirtilen aralıklarda yöntemi yoklayarak sohbet iletileri alabilirsiniz.

```Java
chatThreadClient.listMessages().iterableByPage().forEach(resp -> {
    System.out.printf("Response headers are %s. Url %s  and status code %d %n", resp.getHeaders(),
        resp.getRequest().getUrl(), resp.getStatusCode());
    resp.getItems().forEach(message -> {
        System.out.printf("Message id is %s.", message.getId());
    });
});
```

`listMessages` ileti için. editMessage () ve. deleteMessage () kullanılarak gerçekleşen tüm düzenleme veya silme işlemleri dahil olmak üzere iletinin en son sürümünü döndürür. Silinen iletiler için, `chatMessage.getDeletedOn()` iletinin silindiğini gösteren bir tarih saat değeri döndürür. Düzenlenen iletiler için, `chatMessage.getEditedOn()` iletinin ne zaman düzenlendiğini gösteren bir tarih saat döndürür. İleti oluşturma özgün zamanına kullanılarak erişilebilir `chatMessage.getCreatedOn()` ve iletileri sıralamak için kullanılabilir.

`listMessages` tarafından tanımlanabilecek farklı ileti türlerini döndürür `chatMessage.getType()` . Bu türler şunlardır:

- `text`: Bir iş parçacığı katılımcısı tarafından gönderilen düzenli sohbet iletisi.

- `html`: Bir iş parçacığı katılımcısı tarafından gönderilen HTML sohbet iletisi.

- `topicUpdated`: Konunun güncelleştirildiğini belirten sistem iletisi.

- `participantAdded`: Sohbet iş parçacığına bir veya daha fazla katılımcı eklendiğini belirten sistem iletisi.

- `participantRemoved`: Bir katılımcının sohbet iş parçacığından kaldırıldığını belirten sistem iletisi.

Daha ayrıntılı bilgi için bkz. [Ileti türleri](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-participant-to-the-chat-thread"></a>Sohbet iş parçacığına katılımcı olarak Kullanıcı ekleme

Sohbet iş parçacığı oluşturulduktan sonra, bundan sonra kullanıcı ekleyebilir ve kaldırabilirsiniz. Kullanıcıları ekleyerek, onlara sohbet iş parçacığına ileti gönderme ve başka katılımcılar ekleme/kaldırma erişimi verirsiniz. Bu Kullanıcı için yeni bir erişim belirteci ve kimliği alarak başlamanız gerekir. Addkatılımcılar yöntemini çağırmadan önce, bu kullanıcı için yeni bir erişim belirteci ve kimliği aldığınızdan emin olun. Kullanıcının sohbet istemcisini başlatması için bu erişim belirtecine ihtiyacı olacak.

`addParticipants`ThreadID tarafından tanımlanan iş parçacığına katılımcılar eklemek için yöntemini kullanın.

- `listParticipants`Sohbet iş parçacığına eklenecek katılımcıları listelemek için kullanın.
- `communicationIdentifier`, gerekli, [Kullanıcı erişim belirteci](../../access-tokens.md) hızlı başlangıç bölümünde Communicationıdentityclient tarafından oluşturduğunuz Communicationıdentifier.
- `display_name`, isteğe bağlı, iş parçacığı katılımcısı için görünen addır.
- `share_history_time`, isteğe bağlı, sohbet geçmişinin katılımcının paylaştığı süredir. Sohbet iş parçacığının başlatılmasından bu yana geçmişi paylaşmak için, bu özelliği, iş parçacığı oluşturma zamanından daha küçük veya ona eşit bir tarih olarak ayarlayın. Katılımcının eklendiği tarihten önce geçmiş paylaşmak için, geçerli tarih olarak ayarlayın. Kısmi geçmişi paylaşmak için, bunu gerekli tarihe ayarlayın.

```Java
List<ChatParticipant> participants = new ArrayList<ChatParticipant>();

ChatParticipant firstThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(identity1)
    .setDisplayName("Display Name 1");

ChatParticipant secondThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(identity2)
    .setDisplayName("Display Name 2");

participants.add(firstThreadParticipant);
participants.add(secondThreadParticipant);

AddChatParticipantsOptions addChatParticipantsOptions = new AddChatParticipantsOptions()
    .setParticipants(participants);
chatThreadClient.addParticipants(addChatParticipantsOptions);
```

## <a name="remove-participant-from-a-chat-thread"></a>Bir sohbet iş parçacığından katılımcı kaldırma

Bir iş parçacığına katılımcı eklemeye benzer şekilde, katılımcıları bir sohbet iş parçacığından kaldırabilirsiniz. Bunu yapmak için, eklediğiniz katılımcıların kimliklerini izlemeniz gerekir.

`removeParticipant`Öğesini kullanın, burada `identifier` oluşturduğunuz Communicationıdentifier ' dır.

```Java
chatThreadClient.removeParticipant(identity);
```

## <a name="run-the-code"></a>Kodu çalıştırma

*pom.xml* dosyasını içeren dizine gidin ve aşağıdaki komutu kullanarak projeyi derleyin `mvn` .

```console
mvn compile
```

Ardından, paketini oluşturun.

```console
mvn package
```

`mvn`Uygulamayı yürütmek için aşağıdaki komutu çalıştırın.

```console
mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false
```