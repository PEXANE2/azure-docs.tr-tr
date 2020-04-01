---
title: Python kullanarak Azure Etkinlik Hub'larından etkinlik gönderme veya alma (en son)
description: Bu makalede, en son azure-eventhub sürüm 5 paketini kullanarak Azure Event Hub'larına/azure Olay Hub'larına etkinlik gönderen/alan bir Python uygulaması oluşturmak için bir iz bilgililik sağlanmaktadır.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 352ff91bf26c7ff4f6945431fe6e1357f030e1db
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80477522"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-python-azure-eventhub-version-5"></a>Python kullanarak olay merkezlerine olay hub'larına olay gönderme veya alma (azure-eventhub sürüm 5)
Bu hızlı başlangıç, **azure-eventhub sürüm 5** Python paketini kullanarak bir olay hub'ına olayları nasıl göndereceğinizi ve bir olay merkezinden nasıl alınarak alınabildiğini gösterir.

> [!IMPORTANT]
> Bu quickstart en son azure-eventhub sürüm 5 paketini kullanır. Eski azure-eventhub sürüm 1 paketini kullanan hızlı bir başlangıç için, [azure-eventhub sürüm 1'i kullanarak olayları Gönder ve al'](event-hubs-python-get-started-send.md)a bakın. 

## <a name="prerequisites"></a>Ön koşullar
Azure Etkinlik Hub'larında yeniyseniz, bu hızlı başlangıcı yapmadan önce [Etkinlik Hub'larına genel bakış](event-hubs-about.md) konusubakın. 

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- **Microsoft Azure aboneliği.** Azure Etkinlik Hub'ları da dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğe ihtiyacınız vardır.  Varolan bir Azure hesabınız yoksa, [ücretsiz](https://azure.microsoft.com/free/) deneme sürümüne kaydolabilir veya [bir hesap oluştururken](https://azure.microsoft.com)MSDN abone avantajlarınızı kullanabilirsiniz.
- Python 2.7 veya 3.5 veya daha sonra, PIP yüklü ve güncelleştirildi.
- Olay Hub'ları için Python paketi. 

    Paketi yüklemek için, bu komutu Python'un yoluna çıkan bir komut isteminde çalıştırın:

    ```cmd
    pip install azure-eventhub
    ```

    Denetim noktası deposu olarak Azure Blob depolama sını kullanarak olayları almak için aşağıdaki paketi yükleyin:

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** İlk adım, Olay Hub türünden bir ad alanı oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanmaktır. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek **Olay Hub'ları ad alanının bağlantı dizesini** alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bağlantı dizesini daha sonra bu hızlı başlatmada kullanırsınız.

## <a name="send-events"></a>Olayları gönderme
Bu bölümde, olayları daha önce oluşturduğunuz olay merkezine göndermek için bir Python komut dosyası oluşturursunuz.

1. [Visual Studio Code](https://code.visualstudio.com/)gibi en sevdiğiniz Python düzenleyicisini açın.
2. *send.py*adlı bir komut dosyası oluşturun. Bu komut dosyası, daha önce oluşturduğunuz olay hub'ına bir dizi olay gönderir.
3. Aşağıdaki kodu *send.py*yapıştırın:

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
    > Bilgilendirme yorumları da dahil olmak üzere tam kaynak kodu için [GitHub send_async.py sayfasına](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py)gidin.
    

## <a name="receive-events"></a>Olayları alma
Bu hızlı başlatma, Azure Blob depolama noktasını denetim noktası deposu olarak kullanır. Denetim noktası deposu denetim noktalarını (diğer bir şekilde son okuma konumlarını) sürdürmek için kullanılır.  

> [!NOTE]
> Azure Stack Hub'da çalışıyorsanız, bu platform Depolama Blob SDK'nın Azure'da bulunanlardan farklı bir sürümünü destekleyebilir. Örneğin, Azure Yığını [Hub sürümü 2002'de](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)çalışıyorsanız, Depolama hizmeti için kullanılabilir en yüksek sürüm 2017-11-09 sürümüdür. Bu durumda, bu bölümdeki aşağıdaki adımların yanı sıra, Depolama hizmeti API sürümünü 2017-11-09'u hedeflemek için kod eklemeniz gerekir. Belirli bir Depolama API sürümünü niçin hedefleneceksiniz hakkında bir örnek için, GitHub'daki [senkron](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/receive_events_using_checkpoint_store_storage_api_version.py) ve eşzamanlı örneklere bakın. [asynchronous](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/receive_events_using_checkpoint_store_storage_api_version_async.py) Azure Yığını Hub'ında desteklenen Azure Depolama hizmeti sürümleri hakkında daha fazla bilgi için lütfen [Azure Yığın Hub depolama alanına bakın: Farklılıklar ve dikkat edilmesi gerekenler.](https://docs.microsoft.com/azure-stack/user/azure-stack-acs-differences)


### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Azure depolama hesabı ve blob kapsayıcısı oluşturma
Aşağıdaki adımları yaparak bir Azure depolama hesabı ve bir blob kapsayıcısı oluşturun:

1. [Azure Depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Bağlantı dizesini depolama hesabına alma](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

Daha sonra kullanım için bağlantı dizesini ve kapsayıcı adını alma koduna kaydettiğinden emin olun.


### <a name="create-a-python-script-to-receive-events"></a>Olayları almak için Python komut dosyası oluşturma

Bu bölümde, olay merkezinizden olayları almak için bir Python komut dosyası oluşturursunuz:

1. [Visual Studio Code](https://code.visualstudio.com/)gibi en sevdiğiniz Python düzenleyicisini açın.
2. *recv.py*adlı bir komut dosyası oluşturun.
3. Aşağıdaki kodu *recv.py*yapıştırın:

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
    > Ek bilgi açıklamaları da dahil olmak üzere tam kaynak kodu için [GitHub recv_with_checkpoint_store_async.py sayfasına](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py)gidin.


### <a name="run-the-receiver-app"></a>Alıcı uygulamasını çalıştırma

Komut dosyasını çalıştırmak için, Python'un yoluna çıkan bir komut istemini açın ve ardından şu komutu çalıştırın:

```bash
python recv.py
```

### <a name="run-the-sender-app"></a>Gönderen uygulamasını çalıştırma

Komut dosyasını çalıştırmak için, Python'un yoluna çıkan bir komut istemini açın ve ardından şu komutu çalıştırın:

```bash
python send.py
```

Alıcı penceresi olay hub'ına gönderilen iletileri görüntülemelidir.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, olayları eşit bir şekilde gönderdiniz ve aldınız. Olayları eşzamanlı olarak nasıl göndereceğini ve alacağınızı öğrenmek için [GitHub sync_samples sayfasına](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples)gidin.

GitHub'daki tüm örnekler (hem senkron hem de eşzamanlı) [için Python örnekleri için Azure Event Hub istemci kitaplığına](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples)gidin.
