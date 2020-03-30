---
title: Azure Servis Veri Servisi mesajlaşma - kuyruklar, konular ve abonelikler
description: Bu makalede, Azure Servis Veri Servisi ileti varlıklarına (sıra, konular ve abonelikler) genel bir bakış sunulmaktadır.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/16/2020
ms.author: aschhab
ms.openlocfilehash: 3dc78a22e0e596d812d90fec63475a0b21e9164f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259519"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus kuyrukları, konu başlıkları ve abonelikleri

Microsoft Azure Hizmet Veri Servisi, güvenilir ileti sıraya alma ve dayanıklı yayımlama/abone mesajlaşması da dahil olmak üzere bulut tabanlı, ileti odaklı ara yazılım teknolojilerini destekler. Bu "aracılı" mesajlaşma özellikleri, Hizmet Veri Aracı mesajlaşma iş yükünü kullanarak yayımlama-abone olma, zamansal ayrıştırma ve yük dengeleme senaryolarını destekleyen ayrılmış ileti özellikleri olarak düşünülebilir. Ayrılmış iletişim birçok avantaj sunar. Örneğin, sunucular ve istemciler gerektiğinde bağlantı kurabilir ve kendi işlemlerini zaman uyumsuz olarak gerçekleştirebilir.

Hizmet Veri Tos'taki ileti özelliklerinin çekirdeğini oluşturan ileti varlıkları kuyruklar, konular ve abonelikler ve kurallar/eylemlerdir.

## <a name="queues"></a>Kuyruklar

Kuyruklar, bir veya daha fazla rakip tüketiciye *First In, First Out* (FIFO) ileti teslimi sunar. Diğer bir deyişle, alıcılar genellikle sıraya eklendikleri sırada iletileri alır ve işler ve yalnızca bir ileti tüketici alır ve her iletiyi işler. Kuyrukları kullanmanın önemli bir yararı, uygulama bileşenlerinin "zamansal ayrışmasını" elde etmektir. Başka bir deyişle, iletiler kuyrukta sürekli depolandığı için, üreticiler (gönderenler) ve tüketiciler (alıcılar) aynı anda mesaj gönderip almak zorunda değildir. Ayrıca, üretici nin işlem ve mesaj göndermeye devam edebilmesi için tüketiciden yanıt beklemesi gerekmez.

İlgili bir avantaj, üreticilerin ve tüketicilerin farklı oranlarda mesaj gönderip almalarını sağlayan "yük seviyeleme"dir. Birçok uygulamada, sistem yükü zaman içinde değişir; ancak, her bir çalışma birimi için gereken işlem süresi genellikle sabittir. İleti üreticileri ve tüketicileri sıraya dahil etmek, tüketen uygulamanın yalnızca en yüksek yük yerine ortalama yükü işleyebilmek için sağlanması gerektiği anlamına gelir. Gelen yük hacmi değiştikçe kuyruğun derinliği artar ve daralır. Bu özellik, uygulama yükünü n için gereken altyapı miktarı yla ilgili olarak doğrudan para tasarrufu sağlar. Yük arttıkça, kuyruktan okumak için daha fazla alt işlem eklenebilir. Her ileti yalnızca bir çalışan işlemi tarafından işlenir. Ayrıca, bu çekme tabanlı yük dengeleme, alt bilgisayarlar kendi maksimum hızlarında iletileri çekmek gibi, işlem gücü açısından farklı olsa bile, işçi bilgisayarların optimum kullanımı için izin verir. Bu desen genellikle "rakip tüketici" deseni olarak adlandırılır.

İleti üreticileri ve tüketiciler arasında ara sıralar kullanarak bileşenleri arasında doğal bir gevşek bağlantı sağlar. Üreticiler ve tüketiciler birbirlerinden haberdar olmadıkları için, tüketici üretici üzerinde herhangi bir etkisi olmadan yükseltilebilir.

### <a name="create-queues"></a>Kuyruklar oluşturma

[Azure portalı](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md)veya [Kaynak Yöneticisi şablonlarını kullanarak kuyruklar oluşturursunuz.](service-bus-resource-manager-namespace-queue.md) Daha sonra bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesnesi kullanarak ileti gönderip alırsınız.

Kuyruk oluşturmayı hızlı bir şekilde öğrenmek için, ardından sıraya ileti gönderip almak için her yöntemin [hızlı başlangıçlarına](service-bus-quickstart-portal.md) bakın. Kuyrukların nasıl kullanılacağı hakkında daha ayrıntılı bir öğretici [için](service-bus-dotnet-get-started-with-queues.md)bkz.

Çalışan bir örnek için GitHub'daki [BasicSendReceiveUsingQueueClient örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) bakın.

### <a name="receive-modes"></a>Alma modları

Service Bus'un ileti aldığı iki farklı mod belirtebilirsiniz: *ReceiveAndDelete* veya *PeekLock*. [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda, alma işlemi tek çekimdir; diğer bir de, Servis Veri Servisi tüketiciden istek aldığında, mesajın tüketildiği gibi işaretler ve tüketici uygulamasına iade edilir. **ReceiveAndDelete** modu en basit modeldir ve bir hata oluşursa uygulamanın bir iletiyi işlememeye tahammül edebileceği senaryolar için en iyi şekilde çalışır. Bu senaryoyu anlamak için, tüketicinin alma isteğini sorunları ve işlemeden önce çöktüğün bir senaryoyu düşünün. Servis Veri Kurumu, iletinin tüketildiği olarak işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden tüketmeye başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmış olur.

[PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda, alma işlemi iki aşamalı olur ve bu da eksik iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılar. Servis Veri Mes'i isteği aldığında, bir sonraki iletinin tüketilmesi gerektiğini bulur, diğer tüketicilerin almasını önlemek için kilitler ve sonra uygulamaya döndürür. Uygulama iletiyi işlemeyi bitirdikten sonra (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) alınan iletide [CompleteAsync'i](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) arayarak alma işleminin ikinci aşamasını tamamlar. Servis Veri **Servisi, CompleteAsync** çağrısını gördüğünde, iletinin tüketildiğini işaretler.

Uygulama iletiyi herhangi bir nedenle işleyemiyorsa, alınan iletide [(CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) [yerine) AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) yöntemini çağırabilir. Bu yöntem, Servis Veri Servisi'nin iletiyi kilidini açmasını ve aynı tüketici veya başka bir rakip tüketici tarafından yeniden alınmak üzere kullanılabilir hale getirmesini sağlar. İkinci olarak, kilitle ilişkili bir zaman aşımı vardır ve uygulama kilit süresi dolmadan önce iletiyi işlemezse (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletiyi açar ve yeniden alınmak üzere kullanılabilir hale getirir (temelde varsayılan olarak bir [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) işlemi gerçekleştirir).

Uygulamanın iletiyi işledikten sonra çökmesi, ancak **TamAsync** isteği verilmeden önce, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu işlem genellikle *En Az Bir Kez* işleme denir; diğer bir tarihte, her ileti en az bir kez işlenir. Ancak, bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme tolere edemiyorsa, iletinin iletinin [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) özelliğine bağlı olarak elde edilebilen ve teslim girişimleri arasında sabit kalan yinelenenleri algılamak için uygulamada ek mantık gerekir. Bu özellik *Tam Olarak Bir kez* işleme olarak bilinir.

## <a name="topics-and-subscriptions"></a>Konular ve abonelikler

Her iletinin tek bir tüketici tarafından işlendiği kuyrukların aksine, *konular* ve *abonelikler* *yayımlama/abone oltasında* bir-çok iletişim biçimi sağlar. Çok sayıda alıcıya ölçekleme için kullanışlı olan, yayınlanan her ileti, konuyla birlikte kayıtlı her abonelik için kullanılabilir hale getirilir. İletiler bir konuya gönderilir ve abonelik başına ayarlanabilen filtre kurallarına bağlı olarak bir veya daha fazla ilişkili aboneye teslim edilir. Abonelikler, almak istedikleri iletileri kısıtlamak için ek filtreler kullanabilir. İletiler bir konuya kuyruğa gönderildikleri gibi gönderilir, ancak iletiler doğrudan konudan alınmaz. Bunun yerine, aboneliklerden alınırlar. Konu aboneliği, konuya gönderilen iletilerin kopyalarını alan sanal bir kuyruğa benzer. İletiler, bir abonelikten, kuyruktan alınanlarla aynı şekilde alınır.

Karşılaştırma yolu olarak, bir sıranın ileti gönderme işlevi doğrudan bir konuya eşlenir ve ileti alma işlevi bir abonelikle eşlenir. Diğer şeylerin yanı sıra, bu özellik aboneliklerin kuyruklarla ilgili olarak bu bölümde daha önce açıklanan desenleri desteklediği anlamına gelir: rakip tüketici, zamansal ayrıştırma, yük tesviye ve yük dengeleme.

### <a name="create-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri oluşturma

Bir konu oluşturma, önceki bölümde açıklandığı gibi bir sıra oluşturmaya benzer. Daha sonra [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient) sınıfını kullanarak ileti gönderirsiniz. İletileri almak için konuya bir veya daha fazla abonelik oluşturursunuz. Kuyruklara benzer şekilde, iletiler [bir abonelikten QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesnesi yerine Bir [Abonelikİstemci](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) nesnesi kullanılarak alınır. Konunun adını, aboneliğin adını ve (isteğe bağlı olarak) parametre olarak alma modunu geçen abonelik istemcisini oluşturun.

Tam çalışma örneği için GitHub'daki [BasicSendReceiveUsingTopicSubscriptionClient örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) bakın.

### <a name="rules-and-actions"></a>Kurallar ve eylemler

Birçok senaryoda, belirli özelliklere sahip iletiler farklı şekillerde işlenmelidir. Bu işleme etkinleştirmek için, abonelikleri istenen özelliklere sahip iletileri bulmak için yapılandırabilir ve ardından bu özelliklerde belirli değişiklikler gerçekleştirebilirsiniz. Servis Veri Hizmetleri abonelikleri konuya gönderilen tüm iletileri görse de, bu iletilerin bir alt kümesini yalnızca sanal abonelik kuyruğuna kopyalayabilirsiniz. Bu filtreleme abonelik filtreleri kullanılarak gerçekleştirilir. Bu tür *değişikliklere filtre eylemleri*denir. Bir abonelik oluşturulduğunda, iletinin özellikleri, hem sistem özellikleri (örneğin, **Etiket)** hem de özel uygulama özellikleri (örneğin, **StoreName**.) üzerinde çalışan bir filtre ifadesi sağlayabilirsiniz. Bu durumda SQL filtresi ifadesi isteğe bağlıdır; BIR SQL filtresi ifadesi olmadan, abonelikte tanımlanan herhangi bir filtre eylemi, bu abonenin tüm iletilerinde gerçekleştirilir.

Tam çalışma örneği için GitHub'daki [TopicSubscriptionWithRuleOperationsSample örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) bakın.

Olası filtre değerleri hakkında daha fazla bilgi için [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) ve [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) sınıfları için belgelere bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi ve Servis Veri Servisi mesajlaşması kullanma örnekleri için aşağıdaki gelişmiş konulara bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Hızlı başlangıç: Azure portalı ve .NET kullanarak iletileri gönderme ve alma](service-bus-quickstart-portal.md)
* [Öğretici: Azure portalı ve konuları/abonelikleri kullanarak envanter güncelleştirme](service-bus-tutorial-topics-subscriptions-portal.md)


