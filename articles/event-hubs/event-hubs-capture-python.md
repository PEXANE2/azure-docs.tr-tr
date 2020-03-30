---
title: "Hızlı başlangıç: Python uygulamasından yakalanan verileri okuyun - Azure Etkinlik Hub'ları"
description: "Hızlı başlatma: Olay Hub'ları Yakalama özelliğini göstermek için Azure Python SDK'yı kullanan komut dosyaları."
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
editor: ''
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: seodec18
ms.date: 01/15/2020
ms.author: shvija
ms.openlocfilehash: 6c830cf871c2ae650bb61e8b3712a664e9e405d4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77187286"
---
# <a name="quickstart-event-hubs-capture-walkthrough-python-azure-eventhub-version-1"></a>Quickstart: Olay Hub'ları Yakalama izgeçidi: Python (azure-eventhub sürüm 1)

Yakalama, Azure Etkinlik Hub'larının bir özelliğidir. Etkinlik merkezinizdeki akış verilerini seçtiğiniz bir Azure Blob depolama hesabına otomatik olarak iletmek için Capture'i kullanabilirsiniz. Bu özellik, gerçek zamanlı akış verilerinde toplu iş işleme yapmayı kolaylaştırır. Bu makalede, Python ile Olay Hub'ları Yakalama'nın nasıl kullanılacağı açıklanmaktadır. Olay Hub'ları Yakalama hakkında daha fazla bilgi için Azure [Etkinlik Hub'ları aracılığıyla olayları yakalama][Overview of Event Hubs Capture]konusuna bakın.

Bu gözden geçirme, Yakalama özelliğini göstermek için [Azure Python SDK'yı](https://azure.microsoft.com/develop/python/) kullanır. *sender.py* programı JSON formatında Olay Hub'larına simüle edilmiş çevresel telemetri gönderir. Olay hub'ı, bu verileri toplu olarak Blob depolama alanına yazmak için Yakalama özelliğini kullanır. *capturereader.py* uygulaması bu lekeleri okur, her aygıtınız için bir ek dosyası oluşturur ve verileri her aygıttaki *.csv* dosyalarına yazar.

> [!WARNING]
> Bu hızlı başlangıç, Azure Etkinlik Hub'ları Python SDK'nın 1 sürümü içindir. Kodunuzu Python [migrate](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md) [SDK'nın 5.](get-started-capture-python-v2.md)

Bu gözden geçirmede, siz: 

> [!div class="checklist"]
> * Azure portalında bir Azure Blob depolama hesabı ve kapsayıcı oluşturun.
> * Olay Hub'larının Yakalanmasını etkinleştirin ve depolama hesabınıza yönlendirin.
> * Python komut dosyası kullanarak olay merkezinize veri gönderin.
> * Başka bir Python komut dosyası kullanarak Olay Hub'larını Yakalama'daki dosyaları okuyun ve işleyin.

## <a name="prerequisites"></a>Ön koşullar

- Python 3.4 veya `pip` daha sonra, yüklü ve güncelleştirilmiş.
  
- Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
  
- Quickstart'taki yönergeleri izleyerek oluşturulan etkin Olay Hub'ları ad alanı ve etkinlik hub'ı: [Azure portalını kullanarak bir etkinlik merkezi oluşturun.](event-hubs-create.md) Bu izbarada daha sonra kullanmak üzere ad alanınızı ve olay hub adlarınızı not edin. 
  
  > [!NOTE]
  > Zaten kullanmak için bir depolama kapsayıcınız varsa, Olay Hub'ını oluştururken Capture'i etkinleştirebilir ve depolama kapsayıcısını seçebilirsiniz. 
  > 
  
- Etkinlik Hub'larınız erişim anahtar adını ve birincil anahtar değerini paylaştı. Etkinlik **Hub'ları** sayfanızda Paylaşılan erişim ilkeleri altında bu değerleri bulun veya oluşturun. Varsayılan erişim anahtar adı **RootManageSharedAccessKey**olduğunu. Erişim anahtarı adını ve bu iznin daha sonra kullanmak üzere birincil anahtar değerini kopyalayın. 

## <a name="create-an-azure-blob-storage-account-and-container"></a>Azure Blob depolama hesabı ve kapsayıcı oluşturma

Yakalama için kullanılacak bir depolama hesabı ve kapsayıcı oluşturun. 

1. [Azure portalında][Azure portal]oturum açın.
2. Sol gezintide, **Depolama hesapları'nı**seçin ve **Depolama hesapları** ekranında **Ekle'yi**seçin.
3. Depolama hesabı oluşturma ekranında bir abonelik ve kaynak grubu seçin ve depolama hesabına bir ad verin. Diğer seçimleri varsayılan olarak bırakabilirsiniz. **Gözden Geçir + oluştur'** seçeneğini seçin, ayarları gözden geçirin ve sonra **Oluştur'u**seçin. 
   
   ![Depolama hesabı oluştur][1]
   
4. Dağıtım tamamlandığında **kaynağa git'i**ve depolama hesabına **Genel Bakış** ekranında **Kapsayıcılar'ı**seçin.
5. **Kapsayıcılar** ekranında **+ Kapsayıcı'yı**seçin. 
6. Yeni **kapsayıcı** ekranında, kapsayıcıya bir ad verin ve ardından **Tamam'ı**seçin. Daha sonra izbinde kullanmak üzere kapsayıcı adını not edin. 
7. **Kapsayıcılar** ekranının sol daki navigasyonunda **Erişim tuşlarını**seçin. Daha sonra **izbinde**kullanmak üzere Depolama hesap adını ve **Anahtar** değerini **key1**altında kopyalayın.
 
## <a name="enable-event-hubs-capture"></a>Olay Hub'larının Yakalanmasını Etkinleştir

1. Azure portalında, **Tüm kaynaklardan**Olay Hub'ları Ad Alanı'nı seçerek , sol gezintide **Olay hub'larını** seçerek ve ardından etkinlik hub'ınızı seçerek etkinlik merkezinize gidin. 
2. Etkinlik merkezine **Genel Bakış** **ekranında, olayları yakalayı'yı**seçin.
3. **Yakalama** ekranında, **A'yı**seçin. Ardından, **Azure Depolama Kapsayıcısı'nın**altında **Kapsayıcı'yı seçin.** 
4. **Kapsayıcılar** ekranında, kullanmak istediğiniz depolama kapsayıcısını seçin ve sonra **Seç'i**seçin. 
5. **Yakalama** ekranında **değişiklikleri kaydet'i**seçin. 

## <a name="create-a-python-script-to-send-events-to-event-hub"></a>Olay Hub'ına olay göndermek için Bir Python komut dosyası oluşturma
Bu komut dosyası, etkinlik merkezinize 200 olay gönderir. Olaylar JSON gönderilen basit çevresel okumalar vardır.

1. [Visual Studio Code][Visual Studio Code]gibi en sevdiğiniz Python düzenleyicisini açın.
2. *sender.py*adlı yeni bir dosya oluşturun. 
3. Aşağıdaki kodu *sender.py*yapıştırın. Kendi değerlerinizi \<Event Hub'ları ad \<alanı>, \<AccessKeyName>, \<birincil anahtar değeri> ve eventhub> yerine değiştirin.
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus.control_client import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='<namespace>', shared_access_key_name='<AccessKeyName>', shared_access_key_value='<primary key value>')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('<eventhub>', s)
       print(y)
   ```
4. Dosyayı kaydedin.

## <a name="create-a-python-script-to-read-capture-files"></a>Yakalama dosyalarını okumak için bir Python komut dosyası oluşturma

Bu komut dosyası yakalanan dosyaları okur ve her aygıtınızın yalnızca o aygıt için verileri yazması için bir dosya oluşturur.

1. Python düzenleyicinizde *capturereader.py*adında yeni bir dosya oluşturun. 
2. Aşağıdaki kodu *capturereader.py*yapıştırın. Kaydettiğiniz değerleri \<depolama hesabınız>, \<depolama hesabı erişim \<anahtarı> ve depolama kapsayıcı>.
   
   ```python
   import os
   import string
   import json
   import avro.schema
   from avro.datafile import DataFileReader, DataFileWriter
   from avro.io import DatumReader, DatumWriter
   from azure.storage.blob import BlockBlobService
   
   def processBlob(filename):
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
           deviceFile = open(device + '.csv', "a")
           for r in dict[device]:
               deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
   def startProcessing(accountName, key, container):
       print('Processor started using path: ' + os.getcwd())
       block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
       generator = block_blob_service.list_blobs(container)
       for blob in generator:
           #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
           if blob.properties.content_length > 508:
               print('Downloaded a non empty blob: ' + blob.name)
               cleanName = str.replace(blob.name, '/', '_')
               block_blob_service.get_blob_to_path(container, blob.name, cleanName)
               processBlob(cleanName)
               os.remove(cleanName)
           block_blob_service.delete_blob(container, blob.name)
   startProcessing('<storageaccount>', '<storage account access key>', '<storagecontainer>')
   ```

## <a name="run-the-python-scripts"></a>Python komut dosyalarını çalıştırma

1. Python'un yoluna çıkan bir komut istemini açın ve Python ön koşul paketlerini yüklemek için bu komutları çalıştırın:
   
   ```cmd
   pip install azure-storage
   pip install azure-servicebus
   pip install avro-python3
   ```
   
   Daha önceki bir sürümünüz varsa `azure-storage` veya `azure`bu `--upgrade` seçeneği kullanmanız gerekebilir.
   
   Ayrıca aşağıdaki komutu çalıştırmanız gerekebilir. Bu komutu çalıştırmak çoğu sistemde gerekli değildir. 
   
   ```cmd
   pip install cryptography
   ```
   
2. *sender.py* kaydettiğiniz ve *capturereader.py*bu komutu çalıştırdığınız dizinden:
   
   ```cmd
   start python sender.py
   ```
   
   Komut göndereni çalıştırmak için yeni bir Python işlemi başlatır.
   
3. Yakalama çalışması bittiğinde, şu komutu çalıştırın:
   
   ```cmd
   python capturereader.py
   ```

   Yakalama işlemcisi, depolama hesabı kapsayıcısından boş olmayan tüm lekeleri indirir ve sonuçları yerel dizine *.csv* dosyaları olarak yazar. 

## <a name="next-steps"></a>Sonraki adımlar

Etkinlik Hub'ları hakkında daha fazla bilgi edinmek için bkz: 

* [Olay Hub'larının Yakalanmasına Genel Bakış][Overview of Event Hubs Capture]
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Olay Hub'larına genel bakış][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
