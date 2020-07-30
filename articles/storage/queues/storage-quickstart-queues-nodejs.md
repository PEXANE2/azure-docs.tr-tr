---
title: 'Hızlı başlangıç: Azure kuyruk depolama kitaplığı V12-JavaScript'
description: Kuyruk oluşturmak ve kuyruğa ileti eklemek için Azure kuyruğu JavaScript V12 kitaplığı 'nı nasıl kullanacağınızı öğrenin. Ardından, sıradaki iletileri okumayı ve silmeyi öğreneceksiniz. Ayrıca, bir kuyruğu silmeyi de öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/13/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.custom: devx-track-javascript
ms.openlocfilehash: f2d740184c160faa241e3c0267ec39e1309725a8
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87431313"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-javascript"></a>Hızlı başlangıç: JavaScript için Azure kuyruk depolama istemci kitaplığı V12

JavaScript için Azure kuyruk depolama istemci kitaplığı sürüm 12 ile çalışmaya başlayın. Azure kuyruk depolaması, daha sonra almak ve işlemek üzere çok sayıda ileti depolamaya yönelik bir hizmettir. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere bu adımları izleyin.

JavaScript için Azure kuyruk depolama istemci kitaplığı V12 ' nı kullanarak şunları yapın:

* Bir kuyruk oluşturma
* Bir kuyruğa ileti ekleme
* Kuyruktaki iletilere göz atın
* Kuyruktaki bir iletiyi güncelleştirme
* Kuyruktan ileti alma
* Kuyruktaki iletileri silme
* Bir kuyruk silme

Ek kaynaklar:

* [API başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/storage-queue/)
* [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-queue)
* [Paket (düğüm Paket Yöneticisi)](https://www.npmjs.com/package/@azure/storage-queue)
* [Örnekler](https://docs.microsoft.com/azure/storage/common/storage-samples-javascript?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı- [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için geçerli [Node.js](https://nodejs.org/en/download/) .

## <a name="setting-up"></a>Ayarlanıyor

Bu bölümde, bir projeyi, JavaScript için Azure kuyruk depolama istemci kitaplığı V12 ile çalışacak şekilde hazırlama adımları gösterilmektedir.

### <a name="create-the-project"></a>Proje oluşturma

Kuyruklar adlı bir Node.js uygulaması oluşturun *-hızlı başlangıç-V12*.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), proje için yeni bir dizin oluşturun.

    ```console
    mkdir queues-quickstart-v12
    ```

1. Yeni oluşturulan *kuyruklara geç hızlı başlangıç-V12* dizini.

    ```console
    cd queues-quickstart-v12
    ```

1. *Üzerindepackage.js*adlı yeni bir metin dosyası oluşturun. Bu dosya Node.js projesi tanımlar. Bu dosyayı *Kuyruklar-QuickStart-V12* dizinine kaydedin. Dosyanın içeriği aşağıdadır:

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

    İsterseniz alanı için kendi adınızı koyabilirsiniz `author` .

### <a name="install-the-package"></a>Paketi yükler

*Kuyruklar-QuickStart-V12* dizininde olmaya devam ederken komutunu kullanarak JavaScript Için Azure kuyruk depolama istemci Kitaplığı ' nı yükler `npm install` .

```console
npm install
```

 Bu komut, dosyadaki *package.js* okur ve JavaScript paketi Için Azure kuyruk depolama istemci kitaplığı V12 ve bağımlı olduğu tüm kitaplıkları kurar.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Kod Düzenleyicinizde başka bir yeni metin dosyası açın
1. `require`Azure ve Node.js modüllerini yüklemek için çağrılar ekleme
1. Çok temel özel durum işleme dahil olmak üzere programın yapısını oluşturma

    Kod şu şekildedir:

    ```javascript
    const { QueueClient } = require("@azure/storage-queue");
    const uuidv1 = require("uuid/v1");

    async function main() {
        console.log("Azure Queue storage v12 - JavaScript quickstart sample");
        // Quick start code goes here
    }

    main().then(() => console.log("\nDone")).catch((ex) => console.log(ex.message));

    ```

1. Yeni dosyayı *Kuyruklar-QuickStart-V12* dizinine *queues-quickstart-v12.js* olarak kaydedin.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Kuyruk iletisi boyutu 64 KB 'ye kadar olabilir. Bir kuyruk, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle zaman uyumsuz olarak işlenecek iş biriktirme listesi oluşturmak için kullanılır. Kuyruk depolama, üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabındaki bir kuyruk
* Kuyruktaki iletiler

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Kuyruk depolama mimarisi diyagramı](./media/storage-queues-introduction/queue1.png)

Şu kaynaklarla etkileşim kurmak için aşağıdaki JavaScript sınıflarını kullanın:

* [QueueServiceClient](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueserviceclient):, `QueueServiceClient` Depolama hesabınızdaki tüm kuyrukları yönetmenizi sağlar.
* [Queueclient](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient): `QueueClient` sınıfı, tek bir kuyruğu ve iletilerini yönetmenizi ve düzenlemenizi sağlar.
* [Queuemessage](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queuemessage): `QueueMessage` sınıf, bir kuyrukta [receivemessages](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient?view=azure-node-latest#receivemessages-queuereceivemessageoptions-) çağrılırken döndürülen ayrı nesneleri temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, JavaScript için Azure kuyruk depolama istemci kitaplığı ile aşağıdaki eylemleri nasıl yapılacağını gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kuyruk oluşturma](#create-a-queue)
* [Bir kuyruğa ileti ekleme](#add-messages-to-a-queue)
* [Kuyruktaki iletilere göz atın](#peek-at-messages-in-a-queue)
* [Kuyruktaki bir iletiyi güncelleştirme](#update-a-message-in-a-queue)
* [Kuyruktan ileti alma](#receive-messages-from-a-queue)
* [Kuyruktaki iletileri silme](#delete-messages-from-a-queue)
* [Bir kuyruk silme](#delete-a-queue)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, depolama [bağlantı dizesini yapılandırma](#configure-your-storage-connection-string) bölümünde oluşturulan ortam değişkeninden depolama hesabının bağlantı dizesini alır.

Bu kodu işlevin içine ekleyin `main` :

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

Yeni sıra için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için kuyruk adına bir UUID değeri ekler.

> [!IMPORTANT]
> Kuyruk adları yalnızca küçük harf, sayı ve kısa çizgi içerebilir ve bir harf veya sayı ile başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır. Ad ayrıca 3 ila 63 karakter uzunluğunda olmalıdır. Adlandırma sıraları hakkında daha fazla bilgi için bkz. [adlandırma sıraları ve meta verileri](https://docs.microsoft.com/rest/api/storageservices/naming-queues-and-metadata).

[Queueclient](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızda kuyruğu oluşturmak için [Create](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#create-queuecreateoptions-) yöntemini çağırın.

Bu kodu işlevin sonuna ekleyin `main` :

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

### <a name="add-messages-to-a-queue"></a>Bir kuyruğa ileti ekleme

Aşağıdaki kod parçacığı, [SendMessage](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#sendmessage-string--queuesendmessageoptions-) metodunu çağırarak kuyruğa ileti ekler. Ayrıca, üçüncü çağrıdan döndürülen [Queuemessage](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queuemessage) öğesini de kaydeder `sendMessage` . Döndürülen `sendMessageResponse` ileti içeriğini programda daha sonra güncelleştirmek için kullanılır.

Bu kodu işlevin sonuna ekleyin `main` :

```javascript
console.log("\nAdding messages to the queue...");

// Send several messages to the queue
await queueClient.sendMessage("First message");
await queueClient.sendMessage("Second message");
const sendMessageResponse = await queueClient.sendMessage("Third message");

console.log("Messages added, requestId:", sendMessageResponse.requestId);
```

### <a name="peek-at-messages-in-a-queue"></a>Kuyruktaki iletilere göz atın

[PeekMessages](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#peekmessages-queuepeekmessagesoptions-) yöntemini çağırarak kuyruktaki iletilere göz atın. `peekMessages`Yöntemi, sıranın önüne bir veya daha fazla ileti alır ancak iletinin görünürlüğünü değiştirmez.

Bu kodu işlevin sonuna ekleyin `main` :

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

[Updatemessage](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#updatemessage-string--string--string--undefined---number--queueupdatemessageoptions-) yöntemini çağırarak bir iletinin içeriğini güncelleştirin. `updateMessage`Yöntemi, bir iletinin görünürlük zaman aşımını ve içeriğini değiştirebilir. İleti içeriği, boyutu 64 KB 'a kadar olan bir UTF-8 kodlu dize olmalıdır. Yeni içerikle birlikte, `messageId` `popReceipt` kodda daha önce kaydedilen yanıtı ve yanıtı geçirin. `sendMessageResponse`Özellikler güncelleştirilecek iletiyi belirler.

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

[Receivemessages](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#receivemessages-queuereceivemessageoptions-) yöntemini çağırarak önceden eklenmiş iletileri indirin.  `numberOfMessages`Alanında bu çağrı için alacak en fazla ileti sayısını geçirin.

Bu kodu işlevin sonuna ekleyin `main` :

```javascript
console.log("\nReceiving messages from the queue...");

// Get messages from the queue
const receivedMessagesResponse = await queueClient.receiveMessages({ numberOfMessages : 5 });

console.log("Messages received, requestId:", receivedMessagesResponse.requestId);
```

### <a name="delete-messages-from-a-queue"></a>Kuyruktaki iletileri silme

İletileri alındıktan ve işlendikten sonra kuyruktan silin. Bu durumda, işleme yalnızca konsolda iletiyi görüntülüyor.

[DeleteMessage](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#deletemessage-string--string--queuedeletemessageoptions-) yöntemini çağırarak iletileri silin. Açıkça silinmeyen tüm iletiler, daha sonra bu işlemleri işlemek için bir süre sonra sırada görünür hale gelir.

Bu kodu işlevin sonuna ekleyin `main` :

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

Aşağıdaki kod, [silme](https://docs.microsoft.com/javascript/api/@azure/storage-queue/queueclient#delete-queuedeleteoptions-) yöntemi kullanılarak sırayı silerek uygulamanın oluşturduğu kaynakları temizler.

Bu kodu işlevin sonuna ekleyin `main` ve dosyayı kaydedin:

```javascript
// Delete the queue
console.log("\nDeleting queue...");
const deleteQueueResponse = await queueClient.delete();
console.log("Queue deleted, requestId:", deleteQueueResponse.requestId);
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, bir Azure kuyruğuna üç ileti oluşturur ve ekler. Kod kuyruktaki iletileri listeler, ardından kuyruğu silmeden önce bunları alır ve siler.

Konsol pencerenizde *queues-quickstart-v12.js* dosyasını içeren dizine gidin, ardından `node` uygulamayı çalıştırmak için aşağıdaki komutu yürütün.

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

Hata Ayıklayıcıdaki kodda adım adım ilerleyin ve işlem boyunca [Azure Portal](https://portal.azure.com) denetleyin. Kuyruktaki iletilerin oluşturulup silindiğini doğrulamak için depolama hesabınızı kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir sıranın nasıl oluşturulacağını ve JavaScript kodunu kullanarak iletiye nasıl ileti ekleneceğini öğrendiniz. Ardından iletileri göz atmayı, almayı ve silmeyi öğrendiniz. Son olarak, bir ileti sırasının nasıl silineceğini öğrendiniz.

Öğreticiler, örnekler, hızlı ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [JavaScript için Azure belgeleri](https://docs.microsoft.com/azure/developer/javascript/)

* Daha fazla bilgi edinmek için bkz. [JavaScript Için Azure depolama kuyruğu istemci kitaplığı](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-queue).
* Daha fazla Azure kuyruk depolama örneği uygulaması görmek için [Azure kuyruk depolama istemci kitaplığı V12 JavaScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-queue/samples)' ne geçin.
