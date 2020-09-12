---
title: Azure kuyruk depolama Node.js-Azure depolama 'dan nasıl kullanılır
description: Kuyrukları oluşturmak ve silmek için Azure Kuyruk hizmeti kullanmayı öğrenin. Node.js kullanarak ileti eklemeyi, almayı ve silmeyi öğrenin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/31/2020
ms.service: storage
ms.subservice: queues
ms.topic: how-to
ms.reviewer: dineshm
ms.custom: seo-javascript-september2019, devx-track-javascript
ms.openlocfilehash: 18e184ed126aab8d03867db7b6b7d28c88644366
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89288823"
---
# <a name="how-to-use-azure-queue-storage-from-nodejs"></a>Node.js Azure kuyruk depolamayı kullanma

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

## <a name="overview"></a>Genel Bakış

Bu kılavuzda, Microsoft Azure Kuyruk hizmeti kullanarak genel senaryoları nasıl gerçekleştirebileceğiniz gösterilmektedir. Örnekler Node.js API kullanılarak yazılır. Kapsanan senaryolar sıra iletilerini ekleme, göz atma, alma ve silmeyi içerir. Kuyrukları oluşturmayı ve silmeyi de öğrenin.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Boş bir Node.js uygulaması oluşturmak için, bkz. [Azure App Service 'da Node.js Web uygulaması oluşturma][Create a Node.js web app in Azure App Service], Windows PowerShell kullanarak [Azure bulut hizmetinde bir Node.js uygulaması oluşturma ve dağıtma][Build and deploy a Node.js application to an Azure Cloud Service] , veya [Visual Studio Code][Visual Studio Code].

## <a name="configure-your-application-to-access-storage"></a>Uygulamanızı depolamaya erişecek şekilde yapılandırma

[JavaScript Için Azure Storage istemci kitaplığı][Azure Storage client library for JavaScript] , depolama Rest hizmetleriyle iletişim kuran bir dizi kullanışlı kitaplık içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paketi edinmek için düğüm Paket Yöneticisi 'Ni (NPM) kullanın

1. PowerShell (Windows), Terminal (Mac) veya Bash (Unix) gibi bir komut satırı arabirimi kullanın, örnek uygulamanızı oluşturduğunuz klasöre gidin.

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

1. Komut penceresine **NPM Install \@ Azure/Storage-Queue** yazın.

1. Bir **düğüm \_ modülleri** klasörünün oluşturulduğunu doğrulayın. Bu klasörün içinde, depolamaya erişmeniz gereken istemci kitaplığını içeren ** \@ Azure/Storage-Queue** paketini bulacaksınız.

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

1. Komut penceresine **npm install azure-storage** yazın.

1. Bir **düğüm \_ modülleri** klasörünün oluşturulduğunu doğrulayın. Bu klasörün içinde, depolamaya erişmeniz gereken kitaplıkları içeren **Azure depolama** paketini bulacaksınız.

---

### <a name="import-the-package"></a>Paketi içeri aktarma

Kod düzenleyicinizi kullanarak, kuyrukları kullanmayı düşündüğünüz JavaScript dosyasına aşağıdakini ekleyin.

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_ImportStatements":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

```javascript
var azure = require('azure-storage');
```

---

## <a name="how-to-create-a-queue"></a>Kuyruk oluşturma

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

Aşağıdaki kod, adlı bir ortam değişkeninin değerini alır `AZURE_STORAGE_CONNECTION_STRING` ve bunu bir [QueueServiceClient](/javascript/api/@azure/storage-queue/queueserviceclient) nesnesi oluşturmak için kullanır. **QueueServiceClient** nesnesi daha sonra bir [queueclient](/javascript/api/@azure/storage-queue/queueclient) nesnesi oluşturmak için kullanılır. **Queueclient** nesnesi belirli bir kuyruk ile çalışmanıza olanak sağlar.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_CreateQueue":::

Sıra zaten varsa, bir özel durum oluşturulur.

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Azure modülü, ortam değişkenlerini `AZURE_STORAGE_ACCOUNT` ve `AZURE_STORAGE_ACCESS_KEY` ya da `AZURE_STORAGE_CONNECTION_STRING` Azure depolama hesabınıza bağlanmak için gereken bilgileri okuyacaktır. Bu ortam değişkenleri ayarlanmamışsa, **Createqueueservice**çağrılırken hesap bilgilerini belirtmeniz gerekir.

Aşağıdaki kod, kuyruklarla çalışmanıza olanak sağlayan bir **QueueService** nesnesi oluşturur.

```javascript
var queueSvc = azure.createQueueService();
```

Belirtilen ada sahip yeni bir sıra oluşturmak için **Createqueueifnotexists** metodunu çağırın veya zaten varsa kuyruğu döndürün.

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

Sıra oluşturulduysa, `result.created` doğru olur. Sıra varsa, `result.created` false olur.

---

## <a name="how-to-insert-a-message-into-a-queue"></a>Bir kuyruğa ileti ekleme

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

Bir kuyruğa ileti eklemek için, [SendMessage](/javascript/api/@azure/storage-queue/queueclient#sendmessage-string--queuesendmessageoptions-) metodunu çağırın.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_AddMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Bir kuyruğa ileti eklemek için, **CreateMessage** yöntemini çağırarak yeni bir ileti oluşturun ve kuyruğa ekleyin.

```javascript
queueSvc.createMessage('myqueue', "Hello world!", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

---

## <a name="how-to-peek-at-the-next-message"></a>Sonraki iletiye nasıl göz atın

Kuyruktaki iletilere, **peekMessages** yöntemini çağırarak kuyruktan kaldırmadan göz atmayı sağlayabilirsiniz.

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

Varsayılan olarak, [peekMessages](/javascript/api/@azure/storage-queue/queueclient#peekmessages-queuepeekmessagesoptions-) , tek bir iletiye bakar. Aşağıdaki örnekte kuyruktaki ilk beş ileti göz atar. Beşten az ileti görünür durumdaysa, yalnızca görünür iletiler döndürülür.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_PeekMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Varsayılan olarak, **peekMessages** , tek bir iletiye bakar.

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

`result`İletiyi içerir.

---

Kuyrukta hiçbir ileti olmadığında **peekMessages** çağırma bir hata döndürmez. Ancak, hiçbir ileti döndürülmez.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Sıraya alınan iletinin içeriğini değiştirme

Aşağıdaki örnek bir iletinin metnini güncelleştirir.

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

[Updatemessage](/javascript/api/@azure/storage-queue/queueclient#updatemessage-string--string--string--number--queueupdatemessageoptions-)' i çağırarak kuyruktaki bir iletinin içeriğini değiştirme. 

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_UpdateMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

**Updatemessage**' i çağırarak kuyruktaki bir iletinin içeriğini değiştirme. 

```javascript
queueSvc.getMessages('myqueue', function(error, getResults, getResponse){
  if(!error){
    // Got the message
    var message = getResults[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, updateResults, updateResponse){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

---

## <a name="how-to-dequeue-a-message"></a>İletiyi sıradan çıkarma

Bir iletinin kuyruğa çıkarılması iki aşamalı bir işlemdir:

1. İletiyi alın.

1. İletiyi silin.

Aşağıdaki örnekte bir ileti alınır ve silinir.

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

Bir ileti almak için, [Receivemessages](/javascript/api/@azure/storage-queue/queueclient#receivemessages-queuereceivemessageoptions-) yöntemini çağırın. Bu çağrı, iletileri kuyrukta görünmez hale getirir, dolayısıyla başka hiçbir istemci bunları işleyemez. Uygulamanız bir iletiyi işledikten sonra sıradan silmek için [deleteMessage](/javascript/api/@azure/storage-queue/queueclient#deletemessage-string--string--queuedeletemessageoptions-) ' ı çağırın.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DequeueMessage":::

Varsayılan olarak, bir ileti yalnızca 30 saniye gizli olur. 30 saniye sonra diğer istemcilere görünür olur. **Receivemessages**'i çağırdığınızda [Options. visibilitytimeout seçeneğini](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#visibilitytimeout) ayarlayarak farklı bir değer belirtebilirsiniz.

Kuyrukta hiçbir ileti olmadığında **receivemessages** çağırma bir hata döndürmez. Ancak, hiçbir ileti döndürülmeyecektir.

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Bir ileti almak için **GetMessages** yöntemini çağırın. Bu çağrı, iletileri kuyrukta görünmez hale getirir, dolayısıyla başka hiçbir istemci bunları işleyemez. Uygulamanız bir iletiyi işledikten sonra sıradan silmek için **deleteMessage** ' ı çağırın.

```javascript
queueSvc.getMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
    var message = results[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

Varsayılan olarak, bir ileti yalnızca 30 saniye gizli olur. 30 saniye sonra diğer istemcilere görünür olur. `options.visibilityTimeout` **GetMessages**ile kullanarak farklı bir değer belirtebilirsiniz.

Kuyrukta hiçbir ileti olmadığında **GetMessages** kullanımı bir hata döndürmez. Ancak, hiçbir ileti döndürülmeyecektir.

---

## <a name="additional-options-for-dequeuing-messages"></a>Dequeuing iletileri için ek seçenekler

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

Bir kuyruktan ileti alımını özelleştirmek için iki yol vardır:

* [Options. numberofmessages](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#numberofmessages) -bir toplu ileti alma (32 'e kadar)
* [Options. visibilityTimeout](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#visibilitytimeout) -daha uzun veya daha kısa görünürlük zaman aşımı ayarlayın.

Aşağıdaki örnek, tek bir çağrıda beş ileti almak için **receivemessages** yöntemini kullanır. Ardından, her iletiyi bir döngü kullanarak işler `for` . Ayrıca, bu yöntemin döndürdüğü tüm iletiler için geçersiz kılma zaman aşımını beş dakikaya ayarlar.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DequeueMessages":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Bir kuyruktan ileti alımını özelleştirmek için iki yol vardır:

* `options.numOfMessages` -Bir ileti toplu işi alın (32 'e kadar.)
* `options.visibilityTimeout` -Daha uzun veya daha kısa görünürlük zaman aşımı ayarlayın.

Aşağıdaki örnek, bir çağrıda 15 ileti almak için **GetMessages** yöntemini kullanır. Ardından, her iletiyi bir döngü kullanarak işler `for` . Ayrıca, bu yöntemin döndürdüğü tüm iletiler için geçersiz kılma zaman aşımını beş dakikaya ayarlar.

```javascript
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, results, getResponse){
  if(!error){
    // Messages retrieved
    for(var index in results){
      // text is available in result[index].messageText
      var message = results[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, deleteResponse){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

---

## <a name="how-to-get-the-queue-length"></a>Sıra uzunluğunu alma

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

[GetProperties](/javascript/api/@azure/storage-queue/queueclient#getproperties-queuegetpropertiesoptions-) metodu, sırada bekleyen ileti sayısı dahil olmak üzere kuyrukla ilgili meta verileri döndürür.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_QueueLength":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

**Getqueuemetadata** yöntemi, sırada bekleyen yaklaşık ileti sayısı dahil olmak üzere kuyruk hakkında meta veriler döndürür.

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

---

## <a name="how-to-list-queues"></a>Kuyrukları listeleme

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

Sıraların listesini almak için [QueueServiceClient. listqueues]()çağırın. Belirli bir önekle filtrelenmiş bir listeyi almak için, **Listqueues**çağrın çağrdaki [Options. Prefix](/javascript/api/@azure/storage-queue/servicelistqueuesoptions#prefix) ' i ayarlayın.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_ListQueues":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Kuyrukların listesini almak için **Listqueueskesimli**öğesini kullanın. Belirli bir ön eke göre filtrelenmiş bir listeyi almak için **listQueuesSegmentedWithPrefix**kullanın.

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

Tüm kuyruklar döndürülmezse, `result.continuationToken` daha fazla sonuç almak Için **Listqueuessegmentinin** ilk parametresi olarak veya **listQueuesSegmentedWithPrefix** ikinci parametresi olarak geçirin.

---

## <a name="how-to-delete-a-queue"></a>Kuyruğu silme

# <a name="javascript-v12"></a>[JavaScript V12](#tab/javascript)

Bir kuyruğu ve içerdiği tüm iletileri silmek için **Queueclient** nesnesinde [DeleteQueue](/javascript/api/@azure/storage-queue/queueclient#delete-queuedeleteoptions-) yöntemini çağırın.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DeleteQueue":::

Bir kuyruktan silmeden tüm iletileri silmek için [ClearMessages](/javascript/api/@azure/storage-queue/queueclient#clearmessages-queueclearmessagesoptions-)çağrısı yapın.

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Bir kuyruğu ve içerdiği tüm iletileri silmek için, kuyruk nesnesi üzerinde **DeleteQueue** yöntemini çağırın.

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Bir kuyruktan silmeden tüm iletileri silmek için **ClearMessages**çağrısı yapın.

---

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="next-steps"></a>Sonraki adımlar

Sıra depolamanın temellerini öğrendiğinize göre, daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* Yenilikleri öğrenmek için [Azure depolama ekibi blogu][Azure Storage Team Blog] ' nu ziyaret edin
* GitHub 'da [JavaScript deposu Için Azure depolama istemci kitaplığı][Azure Storage client library for JavaScript] ' nı ziyaret edin

[Azure Storage client library for JavaScript]: https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage#azure-storage-client-library-for-javascript
[Azure Storage Team Blog]: https://techcommunity.microsoft.com/t5/azure-storage/bg-p/AzureStorageBlog
[Build and deploy a Node.js application to an Azure Cloud Service]: ../../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Create a Node.js web app in Azure App Service]: ../../app-service/quickstart-nodejs.md
[Visual Studio Code]: https://code.visualstudio.com/docs/nodejs/nodejs-tutorial
