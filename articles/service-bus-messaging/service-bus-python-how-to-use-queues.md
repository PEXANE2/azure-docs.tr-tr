---
title: 'Hızlı başlangıç: Python ile Azure Servis Veri Servisi kuyruklarını kullanma'
description: Bu makalede, Azure Hizmet Veri Hizmetleri kuyrukları oluşturmak, ileti göndermek ve ileti almak için Python'u nasıl kullanacağınızı gösterir.
services: service-bus-messaging
documentationcenter: python
author: axisc
editor: spelluru
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: aschhab
ms.custom: seo-python-october2019
ms.openlocfilehash: acb0b0e84804ecf6025e05590133dee9b0d54c48
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478644"
---
# <a name="quickstart-use-azure-service-bus-queues-with-python"></a>Hızlı başlangıç: Python ile Azure Servis Veri Servisi kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu makalede, Azure Hizmet Veri Hizmetleri kuyrukları oluşturmak, ileti göndermek ve ileti almak için Python'u nasıl kullanacağınızı gösterir. 

Python Azure Hizmet Veri Veri Günü kitaplıkları hakkında daha fazla bilgi için [Python için Hizmet Veri Servisi kitaplıklarına](/python/api/overview/azure/servicebus?view=azure-python)bakın.

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliği. Visual Studio [veya MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
- Quickstart'taki adımları izleyerek oluşturulan Hizmet Veri Kurumu ad [alanı: Hizmet Veri Servisi konusu ve abonelikleri oluşturmak için Azure portalını kullanın.](service-bus-quickstart-topics-subscriptions-portal.md) Bu makalenin ilerleyen saatlerinde kullanmak üzere **Paylaşılan erişim ilkeleri** ekranından birincil bağlantı dizesini kopyalayın. 
- [Python Azure Servis Veri Servisi][Python Azure Service Bus package] paketi yüklü olan Python 3.4x veya üzeri. Daha fazla bilgi için [Python Yükleme Kılavuzu'na](/azure/developer/python/azure-sdk-install)bakın. 

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

**ServiceBusClient nesnesi** kuyruklarla çalışmanızı sağlar. Hizmet Veri Veri Tos'a programlı bir şekilde erişmek için Python dosyanızın üst bölümüne aşağıdaki satırı ekleyin:

```python
from azure.servicebus import ServiceBusClient
```

**ServiceBusClient** nesnesi oluşturmak için aşağıdaki kodu ekleyin. Servis `<connectionstring>` Veri Kurumu birincil bağlantı dize değeri ile değiştirin. Bu değeri [Azure portalında][Azure portal]Hizmet Veri Kurumu ad alanınızda **Paylaşılan erişim ilkeleri** altında bulabilirsiniz.

```python
sb_client = ServiceBusClient.from_connection_string('<connectionstring>')
```

Aşağıdaki kod, `create_queue` varsayılan ayarları ile adlı `taskqueue` bir sıra oluşturmak için **ServiceBusClient** yöntemini kullanır:

```python
sb_client.create_queue("taskqueue")
```

İleti süresi (TTL) veya maksimum konu boyutu gibi varsayılan sıra ayarlarını geçersiz kılmak için seçenekleri kullanabilirsiniz. Aşağıdaki kod, en fazla `taskqueue` 5 GB sıra boyutu ve 1 dakika TTL değeri ile adlandırılan bir sıra oluşturur:

```python
sb_client.create_queue("taskqueue", max_size_in_megabytes=5120,
                       default_message_time_to_live=datetime.timedelta(minutes=1))
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme

Hizmet Veri Servisi sırasına ileti göndermek için, `send` bir uygulama **ServiceBusClient** nesnesindeki yöntemi çağırır. Aşağıdaki kod örneği bir sıra istemcisi oluşturur `taskqueue` ve kuyruğa bir test iletisi gönderir. Servis `<connectionstring>` Veri Kurumu birincil bağlantı dize değeri ile değiştirin. 

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string("<connectionstring>", "taskqueue")

# Send a test message to the queue
msg = Message(b'Test Message')
queue_client.send(msg)
```

### <a name="message-size-limits-and-quotas"></a>İleti boyutu sınırları ve kotaları

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir kuyruğun tutabileceği ileti sayısında bir sınır yoktur, ancak sıranın tuttuğu iletilerin toplam boyutunda bir kapak vardır. 5 GB üst sınırı ile oluşturma sırasında sıra boyutunu tanımlayabilirsiniz. 

Kotalar hakkında daha fazla bilgi için [Servis Otobüsü kotalarına][Service Bus quotas]bakın.

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

Sıra istemcisi `get_receiver` **ServiceBusClient** nesnesindeki yöntemi kullanarak bir kuyruktan iletileri alır. Aşağıdaki kod örneği bir sıra istemcisi oluşturur `taskqueue` ve kuyruktan bir ileti alır. Servis `<connectionstring>` Veri Kurumu birincil bağlantı dize değeri ile değiştirin. 

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string("<connectionstring>", "taskqueue")

# Receive the message from the queue
with queue_client.get_receiver() as queue_receiver:
    messages = queue_receiver.fetch_next(timeout=3)
    for message in messages:
        print(message)
        message.complete()
```

### <a name="use-the-peek_lock-parameter"></a>peek_lock parametresini kullanma

İsteğe `peek_lock` bağlı `get_receiver` parametre, Servis Veri Tos'unun iletileri okundukları sırada kuyruktan silip silmeyeceğini belirler. İleti alma için varsayılan mod *PeekLock'tır*veya `peek_lock` **True**olarak ayarlanır, bu modu okur (peeks) ve sırayla silmeden iletileri kilitler. Daha sonra her iletinin kuyruktan kaldırılması için açıkça tamamlanması gerekir.

İletileri okundukları sırada kuyruktan silmek **için**False `peek_lock` parametresini `get_receiver` ayarlayabilirsiniz. İletileri alma işleminin bir parçası olarak silerbırakmak en basit modeldir, ancak yalnızca uygulama bir hata olduğunda eksik iletileri tolere edebilirse çalışır. Bu davranışı anlamak için, tüketicinin bir alma isteği yayınladığı ve işlemeden önce çöktüğün bir senaryoyu düşünün. İleti alınırken silinmişse, uygulama yeniden başlatıldığında ve iletileri yeniden tüketmeye başladığında, kilitlenmeden önce aldığı iletiyi kaçırmıştır.

Uygulamanız kaçırılan iletilere tahammül edemiyorsa, almak iki aşamalı bir işlemdir. PeekLock, bir sonraki iletinin tüketilmesi gerektiğini bulur, diğer tüketicilerin bu iletiyi almasını önlemek için kilitler ve uygulamaya geri verir. İletiyi işledikten veya depoladıktan sonra, uygulama `complete` **İleti** nesnesindeki yöntemi çağırarak alma işleminin ikinci aşamasını tamamlar.  Yöntem, `complete` iletinin tüketildiğini işaretler ve sıradan kaldırır.

## <a name="handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunamayan iletileri işleme

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması bir iletiyi nedense işlemiyorsa, `unlock` **İleti** nesnesi üzerindeki yöntemi çağırabilir. Servis Veri Servisi, sıra içindeki iletinin kilidini açar ve aynı veya başka bir tüketen uygulama tarafından yeniden alınmak üzere kullanılabilir hale getirir.

Sıra içinde kilitli iletiler için bir zaman arası da vardır. Bir uygulama kilit süresi dolmadan önce bir iletiyi işlemezse (örneğin uygulama çöküyorsa, Servis Veri Servisi iletinin kilidini otomatik olarak açar ve yeniden alınmak üzere kullanılabilir hale getirir.

Bir uygulama bir iletiyi işledikten `complete` sonra ancak yöntemi aramadan önce çökerse, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu davranış genellikle *En az bir kez İşleme*olarak adlandırılır. Her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryonuz yinelenen işleme tolere edemiyorsa, yinelenen ileti teslimini işlemek için teslim girişimlerinde sabit kalan iletinin **MessageId** özelliğini kullanabilirsiniz. 

> [!TIP]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, Bir Servis Veri Günü ad alanına bağlanmanızı ve ileti varlıklarını kolayca uygulamanızı sağlar. Araç, alma/dışa aktarma işlevselliği ve konuları, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Artık Servis Veri Yolu kuyruklarının temellerini öğrendiğiniz için, daha fazla bilgi edinmek için [Kuyruklar, konular ve aboneliklere][Queues, topics, and subscriptions] bakın.

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md
