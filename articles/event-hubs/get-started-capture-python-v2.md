---
title: Azure Etkinlik Hub'larını oku Python uygulamasından yakalanan verileri (en son)
description: Bu makalede, bir etkinlik merkezine gönderilen verileri yakalamak ve bir Azure depolama hesabından yakalanan olay verilerini okumak için Python kodunun nasıl yazılabildiğiniz gösterilmektedir.
services: event-hubs
documentationcenter: ''
author: spelluru
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: 34583ef49b2f919391af3fe5700a558b2dc40700
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77187235"
---
# <a name="capture-event-hubs-data-in-azure-storage-and-read-it-by-using-python-azure-eventhub-version-5"></a>Azure Depolama'da Olay Hub'ları verilerini yakalayın ve Python kullanarak okuyun (azure-eventhub sürüm 5)

Bir olay hub'ına gönderilen verilerin bir Azure depolama hesabında veya Azure Veri Gölü Depolama Gen 1 veya Gen 2'de yakalanması için bir olay hub'ı yapılandırabilirsiniz. Bu makalede, olayları bir olay merkezine göndermek ve **Azure Blob depolamadan**yakalanan verileri okumak için Python kodu nasıl yazabileceğinizi gösterilmektedir. Bu özellik hakkında daha fazla bilgi için [Olay Hub'ları Yakalama özelliğine genel bakış](event-hubs-capture-overview.md)alameti bakın.

Bu hızlı başlatma, Yakalama özelliğini göstermek için [Azure Python SDK'yı](https://azure.microsoft.com/develop/python/) kullanır. *sender.py* uygulaması JSON formatındaki etkinlik merkezlerine simüle edilmiş çevresel telemetri gönderir. Olay hub'ı, bu verileri toplu olarak Blob depolamaalanına yazmak için Yakalama özelliğini kullanacak şekilde yapılandırılır. *capturereader.py* uygulaması bu lekeleri okur ve her aygıt için bir ek dosyası oluşturur. Uygulama daha sonra verileri CSV dosyalarına yazar.

> [!IMPORTANT]
> Bu hızlı başlangıç, Azure Etkinlik Hub'ları Python SDK'nın 5 sürümünü kullanır. Python SDK sürümü 1 kullanan bir quickstart için [bu makaleye](event-hubs-capture-python.md)bakın. 

Bu hızlı başlangıçta: 

> [!div class="checklist"]
> * Azure portalında bir Azure Blob depolama hesabı ve kapsayıcı oluşturun.
> * Azure portalını kullanarak Bir Etkinlik Hub'ları ad alanı oluşturun.
> * Yakalama özelliği etkinleştirilmiş bir etkinlik hub'ı oluşturun ve depolama hesabınıza bağlayın.
> * Python komut dosyası kullanarak olay merkezinize veri gönderin.
> * Başka bir Python komut dosyası kullanarak Olay Hub'larını Yakalama'daki dosyaları okuyun ve işleyin.

## <a name="prerequisites"></a>Ön koşullar

- Python 2.7 ve 3.5 veya sonraki, PIP yüklü ve güncelleştirildi.  
- Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)  
- Etkin Olay Hub'ları ad alanı ve olay hub'ı.
[Ad alanında olay hub'ları ad alanı ve olay merkezi oluşturun.](event-hubs-create.md) Olay Hub'larının adını, olay hub'ının adını ve ad alanının birincil erişim anahtarını kaydedin. Erişim anahtarını almak için olay [hub'ları bağlantı dizesi alın'a](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)bakın. Varsayılan anahtar adı *RootManageSharedAccessKey*olduğunu. Bu hızlı başlangıç için yalnızca birincil anahtara ihtiyacınız var. Bağlantı dizesini gerek yok.  
- Azure depolama hesabı, depolama hesabında bir blob kapsayıcısı ve depolama hesabına bağlantı dizesi. Bu öğeler emzemezseniz, aşağıdakileri yapın:  
    1. [Azure depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md?tabs=azure-portal)  
    1. [Depolama hesabında bir blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)  
    1. [Bağlantı dizesini depolama hesabına alma](../storage/common/storage-configure-connection-string.md#view-and-copy-a-connection-string)

    Bu hızlı başlatmada daha sonra kullanmak üzere bağlantı dizesini ve kapsayıcı adını kaydettiğinden emin olun.  
- Olay hub'ı için Yakalama özelliğini etkinleştirin. Bunu yapmak için, [Azure portalını kullanarak Etkinlik Hub'larını Yakalamayı Etkinleştir'deki](event-hubs-capture-enable-through-portal.md)yönergeleri izleyin. Önceki adımda oluşturduğunuz depolama hesabını ve blob kapsayıcısını seçin. Bir olay hub'ı oluşturduğunuzda özelliği de etkinleştirebilirsiniz.  

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Olay merkezinize olay göndermek için bir Python komut dosyası oluşturma
Bu bölümde, bir olay hub'ına 200 olay (10 aygıt * 20 olay) gönderen bir Python komut dosyası oluşturursunuz. Bu olaylar JSON formatında gönderilen örnek bir çevre okumavardır. 

1. [Visual Studio Code][Visual Studio Code]gibi en sevdiğiniz Python düzenleyicisini açın.
2. *sender.py*adlı bir komut dosyası oluşturun. 
3. Aşağıdaki kodu *sender.py*yapıştırın. 
   
    ```python
    import time
    import os
    import uuid
    import datetime
    import random
    import json
    
    from azure.eventhub import EventHubProducerClient, EventData
    
    # This script simulates the production of events for 10 devices.
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    # Create a producer client to produce and publish events to the event hub.
    producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESAPCE CONNECTION STRING", eventhub_name="EVENT HUB NAME")
    
    for y in range(0,20):    # For each device, produce 20 events. 
        event_data_batch = producer.create_batch() # Create a batch. You will add events to the batch later. 
        for dev in devices:
            # Create a dummy reading.
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading) # Convert the reading into a JSON string.
            event_data_batch.add(EventData(s)) # Add event data to the batch.
        producer.send_batch(event_data_batch) # Send the batch of events to the event hub.
    
    # Close the producer.    
    producer.close()
    ```
4. Komut dosyalarında aşağıdaki değerleri değiştirin:  
    * Olay `EVENT HUBS NAMESPACE CONNECTION STRING` Hub'larınız ad alanınız için bağlantı dizesiyle değiştirin.  
    * Etkinlik `EVENT HUB NAME` hub'ınızın adıyla değiştirin.  
5. Olayları olay merkezine göndermek için komut dosyasını çalıştırın.  
6. Azure portalında, olay merkezinin iletileri aldığını doğrulayabilirsiniz. **Ölçümler** bölümünde **İletiler** görünümüne geçin. Grafiği güncellemek için sayfayı yenileyin. Sayfanın iletilerin alındığını görüntülemesi birkaç saniye sürebilir. 

    [![Olay merkezinin iletileri aldığını doğrulama](./media/get-started-capture-python-v2/messages-portal.png)](./media/get-started-capture-python-v2/messages-portal.png#lightbox)

## <a name="create-a-python-script-to-read-your-capture-files"></a>Yakalama dosyalarınızı okumak için bir Python komut dosyası oluşturma
Bu örnekte, yakalanan veriler Azure Blob depolama alanında depolanır. Bu bölümdeki komut dosyası, Azure depolama hesabınızdan yakalanan veri dosyalarını okur ve kolayca açıp görüntülemeniz için CSV dosyaları oluşturur. Uygulamanın geçerli çalışma dizininde 10 dosya görürsünüz. Bu dosyalar 10 cihazın çevresel okumalarını içerecektir. 

1. Python düzenleyicinizde *capturereader.py*adlı bir komut dosyası oluşturun. Bu komut dosyası yakalanan dosyaları okur ve her aygıtın yalnızca o aygıt için verileri yazması için bir dosya oluşturur.
2. Aşağıdaki kodu *capturereader.py*yapıştırın. 
   
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
        # Create a blob container client.
        container = ContainerClient.from_connection_string("AZURE STORAGE CONNECTION STRING", container_name="BLOB CONTAINER NAME")
        blob_list = container.list_blobs() # List all the blobs in the container.
        for blob in blob_list:
            # Content_length == 508 is an empty file, so process only content_length > 508 (skip empty files).        
            if blob.size > 508:
                print('Downloaded a non empty blob: ' + blob.name)
                # Create a blob client for the blob.
                blob_client = ContainerClient.get_blob_client(container, blob=blob.name)
                # Construct a file name based on the blob name.
                cleanName = str.replace(blob.name, '/', '_')
                cleanName = os.getcwd() + '\\' + cleanName 
                with open(cleanName, "wb+") as my_file: # Open the file to write. Create it if it doesn't exist. 
                    my_file.write(blob_client.download_blob().readall()) # Write blob contents into the file.
                processBlob2(cleanName) # Convert the file into a CSV file.
                os.remove(cleanName) # Remove the original downloaded file.
                # Delete the blob from the container after it's read.
                container.delete_blob(blob.name)
    
    startProcessing()    
    ```
3. Azure `AZURE STORAGE CONNECTION STRING` depolama hesabınız için bağlantı dizesiyle değiştirin. Bu hızlı başlatmada oluşturduğunuz kapsayıcının adı *yakalamadır.* Kapsayıcı için farklı bir ad kullandıysanız, depolama hesabındaki kapsayıcının adıyla *yakalama'yı* değiştirin. 

## <a name="run-the-scripts"></a>Komut dosyalarını çalıştırma
1. Python'un yoluna çıkan bir komut istemini açın ve Python ön koşul paketlerini yüklemek için bu komutları çalıştırın:
   
   ```
   pip install azure-storage-blob
   pip install azure-eventhub
   pip install avro-python3
   ```
2. Dizininizi, *sender.py* kaydettiğiniz ve *capturereader.py*kaydettiğiniz dizinle değiştirin ve bu komutu çalıştırın:
   
   ```
   python sender.py
   ```
   
   Bu komut göndereni çalıştırmak için yeni bir Python işlemi başlatır.
3. Yakalamanın çalışması için birkaç dakika bekleyin ve ardından özgün komut pencerenizde aşağıdaki komutu girin:
   
   ```
   python capturereader.py
   ```

   Bu yakalama işlemcisi, depolama hesabından ve kapsayıcıdan tüm lekeleri indirmek için yerel dizini kullanır. Boş olmayan ları işler ve sonuçları CSV dosyaları olarak yerel dizine yazar.

## <a name="next-steps"></a>Sonraki adımlar
[GitHub'daki Python örneklerine](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples)göz atın. 


[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
