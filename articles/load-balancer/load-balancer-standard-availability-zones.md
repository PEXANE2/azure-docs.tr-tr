---
title: Azure Standart Yük Dengeleyicisi ve Kullanılabilirlik Bölgeleri
titleSuffix: Azure Load Balancer
description: Bu öğrenme yolu ile Azure Standart Yük Dengeleyicisi ve Kullanılabilirlik Bölgeleri ile başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: 5a65982c5c13eb4e4273efcfd8d14910b0f35572
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78197156"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Standard Load Balancer ve Kullanılabilirlik Bölgeleri

Azure Standart Yük [Dengeleyicikullanılabilirlik bölgeleri](../availability-zones/az-overview.md) senaryolarını destekler. Kaynakları bölgelerle hizalayarak ve bölgeler e göre dağıtarak, uçdan uca senaryonuzdaki kullanılabilirliği optimize etmek için Standart Yük Dengeleyicisini kullanabilirsiniz.  Kullanılabilirlik bölgelerinin ne olduğu, hangi bölgelerin şu anda kullanılabilirlik bölgelerini desteklediği ve diğer ilgili kavramlar ve ürünler hakkında rehberlik için [kullanılabilirlik bölgelerini](../availability-zones/az-overview.md) gözden geçirin. Standart Yük Dengeleyicisi ile birlikte kullanılabilirlik bölgeleri, birçok farklı senaryo oluşturabilen geniş ve esnek bir özellik kümesidir.  Bu [kavramları](#concepts) ve temel senaryo [tasarım kılavuzunu](#design)anlamak için bu belgeyi gözden geçirin.

## <a name="availability-zones-concepts-applied-to-load-balancer"></a><a name="concepts"></a>Yük Dengeleyicisine Uygulanan Kullanılabilirlik Bölgeleri kavramları

Yük Dengeleyici kaynağının kendisi bölgeseldir ve asla bölgesel değildir. Yapılandırabileceğiniz şeyin parçalılığı, ön uç, kural ve arka uç havuzu tanımının her yapılandırması ile sınırlandırılmıştır.
Kullanılabilirlik bölgeleri bağlamında, Yük Dengeleyici kuralının davranışı ve özellikleri bölge yedekli veya zonal olarak tanımlanır.  Bölge gereksiz ve zonal bir özelliğin zonality açıklar.  Yük Dengeleyici sbağlamında, bölge yedekli her zaman *birden çok bölge* anlamına gelir ve bölge tek bir *bölgeye*hizmet yalıtmak anlamına gelir.
Hem genel hem de dahili Yük Dengeleyici sebunu yedekli ve bölgesel senaryoları destekler ve her ikisi de gerektiğinde bölgeler arasında trafiği yönlendirebilir *(çapraz bölge yük dengeleme).* 

### <a name="frontend"></a>Ön uç

Load Balancer frontend, bir genel IP adresi kaynağına veya sanal ağ kaynağının alt ağındaki özel bir IP adresine başvuran bir ön uç IP yapılandırmasıdır.  Hizmetinizin açığa çıktığı yük dengeli uç noktasını oluşturur.
Yük Dengeleyici sağlık günü, zonal ve zone yedekli frontends ile kurallar aynı anda içerebilir. Bir genel IP kaynağı veya özel bir IP adresi bir bölgeye garanti edildiğinde, zonality (veya bunların eksikliği) değişmez değildir.  Genel bir IP veya özel IP adresi ön ucunun zonality değiştirmek veya atlamak istiyorsanız, uygun bölgede ortak IP yeniden oluşturmanız gerekir.  Kullanılabilirlik bölgeleri birden çok ön uç için kısıtlamaları değiştirmez, bu yeteneğin ayrıntıları [için Load Balancer için birden çok ön uçgözden.](load-balancer-multivip-overview.md)

#### <a name="zone-redundant"></a>Bölge yedekli 

Kullanılabilirlik bölgeleri olan bir bölgede, Standart Yük Dengeleyici ön uç bölge yedekli olabilir.  Bölge yedekli, gelen veya giden tüm akışların, tek bir IP adresi kullanılarak aynı anda bir bölgedeki birden çok kullanılabilirlik bölgesi tarafından sunulduğu anlamına gelir. DNS artıklık şemaları gerekli değildir. Tek bir frontend IP adresi bölge arızasını atlatabilir ve bölgeye bakılmaksızın tüm (etkilenmemiş) arka uç havuzu üyelerine ulaşmak için kullanılabilir. Bir veya daha fazla kullanılabilirlik bölgesi başarısız olabilir ve bölgedeki bir bölge sağlıklı kaldığı sürece veri yolu devam eder. Frontend'in tek IP adresi, birden çok kullanılabilirlik bölgesinde birden çok bağımsız altyapı dağıtımı yla aynı anda sunulur.  Bu isabetsiz veri yolu anlamına gelmez, ancak herhangi bir yeniden deneme veya yeniden kurulması bölge hatası ndan etkilenmeyen diğer bölgelerde başarılı olacaktır.   

#### <a name="optional-zone-isolation"></a>İsteğe bağlı bölge yalıtımı

Bir ön uç olarak bilinen tek bir bölge için garanti bir ön *uç*seçebilirsiniz.  Bu, herhangi bir gelen veya giden akış bir bölgede tek bir bölge tarafından hizmet anlamına gelir.  Ön yüz, bölgenin sağlığıyla kaderi paylaşıyor.  Veri yolu, garanti edildiği yer dışındaki bölgelerdeki hatalardan etkilenmez. Kullanılabilirlik Bölgesi başına bir IP adresini ortaya çıkarmak için zonal frontends kullanabilirsiniz.  

Ayrıca, her bölge içinde yük dengeli uç noktaları için doğrudan zonal frontends tüketebilirsiniz. Bunu, her bölgeyi ayrı ayrı izlemek için bölge başına yük dengeli uç noktaları ortaya çıkarmak için de kullanabilirsiniz.  Veya ortak uç noktalar için bunları [Traffic Manager](../traffic-manager/traffic-manager-overview.md) gibi bir DNS yük dengeleme ürünüyle tümleştirebilir ve tek bir DNS adı kullanabilirsiniz. İstemci daha sonra bu DNS adını birden çok zonal IP adreslerine çözer.  

Bu kavramları (aynı arka uç için bölge gereksiz ve bölge) karıştırmak istiyorsanız, [Azure Yük Dengeleyicisi için birden çok ön uçtan](load-balancer-multivip-overview.md)gözden geçirin.

Ortak Yük Dengeleyicisi ön uç için, ilgili kural tarafından kullanılan ön uç IP yapılandırması tarafından başvurulan genel IP kaynağına bir *bölge* parametresi eklersiniz.

Dahili Yük Dengeleyici ön uç için, dahili Yük Dengeleyici frontend IP yapılandırmasına bir *bölge* parametresi ekleyin. Zonal frontend, Yük Dengeleyicisinin bir alt ağdaki IP adresini belirli bir bölgeye garanti etmesini sağlar.

### <a name="cross-zone-load-balancing"></a>Çapraz Bölge Yük Dengeleme

Çapraz bölge yük dengeleme, Yük Dengeleyici'nin herhangi bir bölgede bir arka uç uç noktasına ulaşabilmesidir ve ön uçve zonality'den bağımsızdır.  Herhangi bir yük dengeleme kuralı, herhangi bir kullanılabilirlik bölgesinde veya bölgesel örneklerde arka uç örneğini hedefleyebilir.

Senaryonuzu kullanılabilirlik bölgeleri kavramını ifade edecek şekilde oluşturmaya dikkat etmeniz gerekir. Örneğin, sanal makine dağıtımınızı tek bir bölge veya birden çok bölge içinde garanti etmeniz ve zonal frontend ve zonal arka uç kaynaklarını aynı bölgeye hizalamanız gerekir.  Kullanılabilirlik bölgelerini yalnızca bölge kaynaklarıyla geçerseniz, senaryo çalışır, ancak kullanılabilirlik bölgeleriyle ilgili net bir hata moduna sahip olmayabilir. 

### <a name="backend"></a>Arka uç

Yük Dengeleyici sanal makineler örnekleri ile çalışır.  Bunlar tek başına, kullanılabilirlik kümeleri veya sanal makine ölçek kümeleri olabilir.  Tek bir sanal ağdaki herhangi bir sanal makine örneği, bir bölgeye garanti edilip edilmediğine veya hangi bölgeye garanti edildiğine bakılmaksızın arka uç havuzunun bir parçası olabilir.

Ön yüzünüzü ve arka yüzünüzü tek bir bölgeyle hizalamak ve garanti etmek istiyorsanız, aynı bölge içindeki sanal makineleri yalnızca ilgili arka uç havuzuna yerleştirin.

Sanal makinelere birden çok bölgede hitap etmek istiyorsanız, birden çok bölgeden gelen sanal makineleri aynı arka uç havuzuna yerleştirmeniz yeterlidir.  Sanal makine ölçek kümelerini kullanırken, bir veya daha fazla sanal makine ölçek kümesini aynı arka uç havuzuna yerleştirebilirsiniz.  Ve bu sanal makine ölçek kümelerinin her biri tek veya birden çok bölgede olabilir.

### <a name="outbound-connections"></a>Giden bağlantılar

Aynı bölge yedekli ve bölgesel özellikleri [giden bağlantılar](load-balancer-outbound-connections.md)için geçerlidir.  Giden bağlantılar için kullanılan bölge yedekli genel IP adresi tüm bölgeler tarafından sunulur. Bir bölge genel IP adresi yalnızca garanti olduğu bölge tarafından sunulur.  Giden bağlantı SNAT bağlantı noktaları bölge hatalarından kurtulur ve senaryonuz bölge hatasından etkilenmezse giden SNAT bağlantısı sağlamaya devam eder.  Bu, bir akış etkilenen bir bölge tarafından sunulduysa, iletimlerin veya bölgelerin bölge gereksiz senaryoları için yeniden kurulmasını gerektirebilir.  Etkilenen bölgeler dışındaki bölgelerdeki akışlar etkilenmez.

SNAT bağlantı noktası preallocation algoritması kullanılabilirlik bölgeleri ile veya olmadan aynıdır.

### <a name="health-probes"></a>Sistem durumu araştırmaları

Mevcut sağlık sondası tanımlarınız kullanılabilirlik bölgeleri olmadığı için kalır.  Ancak, sağlık modelini altyapı düzeyinde genişlettik. 

Bölge yedekli ön uçlarını kullanırken, Load Balancer her kullanılabilirlik bölgesinden sanal bir makinenin ulaşılabilirliğini bağımsız olarak araştırmak ve müşteri müdahalesi olmadan başarısız olmuş bölgeler arası yolları kapatmak için dahili sistem modelini genişletir.  Belirli bir yol bir bölgenin Yük Dengeleyici altyapısından başka bir bölgede sanal bir makineye kullanılamıyorsa, Yük Dengeleyici bu hatayı algılayabilir ve önleyebilir. Bu VM'ye ulaşabilen diğer bölgeler, VM'ye kendi cephelerinden hizmet vermeye devam edebilirler.  Sonuç olarak, hata olayları sırasında, her bölge uçtan uca hizmetin genel durumunu korurken yeni akışların biraz farklı dağılımlarına sahip olabilir.

## <a name="design-considerations"></a><a name="design"></a>Tasarım konuları

Yük Dengeleyicisi kullanılabilirlik bölgeleri bağlamında kasıtlı olarak esnektir. Bölgelere hizalamayı seçebilir veya her kural için bölge gereksiz olmayı seçebilirsiniz.  Artan kullanılabilirlik, artan karmaşıklık pahasına gelebilir ve en iyi performans için kullanılabilirlik için tasarım yapmalısınız.  Bazı önemli tasarım konuları bir göz atalım.

### <a name="automatic-zone-redundancy"></a>Otomatik bölge artıklığı

Yük Dengeleyici, bölge yedekli ön uç olarak tek bir IP'ye sahip olmayı kolaylaştırır. Bölge yedekli bir IP adresi, herhangi bir bölgede güvenli bir şekilde bir bölge kaynağına hizmet edebilir ve bir bölge bölge içinde sağlıklı kaldığı sürece bir veya daha fazla bölge hatasından kurtulabilir. Tersine, bir bölge frontend tek bir bölgeye hizmetin bir azalma ve ilgili bölge ile kaderi paylaşır.

Bölge artıklığı isabetsiz veri yolu veya kontrol düzlemi anlamına gelmez;  açıkça veri düzlemidir. Bölge yedekli akışlar herhangi bir bölgeyi kullanabilir ve müşterinin akışları bir bölgedeki tüm sağlıklı bölgeleri kullanır. Bölge arızası durumunda, o noktada sağlıklı bölgeleri kullanan trafik akışları etkilenmez.  Bölge arızası sırasında bir bölge kullanarak trafik akışları etkilenebilir, ancak uygulamalar kurtarılabilir. Azure bölge hatası etrafında birleştikten sonra, bu akışlar yeniden iletim veya yeniden kurulması yla bölgedeki kalan sağlıklı bölgelerde devam edebilir.

### <a name="cross-zone-boundaries"></a><a name="xzonedesign"></a>Çapraz bölge sınırları

Bir uçtan uca hizmet bölgeleri her geçtiğinde, kaderi tek bir bölgeyle değil, potansiyel olarak birden fazla bölgeyle paylaştığınızı anlamak önemlidir.  Sonuç olarak, uçlardan uca hizmetiniz dağıtımları üzerinde herhangi bir kullanılabilirlik kazanmış olmayabilir.

Kullanılabilirlik bölgelerini kullanırken kullanılabilirlik kazanımlarını geçersiz kılacak istenmeyen bölgeler arası bağımlılıklar getirmekten kaçının.  Uygulamanız birden çok bileşenden oluşuyorsa ve bölge arızalarına karşı dirençli olmak istiyorsanız, bir bölgenin arızalanması durumunda yeterli kritik bileşenlerin hayatta kalmasını sağlamaya dikkat etmelidir.  Örneğin, uygulamanız için tek bir kritik bileşen, yalnızca kalan bölge(ler dışında bir bölgede varsa) tüm uygulamanızı etkileyebilir.  Buna ek olarak, bölge restorasyonunu ve uygulamanızın nasıl yakınsadığını da göz önünde bulundurun. Uygulamanızın bazı bölümlerinin arızaları ile ilgili nedenlerini anlamanız gerekir. Bazı önemli noktaları gözden geçirelim ve bunları belirli senaryonuzda düşündüğünüz gibi sorular için ilham kaynağı olarak kullanalım.

- Uygulamanızda IP adresi ve yönetilen diske sahip sanal makine gibi iki bileşen varsa ve bunlar bölge 1 ve bölge 2'de garanti lenmişse, bölge 1 başarısız olduğunda, bölge 1 başarısız olduğunda, bölge 1 başarısız olduğunda hizmethayatta kalmaz.  Potansiyel olarak tehlikeli bir hata modu oluşturduğunuzu tam olarak anlamadığınız sürece bölge senaryolarıyla bölgeleri geçmeyin.  Bu senaryonun esneklik sağlamasına izin verilir.

- Uygulamanızda IP adresi ve yönetilen diske sahip sanal makine gibi iki bileşen varsa ve sırasıyla bölge yedekli ve bölge 1 olması garanti edilirse, bölge 1 başarısız olmadıkça uçtan uca hizmetiniz bölge 2, bölge 3 veya her ikisinin de hatalarından kurtulur.  Ancak, tüm gözlemlediğiniz ön uç ulaşılabilirlik ise hizmetin sağlık hakkında neden bazı yeteneği nizi kaybedersiniz.  Daha kapsamlı bir sağlık ve kapasite modeli geliştirmeyi düşünün.  Öngörü ve yönetilebilirliği genişletmek için bölge gereksiz ve bölge kavramlarını birlikte kullanabilirsiniz.

- Uygulamanızda bölge yedekli Yük Dengeleyici ön ucu ve üç bölgede ayarlanmış bir çapraz bölge sanal makine ölçeği gibi iki bileşen varsa, hatadan etkilenmediği bölgelerdeki kaynaklarınız kullanılabilir ancak uçtan uca hizmet kapasiteniz bozulabilir bölge arızası sırasında. Altyapı açısından bakıldığında, dağıtımınız bir veya daha fazla bölge hatasından kurtulabilir ve bu da aşağıdaki soruları ortaya çıkarır:
  - Uygulamanızın bu tür arızalar ve azalan kapasite yle ilgili nedenlerini anlıyor musunuz?
  - Gerekirse bölge çiftini başarısız olmaya zorlamak için hizmetinizde güvenlik önlemlerine ihtiyacınız var mı?
  - Böyle bir senaryoyu nasıl izleyecek, algılayacak ve hafifleteceksiniz? Uçtan uca hizmet performansınızın izlenmesini artırmak için Standart Yük Dengeleyici tanılama yöntemini kullanabilirsiniz. Tam bir resim için nelerin kullanılabilip neleri büyütmesi gerekebileceğini göz önünde bulundurun.

- Bölgeler hataları daha kolay anlaşılır ve kontrol altına alabilir hale getirebilir.  Ancak, zaman ayırışları, yeniden denemeler ve geri dönüş algoritmaları gibi kavramlar söz konusu olduğunda, bölge hatası diğer hatalardan farklı değildir. Azure Yük Bakiyesi, bölge gereksiz yollar sunsa ve hızlı bir şekilde kurtarmaya çalışsa da, gerçek zamanlı olarak paket düzeyinde yeniden aktarımlar veya yeniden yüklemeler bir hatanın başlangıcında oluşabilir ve uygulamanızın nasıl başa çıkacağını anlamak önemlidir Başarısızlık. Yük dengeleme şemanız hayatta kalacaktır, ancak aşağıdakileri planlamanız gerekir:
  - Bir bölge başarısız olduğunda, uçuça kadar hizmetiniz bunu anlıyor mu ve devlet kaybedilirse, nasıl toparlanacaksınız?
  - Bir bölge döndüğünde, uygulamanız güvenli bir şekilde nasıl yakınsalayacağınızı anlıyor mu?

Uygulamanızın hata senaryolarına karşı dayanıklılığını artırmak için [Azure bulut tasarım desenlerini](https://docs.microsoft.com/azure/architecture/patterns/) gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar
- [Kullanılabilirlik Bölgeleri](../availability-zones/az-overview.md) hakkında daha fazla bilgi edinin
- [Standart Yük Dengeleyicisi](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- Bölge ön ucuna sahip [standart yük dengeleyicisini kullanarak bir bölge içindeki denge VM'lerini](load-balancer-standard-public-zonal-cli.md) nasıl yükleyini öğrenin
- [Bölge yedekli ön uça sahip standart yük dengeleyicisini kullanarak denge VM'lerini bölgeler arasında](load-balancer-standard-public-zone-redundant-cli.md) nasıl yükleyeceğizi öğrenin
- Uygulamanızın hata senaryolarına dayanıklılığını artırmak için [Azure bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns/) hakkında bilgi edinin.
