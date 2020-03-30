---
title: Coğrafi felaket kurtarma - Azure Etkinlik Hub'ları| Microsoft Dokümanlar
description: Azure Olay Hub'larında başarısız olmak ve olağanüstü durum kurtarma gerçekleştirmek için coğrafi bölgeleri kullanma
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
ms.openlocfilehash: 40db6e9f429569bc19641aa5f0f371f287db7b18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281476"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Azure Olay Hub'ları - Coğrafi felaket kurtarma 

Tüm Azure bölgeleri veya veri merkezleri [(kullanılabilirlik bölgeleri](../availability-zones/az-overview.md) kullanılıyorsa) kapalı kalma süresi yle karşılaştığında, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam etmesi çok önemlidir. Bu nedenle, *Jeo-felaket kurtarma* ve *Geo-çoğaltma* herhangi bir kuruluş için önemli özelliklerdir. Azure Olay Hub'ları, ad alanı düzeyinde hem coğrafi felaket kurtarma yı hem de coğrafi çoğaltmayı destekler. 

> [!NOTE]
> Geo-disaster kurtarma özelliği yalnızca [standart ve özel SNU'lar](https://azure.microsoft.com/pricing/details/event-hubs/)için kullanılabilir.  

## <a name="outages-and-disasters"></a>Kesintiler ve felaketler

"Kesintiler" ve "felaketler" arasındaki ayrımı dikkate almak önemlidir. *Kesinti,* Azure Olay Hub'larının geçici olarak kullanılamamasıdır ve ileti deposu ve hatta tüm veri merkezi gibi hizmetin bazı bileşenlerini etkileyebilir. Ancak, sorun giderildikten sonra Olay Hub'ları yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletilerin veya diğer verilerin kaybına neden olmaz. Böyle bir kesintiörneği veri merkezinde bir güç kesintisi olabilir. Bazı kesintiler geçici veya ağ sorunları nedeniyle yalnızca kısa bağlantı kayıplarıdır. 

*Bir felaket,* olay hub'ları kümesinin, Azure bölgesinin veya veri merkezinin kalıcı veya uzun vadeli kaybı olarak tanımlanır. Bölge veya veri merkezi yeniden kullanılabilir olabilir veya olmayabilir veya saatlerce veya günlerdir kapanabilir. Bu tür felaketlere örnek olarak yangın, sel veya deprem verilebilir. Kalıcı hale gelen bir felaket, bazı iletilerin, olayların veya diğer verilerin kaybolmasına neden olabilir. Ancak, çoğu durumda veri kaybı olmamalıdır ve veri merkezi yedeklendikten sonra iletiler kurtarılabilir.

Azure Olay Hub'larının Coğrafi durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan kavramlar ve iş akışı, geçici veya geçici kesintiler için değil, felaket senaryoları için geçerlidir. Microsoft Azure'da olağanüstü durum kurtarma yla ilgili ayrıntılı bir tartışma için [bu makaleye](/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü durum kurtarma özelliği meta veri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarına dayanır. 

Geo-disaster kurtarma özelliği yalnızca [standart ve özel SNU'lar](https://azure.microsoft.com/pricing/details/event-hubs/) için kullanılabilir. Bağlantı bir takma ad aracılığıyla yapıldığından, bağlantı dizedeğişiklikleri yapmanız gerekmez.

Bu makalede aşağıdaki terimler kullanılmıştır:

-  *Diğer Ad*: Ayarladığınız olağanüstü durum kurtarma yapılandırmasının adı. Takma ad, tek bir kararlı Tam Nitelikli Etki Alanı Adı (FQDN) bağlantı dizesini sağlar. Uygulamalar, ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanır. 

-  *Birincil/ikincil ad alanı*: Diğer ada karşılık gelen ad alanları. Birincil ad alanı "etkin" dir ve iletileri alır (bu varolan veya yeni bir ad alanı olabilir). İkincil ad alanı "pasif" dir ve ileti almaz. Her ikisi arasındaki meta veriler eşitlenir, böylece her ikisi de herhangi bir uygulama kodu veya bağlantı dizesi değişiklikleri olmadan iletileri sorunsuz bir şekilde kabul edebilir. Yalnızca etkin ad alanının ileti aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veriler*: Etkinlik merkezleri ve tüketici grupları gibi varlıklar; ve ad alanıile ilişkili hizmetin özellikleri. Yalnızca varlıkların ve ayarlarının otomatik olarak çoğaltıldığını unutmayın. İletiler ve olaylar çoğaltılamaz. 

-  *Failover*: İkincil ad alanını etkinleştirme işlemidir.

## <a name="supported-namespace-pairs"></a>Desteklenen ad alanı çiftleri
Birincil ve ikincil ad alanlarının aşağıdaki birleşimleri desteklenir:  

| Birincil ad alanı | İkincil ad alanı | Destekleniyor | 
| ----------------- | -------------------- | ---------- |
| Standart | Standart | Evet | 
| Standart | Ayrılmış | Evet | 
| Ayrılmış | Ayrılmış | Evet | 
| Ayrılmış | Standart | Hayır | 

> [!NOTE]
> Aynı özel kümede bulunan ad alanlarını eşleştiremezsiniz. Ayrı kümelerde bulunan ad alanlarını eşleştirebilirsiniz. 

## <a name="setup-and-failover-flow"></a>Kurulum ve arıza akışı

Aşağıdaki bölüm, başarısız sürecin genel bir özetidir ve ilk başarısızın nasıl ayarlanılabildiğini açıklar. 

![1][]

### <a name="setup"></a>Kurulum

Önce varolan bir birincil ad alanı ve yeni bir ikincil ad alanı oluşturun veya kullanın, sonra ikisini eşleştirin. Bu eşleştirme, bağlanmak için kullanabileceğiniz bir diğer ad verir. Takma ad kullandığınızdan, bağlantı dizelerini değiştirmeniz gerekmez. Başarısız eşleştirmenize yalnızca yeni ad alanları eklenebilir. Son olarak, bir hata nın gerekli olup olmadığını algılamak için bazı izleme ler eklemeniz gerekir. Çoğu durumda, hizmet büyük bir ekosistemin bir parçasıdır, bu nedenle otomatik arızalar nadiren mümkündür, çünkü çoğu zaman başarısız lar kalan alt sistem veya altyapıyla senkronize olarak gerçekleştirilmelidir.

### <a name="example"></a>Örnek

Bu senaryonun bir örneğinde, ileti ler veya olaylar yayıtan bir Satış Noktası (POS) çözümünü düşünün. Olay Hub'ları bu olayları, daha fazla işleme için eşlenen verileri başka bir sisteme ileten bazı eşleme veya yeniden biçimlendirme çözümüne geçirir. Bu noktada, bu sistemlerin tümü aynı Azure bölgesinde barındırılabilir. Ne zaman ve hangi bölümde başarısız olursa karar, altyapınızdaki veri akışına bağlıdır. 

Failover'ı izleme sistemleriyle veya özel olarak oluşturulmuş izleme çözümleriyle otomatikleştirebilirsiniz. Ancak, bu tür otomasyon bu makalenin kapsamı dışında ekstra planlama ve iş alır.

### <a name="failover-flow"></a>Başarısız akış

Başarısız ı başlatırsanız, iki adım gerekir:

1. Başka bir kesinti oluşursa, tekrar başarısız olmak isteyebilirsiniz. Bu nedenle, başka bir pasif ad alanı ayarlayın ve eşleştirmegüncelleştirin. 

2. Yeniden kullanılabilir olduğunda eski birincil ad alanından iletileri çekin. Bundan sonra, bu ad alanını coğrafi kurtarma kurulumuzun dışında düzenli ileti için kullanın veya eski birincil ad alanını silin.

> [!NOTE]
> Sadece ileri semantik desteklenir başarısız. Bu senaryoda, üzerinde başarısız ve sonra yeni bir ad alanı ile yeniden eşleştirmek. Geri başarısız desteklenmez; örneğin, bir SQL kümesinde. 

![2][]

## <a name="management"></a>Yönetim

Eğer bir hata yaptıysanız; örneğin, ilk kurulum sırasında yanlış bölgeleri eşlediniz, istediğiniz zaman iki ad alanının eşleşmesini kırabilirsiniz. Eşleştirilmiş ad alanlarını normal ad alanları olarak kullanmak istiyorsanız, diğer adı silin.

## <a name="samples"></a>Örnekler

[GitHub'daki örnek,](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) nasıl bir hata ayarlayıp başlatılabildiğini gösterir. Bu örnek aşağıdaki kavramları göstermektedir:

- Azure Etkin Dizini'nde Etkinlik Hub'larıyla Azure Kaynak Yöneticisi'ni kullanmak için gereken ayarlar. 
- Örnek kodu yürütmek için gereken adımlar. 
- Geçerli birincil ad alanından gönderin ve alın. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürümde göz önünde bulundurulması gereken hususlara dikkat edin:

1. Tasarım gereği, Olay Hub'ları coğrafi durum kurtarma verileri çoğaltmaz ve bu nedenle birincil olay hub'ınızın eski ofset değerini ikincil olay hub'ınızda yeniden kullanamazsınız. Etkinlik alıcınızı aşağıdakilerden biriyle yeniden başlatmanızı öneririz:

- *EventPosition.FromStart()* - İsterseniz ikincil etkinlik merkezinizdeki tüm verileri okuyun.
- *EventPosition.FromEnd()* - İkincil olay merkezinize bağlantı zamanındaki tüm yeni verileri okumak isterseniz.
- *EventPosition.FromEnqueuedTime(dateTime)* - Belirli bir tarih ve saatten itibaren ikincil etkinlik merkezinizde alınan tüm verileri okumak isterseniz.

2. Başarısız planlama, aynı zamanda zaman faktörü düşünmelisiniz. Örneğin, bağlantınızı 15 ila 20 dakikadan uzun süre kaybederseniz, başarısızlığı başlatmaya karar verebilirsiniz. 
 
3. Hiçbir verinin çoğaltılmadı olması, şu anda etkin oturumların çoğaltılmadı anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanan iletiler çalışmayabilir. Yeni oturumlar, zamanlanmış iletiler ve yeni yinelenenler çalışır. 

4. Karmaşık dağıtılmış bir altyapı üzerinde başarısız en az bir kez [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) edilmelidir. 

5. Eşitleme varlıkları, dakikada yaklaşık 50-100 varlıklar biraz zaman alabilir.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları 

Etkinlik Hub'ları Standart SKU, Azure bölgesinde hataya yalıtılmış konumlar sağlayarak [Kullanılabilirlik Bölgelerini](../availability-zones/az-overview.md)destekler. 

> [!NOTE]
> Azure Etkinlik Hub'ları Standardı için Kullanılabilirlik Bölgeleri desteği yalnızca kullanılabilirlik bölgelerinin bulunduğu [Azure bölgelerinde](../availability-zones/az-overview.md#services-support-by-region) kullanılabilir.

Kullanılabilirlik Bölgelerini yalnızca yeni ad alanlarında Azure portalını kullanarak etkinleştirebilirsiniz. Olay Hub'ları varolan ad alanlarının geçişini desteklemez. Ad alanınızda etkinleştirdikten sonra bölge artıklığını devre dışı bırakamazsınız.

![3][]

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub'daki örnek,](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) coğrafi eşleştirme oluşturan basit bir iş akışından geçer ve olağanüstü durum kurtarma senaryosu için bir hata başlatma işlemi başlatır.
* [REST API başvurusu,](/rest/api/eventhub/disasterrecoveryconfigs) Coğrafi felaket kurtarma yapılandırmasını gerçekleştirmek için API'leri açıklar.

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

- Event Hubs kullanmaya başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [Javascript](get-started-java-send-v2.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png
