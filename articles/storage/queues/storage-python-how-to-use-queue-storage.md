---
title: Python-Azure depolama 'dan Azure kuyruk depolama v 2.1 kullanma
description: Sıraları oluşturmak ve silmek ve iletileri eklemek, almak ve silmek için Python 'dan Azure Kuyruk hizmeti v 2.1 kullanmayı öğrenin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/17/2019
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.custom: seo-javascript-october2019
ms.openlocfilehash: ca0831fd7554058d21e315b67d6965579af1d38b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80060911"
---
# <a name="how-to-use-azure-queue-storage-v21-from-python"></a>Python 'dan Azure kuyruk depolama v 2.1 kullanma

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

Bu makalede, Azure kuyruk depolama hizmetini kullanan yaygın senaryolar gösterilmektedir. Kapsanan senaryolar sıra iletilerini ekleme, göz atma, alma ve silme, kuyrukları oluşturma ve silme içerir.

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış

Bu makaledeki örnekler Python 'da yazılmıştır ve [Python için Microsoft Azure depolama SDK 'sını]kullanır. Kuyruklar hakkında daha fazla bilgi için [sonraki adımlar](#next-steps) bölümüne bakın.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Python için Azure depolama SDK 'sını indirme ve yükleme

[Python Için Azure depolama SDK 'sı](https://github.com/azure/azure-storage-python) , python sürüm 2,7, 3,3 veya üstünü gerektirir.
 
### <a name="install-via-pypi"></a>PyPi aracılığıyla yüklemesi

Python paket dizini (Pypı) aracılığıyla yüklemek için şunu yazın:

```bash
pip install azure-storage-queue==2.1.0
```

> [!NOTE]
> Python sürüm 0,36 veya önceki bir sürümü için Azure Storage SDK 'dan yükseltiyorsanız, en son paketi yüklemeden önce kullanarak `pip uninstall azure-storage` eski SDK 'yı kaldırın.

Alternatif yükleme yöntemleri için bkz. [Python Için Azure Storage SDK](https://github.com/Azure/azure-storage-python/).

## <a name="view-the-sample-application"></a>Örnek uygulamayı görüntüleme

Azure kuyrukları ile Python 'un nasıl kullanılacağını gösteren örnek bir uygulamayı görüntülemek ve çalıştırmak için bkz. [Azure Storage: Python 'Da Azure Kuyrukları Ile çalışmaya](https://github.com/Azure-Samples/storage-queue-python-getting-started)başlama. 

Örnek uygulamayı çalıştırmak için hem `azure-storage-queue` hem `azure-storage-common` de paketlerini yüklediğinizden emin olun.

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

[QueueService](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice) nesnesi kuyruklarla çalışmanıza olanak sağlar. Aşağıdaki kod bir `QueueService` nesnesi oluşturur. Azure Storage 'a programlı bir şekilde erişmek istediğiniz herhangi bir Python dosyasının en üstüne aşağıdakileri ekleyin:

```python
from azure.storage.queue import QueueService
```

Aşağıdaki kod, depolama hesabı `QueueService` adını ve hesap anahtarını kullanarak bir nesnesi oluşturur. *Myaccount* ve *MyKey* değerini hesap adınızla ve anahtarınızla değiştirin.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="insert-a-message-into-a-queue"></a>Kuyruğa bir ileti yerleştirme

Bir kuyruğa ileti eklemek için [put_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#put-message-queue-name--content--visibility-timeout-none--time-to-live-none--timeout-none-) yöntemini kullanarak yeni bir ileti oluşturun ve kuyruğa ekleyin.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

Azure kuyruk iletileri metin olarak depolanır. İkili verileri depolamak istiyorsanız, kuyruğa bir ileti koymadan önce, kuyruk hizmeti nesnesinde Base64 kodlama ve kod çözme işlevleri ayarlayın.

```python
# setup queue Base64 encoding and decoding functions
queue_service.encode_function = QueueMessageFormat.binary_base64encode
queue_service.decode_function = QueueMessageFormat.binary_base64decode
```

## <a name="peek-at-the-next-message"></a>Sonraki iletiye gözatın

[Peek_messages](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#peek-messages-queue-name--num-messages-none--timeout-none-) yöntemini çağırarak sıradan kaldırmadan bir kuyruğun önündeki iletiye göz atmayı sağlayabilirsiniz. Varsayılan olarak tek `peek_messages` bir iletiye göz atar.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="dequeue-messages"></a>İletileri sıradan çıkarma

Kodunuz bir iletiyi iki adımda kuyruktan kaldırır. [Get_messages](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#get-messages-queue-name--num-messages-none--visibility-timeout-none--timeout-none-)çağırdığınızda, bir sıradaki bir sonraki iletiyi varsayılan olarak alırsınız. Öğesinden `get_messages` döndürülen bir ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırma işleminin tamamlanabilmesi için, [delete_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#delete-message-queue-name--message-id--pop-receipt--timeout-none-)de çağırmanız gerekir. Bir iletiyi kaldırmanın bu iki adımlı işlemi, kodunuz, donanım veya yazılım arızasından kaynaklanan bir iletiyi işleyemediğinde, kodunuzun başka bir örneğinin aynı mesajı almasını ve yeniden denemesini sağlar. İleti işlendikten sonra `delete_message` kodunuz doğru şekilde çağırır.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneği, tek bir `get_messages` çağrıda 16 ileti almak için yöntemini kullanır. Sonra her iletiyi bir for döngüsü kullanarak işler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```python
messages = queue_service.get_messages(
    'taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

## <a name="change-the-contents-of-a-queued-message"></a>Kuyruğa alınan iletinin içeriğini değiştirme

Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod, bir iletiyi güncelleştirmek için [update_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#update-message-queue-name--message-id--pop-receipt--visibility-timeout--content-none--timeout-none-) yöntemini kullanır. Görünürlük zaman aşımı 0 olarak ayarlanır, yani ileti hemen görünür ve içerik güncellenir.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message(
        'taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. [Get_queue_metadata](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#get-queue-metadata-queue-name--timeout-none-) yöntemi, kuyruk hizmetinin kuyruğa ilişkin meta verileri döndürmesini ve `approximate_message_count`. Sonuç yalnızca, kuyruk hizmeti isteğinize yanıt verdikten sonra iletiler eklenebildiğinden veya kaldırılacağından yaklaşık olur.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir kuyruğu ve içerdiği tüm iletileri silmek için [delete_queue](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#delete-queue-queue-name--fail-not-exist-false--timeout-none-) yöntemi çağırın.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Sonraki adımlar

Sıra depolamanın temellerini öğrendiğinize göre, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Azure Kuyrukları Python API başvurusu](/python/api/azure-storage-queue)
* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Azure Storage Hizmetleri REST API’si](https://msdn.microsoft.com/library/azure/dd179355)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[Python için SDK Microsoft Azure Depolama]: https://github.com/Azure/azure-storage-python
