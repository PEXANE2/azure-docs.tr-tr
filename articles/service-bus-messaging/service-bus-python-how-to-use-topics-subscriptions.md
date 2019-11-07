---
title: 'Hızlı başlangıç: Python ile Azure Service Bus konuları kullanma'
description: "Hızlı başlangıç: Python 'dan Azure Service Bus konuları ve abonelikleri kullanmayı öğrenin."
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: aschhab
ms.openlocfilehash: 8f7d47879a025742dbca6a5cafa634899e60ee68
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73719170"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Hızlı başlangıç: Python ile Service Bus konuları ve abonelikleri kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makale, Service Bus konu başlıklarını ve aboneliklerini kullanmayı açıklar. Örnekler Python 'da yazılır ve [Azure Python SDK paketini][Azure Python package]kullanır. Kapsanan senaryolar şunlardır:

- Konular ve abonelikler oluşturma 
- Abonelik filtreleri oluşturma 
- Konuya ileti gönderme 
- Bir abonelikten ileti alma
- Konuları ve abonelikleri silme

## <a name="prerequisites"></a>Ön koşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [Visual Studio veya MSDN abonesi avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
2. Hızlı başlangıç içindeki adımları izleyin: bir Service Bus **ad alanı** oluşturmak ve **bağlantı dizesini**almak için [Azure Portal Service Bus konu başlığı ve abonelikleri oluşturmak üzere kullanın](service-bus-quickstart-topics-subscriptions-portal.md) .

    > [!NOTE]
    > Bu hızlı başlangıçta **Python** kullanarak konu için bir **Konu** ve bir **abonelik** oluşturacaksınız. 
3. [Azure Python paketini][Azure Python package]yükler. Bkz. [Python Yükleme Kılavuzu](/azure/python/python-sdk-azure-install).

## <a name="create-a-topic"></a>Konu başlığı oluşturma

**Servicebusservice** nesnesi konularda çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek istediğiniz herhangi bir Python dosyasının üst tarafına yakın bir şekilde aşağıdaki kodu ekleyin:

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Aşağıdaki kod bir **Servicebusservice** nesnesi oluşturur. `mynamespace`, `sharedaccesskeyname`ve `sharedaccesskey` gerçek ad alanınız, paylaşılan erişim Imzası (SAS) anahtar adı ve anahtar değeri ile değiştirin.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

SAS anahtar adı ve değerinin değerlerini [Azure Portal][Azure portal]elde edebilirsiniz.

```python
bus_service.create_topic('mytopic')
```

`create_topic` yöntemi, ileti Süresi canlı veya en büyük konu boyutu gibi varsayılan konu ayarlarını geçersiz kılmanızı sağlayan ek seçenekleri de destekler. Aşağıdaki örnek, en büyük konu boyutunu 5 GB ve bir dakikalık yaşam süresi (TTL) değeri olarak ayarlar:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonelik oluşturma

Konuya abonelikler **Servicebusservice** nesnesiyle de oluşturulur. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim edilen ileti kümesini kısıtlayan isteğe bağlı bir filtreye sahip olabilir.

> [!NOTE]
> Varsayılan olarak, abonelikler kalıcıdır ve bunlar ya da abone oldukları konu silinene kadar var olmaya devam eder.
> 
> [Auto_delete_on_idle özelliğini](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python)ayarlayarak aboneliklerin otomatik olarak silinmesini sağlayabilirsiniz.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma

Yeni bir abonelik oluşturulduğunda bir filtre belirtilmemişse, **Matchall** filtresi (varsayılan) kullanılır. **Matchall** filtresi kullanıldığında, konuya yayınlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek, `AllMessages` adlı bir abonelik oluşturur ve varsayılan **Matchall** filtresini kullanır.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma

Ayrıca, bir konuya gönderilen iletilerin belirli bir konu aboneliği içinde gösterilmesi gerektiğini belirtmenize imkan tanıyan filtreler de tanımlayabilirsiniz.

Abonelikler tarafından desteklenen en esnek filtre türü, SQL92 alt kümesini uygulayan bir **sqlfilter**. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL filtresi ile kullanılabilen ifadeler hakkında daha fazla bilgi edinmek için [SqlFilter.SqlExpression][SqlFilter.SqlExpression] söz dizimine bakın.

**Servicebusservice** nesnesinin **Create\_Rule** metodunu kullanarak bir aboneliğe filtre ekleyebilirsiniz. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenize olanak tanır.

> [!NOTE]
> Varsayılan filtre tüm yeni aboneliklere otomatik olarak uygulandığından, ilk olarak varsayılan filtreyi kaldırmanız gerekir veya **Matchall** , belirtebileceğiniz diğer filtreleri geçersiz kılar. **Servicebusservice** nesnesinin `delete_rule` yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz.
> 
> 

Aşağıdaki örnekte, yalnızca 3 ' ten büyük özel bir `messagenumber` özelliğine sahip iletileri seçen **Sqlfilter** ile `HighMessages` adlı bir abonelik oluşturulur:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Benzer şekilde, aşağıdaki örnekte, yalnızca 3 ' ten küçük veya buna eşit bir `messagenumber` özelliğine sahip iletileri seçen **Sqlfilter** ile `LowMessages` adlı bir abonelik oluşturulur:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Artık `mytopic` bir ileti gönderildiğinde, her zaman **allmessages** konu aboneliğine abone olan alıcılar ' e dağıtılır ve **Highmessages** ve **lowmessages** konusuna abone olunan alıcılar için seçmeli olarak dağıtılır Abonelikler (ileti içeriğine bağlı olarak).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme

Service Bus bir konuya ileti göndermek için uygulamanızın **Servicebusservice** nesnesinin `send_topic_message` yöntemini kullanması gerekir.

Aşağıdaki örnek, `mytopic`beş test iletisinin nasıl gönderileceğini gösterir. Her iletinin `messagenumber` Özellik değeri, döngünün yinelemesinde farklılık gösterir (Bu, hangi aboneliklerin bunu alacağını belirler):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'),
                  custom_properties={'messagenumber': i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz. [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Bir abonelikten ileti alma

**Servicebusservice** nesnesindeki `receive_subscription_message` yöntemi kullanılarak bir abonelikten iletiler alınır:

```python
msg = bus_service.receive_subscription_message(
    'mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

`peek_lock` parametresi **false**olarak ayarlandığında okunan iletiler, abonelikten silinir. `peek_lock` parametresini **true**olarak ayarlayarak iletiyi, kuyruktan silmeden bir şekilde okuyabilir (Peek) ve kilitleyebilir.

İletiyi alma işleminin bir parçası olarak okuma ve silme davranışı en basit modeldir ve bir hata olduğunda uygulamanın bir iletiyi işlememesinin kabul edebildiği senaryolar için en iyi sonuç verir. Bu davranışı anlamak için, tüketicinin alma isteğini yaptığı ve işlemeden önce çöktüğü bir senaryo düşünün. Service Bus ileti tüketildiği gibi işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenme öncesinde tüketilen iletiyi kaçırmıştır.

`peek_lock` parametresi **true**olarak ayarlanırsa, Receive iki aşamalı bir işlem olur ve bu da eksik iletilere izin verilmeyen uygulamaları desteklemeyi olanaklı kılar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan (veya gelecekteki işlemler için güvenilir bir şekilde depolar), **ileti** nesnesine `delete` yöntemi çağırarak alma işleminin ikinci aşamasını tamamlar. `delete` yöntemi iletiyi tüketildiği gibi işaretler ve abonelikten kaldırır.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
print(msg.body)
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulamasının iletiyi bazı nedenlerle işleyemeyeceği takdirde **ileti** nesnesinde `unlock` yöntemi çağırabilir. Bu yöntem Service Bus, aboneliğin içindeki iletinin kilidini açma ve bu uygulamayı aynı uygulama veya başka bir uygulama tarafından yeniden alınmak üzere kullanılabilir hale getirmesine neden olur.

Ayrıca, abonelik içinde kilitlenen bir iletiyle ilişkili bir zaman aşımı vardır. uygulama, kilit zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama çökerse), Service Bus otomatik olarak iletinin kilidini açar ve yeniden alınmak üzere kullanılabilir hale getirir.

İleti işlendikten sonra, ancak `delete` yöntemi çağrılmadan önce uygulamanın çöktüğü durumda, yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu davranış genellikle çağırılır. En az bir kez Işleme\*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bunu yapmak için, iletinin **MessageID** özelliğini kullanabilirsiniz. Bu, teslim girişimleri arasında sabitde kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme

[Auto_delete_on_idle özelliği](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python) ayarlanmadığı takdirde konular ve abonelikler kalıcıdır. [Azure Portal][Azure portal] veya program aracılığıyla silinebilirler. Aşağıdaki örnek, `mytopic`adlı konunun nasıl silineceğini göstermektedir:

```python
bus_service.delete_topic('mytopic')
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki kod, `mytopic` konusunun `HighMessages` adlı bir aboneliğin nasıl silineceğini gösterir:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Service Bus konuların temellerini öğrendiğinize göre, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* Bkz. [Kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions].
* [Sqlfilter. SqlExpression][SqlFilter.SqlExpression]için başvuru.

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
