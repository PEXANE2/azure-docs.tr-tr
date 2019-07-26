---
title: Python ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Python 'dan Azure Service Bus kuyruklarını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: fa3aedf138564fedafe555adfbaf6c56efc1813e
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360850"
---
# <a name="how-to-use-service-bus-queues-with-python"></a>Python ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu öğreticide, Service Bus kuyruğuna ileti göndermek ve ileti almak için Python uygulamaları oluşturmayı öğreneceksiniz. 

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
2. [Service Bus kuyruğu oluşturmak için Azure Portal kullanma](service-bus-quickstart-portal.md) adımlarını izleyin.
    1. Service Bus **kuyruklara**hızlı **genel bakış** konusunu okuyun. 
    2. Service Bus **ad alanı**oluşturun. 
    3. **Bağlantı dizesini**alın. 

        > [!NOTE]
        > Bu öğreticide Python kullanarak Service Bus ad alanında bir **sıra** oluşturacaksınız. 
1. Python veya [python Azure Service Bus paketini][Python Azure Service Bus package]yükleme, bkz. [Python Yükleme Kılavuzu](../python-how-to-install.md). Service Bus [Python SDK 'nın](/python/api/overview/azure/servicebus?view=azure-python)tam belgelerine bakın.

## <a name="create-a-queue"></a>Kuyruk oluştur
**Servicebusclient** nesnesi kuyruklarla çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek istediğiniz herhangi bir Python dosyasının üst tarafına yakın bir şekilde aşağıdaki kodu ekleyin:

```python
from azure.servicebus import ServiceBusClient
```

Aşağıdaki kod bir **Servicebusclient** nesnesi oluşturur. ServiceBus ConnectionString ile değiştirin `<CONNECTION STRING>` .

```python
sb_client = ServiceBusClient.from_connection_string('<CONNECTION STRING>')
```

SAS anahtar adı ve değeri için değerler [Azure Portal][Azure portal] bağlantı bilgilerinde veya Sunucu Gezgini içinde Service Bus ad alanı seçerken Visual Studio **Özellikler** bölmesinde bulunabilir (önceki bölümde gösterildiği gibi).

```python
sb_client.create_queue("taskqueue")
```

`create_queue` Yöntemi Ayrıca, ileti yaşam süresi (TTL) veya maksimum sıra boyutu gibi varsayılan sıra ayarlarını geçersiz kılmanızı sağlayan ek seçenekleri de destekler. Aşağıdaki örnek, en büyük sıra boyutunu 5 GB ve TTL değerini 1 dakikaya ayarlar:

```python
sb_client.create_queue("taskqueue", max_size_in_megabytes=5120,
                       default_message_time_to_live=datetime.timedelta(minutes=1))
```

Daha fazla bilgi için bkz. [Python belgelerini Azure Service Bus](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Service Bus kuyruğuna ileti göndermek için, uygulamanız `send` `ServiceBusClient` nesne üzerindeki yöntemini çağırır.

Aşağıdaki örnek, kullanılarak `taskqueue` `send_queue_message`adlı sıraya bir test iletisinin nasıl gönderileceğini göstermektedir:

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string(
    "<CONNECTION STRING>", "<QUEUE NAME>")

# Send a test message to the queue
msg = Message(b'Test Message')
queue_client.send(msg)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz. [Service Bus kotaları][Service Bus quotas].

Daha fazla bilgi için bkz. [Python belgelerini Azure Service Bus](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İleti, `get_receiver` `ServiceBusService` nesne üzerindeki yöntemi kullanılarak bir kuyruktan alınır:

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string(
    "<CONNECTION STRING>", "<QUEUE NAME>")

# Receive the message from the queue
with queue_client.get_receiver() as queue_receiver:
    messages = queue_receiver.fetch_next(timeout=3)
    for message in messages:
        print(message)
        message.complete()
```

Daha fazla bilgi için bkz. [Python belgelerini Azure Service Bus](/python/api/overview/azure/servicebus?view=azure-python).


Mesajlar, parametre `peek_lock` **false**olarak ayarlandığında okunduklarında silinir. Parametreyi `peek_lock` , **doğru**olarak ayarlayarak iletiyi okuyabilir (Peek) ve kilitle 'yi kuyruktan silebilirsiniz.

İletiyi alma işleminin bir parçası olarak okuma ve silme davranışı en basit modeldir ve bir uygulamanın hata durumunda bir iletiyi işlememesinin kabul edebildiği senaryolar için en iyi sonuç verir. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi tüketildiği gibi işaretlediği için, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmış olur.

Parametresi true olarak ayarlanırsa, Receive iki aşamalı bir işlem haline gelir ve bu, eksik iletilere izin verilmeyen uygulamaları desteklemeyi olanaklı kılar.  `peek_lock` Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan (veya gelecekteki işlemler için güvenilir bir şekilde depolar), **ileti** nesnesindeki **Delete** yöntemini çağırarak alma işleminin ikinci aşamasını tamamlar. **Delete** yöntemi iletiyi tüketildiği gibi işaretleyecek ve kuyruktan kaldıracak.

```python
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi bir nedenden dolayı işleyebilirse, **ileti** nesnesinde **unlock** metodunu çağırabilir. Bu, Service Bus kuyruktaki iletinin kilidini açmasına ve aynı uygulama ya da başka bir uygulama tarafından yeniden alınabilir olmasını sağlar.

Ayrıca, kuyruk içinde kilitlenen bir iletiyle ilişkili bir zaman aşımı vardır ve uygulamanın kilit zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama çökerse), Service Bus otomatik olarak iletinin kilidini açar ve bunu yapar yeniden alınmak üzere kullanılabilir.

İleti işlendikten sonra, ancak **Delete** yöntemi çağrılmadan önce uygulamanın çöktüğü durumda, yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu genellikle **en az bir kez işleme**olarak adlandırılır, diğer bir deyişle her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu işlem, genellikle iletinin teslimat denemelerinde korunan **MessageId** özelliği kullanılarak gerçekleştirilir.

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Service Bus kuyrukların temellerini öğrendiğinize göre, daha fazla bilgi edinmek için şu makalelere bakın.

* [Kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

