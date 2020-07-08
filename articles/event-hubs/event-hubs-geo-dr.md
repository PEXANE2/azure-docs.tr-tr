---
title: Coğrafi olağanüstü durum kurtarma-Azure Event Hubs | Microsoft Docs
description: Coğrafi bölgeleri kullanarak yük devretme ve Azure Event Hubs olağanüstü durum kurtarma gerçekleştirme
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 142e2b99376bef24a6477f7b40394ca2b67f292b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85320555"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Azure Event Hubs-coğrafi olağanüstü durum kurtarma 
Tüm Azure bölgelerinin veya veri merkezlerinin (hiçbir [kullanılabilirlik](../availability-zones/az-overview.md) alanı kullanılmıyorsa) çalışma süresi kapalı olursa, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam etmesi kritik öneme sahiptir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* , herhangi bir kuruluş için önemli özelliklerdir. Azure Event Hubs, ad alanı düzeyinde hem coğrafi olağanüstü durum kurtarmayı hem de Coğrafi çoğaltmayı destekler. 

> [!NOTE]
> Coğrafi olağanüstü durum kurtarma özelliği yalnızca [Standart ve adanmış SKU 'lar](https://azure.microsoft.com/pricing/details/event-hubs/)için kullanılabilir.  

## <a name="outages-and-disasters"></a>Kesintiler ve olağanüstü durumlar

"Kesintiler" ve "olağanüstü durumlar" arasındaki ayrımı dikkate almak önemlidir. **Kesinti** , Azure Event Hubs geçici olarak kullanılamaz ve hizmetin bir mesajlaşma deposu veya hatta tüm veri merkezinin bazı bileşenlerini etkileyebilir. Ancak, sorun giderildikten sonra Event Hubs yeniden kullanılabilir hale gelir. Genellikle kesinti, ileti veya diğer verilerin kaybedilmesine neden olmaz. Bu tür bir kesinti, veri merkezinde bir güç hatası olabilir. Bazı kesintiler, geçici veya ağ sorunları nedeniyle yalnızca kısa bağlantı kayıplarıdır. 

*Olağanüstü durum* , Event Hubs kümesi, Azure bölgesi veya veri merkezi 'nin kalıcı veya uzun süreli kaybı olarak tanımlanır. Bölge veya veri merkezi bir kez daha kullanılamayabilir veya kullanılabilir olmayabilir ya da saat veya günler için kapatılmış olabilir. Bu tür felate örnek olarak Fire, taşmasını veya deprem verilebilir. Kalıcı hale gelen bir olağanüstü durum, bazı ileti, olay veya diğer verilerin kaybedilmesine neden olabilir. Ancak çoğu durumda, veri merkezi yedeklendikten sonra veri kaybı olmaması ve iletiler kurtarılabilir.

Azure Event Hubs coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan kavramlar ve iş akışı, geçici veya geçici kesintilere değil olağanüstü durum senaryoları için geçerlidir. Microsoft Azure ' de olağanüstü durum kurtarma hakkında ayrıntılı bir tartışma için [Bu makaleye](/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü durum kurtarma özelliği, meta veri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarını kullanır. 

Coğrafi olağanüstü durum kurtarma özelliği yalnızca [Standart ve adanmış SKU 'lar](https://azure.microsoft.com/pricing/details/event-hubs/) için kullanılabilir. Bağlantı bir diğer ad aracılığıyla yapıldığından herhangi bir bağlantı dizesi değişikliği yapmanız gerekmez.

Bu makalede aşağıdaki terimler kullanılmaktadır:

-  *Diğer ad*: ayarladığınız bir olağanüstü durum kurtarma yapılandırması adı. Diğer ad, tek bir tutarlı tam etki alanı adı (FQDN) bağlantı dizesi sağlar. Uygulamalar, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanır. 

-  *Birincil/ikincil ad alanı*: diğer ada karşılık gelen ad alanları. Birincil ad alanı "etkin" ve iletileri alır (var olan veya yeni bir ad alanı olabilir). İkincil ad alanı "pasif" ve ileti almaz. Her ikisi arasındaki meta veriler eşitlenmiş olduğundan, her ikisi de herhangi bir uygulama kodu veya bağlantı dizesi değişikliği olmadan iletileri sorunsuzca kabul edebilir. Yalnızca etkin ad alanının iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veri*: Olay Hub 'ları ve tüketici grupları gibi varlıklar; ve ad alanıyla ilişkili hizmetin özellikleri. Yalnızca varlıklar ve ayarları otomatik olarak çoğaltılır. İletiler ve olaylar çoğaltılmaz. 

-  *Yük devretme*: ikincil ad alanını etkinleştirme işlemi.

## <a name="supported-namespace-pairs"></a>Desteklenen ad alanı çiftleri
Aşağıdaki birincil ve ikincil ad alanları birleşimleri desteklenir:  

| Birincil ad alanı | İkincil ad alanı | Destekleniyor | 
| ----------------- | -------------------- | ---------- |
| Standart | Standart | Evet | 
| Standart | Ayrılmış | Evet | 
| Ayrılmış | Ayrılmış | Evet | 
| Ayrılmış | Standart | Hayır | 

> [!NOTE]
> Aynı adanmış kümede bulunan ad alanlarını eşleştiriyorsunuz. Ayrı kümelerdeki ad alanlarını eşleştirde ayırabilirsiniz. 

## <a name="setup-and-failover-flow"></a>Kurulum ve yük devretme akışı

Aşağıdaki bölüm, yük devretme işlemine genel bakış ve ilk yük devretmeyi ayarlamayı açıklar. 

![1][]

### <a name="setup"></a>Kurulum

Önce mevcut bir birincil ad alanını ve yeni bir ikincil ad alanını oluşturun veya kullanın, ardından ikisini eşleştirin. Bu eşleştirme size bağlanmak için kullanabileceğiniz bir diğer ad sağlar. Bir diğer ad kullandığınız için bağlantı dizelerini değiştirmeniz gerekmez. Yalnızca yeni ad alanları, yük devretme eşleştirmeye eklenebilir. Son olarak, bir yük devretmenin gerekli olup olmadığını algılamak için bazı izleme eklemelisiniz. Çoğu durumda, hizmet büyük bir ekosisteminin bir parçasıdır. bu nedenle, yük devretme işlemleri genellikle geri kalan alt sistem veya altyapıyla eşitlenmelidir.

### <a name="example"></a>Örnek

Bu senaryonun bir örneğinde, ileti ya da olay gösteren bir satış noktası (POS) çözümü düşünün. Event Hubs, bu olayları bazı eşleme veya yeniden biçimlendirme çözümüne geçirir ve daha sonra daha fazla işlenmek üzere eşlenen verileri başka bir sisteme iletir. Bu noktada, tüm bu sistemler aynı Azure bölgesinde barındırılıyor olabilir. Ne zaman ve hangi parçanın yük devretme kararı, altyapınızdaki verilerin akışına bağlıdır. 

Yük devretmeyi, izleme sistemleriyle veya özel olarak oluşturulmuş izleme çözümleriyle otomatik hale getirebilirsiniz. Ancak, bu tür otomasyon, bu makalenin kapsamı dışında daha fazla planlama ve iş alır.

### <a name="failover-flow"></a>Yük devretme akışı

Yük devretmeyi başlatırsanız iki adım gereklidir:

1. Başka bir kesinti oluşursa, yeniden yük devredebilmek istersiniz. Bu nedenle, başka bir pasif ad alanı ayarlayın ve eşlemeyi güncelleştirin. 

2. Yeniden kullanılabilir olduktan sonra eski birincil ad alanından ileti çekin. Bundan sonra, coğrafi kurtarma kurulumunuzu dışında düzenli mesajlaşma için bu ad alanını kullanın veya eski birincil ad alanını silin.

> [!NOTE]
> Yalnızca başarısız iletme semantiği desteklenir. Bu senaryoda yük devreder ve sonra yeni bir ad alanı ile yeniden eşleştirin. Yeniden başarısız olma desteklenmez; Örneğin, bir SQL kümesinde. 

![2][]

## <a name="management"></a>Yönetim

Bir hata yaptıysanız, Örneğin, ilk kurulum sırasında yanlış bölgeleri eşleştirdikten sonra, iki ad alanının eşleştirmesini dilediğiniz zaman kesebilirsiniz. Eşleştirilmiş ad alanlarını normal ad alanları olarak kullanmak istiyorsanız, diğer adı silin.

## <a name="samples"></a>Örnekler

[GitHub 'daki örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) , bir yük devretme ayarlamayı ve başlatmayı gösterir. Bu örnek aşağıdaki kavramları göstermektedir:

- Event Hubs ile Azure Resource Manager kullanmak için Azure Active Directory gereken ayarlar. 
- Örnek kodu yürütmek için gereken adımlar. 
- Geçerli birincil ad alanından gönderin ve alın. 

## <a name="considerations"></a>Önemli noktalar

Bu sürümü göz önünde bulundurmanız için aşağıdaki noktalara dikkat edin:

1. Tasarım, coğrafi olağanüstü durum kurtarma Event Hubs verileri çoğaltmaz ve bu nedenle ikincil Olay Hub 'ınızdaki birincil olay hub 'ınızın eski değer değerini yeniden kullanamazsınız. Aşağıdaki yöntemlerden birini kullanarak olay alıcılarınızı yeniden başlatmanız önerilir:

- *Eventposition. FromStart ()* -ikincil Olay Hub 'ınızdaki tüm verileri okumak istiyorsanız.
- *Eventposition. FromEnd ()* -ikincil Olay Hub 'ınıza bağlantı sırasında tüm yeni verileri okumak istiyorsanız.
- *Eventposition. FromEnqueuedTime (DateTime)* -belirli bir tarih ve saatten itibaren ikincil Olay Hub 'ınızdaki alınan tüm verileri okumak istiyorsanız.

2. Yük devretme planlamadaki zaman etmenini de göz önünde bulundurmanız gerekir. Örneğin, 15 ila 20 dakikaya kadar olan bağlantıyı kaybederseniz, yük devretmeyi başlatmaya karar verebilirsiniz. 
 
3. Hiçbir veri çoğaltılmaması, etkin olan oturumların çoğaltılmadığı anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanan iletiler çalışmayabilir. Yeni oturumlar, zamanlanan mesajlar ve yeni yinelemeler çalışacaktır. 

4. Karmaşık bir dağıtılmış altyapının yük devretmesi en az bir kez [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) edilmelidir. 

5. Varlıkların eşitlenmesi birkaç dakika sürebilir ve dakikada yaklaşık 50-100 varlık olabilir.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları 

Event Hubs standart SKU, bir Azure bölgesi içinde hataya yalıtılmış konumlar sağlayarak [kullanılabilirlik alanları](../availability-zones/az-overview.md)destekler. 

> [!NOTE]
> Azure Event Hubs Standard için Kullanılabilirlik Alanları desteği yalnızca kullanılabilirlik bölgelerinin bulunduğu [Azure bölgelerinde](../availability-zones/az-region.md) kullanılabilir.

Azure portal kullanarak yalnızca yeni ad alanlarında Kullanılabilirlik Alanları etkinleştirebilirsiniz. Event Hubs var olan ad alanlarının geçişini desteklemez. Ad alanınız üzerinde etkinleştirildikten sonra bölge yedekliliği devre dışı bırakılamıyor.

![3][]

## <a name="private-endpoints"></a>Özel uç noktalar
Bu bölümde, Özel uç noktaları kullanan ad alanları ile coğrafi olağanüstü durum kurtarma kullanılırken ek konular sunulmaktadır. Genel olarak Event Hubs özel uç noktaları kullanma hakkında bilgi edinmek için bkz. [Özel uç noktaları yapılandırma](private-link-service.md).

### <a name="new-pairings"></a>Yeni eşleştirmelerinin
Özel bir uç nokta ve ikincil ad alanı olan bir birincil ad alanı arasında özel bir uç nokta olmadan bir eşleştirme oluşturmaya çalışırsanız, eşleştirme başarısız olur. Eşleştirme yalnızca birincil ve ikincil ad alanlarının özel uç noktaları varsa başarılı olur. Birincil ve ikincil ad alanlarında ve özel uç noktaların oluşturulduğu sanal ağlarda aynı konfigürasyonları kullanmanızı öneririz.  

> [!NOTE]
> Birincil ad alanını özel uç nokta ve ikincil ad alanıyla eşleştirmeye çalıştığınızda, doğrulama işlemi yalnızca ikincil ad alanında özel bir uç noktanın bulunup bulunmadığını denetler. Uç noktanın çalışıp çalışmadığını denetlemez veya yük devretmeden sonra çalışır. Özel uç nokta olan ikincil ad alanının Yük devretme sonrasında beklendiği gibi çalışmasını sağlamak sizin sorumluluğunuzdadır.
>
> Özel uç nokta yapılandırmalarının birincil ve ikincil ad alanlarında aynı olduğunu sınamak için, sanal ağın dışından ikincil ad alanına bir okuma isteği (örneğin: [Olay Hub 'ı al](/rest/api/eventhub/get-event-hub)) gönderin ve hizmetten bir hata iletisi alduğunuzu doğrulayın.

### <a name="existing-pairings"></a>Mevcut eşleştirmelerinin
Birincil ve ikincil ad alanı arasında eşleştirme zaten varsa, birincil ad alanı üzerinde özel uç nokta oluşturma başarısız olur. Çözümlemek için, önce ikincil ad alanında özel bir uç nokta oluşturun ve ardından birincil ad alanı için bir tane oluşturun.

> [!NOTE]
> İkincil ad alanına salt okuma erişimine izin verdiğimiz için, Özel uç nokta yapılandırmalarına yönelik güncelleştirmelere izin verilir. 

### <a name="recommended-configuration"></a>Önerilen yapılandırma
Uygulamanız için bir olağanüstü durum kurtarma yapılandırması oluştururken ve ad alanları Event Hubs, hem birincil hem de ikincil Event Hubs ad alanları için, uygulamanızın birincil ve ikincil örneklerini barındıran sanal ağlara karşı özel uç noktalar oluşturmanız gerekir. 

İki sanal ağınız olduğunu varsayalım: VNET-1, VNET-2 ve bu birincil ve ikincil ad alanları: EventHubs-Namespace1-PRIMARY, EventHubs-Namespace2-Secondary. Aşağıdaki adımları gerçekleştirmeniz gerekir: 

- EventHubs-Namespace1-Primary üzerinde, VNET-1 ve VNET-2 alt ağlarını kullanan iki özel uç nokta oluşturun
- EventHubs-Namespace2-Secondary üzerinde, VNET-1 ve VNET-2 ' d a aynı alt ağları kullanan iki özel uç nokta oluşturun 

![Özel uç noktalar ve sanal ağlar](./media/event-hubs-geo-dr/private-endpoints-virtual-networks.png)

Bu yaklaşımın avantajı, yük devretmenin uygulama katmanında Event Hubs ad alanından bağımsız olmasını sağlayabilmektedir. Aşağıdaki senaryoları göz önünde bulundurun: 

**Yalnızca uygulama yük devretmesi:** Burada uygulama VNET-1 ' de mevcut olmayacaktır, ancak VNET-2 ' ye geçmeyecektir. Hem birincil hem de ikincil ad alanları için hem VNET-1 hem de VNET-2 üzerinde hem özel uç noktalar yapılandırıldığından, uygulama çalışacaktır. 

**Yalnızca ad alanı yük devretme Event Hubs**: her iki özel uç nokta hem birincil hem de ikincil ad alanları için her iki sanal ağda de yapılandırıldığı için, uygulama çalışacaktır. 

> [!NOTE]
> Bir sanal ağın coğrafi olağanüstü durum kurtarma hakkında yönergeler için bkz. [sanal ağ-Iş sürekliliği](../virtual-network/virtual-network-disaster-recovery-guidance.md).
 
## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'daki örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) , coğrafi eşleme oluşturan ve olağanüstü durum kurtarma senaryosu için yük devretme Başlatan basit bir iş akışında gösterilmektedir.
* [REST API başvurusu](/rest/api/eventhub/) , coğrafi olağanüstü durum kurtarma yapılandırmasını gerçekleştirmek Için API 'leri açıklar.

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

- Event Hubs kullanmaya başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [JavaScript](get-started-java-send-v2.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png
