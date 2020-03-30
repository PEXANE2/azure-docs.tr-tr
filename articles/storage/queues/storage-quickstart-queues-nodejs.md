---
title: 'Hızlı başlangıç: Azure Sıra depolama kitaplığı v12 - JavaScript'
description: Kuyruk oluşturmak ve kuyruğa ileti eklemek için Azure Queue JavaScript v12 kitaplığını nasıl kullanacağınızı öğrenin. Ardından, sıradaki iletileri nasıl okuyup sildiğinizi öğrenirsiniz. Ayrıca bir sırayı nasıl sildiğinizi de öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/13/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.openlocfilehash: 59a5308d2c0a1fa2e1f38f2fe3da3a2cc29448be
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78199793"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-javascript"></a>Quickstart: JavaScript için Azure Queue depolama istemcisi kitaplığı v12

JavaScript için Azure Queue depolama istemcisi kitaplığı sürüm 12 ile başlayın. Azure Sıra depolama, daha sonra alma ve işleme için çok sayıda ileti depolamak için bir hizmettir. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

JavaScript için Azure Kuyruk depolama istemcisi v12'yi kullanın:

* Bir kuyruk oluşturma
* Kuyruğa ileti ekleme
* Kuyruktaki iletilere göz atın
* Kuyruktaki bir iletiyi güncelleştirme
* Kuyruktan ileti alma
* İletileri kuyruktan silme
* Bir kuyruk silme

[API referans belgeleri](https://docs.microsoft.com/javascript/api/@azure/storage-queue/) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-queue) | [Paketi (Düğüm Paket Yöneticisi)](https://www.npmjs.com/package/@azure/storage-queue) | [Örnekleri](https://docs.microsoft.com/azure/storage/common/storage-samples-javascript?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı - [bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için geçerli [Düğüm.js.](https://nodejs.org/en/download/)

## <a name="setting-up"></a>Ayarlama

Bu bölüm, JavaScript için Azure Queue depolama istemcisi kitaplığı v12 ile çalışmak üzere bir proje hazırlamakonusunda size yol sunar.

### <a name="create-the-project"></a>Proje oluşturma

*Kuyruklar-quickstart-v12*adlı bir Düğüm.js uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), proje için yeni bir dizin oluşturun.

    ```console
    mkdir queues-quickstart-v12
    ```

1. Yeni oluşturulan *kuyruklar-quickstart-v12* dizinine geçin.

    ```console
    cd queues-quickstart-v12
    ```

1. *package.json*adlı yeni bir metin dosyası oluşturun. Bu dosya Düğüm.js projesini tanımlar. Bu dosyayı *kuyruklar-quickstart-v12* dizinine kaydedin. İşte dosyanın içeriği:

    ```json
    {
        "name": "queues-quickstart-v12",
        "version": "1.0.0",
        "description": "Use the @azure/storage-queue SDK version 12 to interact with Azure Queue storage",
        "main": "queues-quickstart-v12.js",
        "scripts": {
            "start": "node queues-quickstart-v12.js"
        },
        "author": "Your Name",
        "license": "MIT",
        "dependencies": {
            "@azure/storage-queue": "^12.0.0",
            "@types/dotenv": "^4.0.3",
            "dotenv": "^6.0.0"
        }
    }
    ```

    İstersen sahaya kendi adını `author` koyabilirsin.

### <a name="install-the-package"></a>Paketi yükleyin

Kuyruklarda hala *hızlı başlat-v12* dizinindeyken, komutu kullanarak JavaScript paketi için Azure `npm install` Kuyruk depolama istemcisi kitaplığını yükleyin.

```console
npm install
```

 Bu komut *package.json* dosyasını okur ve JavaScript paketi için Azure Queue depolama istemcisi kitaplığını v12'yi ve bağlı olduğu tüm kitaplıkları yükler.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Kod düzenleyicinizde başka bir yeni metin dosyası açma
1. Azure `require` ve Node.js modüllerini yüklemek için arama ekleme
1. Çok temel özel durum işleme de dahil olmak üzere program için yapı oluşturma

    İşte kod:

    ```javascript
    const { QueueClient } = require("@azure/storage-queue");
    const uuidv1 = require("uuid/v1");

    async function main() {
        console.log("Azure Queue storage v12 - JavaScript quickstart sample");
        // Quick start code goes here
    }

    main().then(() => console.log("\nDone")).catch((ex) => console.log(ex.message));

    ```

1. Yeni dosyayı *kuyruklar-quickstart-v12.js* olarak *sıralar-quickstart-v12* dizininde kaydedin.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Bir sıra iletisi 64 KB boyutuna kadar olabilir. Bir sıra, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle eşzamanlı olarak işlemek için bir çalışma biriktirme listesi oluşturmak için kullanılır. Sıra depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabında bir sıra
* Sıra içindeki iletiler

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Sıra depolama mimarisi diyagramı](./media/storage-queues-introduction/queue1.png)

Bu kaynaklarla etkileşimde kalmak için aşağıdaki JavaScript sınıflarını kullanın:

* [QueueServiceClient](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueserviceclient): `QueueServiceClient` Depolama hesabınızdaki tüm kuyrukları yönetmenize olanak tanır.
* [QueueClient](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient): `QueueClient` Sınıf, tek bir sırayı ve iletilerini yönetmenize ve işlemenize olanak tanır.
* [QueueMessage](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queuemessage): `QueueMessage` Sınıf, bir sırada [receiveMessage'ları](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient?view=azure-node-latest#receivemessages-queuereceivemessageoptions-) ararken döndürülen tek tek nesneleri temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, JavaScript için Azure Kuyruk depolama istemcisi kitaplığıyla aşağıdaki işlemleri nasıl yapacağınızı gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kuyruk oluşturma](#create-a-queue)
* [Kuyruğa ileti ekleme](#add-messages-to-a-queue)
* [Kuyruktaki iletilere göz atın](#peek-at-messages-in-a-queue)
* [Kuyruktaki bir iletiyi güncelleştirme](#update-a-message-in-a-queue)
* [Kuyruktan ileti alma](#receive-messages-from-a-queue)
* [İletileri kuyruktan silme](#delete-messages-from-a-queue)
* [Bir kuyruk silme](#delete-a-queue)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, [depolama bağlantı dizesi bölümüyapılanınoluşturulan](#configure-your-storage-connection-string) ortam değişkeninden depolama hesabının bağlantı dizesini alır.

Bu kodu işlevin `main` içine ekleyin:

```javascript
// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called AZURE_STORAGE_CONNECTION_STRING. If the
// environment variable is created after the application is launched in a
// console or with Visual Studio, the shell or application needs to be 
// closed and reloaded to take the environment variable into account.
const AZURE_STORAGE_CONNECTION_STRING = process.env.AZURE_STORAGE_CONNECTION_STRING;
```

### <a name="create-a-queue"></a>Bir kuyruk oluşturma

Yeni sıra için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için sıra adına bir UUID değerini ekler.

> [!IMPORTANT]
> Sıra adları yalnızca küçük harfler, sayılar ve tireler içerebilir ve bir harf veya sayıyla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır. Ad da 3 ve 63 karakter uzunluğunda olmalıdır. Sıraları adlandırma hakkında daha fazla bilgi için [Bkz.](https://docs.microsoft.com/rest/api/storageservices/naming-queues-and-metadata)

[QueueClient](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızdaki sırayı oluşturmak için [oluşturma](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#create-queuecreateoptions-) yöntemini arayın.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
// Create a unique name for the queue
const queueName = "quickstart" + uuidv1();

console.log("\nCreating queue...");
console.log("\t", queueName);

// Instantiate a QueueClient which will be used to create and manipulate a queue
const queueClient = new QueueClient(AZURE_STORAGE_CONNECTION_STRING, queueName);

// Create the queue
const createQueueResponse = await queueClient.create();
console.log("Queue created, requestId:", createQueueResponse.requestId);
```

### <a name="add-messages-to-a-queue"></a>Kuyruğa ileti ekleme

Aşağıdaki kod snippet [sendMessage](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#sendmessage-string--queuesendmessageoptions-) yöntemini çağırarak sıraya iletiekler. Ayrıca üçüncü `sendMessage` çağrıdan döndürülen [QueueMessage'ı](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queuemessage) da kaydeder. Döndürülen `sendMessageResponse` ileti içeriğini daha sonra programda güncelleştirmek için kullanılır.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
console.log("\nAdding messages to the queue...");

// Send several messages to the queue
await queueClient.sendMessage("First message");
await queueClient.sendMessage("Second message");
const sendMessageResponse = await queueClient.sendMessage("Third message");

console.log("Messages added, requestId:", sendMessageResponse.requestId);
```

### <a name="peek-at-messages-in-a-queue"></a>Kuyruktaki iletilere göz atın

PeekMessages yöntemini arayarak kuyruktaki [iletilere göz atın.](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#peekmessages-queuepeekmessagesoptions-) Yöntem, `peekMessages` sıranın önünden bir veya daha fazla ileti alır, ancak iletinin görünürlüğünü değiştirmez.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
console.log("\nPeek at the messages in the queue...");

// Peek at messages in the queue
const peekedMessages = await queueClient.peekMessages({ numberOfMessages : 5 });

for (i = 0; i < peekedMessages.peekedMessageItems.length; i++) {
    // Display the peeked message
    console.log("\t", peekedMessages.peekedMessageItems[i].messageText);
}
```

### <a name="update-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi güncelleştirme

UpdateMessage yöntemini çağırarak iletinin içeriğini [güncelleştirin.](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#updatemessage-string--string--string--undefined---number--queueupdatemessageoptions-) Yöntem, `updateMessage` iletinin görünürlük zaman anına ve içeriğini değiştirebilir. İleti içeriği 64 KB boyutuna kadar olan UTF-8 kodlanmış bir dize olmalıdır. Yeni içerikle birlikte, `messageId` kodda daha önce kaydedilmiş yanıtı ve yanıtı `popReceipt` iletin. Özellikler `sendMessageResponse` hangi iletiyi güncelleştireceklerini belirler.

```javascript
console.log("\nUpdating the third message in the queue...");

// Update a message using the response saved when calling sendMessage earlier
updateMessageResponse = await queueClient.updateMessage(
    sendMessageResponse.messageId,
    sendMessageResponse.popReceipt,
    "Third message has been updated"
);

console.log("Message updated, requestId:", updateMessageResponse.requestId);
```

### <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

Daha önce eklenen iletileri [receiveMessages](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#receivemessages-queuereceivemessageoptions-) yöntemini arayarak indirin.  `numberOfMessages` Alanında, bu arama için alınması gereken en fazla ileti sayısını geçirin.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
console.log("\nReceiving messages from the queue...");

// Get messages from the queue
const receivedMessagesResponse = await queueClient.receiveMessages({ numberOfMessages : 5 });

console.log("Messages received, requestId:", receivedMessagesResponse.requestId);
```

### <a name="delete-messages-from-a-queue"></a>İletileri kuyruktan silme

İletileri alındıktan ve işlendikten sonra kuyruktan silin. Bu durumda, işleme yalnızca konsolda ileti yi görüntüler.

DeleteMessage yöntemini arayarak iletileri [silin.](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#deletemessage-string--string--queuedeletemessageoptions-) Açıkça silinmeyen iletiler, bunları işlemek için başka bir şans için kuyrukta tekrar görünür hale gelir.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
// 'Process' and delete messages from the queue
for (i = 0; i < receivedMessagesResponse.receivedMessageItems.length; i++) {
    receivedMessage = receivedMessagesResponse.receivedMessageItems[i];

    // 'Process' the message
    console.log("\tProcessing:", receivedMessage.messageText);

    // Delete the message
    const deleteMessageResponse = await queueClient.deleteMessage(
        receivedMessage.messageId,
        receivedMessage.popReceipt
    );
    console.log("\tMessage deleted, requestId:", deleteMessageResponse.requestId);
}
```

### <a name="delete-a-queue"></a>Bir kuyruk silme

Aşağıdaki kod, [sil](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#delete-queuedeleteoptions-) yöntemini kullanarak sırayı silerek oluşturulan kaynakları temizler.

Bu kodu `main` işlevin sonuna ekleyin ve dosyayı kaydedin:

```javascript
// Delete the queue
console.log("\nDeleting queue...");
const deleteQueueResponse = await queueClient.delete();
console.log("Queue deleted, requestId:", deleteQueueResponse.requestId);
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, Azure kuyruğuna üç ileti oluşturur ve ekler. Kod, kuyruktaki iletileri listeler, ardından sırayı silmeden önce alır ve siler.

Konsol pencerenizde, kuyrukları içeren dizine *gidin-quickstart-v12.js* dosyası, ardından `node` uygulamayı çalıştırmak için aşağıdaki komutu çalıştırın.

```console
node queues-quickstart-v12.js
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Queue storage v12 - JavaScript quickstart sample

Creating queue...
         quickstartc095d120-1d04-11ea-af30-090ee231305f
Queue created, requestId: 5c0bc94c-6003-011b-7c11-b13d06000000

Adding messages to the queue...
Messages added, requestId: a0390321-8003-001e-0311-b18f2c000000

Peek at the messages in the queue...
         First message
         Second message
         Third message

Updating the third message in the queue...
Message updated, requestId: cb172c9a-5003-001c-2911-b18dd6000000

Receiving messages from the queue...
Messages received, requestId: a039036f-8003-001e-4811-b18f2c000000
        Processing: First message
        Message deleted, requestId: 4a65b82b-d003-00a7-5411-b16c22000000
        Processing: Second message
        Message deleted, requestId: 4f0b2958-c003-0030-2a11-b10feb000000
        Processing: Third message has been updated
        Message deleted, requestId: 6c978fcb-5003-00b6-2711-b15b39000000

Deleting queue...
Queue deleted, requestId: 5c0bca05-6003-011b-1e11-b13d06000000

Done
```

Hata ayıklayıcınızdaki kodu gözden geçirin ve işlem boyunca [Azure portalınızı](https://portal.azure.com) kontrol edin. Kuyruktaki iletilerin oluşturulduğunu ve silinip silindiğinden doğrulamak için depolama hesabınızı kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, JavaScript kodunu kullanarak kuyruk oluşturmayı ve ona ileti eklemeyi öğrendiniz. Ardından iletileri gözetlemeyi, almayı ve silmeyi öğrendiniz. Son olarak, ileti kuyruğunun nasıl silinir öğrenilir.

Öğreticiler, örnekler, hızlı başlangıçlar ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [JavaScript belgeleri için Azure](https://docs.microsoft.com/azure/javascript/)

* Daha fazla bilgi edinmek [için JavaScript için Azure Depolama Kuyruğu istemci kitaplığına](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-queue)bakın.
* Daha fazla Azure Kuyruğu depolama örneği uygulaması görmek için [Azure Queue depolama istemcikitapv12 JavaScript örneklerine](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-queue/samples)devam edin.
