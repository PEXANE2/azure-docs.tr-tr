---
title: Azure Service Bus coğrafi olağanüstü durum kurtarma | Microsoft Docs
description: Azure Service Bus için coğrafi bölgeleri kullanarak yük devretme ve olağanüstü durum kurtarma gerçekleştirme
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: fcdeb499b8ebecc4ecddbfcbe32b812ce7e3efe5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341480"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus coğrafi olağanüstü durum kurtarma

Azure bölgelerinin veya veri merkezlerinin (hiçbir [kullanılabilirlik](../availability-zones/az-overview.md) alanı kullanılmıyorsa) çalışma süresi kapalı kalma süresi, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam edebilmesi için kritik öneme sahiptir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* , tüm kuruluşlar için önemli bir özelliktir. Azure Service Bus, ad alanı düzeyinde coğrafi olağanüstü durum kurtarmayı destekler.

Coğrafi olağanüstü durum kurtarma özelliği Service Bus Premium SKU için genel kullanıma sunulmuştur. 

>[!NOTE]
> Coğrafi olağanüstü durum kurtarma şu anda yalnızca meta verilerin (kuyruklar, konular, abonelikler, filtreler) birincil ad alanından eşleştirildiği ikincil ad alanına kopyalanmasını sağlar.

## <a name="outages-and-disasters"></a>Kesintiler ve olağanüstü durumlar

"Kesintiler" ve "olağanüstü durumlar" arasındaki ayrımı dikkate almak önemlidir. 

*Kesinti* , Azure Service Bus geçici olarak kullanılamaz ve hizmetin bir mesajlaşma deposu gibi bazı bileşenlerini veya hatta tüm veri merkezini etkileyebilir. Ancak, sorun giderildikten sonra Service Bus yeniden kullanılabilir hale gelir. Genellikle kesinti, ileti veya diğer verilerin kaybedilmesine neden olmaz. Bu tür bir kesinti, veri merkezinde bir güç hatası olabilir. Bazı kesintiler, geçici veya ağ sorunları nedeniyle yalnızca kısa bağlantı kayıplarıdır. 

*Olağanüstü durum* , Service Bus kümesi, Azure bölgesi veya veri merkezi 'nin kalıcı veya uzun süreli kaybı olarak tanımlanır. Bölge veya veri merkezi bir kez daha kullanılamayabilir veya kullanılabilir olmayabilir ya da saat veya günler için kapatılmış olabilir. Bu tür felate örnek olarak Fire, taşmasını veya deprem verilebilir. Kalıcı hale gelen bir olağanüstü durum, bazı ileti, olay veya diğer verilerin kaybedilmesine neden olabilir. Ancak çoğu durumda, veri merkezi yedeklendikten sonra veri kaybı olmaması ve iletiler kurtarılabilir.

Azure Service Bus coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan kavramlar ve iş akışı, geçici veya geçici kesintilere değil olağanüstü durum senaryoları için geçerlidir. Microsoft Azure ' de olağanüstü durum kurtarma hakkında ayrıntılı bir tartışma için [Bu makaleye](/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.   

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü durum kurtarma özelliği, meta veri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarını kullanır. Coğrafi olağanüstü durum kurtarma özelliğinin yalnızca [PREMIUM SKU](service-bus-premium-messaging.md) için kullanılabilir olduğunu unutmayın. Bağlantı bir diğer ad aracılığıyla yapıldığından herhangi bir bağlantı dizesi değişikliği yapmanız gerekmez.

Bu makalede aşağıdaki terimler kullanılmaktadır:

-  *Diğer ad*: ayarladığınız bir olağanüstü durum kurtarma yapılandırması adı. Diğer ad, tek bir tutarlı tam etki alanı adı (FQDN) bağlantı dizesi sağlar. Uygulamalar, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanır. Diğer ad kullanılması, yük devretme tetiklendiğinde bağlantı dizesinin değişmeden olmasını sağlar.

-  *Birincil/ikincil ad alanı*: diğer ada karşılık gelen ad alanları. Birincil ad alanı "etkin" ve iletileri alır (Bu, var olan veya yeni bir ad alanı olabilir). İkincil ad alanı "pasif" ve ileti almaz. Her ikisi arasındaki meta veriler eşitlenmiş olduğundan, her ikisi de herhangi bir uygulama kodu veya bağlantı dizesi değişikliği olmadan iletileri sorunsuzca kabul edebilir. Yalnızca etkin ad alanının iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veriler*: kuyruklar, konu başlıkları ve abonelikler gibi varlıklar; ve ad alanıyla ilişkili hizmetin özellikleri. Yalnızca varlıkların ve ayarlarının otomatik olarak çoğaltıldığını unutmayın. İletiler çoğaltılmaz.

-  *Yük devretme*: ikincil ad alanını etkinleştirme işlemi.

## <a name="setup"></a>Kurulum

Aşağıdaki bölüm, ad alanları arasında eşleştirmeye kurulum için bir genel bakıştır.

![1][]

Kurulum işlemi aşağıdaki gibidir-

1. ***Birincil*** Service Bus Premium ad alanı sağlayın.

2. *Birincil ad alanının sağlandığı farklı*bir bölgede ***Ikincil*** Service Bus Premium ad alanı sağlayın. Bu, farklı veri merkezi bölgelerinde hata yalıtımına izin verir.

3. ***Diğer adı***almak için birincil ad alanı ve ikincil ad alanı arasında eşleştirme oluşturun.

    >[!NOTE] 
    > [Azure Service Bus standart ad alanınızı Azure Service Bus Premium 'a geçirdiyseniz](service-bus-migrate-standard-premium.md), **PS/CLI** veya **REST API**aracılığıyla olağanüstü durum kurtarma yapılandırması oluşturmak için önceden mevcut diğer adı (yani Service Bus standart ad alanı bağlantı dizeniz) kullanmanız gerekir.
    >
    >
    > Bunun nedeni, geçiş sırasında Azure Service Bus standart ad alanı bağlantı dizesinin/DNS adınızın, Azure Service Bus Premium ad alanınız için bir diğer ad haline geldiği bir addır.
    >
    > İstemci uygulamalarınızın, olağanüstü durum kurtarma eşinin ayarlandığı Premium ad alanına bağlanmak için bu diğer adı (örneğin, Azure Service Bus standart ad alanı bağlantı dizesi) kullanmanız gerekir.
    >
    > Olağanüstü durum kurtarma yapılandırmasını kurmak için portalını kullanıyorsanız, Portal bu desteklenmediği uyarısıyla sizin için Özet olur.


4. İstemci uygulamalarınızı coğrafi DR özellikli birincil ad alanına bağlamak için adım 3 ' te elde edilen ***diğer adı*** kullanın. Başlangıçta, diğer ad birincil ad alanını işaret eder.

5. Seçim Yük devretme gerekli olup olmadığını algılamak için bazı izleme ekleyin.

## <a name="failover-flow"></a>Yük devretme akışı

Yük devretme, müşteri tarafından el ile tetiklenir (bir komutla veya komutu tetikleyen istemci iş mantığı aracılığıyla veya hiçbir şekilde Azure tarafından). Bu, müşteriye tam sahiplik ve Azure 'ın omurga üzerinde kesinti çözümlemesi için görünürlük sağlar.

![4][]

Yük devretme tetiklendikten sonra-

1. ***Diğer ad*** bağlantı dizesi, ikincil Premium ad alanını işaret etmek üzere güncelleştirilir.

2. İstemciler (Gönderenler ve alıcılar) Ikincil ad alanına otomatik olarak bağlanır.

3. Birincil ve Ikincil Premium ad alanı arasındaki mevcut eşleştirme bozuk.

Yük devretme başlatıldıktan sonra-

1. Başka bir kesinti oluşursa, yeniden yük devredebilmek istersiniz. Bu nedenle, başka bir pasif ad alanı ayarlayın ve eşlemeyi güncelleştirin. 

2. Yeniden kullanılabilir olduktan sonra eski birincil ad alanından ileti çekin. Bundan sonra, coğrafi kurtarma kurulumunuzu dışında düzenli mesajlaşma için bu ad alanını kullanın veya eski birincil ad alanını silin.

> [!NOTE]
> Yalnızca başarısız iletme semantiği desteklenir. Bu senaryoda yük devreder ve sonra yeni bir ad alanı ile yeniden eşleştirin. Yeniden başarısız olma desteklenmez; Örneğin, bir SQL kümesinde. 

Yük devretmeyi, izleme sistemleriyle veya özel olarak oluşturulmuş izleme çözümleriyle otomatik hale getirebilirsiniz. Ancak, bu tür otomasyon, bu makalenin kapsamı dışında daha fazla planlama ve iş alır.

![2][]

## <a name="management"></a>Yönetim

Bir hata yaptıysanız, Örneğin, ilk kurulum sırasında yanlış bölgeleri eşleştirdikten sonra, iki ad alanının eşleştirmesini dilediğiniz zaman kesebilirsiniz. Eşleştirilmiş ad alanlarını normal ad alanları olarak kullanmak istiyorsanız, diğer adı silin.

## <a name="use-existing-namespace-as-alias"></a>Mevcut ad alanını diğer ad olarak kullan

Üreticileri ve tüketicilerinin bağlantılarını değiştirebir senaryonuz varsa, ad alanı adınızı diğer ad olarak yeniden kullanabilirsiniz. [GitHub 'daki örnek koda buradan](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name)bakın.

## <a name="samples"></a>Örnekler

[GitHub 'daki örneklerde](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) yük devretmeyi ayarlama ve başlatma işlemi gösterilmektedir. Bu örnekler aşağıdaki kavramları göstermektedir:

- Coğrafi olağanüstü durum kurtarma 'yı ayarlamak ve etkinleştirmek için Service Bus Azure Resource Manager kullanmak üzere Azure Active Directory için gereken bir .NET örneği ve ayarları.
- Örnek kodu yürütmek için gereken adımlar.
- Mevcut bir ad alanını diğer ad olarak kullanma.
- Alternatif olarak PowerShell veya CLı ile coğrafi olağanüstü durum kurtarmayı etkinleştirme adımları.
- Diğer adı kullanarak geçerli birincil veya ikincil ad alanından [gönderin ve alın](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) .

## <a name="considerations"></a>Önemli noktalar

Bu sürümü göz önünde bulundurmanız için aşağıdaki noktalara dikkat edin:

1. Yük devretme planlamadaki zaman etmenini de göz önünde bulundurmanız gerekir. Örneğin, 15 ila 20 dakikaya kadar olan bağlantıyı kaybederseniz, yük devretmeyi başlatmaya karar verebilirsiniz.

2. Hiçbir veri çoğaltılmaması, o anda etkin oturumların çoğaltılmadığı anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanan iletiler çalışmayabilir. Yeni oturumlar, yeni zamanlanmış iletiler ve yeni yinelemeler çalışacaktır. 

3. Karmaşık bir dağıtılmış altyapının yük devretmesi en az bir kez [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) edilmelidir.

4. Varlıkların eşitlenmesi birkaç dakika sürebilir ve dakikada yaklaşık 50-100 varlık olabilir. Abonelikler ve kurallar da varlık olarak sayılır.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Service Bus Premium SKU, bir Azure bölgesi içinde hataya yalıtılmış konumlar sağlayarak [kullanılabilirlik alanları](../availability-zones/az-overview.md)de destekler.

> [!NOTE]
> Azure Service Bus Premium için Kullanılabilirlik Alanları desteği yalnızca kullanılabilirlik alanlarının bulunduğu [Azure bölgelerinde](../availability-zones/az-region.md) kullanılabilir.

Azure portal kullanarak yalnızca yeni ad alanlarında Kullanılabilirlik Alanları etkinleştirebilirsiniz. Service Bus var olan ad alanlarının geçişini desteklemez. Ad alanınız üzerinde etkinleştirildikten sonra bölge yedekliliği devre dışı bırakılamaz.

![3][]

## <a name="private-endpoints"></a>Özel uç noktalar
Bu bölümde, Özel uç noktaları kullanan ad alanları ile coğrafi olağanüstü durum kurtarma kullanılırken ek konular sunulmaktadır. Genel olarak Service Bus özel uç noktaları kullanma hakkında bilgi edinmek için bkz. [Azure özel bağlantısı ile Azure Service Bus tümleştirme](private-link-service.md).

### <a name="new-pairings"></a>Yeni eşleştirmelerinin
Özel bir uç nokta ve ikincil ad alanı olan bir birincil ad alanı arasında özel bir uç nokta olmadan bir eşleştirme oluşturmaya çalışırsanız, eşleştirme başarısız olur. Eşleştirme yalnızca birincil ve ikincil ad alanlarının özel uç noktaları varsa başarılı olur. Birincil ve ikincil ad alanlarında ve özel uç noktaların oluşturulduğu sanal ağlarda aynı konfigürasyonları kullanmanızı öneririz. 

> [!NOTE]
> Birincil ad alanını özel bir uç nokta ve ikincil ad alanıyla eşleştirmeye çalıştığınızda, doğrulama işlemi yalnızca ikincil ad alanında özel bir uç noktanın bulunup bulunmadığını denetler. Uç noktanın çalışıp çalışmadığını denetlemez veya yük devretmeden sonra çalışır. Özel uç nokta olan ikincil ad alanının Yük devretme sonrasında beklendiği gibi çalışmasını sağlamak sizin sorumluluğunuzdadır.
>
> Özel uç nokta yapılandırmalarının aynı olduğunu sınamak için, sanal ağın dışından ikincil ad alanına bir [kuyruk alma](/rest/api/servicebus/queues/get) isteği gönderin ve hizmetten bir hata iletisi aldiğinizi doğrulayın.

### <a name="existing-pairings"></a>Mevcut eşleştirmelerinin
Birincil ve ikincil ad alanı arasında eşleştirme zaten varsa, birincil ad alanı üzerinde özel uç nokta oluşturma başarısız olur. Çözümlemek için, önce ikincil ad alanında özel bir uç nokta oluşturun ve ardından birincil ad alanı için bir tane oluşturun.

> [!NOTE]
> İkincil ad alanına salt okuma erişimine izin verdiğimiz için, Özel uç nokta yapılandırmalarına yönelik güncelleştirmelere izin verilir. 

### <a name="recommended-configuration"></a>Önerilen yapılandırma
Uygulamanız ve Service Bus için bir olağanüstü durum kurtarma yapılandırması oluştururken, uygulamanızın birincil ve ikincil örneklerini barındıran sanal ağlarda hem birincil hem de ikincil Service Bus ad alanları için özel uç noktalar oluşturmanız gerekir.

İki sanal ağınız olduğunu varsayalım: VNET-1, VNET-2 ve bu birincil ve ikinci ad alanları: ServiceBus-Namespace1-PRIMARY, ServiceBus-Namespace2-Secondary. Aşağıdaki adımları gerçekleştirmeniz gerekir: 

- ServiceBus-Namespace1-Primary üzerinde, VNET-1 ve VNET-2 ' den alt ağları kullanan iki özel uç nokta oluşturun
- ServiceBus-Namespace2-Secondary üzerinde, VNET-1 ve VNET-2 ' den aynı alt ağları kullanan iki özel uç nokta oluşturun 

![Özel uç noktalar ve sanal ağlar](./media/service-bus-geo-dr/private-endpoints-virtual-networks.png)


Bu yaklaşımın avantajı, yük devretmenin uygulama katmanında Service Bus ad alanından bağımsız olmasını sağlayabilmektedir. Aşağıdaki senaryoları göz önünde bulundurun: 

**Yalnızca uygulama yük devretmesi:** Burada uygulama VNET-1 ' de mevcut olmayacaktır, ancak VNET-2 ' ye geçmeyecektir. Hem birincil hem de ikincil ad alanları için hem VNET-1 hem de VNET-2 üzerinde hem özel uç noktalar yapılandırıldığından, uygulama çalışacaktır. 

**Yalnızca ad alanı yük devretme Service Bus**: her iki özel uç nokta hem birincil hem de ikincil ad alanları için her iki sanal ağda de yapılandırıldığı için, uygulama çalışacaktır. 

> [!NOTE]
> Bir sanal ağın coğrafi olağanüstü durum kurtarma hakkında yönergeler için bkz. [sanal ağ-Iş sürekliliği](../virtual-network/virtual-network-disaster-recovery-guidance.md).

## <a name="next-steps"></a>Sonraki adımlar

- Coğrafi olağanüstü durum kurtarma [REST API başvurusunu buraya](/rest/api/servicebus/disasterrecoveryconfigs)bakın.
- [GitHub 'Da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2)coğrafi olağanüstü durum kurtarma örneğini çalıştırın.
- [Bir diğer ada ileti gönderen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1)coğrafi olağanüstü durum kurtarma örneğine bakın.

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST API 'SI](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png
