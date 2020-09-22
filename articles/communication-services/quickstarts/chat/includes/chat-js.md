---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: a50f09dd52ccf4d35fd3803967898a4a25e3299f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948080"
---
## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunları yaptığınızdan emin olun:

- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Node.js](https://nodejs.org/en/download/) etkin LTS ve bakım LTS sürümlerini (8.11.1 ve 10.14.1 önerilir) yükler.
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../../create-communication-resource.md). Bu hızlı başlangıç için kaynak **uç** noktanızı kaydetmeniz gerekir.
- Bir [Kullanıcı erişim belirteci](../../access-tokens.md). Kapsamı "sohbet" olarak ayarladığınızdan emin olun ve belirteç dizesinin yanı sıra Kullanıcı kimliği dizesini de unutmayın.

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

Proje kök dizininde **start-chat.js** adlı bir dosya oluşturmak için bir metin düzenleyici kullanın. Bu hızlı başlangıç için tüm kaynak kodu aşağıdaki bölümlerde bu dosyaya ekleyeceksiniz.

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

Bu hızlı başlangıç, uygulama varlıklarını paketleyip Web Pack kullanır. WebPack, WebPack-CLI ve WebPack-dev-Server NPM paketlerini yüklemek için aşağıdaki komutu çalıştırın ve bunları ** üzerindepackage.js**geliştirme bağımlılıkları olarak listeleyin:

```console
npm install webpack webpack-cli webpack-dev-server --save-dev
```

Projenizin kök dizininde bir **index.html** dosyası oluşturun. JavaScript için Azure Iletişim sohbeti istemci kitaplığı 'nı kullanarak sohbet özelliği eklemek için bu dosyayı şablon olarak kullanacağız.

Kod aşağıdaki gibidir:

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

Web uygulamanızda bir sohbet istemcisi oluşturmak için, Iletişim hizmeti uç noktasını ve önkoşul adımlarının bir parçası olarak oluşturulan erişim belirtecini kullanacaksınız. Kullanıcı erişimi belirteçleri, Azure Iletişim hizmetlerinde doğrudan kimlik doğrulayan istemci uygulamaları oluşturmanızı sağlar. Bu belirteçleri sunucunuzda oluşturduktan sonra bunları bir istemci cihazına geri geçirin. `AzureCommunicationUserCredential` `Common client library` Belirteci sohbet istemcinize geçirmek için içinden sınıfını kullanmanız gerekir.

```JavaScript

import { ChatClient } from '@azure/communication-chat';
import { AzureCommunicationUserCredential } from '@azure/communication-common';

// Your unique Azure Communication service endpoint
let endpointUrl = 'https://<RESOURCE_NAME>.communication.azure.com';
let userAccessToken = '<USER_ACCESS_TOKEN>';

let chatClient = new ChatClient(endpointUrl, new AzureCommunicationUserCredential(userAccessToken));
console.log('Azure Communication Chat client created!');
```
**Bitiş noktasını** , [Azure iletişim kaynağı oluşturma](../../create-communication-resource.md) belgelerine bağlı olarak, önce oluşturulan bir ile değiştirin.
**USER_ACCESS_TOKEN** , [Kullanıcı erişim belirteci](../../access-tokens.md) belgelerine göre verilen bir belirteç ile değiştirin.
Bu kodu **client.js** dosyasına ekleyin


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
- `members`Sohbet iş parçacığına eklenecek üyeleri listelemek için kullanın;

Çözümlendiğinde,, `createChatThread` `threadId` sohbet iş parçacığına üye ekleme, ileti gönderme, ileti silme vb. gibi yeni oluşturulan sohbet iş parçacığı üzerinde işlem gerçekleştirmek için kullanılan yöntemi döndürür.

```Javascript
async function createChatThread() {
   let createThreadRequest = {
       topic: 'Preparation for London conference',
       members: [{
                   user: { communicationUserId: '<USER_ID_FOR_JACK>' },
                   displayName: 'Jack'
               }, {
                   user: { communicationUserId: '<USER_ID_FOR_GEETA>' },
                   displayName: 'Geeta'
               }]
   };
   let chatThreadClient= await chatClient.createChatThread(createThreadRequest);
   let threadId = chatThreadClient.threadId;
   return threadId;
}

createChatThread().then(async threadId => {
   console.log(`Thread created:${threadId}`);
   // PLACEHOLDERS
   // <CREATE CHAT THREAD CLIENT>
   // <RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>
   // <SEND MESSAGE TO A CHAT THREAD>
   // <LIST MESSAGES IN A CHAT THREAD>
   // <ADD NEW MEMBER TO THREAD>
   // <LIST MEMBERS IN A THREAD>
   // <REMOVE MEMBER FROM THREAD>
});
```

**USER_ID_FOR_JACK** ve **USER_ID_FOR_GEETA** önceki adımdan elde edilen Kullanıcı kimlikleriyle değiştirin (Kullanıcı oluşturma ve [Kullanıcı erişim belirteçleri](../../access-tokens.md)verme)

Tarayıcı sekmelerinizi yenilediğinizde, konsolunda aşağıdakileri görmeniz gerekir
```console
Thread created: <threadId>
```

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al

`getChatThreadClient`Yöntemi, `chatThreadClient` zaten var olan bir iş parçacığı için bir döndürür. Oluşturulan iş parçacığında işlem gerçekleştirmek için kullanılabilir: üye ekleme, ileti gönderme vb. ThreadId, mevcut sohbet iş parçacığının benzersiz KIMLIĞIDIR.

```JavaScript

let chatThreadClient = await chatClient.getChatThreadClient(threadId);
console.log(`Chat Thread client for threadId:${chatThreadClient.threadId}`);

```
Bu kodu `<CREATE CHAT THREAD CLIENT>` **client.js**açıklamanın yerine ekleyin, tarayıcı sekmesini yenileyin ve konsolu kontrol edin, şunları görmeniz gerekir:
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

Yanıt, `sendChatMessageResult` Bu iletinin BENZERSIZ kimliği olan bir "kimlik" içeriyor.

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
Bu kodu `<SEND MESSAGE TO A CHAT THREAD>` **client.js**açıklamanın yerine ekleyin, tarayıcı sekmesini yenileyin ve konsolu denetleyin.
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
Bu kodu `<RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>` **client.js**yorum yerine ekleyin.
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
Bu kodu `<LIST MESSAGES IN A CHAT THREAD>` **client.js**açıklamanın yerine ekleyin.
Sekmeden yenileyin, konsolunda bu sohbet iş parçacığında gönderilen iletilerin listesini bulmanız gerekir.


`listMessages` ve kullanarak iletide gerçekleşen tüm düzenleme veya silme işlemleri dahil olmak üzere iletinin en son sürümünü döndürür `updateMessage` `deleteMessage` .
Silinen iletiler için `chatMessage.deletedOn` , iletinin silindiğini gösteren bir tarih saat değeri döndürür. Düzenlenen iletiler için, `chatMessage.editedOn` iletinin ne zaman düzenlendiğini gösteren bir tarih saat döndürür. İleti oluşturmaya yönelik özgün saate, `chatMessage.createdOn` iletileri sıralamak için kullanılabilecek kullanılarak erişilebilir.

`listMessages` tarafından tanımlanabilecek farklı ileti türlerini döndürür `chatMessage.type` . Bu türler şunlardır:

- `Text`: Bir iş parçacığı üyesi tarafından gönderilen normal sohbet iletisi.

- `ThreadActivity/TopicUpdate`: Konunun güncelleştirildiğini belirten sistem iletisi.

- `ThreadActivity/AddMember`: Sohbet iş parçacığına bir veya daha fazla üye eklendiğini belirten sistem iletisi.

- `ThreadActivity/RemoveMember`: Bir üyenin sohbet iş parçacığından kaldırıldığını belirten sistem iletisi.

Daha ayrıntılı bilgi için bkz. [Ileti türleri](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-member-to-the-chat-thread"></a>Sohbet iş parçacığına üye olarak Kullanıcı ekleme

Sohbet iş parçacığı oluşturulduktan sonra, bundan sonra kullanıcı ekleyebilir ve kaldırabilirsiniz. Kullanıcıları ekleyerek, onlara sohbet iş parçacığına ileti gönderme ve diğer üyeleri ekleme/kaldırma erişimi verirsiniz. Yöntemi çağırmadan önce `addMembers` , bu kullanıcı için yeni bir erişim belirteci ve kimliği aldığınızdan emin olun. Kullanıcının sohbet istemcisini başlatması için bu erişim belirtecine ihtiyacı olacak.

`addMembersRequest``members`sohbet iş parçacığına eklenecek üyeleri listeleyen istek nesnesini açıklar;
- `user`, gerekli, sohbet iş parçacığına eklenecek iletişim Kullanıcı.
- `displayName`, isteğe bağlı, iş parçacığı üyesi için görünen addır.
- `shareHistoryTime`, isteğe bağlı, sohbet geçmişinin üye ile paylaşıldıkları süredir. Sohbet iş parçacığının başlatılmasından bu yana geçmişi paylaşmak için, bu özelliği, iş parçacığı oluşturma zamanından daha küçük veya ona eşit bir tarih olarak ayarlayın. Üyenin eklendiği tarihten önce hiçbir geçmiş paylaşmak için, geçerli tarih olarak ayarlayın. Kısmi geçmişi paylaşmak için, bunu tercih ettiğiniz tarihe ayarlayın.

```JavaScript

let addMembersRequest =
{
    members: [
        {
            user: { communicationUserId: '<NEW_MEMBER_USER_ID>' },
            displayName: 'Jane'
        }
    ]
};

await chatThreadClient.addMembers(addMembersRequest);

```
**NEW_MEMBER_USER_ID** [Yeni bir kullanıcı kimliğiyle](../../access-tokens.md) değiştirin `<ADD NEW MEMBER TO THREAD>` **client.js** yorum yerine bu kodu ekleyin

## <a name="list-users-in-a-chat-thread"></a>Sohbet iş parçacığında kullanıcıları listeleme
```JavaScript
async function listThreadMembers() {
   let pagedAsyncIterableIterator = await chatThreadClient.listMembers();
   let next = await pagedAsyncIterableIterator.next();
   while (!next.done) {
      let user = next.value;
      console.log(`User :${user.displayName}`);
      next = await pagedAsyncIterableIterator.next();
   }
}
await listThreadMembers();
```
client.jsaçıklama yerine bu kodu ekleyin `<LIST MEMBERS IN A THREAD>` , tarayıcı sekmesini ** **yenileyin ve konsolu kontrol edin, bir iş parçacığında kullanıcılar hakkında bilgi görmeniz gerekir.

## <a name="remove-user-from-a-chat-thread"></a>Kullanıcı sohbet iş parçacığından kaldır

Üye eklemeye benzer şekilde, bir sohbet iş parçacığından üye kaldırabilirsiniz. ' I kaldırmak için, eklediğiniz üyelerin kimliklerini izlemeniz gerekir.

`removeMember` `member` İş parçacığından kaldırılacak iletişim kullanıcısının olduğu yöntemi kullanın.

```JavaScript

await chatThreadClient.removeMember({ communicationUserId: <MEMBER_ID> });
await listThreadMembers();
```
**MEMBER_ID** , önceki adımda kullanılan BIR kullanıcı kimliğiyle değiştirin (<NEW_MEMBER_USER_ID>).
Bu kodu `<REMOVE MEMBER FROM THREAD>` **client.js**açıklamanın yerine ekleyin,
