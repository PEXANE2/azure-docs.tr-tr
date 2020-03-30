---
title: Python kullanarak Azure Etkinlik Hub'larından etkinlik gönderme veya alma (eski)
description: Bu walkthrough, eski azure-eventhub sürüm 1 paketini kullanarak Azure Olay Hub'larına etkinlik gönderen veya alan Python komut dosyalarını nasıl oluşturup çalıştırabileceğinizi gösterir.
services: event-hubs
author: spelluru
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: 22f6b2aba36e560e9bd335baa92925fe9846c670
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77162608"
---
# <a name="quickstart-send-and-receive-events-with-event-hubs-using-python-azure-eventhub-version-1"></a>Hızlı başlatma: Python kullanarak Olay Hub'ları ile etkinlik gönderme ve alma (azure-eventhub sürüm 1)
Bu hızlı başlangıç, **azure-eventhub sürüm 1** Python paketini kullanarak bir olay hub'ına olayları nasıl göndereceğinizi ve bir olay merkezinden nasıl alınarak alınabildiğini gösterir. 

> [!WARNING]
> Bu quickstart eski azure-eventhub sürüm 1 paketini kullanır. Paketin en son sürüm **5'ini** kullanan hızlı bir başlangıç için [azure-eventhub sürüm 5'i kullanarak etkinlik gönder ve al'](get-started-python-send-v2.md)bakın. Uygulamanızı eski paketi kullanmaktan yenisine taşımak [için azure-eventhub sürüm 1'den sürüm 5'e geçiş kılavuzuna](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md)bakın.
 

## <a name="prerequisites"></a>Ön koşullar
Azure Etkinlik Hub'larında yeniyseniz, bu hızlı başlangıcı yapmadan önce [Etkinlik Hub'larına genel bakış](event-hubs-about.md) bakın. 

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- **Microsoft Azure aboneliği.** Azure Etkinlik Hub'ları da dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğe ihtiyacınız vardır.  Varolan bir Azure hesabınız yoksa, [ücretsiz](https://azure.microsoft.com/free/) deneme sürümüne kaydolabilir veya [bir hesap oluştururken](https://azure.microsoft.com)MSDN abone avantajlarınızı kullanabilirsiniz.
- Python 3.4 veya `pip` daha sonra, yüklü ve güncelleştirilmiş.
- Olay Hub'ları için Python paketi. Paketi yüklemek için, bu komutu Python'un yoluna çıkan bir komut isteminde çalıştırın: 
  
  ```cmd
  pip install azure-eventhub==1.3.*
  ```
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** İlk adım, Olay Hub türünden bir ad alanı oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanmaktır. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek olay hub'ı için erişim anahtarının değerini alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bu hızlı başlatmada daha sonra yazdığınız koddaki erişim anahtarını kullanırsınız. Varsayılan anahtar adı: **RootManageSharedAccessKey**. 


## <a name="send-events"></a>Olayları gönderme

Olayları bir olay hub'ına gönderen bir Python uygulaması oluşturmak için:

> [!NOTE]
> Hızlı başlangıç ile çalışmak yerine, [örnek uygulamaları](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) GitHub'dan indirebilir ve çalıştırabilirsiniz. `EventHubConnectionString` Ve `EventHubName` dizeleri olay hub değerlerinizle değiştirin.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz Python düzenleyicisini açın
2. *send.py*adlı yeni bir dosya oluşturun. Bu komut dosyası, etkinlik merkezinize 100 olay gönderir.
3. Event Hub'ları \<ad alanı>, eventhub>, \< \<AccessKeyName> ve \<birincil anahtar değeri> değerleriniz yerine aşağıdaki kodu *send.py*yapıştırın: 
   
   ```python
   import sys
   import logging
   import datetime
   import time
   import os
   
   from azure.eventhub import EventHubClient, Sender, EventData
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<namespace>.servicebus.windows.net/eventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   try:
       if not ADDRESS:
           raise ValueError("No EventHubs URL supplied.")
   
       # Create Event Hubs client
       client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
       sender = client.add_sender(partition="0")
       client.run()
       try:
           start_time = time.time()
           for i in range(100):
               print("Sending message: {}".format(i))
               message = "Message {}".format(i)
               sender.send(EventData(message))
       except:
           raise
       finally:
           end_time = time.time()
           client.stop()
           run_time = end_time - start_time
           logger.info("Runtime: {} seconds".format(run_time))
   
   except KeyboardInterrupt:
       pass
   ```
   
4. Dosyayı kaydedin. 

Komut dosyasını çalıştırmak için, *send.py*kaydettiğiniz dizinden bu komutu çalıştırın:

```cmd
start python send.py
```

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="receive-events"></a>Olayları alma

Olay hub'ından olayları alan bir Python uygulaması oluşturmak için:

1. Python düzenleyicinizde *recv.py*adlı bir dosya oluşturun.
2. Event Hub'ları \<ad \<alanı>, eventhub>, \<AccessKeyName> ve \<birincil anahtar değerini değerlerinizle> yerine aşağıdaki kodu *recv.py*yapıştırın: 
   
   ```python
   import os
   import sys
   import logging
   import time
   from azure.eventhub import EventHubClient, Receiver, Offset
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   
   CONSUMER_GROUP = "$default"
   OFFSET = Offset("-1")
   PARTITION = "0"
   
   total = 0
   last_sn = -1
   last_offset = "-1"
   client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
   try:
       receiver = client.add_receiver(
           CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
       client.run()
       start_time = time.time()
       for event_data in receiver.receive(timeout=100):
           print("Received: {}".format(event_data.body_as_str(encoding='UTF-8')))
           total += 1
   
       end_time = time.time()
       client.stop()
       run_time = end_time - start_time
       print("Received {} messages in {} seconds".format(total, run_time))
   
   except KeyboardInterrupt:
       pass
   finally:
       client.stop()
   ```
   
4. Dosyayı kaydedin.

Komut dosyasını çalıştırmak için, *recv.py*kaydettiğiniz dizinden bu komutu çalıştırın:

```cmd
start python recv.py
```

## <a name="next-steps"></a>Sonraki adımlar
Olay Hub'ları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Azure Event Hubs'ın özellikleri ve terminolojisi](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)

