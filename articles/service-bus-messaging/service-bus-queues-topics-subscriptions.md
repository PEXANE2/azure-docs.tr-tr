---
title: Azure Service Bus mesajlaşma-kuyruklar, konular ve abonelikler
description: Bu makalede, Azure Service Bus mesajlaşma varlıklarına (kuyruk, konular ve abonelikler) genel bir bakış sunulmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: deeebf56d6e2f4ccfac37c70170a0d1cb4d272a9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119183"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus kuyrukları, konu başlıkları ve abonelikleri

Microsoft Azure Service Bus, güvenilir Message Queuing ve dayanıklı yayımlama/abone olma iletileri dahil olmak üzere bulut tabanlı, ileti yönelimli bir ara yazılım teknolojilerini destekler. Bu "aracılı" mesajlaşma özellikleri, Service Bus mesajlaşma iş yükünü kullanarak yayımlama-abone olma, zamana bağlı ayırma ve yük dengeleme senaryolarını destekleyen ayrılmış mesajlaşma özellikleri olarak düşünülebilir. Ayrılmış iletişim birçok avantaj sunar. Örneğin, sunucular ve istemciler gerektiğinde bağlantı kurabilir ve kendi işlemlerini zaman uyumsuz olarak gerçekleştirebilir.

Service Bus 'deki mesajlaşma yeteneklerinin çekirdeğini oluşturan mesajlaşma varlıkları kuyruklar, konular ve abonelikler ve kurallar/eylemlerdir.

## <a name="queues"></a>Kuyruklar

Kuyruklar, bir veya daha fazla rakip tüketiciye *ilk olarak, Ilk çıkar* (FIFO) ileti teslimi sunar. Diğer bir deyişle alıcılar, iletileri sıraya eklendikleri sırayla alır ve işler ve her iletiyi yalnızca bir ileti tüketicisini alır ve işler. Kuyrukları kullanmanın önemli bir avantajı, uygulama bileşenlerinin "zamana bağlı ayrışmasıyla" elde etmenizi sağlar. Diğer bir deyişle, üreticileri (göndericiler) ve tüketicilerin (Alıcılar) iletileri kuyrukta zaman içinde depolandığından, aynı anda ileti göndermesi ve alması gerekmez. Ayrıca, üreticinin iletileri işlemeye ve gönderilmeye devam edebilmesi için tüketiciden bir Yanıt beklemesi gerekmez.

İlgili bir avantaj, üreticileri ve tüketicilerin farklı oranlarda ileti göndermesini ve almasını sağlayan "Yük Dengeleme" ' dir. Birçok uygulamada, sistem yükü zamana göre değişir; Ancak, her iş birimi için gereken işleme süresi genellikle sabittir. Aracılığıyla Message üreticileri ve bir kuyruğu olan tüketiciler, tüketen uygulamanın yalnızca en yoğun yük yerine ortalama yükü işleyebilmek için sağlanması gereken anlamına gelir. Gelen yük hacmi değiştikçe kuyruğun derinliği artar ve daralır. Bu özellik, uygulama yüküne hizmet vermek için gereken altyapı miktarına göre doğrudan para tasarrufu sağlar. Yük arttıkça, kuyruktan okumak için daha fazla çalışan işlemi eklenebilir. Her ileti yalnızca bir çalışan işlemi tarafından işlenir. Ayrıca, bu çekme tabanlı yük dengeleme, çalışan bilgisayarların, kendi en yüksek ücretlerinden ileti çektikleri sırada çalışan bilgisayarların en iyi şekilde kullanılmasına olanak tanır. Bu desen genellikle "rekabet eden tüketici" deseninin altına kaydedilir.

İleti üreticileri ve tüketiciler arasında ara için kuyrukları kullanmak, bileşenler arasında devralınan bir gevşek bağ sağlar. Üreticileri ve tüketiciler birbirleriyle uyumlu olmadığından, bir tüketici üreticisi üzerinde herhangi bir etkiye gerek kalmadan yükseltilebilir.

### <a name="create-queues"></a>Kuyruklar Oluşturma

[Azure Portal](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md)veya [Kaynak Yöneticisi şablonlarını](service-bus-resource-manager-namespace-queue.md)kullanarak kuyruklar oluşturursunuz. Daha sonra bir [Queueclient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesnesi kullanarak ileti gönderir ve alırsınız.

Sıra oluşturmayı ve kuyruktan ileti gönderme ve alma işlemlerinin nasıl yapılacağını hızlı bir şekilde öğrenmek için her yöntemin [hızlı](service-bus-quickstart-portal.md) başlangıçlarına bakın. Kuyrukları kullanma hakkında daha ayrıntılı bir öğretici için bkz. [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md).

Çalışan bir örnek için GitHub 'da [Basicsendreceiveusingqueueclient örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) bakın.

### <a name="receive-modes"></a>Alma modları

Service Bus iletileri aldığı iki farklı mod belirtebilirsiniz: *Receiveanddelete* veya *PeekLock*. [Receiveanddelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda alma işlemi tek atışı olur; diğer bir deyişle, Service Bus tüketiciden aldığında, iletiyi tüketildiği gibi işaretler ve tüketici uygulamasına geri döndürür. **Receiveanddelete** modu, en basit modeldir ve bir hata oluşursa uygulamanın bir iletiyi işlememesi için kabul edebildiği senaryolar için en iyi şekilde kullanılır. Bu senaryoyu anlamak için, tüketicinin alma isteğini yaptığı ve işlemeden önce çöktüğü bir senaryo düşünün. Service Bus ileti tüketildiği gibi işaretlediği için, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenme öncesinde tüketilen ileti kaçırılmış olur.

[PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda alma işlemi iki aşamalı hale gelir ve bu da eksik iletilere izin verilmeyen uygulamaları desteklemeyi olanaklı kılar. Service Bus isteği aldığında, kullanılacak sonraki iletiyi bulur, diğer tüketicilerin bunu almasını engellemek için onu kilitler ve sonra uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan (veya gelecekteki işlemler için güvenilir bir şekilde depolar), alınan ileti üzerinde tamamlanan [ekip eşitlemesini](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) çağırarak alma işleminin ikinci aşamasını tamamlar. Service Bus, **tamamlanmakta olan eşitleme** çağrısını gördüğünde, iletiyi tüketildiği gibi işaretler.

Uygulama herhangi bir nedenden dolayı iletiyi işleyemeyebilir, alınan iletide [terk zaman uyumsuz](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) yöntemini çağırabilir (yerine, [tamamlana](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Bu yöntem, Service Bus iletinin kilidini açmak ve aynı tüketici ya da başka bir rekabet tüketicisi tarafından yeniden alınmalarını sağlar. İkinci olarak, kilit ile ilişkili bir zaman aşımı vardır ve uygulamanın kilit zaman aşımı süresi dolmadan önce iletiyi işleyemezse (örneğin, uygulama çökerse), Service Bus iletinin kilidini açar ve yeniden alınmak üzere kullanılabilir hale gelir (aslında varsayılan olarak bir [terk zaman uyumsuz](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) işlem gerçekleştirir).

İleti işlendikten sonra uygulamanın çöktüğü durumda, ancak **tamamlanılan eşitleme** isteği verilmeden önce, yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu işlem genellikle *en az bir kez* işleme olarak adlandırılır; diğer bir deyişle, her ileti en az bir kez işlenir. Ancak, bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme kabul edebiliyorsa, yinelenen verileri algılamak için uygulamada ek mantık gerekir ve bu, teslim girişimleri arasında sabit kalacak şekilde iletinin [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid) özelliğine dayalı olarak elde edilebilir. Bu özellik *tam olarak bir kez* işlenirken bilinir.

## <a name="topics-and-subscriptions"></a>Konular ve abonelikler

Her iletinin tek bir tüketici tarafından işlendiği kuyrukların aksine, *konular* ve *abonelikler* , bir *Yayımla/abone ol* düzeninde bire çok bir iletişim biçimi sağlar. Çok sayıda alıcıyı ölçeklendirmek için faydalı olan her yayımlanan ileti, konuya kayıtlı her abonelik için kullanılabilir hale getirilir. Mesajlar bir konuya gönderilir ve abonelik başına ayarlanabilir filtre kurallarına bağlı olarak ilişkili bir veya daha fazla aboneliğe dağıtılır. Abonelikler, almak istedikleri iletileri kısıtlamak için ek filtreler kullanabilir. Mesajlar bir sıraya gönderildikleri gibi bir konuya gönderilir, ancak doğrudan bu konudan iletiler alınmaz. Bunun yerine, aboneliklerden alınır. Konu aboneliği, konuya gönderilen iletilerin kopyalarını alan bir sanal kuyruğa benzer. İletiler bir abonelikten bir kuyruktan alındıkları yönteme aynı şekilde alınır.

Karşılaştırma yöntemiyle, bir sıranın ileti gönderme işlevi doğrudan bir konuya eşlenir ve ileti alma işlevselliği bir abonelikle eşlenir. Diğer şeyler arasında bu özellik, aboneliklerin bu bölümde daha önce açıklanan desenleri desteklediği, kuyruklar: rekabet eden tüketici, zamana bağlı ayırma, yük dengeleme ve yük dengeleme anlamına gelir.

### <a name="create-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri oluşturma

Bir konu oluşturmak, önceki bölümde açıklandığı gibi kuyruk oluşturmaya benzer. Daha sonra [Topicclient](/dotnet/api/microsoft.azure.servicebus.topicclient) sınıfını kullanarak iletiler gönderirsiniz. İleti almak için, konusunda bir veya daha fazla abonelik oluşturursunuz. Sıralara benzer şekilde, iletiler bir [Queueclient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesnesi yerine bir [subscriptionclient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) nesnesi kullanılarak bir abonelikten alınır. Abonelik istemcisini oluşturun, konunun adını, aboneliğin adını ve (isteğe bağlı olarak) alma modunu parametre olarak geçirerek.

Tam çalışma örneği için GitHub 'daki [Basicsendreceiveusingtopicsubscriptionclient örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) bakın.

### <a name="rules-and-actions"></a>Kurallar ve eylemler

Birçok senaryoda, belirli özelliklere sahip olan iletiler farklı yollarla işlenmelidir. Bu işlemeyi etkinleştirmek için, istediğiniz özelliklere sahip iletileri bulmak ve ardından bu özelliklerde belirli değişiklikler yapmak üzere abonelikleri yapılandırabilirsiniz. Service Bus abonelikler konuya gönderilen tüm iletileri görirken, bu iletilerin bir alt kümesini yalnızca sanal abonelik kuyruğuna kopyalayabilirsiniz. Bu filtreleme, abonelik filtreleri kullanılarak gerçekleştirilir. Bu değişiklikler, *filtre eylemleri*olarak adlandırılır. Bir abonelik oluşturulduğunda, iletinin özelliklerinde, hem sistem özellikleri (örneğin, **etiket**) hem de özel uygulama özellikleri (örneğin, **StoreName**) üzerinde çalışan bir filtre ifadesi sağlayabilirsiniz. SQL filtresi ifadesi bu durumda isteğe bağlıdır; bir SQL filtresi ifadesi olmadan, bir abonelikte tanımlı herhangi bir filtre eylemi bu aboneliğin tüm iletilerinde gerçekleştirilir.

Tam çalışma örneği için GitHub 'da [Topicsubscriptionwithruleoperationssample örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) bakın.

Olası filtre değerleri hakkında daha fazla bilgi için [Sqlfilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) ve [sqlruleaction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) sınıfları belgelerine bakın.

## <a name="java-message-service-jms-20-entities-preview"></a>Java ileti hizmeti (JMS) 2,0 varlıkları (Önizleme)

Azure Service Bus Premium 'a bağlanan ve [JMS kitaplığı Azure Service Bus](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms) kullanan istemci uygulamalar aşağıdaki varlıkların faydalanabilir.

### <a name="queues"></a>Kuyruklar

JMS 'deki kuyruklar, yukarıda açıklanan geleneksel Service Bus kuyruklarında anlam ile karşılaştırılabilir.

Bir kuyruk oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
Queue createQueue(String queueName)
```

### <a name="topics"></a>Konu başlıkları

JMS 'deki konular, yukarıda açıklanan geleneksel Service Bus konularıyla anlam ile karşılaştırılabilir.

Bir konu oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
Topic createTopic(String topicName)
```

### <a name="temporary-queues"></a>Geçici kuyruklar

Bir istemci uygulaması, uygulamanın ömrü için mevcut olan geçici bir varlık gerektirdiğinde geçici kuyrukları kullanabilir. Bunlar, [istek-yanıt](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) modelinde kullanılır.

Geçici bir sıra oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
TemporaryQueue createTemporaryQueue()
```

### <a name="temporary-topics"></a>Geçici konular

Geçici kuyruklarda olduğu gibi, uygulama ömrü için mevcut olan geçici bir varlık aracılığıyla yayımla/abone ol ' u etkinleştirmek için geçici konular vardır.

Geçici bir konu oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
TemporaryTopic createTemporaryTopic()
```

### <a name="java-message-service-jms-subscriptions"></a>Java ileti hizmeti (JMS) abonelikleri

Bunlar, yukarıda açıklanan aboneliklere (örneğin, bir konu üzerinde mevcuttur ve yayımlama/abone olma semantiğini etkinleştirir) benzer şekilde, Java Ileti hizmeti özelliği belirli bir abonelik için **paylaşılan**, **paylaşılmayan**, **dayanıklı** ve **dayanıklı olmayan** özniteliklerin kavramlarını tanıtır.

> [!NOTE]
> Aşağıdaki abonelikler, [Azure Service Bus JMS kitaplığı](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms)kullanarak Azure Service Bus bağlanan Istemci uygulamalarının önizlemesi Için Azure Service Bus Premium katmanında kullanılabilir.
>
> Genel önizleme için, bu abonelikler Azure portal kullanılarak oluşturulamaz.
>

#### <a name="shared-durable-subscriptions"></a>Paylaşılan dayanıklı abonelikler

Paylaşılan dayanıklı bir abonelik, bir konu üzerinde yayınlanan tüm iletiler, uygulamanın abonelikten her zaman etkin bir şekilde kullanılıp kullanılmadığına bakılmaksızın bir uygulama tarafından alınıp işleneceğinden kullanılır.

Bu paylaşılan bir abonelik olduğundan, Service Bus içinden alma kimliği doğrulanmış tüm uygulamalar abonelikten alabilir.

Paylaşılan dayanıklı bir abonelik oluşturmak için sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
JMSConsumer createSharedDurableConsumer(Topic topic, String name)

JMSConsumer createSharedDurableConsumer(Topic topic, String name, String messageSelector)
```

Paylaşılan dayanıklı abonelik, sınıfında yöntemi kullanılarak silinmedikleri takdirde mevcut olmaya devam eder `unsubscribe` `JMSContext` .

```java
void unsubscribe(String name)
```

#### <a name="unshared-durable-subscriptions"></a>Paylaşılmayan dayanıklı abonelikler

Paylaşılan dayanıklı bir abonelikte olduğu gibi, bir konu üzerinde yayınlanan tüm iletiler, uygulamanın aboneliğin her zaman etkin bir şekilde tüketilip alınmadığına bakılmaksızın, bir konu üzerinde yayımlanan tüm iletiler bir uygulama tarafından alınıp işlenecektir.

Ancak, bu paylaşılmayan bir abonelik olduğundan, yalnızca aboneliği oluşturan uygulama bundan alabilir.

Paylaşılmayan dayanıklı bir abonelik oluşturmak için, sınıfından aşağıdaki yöntemleri kullanın `JMSContext` - 

```java
JMSConsumer createDurableConsumer(Topic topic, String name)

JMSConsumer createDurableConsumer(Topic topic, String name, String messageSelector, boolean noLocal)
```

> [!NOTE]
> `noLocal`Özellik şu anda desteklenmiyor ve yoksayıldı.
>

Paylaşılmayan dayanıklı abonelik, sınıfında yöntemi kullanılarak silinmedikleri takdirde mevcut olmaya devam eder `unsubscribe` `JMSContext` .

```java
void unsubscribe(String name)
```

#### <a name="shared-non-durable-subscriptions"></a>Paylaşılan, dayanıklı olmayan abonelikler

Birden çok istemci uygulamanın iletileri tek bir abonelikten alması ve işlemesi gerektiğinde, yalnızca bu bilgisayardan etkin bir şekilde tüketerek/alınana kadar, paylaşılan, dayanıklı olmayan bir abonelik kullanılır.

Abonelik dayanıklı olmadığından kalıcı değil. Üzerinde etkin bir tüketici olmadığında bu abonelik tarafından ileti alınmaz.

Paylaşılan, dayanıklı olmayan bir abonelik oluşturmak için, `JmsConsumer` sınıfında aşağıdaki yöntemlerle gösterildiği gibi bir oluşturun `JMSContext` -

```java
JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName)

JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName, String messageSelector)
```

Paylaşılan dayanıklı olmayan abonelik, bundan alınan etkin tüketiciler olana kadar mevcut olmaya devam eder.

#### <a name="unshared-non-durable-subscriptions"></a>Paylaşılmayan kalıcı olmayan abonelikler

Yönetici olmayan bir abonelik, istemci uygulamanın iletiyi bir abonelikten alması ve işlemesi gerektiğinde, yalnızca onu etkin bir şekilde tüketene kadar kullanılır. Bu abonelikte yalnızca bir tüketici bulunabilir, yani aboneliği oluşturan istemci.

Abonelik dayanıklı olmadığından kalıcı değil. Üzerinde etkin bir tüketici olmadığında bu abonelik tarafından ileti alınmaz.

Paylaşılmayan dayanıklı olmayan bir abonelik oluşturmak için, `JMSConsumer` ' JMSContext sınıfından aşağıdaki yöntemlerde gösterildiği gibi bir oluşturun. 

```java
JMSConsumer createConsumer(Destination destination)

JMSConsumer createConsumer(Destination destination, String messageSelector)

JMSConsumer createConsumer(Destination destination, String messageSelector, boolean noLocal)
```

> [!NOTE]
> `noLocal`Özellik şu anda desteklenmiyor ve yoksayıldı.
>

Paylaşılmayan dayanıklı olmayan abonelik, bundan sonra gelen etkin bir tüketici olana kadar mevcut olmaya devam eder.

#### <a name="message-selectors"></a>İleti seçicileri

Normal Service Bus abonelikler için **filtreler ve eylemler** gıbı, JMS abonelikleri Için de **ileti seçicileri** vardır.

İleti seçicileri her JMS aboneliğine ayarlanabilir ve ileti üst bilgisi özelliklerinde bir filtre koşulu olarak mevcuttur. Yalnızca ileti seçici ifadesiyle eşleşen üst bilgi özelliklerine sahip iletiler sunulur. Null veya boş bir dize değeri, JMS aboneliği/tüketicisi için bir ileti seçici olmadığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma kullanma hakkında daha fazla bilgi ve örnekler için aşağıdaki gelişmiş konulara bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Hızlı başlangıç: Azure portalı ve .NET kullanarak iletileri gönderme ve alma](service-bus-quickstart-portal.md)
* [Öğretici: Azure portalı ve konuları/abonelikleri kullanarak envanter güncelleştirme](service-bus-tutorial-topics-subscriptions-portal.md)


