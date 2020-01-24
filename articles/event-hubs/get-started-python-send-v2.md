---
title: Python kullanarak olay gönderme veya alma-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs olayları gönderen bir Python uygulaması oluşturmak için İzlenecek yol sunulmaktadır.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 01/08/2020
ms.author: spelluru
ms.openlocfilehash: d7ab79d49aade7dd6e98cf33ce538174d176c784
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705352"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-using-python"></a>Python kullanarak Event Hubs olayları gönderme veya olayları alma

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı veya işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Event Hubs’a ayrıntılı bir genel bakış için bkz. [Event Hubs'a genel bakış](event-hubs-about.md) ve [Event Hubs özellikleri](event-hubs-features.md).

Bu öğreticide, Olay Hub 'ından olayları göndermek veya olayları almak için Python uygulamalarının nasıl oluşturulacağı açıklanmaktadır.

> [!IMPORTANT]
> Bu hızlı başlangıçta Azure Event Hubs Python SDK 'sının 5. sürümü kullanılmaktadır. Python SDK 'sının eski sürüm 1 ' i kullanan bir hızlı başlangıç için, [Bu makaleye](event-hubs-python-get-started-send.md)bakın. SDK sürüm 1 ' i kullanıyorsanız, kodunuzu en son sürüme geçirmeniz önerilir. Ayrıntılar için bkz. [Geçiş Kılavuzu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- [Hızlı başlangıç: Azure Portal kullanarak bir olay hub 'ı oluşturmak](event-hubs-create.md)için bir Active Event Hubs ad alanı ve Olay Hub 'ı oluşturulur. Bu izlenecek yolda daha sonra kullanmak üzere ad alanı ve Olay Hub 'ı adlarını bir yere unutmayın.
- Event Hubs ad alanınız için paylaşılan erişim anahtarı adı ve birincil anahtar değeri. [Bağlantı dizesini al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)bölümündeki yönergeleri izleyerek erişim anahtarı adını ve değerini alın. Varsayılan erişim anahtarı adı **RootManageSharedAccessKey**' dir. Bu izlenecek yolda daha sonra kullanmak için anahtar adını ve birincil anahtar değerini kopyalayın.
- `pip` yüklenip güncelleştirildiği için Python 2,7 ve 3,5 ya da üzeri.
- Event Hubs için Python paketi. Paketi yüklemek için bu komutu, yolunda Python içeren bir komut isteminde çalıştırın:

    ```cmd
    pip install azure-eventhub
    ```

    Denetim noktası deposu olarak bir Azure Blob depolama alanı kullanarak olayları almak için bu paketi yükler.

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```

## <a name="send-events"></a>Olayları gönderme
Bu bölümde, daha önce oluşturduğunuz Olay Hub 'ına olayları göndermek için bir Python betiği oluşturacaksınız.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz Python düzenleyicisini açın
2. Adlı bir betik oluşturma **send.py**. Bu betik, daha önce oluşturduğunuz Olay Hub 'ına bir olay toplu işi gönderir.
3. Aşağıdaki kodu send.py ' ye yapıştırın. Ayrıntılar için bkz. kod açıklamaları.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubProducerClient
    from azure.eventhub import EventData

    async def run():
        # create a producer client to send messages to the event hub
        # specify connection string to your event hubs namespace and
            # the event hub name
        producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESPACE - CONNECTION STRING", eventhub_name="EVENT HUB NAME")
        async with producer:
            # create a batch
            event_data_batch = await producer.create_batch()

            # add events to the batch
            event_data_batch.add(EventData('First event '))
            event_data_batch.add(EventData('Second event'))
            event_data_batch.add(EventData('Third event'))

            # send the batch of events to the event hub
            await producer.send_batch(event_data_batch)

    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())

    ```

    > [!NOTE]
    > Çok faydalı yorumlarla tüm kaynak kodu için [GitHub 'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py) bakın

## <a name="receive-events"></a>Olayları alma
Bu hızlı başlangıç, bir Azure Blob depolama alanını denetim noktası deposu olarak kullanır. Denetim noktası deposu, kontrol noktalarını kalıcı hale getirmek için kullanılır (son okuma konumu).  

### <a name="create-an-azure-storage-and-a-blob-container"></a>Azure depolama ve BLOB kapsayıcısı oluşturma
Bir Azure Depolama hesabını bir blob kapsayıcısı oluşturmak için bu adımları izleyin.

1. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Bağlantı dizesini depolama hesabına al](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Bağlantı dizesi ve kapsayıcı adı ' nı aklınızda edin. Bunları alma kodunda kullanacaksınız.


### <a name="create-python-script-to-receive-events"></a>Olayları almak için Python betiği oluşturma

Bu bölümde, Olay Hub 'ınızdan olayları almak için bir Python betiği oluşturursunuz:

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz Python düzenleyicisini açın
2. Adlı bir betik oluşturma **recv.py**.
3. Aşağıdaki kodu recv.py ' ye yapıştırın. Ayrıntılar için bkz. kod açıklamaları.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubConsumerClient
    from azure.eventhub.extensions.checkpointstoreblobaio import BlobCheckpointStore


    async def on_event(partition_context, event):
        # print the event data
        print("Received the event: \"{}\" from the partition with ID: \"{}\"".format(event.body_as_str(encoding='UTF-8'), partition_context.partition_id))

        # update the checkpoint so that the program doesn't read the events
        # that it has already read when you run it next time
        await partition_context.update_checkpoint(event)

    async def main():
        # create an Azure blob checkpoint store to store the checkpoints
        checkpoint_store = BlobCheckpointStore.from_connection_string("AZURE STORAGE CONNECTION STRING", "BLOB CONTAINER NAME")

        # create a consumer client for the event hub
        client = EventHubConsumerClient.from_connection_string("EVENT HUBS NAMESPACE CONNECTION STRING", consumer_group="$Default", eventhub_name="EVENT HUB NAME", checkpoint_store=checkpoint_store)
        async with client:
            # call the receive method
            await client.receive(on_event=on_event)

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # run the main method
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > Çok faydalı yorumlarla tüm kaynak kodu için [GitHub 'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py) bakın


### <a name="run-the-receiver-app"></a>Alıcı uygulamasını çalıştırma

Betiği çalıştırmak için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın:

```bash
python recv.py
```

### <a name="run-the-sender-app"></a>Gönderen uygulamasını çalıştırma

Betiği çalıştırmak için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın:

```bash
python send.py
```

Alıcı penceresinde Olay Hub 'ına gönderilen iletileri görmeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, olayları zaman uyumsuz olarak gönderdiniz ve alacaksınız. Olayları zaman uyumlu olarak gönderme ve alma hakkında bilgi edinmek için [Bu konumdaki](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples)örneklere bakın.

GitHub 'da tüm örnekleri (eşitleme ve zaman uyumsuz) [buradan](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples)bulabilirsiniz.
