---
title: 'Quickstart: Servis Veri Servisi konuları (Ruby) nasıl kullanılır?'
description: "Hızlı başlangıç: Azure'da Hizmet Veri Yolu konularını ve abonelikleri nasıl kullanacağınızı öğrenin. Kod örnekleri Ruby uygulamaları için yazılmıştır."
services: service-bus-messaging
documentationcenter: ruby
author: axisc
manager: timlt
editor: ''
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: aschhab
ms.openlocfilehash: b5401eae844ed2113a9fbc07c8b3ad8601709d43
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73718937"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Quickstart: Ruby ile Servis Veri Servisi konuları ve abonelikleri nasıl kullanılır?
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede, Ruby uygulamalarından Hizmet Veri Meskonu konuları ve abonelikleri nasıl kullanılacağı açıklanmaktadır. Kapsanan senaryolar şunlardır:

- Konu ve abonelik oluşturma 
- Abonelik filtreleri oluşturma 
- Bir konuya ileti gönderme 
- Abonelikten ileti alma
- Konuları ve abonelikleri silme


## <a name="prerequisites"></a>Ön koşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Visual Studio [veya MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
2. Quickstart'taki adımları izleyin: Hizmet Veri Yolunda niçin [bir konu oluşturmak için Azure portalını kullanın ve bir](service-bus-quickstart-topics-subscriptions-portal.md) Hizmet Veri Servisi ad **alanı** oluşturmak ve bağlantı **dizesini**almak için konuya abonelikler oluşturun. 

    > [!NOTE]
    > Bu hızlı başlangıçta **Ruby'yi** kullanarak bir **konu** ve konuya **abonelik** oluşturacaksınız. 

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Konu başlığı oluşturma
**Azure::ServiceBusService nesnesi** konularla çalışmanızı sağlar. Aşağıdaki kod bir **Azure oluşturur::ServiceBusService nesnesi.** Bir konu oluşturmak için `create_topic()` yöntemi kullanın. Aşağıdaki örnekbir konu oluşturur veya hataları yazdırır.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_topic("test-topic")
rescue
  puts $!
end
```

Ayrıca bir **Azure geçirebilirsiniz::ServiceBus::Ek** seçenekleri olan konu nesnesi, ileti süresi veya maksimum sıra boyutu gibi varsayılan konu ayarlarını geçersiz kılmanızı sağlar. Aşağıdaki örnek, en fazla sıra boyutunun 5 GB'a, yaşama süresinin ise 1 dakikaya ayarını gösterir:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Abonelikleri oluşturma
Konu abonelikleri de **Azure::ServiceBusService** nesnesi ile oluşturulur. Abonelikler adlandırılmış ve aboneliğin sanal kuyruğa teslim edilen ileti kümesini kısıtlayan isteğe bağlı bir filtreye sahip olabilir.

Varsayılan olarak, abonelikler kalıcıdır. Onlar veya ilişkili oldukları konu silinene kadar var olmaya devam ederler. Uygulamanız abonelik oluşturmak için mantık içeriyorsa, öncelikle getSubscription yöntemini kullanarak aboneliğin zaten var olup olmadığını denetlemelidir.

[AutoDeleteOnIdle özelliğini](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.autodeleteonidle)ayarlayarak aboneliklerin otomatik olarak silinmesini sağlayabilirsiniz.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
Yeni bir abonelik oluşturulduğunda filtre belirtilmezse, **MatchAll** filtresi (varsayılan) kullanılır. **MatchAll** filtresi kullanıldığında, konuya göre yayınlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte "tüm iletiler" adlı bir abonelik oluşturulur ve varsayılan **MatchAll** filtresi ni kullanır.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Ayrıca, bir konuya gönderilen iletilerin belirli bir abonelik içinde gösterilmesi gerektiğini belirtmenizi sağlayan filtreler de tanımlayabilirsiniz.

Abonelikler tarafından desteklenen en esnek filtre türü, SQL92 alt kümesini uygulayan **Azure:ServiceBus::SqlFilter'dir.** SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL filtresi ile kullanılabilecek ifadeler hakkında daha fazla bilgi için [SqlFilter](service-bus-messaging-sql-filter.md) sözdizimini inceleyin.

**Azure:ServiceBusService** nesnesi `create_rule()` yöntemini kullanarak aboneye filtre ekleyebilirsiniz. Bu yöntem, varolan bir aboneye yeni filtreler eklemenize olanak tanır.

Varsayılan filtre tüm yeni aboneliklere otomatik olarak uygulandığından, önce varsayılan filtreyi kaldırmanız gerekir veya **MatchAll** belirtebileceğiniz diğer filtreleri geçersiz kılar. `delete_rule()` **Azure:ServiceBusService** nesnesi yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz.

Aşağıdaki örnekte, **azure::ServiceBus::SqlFilter** yalnızca 3'ten büyük özel `message_number` bir özelliğe sahip iletileri seçen "yüksek iletiler" adlı bir abonelik oluşturulur:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Benzer şekilde, aşağıdaki örnek, yalnızca `low-messages` 3'ten az veya eşit bir özelliği olan iletileri seçen `message_number` Bir **Azure::ServiceBus::SqlFilter** ile adında bir abonelik oluşturur:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Bir mesaj `test-topic`şimdi gönderildiğinde, her zaman `all-messages` konu aboneliğine abone olan alıcılara teslim edilir ve seçici `high-messages` olarak `low-messages` konu aboneliklerine abone olan alıcılara teslim edilir (ileti içeriğine bağlı olarak).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Bir Hizmet Veri Yolundan'a ileti göndermek için `send_topic_message()` uygulamanızın **Azure:ServiceBusService** nesnesi yöntemini kullanması gerekir. Hizmet Veri Mes'i konularına gönderilen iletiler **Azure örnekleridir::ServiceBus::Aracılı İleti** nesneleri. **Azure::ServiceBus::Aracılı İleti** nesnelerinde, özel uygulamaya özgü `label` `time_to_live`özellikleri tutmak için kullanılan bir sözlük ve dize verileri kümesi gibi bir standart özellikler kümesi vardır. Bir uygulama yönteme bir dize değeri geçirerek `send_topic_message()` iletinin gövdesini ayarlayabilir ve gerekli standart özellikler varsayılan değerlere göre doldurulur.

Aşağıdaki örnek, `test-topic`''ye beş test iletisi nasıl gönderilebildiğini gösterir Her `message_number` iletinin özel özellik değeri döngü yinelemeye göre değişir (hangi aboneliğin aldığını belirler):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
İletiler, `receive_subscription_message()` **Azure:ServiceBusService** nesnesi üzerindeki yöntem kullanılarak abonelikten alınır. Varsayılan olarak, iletiler okunur(tepe) ve abonelikten silmeden kilitlenir. `peek_lock` Seçeneği **false'a**ayarlayarak iletiyi abonelikten okuyabilir ve silebilirsiniz.

Varsayılan davranış, iki aşamalı bir işlemi okumayı ve silmeyi sağlar ve bu da eksik iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi bitirdikten (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) yöntemi arayarak `delete_subscription_message()` ve iletinin parametre olarak silineceğini sağlayarak alma işleminin ikinci aşamasını tamamlar. Yöntem, `delete_subscription_message()` iletinin tüketildiğini ve abonelikten kaldırılmasını işaretler.

`:peek_lock` Parametre **yanlış**olarak ayarlanmışsa, iletiyi okumak ve silerseniz en basit model olur ve bir hata oluştuğunda bir iletiyi işlememeye tahammül edemediği senaryolar için en iyi şekilde çalışır. Tüketicinin alma isteğini sorunları ve işlemeden önce çöktüğün bir senaryodüşünün. Servis Veri Kurumu iletinin tüketildiği olarak işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden almaya başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmıştır.

Aşağıdaki örnek, iletilerin nasıl alınabileceğini ve `receive_subscription_message()`nasıl işlenebileceğini gösterir. Örnek ilk alır ve **yanlış** `:peek_lock` için `low-messages` ayarlayarak abonelikten bir ileti siler, sonra başka bir `delete_subscription_message()`ileti alır `high-messages` ve sonra kullanarak ileti siler:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi nedense işleyemiyorsa, `unlock_subscription_message()` **azure:ServiceBusService** nesnesi üzerindeki yöntemi çağırabilir. Hizmet Veri Servisi'nin abonelik içindeki iletinin kilidini açmasına ve aynı alıcı uygulama veya başka bir tüketen uygulama tarafından yeniden alınabilmek için kullanılabilir hale getirmesine neden olur.

Abonelik içinde kilitli bir iletiyle ilişkili bir zaman aşımı da vardır ve uygulama kilit süresi sona ermeden önce iletiyi işlemezse (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletiyi otomatik olarak açar ve tekrar alınmak üzere kullanılabilir hale getirin.

Uygulamanın iletiyi işledikten sonra ancak `delete_subscription_message()` yöntem çağrılmadan önce çökmesi durumunda, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Genellikle en *az bir kez işleme*denir; diğer bir deyişle, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu mantık genellikle teslim `message_id` girişimleri arasında sabit kalır iletinin özelliği kullanılarak elde edilir.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
[AutoDeleteOnIdle özelliği](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.autodeleteonidle) ayarlınca konular ve abonelikler kalıcıdır. Azure [portalı][Azure portal] üzerinden veya programlı olarak silinebilir. Aşağıdaki örnek, adı geçen `test-topic`konunun nasıl silinir olduğunu gösterir.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki kod, adından da anlaşılan `high-messages` `test-topic` aboneliğin nasıl silinir olduğunu gösterir:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Artık Servis Veri Yolu konularının temellerini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları takip edin.

* [Bkz. Kuyruklar, konular ve abonelikler.](service-bus-queues-topics-subscriptions.md)
* [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) için API başvurusu
* GitHub'daki Ruby deposu [için Azure SDK'yı](https://github.com/Azure/azure-sdk-for-ruby) ziyaret edin.

[Azure portal]: https://portal.azure.com
