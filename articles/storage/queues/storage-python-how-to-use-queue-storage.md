---
title: Python'dan Azure Queue depolama v2.1 nasıl kullanılır - Azure Depolama
description: Kuyrukoluşturmak ve silmek ve ileti eklemek, almak ve silmek için Python'dan Azure Queue v2.1 hizmetini nasıl kullanacağınızı öğrenin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/17/2019
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.custom: seo-javascript-october2019
ms.openlocfilehash: ca0831fd7554058d21e315b67d6965579af1d38b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060911"
---
# <a name="how-to-use-azure-queue-storage-v21-from-python"></a>Python'dan Azure Queue depolama v2.1 nasıl kullanılır?

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

Bu makalede, Azure Sıra depolama hizmeti kullanılarak sık karşılaşılan senaryolar gösterilmiş. Kapsanan senaryolar arasında sıra iletileri ekleme, gözetleme, alma ve silme ve kuyruk oluşturma ve silme yer alır.

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış

Bu makaledeki örnekler Python'da yazılır ve [Python için Microsoft Azure Depolama SDK'sını]kullanın. Kuyruklar hakkında daha fazla bilgi için [Sonraki adımlar](#next-steps) bölümüne bakın.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Python için Azure Depolama SDK'sını indirin ve yükleyin

[Python için Azure Depolama SDK'sı](https://github.com/azure/azure-storage-python) Python sürüm 2.7, 3.3 veya daha sonra gerektirir.
 
### <a name="install-via-pypi"></a>PyPi üzerinden yükleyin

Python Paket Dizini (PyPI) üzerinden yüklemek için şunları yazın:

```bash
pip install azure-storage-queue==2.1.0
```

> [!NOTE]
> Python sürümü 0.36 veya daha önceki Azure Depolama SDK'sından yükseltme alıyorsanız, `pip uninstall azure-storage` en son paketi yüklemeden önce eski SDK'yı kaldırın.

Alternatif yükleme yöntemleri [için Python için Azure Depolama SDK'ya](https://github.com/Azure/azure-storage-python/)bakın.

## <a name="view-the-sample-application"></a>Örnek uygulamayı görüntüleme

Python'un Azure Kuyruklarıyla nasıl kullanılacağını gösteren bir örnek uygulamayı görüntülemek ve çalıştırmak için [Bkz. Azure Depolama: Python'da Azure Kuyruklarıyla Başlarken.](https://github.com/Azure-Samples/storage-queue-python-getting-started) 

Örnek uygulamayı çalıştırmak için hem paketleri `azure-storage-queue` yüklediğinizden `azure-storage-common` emin olun.

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

[QueueService](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice) nesnesi kuyruklarla çalışmanızı sağlar. Aşağıdaki kod bir `QueueService` nesne oluşturur. Azure Depolama'ya programlı olarak erişmek istediğiniz herhangi bir Python dosyasının üst yanına aşağıdakileri ekleyin:

```python
from azure.storage.queue import QueueService
```

Aşağıdaki kod, depolama `QueueService` hesabı adı ve hesap anahtarını kullanarak bir nesne oluşturur. *Hesabımı* ve *anahtarımı* hesap adınızı ve anahtarınızla değiştirin.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="insert-a-message-into-a-queue"></a>Kuyruğa bir ileti yerleştirme

Bir iletiyi kuyruğa eklemek için, yeni bir ileti oluşturmak ve kuyruğa eklemek için [put_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#put-message-queue-name--content--visibility-timeout-none--time-to-live-none--timeout-none-) yöntemini kullanın.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

Azure sıra iletileri metin olarak depolanır. İkili verileri depolamak istiyorsanız, sıraya bir ileti koymadan önce base64 kodlama ve kod çözme işlevlerini sıraya koymadan önce sıralı hizmet nesnesinde düzenleyin.

```python
# setup queue Base64 encoding and decoding functions
queue_service.encode_function = QueueMessageFormat.binary_base64encode
queue_service.decode_function = QueueMessageFormat.binary_base64decode
```

## <a name="peek-at-the-next-message"></a>Sonraki iletiye gözatın

[peek_messages](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#peek-messages-queue-name--num-messages-none--timeout-none-) yöntemini arayarak sıranın önündeki iletiye sıranın önünden göz atabilirsiniz. Varsayılan olarak, `peek_messages` tek bir iletiyi gözetler.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="dequeue-messages"></a>İletileri sıradan ayır

Kodunuz iki adımda bir iletiyi kuyruktan kaldırır. [get_messages](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#get-messages-queue-name--num-messages-none--visibility-timeout-none--timeout-none-)aradiğinizde, varsayılan olarak bir sırada bir sonraki iletiyi alırsınız. Döndürülen `get_messages` ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı bitirmek için [delete_message'ı](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#delete-message-queue-name--message-id--pop-receipt--timeout-none-)da aramanız gerekir. İletiyi kaldırma işlemi, kodunuz donanım veya yazılım hatası nedeniyle bir iletiyi işlemezse, kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. İleti işlendikten hemen sonra kodunuz çağrır. `delete_message`

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneği, `get_messages` tek bir aramada 16 ileti almak için yöntemi kullanır. Daha sonra her iletiyi for döngüsü kullanarak işler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```python
messages = queue_service.get_messages(
    'taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

## <a name="change-the-contents-of-a-queued-message"></a>Kuyruğa alınan iletinin içeriğini değiştirme

Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod, bir iletiyi güncelleştirmek için [update_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#update-message-queue-name--message-id--pop-receipt--visibility-timeout--content-none--timeout-none-) yöntemini kullanır. Görünürlük zaman süresi 0 olarak ayarlanır, bu da iletinin hemen görüntülenmesi ve içeriğin güncelleştirilmesi anlamına gelir.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message(
        'taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. [get_queue_metadata](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#get-queue-metadata-queue-name--timeout-none-) yöntemi, sıra yla ilgili meta verileri döndürmek `approximate_message_count`için sıra hizmetinden ve . Kuyruk hizmeti isteğinize yanıt verdikten sonra iletiler eklenebileceği veya kaldırılabildiği için sonuç yalnızca yaklaşık olarak karşılanır.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir kuyruğ ve içinde bulunan tüm iletileri silmek için [delete_queue](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#delete-queue-queue-name--fail-not-exist-false--timeout-none-) yöntemini arayın.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Sonraki adımlar

Artık sıra depolamanın temellerini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Azure Kuyrukları Python API başvurusu](/python/api/azure-storage-queue)
* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Azure Storage Hizmetleri REST API’si](https://msdn.microsoft.com/library/azure/dd179355)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[Python için Microsoft Azure Depolama SDK]: https://github.com/Azure/azure-storage-python
