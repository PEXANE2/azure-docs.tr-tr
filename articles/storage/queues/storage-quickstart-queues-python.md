---
title: 'Hızlı başlangıç: Azure kuyruk depolama kitaplığı V12-Python'
description: Bir kuyruk oluşturmak ve kuyruğa ileti eklemek için Azure kuyruğu Python V12 kitaplığı 'nı nasıl kullanacağınızı öğrenin. Ardından, sıradaki iletileri okumayı ve silmeyi öğreneceksiniz. Ayrıca, bir kuyruğu silmeyi de öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/10/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: 12c2652b4dcef46c5affde2c3fb9ef9288176eb9
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852268"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-python"></a>Hızlı başlangıç: Python için Azure kuyruk depolama istemci kitaplığı V12

Python için Azure kuyruk depolama istemci kitaplığı sürüm 12 ile çalışmaya başlayın. Azure kuyruk depolaması, daha sonra almak ve işlemek üzere çok sayıda ileti depolamaya yönelik bir hizmettir. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere bu adımları izleyin.

Python için Azure kuyruk depolama istemci kitaplığı V12 şu şekilde kullanın:

* Bir kuyruk oluşturma
* Bir kuyruğa ileti ekleme
* Kuyruktaki iletilere göz atın
* Kuyruktaki bir iletiyi güncelleştirme
* Kuyruktan ileti alma
* Kuyruktaki iletileri silme
* Bir kuyruk silme

Ek kaynaklar:

* [API başvuru belgeleri](https://docs.microsoft.com/python/api/azure-storage-queue/index)
* [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue)
* [Paket (Python paket dizini)](https://pypi.org/project/azure-storage-queue/)
* [Örnekler](https://docs.microsoft.com/azure/storage/common/storage-samples-python?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı- [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için [Python](https://www.python.org/downloads/) -2,7, 3,5 veya üzeri

## <a name="setting-up"></a>Ayarlanıyor

Bu bölüm, Python için Azure kuyruk depolama istemci kitaplığı V12 ile çalışmak üzere bir proje hazırlama konusunda size yol gösterir.

### <a name="create-the-project"></a>Proje oluşturma

Kuyruklar adlı bir Python uygulaması oluşturma *-hızlı başlangıç-V12*.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), proje için yeni bir dizin oluşturun.

    ```console
    mkdir queues-quickstart-v12
    ```

1. Yeni oluşturulan *kuyruklara geç hızlı başlangıç-V12* dizini.

    ```console
    cd queues-quickstart-v12
    ```

### <a name="install-the-package"></a>Paketi yükler

Komutunu kullanarak Python için Azure Blob Storage istemci kitaplığı 'nı paketi ' ni yükler `pip install` .

```console
pip install azure-storage-queue
```

Bu komut, Python paketi için Azure kuyruk depolama istemci kitaplığını ve bağımlı olduğu tüm kitaplıkları kurar. Bu durumda, yalnızca Python için Azure çekirdek kitaplığı vardır.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

1. Kod Düzenleyicinizde yeni bir metin dosyası açın
1. `import`Deyim Ekle
1. Çok temel özel durum işleme dahil olmak üzere programın yapısını oluşturma

    Kod şu şekildedir:

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

1. Yeni dosyayı *Kuyruklar-QuickStart-V12* dizinine *Queues-QuickStart-v12.py* olarak kaydedin.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Kuyruk iletisi boyutu 64 KB 'ye kadar olabilir. Bir kuyruk, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle zaman uyumsuz olarak işlenecek iş biriktirme listesi oluşturmak için kullanılır. Kuyruk depolama, üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabındaki bir kuyruk
* Kuyruktaki iletiler

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Kuyruk depolama mimarisi diyagramı](./media/storage-queues-introduction/queue1.png)

Şu kaynaklarla etkileşim kurmak için aşağıdaki Python sınıflarını kullanın:

* [QueueServiceClient](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueserviceclient):, `QueueServiceClient` Depolama hesabınızdaki tüm kuyrukları yönetmenizi sağlar.
* [Queueclient](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient): `QueueClient` sınıfı, tek bir kuyruğu ve iletilerini yönetmenizi ve düzenlemenizi sağlar.
* [Queuemessage](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queuemessage): `QueueMessage` sınıfı, bir kuyrukta [receive_messages](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-) çağrılırken döndürülen ayrı nesneleri temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, Python için Azure kuyruk depolama istemci kitaplığı ile aşağıdaki eylemleri nasıl yapılacağını gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kuyruk oluşturma](#create-a-queue)
* [Bir kuyruğa ileti ekleme](#add-messages-to-a-queue)
* [Kuyruktaki iletilere göz atın](#peek-at-messages-in-a-queue)
* [Kuyruktaki bir iletiyi güncelleştirme](#update-a-message-in-a-queue)
* [Kuyruktan ileti alma](#receive-messages-from-a-queue)
* [Kuyruktaki iletileri silme](#delete-messages-from-a-queue)
* [Bir kuyruk silme](#delete-a-queue)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, depolama hesabı için bağlantı dizesini alır. Bağlantı dizesi, [depolama Bağlantı dizenizi yapılandırma](#configure-your-storage-connection-string) bölümünde oluşturulan ortam değişkenini saklı tutulur.

Bu kodu bloğunun içine ekleyin `try` :

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

Yeni sıra için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için kuyruk adına bir UUID değeri ekler.

> [!IMPORTANT]
> Kuyruk adları yalnızca küçük harf, sayı ve kısa çizgi içerebilir ve bir harf veya sayı ile başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır. Ad ayrıca 3 ila 63 karakter uzunluğunda olmalıdır. Adlandırma sıraları hakkında daha fazla bilgi için bkz. [adlandırma sıraları ve meta verileri](https://docs.microsoft.com/rest/api/storageservices/naming-queues-and-metadata).

[Queueclient](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızda kuyruğu oluşturmak için [create_queue](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#create-queue---kwargs-) yöntemini çağırın.

Bu kodu bloğunun sonuna ekleyin `try` :

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

### <a name="add-messages-to-a-queue"></a>Bir kuyruğa ileti ekleme

Aşağıdaki kod parçacığı [send_message](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-) yöntemini çağırarak kuyruğa ileti ekler. Ayrıca, üçüncü çağrıdan döndürülen [Queuemessage](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queuemessage) öğesini de kaydeder `send_message` . , `saved_message` Programın ilerleyen kısımlarında ileti içeriğini güncelleştirmek için kullanılır.

Bu kodu bloğunun sonuna ekleyin `try` :

```python
    print("\nAdding messages to the queue...")

    # Send several messages to the queue
    queue_client.send_message(u"First message")
    queue_client.send_message(u"Second message")
    saved_message = queue_client.send_message(u"Third message")
```

### <a name="peek-at-messages-in-a-queue"></a>Kuyruktaki iletilere göz atın

[Peek_messages](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#peek-messages-max-messages-none----kwargs-) yöntemini çağırarak kuyruktaki iletilere göz atın. `peek_messages`Yöntemi, sıranın önüne bir veya daha fazla ileti alır ancak iletinin görünürlüğünü değiştirmez.

Bu kodu bloğunun sonuna ekleyin `try` :

```python
    print("\nPeek at the messages in the queue...")

    # Peek at messages in the queue
    peeked_messages = queue_client.peek_messages(max_messages=5)

    for peeked_message in peeked_messages:
        # Display the message
        print("Message: " + peeked_message.content)
```

### <a name="update-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi güncelleştirme

[Update_message](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#update-message-message--pop-receipt-none--content-none----kwargs-) yöntemini çağırarak bir iletinin içeriğini güncelleştirin. `update_message`Yöntemi, bir iletinin görünürlük zaman aşımını ve içeriğini değiştirebilir. İleti içeriği, boyutu 64 KB 'a kadar olan bir UTF-8 kodlu dize olmalıdır. Yeni içerikle birlikte, kodda daha önce kaydedilen iletiden değer geçirin. `saved_message`Değerler güncelleştirilecek iletiyi belirler.

```python
    print("\nUpdating the third message in the queue...")

    # Update a message using the message saved when calling send_message earlier
    queue_client.update_message(saved_message, pop_receipt=saved_message.pop_receipt, \
        content="Third message has been updated")
```

### <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

[Receive_messages](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-) yöntemini çağırarak önceden eklenmiş iletileri indirin.

Bu kodu bloğunun sonuna ekleyin `try` :

```python
    print("\nReceiving messages from the queue...")

    # Get messages from the queue
    messages = queue_client.receive_messages(messages_per_page=5)
```

### <a name="delete-messages-from-a-queue"></a>Kuyruktaki iletileri silme

İletileri alındıktan ve işlendikten sonra kuyruktan silin. Bu durumda, işleme yalnızca konsolda iletiyi görüntülüyor.

Uygulama, `input` iletileri işleyerek ve silmeden önce çağırarak kullanıcı girişi için duraklatılır. [Azure Portal](https://portal.azure.com) , kaynakların silinmeden önce doğru şekilde oluşturulduğunu doğrulayın. Açıkça silinmeyen tüm iletiler, daha sonra bu işlemleri işlemek için bir süre sonra sırada görünür hale gelir.

Bu kodu bloğunun sonuna ekleyin `try` :

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

Aşağıdaki kod, [delete_queue](https://docs.microsoft.com/python/api/azure-storage-queue/azure.storage.queue.queueclient#delete-queue---kwargs-) yöntemi kullanılarak sırayı silerek uygulamanın oluşturduğu kaynakları temizler.

Bu kodu bloğunun sonuna ekleyin `try` ve dosyayı kaydedin:

```python
    print("\nPress Enter key to delete the queue...")
    input()

    # Clean up
    print("Deleting queue...")
    queue_client.delete_queue()

    print("Done")
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, bir Azure kuyruğuna üç ileti oluşturur ve ekler. Kod kuyruktaki iletileri listeler, ardından kuyruğu silmeden önce bunları alır ve siler.

Konsol pencerenizde, *Queues-QuickStart-v12.py* dosyasını içeren dizine gidin ve `python` uygulamayı çalıştırmak için aşağıdaki komutu yürütün.

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

Uygulama iletileri almadan önce durakladığında, [Azure Portal](https://portal.azure.com)depolama hesabınızı kontrol edin. İletilerin kuyrukta olduğunu doğrulayın.

İletileri almak ve silmek için **ENTER** tuşuna basın. İstendiğinde, kuyruğu silmek ve tanıtımı sona ermesini sağlamak için **ENTER** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir sıranın nasıl oluşturulacağını ve Python kodunu kullanarak iletiye nasıl ileti ekleneceğini öğrendiniz. Ardından iletileri göz atmayı, almayı ve silmeyi öğrendiniz. Son olarak, bir ileti sırasının nasıl silineceğini öğrendiniz.

Öğreticiler, örnekler, hızlı ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [Python Geliştiricileri için Azure](https://docs.microsoft.com/azure/python/)

* Daha fazla bilgi edinmek için bkz. [Python Için Azure depolama kitaplıkları](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage).
* Daha fazla Azure kuyruk depolama örnek uygulaması görmek için [Azure kuyruk depolama V12 Python istemci kitaplığı örneklerine](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue/samples)geçin.
