---
title: Python (eski) kullanarak Azure Event Hubs olay gönderme veya alma
description: Bu izlenecek yol, Azure Event Hubs eski Azure-eventhub sürüm 1 paketini kullanarak olayları gönderen veya olayları alan Python betikleri oluşturmayı ve çalıştırmayı gösterir.
services: event-hubs
author: spelluru
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: 22f6b2aba36e560e9bd335baa92925fe9846c670
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162608"
---
# <a name="quickstart-send-and-receive-events-with-event-hubs-using-python-azure-eventhub-version-1"></a>Hızlı başlangıç: Python kullanarak Event Hubs olay gönderme ve alma (Azure-eventhub sürüm 1)
Bu hızlı başlangıçta, **Azure-eventhub sürüm 1** Python paketini kullanarak Olay Hub 'ından olayları gönderme ve olayları alma işlemlerinin nasıl yapılacağı gösterilmektedir. 

> [!WARNING]
> Bu hızlı başlangıç, eski Azure-eventhub sürüm 1 paketini kullanır. Paketin en son **sürüm 5** ' i kullanan bir hızlı başlangıç için bkz. [Azure-eventhub sürüm 5 kullanarak olay gönderme ve alma](get-started-python-send-v2.md). Uygulamanızı eski paketi kullanarak yeni bir sürüme taşımak için [Azure-eventhub sürüm 1 ' den sürüm 5 ' e geçiş kılavuzuna](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md)bakın.
 

## <a name="prerequisites"></a>Önkoşullar
Azure Event Hubs 'yi yeni kullanıyorsanız, bu hızlı başlangıcı uygulamadan önce [Event Hubs genel bakış](event-hubs-about.md) bölümüne bakın. 

Bu hızlı başlangıcı tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- **Microsoft Azure aboneliği**. Azure Event Hubs dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğiniz olması gerekir.  Mevcut bir Azure hesabınız yoksa, [ücretsiz deneme](https://azure.microsoft.com/free/) için kaydolabilir veya [BIR hesap oluştururken](https://azure.microsoft.com)MSDN abonesi avantajlarınızı kullanabilirsiniz.
- `pip` yüklenip güncelleştirildiğinden Python 3,4 veya sonraki bir sürümü.
- Event Hubs için Python paketi. Paketi yüklemek için bu komutu, yolunda Python içeren bir komut isteminde çalıştırın: 
  
  ```cmd
  pip install azure-eventhub==1.3.*
  ```
- **Event Hubs bir ad alanı ve bir olay hub 'ı oluşturun**. İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve Olay Hub 'ı oluşturmak için [Bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek Olay Hub 'ı için erişim anahtarı değerini alın: [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu hızlı başlangıçta yazdığınız kodda erişim anahtarını kullanın. Varsayılan anahtar adı: **RootManageSharedAccessKey**. 


## <a name="send-events"></a>Olayları gönderme

Olayları bir olay hub 'ına gönderen bir Python uygulaması oluşturmak için:

> [!NOTE]
> Hızlı başlangıç üzerinden çalışmak yerine, GitHub 'dan [örnek uygulamaları](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) indirebilir ve çalıştırabilirsiniz. `EventHubConnectionString` ve `EventHubName` dizelerini Olay Hub değerlerinizle değiştirin.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz Python düzenleyicisini açın
2. *Send.py*adlı yeni bir dosya oluşturun. Bu betik, 100 olayları olay hub'ınıza gönderir.
3. Aşağıdaki *kodu, Event Hubs*\<ad alanı >, \<eventhub >, \<accesskeyname > ve \<birincil anahtar değeri > değerlerinizle değiştirin: 
   
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

Betiği çalıştırmak için *Send.py*kaydettiğiniz dizinden şu komutu çalıştırın:

```cmd
start python send.py
```

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="receive-events"></a>Olayları alma

Olay Hub 'ından olayları alan bir Python uygulaması oluşturmak için:

1. Python Düzenleyicinizde *recv.py*adlı bir dosya oluşturun.
2. Aşağıdaki *kodu, Event Hubs*\<ad alanı >, \<eventhub >, \<accesskeyname > ve \<birincil anahtar değeri > değerlerinizle değiştirin: 
   
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

Betiği çalıştırmak için *recv.py*kaydettiğiniz dizinden şu komutu çalıştırın:

```cmd
start python recv.py
```

## <a name="next-steps"></a>Sonraki adımlar
Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Azure Event Hubs'ın özellikleri ve terminolojisi](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)

