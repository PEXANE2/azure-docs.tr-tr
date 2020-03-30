---
title: Azure Depolama kuyruklarını ve Servis Veri Hizmeti Sıralarını Karşılaştırma
description: Azure tarafından sunulan iki tür kuyruk arasındaki farklılıkları ve benzerlikleri analiz eder.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 09/04/2019
ms.author: aschhab
ms.openlocfilehash: 8379b7f48e7e494370f3fdba81676d34821d7b6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75563386"
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Depolama kuyrukları ve Servis Veri Servisi sıraları - karşılaştırıldığında ve karşıt
Bu makalede, Microsoft Azure tarafından bugün sunulan iki tür kuyruk arasındaki farkları ve benzerlikleri çözümlenetürü: Depolama kuyrukları ve Hizmet Veri Servisi kuyrukları. Bu bilgileri kullanarak, ilgili teknolojileri karşılaştırabilir ve karşılaştırabilir ve hangi çözümün ihtiyaçlarınızı en iyi karşıladığına daha bilinçli bir karar verebilirsiniz.

## <a name="introduction"></a>Giriş
Azure iki tür sıra mekanizmasını destekler: **Depolama kuyrukları** ve **Servis Veri Servisi sırası.**

[Azure depolama](https://azure.microsoft.com/services/storage/) altyapısının bir parçası olan **depolama kuyrukları,** hizmetler içinde ve arasında güvenilir ve kalıcı mesajlaşma sağlayan basit bir REST tabanlı GET/PUT/PEEK arabirimine sahiptir.

**Servis Veri Hizmetleri kuyrukları,** sıraya girmenin yanı sıra yayımlama/abone olun ve daha gelişmiş tümleştirme modellerini destekleyen daha geniş bir [Azure ileti](https://azure.microsoft.com/services/service-bus/) altyapısının parçasıdır. Servis Veri Servisi kuyrukları/konuları/abonelikleri hakkında daha fazla bilgi için [Servis Veri Servisi'ne genel bakış](service-bus-messaging-overview.md)bölümüne bakın.

Her iki kuyruk teknolojisi de aynı anda mevcut olsa da, Azure Depolama hizmetlerinin üzerine yerleşik özel bir sıra depolama mekanizması olarak önce Depolama kuyrukları tanıtıldı. Servis Veri Hizmetleri kuyrukları, birden çok iletişim protokolü, veri sözleşmeleri, güven etki alanlarını ve/veya ağ ortamlarını kapsayabilecek uygulamaları veya uygulama bileşenlerini tümleştirmek üzere tasarlanmış daha geniş ileti altyapısının üzerine inşa edilmiştir.

## <a name="technology-selection-considerations"></a>Teknoloji seçimi nde dikkat edilmesi gerekenler
Hem Depolama kuyrukları hem de Hizmet Veri Servisi kuyrukları, Şu anda Microsoft Azure tarafından sunulan ileti sıraya hizmetinin uygulamalarıdır. Her biri biraz farklı bir özellik kümesine sahiptir, bu da çözümünüzün veya çözdüğününüzün veya iş/teknik sorunun ihtiyaçlarına bağlı olarak birini veya her ikisini de kullanabileceğiniz anlamına gelir.

Hangi kuyruk teknolojisinin belirli bir çözüm amacına uyduğunu belirlerken, çözüm mimarları ve geliştiricilerbu önerileri göz önünde bulundurmalıdır. Daha fazla bilgi için bir sonraki bölüme bakın.

Çözüm mimarı/geliştiricisi olarak, şu **anda Depolama kuyruklarını kullanmayı düşünmelisiniz:**

* Uygulamanız 80 GB'tan fazla iletiyi kuyrukta depolamalıdır.
* Uygulamanız, sıranın içindeki bir iletiyi işlemek için ilerlemeyi izlemek istiyor. İletiyi işleyen alt kişi çöküyorsa bu yararlıdır. Sonraki bir çalışan daha sonra önceki çalışanın bıraktığı yerden devam etmek için bu bilgileri kullanabilirsiniz.
* Kuyruklarınıza karşı yürütülen tüm işlemlerin sunucu yan günlüklerini gerektirirsiniz.

Çözüm mimarı/geliştiricisi olarak, **şu anda Servis Veri Servisi kuyruklarını kullanmayı düşünmelisiniz:**

* Çözümünüz, sırayı yoklamak zorunda kalmadan ileti alabilmeli. Service Bus ile bu, Hizmet Veri Servisi'nin desteklediği TCP tabanlı protokolleri kullanarak uzun yoklama alma işlemi kullanılarak elde edilebilir.
* Çözümünüz, sipariş edilen ilk ilk (FIFO) garantili teslimatı sağlamak için sırayı gerektirir.
* Çözümünüz otomatik yinelenen algılamayı destekleyebilmeli.
* Uygulamanızın iletileri paralel uzun süren akışlar olarak işlemesini istiyorsunuz (iletiler, iletideki [SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid) özelliğini kullanarak bir akışla ilişkilidir). Bu modelde, alıcı uygulamasındaki her düğüm iletilerin aksine akışlar için yarışıyor. Bir akış tüketen bir düğüme verildiğinde, düğüm hareketleri kullanarak uygulama akışı durumunu inceleyebilir.
* Çözümünüz, bir kuyruktan birden çok ileti gönderirken veya alırken işlem davranışı ve atomiklik gerektirir.
* Uygulamanız 64 KB'yi aşan ancak 256 KB sınırına yaklaşmayan iletileri işler.
* Kuyruklara rol tabanlı bir erişim modeli ve gönderenler ve alıcılar için farklı haklar/izinler sağlama gereksinimiyle ilgilenirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:
    - [Yönetilen kimliklerle kimlik doğrulaması](service-bus-managed-service-identity.md)
    - [Uygulamadan kimlik doğrulaması yapma](authenticate-application.md)
* Kuyruk boyutunuz 80 GB'ın üzerine çıkmayacaktır.
* AMQP 1.0 standart tabanlı ileti protokolünü kullanmak istiyorsunuz. AMQP hakkında daha fazla bilgi için [Servis Veri Servisi AMQP Genel Bakış'a](service-bus-amqp-overview.md)bakın.
* Sıra tabanlı noktadan noktaya iletişimden, her biri bir kısmının veya tamamının bağımsız kopyalarını alan ek alıcıların (abonelerin) sorunsuz entegrasyonunu sağlayan bir ileti alışverişi desenine nihai bir geçiş öngörebilirsiniz sıraya gönderilen iletiler. İkincisi, Service Bus tarafından yerel olarak sağlanan yayımlama/abone etme özelliğianlamına gelir.
* İleti çözümünüz, ek altyapı bileşenlerini oluşturmanıza gerek kalmadan "En Çok At-Most-Once" teslim garantisini destekleyebilmeli.
* İletileri toplu halde yayımlayabilmek ve kullanabilmek istersiniz.

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Depolama kuyruklarını ve Servis Veri Veri Sırası kuyruklarını karşılaştırma
Aşağıdaki bölümlerdeki tablolar, sıra özelliklerinin mantıksal bir gruplandırmasını sağlar ve bir bakışta hem Azure Depolama kuyruklarında hem de Servis Veri Servisi kuyruklarında bulunan yetenekleri karşılaştırmanızı sağlar.

## <a name="foundational-capabilities"></a>Temel yetenekler
Bu bölümde, Depolama kuyrukları ve Servis Veri Servisi kuyrukları tarafından sağlanan bazı temel kuyruk özellikleri karşılaştırılır.

| Karşılaştırma Kriterleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Sipariş garantisi |**Hayır** <br/><br>Daha fazla bilgi için "Ek Bilgiler" bölümündeki ilk nota bakın.</br> |**Evet - İlk İlk Çıkış (FIFO)**<br/><br>(mesajlaşma oturumları nın kullanımı yoluyla) |
| Teslimat garantisi |**En az-bir kez** |**En Az-Once** (PeekLock alma modunu kullanarak - bu varsayılan) <br/><br/>**En Çok At** (ReceiveAndDelete alma modunu kullanarak) <br/> <br/> Çeşitli Alma [modları](service-bus-queues-topics-subscriptions.md#receive-modes) hakkında daha fazla bilgi edinin  |
| Atomik operasyon desteği |**Hayır** |**Evet**<br/><br/> |
| Davranışı alma |**Engellenmez**<br/><br/>(yeni bir ileti bulunmazsa hemen tamamlar) |**Zaman anına sahip/zaman alamadan engelleme**<br/><br/>(uzun yoklama veya ["Kuyruklu yıldız tekniği"](https://go.microsoft.com/fwlink/?LinkId=613759)sunuyor)<br/><br/>**Engellenmez**<br/><br/>(sadece .NET yönetilen API'nin kullanımı yoluyla) |
| Push tarzı API |**Hayır** |**Evet**<br/><br/>[OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) ve **OnMessage** oturumları .NET API. |
| Alma modu |**Peek & Kiralama** |**Peek & Kilidi**<br/><br/>**Sil& Alma** |
| Özel erişim modu |**Kira tabanlı** |**Kilit tabanlı** |
| Kiralama/Kilitleme süresi |**30 saniye (varsayılan)**<br/><br/>**7 gün (maksimum)** [(UpdateMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.updatemessage) API'sini kullanarak ileti kiralamayı yenileyebilir veya yayınlayabilirsiniz.) |**60 saniye (varsayılan)**<br/><br/>[Yenile Kilidi](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API'sini kullanarak ileti kilidini yenileyebilirsiniz. |
| Kiralama/Kilitleme hassasiyeti |**İleti düzeyi**<br/><br/>(Her iletinin farklı bir zaman önceliği değeri olabilir ve bu değeri [UpdateMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.updatemessage) API'sini kullanarak iletiyi işlerken gerektiğinde güncelleştirebilirsiniz) |**Sıra düzeyi**<br/><br/>(Her kuyruğun tüm iletilerine uygulanan bir kilit hassasiyeti vardır, ancak [Kilidi Yenile](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API'sini kullanarak yenileyebilirsiniz.) |
| Toplu alma |**Evet**<br/><br/>(en fazla 32 ileti alırken ileti sayısını açıkça belirtme) |**Evet**<br/><br/>(örtülü olarak bir ön getirme özelliğini etkinleştirme veya açıkça işlemlerin kullanımı yoluyla) |
| Toplu gönderme |**Hayır** |**Evet**<br/><br/>(işlemlerin kullanımı veya müşteri tarafı toplu işyoluyla) |

### <a name="additional-information"></a>Ek bilgiler
* Depolama kuyruklarında iletiler genellikle ilk ilk çıkış, ancak bazen sıra dışı olabilir; örneğin, bir iletinin görünürlük zaman aşımı süresi sona erdiğinde (örneğin, bir istemci uygulamasının işleme sırasında çökmesinin bir sonucu olarak). Görünürlük süresi sona erdiğinde, başka bir çalışanın sırayı bozması için sırada yine görünür hale gelir. Bu noktada, yeni görünen ileti, başlangıçta sonra sıralanmış bir iletiden sonra kuyruğa (yeniden sıradan çıkmak üzere) yerleştirilebilir.
* Servis Veri Servisi kuyruklarında garantili FIFO deseni, ileti oturumlarının kullanılmasını gerektirir. Peek **& Kilit** modunda alınan bir iletiyi işlerken uygulamanın çökmesi durumunda, bir sonraki sıra alıcısı bir ileti oturumunu kabul ederse, süresi dolan (TTL) süresi dolduktan sonra başarısız iletiyle başlar.
* Depolama kuyrukları, hatalar, yük tesviyesi ve bina işlemi iş akışları için ölçeklenebilirliği ve toleransı artırmak için uygulama bileşenlerini ayrıştma gibi standart sıraya alma senaryolarını desteklemek üzere tasarlanmıştır.
* Servis Veri Aracı oturumları bağlamında ileti işleme ile ilgili tutarsızlıklar, oturumun ileti sırasını işleme ilerlemesine göre uygulamanın durumunu depolamak için oturum durumu kullanılarak ve ilgili işlemler kullanılarak önlenebilir alınan iletileri yerleşme ve oturum durumunu güncelleştirme. Bu tür bir tutarlılık özelliği bazen diğer satıcının ürünlerinde *Tam Olarak-Bir Kez İşleme* olarak etiketlenir, ancak işlem hataları açıkça iletilerin yeniden teslim edilmesine neden olur ve bu nedenle terim tam olarak yeterli değildir.
* Depolama kuyrukları, hem geliştiriciler hem de operasyon ekipleri için kuyruklar, tablolar ve BLOB'lar arasında tek düzenve tutarlı bir programlama modeli sağlar.
* Servis Veri Servisi kuyrukları, tek bir kuyruk bağlamında yerel hareketler için destek sağlar.
* Service Bus tarafından desteklenen **Alma ve Sil** modu, daha düşük teslim güvencesi karşılığında ileti işlem sayısını (ve ilişkili maliyeti) azaltma olanağı sağlar.
* Depolama kuyrukları, iletilerin kiralarını genişletme olanağı yla kiralamasağlar. Bu, çalışanların iletiler üzerinde kısa kiralar tutmalarına olanak tanır. Bu nedenle, bir işçi çökerse, ileti başka bir çalışan tarafından hızlı bir şekilde yeniden işlenebilir. Buna ek olarak, bir işçi, geçerli kira süresinden daha uzun süre işlemesi gerekiyorsa, iletinin kirasüresini uzatabilir.
* Depolama kuyrukları, bir iletiyi sıralayarak veya sıralayarak ayarlayabileceğiniz bir görünürlük zaman acısı sunar. Ayrıca, çalışma zamanında farklı kira değerlerine sahip bir iletiyi güncelleştirebilir ve aynı sırada bulunan iletiler arasında farklı değerleri güncelleştirebilirsiniz. Hizmet Veri İşlemi kilit zaman aşımları sıra meta verilerinde tanımlanır; ancak, [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) yöntemini arayarak kilidi yenileyebilirsiniz.
* Servis Veri Yolu kuyruklarında engelleme alma işlemi için en yüksek zaman aşımı 24 gündür. Ancak, REST tabanlı zaman ekmelerinin maksimum değeri 55 saniyedir.
* Service Bus tarafından sağlanan istemci tarafı toplu işleme, bir kuyruk istemcisi tek bir gönderme işlemi içine birden çok ileti toplu sağlar. Toplu iş yalnızca eşzamanlı gönderme işlemleri için kullanılabilir.
* Depolama kuyruklarının 200 TB tavanı (hesapları sanallaştırdığınızda daha fazla) ve sınırsız kuyruklar gibi özellikler, onu SaaS sağlayıcıları için ideal bir platform haline getirin.
* Depolama kuyrukları esnek ve performanslı bir devralan erişim denetim mekanizması sağlar.

## <a name="advanced-capabilities"></a>Gelişmiş yetenekler
Bu bölümde, Depolama kuyrukları ve Servis Veri Servisi kuyrukları tarafından sağlanan gelişmiş özellikler karşılaştırılır.

| Karşılaştırma Kriterleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Zamanlanmış teslim |**Evet** |**Evet** |
| Otomatik ölü harfler |**Hayır** |**Evet** |
| Sıra zaman-canlı değerini artırma |**Evet**<br/><br/>(görünürlük zaman ayarı yerinde güncelleme yoluyla) |**Evet**<br/><br/>(özel bir API fonksiyonu ile sağlanır) |
| Zehirli mesaj desteği |**Evet** |**Evet** |
| Yerinde güncelleştirme |**Evet** |**Evet** |
| Sunucu tarafı hareket günlüğü |**Evet** |**Hayır** |
| Depolama ölçümleri |**Evet**<br/><br/>**Dakika Ölçümleri**: kullanılabilirlik, TPS, API çağrı sayıları, hata sayıları ve daha fazlası için gerçek zamanlı ölçümler sağlar, hepsi gerçek zamanlı olarak (dakikada toplanır ve üretimde olanlardan birkaç dakika içinde rapor edilir). Daha fazla bilgi için [Depolama Analizi Ölçümleri Hakkında'ya](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics)bakın. |**Evet**<br/><br/>(GetQueues'i arayarak toplu [sorgular)](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues) |
| Durum yönetimi |**Hayır** |**Evet**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.EntityStatus,Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus) [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus) |
| İleti otomatik yönlendirme |**Hayır** |**Evet** |
| Temizleme sırası işlevi |**Evet** |**Hayır** |
| İleti grupları |**Hayır** |**Evet**<br/><br/>(mesajlaşma oturumları nın kullanımı yoluyla) |
| İleti grubuna göre uygulama durumu |**Hayır** |**Evet** |
| Yineleme algılama |**Hayır** |**Evet**<br/><br/>(gönderen tarafında yapılandırılabilir) |
| İleti gruplarına göz atma |**Hayır** |**Evet** |
| İleti oturumlarını id'e göre alma |**Hayır** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Her iki kuyruk teknolojisi de bir iletinin daha sonra teslim edilmesiiçin zamanlanmasını sağlar.
* Sıra otomatik iletme, binlerce sıranın iletilerini tek bir kuyruğa otomatik olarak iletmesini ve alıcı uygulamanın iletiyi tüketmesini sağlar. Bu mekanizmayı, her ileti yayımcısı arasında güvenlik, denetim akışı ve depolamayı yalıtmak için kullanabilirsiniz.
* Depolama kuyrukları ileti içeriğini güncelleştirmek için destek sağlar. Bu işlevi, sıfırdan başlamak yerine bilinen son denetim noktasından işlenebilecek şekilde iletiye kalıcı durum bilgileri ve artımlı ilerleme güncelleştirmeleri için kullanabilirsiniz. Servis Veri Servisi kuyruklarında, ileti oturumları kullanarak aynı senaryoyu etkinleştirebilirsiniz. Oturumlar, uygulama işleme durumunu kaydetmenizi ve almanızı sağlar [(SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) ve [GetState'i](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)kullanarak).
* Yalnızca Servis Veri Servisi kuyrukları tarafından desteklenen [ölü harfler,](service-bus-dead-letter-queues.md)alıcı uygulama tarafından başarılı bir şekilde işlenemeyen veya iletilerin süresi dolan süresi dolan (TTL) özelliği nedeniyle hedeflerine ulaşamaması nedeniyle iletileri yalıtmak için yararlı olabilir. TTL değeri, bir iletinin sırada ne kadar kaldığını belirtir. Servis Veri Servisi ile ileti, TTL süresi sona erdiğinde $DeadLetterQueue adlı özel bir kuyruğa taşınır.
* Depolama kuyruklarında "zehir" iletilerini bulmak için, bir iletiyi sıralarken uygulama iletinin [DequeueCount](/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage.dequeuecount) özelliğini inceler. **DequeueCount** belirli bir eşiğe göre büyükse, uygulama iletiyi uygulama tanımlı bir "ölü harf" kuyruğuna taşır.
* Depolama kuyrukları, sıraya karşı yürütülen tüm işlemlerin yanı sıra toplu ölçümlerin ayrıntılı bir günlüğünü elde etmenizi sağlar. Bu seçeneklerin her ikisi de hata ayıklama ve uygulamanızın Depolama kuyruklarını nasıl kullandığını anlamak için yararlıdır. Ayrıca, uygulamanızın performans ayarı ve kuyrukkullanma maliyetlerini azaltmak için de yararlıdır.
* Service Bus tarafından desteklenen "ileti oturumları" kavramı, belirli bir mantıksal gruba ait iletilerin belirli bir alıcıyla ilişkilendirilmesini sağlar ve bu da iletiler ve ilgili alıcıları arasında oturum benzeri bir yakınlık oluşturur. [OturumKimliği](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) özelliğini bir iletiye ayarlayarak Hizmet Veri Yolunda bu gelişmiş işlevselliği etkinleştirebilirsiniz. Alıcılar daha sonra belirli bir oturum kimliğini dinleyebilir ve belirtilen oturum tanımlayıcısını paylaşan iletiler alabilir.
* Service Bus kuyrukları tarafından desteklenen yineleme algılama işlevi, [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) özelliğinin değerini temel alarak kuyruğa veya konuya gönderilen yinelenen iletileri otomatik olarak kaldırır.

## <a name="capacity-and-quotas"></a>Kapasite ve kotalar
Bu bölümde, depolama kuyrukları ve Servis Veri Servisi kuyrukları kapasite ve uygulanabilecek [kotalar](service-bus-quotas.md) açısından karşılaştırılır.

| Karşılaştırma Kriterleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Maksimum sıra boyutu |**500 TB**<br/><br/>(tek bir [depolama hesabı kapasitesiyle](../storage/common/storage-introduction.md#queue-storage)sınırlıdır) |**1 GB - 80 GB arası**<br/><br/>(bir sıra oluşturulması ve [bölümleme etkinleştirilmesi](service-bus-partitioning.md) üzerine tanımlanan - "Ek Bilgiler" bölümüne bakın) |
| Maksimum ileti boyutu |**64 KB**<br/><br/>**(Base64** kodlama kullanırken 48 KB)<br/><br/>Azure, kuyrukları ve lekeleri birleştirerek büyük iletileri destekler ve bu noktada tek bir öğe için 200 GB'a kadar sıraya girebilirsiniz. |**256 KB** veya **1 MB**<br/><br/>(hem üstbilgi hem de gövde dahil, maksimum üstbilgi boyutu: 64 KB).<br/><br/>[Hizmet katmanına](service-bus-premium-messaging.md)bağlıdır. |
| Maksimum mesaj TTL |**Sonsuz** (api versiyonu itibariyle 2017-07-27) |**Zaman Aralığı.Max** |
| Maksimum sıra sayısı |**Sınırsız** |**10,000**<br/><br/>(hizmet ad alanı başına) |
| Maksimum eşzamanlı istemci sayısı |**Sınırsız** |**Sınırsız**<br/><br/>(100 eşzamanlı bağlantı sınırı yalnızca TCP protokol tabanlı iletişim için geçerlidir) |

### <a name="additional-information"></a>Ek bilgiler
* Service Bus sıra boyutu sınırları zorlar. En büyük sıra boyutu, sıranın oluşturulmasında belirtilir ve 1 ile 80 GB arasında bir değere sahip olabilir. Sıra nın oluşturulmasında ayarlanan sıra boyutu değerine ulaşılırsa, ek gelen iletiler reddedilir ve arama kodu tarafından bir özel durum alınır. Servis Otobüsü kotaları hakkında daha fazla bilgi için [Servis Otobüsü Kotalarına](service-bus-quotas.md)bakın.
* Bölümleme [Premium katmanında](service-bus-premium-messaging.md)desteklenmez. Standart katmanda, 1, 2, 3, 4 veya 5 GB boyutlarında (varsayılan değer 1 GB'dır) Servis Veri Servisi kuyrukları oluşturabilirsiniz. Standart katmanda, bölümleme etkin (varsayılan olan) ile Hizmet Veri Servisi, belirttiğiniz her GB için 16 bölüm oluşturur. Bu nedenle, 16 bölümlü 5 GB boyutunda bir kuyruk oluşturursanız, en yüksek sıra boyutu (5 * 16) = 80 GB olur. [Azure portalındaki][Azure portal]girişine bakarak bölümlenmiş sıranızın veya konunuzun maksimum boyutunu görebilirsiniz.
* Depolama kuyruklarında, iletinin içeriği XML güvenli değilse, **Base64** kodlanmış olmalıdır. İletiyi **temel 64**kodlarsanız, kullanıcı yükü 64 KB yerine 48 KB'ye kadar olabilir.
* Servis Veri Servisi sıralarında, kuyrukta depolanan her ileti iki bölümden oluşur: bir üstbilgi ve gövde. İletinin toplam boyutu, hizmet katmanı tarafından desteklenen maksimum ileti boyutunu aşamaz.
* İstemciler TCP protokolü üzerinden Servis Veri Mes'ü kuyruklarıyla iletişim kurduğunda, tek bir Servis Veri Servisi kuyruğuna en fazla eşzamanlı bağlantı sayısı 100 ile sınırlıdır. Bu sayı gönderenler ve alıcılar arasında paylaşılır. Bu kotaya ulaşılırsa, sonraki ek bağlantı istekleri reddedilir ve arama kodu tarafından bir özel durum alınır. Bu sınır, REST tabanlı API kullanarak kuyruklara bağlanan istemcilere uygulanmaz.
* Tek bir Hizmet Veri Kurumu ad alanında 10.000'den fazla sıraya ihtiyacınız varsa, Azure destek ekibine başvurabilir ve artış talep edebilirsiniz. Service Bus ile 10.000 sıranın ötesine ölçeklendirmek için [Azure portalını][Azure portal]kullanarak ek ad alanları da oluşturabilirsiniz.

## <a name="management-and-operations"></a>Yönetim ve operasyonlar
Bu bölümde Depolama kuyrukları ve Servis Veri Servisi kuyrukları tarafından sağlanan yönetim özellikleri karşılaştırılır.

| Karşılaştırma Kriterleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Yönetim protokolü |**HTTP/HTTPS üzerinde DINLENME** |**HTTPS üzerinde DINLENME** |
| Çalışma zamanı protokolü |**HTTP/HTTPS üzerinde DINLENME** |**HTTPS üzerinde DINLENME**<br/><br/>**AMQP 1.0 Standardı (TLS ile TCP)** |
| .NET API’si |**Evet**<br/><br/>(.NET Depolama İstemci API) |**Evet**<br/><br/>(.NET Servis Veri Servisi API) |
| Yerli C++ |**Evet** |**Evet** |
| Java API’si |**Evet** |**Evet** |
| PHP API |**Evet** |**Evet** |
| Düğüm.js API |**Evet** |**Evet** |
| Rasgele meta veri desteği |**Evet** |**Hayır** |
| Sıra adlandırma kuralları |**63 karaktere kadar uzun**<br/><br/>(Sıra adındaki harfler küçük olmalıdır.) |**260 karaktere kadar uzun**<br/><br/>(Sıra yolları ve adları büyük/küçük harf duyarsızdır.) |
| Sıra uzunluğu işlevini alın |**Evet**<br/><br/>(İletilerin silinmeden TTL'nin ötesinde sona ermesi durumunda yaklaşık değer.) |**Evet**<br/><br/>(Tam, tam, zaman içinde nokta değeri.) |
| Peek fonksiyonu |**Evet** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Depolama kuyrukları, ad/değer çiftleri biçiminde sıra açıklamasına uygulanabilecek rasgele öznitelikler için destek sağlar.
* Her iki sıra teknolojisi de bir iletiyi kilitlemek zorunda kalmadan gözetleme olanağı sunar ve bu da bir sıra gezgini/tarayıcı aracını uygularken yararlı olabilir.
* Servis Veri Mes'ü .NET aracılı mesajlaşma API'leri, HTTP üzerinden REST ile karşılaştırıldığında daha iyi performans için tam çift yönlü TCP bağlantılarından yararlanır ve AMQP 1.0 standart protokolünü destekler.
* Depolama kuyruklarının adları 3-63 karakter uzunluğunda olabilir, küçük harfler, sayılar ve tireler içerebilir. Daha fazla bilgi için [Bkz. Sıraları ve Meta Verileri Adlandırma.](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata)
* Servis Veri Sitesi sıra adları en fazla 260 karakter uzunluğunda olabilir ve daha az kısıtlayıcı adlandırma kurallarına sahip olabilir. Servis Veri Servisi sıra adları harfleri, sayıları, dönemleri, tireleri ve alt çizerleri içerebilir.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Bu bölümde, Depolama kuyrukları ve Servis Veri Servisi sıraları tarafından desteklenen kimlik doğrulama ve yetkilendirme özellikleri açıklanır.

| Karşılaştırma Kriterleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Kimlik doğrulaması |**Simetrik anahtar** |**Simetrik anahtar** |
| Güvenlik modeli |SAS belirteçleri aracılığıyla yetkin erişim. |Sas |
| Kimlik sağlayıcı federasyonu |**Hayır** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Kuyruk teknolojilerinden herhangi birinin her isteği doğrulanmalıdır. Anonim erişimi olan genel kuyruklar desteklenmez. [SAS'ı](service-bus-sas.md)kullanarak, bu senaryoyu yalnızca yazma SAS'ı, salt okunur SAS'ı ve hatta tam erişimli bir SAS yayımlayarak ele alabilirsiniz.
* Depolama kuyrukları tarafından sağlanan kimlik doğrulama düzeni, SHA-256 algoritması ile hesaplanan ve **Base64** dizesi olarak kodlanan karma tabanlı İleti Kimlik Doğrulama Kodu (HMAC) olan simetrik bir anahtarın kullanımını içerir. İlgili protokol hakkında daha fazla bilgi için [Azure Depolama Hizmetleri için Kimlik Doğrulaması'na](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services)bakın. Servis Veri Servisi kuyrukları, simetrik tuşları kullanarak benzer bir modeli destekler. Daha fazla bilgi için [Hizmet Veri Servisi ile Paylaşılan Erişim İmza Kimlik Doğrulaması'na](service-bus-sas.md)bakın.

## <a name="conclusion"></a>Sonuç
İki teknoloji hakkında daha derin bir anlayış kazanarak, hangi sıra teknolojisinin kullanılacağına ve ne zaman kullanılacağına daha bilinçli bir karar verebilirsiniz. Depolama kuyruklarının veya Servis Veri Servisi kuyruklarının ne zaman kullanılacağına ilişkin karar, açıkça bir dizi etkene bağlıdır. Bu faktörler büyük ölçüde uygulamanızın bireysel ihtiyaçlarına ve mimarisine bağlı olabilir. Uygulamanız Microsoft Azure'un temel özelliklerini zaten kullanıyorsa, özellikle hizmetler arasında temel iletişim ve ileti ye ihtiyaç duyuyorsanız veya 80 GB'tan büyük boyutlarda kuyruklara ihtiyaç duyuyorsanız, Depolama kuyruklarını seçmeyi tercih edebilirsiniz.

Servis Veri Hizmeti sırası oturumlar, hareketler, yinelenen algılama, otomatik ölü yazma ve dayanıklı yayımlama/abone olma özellikleri gibi bir dizi gelişmiş özellik sağladığından, karma bir uygulama oluşturuyorsanız veya uygulamanız bu özellikleri gerektiriyorsa tercih edilebilir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler, Depolama kuyruklarını veya Servis Veri Servisi kuyruklarını kullanma hakkında daha fazla kılavuz ve bilgi sağlar.

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Sıra Depolama Hizmeti Nasıl Kullanılır?](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Service Bus aracılı mesajlaşmayı kullanarak performans iyileştirmeleri için en iyi uygulamalar](service-bus-performance-improvements.md)
* [Azure Hizmet Veri Tos'unda Kuyruklar ve Konular (blog gönderimi) Tanıtımı](https://www.serverless360.com/blog/azure-service-bus-queues-vs-topics)
* [Geliştiricinin Servis Otobüsü Rehberi](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Azure'da Kuyruk Hizmetini Kullanma](https://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

