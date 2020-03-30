---
title: 'Hızlı başlangıç: Azure Sıra depolama kitaplığı v12 - Python'
description: Kuyruk oluşturmak ve kuyruğa ileti eklemek için Azure Queue Python v12 kitaplığını nasıl kullanacağınızı öğrenin. Ardından, sıradaki iletileri nasıl okuyup sildiğinizi öğrenirsiniz. Ayrıca bir sırayı nasıl sildiğinizi de öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/10/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.openlocfilehash: d6ccd3cc61f9d8244874823be76496a4f4e1073c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78199776"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-python"></a>Quickstart: Python için Azure Queue depolama istemcikitapv12

Python için Azure Queue depolama istemcisi kitaplığı sürümü 12 ile başlayın. Azure Sıra depolama, daha sonra alma ve işleme için çok sayıda ileti depolamak için bir hizmettir. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Python için Azure Queue depolama istemcisi v12'yi kullanın:

* Bir kuyruk oluşturma
* Kuyruğa ileti ekleme
* Kuyruktaki iletilere göz atın
* Kuyruktaki bir iletiyi güncelleştirme
* Kuyruktan ileti alma
* İletileri kuyruktan silme
* Bir kuyruk silme

[API referans belgeleri](https://docs.microsoft.com/python/api/azure-storage-queue/index) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue) | [Paketi (Python Paket Dizini)](https://pypi.org/project/azure-storage-queue/) | [Örnekleri](https://docs.microsoft.com/azure/storage/common/storage-samples-python?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı - [bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için [Python](https://www.python.org/downloads/) - 2.7, 3.5 veya üzeri

## <a name="setting-up"></a>Ayarlama

Bu bölüm, Python için Azure Queue depolama istemcisi kitaplığı v12 ile çalışmak üzere bir proje hazırlamakonusunda size yol sunar.

### <a name="create-the-project"></a>Proje oluşturma

*Kuyruklar-quickstart-v12*adlı bir Python uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), proje için yeni bir dizin oluşturun.

    ```console
    mkdir queues-quickstart-v12
    ```

1. Yeni oluşturulan *kuyruklar-quickstart-v12* dizinine geçin.

    ```console
    cd queues-quickstart-v12
    ```

### <a name="install-the-package"></a>Paketi yükleyin

Komutu kullanarak Python paketi için Azure Blob depolama istemcisi kitaplığını yükleyin. `pip install`

```console
pip install azure-storage-queue
```

Bu komut, Python paketi için Azure Kuyruk depolama istemcisi kitaplığını ve bağlı olduğu tüm kitaplıkları yükler. Bu durumda, bu Python için azure çekirdek kitaplığıdır.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

1. Kod düzenleyicinizde yeni bir metin dosyası açma
1. Ekstre ekleme `import`
1. Çok temel özel durum işleme de dahil olmak üzere program için yapı oluşturma

    İşte kod:

    ```python
    import os, uuid
    from azure.storage.queue import QueueServiceClient, QueueClient, QueueMessage

    try:
        print("Azure Queue storage v12 - Python quickstart sample")
        # Quick start code goes here
    except Exception as ex:
        print('Exception:')
        print(ex)

    ```

1. Yeni dosyayı *kuyruklar-quickstart-v12* dizininde *queues-quickstart-v12.py* olarak kaydedin.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Bir sıra iletisi 64 KB boyutuna kadar olabilir. Bir sıra, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle eşzamanlı olarak işlemek için bir çalışma biriktirme listesi oluşturmak için kullanılır. Sıra depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabında bir sıra
* Sıra içindeki iletiler

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Sıra depolama mimarisi diyagramı](./media/storage-queues-introduction/queue1.png)

Bu kaynaklarla etkileşimkurmak için aşağıdaki Python sınıflarını kullanın:

* [QueueServiceClient](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueserviceclient): `QueueServiceClient` Depolama hesabınızdaki tüm kuyrukları yönetmenize olanak tanır.
* [QueueClient](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient): `QueueClient` Sınıf, tek bir sırayı ve iletilerini yönetmenize ve işlemenize olanak tanır.
* [QueueMessage](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queuemessage): `QueueMessage` Sınıf, receive_messages bir sırada [ararken](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-) döndürülen tek tek nesneleri temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, Python için Azure Kuyruk depolama istemcisi kitaplığıyla aşağıdaki işlemleri nasıl yapacağınızı gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kuyruk oluşturma](#create-a-queue)
* [Kuyruğa ileti ekleme](#add-messages-to-a-queue)
* [Kuyruktaki iletilere göz atın](#peek-at-messages-in-a-queue)
* [Kuyruktaki bir iletiyi güncelleştirme](#update-a-message-in-a-queue)
* [Kuyruktan ileti alma](#receive-messages-from-a-queue)
* [İletileri kuyruktan silme](#delete-messages-from-a-queue)
* [Bir kuyruk silme](#delete-a-queue)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod depolama hesabının bağlantı dizesini alır. Bağlantı dizesi, [depolama bağlantı dizesi](#configure-your-storage-connection-string) bölümüne Yapılandırıldığında oluşturulan ortam değişkeni depolanır.

Bu kodu `try` bloğun içine ekleyin:

```python
    # Retrieve the connection string for use with the application. The storage
    # connection string is stored in an environment variable on the machine
    # running the application called AZURE_STORAGE_CONNECTION_STRING. If the
    # environment variable is created after the application is launched in a
    # console or with Visual Studio, the shell or application needs to be
    # closed and reloaded to take the environment variable into account.
    connect_str = os.getenv('AZURE_STORAGE_CONNECTION_STRING')
```

### <a name="create-a-queue"></a>Bir kuyruk oluşturma

Yeni sıra için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için sıra adına bir UUID değerini ekler.

> [!IMPORTANT]
> Sıra adları yalnızca küçük harfler, sayılar ve tireler içerebilir ve bir harf veya sayıyla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır. Ad da 3 ve 63 karakter uzunluğunda olmalıdır. Sıraları adlandırma hakkında daha fazla bilgi için [Bkz.](https://docs.microsoft.com/rest/api/storageservices/naming-queues-and-metadata)

[QueueClient](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızdaki sırayı oluşturmak için [create_queue](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#create-queue---kwargs-) yöntemini arayın.

Bu kodu `try` bloğun sonuna ekleyin:

```python
    # Create a unique name for the queue
    queue_name = "quickstartqueues-" + str(uuid.uuid4())

    print("Creating queue: " + queue_name)

    # Instantiate a QueueClient which will be
    # used to create and manipulate the queue
    queue_client = QueueClient.from_connection_string(connect_str, queue_name)

    # Create the queue
    queue_client.create_queue()
```

### <a name="add-messages-to-a-queue"></a>Kuyruğa ileti ekleme

Aşağıdaki kod snippet [send_message](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-) yöntemini çağırarak sıraya iletiekler. Ayrıca üçüncü `send_message` çağrıdan döndürülen [QueueMessage'ı](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queuemessage) da kaydeder. İleti `saved_message` içeriğini daha sonra programda güncelleştirmek için kullanılır.

Bu kodu `try` bloğun sonuna ekleyin:

```python
    print("\nAdding messages to the queue...")

    # Send several messages to the queue
    queue_client.send_message(u"First message")
    queue_client.send_message(u"Second message")
    saved_message = queue_client.send_message(u"Third message")
```

### <a name="peek-at-messages-in-a-queue"></a>Kuyruktaki iletilere göz atın

[peek_messages](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#peek-messages-max-messages-none----kwargs-) yöntemini arayarak kuyruktaki iletilere göz atın. Yöntem, `peek_messages` sıranın önünden bir veya daha fazla ileti alır, ancak iletinin görünürlüğünü değiştirmez.

Bu kodu `try` bloğun sonuna ekleyin:

```python
    print("\nPeek at the messages in the queue...")

    # Peek at messages in the queue
    peeked_messages = queue_client.peek_messages(max_messages=5)

    for peeked_message in peeked_messages:
        # Display the message
        print("Message: " + peeked_message.content)
```

### <a name="update-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi güncelleştirme

[update_message](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#update-message-message--pop-receipt-none--content-none----kwargs-) yöntemini arayarak iletinin içeriğini güncelleştirin. Yöntem, `update_message` iletinin görünürlük zaman anına ve içeriğini değiştirebilir. İleti içeriği 64 KB boyutuna kadar olan UTF-8 kodlanmış bir dize olmalıdır. Yeni içerikle birlikte, kodda daha önce kaydedilmiş iletiden değerleri aktarın. Değerler `saved_message` hangi iletiyi güncelleştireceklerini belirler.

```python
    print("\nUpdating the third message in the queue...")

    # Update a message using the message saved when calling send_message earlier
    queue_client.update_message(saved_message, pop_receipt=saved_message.pop_receipt, \
        content="Third message has been updated")
```

### <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

[receive_messages](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-) yöntemini arayarak daha önce eklenen iletileri indirin.

Bu kodu `try` bloğun sonuna ekleyin:

```python
    print("\nReceiving messages from the queue...")

    # Get messages from the queue
    messages = queue_client.receive_messages(messages_per_page=5)
```

### <a name="delete-messages-from-a-queue"></a>İletileri kuyruktan silme

İletileri alındıktan ve işlendikten sonra kuyruktan silin. Bu durumda, işleme yalnızca konsolda ileti yi görüntüler.

Uygulama, iletileri işleyip silmeden önce arayarak `input` kullanıcı girişi için duraklatır. [Azure portalınızda](https://portal.azure.com) kaynakların silinmeden önce doğru oluşturulduğunu doğrulayın. Açıkça silinmeyen iletiler, bunları işlemek için başka bir şans için kuyrukta tekrar görünür hale gelir.

Bu kodu `try` bloğun sonuna ekleyin:

```python
    print("\nPress Enter key to 'process' messages and delete them from the queue...")
    input()

    for msg_batch in messages.by_page():
            for msg in msg_batch:
                # "Process" the message
                print(msg.content)
                # Let the service know we're finished with
                # the message and it can be safely deleted.
                queue_client.delete_message(msg)
```

### <a name="delete-a-queue"></a>Bir kuyruk silme

Aşağıdaki kod, [delete_queue](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#delete-queue---kwargs-) yöntemini kullanarak sırayı silerek oluşturulan kaynakları temizler.

Bu kodu `try` bloğun sonuna ekleyin ve dosyayı kaydedin:

```python
    print("\nPress Enter key to delete the queue...")
    input()

    # Clean up
    print("Deleting queue...")
    queue_client.delete_queue()

    print("Done")
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, Azure kuyruğuna üç ileti oluşturur ve ekler. Kod, kuyruktaki iletileri listeler, ardından sırayı silmeden önce alır ve siler.

Konsol pencerenizde, *queues-quickstart-v12.py* dosyasını içeren dizine gidin ve `python` uygulamayı çalıştırmak için aşağıdaki komutu çalıştırın.

```console
python queues-quickstart-v12.py
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Queue storage v12 - Python quickstart sample
Creating queue: quickstartqueues-cac365be-7ce6-4065-bd65-3756ea052cb8

Adding messages to the queue...

Peek at the messages in the queue...
Message: First message
Message: Second message
Message: Third message

Updating the third message in the queue...

Receiving messages from the queue...

Press Enter key to 'process' messages and delete them from the queue...

First message
Second message
Third message has been updated

Press Enter key to delete the queue...

Deleting queue...
Done
```

Uygulama ileti almadan önce durakladığında, [Azure portalındaki](https://portal.azure.com)depolama hesabınızı kontrol edin. İletilerin sırada olduğunu doğrulayın.

İletileri almak ve silmek için **Enter** tuşuna basın. İstendiğinde, sırayı silmek ve demoyu bitirmek için **Enter** tuşuna tekrar basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, Python kodunu kullanarak kuyruk oluşturmayı ve ona ileti eklemeyi öğrendiniz. Ardından iletileri gözetlemeyi, almayı ve silmeyi öğrendiniz. Son olarak, ileti kuyruğunun nasıl silinir öğrenilir.

Öğreticiler, örnekler, hızlı başlangıçlar ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [Python Geliştiricileri için Azure](https://docs.microsoft.com/azure/python/)

* Daha fazla bilgi edinmek [için Python için Azure Depolama kitaplıklarına](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage)bakın.
* Daha fazla Azure Kuyruğu depolama örneği uygulaması görmek için [Azure Queue depolama v12 Python istemci kitaplığı örneklerine](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue/samples)devam edin.
