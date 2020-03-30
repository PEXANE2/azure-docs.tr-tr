---
title: Azure Servis Veri Yolu Coğrafi felaket kurtarma | Microsoft Dokümanlar
description: Azure Hizmet Veri Yolu'nda olağanüstü durum kurtarma gerçekleştirmede başarısız olmak ve gerçekleştirmeyapmak için coğrafi bölgeleri kullanma
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 24d6658733ea38c15f0673d10db3c0ff5ef51c23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259584"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Servis Veri Yolu Coğrafi felaket kurtarma

Tüm Azure bölgeleri veya veri merkezleri [(kullanılabilirlik bölgeleri](../availability-zones/az-overview.md) kullanılıyorsa) kapalı kalma süresi yle karşılaştığında, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam etmesi çok önemlidir. Bu nedenle, *Jeo-felaket kurtarma* herhangi bir kuruluş için önemli bir özelliktir. Azure Hizmet Veri Yolu, ad alanı düzeyinde coğrafi felaket kurtarmayı destekler.

Geo-disaster kurtarma özelliği, Service Bus Premium SKU için küresel olarak kullanılabilir. 

>[!NOTE]
> Coğrafi Durum kurtarma şu anda yalnızca meta verilerin (Kuyruklar, Konular, Abonelikler, Filtreler) eşleştirildiğinde birincil ad alanından ikincil ad alanına kopyalanmasını sağlar.

## <a name="outages-and-disasters"></a>Kesintiler ve felaketler

"Kesintiler" ve "felaketler" arasındaki ayrımı dikkate almak önemlidir. 

*Kesinti,* Azure Hizmet Veri Servisi'nin geçici olarak kullanılamamasıdır ve ileti deposu ve hatta tüm veri merkezi gibi hizmetin bazı bileşenlerini etkileyebilir. Ancak, sorun giderildikten sonra Servis Veri Servisi yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletilerin veya diğer verilerin kaybına neden olmaz. Böyle bir kesintiörneği veri merkezinde bir güç kesintisi olabilir. Bazı kesintiler geçici veya ağ sorunları nedeniyle yalnızca kısa bağlantı kayıplarıdır. 

*Bir felaket,* bir Hizmet Veri Aracı kümesinin, Azure bölgesinin veya veri merkezinin kalıcı veya uzun vadeli kaybı olarak tanımlanır. Bölge veya veri merkezi yeniden kullanılabilir olabilir veya olmayabilir veya saatlerce veya günlerdir kapanabilir. Bu tür felaketlere örnek olarak yangın, sel veya deprem verilebilir. Kalıcı hale gelen bir felaket, bazı iletilerin, olayların veya diğer verilerin kaybolmasına neden olabilir. Ancak, çoğu durumda veri kaybı olmamalıdır ve veri merkezi yedeklendikten sonra iletiler kurtarılabilir.

Azure Servis Veri Yolu'nun Coğrafi felaket kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan kavramlar ve iş akışı, geçici veya geçici kesintiler için değil, felaket senaryoları için geçerlidir. Microsoft Azure'da olağanüstü durum kurtarma yla ilgili ayrıntılı bir tartışma için [bu makaleye](/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.   

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü durum kurtarma özelliği meta veri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarına dayanır. Geo-disaster kurtarma özelliğinin yalnızca [Premium SKU](service-bus-premium-messaging.md) için kullanılabildiğini unutmayın. Bağlantı bir takma ad aracılığıyla yapıldığından, bağlantı dizedeğişiklikleri yapmanız gerekmez.

Bu makalede aşağıdaki terimler kullanılmıştır:

-  *Diğer Ad*: Ayarladığınız olağanüstü durum kurtarma yapılandırmasının adı. Takma ad, tek bir kararlı Tam Nitelikli Etki Alanı Adı (FQDN) bağlantı dizesini sağlar. Uygulamalar, ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanır. Takma ad kullanmak, hata tetiklendiğinde bağlantı dizesinin değişmemesini sağlar.

-  *Birincil/ikincil ad alanı*: Diğer ada karşılık gelen ad alanları. Birincil ad alanı "etkin" dir ve iletileri alır (bu varolan veya yeni bir ad alanı olabilir). İkincil ad alanı "pasif" dir ve ileti almaz. Her ikisi arasındaki meta veriler eşitlenir, böylece her ikisi de herhangi bir uygulama kodu veya bağlantı dizesi değişiklikleri olmadan iletileri sorunsuz bir şekilde kabul edebilir. Yalnızca etkin ad alanının ileti aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veriler*: Kuyruklar, konular ve abonelikler gibi varlıklar; ve ad alanıile ilişkili hizmetin özellikleri. Yalnızca varlıkların ve ayarlarının otomatik olarak çoğaltıldığını unutmayın. İletiler çoğaltılamaz.

-  *Failover*: İkincil ad alanını etkinleştirme işlemidir.

## <a name="setup"></a>Kurulum

Aşağıdaki bölüm, ad alanları arasındaki kurulum eşleştirmesine genel bir bakıştır.

![1][]

Kurulum işlemi aşağıdaki gibidir -

1. ***Birincil*** Hizmet Veri Günü Premium Ad Alanı sağlama.

2. *Birincil ad alanının sağlandığı*bölgeden farklı bir bölgede ***İkincil*** Hizmet Veri Günü Premium Ad Alanı sağlama. Bu, farklı veri merkezi bölgeleri arasında hata yalıtımına izin verir.

3. ***Diğer adı***elde etmek için Birincil ad alanı ve İkincil ad alanı arasında eşleştirme oluşturun.

    >[!NOTE] 
    > [Azure Hizmet Veri Yolu Standart ad alanınızı Azure Hizmet Veri Yolu Premium'a geçtiyseniz,](service-bus-migrate-standard-premium.md) **PS/CLI** veya **REST API**aracılığıyla olağanüstü durum kurtarma yapılandırmasını oluşturmak için önceden varolan diğer adı (örneğin Service Bus Standard ad alanı bağlantı dizenizi) kullanmanız gerekir.
    >
    >
    > Bunun nedeni, geçiş sırasında Azure Hizmet Veri Kurumu Standart ad alanı bağlantı dizeniz/DNS adınız Azure Hizmet Veri Günü Premium ad alanınızın diğer adı haline gelir.
    >
    > İstemci uygulamalarınız, olağanüstü durum kurtarma eşleştirmesinin kurulum yaptığı Premium ad alanına bağlanmak için bu diğer adı (örneğin Azure Hizmet Veri Yolu Standart ad alanı bağlantı dizesini) kullanmalıdır.
    >
    > Portalı Olağanüstü Durum kurtarma yapılandırmasını kurmak için kullanırsanız, portal bu uyarıyı sizden soyutlar.


4. İstemci uygulamalarınızı Geo-DR etkin birincil ad alanına bağlamak için adım 3'te elde edilen ***diğer adı*** kullanın. Başlangıçta, diğer ad birincil ad alanına işaret edir.

5. [İsteğe bağlı] Bir hata nın gerekli olup olmadığını algılamak için bazı izlemeler ekleyin.

## <a name="failover-flow"></a>Başarısız akış

Bir hata, müşteri tarafından el ile tetiklenir (açıkça bir komut aracılığıyla veya komutu tetikleyen istemciye ait iş mantığı aracılığıyla) ve asla Azure tarafından değil. Bu, müşteriye Azure'un omurgası üzerindeki kesinti çözünürlüğü için tam sahiplik ve görünürlük sağlar.

![4][]

Failover tetiklendikten sonra -

1. ***Diğer ad*** bağlantı dizesi, İkincil Premium ad alanına işaret etmek için güncelleştirilir.

2. İstemciler (gönderenler ve alıcılar) Otomatik olarak İkincil ad alanına bağlanır.

3. Birincil ve İkincil premium ad alanı arasındaki varolan eşleştirme bozuldu.

Bir kez failover başlatılır -

1. Başka bir kesinti oluşursa, tekrar başarısız olmak isteyebilirsiniz. Bu nedenle, başka bir pasif ad alanı ayarlayın ve eşleştirmegüncelleştirin. 

2. Yeniden kullanılabilir olduğunda eski birincil ad alanından iletileri çekin. Bundan sonra, bu ad alanını coğrafi kurtarma kurulumuzun dışında düzenli ileti için kullanın veya eski birincil ad alanını silin.

> [!NOTE]
> Sadece ileri semantik desteklenir başarısız. Bu senaryoda, üzerinde başarısız ve sonra yeni bir ad alanı ile yeniden eşleştirmek. Geri başarısız desteklenmez; örneğin, bir SQL kümesinde. 

Failover'ı izleme sistemleriyle veya özel olarak oluşturulmuş izleme çözümleriyle otomatikleştirebilirsiniz. Ancak, bu tür otomasyon bu makalenin kapsamı dışında ekstra planlama ve iş alır.

![2][]

## <a name="management"></a>Yönetim

Eğer bir hata yaptıysanız; örneğin, ilk kurulum sırasında yanlış bölgeleri eşlediniz, istediğiniz zaman iki ad alanının eşleşmesini kırabilirsiniz. Eşleştirilmiş ad alanlarını normal ad alanları olarak kullanmak istiyorsanız, diğer adı silin.

## <a name="use-existing-namespace-as-alias"></a>Varolan ad alanını diğer ad olarak kullanma

Üreticilerin ve tüketicilerin bağlantılarını değiştiremeyeceğiniz bir senaryonuz varsa, ad alanı adınızı takma ad olarak yeniden kullanabilirsiniz. [GitHub'daki örnek kodu burada](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name)görebilirsiniz.

## <a name="samples"></a>Örnekler

[GitHub'daki örnekler,](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) nasıl bir hata ayarlayıp başlatılabildiğini gösterir. Bu örnekler aşağıdaki kavramları göstermektedir:

- Bir .NET örneği ve Azure Etkin Dizin'de Hizmet Veri Yolu ile Azure Kaynak Yöneticisi'ni kullanmak, Coğrafi felaket kurtarmayı ayarlamak ve etkinleştirmek için gereken ayarlar.
- Örnek kodu yürütmek için gereken adımlar.
- Varolan bir ad alanı takma ad olarak nasıl kullanılır?
- PowerShell veya CLI aracılığıyla Coğrafi felaket kurtarmayı alternatif olarak etkinleştirme adımları.
- Takma adı kullanarak geçerli birincil veya ikincil ad alanından [gönderme ve alma.](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürümde göz önünde bulundurulması gereken hususlara dikkat edin:

1. Başarısız planlama, aynı zamanda zaman faktörü düşünmelisiniz. Örneğin, bağlantınızı 15 ila 20 dakikadan uzun süre kaybederseniz, başarısızlığı başlatmaya karar verebilirsiniz.

2. Hiçbir verinin çoğaltılmadı olması, şu anda etkin oturumların çoğaltılmadı anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanan iletiler çalışmayabilir. Yeni oturumlar, yeni zamanlanmış iletiler ve yeni yinelenenler çalışacaktır. 

3. Karmaşık dağıtılmış bir altyapı üzerinde başarısız en az bir kez [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) edilmelidir.

4. Eşitleme varlıkları, dakikada yaklaşık 50-100 varlıklar biraz zaman alabilir. Abonelikler ve kurallar da varlıklar olarak sayılır.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Service Bus Premium SKU, Azure bölgesinde hataya yalıtılmış konumlar sağlayarak [Kullanılabilirlik Bölgelerini](../availability-zones/az-overview.md)de destekler.

> [!NOTE]
> Azure Hizmet Veri Servisi Premium için Kullanılabilirlik Bölgeleri desteği yalnızca kullanılabilirlik bölgelerinin bulunduğu [Azure bölgelerinde](../availability-zones/az-overview.md#services-support-by-region) kullanılabilir.

Kullanılabilirlik Bölgelerini yalnızca yeni ad alanlarında Azure portalını kullanarak etkinleştirebilirsiniz. Hizmet Veri Servisi varolan ad boşluklarının geçişini desteklemez. Ad alanınızda etkinleştirdikten sonra bölge artıklığını devre dışı bırakamazsınız.

![3][]

## <a name="next-steps"></a>Sonraki adımlar

- Geo-afet kurtarma [REST API referans burada](/rest/api/servicebus/disasterrecoveryconfigs)bakın.
- [GitHub'da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2)Geo-disaster kurtarma örneğini çalıştırın.
- [İletileri başka bir takma ada gönderen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1)Coğrafi felaket kurtarma örneğine bakın.

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [Dinlenme API'si](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png
