---
title: Python kullanarak Azure Event Hubs olay gönderme veya alma (en son)
description: Bu makale, Azure Event Hubs en son Azure-eventhub sürüm 5 paketini kullanarak olayları gönderen/alan bir Python uygulaması oluşturmaya yönelik bir yol sağlar.
ms.topic: quickstart
ms.date: 02/11/2020
ms.openlocfilehash: 2203b76d22f2a90e89d6e2c4a849b2c8016df89a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86521845"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-python-azure-eventhub-version-5"></a>Python kullanarak Olay Hub 'larına olay gönderme veya olayları alma (Azure-eventhub sürüm 5)
Bu hızlı başlangıçta, **Azure-eventhub sürüm 5** Python paketini kullanarak Olay Hub 'ından olayları gönderme ve olayları alma işlemlerinin nasıl yapılacağı gösterilmektedir.

> [!IMPORTANT]
> Bu hızlı başlangıç, en son Azure-eventhub sürüm 5 paketini kullanır. Eski Azure-eventhub sürüm 1 paketini kullanan hızlı bir başlangıç için bkz. [Azure-eventhub sürüm 1 kullanarak olay gönderme ve alma](event-hubs-python-get-started-send.md). 

## <a name="prerequisites"></a>Önkoşullar
Azure Event Hubs yeni başladıysanız, bu hızlı başlangıcı uygulamadan önce [Event Hubs genel bakış](event-hubs-about.md) bölümüne bakın. 

Bu hızlı başlangıcı tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- **Microsoft Azure aboneliği**. Azure Event Hubs dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğiniz olması gerekir.  Mevcut bir Azure hesabınız yoksa, [ücretsiz deneme](https://azure.microsoft.com/free/) için kaydolabilir veya [BIR hesap oluştururken](https://azure.microsoft.com)MSDN abonesi avantajlarınızı kullanabilirsiniz.
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
- **Event Hubs bir ad alanı ve bir olay hub 'ı oluşturun**. İlk adım, Event Hubs türünde bir ad alanı oluşturmak için [Azure Portal](https://portal.azure.com) ve uygulamanızın Olay Hub 'ı ile iletişim kurması için gereken yönetim kimlik bilgilerini elde etmek için kullanılır. Bir ad alanı ve Olay Hub 'ı oluşturmak için [Bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makalenin yönergelerini izleyerek **Event Hubs ad alanı için bağlantı dizesini** alın: [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bağlantı dizesini daha sonra bu hızlı başlangıçta kullanacaksınız.

## <a name="send-events"></a>Olayları gönderme
Bu bölümde, daha önce oluşturduğunuz Olay Hub 'ına olayları göndermek için bir Python betiği oluşturacaksınız.

1. [Visual Studio Code](https://code.visualstudio.com/)gibi en sevdiğiniz Python düzenleyicisini açın.
2. *Send.py*adlı bir komut dosyası oluşturun. Bu betik, daha önce oluşturduğunuz Olay Hub 'ına bir olay toplu işi gönderir.
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

> [!NOTE]
> Azure Stack Hub üzerinde çalıştırıyorsanız, bu platform Azure 'da genel kullanıma sunulan farklı bir Depolama Blobu SDK sürümü destekleyebilir. Örneğin, [Azure Stack hub sürümü 2002 üzerinde](/azure-stack/user/event-hubs-overview)çalıştırıyorsanız, depolama hizmeti için en yüksek sürüm 2017-11-09 ' dir. Bu durumda, bu bölümdeki adımların yanı sıra Storage Service API sürüm 2017-11-09 ' i hedeflemek için de kod eklemeniz gerekecektir. Belirli bir depolama API sürümünün nasıl hedeflenecek hakkında bir örnek için, GitHub 'da [zaman uyumlu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/receive_events_using_checkpoint_store_storage_api_version.py) ve [zaman uyumsuz](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/receive_events_using_checkpoint_store_storage_api_version_async.py) örneklere bakın. Azure Stack hub 'ında desteklenen Azure depolama hizmeti sürümleri hakkında daha fazla bilgi için lütfen [Azure Stack hub depolama: farklar ve konular](/azure-stack/user/azure-stack-acs-differences)bölümüne bakın.


### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Azure depolama hesabı ve BLOB kapsayıcısı oluşturma
Aşağıdaki adımları uygulayarak bir Azure depolama hesabı ve içinde bir blob kapsayıcısı oluşturun:

1. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Bağlantı dizesini depolama hesabına al](../storage/common/storage-configure-connection-string.md)

Alma kodunda daha sonra kullanmak için bağlantı dizesini ve kapsayıcı adını kaydettiğinizden emin olun.


### <a name="create-a-python-script-to-receive-events"></a>Olayları almak için bir Python betiği oluşturma

Bu bölümde, Olay Hub 'ınızdan olayları almak için bir Python betiği oluşturursunuz:

1. [Visual Studio Code](https://code.visualstudio.com/)gibi en sevdiğiniz Python düzenleyicisini açın.
2. *Recv.py*adlı bir komut dosyası oluşturun.
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
            # Call the receive method. Read from the beginning of the partition (starting_position: "-1")
            await client.receive(on_event=on_event,  starting_position="-1")

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # Run the main method.
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > Ek bilgilendirme açıklamaları dahil olmak üzere, tüm kaynak kodu için [GitHub recv_with_checkpoint_store_async. Kopyala sayfasına](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py)gidin.


### <a name="run-the-receiver-app"></a>Alıcı uygulamasını çalıştırma

Betiği çalıştırmak için, yolunda Python 'a sahip bir komut istemi açın ve ardından şu komutu çalıştırın:

```bash
python recv.py
```

### <a name="run-the-sender-app"></a>Gönderen uygulamasını çalıştırma

Betiği çalıştırmak için, yolunda Python 'a sahip bir komut istemi açın ve ardından şu komutu çalıştırın:

```bash
python send.py
```

Alıcı penceresi, Olay Hub 'ına gönderilen iletileri görüntülemelidir.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta olayları zaman uyumsuz olarak gönderdiniz ve aldınız. Olayları zaman uyumlu olarak gönderme ve alma hakkında bilgi edinmek için [GitHub sync_samples sayfasına](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples)gidin.

GitHub 'da tüm örnekler (hem zaman uyumlu hem de zaman uyumsuz) için, [Python örnekleri Için Azure Event Hubs istemci kitaplığı](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples)' na gidin.
