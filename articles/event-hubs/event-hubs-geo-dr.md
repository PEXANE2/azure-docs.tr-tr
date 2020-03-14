---
title: Coğrafi olağanüstü durum kurtarma - Azure Event Hubs | Microsoft Docs
description: Yük devretme için coğrafi bölgeler kullanın ve Azure Event hubs'ı olağanüstü durum kurtarma gerçekleştirmek nasıl
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79281476"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Azure Event Hubs - coğrafi olağanüstü durum kurtarma 

Azure bölgelerinin veya veri merkezlerinin (hiçbir [kullanılabilirlik](../availability-zones/az-overview.md) alanı kullanılmıyorsa) çalışma süresi kapalı kalma süresi, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam edebilmesi için kritik öneme sahiptir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* , herhangi bir kuruluş için önemli özelliklerdir. Azure Event Hubs, hem coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, ad alanı düzeyinde destekler. 

> [!NOTE]
> Coğrafi olağanüstü durum kurtarma özelliği yalnızca [Standart ve adanmış SKU 'lar](https://azure.microsoft.com/pricing/details/event-hubs/)için kullanılabilir.  

## <a name="outages-and-disasters"></a>Kesintileri ve olağanüstü durumları yönetme

"Kesintiler" ve "olağanüstü durumlar" arasındaki fark dikkate almak önemlidir *Kesinti* , Azure Event Hubs geçici olarak kullanılamaz ve hizmetin bir mesajlaşma deposu veya hatta tüm veri merkezinin bazı bileşenlerini etkileyebilir. Sorun düzeltildikten sonra Bununla birlikte, Event Hubs yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Böyle bir kesinti örneği, veri merkezinde güç kesintisi olabilir. Bazı kesintiler yalnızca kısa bağlantı kayıpları geçici ya da ağ sorunları nedeniyle olduğundan. 

*Olağanüstü durum* , Event Hubs kümesi, Azure bölgesi veya veri merkezi 'nin kalıcı veya uzun süreli kaybı olarak tanımlanır. Bölge veya veri merkezi olabilir veya yeniden kullanılabilir olmaktan çıkabilir veya saatler veya günler için aşağı sonuna olmayabilir. Ateş, taşmasını veya deprem gibi olağanüstü örnekleridir. Kalıcı hale olağanüstü bir durum, bazı iletileri, olayları ya da diğer veri kaybına neden olabilir. Ancak, çoğu durumda olması gerekir veri kaybı olmadan ve yedekleme veri merkezi olduktan sonra iletileri kurtarılabilir.

Azure Event Hubs'ın coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve geçici ve geçici kesintiler için geçerlidir. Microsoft Azure ' de olağanüstü durum kurtarma hakkında ayrıntılı bir tartışma için [Bu makaleye](/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü Durum Kurtarma özelliği meta verileri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarında kullanır. 

Coğrafi olağanüstü durum kurtarma özelliği yalnızca [Standart ve adanmış SKU 'lar](https://azure.microsoft.com/pricing/details/event-hubs/) için kullanılabilir. Bir diğer ad üzerinden bağlantı kurulmadığı herhangi bir bağlantı dizesi değişiklik yapılması gerekmez.

Bu makalede aşağıdaki terimler kullanılır:

-  *Diğer ad*: ayarladığınız bir olağanüstü durum kurtarma yapılandırması adı. Diğer ad, tek bir kararlı tam etki alanı adı (FQDN) bağlantı dizesi sağlar. Uygulamaları, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesi kullanır. 

-  *Birincil/ikincil ad alanı*: diğer ada karşılık gelen ad alanları. Birincil ad "etkin" ve iletileri (Bu, mevcut veya yeni bir ad olabilir) alır. İkincil ad alanı, "pasif" ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitlenmiş olarak olduğundan her ikisi de sorunsuz bir şekilde uygulama kodu veya bağlantı dizesi değişiklik yapmadan iletileri kabul edebilir. Etkin ad alanı iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veri*: Olay Hub 'ları ve tüketici grupları gibi varlıklar; ve ad alanıyla ilişkili hizmetin özellikleri. Varlıklar ve ayarları otomatik olarak çoğaltılır unutmayın. İletileri ve olayları çoğaltılmaz. 

-  *Yük devretme*: ikincil ad alanını etkinleştirme işlemi.

## <a name="supported-namespace-pairs"></a>Desteklenen ad alanı çiftleri
Aşağıdaki birincil ve ikincil ad alanları birleşimleri desteklenir:  

| Birincil ad alanı | İkincil ad alanı | Destekleniyor | 
| ----------------- | -------------------- | ---------- |
| Standart | Standart | Yes | 
| Standart | Ayrılmış | Yes | 
| Ayrılmış | Ayrılmış | Yes | 
| Ayrılmış | Standart | Hayır | 

> [!NOTE]
> Aynı adanmış kümede bulunan ad alanlarını eşleştiriyorsunuz. Ayrı kümelerdeki ad alanlarını eşleştirde ayırabilirsiniz. 

## <a name="setup-and-failover-flow"></a>Kurulum ve yük devretme akışı

Aşağıdaki bölümde, yük devretme işlemini bir genel bakıştır ve ilk devretmeyi açıklanmaktadır. 

![1][]

### <a name="setup"></a>Kurulum

İlk oluşturun veya var olan bir birincil ad alanı ve yeni bir ikincil ad alanı kullanın ve sonra iki eşleştirin. Bu eşleştirme bağlanmak için kullanabileceğiniz bir diğer ad verir. Bir diğer ad kullandığından, bağlantı dizelerini değiştirmek gerekmez. Yeni ad alanları, yük devretme çifti için eklenebilir. Son olarak, bazı bir yük devretme gerekli olup olmadığını algılamak için izleme eklemeniz gerekir. Çoğu durumda, hizmet geniş ekosistem bir parçası olduğundan, çok sık yük devretmeleri kalan alt sistemi veya altyapı ile eşitleme gerçekleştirilmelidir. böylece otomatik yük devretme işlemleri nadiren mümkün olduğundan.

### <a name="example"></a>Örnek

Bu senaryonun bir örneği, iletileri ya da olayların yayan bir satış noktası (POS) çözümünü göz önünde bulundurun. Event hubs'ı, sonra daha fazla işleme için başka bir sistem için eşlenmiş veri ileten bazı eşleştirme veya yeniden biçimlendirme çözüme olaylar geçirir. Bu noktada, tüm bu sistemlerin aynı Azure bölgesinde barındırılabilir. Karar ne zaman ve hangi bölümünün yük devretme altyapınızdaki veri akışını bağlıdır. 

Yük devretme ile izleme sistemlerinden veya özel olarak geliştirilmiş izleme çözümleri ile otomatik hale getirebilirsiniz. Ancak, bu tür Otomasyon ek planlama ve bu makalenin kapsamı dışında olan iş alır.

### <a name="failover-flow"></a>Yük devretme akışı

Yük devretmeyi başlatın, iki adım gereklidir:

1. Başka bir kesinti oluşursa, yeniden yük devretme yönetebilmek istiyorsunuz. Bu nedenle, başka bir pasif ad alanı ayarlama ve eşleştirmesini güncelleştirin. 

2. Yeniden kullanılabilir olduğunda eski birincil ad alanındaki iletileri çekin. Bundan sonra bu ad alanı normal coğrafi kurtarma kurulumunuzu dışında Mesajlaşma için kullanın veya eski birincil ad alanı silin.

> [!NOTE]
> Yalnızca hata iletme semantiği desteklenir. Bu senaryoda, yük devretme ve ardından yeni bir ad alanı ile yeniden eşleştirin. İlk duruma döndürmeden desteklenmiyor; Örneğin SQL kümesi. 

![2][]

## <a name="management"></a>Yönetim

Bir hata yaptıysanız; Örneğin, ilk kurulum sırasında yanlış bölgede eşleştirilmiş, iki ad alanlarını herhangi bir zamanda eşleştirme bozabilir. Eşleştirilen ad alanları normal bir ad kullanmak istiyorsanız, diğer silin.

## <a name="samples"></a>Örnekler

[GitHub 'daki örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) , bir yük devretme ayarlamayı ve başlatmayı gösterir. Bu örnek aşağıdaki kavramları göstermektedir:

- Event Hubs ile Azure Resource Manager'ı kullanmak için Azure Active Directory'de gerekli ayarları. 
- Örnek kod yürütmek için gerekli adımlar. 
- Geçerli birincil ad alanından gönderip yeniden açın. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürümle birlikte göz önünde tutmak için aşağıdaki konuları göz önünde bulundurun:

1. Tasarım, coğrafi olağanüstü durum kurtarma Event Hubs verileri çoğaltmaz ve bu nedenle ikincil Olay Hub 'ınızdaki birincil olay hub 'ınızın eski değer değerini yeniden kullanamazsınız. Olay alıcılarınızı aşağıdakilerden biriyle yeniden başlatmanız önerilir:

- *Eventposition. FromStart ()* -ikincil Olay Hub 'ınızdaki tüm verileri okumak istiyorsanız.
- *Eventposition. FromEnd ()* -ikincil Olay Hub 'ınıza bağlantı sırasında tüm yeni verileri okumak istiyorsanız.
- *Eventposition. FromEnqueuedTime (DateTime)* -belirli bir tarih ve saatten itibaren ikincil Olay Hub 'ınızdaki alınan tüm verileri okumak istiyorsanız.

2. Planınızı yük devretme saat faktörü düşünmelisiniz. Örneğin, 15-20 dakikadan fazla bağlantısını kaybederseniz, yük devretmeyi başlatmak karar verebilirsiniz. 
 
3. Hiçbir veri çoğaltılır şu anda etkin oturumları değil çoğaltıldığından emin anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanmış iletileri çalışmayabilir. Yeni oturumlar, zamanlanan mesajlar ve yeni yinelenen çalışır. 

4. Karmaşık bir dağıtılmış altyapının yük devretmesi en az bir kez [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) edilmelidir. 

5. Varlık eşitleme dakika başına yaklaşık 50-100 varlık biraz zaman alabilir.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları 

Event Hubs standart SKU, bir Azure bölgesi içinde hataya yalıtılmış konumlar sağlayarak [kullanılabilirlik alanları](../availability-zones/az-overview.md)destekler. 

> [!NOTE]
> Azure Event Hubs Standard için Kullanılabilirlik Alanları desteği yalnızca kullanılabilirlik bölgelerinin bulunduğu [Azure bölgelerinde](../availability-zones/az-overview.md#services-support-by-region) kullanılabilir.

Kullanılabilirlik alanları, yeni ad alanları üzerinde yalnızca, Azure portalını kullanarak etkinleştirebilirsiniz. Olay hub'ları, var olan ad alanlarının geçişini desteklemez. Bölge artıklığı ad alanınızı etkinleştirildikten sonra devre dışı bırakılamıyor.

![3][]

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'daki örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) , coğrafi eşleme oluşturan ve olağanüstü durum kurtarma senaryosu için yük devretme Başlatan basit bir iş akışında gösterilmektedir.
* [REST API başvurusu](/rest/api/eventhub/disasterrecoveryconfigs) , coğrafi olağanüstü durum kurtarma yapılandırmasını gerçekleştirmek Için API 'leri açıklar.

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
