---
title: Ruby ile Azure Service Bus kuyruklarını kullanma
description: Bu öğreticide, Service Bus kuyruğuna ileti göndermek ve ileti almak için Ruby uygulamaları oluşturmayı öğreneceksiniz.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76760598"
---
# <a name="quickstart-how-to-use-service-bus-queues-with-ruby"></a>Hızlı başlangıç: Ruby ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu öğreticide, Service Bus kuyruğuna ileti göndermek ve ileti almak için Ruby uygulamaları oluşturmayı öğreneceksiniz. Örnekler Ruby dilinde yazılır ve Azure GED 'yi kullanır.

## <a name="prerequisites"></a>Ön koşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
2. [Service Bus kuyruğu oluşturmak için Azure Portal kullanma](service-bus-quickstart-portal.md) adımlarını izleyin.
    1. Service Bus **kuyruklara**hızlı **genel bakış** konusunu okuyun. 
    2. Service Bus **ad alanı**oluşturun. 
    3. **Bağlantı dizesini**alın. 

        > [!NOTE]
        > Bu öğreticide Ruby kullanarak Service Bus ad alanında bir **sıra** oluşturacaksınız. 

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-to-create-a-queue"></a>Kuyruk oluşturma
**Azure:: ServiceBusService** nesnesi kuyruklarla çalışmanıza olanak sağlar. Bir kuyruk oluşturmak için `create_queue()` yöntemini kullanın. Aşağıdaki örnek bir kuyruk oluşturur veya herhangi bir hata yazdırır.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Ayrıca, canlı veya en fazla sıra boyutu gibi varsayılan sıra ayarlarını geçersiz kılmanızı sağlayan ek seçeneklerle bir **Azure:: ServiceBus:: Queue** nesnesi geçirebilirsiniz. Aşağıdaki örnek, en fazla sıra boyutunun 5 GB ve yaşam süresi olarak 1 dakikaya nasıl ayarlanacağını gösterir:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Bir kuyruğa ileti gönderme
Uygulamanız bir Service Bus kuyruğuna ileti göndermek için `send_queue_message()` **Azure:: servicebusservice** nesnesinde yöntemini çağırır. Service Bus sıralara gönderilen (ve alınan) iletiler **Azure:: ServiceBus:: BrokeredMessage** nesneleri, bir dizi standart Özellik ( `label` ve `time_to_live`gibi), uygulamaya özgü özel özellikleri tutmak için kullanılan bir sözlük ve rastgele uygulama verileri gövdesi vardır. Bir uygulama, ileti olarak bir dize değeri geçirerek ileti gövdesini ayarlayabilir ve gerekli standart özellikler varsayılan değerlerle doldurulur.

Aşağıdaki örnek, kullanılarak `test-queue` `send_queue_message()`adlı sıraya bir test iletisinin nasıl gönderileceğini göstermektedir:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="how-to-receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İletiler, `receive_queue_message()` **Azure:: servicebusservice** nesnesi üzerindeki yöntemi kullanılarak bir kuyruktan alınır. Varsayılan olarak, iletiler kuyruktan silinmeden, salt okunurdur ve kilitlenir. Ancak, bu `:peek_lock` seçeneği **false**olarak ayarlayarak kuyruktaki iletileri okurken silebilirsiniz.

Varsayılan davranış, iki aşamalı bir işlemi okumayı ve silmeyi, ayrıca eksik iletilere izin verilmeyen uygulamaları desteklemeyi olanaklı hale getirir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan (veya gelecekteki işlemler için güvenilir bir şekilde depolar), yöntemi çağırarak `delete_queue_message()` alma işleminin ikinci aşamasını tamamlar ve bir parametre olarak silinecek iletiyi sağlar. `delete_queue_message()` Yöntemi, iletiyi tüketildiği gibi işaretleyecek ve kuyruktan kaldıracak.

`:peek_lock` Parametre **false**olarak ayarlandıysa, iletiyi okumak ve silmek en basit model haline gelir ve bir uygulamanın hata durumunda bir iletiyi işlememesinin kabul edebildiği senaryolar için en iyi sonuç verir. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus ileti tüketildiği gibi işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenme öncesinde tüketilen ileti kaçırılmış olur.

Aşağıdaki örnek kullanarak `receive_queue_message()`iletilerin nasıl alınacağını ve işleyeceğini gösterir. Örnek, önce **false**olarak ayarla ' yı kullanarak `delete_queue_message()` `:peek_lock` bir iletiyi alır ve siler, daha sonra başka bir ileti alır ve şunu kullanarak iletiyi siler:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulamasının iletiyi bir nedenden dolayı işleyemeyeceği, `unlock_queue_message()` **Azure:: servicebusservice** nesnesinde yöntemi çağırabilir. Bu çağrı, Service Bus kuyruktaki iletinin kilidini açmasının yanı sıra, aynı uygulama ya da başka bir uygulama tarafından yeniden alınabileceğinden emin olur.

Ayrıca, kuyruk içinde kilitlenen bir iletiyle ilişkili bir zaman aşımı vardır ve uygulamanın kilit zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus, otomatik olarak iletinin kilidini açar ve tekrar alınabilmesini sağlar.

Uygulamanın ileti işlendikten sonra çöktüğü durumda, ancak `delete_queue_message()` Yöntem çağrılmadan önce, yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu işlem genellikle *en az bir kez işleme*olarak adlandırılır; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu, genellikle ileti `message_id` özelliği kullanılarak elde edilir ve bu da teslim girişimleri arasında sabit kalır.

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Artık Service Bus kuyruklarına ilişkin temel bilgileri öğrendiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [Kuyruklara, konulara ve aboneliklere](service-bus-queues-topics-subscriptions.md)genel bakış.
* GitHub 'daki [Ruby deposu Için Azure SDK 'sını](https://github.com/Azure/azure-sdk-for-ruby) ziyaret edin.

Bu makalede açıklanan Azure Service Bus kuyrukları ve [Ruby 'Den kuyruk depolama 'yı kullanma](../storage/queues/storage-ruby-how-to-use-queue-storage.md) bölümünde açıklanan Azure Kuyrukları arasında bir karşılaştırma için bkz. [azure kuyrukları ve Azure Service Bus kuyrukları-karşılaştırılan ve değişken maliyetli](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

