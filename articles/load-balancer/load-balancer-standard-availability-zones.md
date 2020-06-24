---
title: Azure Standart Load Balancer ve Kullanılabilirlik Alanları
titleSuffix: Azure Load Balancer
description: Bu öğrenme yoluyla Azure Standart Load Balancer ve Kullanılabilirlik Alanları ile çalışmaya başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2020
ms.author: allensu
ms.openlocfilehash: 2c16c40ba7ed97f933516811432c3d4768bd0c62
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84888485"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Standard Load Balancer ve Kullanılabilirlik Bölgeleri

Azure Standart Load Balancer, kullanılabilirlik alanları senaryolarını destekler. Standart yük dengeleyiciyi kullanarak, kaynakları ve bölgeler arasında dağıtımı yaparak senaryonuz genelinde kullanılabilirliği artırabilirsiniz. Standart yük dengeleyici ile birlikte kullanılabilirlik alanları, birçok farklı senaryo oluşturabileceğiniz bir expantik ve esnek özellik kümesidir.  Bu [kavramları](#concepts) ve temel senaryo [tasarım kılavuzunu](#design)anlamak için bu belgeyi gözden geçirin.

## <a name="availability-zones-concepts-applied-to-load-balancer"></a><a name="concepts"></a>Load Balancer uygulanan Kullanılabilirlik Alanları kavramları

Yük dengeleyici, bileşenlerinden bölge yapılandırmasını devralır: 

* Ön uç
* Kurallar
* Arka uç havuzu tanımı

Kullanılabilirlik alanları bağlamında, bir yük dengeleyici kuralının davranışı ve özellikleri, bölgesel olarak yedekli veya zonal olarak açıklanmaktadır.  Yük dengeleyici bağlamında, bölgesel olarak yedekli her zaman **birden çok bölge** ve ZGen, hizmeti **tek bir bölgeye**ayırma anlamına gelir. Azure Load Balancer, ortak ve dahili iki tür içerir. Her iki yük dengeleyici türü de bölge yedekliliği ve bölgesel dağıtımını destekler.  Her iki yük dengeleyici türü, gerektiğinde trafiği bölgeler arasında yönlendirebilir.

## <a name="frontend"></a>Ön uç

Yük dengeleyici ön ucu, bir sanal ağın alt ağı içinde genel bir IP adresine veya özel bir IP adresine başvuran bir ön uç IP yapılandırması olabilir. Hizmetinizin açığa çıkarılabileceği yük dengeli uç noktayı oluşturur.

Yük dengeleyici kaynağı, aynı anda bölgesel ve bölgesel olarak yedekli ön uçları içeren kurallar içerebilir. Bir bölgenin ortak veya özel IP 'si garanti edildiğinde, bölge bölgesi (veya olmaması) değiştirilemez. Genel veya özel IP adresi ön ucu 'nın bölge düzeyini değiştirmek veya atlamak için, IP 'yi uygun bölgede yeniden oluşturun. 

Kullanılabilirlik alanları, birden fazla ön uçların kısıtlamalarını değiştirmez. Bu özellik için Ayrıntılar için [birden çok ön uç Load Balancer](load-balancer-multivip-overview.md) inceleyin.

### <a name="zone-redundant"></a>Bölge yedekli 

Kullanılabilirlik alanları olan bir bölgede, standart yük dengeleyici ön ucu bölge yedekli olabilir. Birden çok bölge, bir bölgede gelen veya giden trafiğe sunabilir. Bu trafik, tek bir IP adresi tarafından sunulur. DNS artıklık şemaları gerekli değildir. 

Tek bir ön uç IP adresi bölge hatasını sürdüren. Ön uç IP, bölge ne olduğuna bakılmaksızın, tüm etkilenmeyen arka uç havuzu üyelerine erişmek için kullanılabilir. Bir veya daha fazla kullanılabilirlik bölgesi başarısız olabilir ve bölgedeki bir bölge sağlıklı kaldığı sürece veri yolu daha uzundur. 

Ön uç 'nin IP adresi birden çok kullanılabilirlik alanında birden çok bağımsız altyapı dağıtımı tarafından aynı anda sunulur. Herhangi bir yeniden deneme veya yeniden yapılandırma, bölge hatasından etkilenmeyen diğer bölgelerde başarılı olur. 

<p align="center">
  <img src="./media/az-zonal/zone-redundant-lb-1.svg" width="512" title="Sanal ağ NAT">
</p>

*Şekil: bölge yedekli yük dengeleyici*

### <a name="zonal"></a>Bölgesel

Tek bir bölge için bir ön uç garantisini seçebilirsiniz ve bu da *Bölgesel ön*ucu olarak bilinir.  Bu senaryo, herhangi bir gelen veya giden akışın bir bölgedeki tek bir bölge tarafından sunulduğunu gösterir.  Ön uç paylaşımlarınız, bölgenin sistem durumuyla birlikte fada.  Veri yolu, garantide olduğu yerde yer alan bölgelerde hatalara göre etkilenmemiştir. Kullanılabilirlik alanı başına bir IP adresi göstermek için, bölgesel ön uçları kullanabilirsiniz.  

Ayrıca, her bir bölgedeki yük dengeli uç noktalar için doğrudan bölgesel ön uçları kullanılması desteklenir. Her bölgeyi tek tek izlemek için, bu yapılandırmayı her bölge için yük dengeli uç nokta başına göstermek üzere kullanabilirsiniz. Ortak uç noktalar için, bunları [Traffic Manager](../traffic-manager/traffic-manager-overview.md) gıbı bir DNS Yük Dengeleme ürünüyle tümleştirilebilir ve tek bir DNS adı kullanabilirsiniz.


<p align="center">
  <img src="./media/az-zonal/zonal-lb-1.svg" width="512" title="Sanal ağ NAT">
</p>

*Şekil: ZGen yedekli yük dengeleyici*

Bu kavramları karıştırmak istiyorsanız (aynı arka uç için bölgesel olarak yedekli ve bölgesel), [Azure Load Balancer için birden fazla](load-balancer-multivip-overview.md)ön eki gözden geçirin.

Ortak yük dengeleyici ön ucu için genel IP 'ye bir **bölgeler** parametresi eklersiniz. Bu genel IP 'ye ilgili kural tarafından kullanılan ön uç IP yapılandırması tarafından başvurulur.

İç yük dengeleyici ön ucu için iç yük dengeleyici ön uç IP yapılandırmasına bir **bölgeler** parametresi ekleyin. Bir bölgesel ön ucu, alt ağdaki bir IP adresinin belirli bir bölgeye göre olmasını sağlar.

## <a name="backend"></a>Arka uç

Azure Load Balancer sanal makine örnekleriyle birlikte kullanılabilir. Bu örnekler tek başına, kullanılabilirlik kümelerine veya sanal makine ölçek kümelerine sahip olabilir. Tek bir sanal ağdaki herhangi bir sanal makine, sanal makinenin garantisi olduğu bölge olan arka uç havuzunun bir parçası olabilir.

Ön uç ve arka ucunuzu tek bir bölge ile hizalamak ve garantilemek için, yalnızca aynı bölgedeki sanal makineleri ilgili arka uç havuzuna yerleştirebilirsiniz.

Birden çok bölge genelinde sanal makineleri ele almak için, sanal makineleri birden çok bölgede aynı arka uç havuzuna yerleştirin. 

Sanal Makine Ölçek Kümeleri kullanırken, bir veya daha fazla sanal makine ölçek kümesini aynı arka uç havuzuna yerleştirin. Ölçek Kümeleri tek veya birden çok bölgede olabilir.

## <a name="outbound-connections"></a>Giden bağlantılar

Bölge yedekli ve bölgesel, [giden bağlantılara](load-balancer-outbound-connections.md)uygulanır. Giden bağlantılar için kullanılan bölge yedekli genel IP adresi tüm bölgeler tarafından sunulur. Bir genel IP adresi yalnızca içinde garanti ettiği bölge tarafından sunulur. 

Giden bağlantı SNAT bağlantı noktası ayırmaları kalan bölge hataları ve senaryonuz, bölge hatasından etkilenmedikleri takdirde giden SNAT bağlantısı sağlamaya devam edecektir. Trafiğe etkilenen bir bölge tarafından sunulduysa, bölge arızaları, bölgesel olarak yedekli senaryolar için bağlantıların yeniden oluşturulması gerekebilir. Etkilenen bölgeler dışındaki bölgelerde yer alan akışlar etkilenmez.

SNAT bağlantı noktası ön ayırma algoritması, kullanılabilirlik alanları olmadan veya bunlarla aynı değildir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Mevcut durum araştırma tanımlarınız, kullanılabilirlik alanları olmayan gibi kalır. Ancak sistem durumu modelini bir altyapı düzeyinde genişlettik. 

Bölgesel olarak yedekli ön uçlar kullanılırken, yük dengeleyici iç sistem durumu denetimini genişletir. Yük dengeleyici, her bir bölgeden bir sanal makinenin kullanılabilirliğini bağımsız olarak yoklamakta ve müdahale etmeden başarısız olan bölgelerde yolları kapatır.  

Bu sanal makineye erişebilen diğer bölgeler, sanal makineyi ilgili ön uçlarından sunmaya devam edebilir. Hata olayları sırasında, hizmetinizin genel sistem durumunu korurken her bir bölgenin farklı yeni akış dağıtımları olabilir.

## <a name="design-considerations"></a><a name="design"></a>Tasarım konuları

Yük dengeleyici, kullanılabilirlik alanları bağlamında esnektir. Bölgelere hizalamayı veya her kural için bölgeye yedekli olmasını seçebilirsiniz. Artan kullanılabilirlik, artan karmaşıklık fiyatına göre gelebilir. En iyi performans için kullanılabilirlik tasarımı.

### <a name="automatic-zone-redundancy"></a>Otomatik bölge-artıklık

Load Balancer, bölge yedekli ön uç olarak tek bir IP 'nin olmasını kolaylaştırır. Bölgesel olarak yedekli bir IP adresi, herhangi bir bölgede bir bölgesel kaynağı sunabilir.  Bir bölge bölgede sağlıklı kaldığı sürece IP bir veya daha fazla bölge hatasını daha fazla sürebilir.  Bunun yerine, bir bölgesel ön ucu hizmetin tek bir bölgeye bir azalmasıyla birlikte Fate 'yı ilgili bölge ile paylaşır.

Bölge artıklığı, itless veri yolu veya denetim düzlemi göstermez; veri düzlemi. Bölgesel olarak yedekli akışlar herhangi bir bölgeyi kullanabilir ve bir müşterinin akışları bir bölgedeki tüm iyi alanları kullanır. Bir bölge hatasında, sağlıklı bölgeler kullanılarak trafik akışları etkilenmez. 

Bölge hatası sırasında bölge kullanan trafik akışları etkilenebilir, ancak uygulamalar kurtarabilir. Trafik, Azure 'un bölge hatasını aşmak için yeniden aktarım sırasında bölge içindeki sağlıklı bölgelerde devam eder.

### <a name="cross-zone-boundaries"></a><a name="xzonedesign"></a>Çapraz bölge sınırları

Bir hizmetin bölgeleri ne zaman söylediğinden emin olmak önemlidir, Fate 'yi tek bir bölge ve potansiyel olarak birden çok bölge ile paylaşabilirsiniz. Sonuç olarak, hizmetiniz, hiç türlü dağıtım dışı dağıtımlar üzerinde herhangi bir kullanılabilirlik kazanmamış olabilir.

Kullanılabilirlik alanları kullanılırken kullanılabilirlik kazançlarını null olarak belirleyen, istenmeyen bölgeler arası bağımlılıklardan kaçının. Uygulamanız birden çok kritik bileşen içeriyorsa, bir bölgenin başarısız olması durumunda kalan değer olduğundan emin olun.

Uygulamanız için tek bir kritik bileşen yalnızca, kalan bölge (ler) den başka bir bölgede mevcutsa uygulamanızın tamamını etkileyebilir. Bölge geri yüklemesini ve uygulamanızın nasıl yakınlaşmasını göz önünde bulundurun. Uygulamanızın bölümlerinin hatalara nasıl yanıt vereceğini anlayın. Önemli noktaları gözden geçirin ve size özel senaryolarınızı düşünürken soruları kullanın.

- Uygulamanızın iki bileşeni varsa:

    * IP adresi
    * Yönetilen diske sahip sanal makine

Bunlar bölge 1 ve bölge 2 ' de yapılandırılır. Bölge 1 başarısız olduğunda hizmetiniz devam görmez. Potansiyel olarak tehlikeli bir hata modu oluşturmadığınızı tam olarak anlamadığınız müddetçe, Dilimsiz senaryolarla çapraz bölgeler oluşturmayın. Bu senaryonun esneklik sağlamasına izin verilir.

- Uygulamanızın iki bileşeni varsa:

    * IP adresi
    * Yönetilen diske sahip sanal makine

Bunlar, bölge yedekli ve bölge 1 olacak şekilde yapılandırılmıştır. Bölge 1 başarısız olmadığı takdirde hizmetiniz bölge 2, bölge 3 veya her ikisi de bölge arızasından kaçınacaktır. Ancak, tüm gözlerinizin ön ucu için ulaşılabilirlik durumunda hizmetinizin sistem durumu hakkında bir neden özelliği kaybedersiniz.  Daha kapsamlı bir sistem durumu ve kapasite modeli geliştirmeyi düşünün.  İçgörüler ve yönetilebilirlik ' i genişletmek için, bölgesel olarak yedekli ve bölgesel kavramlarını birlikte kullanabilirsiniz.

- Uygulamanızın iki bileşeni varsa:

    * Bölge yedekli yük dengeleyici ön ucu
    * Üç bölgede çapraz bölge sanal makine ölçek kümesi

Hata ile etkilenmeyen bölgelerdeki kaynaklarınız kullanılabilir olacaktır. Arıza sırasında hizmet kapasiteniz azaltılabilir. Bir altyapı perspektifinden, dağıtımınız bir veya daha fazla bölge hatasından devam edebilir. Bu senaryo aşağıdaki soruları başlatır:

  - Uygulamanızın hatalara ve düşürülmüş kapasiteye nasıl yanıt verdiğini anlamış musunuz?
  - Gerekirse bir bölge çiftine yük devretmeyi zorlamak için hizmetinize korumalar olması gerekiyor mu?
  - Böyle bir senaryoyu nasıl izleyebilir, algılayabilir ve azaltabilirsiniz? Standart yük dengeleyici tanılamayı, hizmet performanlarınızın izlenmesini artırmak için kullanabilirsiniz. Nelerin kullanılabilir olduğunu ve genişletmede neler olabileceğini göz önünde bulundurun.

- Bölgeler, hataların daha kolay anlaşılır ve dahil olmasını sağlayabilir. Bölge hatası, zaman aşımları, yeniden denemeler ve geri alma algoritmalarına geldiğinde diğer hatalardan farklı değildir. Azure Load Balancer, bölgesel olarak yedekli yollar sağlar. Yük dengeleyici, bir paket düzeyinde gerçek zamanlı olarak hızlı bir şekilde kurtarmaya çalışır. Yeniden aktarımlar veya reestablishments bir hata başladıkları noktada sırasında gerçekleşebilir. Uygulamanızın hatalarla nasıl kopdığına anlamak önemlidir. Yük Dengeleme şemanızın devam edecek, ancak aşağıdaki soruları planlamanız gerekir:

  - Bir bölge başarısız olduğunda, hizmetiniz bu hatayı anladığında durum kaybedilmişse nasıl kurtarılır?
  - Bir bölge döndürüldüğünde, uygulamanız güvenle nasıl yakınsama yapılacağını anlasın mı?

Uygulamanızın hata senaryolarıyla olan dayanıklılığını artırmak için [Azure bulut tasarım modellerini](https://docs.microsoft.com/azure/architecture/patterns/) gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar
- [Kullanılabilirlik alanları](../availability-zones/az-overview.md) hakkında daha fazla bilgi edinin
- [Standart Load Balancer](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- Bir [bölge Içinde VM 'lerin yükünü bir standart Load Balancer kullanarak bir bölgede yük ön uç ile yükleme](load-balancer-standard-public-zonal-cli.md) hakkında bilgi edinin
- Bölgesel olarak [yedekli bir ön uç ile standart Load Balancer kullanarak sanal makinelerin bölge üzerinden yük dengelenmesi](load-balancer-standard-public-zone-redundant-cli.md) hakkında bilgi edinin
- Uygulamanızın hata senaryolarıyla olan dayanıklılığını artırmak için [Azure bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns/) hakkında bilgi edinin.
