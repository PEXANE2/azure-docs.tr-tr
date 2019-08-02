---
title: Python 'da kuyruk depolama kullanma-Azure depolama
description: Sıraları oluşturmak ve silmek ve iletileri eklemek, almak ve silmek için Python 'dan Azure Kuyruk hizmeti kullanmayı öğrenin.
author: mhopkins-msft
ms.service: storage
ms.author: mhopkins
ms.date: 12/14/2018
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: 1ed084bfa0cf6879983e38ac6a8c5ab57e8948a8
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721364"
---
# <a name="how-to-use-queue-storage-from-python"></a>Python’dan Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuzda, Azure kuyruk depolama hizmetini kullanarak genel senaryoları nasıl gerçekleştireceğiniz gösterilmektedir. Örnekler Python 'da yazılır ve [Python için SDK Microsoft Azure Depolama]kullanır. Kapsanan senaryolar sıra iletilerini **ekleme**, göz **atma**, **alma**ve **silme** , Ayrıca kuyruk **oluşturma ve silme**içerir. Kuyruklar hakkında daha fazla bilgi için, [sonraki adımlar] bölümüne bakın.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Python için Azure depolama SDK 'sını indirme ve yükleme

[Python Için Azure depolama SDK 'sı](https://github.com/azure/azure-storage-python) , Python 2,7, 3,3, 3,4, 3,5 veya 3,6 gerektirir.
 
### <a name="install-via-pypi"></a>PyPi aracılığıyla yüklemesi

Python paket dizini (Pypı) aracılığıyla yüklemek için şunu yazın:

```bash
pip install azure-storage-queue
```

> [!NOTE]
> Python sürüm 0,36 veya önceki bir sürümü için Azure Storage SDK 'dan yükseltiyorsanız, en son paketi yüklemeden önce kullanarak `pip uninstall azure-storage` eski SDK 'yı kaldırın.

Alternatif yükleme yöntemleri için bkz. [Python Için Azure Storage SDK](https://github.com/Azure/azure-storage-python/).

## <a name="view-the-sample-application"></a>Örnek uygulamayı görüntüleme

Azure kuyrukları ile Python 'un nasıl kullanılacağını gösteren örnek bir uygulamayı görüntülemek ve çalıştırmak için bkz [. Azure Storage: Python](https://github.com/Azure-Samples/storage-queue-python-getting-started)'da Azure kuyrukları ile çalışmaya başlama. 

Örnek uygulamayı çalıştırmak için hem `azure-storage-queue` hem `azure-storage-common` de paketlerini yüklediğinizden emin olun.

## <a name="how-to-create-a-queue"></a>Nasıl Yapılır: Sıra oluşturma

**QueueService** nesnesi kuyruklarla çalışmanıza olanak sağlar. Aşağıdaki kod bir **QueueService** nesnesi oluşturur. Azure Storage 'a programlı bir şekilde erişmek istediğiniz herhangi bir Python dosyasının en üstüne aşağıdakileri ekleyin:

```python
from azure.storage.queue import QueueService
```

Aşağıdaki kod, depolama hesabı adını ve hesap anahtarını kullanarak bir **QueueService** nesnesi oluşturur. ' Myaccount ' ve ' MyKey ' değerini hesap adınızla ve anahtarınızla değiştirin.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl Yapılır: Bir kuyruğa Ileti ekleme
Bir kuyruğa ileti eklemek için, **PUT\_Message** metodunu kullanarak yeni bir ileti oluşturun ve kuyruğa ekleyin.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl Yapılır: Sonraki Iletiye göz atın
**İletileri Peek\_** yöntemini çağırarak sıradan kaldırmadan bir kuyruğun önündeki iletiye göz atmayı sağlayabilirsiniz. Varsayılan olarak, **iletilerin\_göz atma** tek bir iletiye göz atar.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Nasıl Yapılır: Iletileri sıradan çıkarma
Kodunuz bir iletiyi iki adımda kuyruktan kaldırır. **İletileri al\_** ' ı çağırdığınızda, bir sıradaki bir sonraki iletiyi varsayılan olarak alırsınız. **Get\_iletilerinden** döndürülen bir ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırma işleminin tamamlanabilmesi için, **silme\_iletisini**de çağırmanız gerekir. Bir iletiyi kaldırmanın bu iki adımlı işlemi, kodunuz, donanım veya yazılım arızasından kaynaklanan bir iletiyi işleyemediğinde, kodunuzun başka bir örneğinin aynı mesajı almasını ve yeniden denemesini sağlar. Kodunuz, ileti işlendikten hemen sonra **iletiyi Sil\_** ' i çağırır.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.
İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde, tek bir çağrıda 16 ileti almak için **iletileri al\_** yöntemi kullanılmaktadır. Sonra her iletiyi bir for döngüsü kullanarak işler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```python
messages = queue_service.get_messages(
    'taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl Yapılır: Sıraya alınan Iletinin Içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod, bir iletiyi güncelleştirmek için **Update\_Message** yöntemini kullanır. Görünürlük zaman aşımı 0 olarak ayarlanır, yani ileti hemen görünür ve içerik güncellenir.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message(
        'taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a>Nasıl Yapılır: Sıra uzunluğunu al
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **\_Sırameta\_verilerini al** yöntemi, kuyruk hizmetinin kuyruğa ve **approximate_message_count**ilgili meta verileri döndürmesini ister. Sonuç yalnızca, kuyruk hizmeti isteğinize yanıt verdikten sonra iletiler eklenebildiğinden veya kaldırılacağından yaklaşık olur.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Nasıl Yapılır: Kuyruğu silme
Bir kuyruğu ve içerdiği tüm iletileri silmek için **sırayı Sil\_** yöntemini çağırın.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Sonraki Adımlar
Sıra depolamanın temellerini öğrendiğinize göre, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[Python için SDK Microsoft Azure Depolama]: https://github.com/Azure/azure-storage-python
