---
title: 'Hızlı başlangıç: Python ile Azure Hizmet Veri Servisi konularını ve aboneliklerini kullanma'
description: Bu makalede, bir Azure Hizmet Veri Servisi konusu, abonelik, bir konuya ileti gönderme ve abonelikten ileti alma gibi bir konunun nasıl oluşturulabileceğinizgösterilmektedir.
services: service-bus-messaging
documentationcenter: python
author: axisc
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: aschhab
ms.openlocfilehash: 03e22c4c179850e5140015c0abc2d89f16d4b624
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76774554"
---
# <a name="quickstart-use-service-bus-topics-and-subscriptions-with-python"></a>Quickstart: Python ile Servis Veri Servisi konularını ve aboneliklerini kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede, Python'un Azure Hizmet Veri Servisi konuları ve abonelikleriyle nasıl kullanılacağı açıklanmaktadır. Örnekler, [Azure Python SDK][Azure Python package] paketini aşağıdakiler için kullanır: 

- Konulara ve konulara abonelikler oluşturma
- Abonelik filtreleri ve kuralları oluşturma
- Konulara ileti gönderme 
- Aboneliklerden ileti alma
- Konu başlıklarını ve abonelikleri silme

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliği. Visual Studio [veya MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
- Quickstart'taki adımları izleyerek oluşturulan Hizmet Veri Kurumu ad [alanı: Hizmet Veri Servisi konusu ve abonelikleri oluşturmak için Azure portalını kullanın.](service-bus-quickstart-topics-subscriptions-portal.md) Daha sonra bu hızlı başlatmada kullanmak üzere Paylaşılan **erişim ilkeleri** ekranından ad alanı adını, paylaşılan erişim anahtarı adını ve birincil anahtar değerini kopyalayın. 
- Python 3.4x veya üzeri, [Azure Python SDK][Azure Python package] paketi yüklü. Daha fazla bilgi için [Python Yükleme Kılavuzu'na](/azure/python/python-sdk-azure-install)bakın.

## <a name="create-a-servicebusservice-object"></a>ServiceBusService nesnesi oluşturma

**ServiceBusService nesnesi,** konularve konulara abonelikler ile çalışmanıza olanak tanır. Hizmet Veri Veri Tos'a programlı bir şekilde erişmek için Python dosyanızın üst bölümüne aşağıdaki satırı ekleyin:

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

**ServiceBusService** nesnesi oluşturmak için aşağıdaki kodu ekleyin. 'yi ve `<sharedaccesskeyvalue>` Hizmet Veri Aracı ad alanını, Paylaşılan Erişim İmzası (SAS) anahtar adını ve birincil anahtar değerini değiştirin. `<namespace>` `<sharedaccesskeyname>` Bu değerleri Azure [portalında][Azure portal]Hizmet Veri Kurumu ad alanınızda **Paylaşılan erişim ilkeleri** altında bulabilirsiniz.

```python
bus_service = ServiceBusService(
    service_namespace='<namespace>',
    shared_access_key_name='<sharedaccesskeyname>',
    shared_access_key_value='<sharedaccesskeyvalue>')
```

## <a name="create-a-topic"></a>Konu başlığı oluşturma

Aşağıdaki `mytopic`kod, `create_topic` varsayılan ayarları olan Bir Hizmet Veri Servisi konusu oluşturmak için yöntemi kullanır:

```python
bus_service.create_topic('mytopic')
```

İleti süresi (TTL) veya maksimum konu boyutu gibi varsayılan konu ayarlarını geçersiz kılmak için konu seçeneklerini kullanabilirsiniz. Aşağıdaki örnekte, maksimum `mytopic` konu boyutu 5 GB ve varsayılan ileti TTL bir dakika olan bir konu oluşturulur:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonelikleri oluşturma

Konulara abonelik ler oluşturmak için **ServiceBusService** nesnesini de kullanırsınız. Aboneliğin, ileti kümesini sanal kuyruğa teslim etmesini kısıtlayan bir filtresi olabilir. Bir filtre belirtmezseniz, yeni abonelikler konuya göre yayınlanan tüm iletileri aboneliğin sanal kuyruğuna yerleştiren varsayılan **MatchAll** filtresini kullanır. Aşağıdaki örnek, **MatchAll** `mytopic` filtresini kullanan adlandırılmış `AllMessages` bir abonelik oluşturur:

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="use-filters-with-subscriptions"></a>Aboneliklerle filtre kullanma

Abonelikte `create_rule` görünen iletileri filtrelemek için **ServiceBusService** nesnesinin yöntemini kullanın. Aboneliği oluştururken kurallar belirleyebilir veya varolan aboneliklere kurallar ekleyebilirsiniz.

En esnek filtre türü, SQL-92 alt kümesini kullanan bir **SqlFilter'dir.** SQL filtreleri, konuya yayınlanan iletilerin özelliklerine göre çalışır. SQL filtresi ile kullanabileceğiniz ifadeler hakkında daha fazla bilgi için [SqlFilter.SqlExpression][SqlFilter.SqlExpression] sözdizimine bakın.

**MatchAll** varsayılan filtresi tüm yeni aboneliklere otomatik olarak uygulandığından, filtrelemek istediğiniz aboneliklerden kaldırmanız gerekir veya **MatchAll** belirttiğiniz diğer filtreleri geçersiz kılar. `delete_rule` **ServiceBusService** nesnesinin yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz.

Aşağıdaki örnek, sqlfilter `mytopic` kuralı `HighMessages`adlı `HighMessageFilter`bir **abonelik** oluşturur. Kural, `HighMessageFilter` yalnızca 3'ten büyük `messageposition` özel bir özelliğe sahip iletileri seçer:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messageposition > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Aşağıdaki örnek, sqlfilter `mytopic` kuralı `LowMessages`adlı `LowMessageFilter`bir **abonelik** oluşturur. Kural, `LowMessageFilter` yalnızca 3'ten `messageposition` az veya eşit bir özelliği olan iletileri seçer:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messageposition <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

`AllMessages`, `HighMessages`, `LowMessages` ve tüm yürürlükte, gönderilen `mytopic` iletiler her zaman `AllMessages` abonelik alıcılarına teslim edilir. İletiler, iletinin `HighMessages` `LowMessages` `messageposition` özellik değerine bağlı olarak seçici olarak aboneliğe de teslim edilir. 

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme

Uygulamalar, Bir `send_topic_message` Hizmet Veri Servisi konusuna ileti göndermek için **ServiceBusService** nesnesinin yöntemini kullanır.

Aşağıdaki örnek, konuya beş test `mytopic` iletisi gönderir. Özel `messageposition` özellik değeri döngüyinelemebağlıdır ve hangi abonelikleri iletileri alır belirler. 

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'),
                  custom_properties={'messageposition': i})
    bus_service.send_topic_message('mytopic', msg)
```

### <a name="message-size-limits-and-quotas"></a>İleti boyutu sınırları ve kotaları

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir konunun tutabileceği ileti sayısında bir sınır yoktur, ancak konunun tuttuğu iletilerin toplam boyutuhakkında bir kapak vardır. 5 GB üst sınırı ile, oluşturma zamanında konu boyutunu tanımlayabilirsiniz. 

Kotalar hakkında daha fazla bilgi için [Servis Otobüsü kotalarına][Service Bus quotas]bakın.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma

Uygulamalar, bir `receive_subscription_message` abonelikten ileti almak için **ServiceBusService** nesnesindeki yöntemi kullanır. Aşağıdaki `LowMessages` örnek, abonelikten iletileri alır ve okundukları gibi siler:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

İsteğe `peek_lock` bağlı `receive_subscription_message` parametre, Servis Veri Tos'unun iletileri okundukları gibi abonelikten silip silmeyeceğini belirler. İleti alma için varsayılan mod *PeekLock'tır*veya `peek_lock` **iletileri**abonelikten silmeden okuyan (peeks) ve kilitleyen True olarak ayarlanır. Daha sonra her iletinin abonelikten kaldırılması için açıkça tamamlanması gerekir.

İletileri okunurken abonelikten silmek için, parametreyi `peek_lock` önceki örnekte olduğu gibi **False**olarak ayarlayabilirsiniz. Alma işleminin bir parçası olarak iletileri siletmek en basit modeldir ve uygulama bir hata varsa eksik iletileri tolere edebilirse iyi çalışır. Bu davranışı anlamak için, uygulamanın bir alma isteği ne zaman sorun çıkaran ve işlemeden önce kilitlendiği bir senaryo düşünün. İleti alınırken silinmişse, uygulama yeniden başlatıldığında ve iletileri yeniden tüketmeye başladığında, kilitlenmeden önce aldığı iletiyi kaçırmıştır.

Uygulamanız kaçırılan iletilere tahammül edemiyorsa, alma iki aşamalı bir işlem haline gelir. PeekLock, bir sonraki iletinin tüketilmesi gerektiğini bulur, diğer tüketicilerin bu iletiyi almasını önlemek için kilitler ve uygulamaya geri verir. İletiyi işledikten veya depoladıktan sonra, uygulama `complete` **İleti** nesnesindeki yöntemi çağırarak alma işleminin ikinci aşamasını tamamlar.  Yöntem, `complete` iletinin tüketildiğini işaretler ve abonelikten kaldırır.

Aşağıdaki örnek, bir göz kilidi senaryosu gösterir:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
    print(msg.body)
    msg.complete()
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunamayan iletileri işleme

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması bir iletiyi nedense işlemiyorsa, `unlock` **İleti** nesnesi üzerindeki yöntemi çağırabilir. Servis Veri Servisi, abonelik içindeki iletinin kilidini açar ve aynı veya başka bir tüketen uygulama tarafından yeniden alınmak üzere kullanılabilir hale getirir.

Abonelik içinde kilitli iletiler için bir zaman arası da vardır. Bir uygulama kilit süresi dolmadan önce bir iletiyi işlemezse (örneğin uygulama çöküyorsa, Servis Veri Servisi iletinin kilidini otomatik olarak açar ve yeniden alınmak üzere kullanılabilir hale getirir.

Bir uygulama bir iletiyi işledikten `complete` sonra ancak yöntemi aramadan önce çökerse, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu davranış genellikle *En az bir kez İşleme*olarak adlandırılır. Her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryonuz yinelenen işleme tolere edemiyorsa, yinelenen ileti teslimini işlemek için teslim girişimlerinde sabit kalan iletinin **MessageId** özelliğini kullanabilirsiniz. 

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme

Konuları ve abonelikleri silmek için Azure `delete_topic` [portalını][Azure portal] veya yöntemi kullanın. Aşağıdaki kod adlı `mytopic`konuyu siler:

```python
bus_service.delete_topic('mytopic')
```

Bir konuyu silmek, konuyla ilgili tüm abonelikleri siler. Abonelikleri bağımsız olarak da silebilirsiniz. Aşağıdaki `mytopic` kod, başlıktan `HighMessages` adı geçen aboneliği siler:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

Varsayılan olarak, konular ve abonelikler kalıcıdır ve siz bunları silene kadar var olur. Belirli bir süre geçtikten sonra abonelikleri otomatik olarak silmek için, abonelikteki [auto_delete_on_idle](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python) parametresini ayarlayabilirsiniz. 

> [!TIP]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, Bir Servis Veri Günü ad alanına bağlanmanızı ve ileti varlıklarını kolayca uygulamanızı sağlar. Araç, alma/dışa aktarma işlevselliği ve konuları, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Service Bus konularının temellerini öğrendiğiniz için, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions]
* [SqlFilter.SqlExpression][SqlFilter.SqlExpression] başvurusu

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
