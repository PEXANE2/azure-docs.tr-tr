---
title: Ruby ile Azure Servis Veri Servisi kuyrukları nasıl kullanılır?
description: Bu eğitimde, Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için Ruby uygulamalarının nasıl oluşturulacağımı öğrenirsiniz.
services: service-bus-messaging
documentationcenter: ruby
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: quickstart
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: a699543bb442e7c57d57e72acb2cdf6ac40159c1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76760598"
---
# <a name="quickstart-how-to-use-service-bus-queues-with-ruby"></a>Quickstart: Ruby ile Servis Veri Servisi kuyrukları nasıl kullanılır?

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu eğitimde, Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için Ruby uygulamalarının nasıl oluşturulacağımı öğrenirsiniz. Örnekler Ruby ile yazılır ve Azure mücevherini kullanır.

## <a name="prerequisites"></a>Ön koşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
2. Hizmet Veri Servisi sırası makalesi [oluşturmak için Azure'u kullan portalındaki](service-bus-quickstart-portal.md) adımları izleyin.
    1. Servis Veri Servisi **kuyruklarına**hızlı **bir genel bakış** okuyun. 
    2. Hizmet Veri Günü **ad alanı**oluşturun. 
    3. Bağlantı **dizesini**alın. 

        > [!NOTE]
        > Bu öğreticide Ruby'yi kullanarak Hizmet Veri Servisi ad alanında bir **kuyruk** oluşturacaksınız. 

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-to-create-a-queue"></a>Kuyruk oluşturma
**Azure::ServiceBusService** nesnesi kuyruklarla çalışmanızı sağlar. Sıra oluşturmak için `create_queue()` yöntemi kullanın. Aşağıdaki örnek bir sıra oluşturur veya hataları yazdırır.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Ayrıca bir **Azure::ServiceBus::Ek** seçeneklerle sıra nesnesi geçirebilirsiniz, bu da ileti süresi veya maksimum sıra boyutu gibi varsayılan sıra ayarlarını geçersiz kılmanızı sağlar. Aşağıdaki örnek, maksimum sıra boyutunun 5 GB'a ve 1 dakikaya kadar yaşama süresini nasıl ayarlayabilirsiniz:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Sıraya ileti gönderme
Hizmet Veri Servisi kuyruğuna ileti göndermek için `send_queue_message()` uygulamanız **Azure:ServiceBusService** nesnesi üzerindeki yöntemi çağırır. Hizmet Veri Hizmetleri sıralarına gönderilen (ve alınan) Iletiler **Azure::ServiceBus::Aracılı İleti** nesneleridir ve özel `label` `time_to_live`uygulamaya özgü özellikleri tutmak için kullanılan bir sözlük ve rasgele uygulama verileri kümesi gibi standart özelliklere (örneğin ve), sahip olunur. Bir uygulama ileti olarak bir dize değeri geçirerek iletinin gövdesini ayarlayabilir ve gerekli standart özellikleri varsayılan değerlerle doldurulur.

Aşağıdaki örnek, aşağıdaki kullanarak adlı `test-queue` kuyruğa `send_queue_message()`nasıl bir test iletisi gönderilen gösterir:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="how-to-receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İletiler, `receive_queue_message()` **Azure:ServiceBusService** nesnesi üzerindeki yöntemi kullanarak bir kuyruktan alınır. Varsayılan olarak, iletiler sırayla silinmeden okunur ve kilitlenir. Ancak, iletileri okundukları sırada seçenek **yanlış**olarak `:peek_lock` ayarlayarak silebilirsiniz.

Varsayılan davranış, iki aşamalı bir işlemi okumayı ve silmeyi sağlar ve bu da eksik iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi bitirdikten (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) yöntemi arayarak `delete_queue_message()` ve iletinin parametre olarak silineceğini sağlayarak alma işleminin ikinci aşamasını tamamlar. Yöntem, `delete_queue_message()` iletinin tüketildiği olarak işaretlenecek ve sıradan kaldıracaktır.

`:peek_lock` Parametre **yanlış**olarak ayarlanmışsa, iletiyi okumak ve silerseniz en basit model olur ve bir uygulamanın bir hata durumunda bir iletiyi işlememeye tahammül edebileceği senaryolar için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Servis Veri Kurumu, iletinin tüketildiği olarak işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden almaya başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmış olur.

Aşağıdaki örnek, iletilerin nasıl alınıp `receive_queue_message()`işlenirken yapılacağını gösterir. Örnek ilk alır ve `:peek_lock` **false**için ayarlayarak bir ileti siler, sonra başka `delete_queue_message()`bir ileti alır ve sonra kullanarak ileti siler:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi nedense işleyemiyorsa, `unlock_queue_message()` **azure:ServiceBusService** nesnesi üzerindeki yöntemi çağırabilir. Bu çağrı, Servis Veri Servisi'nin sıra içindeki iletinin kilidini açmasına ve aynı alıcı uygulama veya başka bir tüketen uygulama tarafından yeniden alınmak üzere kullanılabilir hale getirmesine neden olur.

Sıra içinde kilitli bir iletiyle ilişkili bir zaman aşımı da vardır ve uygulama kilit süresi sona ermeden önce iletiyi işlemezse (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletiyi otomatik olarak açar ve tekrar alınabilir.

Uygulamanın iletiyi işledikten sonra ancak `delete_queue_message()` yöntem çağrılmadan önce çökmesi durumunda, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu işlem genellikle *En Az Bir Kez İşleme*denir; diğer bir deyişle, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle teslim `message_id` girişimleri arasında sabit kalır iletinin özelliği kullanılarak elde edilir.

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Artık Service Bus kuyruklarına ilişkin temel bilgileri öğrendiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [Kuyruklara, konulara ve aboneliklere](service-bus-queues-topics-subscriptions.md)genel bakış.
* GitHub'daki Ruby deposu [için Azure SDK'yı](https://github.com/Azure/azure-sdk-for-ruby) ziyaret edin.

Bu makalede tartışılan Azure Hizmet Veri Mes'leri ile [Ruby makalesinden Kuyruk depolama alanının nasıl kullanılacağı](../storage/queues/storage-ruby-how-to-use-queue-storage.md) tartışılan Azure Kuyrukları arasında karşılaştırma yapmak için Azure [Kuyrukları ve Azure Hizmet Veri Sırası Kuyrukları - Karşılaştırıldığında ve Zıt](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

