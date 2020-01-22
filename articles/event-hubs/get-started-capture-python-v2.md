---
title: Python uygulamasından Azure Event Hubs yakalanan verileri okuma | Microsoft Docs
description: Bu makalede, bir olay hub 'ına gönderilen verileri yakalamak ve bir Azure depolama alanından yakalanan olay verilerini okumak için Python kodu yazma yöntemi gösterilmektedir.
services: event-hubs
documentationcenter: ''
author: spelluru
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2019
ms.author: spelluru
ms.openlocfilehash: 43223f7cb9ed254340c99d235d494d1e93583c7f
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76293547"
---
# <a name="capture-event-hubs-data-in-azure-storage-and-read-it-using-python"></a>Azure depolama 'da Event Hubs verileri yakalayın ve Python kullanarak okuyun 
Bir olay hub 'ına gönderilen verilerin bir Azure depolama alanında veya Azure Data Lake Storage yakalanabilmesi için bir olay hub 'ı yapılandırma kullanabilirsiniz. Bu makalede, bir olay hub 'ına olayları göndermek ve Azure Blob depolamadan yakalanan verileri okumak için Python kodu yazma ' nın nasıl kullanılacağı gösterilmektedir. Bu özellik hakkında daha fazla bilgi için [Event Hubs yakalama özelliğine genel bakış](event-hubs-capture-overview.md)bölümüne bakın.

Bu örnekte [Azure Python SDK'sı](https://azure.microsoft.com/develop/python/) yakalama özelliği göstermek için. Sender.py programı, Event Hubs'a JSON biçiminde sanal ortam telemetri gönderir. Olay hub'ı, bu verileri Blob Depolama alanında toplu yazılacak yakalama özelliğini kullanmak için yapılandırılır. Capturereader.py uygulama, bu BLOB'ları okur ve cihaz başına bir ekleme dosyası oluşturur. Uygulama, ardından .csv dosyalarına veri yazar.

> [!IMPORTANT]
> Bu hızlı başlangıçta Azure Event Hubs Python SDK 'sının 5. sürümü kullanılmaktadır. Python SDK 'sının eski sürüm 1 ' i kullanan bir hızlı başlangıç için, [Bu makaleye](event-hubs-capture-python.md)bakın. SDK sürüm 1 ' i kullanıyorsanız, kodunuzu en son sürüme geçirmeniz önerilir. Ayrıntılar için bkz. [Geçiş Kılavuzu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).

Bu hızlı başlangıçta: 

> [!div class="checklist"]
> * Azure portal bir Azure Blob depolama hesabı ve kapsayıcısı oluşturun.
> * Azure portal kullanarak Event Hubs bir ad alanı oluşturun.
> * Yakalama özelliğini etkin bir olay hub 'ı oluşturun ve depolama hesabınıza bağlayın.
> * Bir Python betiği kullanarak Olay Hub 'ınıza veri gönderme.
> * Başka bir Python betiği kullanarak Event Hubs yakalamadan dosyaları okuyun ve işleyin.

## <a name="prerequisites"></a>Ön koşullar

- `pip` yüklenip güncelleştirildiği için Python 2,7 ve 3,5 ya da üzeri.
- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- [Ad alanında bir Event Hubs ad alanı ve bir olay hub 'ı oluşturun](event-hubs-create.md). Ad alanı adı, Olay Hub 'ı adı ve ad alanı için birincil erişim anahtarı ' na göz önünde Event Hubs. Makale: [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)makalesindeki yönergeleri izleyerek erişim anahtarını alın. Varsayılan anahtar adı: **RootManageSharedAccessKey**. Öğretici için bağlantı dizesine ihtiyacınız yoktur. Yalnızca birincil anahtara ihtiyacınız vardır. 
- Bir **Azure depolama hesabı** ve bir **BLOB kapsayıcısı**oluşturmak için aşağıdaki adımları izleyin:
    1. [Bir Azure depolama hesabı oluşturun](../storage/common/storage-quickstart-create-account.md?tabs=azure-portal).
    2. [Depolama alanında bir blob kapsayıcısı oluşturun](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container). 
    3. [Bağlantı dizesini depolama hesabına alın](../storage/common/storage-configure-connection-string.md#view-and-copy-a-connection-string).

        **Bağlantı dizesi** ve **kapsayıcı adı**' nı aklınızda edin. Bunları daha sonra bu kodda kullanacaksınız. 
- Aşağıdaki yönergeleri izleyerek Olay Hub 'ı için **yakalama** özelliğini etkinleştirin: [Azure Portal kullanarak Event Hubs yakalamayı etkinleştirin](event-hubs-capture-enable-through-portal.md). Önceki adımda oluşturduğunuz depolama hesabını ve BLOB kapsayıcısını seçin. Ayrıca, bir olay hub 'ı oluştururken özelliği etkinleştirebilirsiniz. 

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Olay hub'ınıza olayları göndermek için bir Python betiği oluşturma
Bu bölümde, bir olay hub 'ına 200 olayları (10 cihaz * 20 olay) gönderen bir Python betiği oluşturacaksınız. Bu olaylar, JSON biçiminde gönderilen örnek çevresel okuörnektir. 

1. [Visual Studio Code][Visual Studio Code]gibi en sevdiğiniz Python düzenleyicisini açın.
2. Adlı bir betik oluşturma **sender.py**. 
3. Aşağıdaki kodu sender.py ' ye yapıştırın. Ayrıntılar için bkz. kod açıklamaları. 
   
    ```python
    import time
    import os
    import uuid
    import datetime
    import random
    import json
    
    from azure.eventhub import EventHubProducerClient, EventData
    
    # this scripts simulates production of events for 10 devices
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    # create a producer client to produce/publish events to the event hub
    producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESAPCE CONNECTION STRING", eventhub_name="EVENT HUB NAME")
    
    for y in range(0,20):    # for each device, produce 20 events 
        event_data_batch = producer.create_batch() # create a batch. you will add events to the batch later. 
        for dev in devices:
            # create a dummy reading
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading) # convert reading into a JSON string
            event_data_batch.add(EventData(s)) # add event data to the batch
        producer.send_batch(event_data_batch) # send the batch of events to the event hub
    
    # close the producer    
    producer.close()
    ```
4. Betiklerdeki aşağıdaki değerleri değiştirin:
    1. `EVENT HUBS NAMESPACE CONNECTION STRING`, Event Hubs ad alanınız için bağlantı dizesiyle değiştirin.
    2. `EVENT HUB NAME`, Olay Hub 'ınızın adıyla değiştirin. 
5. Olayları Olay Hub 'ına göndermek için betiği çalıştırın. 
6. Azure portal, Olay Hub 'ının iletileri aldığını doğrulayabilirsiniz. **Ölçümler** bölümünde **iletiler** görünümüne geçin. Grafiği güncelleştirmek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir. 

    [![Olay Hub 'ının iletileri aldığını doğrulama](./media/get-started-capture-python-v2/messages-portal.png)](./media/get-started-capture-python-v2/messages-portal.png#lightbox)

## <a name="create-a-python-script-to-read-your-capture-files"></a>Yakalama dosyaları okumak için bir Python betiği oluşturma
Bu örnekte yakalanan veriler Azure Blob depolama alanında depolanır. Bu bölümdeki betik, Azure depolama ortamınızdaki yakalama verileri dosyalarını okur ve içeriği kolayca açıp görüntüleyebilmeniz için CSV dosyaları oluşturur. Uygulamanın geçerli çalışma dizininde 10 dosya görürsünüz. Bu dosyalar 10 cihaz için çevresel ayarları içerir. 

1. Python Düzenleyicinizde **capturereader.py**adlı bir komut dosyası oluşturun. Bu betik, yakalanan dosyalarını okur ve bu cihaz için yalnızca veri yazmak için cihaz başına bir dosya oluşturur.
2. Aşağıdaki kodu capturereader.py ' ye yapıştırın. Ayrıntılar için bkz. kod açıklamaları. 
   
    ```python
    import os
    import string
    import json
    import uuid
    import avro.schema
    
    from azure.storage.blob import ContainerClient, BlobClient
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    
    
    def processBlob2(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed_json = json.loads(reading["Body"])
            if not 'id' in parsed_json:
                return
            if not parsed_json['id'] in dict:
                list = []
                dict[parsed_json['id']] = list
            else:
                list = dict[parsed_json['id']]
                list.append(parsed_json)
        reader.close()
        for device in dict.keys():
            filename = os.getcwd() + '\\' + str(device) + '.csv'
            deviceFile = open(filename, "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
    
    def startProcessing():
        print('Processor started using path: ' + os.getcwd())
        # create a blob container client
        container = ContainerClient.from_connection_string("AZURE STORAGE CONNECTION STRING", container_name="BLOB CONTAINER NAME")
        blob_list = container.list_blobs() # list all the blobs in the container
        for blob in blob_list:
            #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)        
            if blob.size > 508:
                print('Downloaded a non empty blob: ' + blob.name)
                # create a blob client for the blob
                blob_client = ContainerClient.get_blob_client(container, blob=blob.name)
                # construct a file name based on the blob name
                cleanName = str.replace(blob.name, '/', '_')
                cleanName = os.getcwd() + '\\' + cleanName 
                with open(cleanName, "wb+") as my_file: # open the file to write. create if it doesn't exist. 
                    my_file.write(blob_client.download_blob().readall()) # write blob contents into the file
                processBlob2(cleanName) # convert the file into a CSV file
                os.remove(cleanName) # remove the original downloaded file
                # delete the blob from the container after it's read
                container.delete_blob(blob.name)
    
    startProcessing()    
    ```
4. `<AZURE STORAGE CONNECTION STRING>`, Azure depolama hesabınızın bağlantı dizesiyle değiştirin. Bu öğreticide oluşturduğunuz kapsayıcının adı: **yakala**' dır. Kapsayıcı için farklı bir ad kullandıysanız, `capture` değerini depolama hesabındaki kapsayıcının adıyla değiştirin. 

## <a name="run-the-scripts"></a>Betikleri çalıştırma
1. Python alt yolu olan bir komut istemi açın ve sonra Python önkoşul paketleri yüklemek için şu komutları çalıştırın:
   
   ```
   pip install azure-storage-blob
   pip install azure-eventhub
   pip install avro-python3
   ```
2. Yerde sender.py ve capturereader.py kaydettiğiniz için dizininize geçin ve şu komutu çalıştırın:
   
   ```
   python sender.py
   ```
   
   Bu komut, gönderen çalıştırmak için yeni bir Python işlemi başlatır.
3. Yakalama çalıştırmak birkaç dakika bekleyin. Ardından, özgün komut penceresine aşağıdaki komutu yazın:
   
   ```
   python capturereader.py
   ```

   Bu yakalama işlemci, depolama hesabı/kapsayıcısından tüm blobları indirmek için yerel bir dizin kullanır. Boş olmayan tüm işler ve sonuçları yerel bir dizine .csv dosyaları olarak yazar.

## <a name="next-steps"></a>Sonraki adımlar
GitHub 'daki Python örneklerine [buradan](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples)göz atın. 


[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
