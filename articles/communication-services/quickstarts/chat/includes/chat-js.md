---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 4f50bce86b43c83401ac41c59dbd4e5e952d15d1
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100379699"
---
## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunları yaptığınızdan emin olun:

- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Node.js](https://nodejs.org/en/download/) etkin LTS ve bakım LTS sürümlerini (8.11.1 ve 10.14.1 önerilir) yükler.
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../../create-communication-resource.md). Bu hızlı başlangıç için **Kaynak uç noktanızı kaydetmeniz** gerekir.
- *Üç* ACS kullanıcısı oluşturun ve bu kullanıcılara bir Kullanıcı erişim belirteci [Kullanıcı erişim belirteci](../../access-tokens.md)verin. Kapsamı **sohbet** olarak ayarladığınızdan emin olun ve **belirteç dizesinin yanı sıra Kullanıcı kimliği dizesini de unutmayın**. Tam demo, iki ilk katılımcı içeren bir iş parçacığı oluşturur ve sonra iş parçacığına üçüncü bir katılımcı ekler.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-web-application"></a>Yeni bir Web uygulaması oluşturma

İlk olarak, Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir chat-quickstart && cd chat-quickstart
```
   
`npm init -y`Dosya **üzerinde** varsayılan ayarlarla birpackage.jsoluşturmak için öğesini çalıştırın.

```console
npm init -y
```

### <a name="install-the-packages"></a>Paketleri yükler

`npm install`JavaScript için aşağıdaki Iletişim Hizmetleri istemci kitaplıklarını yüklemek için komutunu kullanın.

```console
npm install @azure/communication-common --save

npm install @azure/communication-administration --save

npm install @azure/communication-signaling --save

npm install @azure/communication-chat --save

```

`--save`Seçeneği, kitaplığı dosyadaki **package.js** bir bağımlılık olarak listeler.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Bu hızlı başlangıç, uygulama varlıklarını paketleyip Web Pack kullanır. WebPack, WebPack-CLI ve WebPack-dev-Server NPM paketlerini yüklemek için aşağıdaki komutu çalıştırın ve bunları **üzerindepackage.js** geliştirme bağımlılıkları olarak listeleyin:

```console
npm install webpack webpack-cli webpack-dev-server --save-dev
```

Projenizin kök dizininde bir **index.html** dosyası oluşturun. JavaScript için Azure Iletişim sohbeti istemci kitaplığı 'nı kullanarak sohbet özelliği eklemek için bu dosyayı şablon olarak kullanacağız.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Chat Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Chat Quickstart</h1>
    <script src="./bundle.js"></script>
  </body>
</html>
```

Bu hızlı başlangıç için uygulama mantığını içeren **client.js** adlı projenizin kök dizininde bir dosya oluşturun.

### <a name="create-a-chat-client"></a>Sohbet istemcisi oluşturma

Web uygulamanızda bir sohbet istemcisi oluşturmak için, Iletişim hizmeti **uç noktasını** ve önkoşul adımlarının bir parçası olarak oluşturulan **erişim belirtecini** kullanacaksınız. 

Kullanıcı erişimi belirteçleri, Azure Iletişim hizmetlerinde doğrudan kimlik doğrulayan istemci uygulamaları oluşturmanızı sağlar.

##### <a name="server-vs-client-side"></a>Sunucu ve istemci tarafı karşılaştırması

İstemci uygulamasına ileten bir sunucu tarafı bileşeni kullanarak erişim belirteçleri üretmenizi öneririz. Bu senaryoda, sunucu tarafı kullanıcıları oluşturup yönetmekten ve belirteçlerini yayınmaktan sorumludur. İstemci tarafı bundan sonra hizmetten erişim belirteçleri alabilir ve bunları Azure Iletişim Hizmetleri istemci kitaplıklarının kimliğini doğrulamak için kullanabilir.

Belirteçler, JavaScript için Azure Iletişim yönetimi kitaplığı kullanılarak istemci tarafında da verilebilir. Bu senaryoda, istemci tarafının belirteçleri vermek için kullanıcıların farkında olması gerekir.

Daha ayrıntılı [istemci ve sunucu mimarisi](../../../concepts/client-and-server-architecture.md) için aşağıdaki belgelere bakın

Aşağıdaki diyagramda, istemci tarafı uygulamasının bir güvenilen hizmet katmanından bir erişim belirteci alır. Daha sonra uygulama, Iletişim Hizmetleri kitaplıklarının kimliğini doğrulamak için belirtecini kullanır. Kimlik doğrulandıktan sonra, uygulama artık diğer kullanıcılarla sohbet gibi işlemler gerçekleştirmek için Iletişim Hizmetleri istemci tarafı kitaplıklarını kullanabilir.

:::image type="content" source="../../../media/scenarios/archdiagram-access.png" alt-text="Kullanıcı erişim belirteci mimarisini gösteren diyagram.":::

##### <a name="instructions"></a>Yönergeler
Bu tanıtım, sohbet uygulamanız için bir hizmet katmanı oluşturmayı kapsamaz. 

Kullanıcıları ve bunların belirteçlerini oluşturmadınız, bunu yapmak için buradaki yönergeleri izleyin: [Kullanıcı erişim belirteci](../../access-tokens.md). Kapsamı "VoIP" değil "sohbet" olarak ayarlamayı unutmayın.

İçinde **client.js** JavaScript Için Azure iletişim sohbeti istemci kitaplığını kullanarak sohbet yeteneği eklemek için aşağıdaki kodda uç nokta ve erişim belirtecini kullanın.

```JavaScript

import { ChatClient } from '@azure/communication-chat';
import { AzureCommunicationUserCredential } from '@azure/communication-common';

// Your unique Azure Communication service endpoint
let endpointUrl = 'https://<RESOURCE_NAME>.communication.azure.com';
// The user access token generated as part of the pre-requisites
let userAccessToken = '<USER_ACCESS_TOKEN>';

let chatClient = new ChatClient(endpointUrl, new AzureCommunicationUserCredential(userAccessToken));
console.log('Azure Communication Chat client created!');
```
- **EndpointUrl** değerini iletişim Hizmetleri kaynak uç noktasıyla değiştirin, daha önce yapmadıysanız [Azure iletişim kaynağı oluşturma](../../create-communication-resource.md) bölümüne bakın.
- **Useraccesstoken** değerini, verilen belirteç ile değiştirin.


### <a name="run-the-code"></a>Kodu çalıştırma

`webpack-dev-server`Uygulamanızı derlemek ve çalıştırmak için kullanın. ' Deki uygulama konağını yerel bir Web sunucusuna paketetmek için aşağıdaki komutu çalıştırın:
```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```
Tarayıcınızı açın ve adresine gidin http://localhost:8080/ .
Tarayıcınızın içindeki geliştirici araçları konsolunda aşağıdakileri görmeniz gerekir:

```console
Azure Communication Chat client created!
```

## <a name="object-model"></a>Nesne modeli 
Aşağıdaki sınıflar ve arabirimler, JavaScript için Azure Iletişim Hizmetleri sohbet istemci kitaplığı 'nın bazı önemli özelliklerinden bazılarını işler.

| Ad                                   | Açıklama                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient | Bu sınıf, sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| ChatThreadClient | Bu sınıf, sohbet Iş parçacığı işlevselliği için gereklidir. ChatClient aracılığıyla bir örnek edinirsiniz ve bu örneği kullanarak ileti gönderebilir/alabilir/güncelleştirebilir/silebilirsiniz, kullanıcıları ekleyin/kaldırın/alın, yazma bildirimleri gönderin ve okundu bilgilerini okuyun, sohbet etkinliklerine abone olun. |


## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

`createThread`Bir sohbet iş parçacığı oluşturmak için yöntemini kullanın.

`createThreadRequest` iş parçacığı isteğini anlatmak için kullanılır:

- `topic`Bu sohbete bir konu vermek için kullanın; Konu, işlevi kullanılarak sohbet iş parçacığı oluşturulduktan sonra güncelleştirilemeyebilir `UpdateThread` . 
- `participants`Sohbet iş parçacığına eklenecek katılımcıları listelemek için kullanın.

Çözümlendiğinde, `createChatThread` Yöntem bir döndürür `CreateChatThreadResponse` . Bu model `chatThread` , `id` Yeni oluşturulan iş parçacığının öğesine erişebileceğiniz bir özelliği içerir. Daha sonra ' `id` a bir örneğini almak için kullanabilirsiniz `ChatThreadClient` . `ChatThreadClient`Daha sonra, ileti gönderme veya katılımcıları listeleme gibi iş parçacığı içinde işlem yapmak için kullanılabilir.

```JavaScript
async function createChatThread() {
    let createThreadRequest = {
        topic: 'Preparation for London conference',
        participants: [{
                    user: { communicationUserId: '<USER_ID_FOR_JACK>' },
                    displayName: 'Jack'
                }, {
                    user: { communicationUserId: '<USER_ID_FOR_GEETA>' },
                    displayName: 'Geeta'
                }]
    };
    let createThreadResponse = await chatClient.createChatThread(createThreadRequest);
    let threadId = createThreadResponse.chatThread.id;
    return threadId;
    }

createChatThread().then(async threadId => {
    console.log(`Thread created:${threadId}`);
    // PLACEHOLDERS
    // <CREATE CHAT THREAD CLIENT>
    // <RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>
    // <SEND MESSAGE TO A CHAT THREAD>
    // <LIST MESSAGES IN A CHAT THREAD>
    // <ADD NEW PARTICIPANT TO THREAD>
    // <LIST PARTICIPANTS IN A THREAD>
    // <REMOVE PARTICIPANT FROM THREAD>
    });
```

**USER_ID_FOR_JACK** ve **USER_ID_FOR_GEETA** , Kullanıcı ve belirteçler oluşturma ([Kullanıcı erişimi belirteçleri](../../access-tokens.md)) ile elde edilen Kullanıcı kimlikleriyle değiştirin

Tarayıcı sekmelerinizi yenilediğinizde konsolunda aşağıdakileri görmeniz gerekir:
```console
Thread created: <thread_id>
```

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al

`getChatThreadClient`Yöntemi, `chatThreadClient` zaten var olan bir iş parçacığı için bir döndürür. Oluşturulan iş parçacığında işlem gerçekleştirmek için kullanılabilir: katılımcı ekleme, ileti gönderme vb. ThreadId, mevcut sohbet iş parçacığının benzersiz KIMLIĞIDIR.

```JavaScript
let chatThreadClient = await chatClient.getChatThreadClient(threadId);
console.log(`Chat Thread client for threadId:${threadId}`);

```
Bu kodu `<CREATE CHAT THREAD CLIENT>` **client.js** açıklamanın yerine ekleyin, tarayıcı sekmesini yenileyin ve konsolu kontrol edin, şunları görmeniz gerekir:
```console
Chat Thread client for threadId: <threadId>
```

## <a name="send-a-message-to-a-chat-thread"></a>Sohbet iş parçacığına ileti gönderin

`sendMessage`Az önce oluşturduğunuz iş parçacığına, ThreadID tarafından tanımlanan bir sohbet iletisi göndermek için yöntemini kullanın.

`sendMessageRequest` Sohbet iletisi isteğinin gerekli alanlarını açıklar:

- `content`Sohbet iletisi içeriğini sağlamak için kullanın;

`sendMessageOptions` Sohbet iletisi isteğinin isteğe bağlı alanlarını açıklar:

- `priority`' Normal ' veya ' yüksek ' gibi sohbet iletisi öncelik düzeyini belirtmek için kullanın; Bu özellik, uygulamanızdaki alıcı kullanıcısına ileti üzerinde ilgilenmeniz veya özel iş mantığını yürütmek için Kullanıcı arabirimi göstergesinin olması için kullanılabilir.   
- `senderDisplayName`Gönderenin görünen adını belirtmek için kullanın;

Yanıt, `sendChatMessageResult` Bu iletinin BENZERSIZ kimliği olan BIR kimlik içerir.

```JavaScript

let sendMessageRequest =
{
    content: 'Hello Geeta! Can you share the deck for the conference?'
};
let sendMessageOptions =
{
    priority: 'Normal',
    senderDisplayName : 'Jack'
};
let sendChatMessageResult = await chatThreadClient.sendMessage(sendMessageRequest, sendMessageOptions);
let messageId = sendChatMessageResult.id;
console.log(`Message sent!, message id:${messageId}`);

```
Bu kodu `<SEND MESSAGE TO A CHAT THREAD>` **client.js** açıklamanın yerine ekleyin, tarayıcı sekmesini yenileyin ve konsolu denetleyin.
```console
Message sent!, message id:<number>
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Sohbet iş parçacığından sohbet iletileri alma

Gerçek zamanlı sinyalle, yeni gelen iletileri dinlemek ve bellekteki geçerli iletileri uygun şekilde güncelleştirmek üzere abone olabilirsiniz. Azure Iletişim Hizmetleri, [abone olabileceğiniz olayların bir listesini](../../../concepts/chat/concepts.md#real-time-signaling)destekler.

```JavaScript
// open notifications channel
await chatClient.startRealtimeNotifications();
// subscribe to new notification
chatClient.on("chatMessageReceived", (e) => {
    console.log("Notification chatMessageReceived!");
    // your code here
});

```
Bu kodu `<RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>` **client.js** yorum yerine ekleyin.
Tarayıcı sekmesini yenileyin, konsolda bir ileti görürsünüz `Notification chatMessageReceived` ;

Alternatif olarak, belirtilen aralıklarda yöntemi yoklayarak sohbet iletileri alabilirsiniz `listMessages` . 

```JavaScript

let pagedAsyncIterableIterator = await chatThreadClient.listMessages();
let nextMessage = await pagedAsyncIterableIterator.next();
    while (!nextMessage.done) {
        let chatMessage = nextMessage.value;
        console.log(`Message :${chatMessage.content}`);
        // your code here
        nextMessage = await pagedAsyncIterableIterator.next();
    }

```
Bu kodu `<LIST MESSAGES IN A CHAT THREAD>` **client.js** açıklamanın yerine ekleyin.
Sekmeden yenileyin, konsolunda bu sohbet iş parçacığında gönderilen iletilerin listesini bulmanız gerekir.


`listMessages` ve kullanarak iletide gerçekleşen tüm düzenleme veya silme işlemleri dahil olmak üzere iletinin en son sürümünü döndürür `updateMessage` `deleteMessage` .
Silinen iletiler için `chatMessage.deletedOn` , iletinin silindiğini gösteren bir tarih saat değeri döndürür. Düzenlenen iletiler için, `chatMessage.editedOn` iletinin ne zaman düzenlendiğini gösteren bir tarih saat döndürür. İleti oluşturmaya yönelik özgün saate, `chatMessage.createdOn` iletileri sıralamak için kullanılabilecek kullanılarak erişilebilir.

`listMessages` tarafından tanımlanabilecek farklı ileti türlerini döndürür `chatMessage.type` . Bu türler şunlardır:

- `Text`: Bir iş parçacığı katılımcısı tarafından gönderilen düzenli sohbet iletisi.

- `ThreadActivity/TopicUpdate`: Konunun güncelleştirildiğini belirten sistem iletisi.

- `ThreadActivity/AddParticipant`: Sohbet iş parçacığına bir veya daha fazla katılımcı eklendiğini belirten sistem iletisi.

- `ThreadActivity/RemoveParticipant`: Bir katılımcının sohbet iş parçacığından kaldırıldığını belirten sistem iletisi.

Daha ayrıntılı bilgi için bkz. [Ileti türleri](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Sohbet iş parçacığına katılımcı olarak Kullanıcı ekleme

Sohbet iş parçacığı oluşturulduktan sonra, bundan sonra kullanıcı ekleyebilir ve kaldırabilirsiniz. Kullanıcıları ekleyerek, onlara sohbet iş parçacığına ileti gönderme ve başka katılımcılar ekleme/kaldırma erişimi verirsiniz.

Yöntemi çağırmadan önce `addParticipants` , bu kullanıcı için yeni bir erişim belirteci ve kimliği aldığınızdan emin olun. Kullanıcının sohbet istemcisini başlatması için bu erişim belirtecine ihtiyacı olacak.

`addParticipantsRequest``participants`sohbet iş parçacığına eklenecek katılımcıları listeleyen istek nesnesini açıklar;
- `user`, gerekli, sohbet iş parçacığına eklenecek iletişim Kullanıcı.
- `displayName`, isteğe bağlı, iş parçacığı katılımcısı için görünen addır.
- `shareHistoryTime`, isteğe bağlı, sohbet geçmişinin katılımcının paylaştığı süredir. Sohbet iş parçacığının başlatılmasından bu yana geçmişi paylaşmak için, bu özelliği, iş parçacığı oluşturma zamanından daha küçük veya ona eşit bir tarih olarak ayarlayın. Katılımcının eklendiği tarihten önce geçmiş paylaşmak için, geçerli tarih olarak ayarlayın. Kısmi geçmişi paylaşmak için, bunu tercih ettiğiniz tarihe ayarlayın.

```JavaScript

let addParticipantsRequest =
{
    participants: [
        {
            user: { communicationUserId: '<NEW_PARTICIPANT_USER_ID>' },
            displayName: 'Jane'
        }
    ]
};

await chatThreadClient.addParticipants(addParticipantsRequest);

```
**NEW_PARTICIPANT_USER_ID** [yenı bir kullanıcı kimliğiyle](../../access-tokens.md) değiştirin `<ADD NEW PARTICIPANT TO THREAD>` **client.js** yorum yerine bu kodu ekleyin

## <a name="list-users-in-a-chat-thread"></a>Sohbet iş parçacığında kullanıcıları listeleme
```JavaScript
async function listParticipants() {
   let pagedAsyncIterableIterator = await chatThreadClient.listParticipants();
   let next = await pagedAsyncIterableIterator.next();
   while (!next.done) {
      let user = next.value;
      console.log(`User :${user.displayName}`);
      next = await pagedAsyncIterableIterator.next();
   }
}
await listParticipants();
```
client.jsaçıklama yerine bu kodu ekleyin `<LIST PARTICIPANTS IN A THREAD>` , tarayıcı sekmesini **** yenileyin ve konsolu kontrol edin, bir iş parçacığında kullanıcılar hakkında bilgi görmeniz gerekir.

## <a name="remove-user-from-a-chat-thread"></a>Kullanıcı sohbet iş parçacığından kaldır

Katılımcı eklemeye benzer şekilde, bir sohbet iş parçacığından katılımcıları kaldırabilirsiniz. ' I kaldırmak için, eklediğiniz katılımcıların kimliklerini izlemeniz gerekir.

`removeParticipant` `participant` İş parçacığından kaldırılacak iletişim kullanıcısının olduğu yöntemi kullanın.

```JavaScript

await chatThreadClient.removeParticipant({ communicationUserId: <PARTICIPANT_ID> });
await listParticipants();
```
**PARTICIPANT_ID** , önceki adımda kullanılan BIR kullanıcı kimliğiyle değiştirin (<NEW_PARTICIPANT_USER_ID>).
Bu kodu `<REMOVE PARTICIPANT FROM THREAD>` **client.js** açıklamanın yerine ekleyin,
