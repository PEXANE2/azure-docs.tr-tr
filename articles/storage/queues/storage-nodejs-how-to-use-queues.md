---
title: Düğümden Azure Kuyruk depolama alanını kullanma - Azure Depolama
description: Kuyrukoluşturmak ve silmek ve ileti eklemek, almak ve silmek için Azure Kuyruk hizmetini nasıl kullanacağınızı öğrenin. Düğüm.js ile yazılmış örnekler.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/08/2016
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.custom: seo-javascript-september2019
ms.openlocfilehash: 7abcad03678131668700f5d2c64b9c971081cb89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060927"
---
# <a name="use-azure-queue-service-to-create-and-delete-queues-from-nodejs"></a>Node.js'den kuyruklar oluşturmak ve silmek için Azure Kuyruk Hizmeti'ni kullanın
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, Microsoft Azure Kuyruk hizmetini kullanarak sık karşılaşılan senaryoları nasıl gerçekleştireceklerini gösterir. Örnekler Düğüm.js API kullanılarak yazılır. Kapsanan senaryolar arasında kuyruk iletileri **ekleme,** **gözetleme,** **alma**ve **silme** nin yanı sıra **kuyruk oluşturma ve silme**yer alır.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Düğüm Oluşturma.js Uygulaması
Boş bir Düğüm.js uygulaması oluşturun. Bir Düğüm.js uygulaması oluşturan talimatlar için, Azure [Uygulama Hizmeti'nde Bir Düğüm.js web uygulaması oluşturma](../../app-service/app-service-web-get-started-nodejs.md), Windows PowerShell veya Visual Studio [Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)kullanarak bir Azure [Bulut Hizmetine Düğüm uygulaması oluşturma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) ve dağıtma bölümüne bakın.

## <a name="configure-your-application-to-access-storage"></a>Uygulamanızı Access Depolamasına Yapılandırın
Azure depolama alanını kullanmak için, depolama REST hizmetleriyle iletişim kuran bir dizi kolaylık kitaplığı içeren Node.js için Azure Depolama SDK'sına ihtiyacınız vardır.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paketi almak için Düğüm Paket Yöneticisi'ni (NPM) kullanın
1. **PowerShell** (Windows,), **Terminal** (Mac,) veya **Bash** (Unix) gibi bir komut satırı arabirimi kullanın, örnek uygulamanızı oluşturduğunuz klasöre gidin.
2. Komut penceresine **npm install azure-storage** yazın. Komutun çıktısı aşağıdaki örneğe benzer.
 
    ```bash
    azure-storage@0.5.0 node_modules\azure-storage
    +-- extend@1.2.1
    +-- xmlbuilder@0.4.3
    +-- mime@1.2.11
    +-- node-uuid@1.4.3
    +-- validator@3.22.2
    +-- underscore@1.4.4
    +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
    +-- xml2js@0.2.7 (sax@0.5.2)
    +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    ```

3. Düğüm **\_modülleri** klasörü oluşturulduğunu doğrulamak için **lS** komutunu el ile çalıştırabilirsiniz. Bu klasörün içinde, depolama alanına erişmek için ihtiyaç duyduğunuz kitaplıkları içeren **azure-storage** paketini bulacaksınız.

### <a name="import-the-package"></a>Paketi içeri aktarma
Not Defteri'ni veya başka bir metin düzenleyicisini kullanarak, depolama alanını kullanmak istediğiniz uygulamanın **server.js** dosyasının üst bölümüne aşağıdakileri ekleyin:

```javascript
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Azure Depolama Bağlantısı Kurma
Azure modülü,\_Azure depolama hesabınıza\_bağlanmak\_için\_gereken bilgiler için azure depolama hesabı ve AZURE DEPOLAMA\_ACCESS KEY veya\_AZURE DEPOLAMA\_BAĞLANTI\_STRING ortam değişkenlerini okur. Bu ortam değişkenleri ayarlanmazsa, **createQueueService'i**ararken hesap bilgilerini belirtmeniz gerekir.

## <a name="how-to-create-a-queue"></a>Nasıl Yapılsın: Sıra Oluşturma
Aşağıdaki kod, kuyruklarla çalışmanızı sağlayan bir **QueueService** nesnesi oluşturur.

```javascript
var queueSvc = azure.createQueueService();
```

Zaten varsa belirtilen sırayı döndüren veya zaten yoksa belirtilen ada sahip yeni bir sıra oluşturan **createQueueIfNotExists** yöntemini kullanın.

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

Sıra oluşturulursa, `result.created` doğrudur. Sıra varsa, `result.created` yanlıştır.

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri **QueueService**kullanılarak gerçekleştirilen işlemlere uygulanabilir. Filtreleme işlemleri günlüğe kaydetmeyi, otomatik olarak yeniden denemeyi vb. içerebilir. Filtreler, imzaile birlikte bir yöntem uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçenekleri üzerinde ön işleme yaptıktan sonra, yöntem aşağıdaki imza ile bir geri arama geçen "sonraki" aramak gerekir:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri aramada ve returnObject'i (isteğin sunucuya verdiği yanıt) işledikten sonra, diğer filtreleri işlemeye devam etmek için geri aramanın bir sonraki çağrıda bulunması veya yalnızca hizmetin sona ermesi için son Çağrı'yı çağırması gerekir Çağırma.

Yeniden deneme mantığı uygulayan iki filtre (**ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**), Node.js için Azure SDK’sına dahil edilir. Aşağıdaki, **Üstel RetryPolicyFilter**kullanan bir **QueueService** nesnesi oluşturur:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl Alınır: Sıraya İleti Ekleme
Bir iletiyi kuyruğa eklemek için, yeni bir ileti oluşturmak ve kuyruğa eklemek için **CreateMessage** yöntemini kullanın.

```javascript
queueSvc.createMessage('myqueue', "Hello world!", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl Yapılsın: Sonraki İletiye Göz At
**PeekMessages** yöntemini çağırarak sıranın önündeki iletiye kuyruktan çıkarmadan göz atabilirsiniz. Varsayılan olarak, **peekMessages** tek bir iletiyi gözetler.

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

İletiyi `result` içerir.

> [!NOTE]
> Sırada ileti yoksa **peekMessage'ların** kullanılması hata döndürmez, ancak hiçbir ileti döndürülmez.
> 
> 

## <a name="how-to-dequeue-the-next-message"></a>Nasıl YapılSın: Sonraki İletiyi Sırayla Çözme
İletiyi işleme iki aşamalı bir işlemdir:

1. İletiyi sıradan ayırın.
2. İletiyi silin.

İletinin kuyruğundan çıkarmak için **getMessage'ları**kullanın. Bu, iletileri kuyrukta görünmez kılar, böylece başka istemciler bunları işleyebilir. Uygulamanız bir iletiyi işledikten sonra, sıradan silmek için **deleteMessage'ı** arayın. Aşağıdaki örnek bir ileti alır, sonra siler:

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

> [!NOTE]
> Varsayılan olarak, bir ileti yalnızca 30 saniye boyunca gizlenir ve ardından diğer istemciler tarafından görülebilir. `options.visibilityTimeout` **GetMessages**ile kullanarak farklı bir değer belirtebilirsiniz.
> 
> [!NOTE]
> Sırada ileti yokken **getMessage'ların** kullanılması hata döndürmez, ancak hiçbir ileti döndürülmez.
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl YapılSın: Sıralı İletinin İçeriğini Değiştirme
**UpdateMessage'ı**kullanarak kuyruktaki bir iletinin içeriğini yerinde değiştirebilirsiniz. Aşağıdaki örnek, bir iletinin metnini güncelleştirir:

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

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Nasıl Yapılsın: İletileri Sıraya Almak için Ek Seçenekler
İleti alma sıranından özelleştirebileceğiniz iki yolu vardır:

* `options.numOfMessages`- Bir yığın ileti alın (en fazla 32.)
* `options.visibilityTimeout`- Daha uzun veya daha kısa bir görünmezlik zaman aşımı ayarlayın.

Aşağıdaki örnek, tek bir aramada 15 ileti almak için **getMessages** yöntemini kullanır. Daha sonra her iletiyi for döngüsü kullanarak işler. Ayrıca, bu yöntemle döndürülen tüm iletiler için görünmezlik zaman anına beş dakika ayarlar.

```javascript
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, results, getResponse){
  if(!error){
    // Messages retrieved
    for(var index in result){
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

## <a name="how-to-get-the-queue-length"></a>Nasıl Gidilir: Sıra Uzunluğunu Alın
**getQueueMeta verileri,** kuyrukta bekleyen yaklaşık ileti sayısı da dahil olmak üzere kuyrukla ilgili meta verileri döndürür.

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Nasıl YapılSın: Kuyrukları Listele
Kuyrukların listesini almak için **listQueuesSegmented'i**kullanın. Belirli bir önek tarafından filtre uygulanmış bir liste yi almak için **listQueuesSegmentedWithPrefix'i**kullanın.

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

Tüm kuyruklar döndürülemiyorsa, `result.continuationToken` daha fazla sonuç almak için **listQueuesSegmented'in** ilk parametresi veya **listQueuesSegmentedWithPrefix'in** ikinci parametresi olarak kullanılabilir.

## <a name="how-to-delete-a-queue"></a>Nasıl Alınır: Sırayı Silme
Bir kuyruğ ve içindeki tüm iletileri silmek için, sıra nesnesindeki **deleteQueue** yöntemini arayın.

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Tüm iletileri silmeden bir kuyruktan temizlemek için **clearMessages'ı**kullanın.

## <a name="how-to-work-with-shared-access-signatures"></a>Nasıl yapılır: Paylaşılan Erişim İmzalarıyla Çalışma
Paylaşılan Erişim İmzaları (SAS), depolama hesabı adınızı veya anahtarlarınızı sağlamadan kuyruklara parçalı erişim sağlamanın güvenli bir yoludur. SAS genellikle, bir mobil uygulamanın mesaj göndermesine izin vermek gibi kuyruklarınıza sınırlı erişim sağlamak için kullanılır.

Bulut tabanlı bir hizmet gibi güvenilir bir uygulama, **QueueService'in** **oluşturduğu SharedAccessSignature'ı** kullanarak bir SAS oluşturur ve bunu güvenilmeyen veya yarı güvenilen bir uygulamaya sağlar. Örneğin, bir mobil uygulama. SAS’ın geçerli olduğu başlangıç ve bitiş tarihlerini ve SAS sahibine verilen erişim düzeyini açıklayan bir ilke kullanılarak SAS oluşturulur.

Aşağıdaki örnek, SAS sahibinin kuyruğa ileti eklemesine olanak tanıyan ve oluşturulduğu andan 100 dakika sonra sona eren yeni bir paylaşılan erişim ilkesi oluşturur.

```javascript
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};

var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
var host = queueSvc.host;
```

SAS sahibi kuyruğa erişmeye çalıştığında gerekli olduğu gibi ana bilgisayar bilgilerinin de sağlanması gerektiğini unutmayın.

İstemci uygulaması daha sonra sıraya karşı işlemleri gerçekleştirmek için **QueueServiceWithSAS** ile SAS kullanır. Aşağıdaki örnek kuyruğa bağlanır ve bir ileti oluşturur.

```javascript
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

SAS ekle erişimi yle oluşturulduğundan, iletileri okumaya, güncelleştirmeye veya silme girişiminde bulunulduysa, bir hata döndürülür.

### <a name="access-control-lists"></a>Erişim denetimi listeleri
Ayrıca SAS için erişim ilkesini ayarlamak istediğinizde de bir Erişim Denetim Listesi (ACL) kullanabilirsiniz. Bu, birden çok istemcinin kuyruğa erişmesine izin vermek, ancak her istemci için farklı erişim ilkeleri sağlamak istiyorsanız yararlıdır.

Her politikayla ilişkilendirilmiş bir kimlik ile, bir erişim ilkeleri dizisi kullanılarak ACL uygulanır. Aşağıdaki örnekte iki ilke tanımlanır; biri 'user1' ve diğeri 'user2' için:

```javascript
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Aşağıdaki örnek **myqueue**için geçerli ACL alır, sonra **setQueueAcl**kullanarak yeni ilkeler ekler. Bu yaklaşım aşağıdakilere olanak sağlar:

```javascript
var extend = require('extend');
queueSvc.getQueueAcl('myqueue', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

ACL ayarlandıktan sonra, bir ilke için kimlik tabanlı bir SAS oluşturabilirsiniz. Aşağıdaki örnek, 'user2' için yeni bir SAS oluşturur:

```javascript
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Sonraki Adımlar
Artık sıra depolamanın temellerini öğrendiğiniz için, daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* [Azure Storage ekip blogunu][Azure Storage Team Blog] ziyaret edin.
* GitHub'daki Düğüm deposu [için Azure Depolama SDK'sını][Azure Storage SDK for Node] ziyaret edin.



[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[using the REST API]: https://msdn.microsoft.com/library/azure/hh264518.aspx

[Azure Portal]: https://portal.azure.com

[Azure App Service’te bir Node.js web uygulaması oluşturma](../../app-service/app-service-web-get-started-nodejs.md)

[Bir Node.js uygulaması derleme ve Azure Cloud Service’e dağıtma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/
