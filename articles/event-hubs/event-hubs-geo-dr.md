---
title: Coğrafi olağanüstü durum kurtarma-Azure Event Hubs | Microsoft Docs
description: Azure Event Hubs yük devretme ve olağanüstü durum kurtarma işlemleri için coğrafi bölgeleri kullanma
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 2c42637dda9d1a413c0521ea2d7565a63ca58e81
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858294"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Azure Event Hubs-coğrafi olağanüstü durum kurtarma 

Azure bölgelerinin veya veri merkezlerinin (hiçbir [kullanılabilirlik](../availability-zones/az-overview.md) alanı kullanılmıyorsa) çalışma süresi kapalı kalma süresi, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam edebilmesi için kritik öneme sahiptir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* , herhangi bir kuruluş için önemli özelliklerdir. Azure Event Hubs, ad alanı düzeyinde hem coğrafi olağanüstü durum kurtarmayı hem de Coğrafi çoğaltmayı destekler. 

> [!NOTE]
> Coğrafi olağanüstü durum kurtarma özelliği yalnızca [Standart ve adanmış SKU 'lar](https://azure.microsoft.com/pricing/details/event-hubs/)için kullanılabilir.  

## <a name="outages-and-disasters"></a>Kesintiler ve olağanüstü durumlar

"Kesintiler" ve "olağanüstü durumlar" arasındaki ayrımı dikkate almak önemlidir. *Kesinti* , Azure Event Hubs geçici olarak kullanılamaz ve hizmetin bir mesajlaşma deposu veya hatta tüm veri merkezinin bazı bileşenlerini etkileyebilir. Ancak, sorun giderildikten sonra Event Hubs yeniden kullanılabilir hale gelir. Genellikle kesinti, ileti veya diğer verilerin kaybedilmesine neden olmaz. Bu tür bir kesinti, veri merkezinde bir güç hatası olabilir. Bazı kesintiler, geçici veya ağ sorunları nedeniyle yalnızca kısa bağlantı kayıplarıdır. 

*Olağanüstü durum* , Event Hubs kümesi, Azure bölgesi veya veri merkezi 'nin kalıcı veya uzun süreli kaybı olarak tanımlanır. Bölge veya veri merkezi bir kez daha kullanılamayabilir veya kullanılabilir olmayabilir ya da saat veya günler için kapatılmış olabilir. Bu tür felate örnek olarak Fire, taşmasını veya deprem verilebilir. Kalıcı hale gelen bir olağanüstü durum, bazı ileti, olay veya diğer verilerin kaybedilmesine neden olabilir. Ancak çoğu durumda, veri merkezi yedeklendikten sonra veri kaybı olmaması ve iletiler kurtarılabilir.

Azure Event Hubs coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan kavramlar ve iş akışı, geçici veya geçici kesintilere değil olağanüstü durum senaryoları için geçerlidir. Microsoft Azure ' de olağanüstü durum kurtarma hakkında ayrıntılı bir tartışma için [Bu makaleye](/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü durum kurtarma özelliği, meta veri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarını kullanır. 

Coğrafi olağanüstü durum kurtarma özelliği yalnızca [Standart ve adanmış SKU 'lar](https://azure.microsoft.com/pricing/details/event-hubs/) için kullanılabilir. Bağlantı bir diğer ad aracılığıyla yapıldığından herhangi bir bağlantı dizesi değişikliği yapmanız gerekmez.

Bu makalede aşağıdaki terimler kullanılmaktadır:

-  *Diğer ad*: ayarladığınız bir olağanüstü durum kurtarma yapılandırması adı. Diğer ad, tek bir tutarlı tam etki alanı adı (FQDN) bağlantı dizesi sağlar. Uygulamalar, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanır. 

-  *Birincil/ikincil ad alanı*: diğer ada karşılık gelen ad alanları. Birincil ad alanı "etkin" ve iletileri alır (Bu, var olan veya yeni bir ad alanı olabilir). İkincil ad alanı "pasif" ve ileti almaz. Her ikisi arasındaki meta veriler eşitlenmiş olduğundan, her ikisi de herhangi bir uygulama kodu veya bağlantı dizesi değişikliği olmadan iletileri sorunsuzca kabul edebilir. Yalnızca etkin ad alanının iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veri*: Olay Hub 'ları ve tüketici grupları gibi varlıklar; ve ad alanıyla ilişkili hizmetin özellikleri. Yalnızca varlıkların ve ayarlarının otomatik olarak çoğaltıldığını unutmayın. İletiler ve olaylar çoğaltılmaz. 

-  *Yük devretme*: ikincil ad alanını etkinleştirme işlemi.

## <a name="supported-namespace-pairs"></a>Desteklenen ad alanı çiftleri
Aşağıdaki birincil ve ikincil ad alanları birleşimleri desteklenir:  

| Birincil ad alanı | İkincil ad alanı | Destekleniyor | 
| ----------------- | -------------------- | ---------- |
| Standart | Standart | Yes | 
| Standart | Ayrılmış | Yes | 
| Ayrılmış | Ayrılmış | Yes | 
| Ayrılmış | Standart | No | 

> [!NOTE]
> Aynı adanmış kümede bulunan ad alanlarını eşleştiriyorsunuz. Ayrı kümelerdeki ad alanlarını eşleştirde ayırabilirsiniz. 

## <a name="setup-and-failover-flow"></a>Kurulum ve yük devretme akışı

Aşağıdaki bölüm, yük devretme işlemine genel bakış ve ilk yük devretmeyi ayarlamayı açıklar. 

![1][]

### <a name="setup"></a>Kurulum

Önce mevcut bir birincil ad alanını ve yeni bir ikincil ad alanını oluşturun veya kullanın, ardından ikisini eşleştirin. Bu eşleştirme size bağlanmak için kullanabileceğiniz bir diğer ad sağlar. Bir diğer ad kullandığınız için bağlantı dizelerini değiştirmek zorunda değilsiniz. Yalnızca yeni ad alanları, yük devretme eşleştirmeye eklenebilir. Son olarak, bir yük devretmenin gerekli olup olmadığını algılamak için bazı izleme eklemelisiniz. Çoğu durumda, hizmet büyük bir ekosisteminin bir parçasıdır ve bu nedenle otomatik yük devretme işlemleri, kalan alt sistemle veya altyapıyla eşitlenmesinin yapılması gerekir.

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

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürümü göz önünde bulundurmanız için aşağıdaki noktalara dikkat edin:

1. Tasarım, coğrafi olağanüstü durum kurtarma Event Hubs verileri çoğaltmaz ve bu nedenle ikincil Olay Hub 'ınızdaki birincil olay hub 'ınızın eski değer değerini yeniden kullanamazsınız. Olay alıcılarınızı aşağıdakilerden biriyle yeniden başlatmanız önerilir:

- *Eventposition. FromStart ()* -ikincil Olay Hub 'ınızdaki tüm verileri okumak istiyorsanız.
- *Eventposition. FromEnd ()* -ikincil Olay Hub 'ınıza bağlantı sırasında tüm yeni verileri okumak istiyorsanız.
- *Eventposition. FromEnqueuedTime (DateTime)* -belirli bir tarih ve saatten itibaren ikincil Olay Hub 'ınızdaki alınan tüm verileri okumak istiyorsanız.

2. Yük devretme planlamadaki zaman etmenini de göz önünde bulundurmanız gerekir. Örneğin, 15 ila 20 dakikaya kadar olan bağlantıyı kaybederseniz, yük devretmeyi başlatmaya karar verebilirsiniz. 
 
3. Hiçbir veri çoğaltılmaması, o anda etkin oturumların çoğaltılmadığı anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanan iletiler çalışmayabilir. Yeni oturumlar, zamanlanan mesajlar ve yeni yinelemeler çalışacaktır. 

4. Karmaşık bir dağıtılmış altyapının yük devretmesi en az bir kez [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) edilmelidir. 

5. Varlıkların eşitlenmesi birkaç dakika sürebilir ve dakikada yaklaşık 50-100 varlık olabilir.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları 

Event Hubs standart SKU, bir Azure bölgesi içinde hataya yalıtılmış konumlar sağlayarak [kullanılabilirlik alanları](../availability-zones/az-overview.md)destekler. 

> [!NOTE]
> Azure Event Hubs Standard için Kullanılabilirlik Alanları desteği yalnızca kullanılabilirlik bölgelerinin bulunduğu [Azure bölgelerinde](../availability-zones/az-region.md) kullanılabilir.

Azure portal kullanarak yalnızca yeni ad alanlarında Kullanılabilirlik Alanları etkinleştirebilirsiniz. Event Hubs var olan ad alanlarının geçişini desteklemez. Ad alanınız üzerinde etkinleştirildikten sonra bölge yedekliliği devre dışı bırakılamaz.

![3][]

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
