---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 2/16/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 7e62bbc5929eaf23a9b7be12de222105bc2529cd
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100653593"
---
## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunları yaptığınızdan emin olun:

- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Android Studio](https://developer.android.com/studio)yüklemek için Android Studio kullanacağız, böylece hızlı başlangıç Için bir Android uygulaması oluşturun.
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../../create-communication-resource.md). Bu hızlı başlangıç için **Kaynak uç noktanızı kaydetmeniz** gerekir.
- **İki** iletişim hizmeti kullanıcısı oluşturun ve kullanıcılara bir Kullanıcı erişim belirteci [Kullanıcı erişim belirteci](../../access-tokens.md)verin. Kapsamı **sohbet** olarak ayarladığınızdan emin olun ve **belirteç dizesinin yanı sıra Kullanıcı kimliği dizesini de unutmayın**. Bu hızlı başlangıçta, ilk katılımcı içeren bir iş parçacığı oluşturacağız ve sonra iş parçacığına ikinci bir katılımcı ekleyeceğiz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-android-application"></a>Yeni bir Android uygulaması oluşturma

1. Android Studio açın ve öğesini seçin `Create a new project` . 
2. Sonraki pencerede `Empty Activity` proje şablonu olarak öğesini seçin.
3. Seçenekleri seçerken `ChatQuickstart` Proje adı olarak girin.
4. İleri ' ye tıklayın ve projenin oluşturulmasını istediğiniz dizini seçin.

### <a name="install-the-libraries"></a>Kitaplıkları yükler

Gerekli Iletişim Hizmetleri bağımlılıklarını yüklemek için Gradle kullanacağız. Komut satırında, projenin kök dizininin içinde gezinin `ChatQuickstart` . Uygulamanın Build. Gradle dosyasını açın ve şu bağımlılıkları `ChatQuickstart` hedefe ekleyin:

```
implementation 'com.azure.android:azure-communication-common:1.0.0-beta.5'
implementation 'com.azure.android:azure-communication-chat:1.0.0-beta.5'
```

Android Studio ' Şimdi Eşitle ' seçeneğine tıklayın.

#### <a name="alternative-to-install-libraries-through-maven"></a>Yapıyı Maven aracılığıyla kitaplıkları yüklemek için
[Maven](https://maven.apache.org/) derleme sistemini kullanarak kitaplığı projenize aktarmak için, `dependencies` `pom.xml` Yapı kimliğini ve kullanmak istediğiniz sürümü belirterek onu uygulamanızın dosyasının bölümüne ekleyin:

```xml
<dependency>
  <groupId>com.azure.android</groupId>
  <artifactId>azure-communication-chat</artifactId>
  <version>1.0.0-beta.5</version>
</dependency>
```


### <a name="setup-the-placeholders"></a>Yer tutucuları ayarlama

Dosyayı açın ve düzenleyin `MainActivity.java` . Bu hızlı başlangıçta, kodumuzu ' a ekleyecek `MainActivity` ve çıktıyı konsolda görüntülemesi gerekir. Bu hızlı başlangıç, bir kullanıcı arabirimi oluşturmayı gidermez. Dosyanın en üstünde, ve kitaplıklarını içeri aktarın `Communication common` `Communication chat` :

```
import com.azure.android.communication.chat.*;
import com.azure.android.communication.common.*;
```

Aşağıdaki kodu dosyasına kopyalayın `MainActivity` :

```
   @Override
    protected void onStart() {
        super.onStart();
        try {
            // <CREATE A CHAT CLIENT>

            // <CREATE A CHAT THREAD>

            // <CREATE A CHAT THREAD CLIENT>

            // <SEND A MESSAGE>

            // <ADD A USER>

            // <LIST USERS>

            // <REMOVE A USER>
        } catch (Exception e){
            System.out.println("Quickstart failed: " + e.getMessage());
        }
    }
```

Aşağıdaki adımlarda, Azure Iletişim Hizmetleri sohbet kitaplığı 'nı kullanarak yer tutucuları örnek kodla değiştireceksiniz.


### <a name="create-a-chat-client"></a>Sohbet istemcisi oluşturma

Yorumu `<CREATE A CHAT CLIENT>` aşağıdaki kodla değiştirin (içeri aktarma deyimlerini dosyanın en üstüne koyun):

```java
import com.azure.android.communication.chat.ChatClient;
import com.azure.android.core.http.HttpHeader;

final String endpoint = "https://<resource>.communication.azure.com";
final String userAccessToken = "<user_access_token>";

ChatClient client = new ChatClient.Builder()
    .endpoint(endpoint)
    .credentialInterceptor(chain -> chain.proceed(chain.request()
        .newBuilder()
        .header(HttpHeader.AUTHORIZATION, userAccessToken)
        .build());
```

1. `AzureCommunicationChatServiceAsyncClient.Builder`Bir örneğini yapılandırmak ve oluşturmak için öğesini kullanın `AzureCommunicationChatClient` .
2. `<resource>`Iletişim Hizmetleri kaynağı ile değiştirin.
3. `<user_access_token>`Geçerli bir Iletişim Hizmetleri erişim belirteciyle değiştirin.

## <a name="object-model"></a>Nesne modeli
Aşağıdaki sınıflar ve arabirimler, JavaScript için Azure Iletişim Hizmetleri sohbet istemci kitaplığı 'nın bazı önemli özelliklerinden bazılarını işler.

| Ad                                   | Açıklama                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient | Bu sınıf, sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| ChatThreadClient | Bu sınıf, sohbet Iş parçacığı işlevselliği için gereklidir. ChatClient aracılığıyla bir örnek edinirsiniz ve bu örneği kullanarak ileti gönderebilir/alabilir/güncelleştirebilir/silebilirsiniz, kullanıcıları ekleyin/kaldırın/alın, yazma bildirimleri gönderin ve okundu bilgilerini okuyun, sohbet etkinliklerine abone olun. |

## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

`ChatClient`İlk Kullanıcı ile yeni bir iş parçacığı oluşturmak için sitemizi kullanacağız.

`<CREATE A CHAT THREAD>` açıklamasını aşağıdaki kodla değiştirin:

```java
//  The list of ChatParticipant to be added to the thread.
List<ChatParticipant> participants = new ArrayList<>();
// The communication user ID you created before, required.
final String id = "<user_id>";
// The display name for the thread participant.
final String displayName = "initial participant";
participants.add(new ChatParticipant()
    .setId(id)
    .setDisplayName(displayName));

// The topic for the thread.
final String topic = "General";
// The model to pass to the create method.
CreateChatThreadRequest thread = new CreateChatThreadRequest()
    .setTopic(topic)
    .setParticipants(participants);

// optional, set a repeat request ID 
final String repeatabilityRequestID = '123';

client.createChatThread(thread, repeatabilityRequestID, new Callback<CreateChatThreadResult>() {
    public void onSuccess(CreateChatThreadResult result, okhttp3.Response response) {
        // MultiStatusResponse is the result returned from creating a thread.
        // It has a 'multipleStatus' property which represents a list of IndividualStatusResponse.
        String threadId;
        List<IndividualStatusResponse> statusList = result.getMultipleStatus();
        for (IndividualStatusResponse status : statusList) {
            if (status.getId().endsWith("@thread.v2")
                && status.getType().contentEquals("Thread")) {
                threadId = status.getId();
                break;
            }
        }
        // Take further action.
    }

    public void onFailure(Throwable throwable, okhttp3.Response response) {
        // Handle error.
    }
});
```

`<user_id>`Geçerli bir Iletişim Hizmetleri Kullanıcı kimliğiyle değiştirin. `threadId`Sonraki adımlarda, tamamlanma işleyicisine döndürülen yanıtı kullanacağız.

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al

Artık bir sohbet iş parçacığı oluşturduğumuz `ChatThreadClient` , iş parçacığı içinde işlem gerçekleştirmek için bir de elde eteceğiz. `<CREATE A CHAT THREAD CLIENT>` açıklamasını aşağıdaki kodla değiştirin:

```
ChatThreadClient threadClient =
        new ChatThreadClient.Builder()
            .endpoint(<endpoint>))
            .build();
```

`<endpoint>`Iletişim Hizmetleri uç noktanız ile değiştirin.

## <a name="send-a-message-to-a-chat-thread"></a>Sohbet iş parçacığına ileti gönderin

`<SEND A MESSAGE>` açıklamasını aşağıdaki kodla değiştirin:

```java
// The chat message content, required.
final String content = "Test message 1";
// The display name of the sender, if null (i.e. not specified), an empty name will be set.
final String senderDisplayName = "An important person";
SendChatMessageRequest message = new SendChatMessageRequest()
    .setType(ChatMessageType.TEXT)
    .setContent(content)
    .setSenderDisplayName(senderDisplayName);

// The unique ID of the thread.
final String threadId = "<thread_id>";
threadClient.sendChatMessage(threadId, message, new Callback<String>() {
    @Override
    public void onSuccess(String messageId, Response response) {
        // A string is the response returned from sending a message, it is an id, 
        // which is the unique ID of the message.
        final String chatMessageId = messageId;
        // Take further action.
    }

    @Override
    public void onFailure(Throwable throwable, Response response) {
        // Handle error.
    }
});
```

`<thread_id>`İletiyi gönderen iş parçacığı kimliğiyle değiştirin.

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Sohbet iş parçacığına katılımcı olarak Kullanıcı ekleme

`<ADD A USER>` açıklamasını aşağıdaki kodla değiştirin:

```java
//  The list of ChatParticipant to be added to the thread.
List<ChatParticipant> participants = new ArrayList<>();
// The CommunicationUser.identifier you created before, required.
final String id = "<user_id>";
// The display name for the thread participant.
final String displayName = "a new participant";
participants.add(new ChatParticipant().setId(id).setDisplayName(displayName));
// The model to pass to the add method.
AddChatParticipantsRequest participants = new AddChatParticipantsRequest()
    .setParticipants(participants);

// The unique ID of the thread.
final String threadId = "<thread_id>";
threadClient.addChatParticipants(threadId, participants, new Callback<Void>() {
    @Override
    public void onSuccess(Void result, Response response) {
        // Take further action.
    }

    @Override
    public void onFailure(Throwable throwable, Response response) {
        // Handle error.
    }
});
```

1. `<user_id>`Eklenecek kullanıcının Iletişim Hizmetleri Kullanıcı kimliğiyle değiştirin. 
2. `<thread_id>`Kullanıcının eklemekte olduğu iş parçacığı kimliğiyle değiştirin.

## <a name="list-users-in-a-thread"></a>İş parçacığında kullanıcıları listeleme

`<LIST USERS>` açıklamasını aşağıdaki kodla değiştirin:

```java
// The unique ID of the thread.
final String threadId = "<thread_id>";

// The maximum number of participants to be returned per page, optional.
final int maxPageSize = 10;

// Skips participants up to a specified position in response.
final int skip = 0;

threadClient.listChatParticipantsPages(threadId,
    maxPageSize,
    skip,
    new Callback<AsyncPagedDataCollection<ChatParticipant, Page<ChatParticipant>>>() {
    @Override
    public void onSuccess(AsyncPagedDataCollection<ChatParticipant, Page<ChatParticipant>> firstPage,
        Response response) {
        // pageCollection enables enumerating list of chat participants.
        pageCollection.getFirstPage(new Callback<Page<ChatParticipant>>() {
            @Override
            public void onSuccess(Page<ChatParticipant> firstPage, Response response) {
                for (ChatParticipant participant : firstPage.getItems()) {
                    // Take further action.
                }
                retrieveNextParticipantsPages(firstPage.getPageId(), pageCollection);
            }

            @Override
            public void onFailure(Throwable throwable, Response response) {
                // Handle error.
            }
         }
    }

    @Override
    public void onFailure(Throwable throwable, Response response) {
        // Handle error.
    }
});

void listChatParticipantsNext(String nextLink,
    AsyncPagedDataCollection<Page<ChatParticipant>> pageCollection) {
        @Override
        public void onSuccess(Page<ChatParticipant> nextPage, Response response) {
            for (ChatParticipant participant : nextPage.getItems()) {
                // Take further action.
            }
            if (nextPage.getPageId() != null) {
                retrieveNextParticipantsPages(nextPage.getPageId(), pageCollection);
            }
        }

        @Override
        public void onFailure(Throwable throwable, Response response) {
            // Handle error.
        }
}
```

`<thread_id>`Kullanıcıları listelerken iş parçacığı kimliğiyle değiştirin.

## <a name="remove-user-from-a-chat-thread"></a>Kullanıcı sohbet iş parçacığından kaldır

`<REMOVE A USER>` açıklamasını aşağıdaki kodla değiştirin:

```java
// The unique ID of the thread.
final String threadId = "<thread_id>";
// The unique ID of the participant.
final String participantId = "<participant_id>";
threadClient.removeChatParticipant(threadId, participantId, new Callback<Void>() {
    @Override
    public void onSuccess(Void result, Response response) {
        // Take further action.
    }

    @Override
    public void onFailure(Throwable throwable, Response response) {
        // Handle error.
    }
});
```

1. `<thread_id>`Kullanıcıyı kaldıran iş parçacığı kimliğiyle değiştirin.
1. `<participant_id>`Kaldırılmakta olan katılımcının Iletişim Hizmetleri Kullanıcı kimliğiyle değiştirin.

## <a name="run-the-code"></a>Kodu çalıştırma

Android Studio, projeyi derlemek ve çalıştırmak için Çalıştır düğmesine basın. Konsolunda, koddan çıktıyı ve günlükçü çıkışını ChatClient ' dan görüntüleyebilirsiniz.
