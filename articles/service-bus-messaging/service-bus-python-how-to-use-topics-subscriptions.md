---
title: 'Hızlı başlangıç: Python ile Azure Service Bus konuları ve abonelikleri kullanma'
description: Bu makalede bir Azure Service Bus konusu oluşturma, abonelik, bir konuya ileti gönderme ve abonelikten ileti alma işlemlerinin nasıl yapılacağı gösterilir.
documentationcenter: python
author: spelluru
ms.devlang: python
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.custom: devx-track-python
ms.openlocfilehash: 01ed3df547a7dc02e0ca29028876eddb8be21c77
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852455"
---
# <a name="quickstart-use-service-bus-topics-and-subscriptions-with-python"></a>Hızlı başlangıç: Python ile Service Bus konuları ve abonelikleri kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede, Python 'un Azure Service Bus konuları ve abonelikleri ile nasıl kullanılacağı açıklanır. Örnekler için [Azure Python SDK][Azure Python package] paketini kullanın: 

- Konularda konular ve abonelikler oluşturun
- Abonelik filtreleri ve kuralları oluşturma
- Konulara ileti gönder 
- Aboneliklerden ileti alma
- Konu başlıklarını ve abonelikleri silme

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. [Visual Studio veya MSDN abonesi avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- [Hızlı başlangıç: bir Service Bus konusu ve abonelikleri oluşturmak için Azure Portal](service-bus-quickstart-topics-subscriptions-portal.md)kullanarak oluşturulan bir Service Bus ad alanı. Bu hızlı başlangıçta kullanmak üzere **paylaşılan erişim ilkeleri** ekranından ad alanı adını, paylaşılan erişim anahtarı adını ve birincil anahtar değerini kopyalayın. 
- [Azure Python SDK][Azure Python package] paketi yüklüyken Python 3.4 x veya üzeri. Daha fazla bilgi için bkz. [Python Yükleme Kılavuzu](/azure/developer/python/azure-sdk-install).

## <a name="create-a-servicebusservice-object"></a>ServiceBusService nesnesi oluşturma

**Servicebusservice** nesnesi konularda konular ve abonelikler ile çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek için, Python dosyanızın en üstüne aşağıdaki satırı ekleyin:

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

**Servicebusservice** nesnesi oluşturmak için aşağıdaki kodu ekleyin. `<namespace>`, `<sharedaccesskeyname>` Ve `<sharedaccesskeyvalue>` Service Bus ad alanı adınız, paylaşılan ERIŞIM imzası (SAS) anahtar adı ve birincil anahtar değeri ile değiştirin. Bu değerleri, [Azure portal][Azure portal]Service Bus ad uzayındaki **paylaşılan erişim ilkeleri** altında bulabilirsiniz.

```python
bus_service = ServiceBusService(
    service_namespace='<namespace>',
    shared_access_key_name='<sharedaccesskeyname>',
    shared_access_key_value='<sharedaccesskeyvalue>')
```

## <a name="create-a-topic"></a>Konu başlığı oluşturma

Aşağıdaki kod, `create_topic` varsayılan ayarlarla adlı Service Bus bir konu oluşturmak için yöntemini kullanır `mytopic` :

```python
bus_service.create_topic('mytopic')
```

İleti yaşam süresi (TTL) veya en büyük konu boyutu gibi varsayılan konu ayarlarını geçersiz kılmak için konu seçeneklerini kullanabilirsiniz. Aşağıdaki örnek, `mytopic` en fazla 5 GB konu boyutu ve bir dakikalık varsayılan ILETI TTL 'si ile adlı bir konu oluşturur:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonelik oluşturma

Ayrıca, konularla ilgili abonelikler oluşturmak için **Servicebusservice** nesnesini de kullanabilirsiniz. Abonelik, sanal kuyruğuna teslim edilen ileti kümesini kısıtlamak için bir filtreye sahip olabilir. Filtre belirtmezseniz yeni abonelikler, konuya yayınlanan tüm iletileri aboneliğin sanal kuyruğuna yerleştiren varsayılan **Matchall** filtresini kullanır. Aşağıdaki örnek, `mytopic` `AllMessages` **Matchall** filtresini kullanan adlandırılmış bir abonelik oluşturur:

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="use-filters-with-subscriptions"></a>Abonelikler ile filtreleri kullanma

`create_rule`Bir abonelikte görünen iletileri filtrelemek Için **Servicebusservice** nesnesinin yöntemini kullanın. Abonelik oluştururken kuralları belirtebilir veya mevcut aboneliklere kurallar ekleyebilirsiniz.

En esnek filtre türü, bir SQL-92 alt kümesini kullanan bir **Sqlfilter**. SQL filtreleri, konusunda yayınlanan iletilerin özelliklerine göre çalışır. Bir SQL filtresiyle kullanabileceğiniz ifadeler hakkında daha fazla bilgi için, bkz. [sqlfilter. SqlExpression][SqlFilter.SqlExpression] sözdizimi.

Tüm yeni abonelikler için **Matchall** varsayılan filtresi otomatik olarak uygulandığı için, filtre uygulamak istediğiniz aboneliklerden kaldırmanız gerekir ya da **Matchall** , belirttiğiniz diğer filtreleri geçersiz kılar. `delete_rule` **Servicebusservice** nesnesinin yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz.

Aşağıdaki örnek adlı bir `mytopic` `HighMessages` **sqlfilter** kuralıyla adlandırılmış bir abonelik oluşturur `HighMessageFilter` . `HighMessageFilter`Kural yalnızca özel bir `messageposition` özelliği 3 ' ten büyük olan iletileri seçer:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messageposition > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Aşağıdaki örnek adlı bir `mytopic` `LowMessages` **sqlfilter** kuralıyla adlandırılmış bir abonelik oluşturur `LowMessageFilter` . `LowMessageFilter`Kural yalnızca bir `messageposition` özelliği 3 ' ten küçük veya buna eşit olan iletileri seçer:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messageposition <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

`AllMessages`, `HighMessages` Ve `LowMessages` Tüm etkilerden, öğesine gönderilen iletiler `mytopic` her zaman aboneliğin alıcılarına dağıtılır `AllMessages` . İletiler, `HighMessages` `LowMessages` iletinin özellik değerine bağlı olarak ya da aboneliğine seçmeli olarak dağıtılır `messageposition` . 

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme

Uygulamalar, `send_topic_message` Service Bus bir konuya ileti göndermek Için **Servicebusservice** nesnesinin yöntemini kullanır.

Aşağıdaki örnek konuya beş test iletisi gönderir `mytopic` . Özel `messageposition` özellik değeri, döngünün yinelemesine bağlıdır ve iletileri hangi aboneliklerin alacağını belirler. 

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'),
                  custom_properties={'messageposition': i})
    bus_service.send_topic_message('mytopic', msg)
```

### <a name="message-size-limits-and-quotas"></a>İleti boyutu sınırları ve kotaları

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konunun tutamadıkları ileti sayısında bir sınır yoktur, ancak konunun tuttuğu mesajların toplam boyutunun bir üst sınırı vardır. Konu boyutunu oluşturma sırasında, 5 GB üst sınırı ile tanımlayabilirsiniz. 

Kotalar hakkında daha fazla bilgi için bkz. [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Bir abonelikten ileti alma

Uygulamalar, `receive_subscription_message` bir abonelikten ileti almak Için **Servicebusservice** nesnesinde yöntemini kullanır. Aşağıdaki örnek, abonelikten gelen iletileri alır `LowMessages` ve bunları okuduklarında siler:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

İsteğe bağlı `peek_lock` parametresi, `receive_subscription_message` Service Bus, okuma sırasında aboneliğin iletileri silip silmeyeceğini belirler. İleti alma için varsayılan mod *PeekLock*' dir ya da `peek_lock` **doğru**olarak ayarlanır; Bu, iletileri abonelikten silmeksizin okur (göz atar) ve kilitler. Bu durumda, her bir ileti aboneliğin kaldırılması için açıkça tamamlanmış olmalıdır.

Abonelikten gelen iletileri okurken silmek için, `peek_lock` Önceki örnekte olduğu gibi parametresini **false**olarak ayarlayabilirsiniz. İletileri alma işleminin bir parçası olarak silmek en basit modeldir ve bir hata oluşursa uygulamanın eksik iletileri kabul edebildiği durumlarda iyi sonuç verir. Bu davranışı anlamak için, uygulamanın bir alma isteği yaptığı ve işlemeden önce çöktüğü bir senaryo düşünün. İleti alındığı sırada silinirse, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenmeden önce aldığı iletiyi kaçırmıştır.

Uygulamanız eksik iletileri kabul edemediği takdirde, alma iki aşamalı bir işlem haline gelir. PeekLock, tüketilen bir sonraki iletiyi bulur, diğer tüketicilerin bunu almasını engellemek için onu kilitler ve uygulamaya döndürür. İleti işlendikten veya depolandıktan sonra uygulama, `complete` **ileti** nesnesine yöntemini çağırarak alma işleminin ikinci aşamasını tamamlar.  `complete`Yöntemi iletiyi tüketildiği gibi işaretler ve abonelikten kaldırır.

Aşağıdaki örnek, bir Peek kilit senaryosunu göstermektedir:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
    print(msg.body)
    msg.complete()
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Uygulama kilitlenmelerini ve okunamaz iletileri işle

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulama bir nedenden dolayı bir iletiyi işleyebilirse, `unlock` **ileti** nesnesinde yöntemi çağırabilir. Service Bus aboneliğin içindeki iletinin kilidini açar ve aynı ya da başka bir tüketen uygulama tarafından yeniden alınmak üzere kullanılabilir hale gelir.

Ayrıca, abonelik içinde kilitlenen iletiler için bir zaman aşımı vardır. Bir uygulama, kilit zaman aşımı dolmadan önce bir iletiyi işleyemezse, örneğin, uygulama kilitlenirse, Service Bus otomatik olarak iletinin kilidini açar ve tekrar alınabilmesini sağlar.

Bir uygulama bir ileti işlendikten sonra, ancak yöntemi çağrılmadan önce kilitlenirse `complete` , ileti yeniden başlatıldığında uygulamaya yeniden gönderilir. Bu davranış genellikle *en az bir kez işleme*olarak adlandırılır. Her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryonuz yinelenen işleme kabul edememesi durumunda, yinelenen ileti teslimini işlemek için teslim denemelerinde sabit olarak kalan ileti **MessageID** özelliğini kullanabilirsiniz. 

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme

Konuları ve abonelikleri silmek için [Azure Portal][Azure portal] veya `delete_topic` metodunu kullanın. Aşağıdaki kod, adlı konuyu siler `mytopic` :

```python
bus_service.delete_topic('mytopic')
```

Konu başlığı silindiğinde, konu başlığı altındaki tüm abonelikler silinir. Ayrıca, abonelikleri bağımsız olarak da silebilirsiniz. Aşağıdaki kod, konusunun adlı aboneliği siler `HighMessages` `mytopic` :

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

Varsayılan olarak, konular ve abonelikler kalıcı ve siz silene kadar mevcut. Belirli bir süre geçtikten sonra abonelikleri otomatik olarak silmek için, abonelikte [auto_delete_on_idle](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python) parametresini ayarlayabilirsiniz. 

> [!TIP]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, bir Service Bus ad alanına bağlanmanızı ve mesajlaşma varlıklarını kolayca yönetmenizi sağlar. Araç içeri/dışarı aktarma işlevselliği ve konuları, kuyrukları, abonelikleri, geçiş Hizmetleri, Bildirim Hub 'larını ve Olay Hub 'larını test etme özelliği gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Service Bus konuların temellerini öğrendiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions]
* [Sqlfilter. SqlExpression][SqlFilter.SqlExpression] başvurusu

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
