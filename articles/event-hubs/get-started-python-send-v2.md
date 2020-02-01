---
title: Python kullanarak Azure Event Hubs olay gönderme veya alma (en son)
description: Bu makale, Azure Event Hubs en son Azure-eventhub sürüm 5 paketini kullanarak olayları gönderen/alan bir Python uygulaması oluşturmaya yönelik bir yol sağlar.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: d977ae9ea8b78664ac1d3a318f58553da696c089
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906349"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-python-azure-eventhub-version-5"></a>Python kullanarak Olay Hub 'larına olay gönderme veya olayları alma (Azure-eventhub sürüm 5)

Azure Event Hubs, saniyede milyonlarca olayı alabilen ve işleyesağlayan büyük bir veri akışı platformu ve olay alma hizmetidir. Olay Hub 'ları dağıtılmış yazılım ve cihazlar tarafından üretilen olayları, verileri veya Telemetriyi işleyebilir ve saklayabilir. Bir olay hub 'ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Daha fazla bilgi için bkz. [Event Hubs genel bakış](event-hubs-about.md) ve [Event Hubs özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta, Olay Hub 'ından olay gönderebilen veya olayları alabilen Python uygulamalarının nasıl oluşturulacağı açıklanmaktadır.

> [!IMPORTANT]
> Bu hızlı başlangıçta Azure Event Hubs Python SDK 'sının 5. sürümü kullanılmaktadır. Python SDK 'sının 1. sürümünü kullanan hızlı bir başlangıç için [Bu makaleye](event-hubs-python-get-started-send.md)bakın. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- Etkin bir Event Hubs ad alanı ve Olay Hub 'ı. Bunları oluşturmak için [hızlı başlangıç: Azure Portal kullanarak bir olay hub 'ı oluşturma](event-hubs-create.md)' daki yönergeleri izleyin. Bu hızlı başlangıçta kullanmak için ad alanı ve Olay Hub 'ı adlarını kaydedin.
- Event Hubs ad alanınız için paylaşılan erişim anahtarı adı ve birincil anahtar değeri. [Olay Hub 'ları bağlantı dizesi edinme](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)yönergelerini izleyerek erişim anahtarı adını ve değerini alın. Varsayılan erişim anahtarı adı *RootManageSharedAccessKey*' dir. Daha sonra bu hızlı başlangıçta kullanmak için anahtar adını ve birincil anahtar değerini kaydedin.
- PıP yüklü ve güncelleştirilmiş Python 2,7 veya 3,5 ya da üzeri.
- Event Hubs için Python paketi. 

    Paketi yüklemek için bu komutu, yolunda Python içeren bir komut isteminde çalıştırın:

    ```cmd
    pip install azure-eventhub
    ```

    Denetim noktası deposu olarak Azure Blob depolamayı kullanarak olayları almak için aşağıdaki paketi yükleyeceksiniz:

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```

## <a name="send-events"></a>Olayları gönderme
Bu bölümde, daha önce oluşturduğunuz Olay Hub 'ına olayları göndermek için bir Python betiği oluşturacaksınız.

1. [Visual Studio Code](https://code.visualstudio.com/)gibi en sevdiğiniz Python düzenleyicisini açın.
2. Adlı bir betik oluşturma *send.py*. Bu betik, daha önce oluşturduğunuz Olay Hub 'ına bir olay toplu işi gönderir.
3. Aşağıdaki kodu *Send.py*' ye yapıştırın:

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubProducerClient
    from azure.eventhub import EventData

    async def run():
        # Create a producer client to send messages to the event hub.
        # Specify a connection string to your event hubs namespace and
            # the event hub name.
        producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESPACE - CONNECTION STRING", eventhub_name="EVENT HUB NAME")
        async with producer:
            # Create a batch.
            event_data_batch = await producer.create_batch()

            # Add events to the batch.
            event_data_batch.add(EventData('First event '))
            event_data_batch.add(EventData('Second event'))
            event_data_batch.add(EventData('Third event'))

            # Send the batch of events to the event hub.
            await producer.send_batch(event_data_batch)

    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())

    ```

    > [!NOTE]
    > Bilgilendirici açıklamalar da dahil olmak üzere, tüm kaynak kodu için [GitHub send_async. Kopyala sayfasına](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py)gidin.

## <a name="receive-events"></a>Olayları alma
Bu hızlı başlangıç, bir denetim noktası deposu olarak Azure Blob depolamayı kullanır. Denetim noktası deposu, kontrol noktalarını kalıcı hale getirmek için kullanılır (yani, son okuma konumları).  

### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Azure depolama hesabı ve BLOB kapsayıcısı oluşturma
Aşağıdaki adımları uygulayarak bir Azure depolama hesabı ve içinde bir blob kapsayıcısı oluşturun:

1. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Bağlantı dizesini depolama hesabına al](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

Alma kodunda daha sonra kullanmak için bağlantı dizesini ve kapsayıcı adını kaydettiğinizden emin olun.


### <a name="create-a-python-script-to-receive-events"></a>Olaylarını almak için bir Python betiği oluşturma

Bu bölümde, Olay Hub 'ınızdan olayları almak için bir Python betiği oluşturursunuz:

1. [Visual Studio Code](https://code.visualstudio.com/)gibi en sevdiğiniz Python düzenleyicisini açın.
2. Adlı bir betik oluşturma *recv.py*.
3. Aşağıdaki kodu *recv.py*' ye yapıştırın:

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubConsumerClient
    from azure.eventhub.extensions.checkpointstoreblobaio import BlobCheckpointStore


    async def on_event(partition_context, event):
        # Print the event data.
        print("Received the event: \"{}\" from the partition with ID: \"{}\"".format(event.body_as_str(encoding='UTF-8'), partition_context.partition_id))

        # Update the checkpoint so that the program doesn't read the events
        # that it has already read when you run it next time.
        await partition_context.update_checkpoint(event)

    async def main():
        # Create an Azure blob checkpoint store to store the checkpoints.
        checkpoint_store = BlobCheckpointStore.from_connection_string("AZURE STORAGE CONNECTION STRING", "BLOB CONTAINER NAME")

        # Create a consumer client for the event hub.
        client = EventHubConsumerClient.from_connection_string("EVENT HUBS NAMESPACE CONNECTION STRING", consumer_group="$Default", eventhub_name="EVENT HUB NAME", checkpoint_store=checkpoint_store)
        async with client:
            # Call the receive method.
            await client.receive(on_event=on_event)

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # Run the main method.
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > Ek bilgilendirme açıklamaları dahil olmak üzere, tüm kaynak kodu için [GitHub recv_with_checkpoint_store_async. Kopyala sayfasına](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py)gidin.


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

Alıcı penceresi, Olay Hub 'ına gönderilen iletileri görüntülemelidir.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta olayları zaman uyumsuz olarak gönderdiniz ve aldınız. Olayları zaman uyumlu olarak gönderme ve alma hakkında bilgi edinmek için [GitHub sync_samples sayfasına](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples)gidin.

GitHub 'da tüm örnekler (hem zaman uyumlu hem de zaman uyumsuz) için, [Python örnekleri Için Azure Event Hubs istemci kitaplığı](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples)' na gidin.
