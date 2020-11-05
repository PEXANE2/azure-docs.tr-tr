---
title: Azure Service Bus mesajlaşma-kuyruklar, konular ve abonelikler
description: Bu makalede, Azure Service Bus mesajlaşma varlıklarına (kuyruk, konular ve abonelikler) genel bir bakış sunulmaktadır.
ms.topic: article
ms.date: 11/04/2020
ms.openlocfilehash: 7c1d22605e841eac42f2b47aab38777a622bfb90
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93359434"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus kuyrukları, konu başlıkları ve abonelikleri
Azure Service Bus, güvenilir Message Queuing ve dayanıklı yayımlama/abone olma iletileri dahil olmak üzere bulut tabanlı, ileti yönelimli bir ara yazılım teknolojilerini destekler. Bu Aracılı mesajlaşma özellikleri, Service Bus mesajlaşma iş yükünü kullanarak yayımlama-abone olma, zamana bağlı ayırma ve yük dengeleme senaryolarını destekleyen ayrılmış mesajlaşma özellikleri olarak düşünülebilir. Ayrılmış iletişimin birçok avantajı vardır. Örneğin, istemciler ve sunucular gerektiğinde bağlanabilir ve işlemlerini zaman uyumsuz bir biçimde gerçekleştirebilir.

Service Bus 'deki mesajlaşma yeteneklerinin çekirdeğini oluşturan mesajlaşma varlıkları **Kuyruklar** , **konular ve abonelikler** ve kurallar/eylemlerdir.

## <a name="queues"></a>Kuyruklar
Kuyruklar, bir veya daha fazla rakip tüketiciye **ilk olarak, Ilk çıkar** (FIFO) ileti teslimi sunar. Diğer bir deyişle, alıcılar genellikle iletileri kuyruğa eklendikleri sırayla alır ve işler. Ve, her iletiyi yalnızca bir ileti tüketicisi alır ve işler. Kuyrukları kullanmanın önemli bir avantajı, **uygulama bileşenlerinin** zamana bağlı olarak ayrıştırılmasıyla elde edilir. Diğer bir deyişle, üreticileri (Gönderen) ve tüketiciler (Alıcılar) aynı anda ileti alıp göndermek zorunda kalmaz. Çünkü iletiler kuyrukta durda depolanır. Ayrıca, Producer 'ın, tüketiciden yanıt almaya ve iletileri gönderilmeye devam etmesini beklemek zorunda değildir.

İlgili bir avantaj, üreticileri ve tüketicilerin farklı oranlarda ileti göndermesini ve almasını sağlayan **Yük dengelemeye** yönelik bir avantajdır. Birçok uygulamada, sistem yükü zamana göre farklılık gösterir. Ancak, her iş birimi için gereken işleme süresi genellikle sabittir. Aracılığıyla Message üreticileri ve bir kuyruğu olan tüketiciler, tüketen uygulamanın yalnızca en yüksek yük yerine ortalama yükü işleyebilmesi gerektiği anlamına gelir. Gelen yük hacmi değiştikçe kuyruğun derinliği artar ve daralır. Bu özellik, uygulama yüküne hizmet vermek için gereken altyapı miktarına göre doğrudan para tasarrufu sağlar. Yük arttıkça, kuyruktan okumak için daha fazla çalışan işlemi eklenebilir. Her ileti yalnızca bir çalışan işlemi tarafından işlenir. Ayrıca, bu çekme tabanlı yük dengeleme, çalışan bilgisayarların, en yüksek hızda güç çekme iletilerini işlemesi durumunda bile çalışan bilgisayarların en iyi şekilde kullanılmasını sağlar. Bu düzen, genelde **rakip tüketici** düzeni olarak adlandırılır.

İleti üreticileri ve tüketiciler arasında ara için kuyrukları kullanmak, bileşenler arasında devralınan bir gevşek bağ sağlar. Üreticileri ve tüketiciler birbirleriyle uyumlu olmadığından, bir tüketici üreticisi üzerinde herhangi bir etkiye gerek kalmadan yükseltilebilir.

### <a name="create-queues"></a>Kuyruklar Oluşturma
[Azure Portal](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md)veya [Kaynak Yöneticisi şablonlarını](service-bus-resource-manager-namespace-queue.md)kullanarak kuyruklar oluşturabilirsiniz. Ardından, [C#](service-bus-dotnet-get-started-with-queues.md), [Java](service-bus-java-how-to-use-queues.md), [Python](service-bus-python-how-to-use-queues.md), [JavaScript](service-bus-nodejs-how-to-use-queues-new-package.md), [php](service-bus-php-how-to-use-queues.md)ve [Ruby](service-bus-ruby-how-to-use-queues.md)dilinde yazılmış istemcileri kullanarak ileti gönderin ve alın. 

### <a name="receive-modes"></a>Alma modları
Service Bus iletileri aldığı iki farklı mod belirtebilirsiniz: **Receiveanddelete** veya **PeekLock**. [Receiveanddelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda, Service Bus tüketiciden aldığında, iletiyi tüketildiği gibi işaretler ve tüketici uygulamasına geri döndürür. Bu mod en basit modeldir. Bir hata oluşursa uygulamanın bir iletiyi işlememesi için kabul edebildiği senaryolar için en iyi yöntem kullanılır. Bu senaryoyu anlamak için, tüketicinin alma isteğini yaptığı ve işlemeden önce çöktüğü bir senaryo düşünün. Service Bus ileti tüketildiği gibi işaretlediği için, uygulama yeniden başlatma sırasında iletileri tükemeye başlar. Çökmeden önce tüketildiği iletiyi kaçırır.

[PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda, alma işlemi iki aşamalı olur ve bu da eksik iletileri kabul edemediği uygulamaları desteklemeye olanak sağlar. Service Bus isteği aldığında, aşağıdaki işlemleri yapar:

1. Tüketilen sonraki iletiyi bulur.
1. Diğer tüketicilerin bunu almasını engellemek için onu kilitler.
1. Ardından, iletiyi uygulamaya döndürün. 

Uygulama iletiyi işlemeyi tamamladıktan veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan sonra, iletiyi çağırarak alma işleminin ikinci aşamasını tamamlar [`CompleteAsync`](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) . Service Bus, **Tamamlananteasync** isteğini aldığında, iletiyi tüketildiği gibi işaretler.

Uygulama bir nedenden dolayı iletiyi işleyemez, [`AbandonAsync`](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) ileti üzerinde yöntemi çağırabilir (yerine [`CompleteAsync`](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) ). Bu yöntem, Service Bus iletinin kilidini açmak ve aynı tüketici ya da başka bir rekabet tüketicisi tarafından yeniden alınmalarını sağlar. İkinci olarak, kilitle ile ilişkili bir zaman aşımı vardır. Uygulama, kilit zaman aşımı dolmadan önce iletiyi işleyemezse, Service Bus iletinin kilidini açar ve yeniden alınmak üzere kullanılabilir hale gelir.

Uygulama, iletiyi tamamladıktan sonra, ancak çağrı yapmadan önce kilitlenirse [`CompleteAsync`](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) , Service Bus yeniden başlatıldığında iletiyi uygulamaya yeniden gönderir. Bu işlem genellikle **en az bir kez** işleme olarak adlandırılır. Diğer bir deyişle, her ileti en az bir kez işlenir. Ancak, bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryonuz yinelenen işleme tolerans oluşturmazsa, yinelemeleri saptamak için uygulamanıza ek mantık ekleyin. İletinin [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid) özelliğini kullanarak elde edebilirsiniz ve bu, teslim girişimleri arasında sabit kalır. Bu özellik **tam olarak bir kez** işlenirken bilinir.

## <a name="topics-and-subscriptions"></a>Konular ve abonelikler
Sıra, bir iletinin tek bir tüketici tarafından işlenmesine izin verir. Kuyrukların aksine, konular ve abonelikler, **Yayımla ve abone ol** düzeninde bire çok bir iletişim biçimi sağlar. Çok sayıda alıcıya ölçeklendirilmesi yararlı olur. Her yayımlanan ileti, konuya kayıtlı her abonelik için kullanılabilir hale getirilir. Yayımcı bir konuya bir ileti gönderir ve bir veya daha fazla abone, bu aboneliklerde ayarlanan filtre kurallarına bağlı olarak iletinin bir kopyasını alır. Abonelikler, almak istedikleri iletileri kısıtlamak için ek filtreler kullanabilir. Yayımcılar bir konuya ileti gönderdikleri şekilde bir konuya ileti gönderir. Ancak, tüketiciler doğrudan konudan ileti almaz. Bunun yerine, tüketiciler konusunun aboneliklerinden iletiler alırlar. Konu aboneliği, konuya gönderilen iletilerin kopyalarını alan bir sanal kuyruğa benzer. Tüketiciler bir aboneliğden iletileri bir kuyruktan alma yöntemiyle aynı şekilde alırlar.

Bir sıranın ileti gönderme işlevselliği doğrudan bir konuya eşlenir ve ileti alma işlevselliği bir abonelikle eşlenir. Diğer şeyler arasında bu özellik, aboneliklerin bu bölümde daha önce açıklanan desenleri desteklediği, kuyruklar: rekabet eden tüketici, zamana bağlı ayırma, yük dengeleme ve yük dengeleme anlamına gelir.

### <a name="create-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri oluşturma
Bir konu oluşturmak, önceki bölümde açıklandığı gibi kuyruk oluşturmaya benzer. [Azure Portal](service-bus-quickstart-topics-subscriptions-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-tutorial-topics-subscriptions-cli.md)veya [Kaynak Yöneticisi şablonlarını](service-bus-resource-manager-namespace-topic.md)kullanarak konular ve abonelikler oluşturabilirsiniz. Ardından, bir konuya ileti gönderin ve [C#](service-bus-dotnet-how-to-use-topics-subscriptions.md), [Java](service-bus-java-how-to-use-topics-subscriptions.md), [Python](service-bus-python-how-to-use-topics-subscriptions.md), [JavaScript](service-bus-nodejs-how-to-use-topics-subscriptions-new-package.md), [php](service-bus-php-how-to-use-topics-subscriptions.md)ve [Ruby](service-bus-ruby-how-to-use-topics-subscriptions.md)dilinde yazılmış istemcileri kullanarak aboneliklerden ileti alın. 

### <a name="rules-and-actions"></a>Kurallar ve eylemler
Birçok senaryoda, belirli özelliklere sahip olan iletiler farklı yollarla işlenmelidir. Bu işlemeyi etkinleştirmek için, istediğiniz özelliklere sahip iletileri bulmak ve ardından bu özelliklerde belirli değişiklikler yapmak üzere abonelikleri yapılandırabilirsiniz. Service Bus abonelikler konuya gönderilen tüm iletileri görirken, bu iletilerin bir alt kümesini yalnızca sanal abonelik kuyruğuna kopyalayabilirsiniz. Bu filtreleme, abonelik filtreleri kullanılarak gerçekleştirilir. Bu değişiklikler, **filtre eylemleri** olarak adlandırılır. Bir abonelik oluşturulduğunda, iletinin özelliklerinde çalışan bir filtre ifadesi sağlayabilirsiniz. Özellikler, Sistem Özellikleri (örneğin, **etiket** ) ve özel uygulama özellikleri (örneğin, **StoreName** ) olabilir. SQL filtresi ifadesi bu durumda isteğe bağlıdır. Bir SQL filtresi ifadesi olmadan, bir abonelikte tanımlı herhangi bir filtre eylemi, söz konusu aboneliğe yönelik tüm iletilerde yapılır.

Tam çalışma örneği için GitHub 'da [Topicsubscriptionwithruleoperationssample örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) bakın.

Olası filtre değerleri hakkında daha fazla bilgi için [Sqlfilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) ve [sqlruleaction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) sınıfları belgelerine bakın.

## <a name="java-message-service-jms-20-entities-preview"></a>Java ileti hizmeti (JMS) 2,0 varlıkları (Önizleme)
Aşağıdaki varlıklara Java ileti hizmeti (JMS) 2,0 API 'SI aracılığıyla erişilebilir.

  * Geçici kuyruklar
  * Geçici konular
  * Paylaşılan dayanıklı abonelikler
  * Paylaşılmayan dayanıklı abonelikler
  * Paylaşılan, dayanıklı olmayan abonelikler
  * Paylaşılmayan kalıcı olmayan abonelikler

[JMS 2,0 varlıkları](java-message-service-20-entities.md) ve [bunların nasıl kullanılacağı](how-to-use-java-message-service-20.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma kullanma hakkında daha fazla bilgi ve örnekler için aşağıdaki gelişmiş konulara bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Hızlı başlangıç: Azure portalı ve .NET kullanarak iletileri gönderme ve alma](service-bus-quickstart-portal.md)
* [Öğretici: Azure portalı ve konuları/abonelikleri kullanarak envanter güncelleştirme](service-bus-tutorial-topics-subscriptions-portal.md)


