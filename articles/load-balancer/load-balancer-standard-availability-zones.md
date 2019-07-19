---
title: Azure Standart Load Balancer ve Kullanılabilirlik Alanları
titlesuffix: Azure Load Balancer
description: Standard Load Balancer ve Kullanılabilirlik Bölgeleri
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: allensu
ms.openlocfilehash: 5ef7de148d5ef4727602b8287164f2aff9ccf822
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274509"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Standard Load Balancer ve Kullanılabilirlik Bölgeleri

Azure Load Balancer standart SKU 'SU [kullanılabilirlik alanları](../availability-zones/az-overview.md) senaryolarını destekler. Standart Load Balancer ile, kaynakları bölgelere ayırarak ve bunları bölgelere dağıtarak uçtan uca senaryolarınızda kullanılabilirliği en iyileştirmenize olanak tanıyan birkaç yeni kavram vardır.  Kullanılabilirlik Alanları ne olduğunu, hangi bölgelerin Kullanılabilirlik Alanları desteklediğini ve diğer ilgili kavramları ve ürünleri hakkında rehberlik için [kullanılabilirlik alanları](../availability-zones/az-overview.md) gözden geçirin. Standart Load Balancer birlikte Kullanılabilirlik Alanları, birçok farklı senaryo oluşturabileceğiniz bir expantik ve esnek özellik kümesidir.  Bu [kavramları](#concepts) ve temel senaryo [tasarım kılavuzunu](#design)anlamak için bu belgeyi gözden geçirin.

>[!IMPORTANT]
>Bölgeye özgü bilgiler de dahil olmak üzere ilgili konular için [kullanılabilirlik alanları](../availability-zones/az-overview.md) gözden geçirin.

## <a name="concepts"></a>Load Balancer uygulanan Kullanılabilirlik Alanları kavramları

Load Balancer kaynakları ile gerçek altyapı arasında doğrudan ilişki yoktur; Load Balancer oluşturmak bir örnek oluşturmaz. Load Balancer kaynaklar, Azure 'un, oluşturmak istediğiniz senaryoya ulaşmak için önceden oluşturulmuş çok kiracılı altyapısını nasıl programtabileceği hakkında ifade ettiğiniz nesnelerdir.  Tek bir Load Balancer kaynağı birden çok Kullanılabilirlik Alanları altyapı programlamayı bir müşteri bakış noktasından tek kaynak olarak göründüğü için bu Kullanılabilirlik Alanları bağlamında önemlidir.

Load Balancer kaynağın işlevleri ön uç, bir kural, sistem durumu araştırması ve arka uç havuzu tanımı olarak ifade edilir.

Kullanılabilirlik Alanları bağlamında, bir Load Balancer kaynağının davranışı ve özellikleri, bölgesel olarak yedekli veya zonal olarak açıklanmaktadır.  Bölgesel olarak yedekli ve bölgesel bir özelliğin bölge düzeyini betimleyen.  Load Balancer bağlamında, bölgesel olarak yedekli her zaman *birden çok bölge* ve ZGen, hizmeti *tek bir bölgeye*ayırma anlamına gelir.

Hem genel hem de dahili Load Balancer, bölgesel olarak yedekli ve zGenel senaryoları destekler ve her ikisi de gerektiğinde trafiği bölgelere (*çapraz bölge yük dengelemesi*) yönlendirebilir.

Load Balancer kaynağın kendisi bölge ve hiçbir şekilde hiç olamaz.  Hem VNet hem de alt ağ her zaman bölgesel ve hiçbir zaman.

### <a name="frontend"></a>Uçta

Load Balancer ön ucu, bir genel IP adresi kaynağına veya bir sanal ağ kaynağının alt ağı içindeki özel bir IP adresine başvuran bir ön uç IP yapılandırması.  Hizmetinizin açığa çıkarılabileceği yük dengeli uç noktayı oluşturur.

Bir Load Balancer kaynak, aynı anda hem bölgesel hem de bölgesel olarak yedekli ön uçları içerebilir. 

Genel bir IP kaynağı bir bölgeye göre garanti edildiğinde, bölge (veya eksikliği) değişebilir olmaz.  Genel bir IP ön ucu 'nın bölge düzeyini değiştirmek veya atlamak istiyorsanız, genel IP 'yi uygun bölgede yeniden oluşturmanız gerekir.  

Ön uç 'yi kaldırıp yeniden oluşturarak, bölge düzeyini değiştirerek veya atlayarak iç Load Balancer bir ön uç 'nin bölge boyutunu değiştirebilirsiniz.

Birden çok ön uç kullanırken, önemli noktalar için [Load Balancer için birden çok ön uç](load-balancer-multivip-overview.md) inceleyin.

#### <a name="zone-redundant-by-default"></a>Varsayılan olarak bölge yedekli

>[!IMPORTANT]
>Bölgeye özgü bilgiler de dahil olmak üzere ilgili konular için [kullanılabilirlik alanları](../availability-zones/az-overview.md) gözden geçirin.

Kullanılabilirlik Alanları bir bölgede, Standart Load Balancer ön uç varsayılan olarak bölgesel olarak yedekli olur.  Tek bir ön uç IP adresi, bölge başarısızlığından sonra bölge hatasından bağımsız olarak tüm arka uç havuzu üyelerine ulaşmak için kullanılabilir. Bu, daha az veri yolu anlamına gelmez, ancak yeniden denemeler veya yeniden oluşturma işlemi başarılı olur. DNS artıklık şemaları gerekli değildir. Ön uç 'nin tek IP adresi birden çok bağımsız altyapı dağıtımı tarafından aynı anda birden fazla Kullanılabilirlik Alanları olarak sunulur.  Bölgesel olarak yedekli tüm gelen veya giden akışların tek bir IP adresi ile aynı anda bir bölgedeki birden çok Kullanılabilirlik Alanları tarafından sunulduğunu belirtir.

Bir veya daha fazla Kullanılabilirlik Alanları başarısız olabilir ve bölgedeki bir bölge sağlıklı kaldığı sürece veri yolu daha uzundur. Bölgesel olarak yedekli yapılandırma varsayılandır ve ek eylem gerektirmez.  

İç Standart Load Balancer için bölgesel olarak yedekli genel IP adresi oluşturmak için aşağıdaki betiği kullanın. Yapılandırmanızda mevcut Kaynak Yöneticisi şablonları kullanıyorsanız, bu şablonlara **SKU** bölümünü ekleyin.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

İç Standart Load Balancer için bölgesel olarak yedekli bir ön uç IP adresi oluşturmak için aşağıdaki betiği kullanın. Yapılandırmanızda mevcut Kaynak Yöneticisi şablonları kullanıyorsanız, bu şablonlara **SKU** bölümünü ekleyin.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zone_redundant_frontend",
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

#### <a name="optional-zone-isolation"></a>İsteğe bağlı bölge yalıtımı

Tek bir bölge için bir ön uç garantisini seçebilirsiniz ve bu da *Bölgesel ön*ucu olarak bilinir.  Bu, gelen veya giden akışın bir bölgedeki tek bir bölge tarafından hizmet verdiği anlamına gelir.  Ön uç paylaşımlarınız, bölgenin sistem durumuyla birlikte fada.  Veri yolu, garantide olduğu yerde yer alan bölgelerde hatalara göre etkilenmemiştir. Kullanılabilirlik alanı başına bir IP adresi göstermek için, bölgesel ön uçları kullanabilirsiniz.  Ayrıca, doğrudan veya ön uç genel IP adreslerinden oluşuyorsa, bunları [Traffic Manager](../traffic-manager/traffic-manager-overview.md) gıbı bir DNS yük dengelemesi ürünüyle tümleştirmenize ve bir istemcinin bırden çok IP adresine çözümleyebileceği tek bir DNS adı kullanması için doğrudan veya özel olarak kullanabilirsiniz. .  Ayrıca, her bölgeyi tek tek izlemek için her bölge yük dengeli uç nokta için de kullanabilirsiniz.  Bu kavramları karıştırmak istiyorsanız (aynı arka uç için bölgesel olarak yedekli ve bölgesel), [Azure Load Balancer için birden fazla](load-balancer-multivip-overview.md)ön eki gözden geçirin.

Genel Load Balancer ön ucu için, ön uç IP yapılandırması tarafından başvurulan genel IP 'ye bir *bölgeler* parametresi eklersiniz.  

İç Load Balancer ön uç için, iç Load Balancer ön uç IP yapılandırmasına bir *bölgeler* parametresi ekleyin. Bölgesel ön ucu, Load Balancer alt ağdaki bir IP adresinin belirli bir bölgeye karşı sağlanmasına neden olur.

Kullanılabilirlik Bölge 1 bir genel IP adresi oluşturmak için aşağıdaki betiği kullanın. Yapılandırmanızda mevcut Kaynak Yöneticisi şablonları kullanıyorsanız, bu şablonlara **SKU** bölümünü ekleyin.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "zones": [ "1" ],
            "sku":
            {
                "name": "Standard"
            },
```

Kullanılabilirlik Bölge 1 iç Standart Load Balancer ön ucu oluşturmak için aşağıdaki betiği kullanın.

Yapılandırmanızda mevcut Kaynak Yöneticisi şablonları kullanıyorsanız, bu şablonlara **SKU** bölümünü ekleyin. Ayrıca, alt kaynak için ön uç IP yapılandırmasında **bölgeler** özelliğini tanımlayın.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zonal_frontend_in_az1",
                        "zones": [ "1" ],
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

### <a name="cross-zone-load-balancing"></a>Bölgeler arası yük dengeleme

Bölgeler arası yük dengeleme, Load Balancer herhangi bir bölgedeki arka uç uç noktasına ulaşma ve ön uç ve bölge özelliklerinden bağımsız olma olanağıdır.

Dağıtımınızı tek bir bölge içinde hizalamak ve garantilemek istiyorsanız, bölgesel ön uç ve bölgesel arka uç kaynaklarını aynı bölgeye hizalayın. Başka bir eylem gerekli değildir.

### <a name="backend"></a>Sunucusundan

Load Balancer sanal makinelerle birlikte kullanılabilir.  Tek bir sanal ağdaki herhangi bir sanal makine, bir bölgede veya hangi bölgenin garantide garanti edilip edilmediklerine bakılmaksızın arka uç havuzunun bir parçası olabilir.

Ön uç ve arka ucunuzu tek bir bölge ile hizalamak ve garantilemek isterseniz, yalnızca aynı bölgedeki VM 'Leri ilgili arka uç havuzuna yerleştirin.

VM 'Leri birden çok bölgede ele almak istiyorsanız, VM 'Leri birden çok bölgede aynı arka uç havuzuna yerleştirmeniz yeterlidir.  Sanal Makine Ölçek Kümeleri kullanırken, bir veya daha fazla sanal makine ölçek kümesini aynı arka uç havuzuna yerleştirebilirsiniz.  Bu sanal makine ölçek kümelerinin her biri, tek veya birden çok bölgede olabilir.

### <a name="outbound-connections"></a>Giden bağlantılar

[Giden bağlantılar](load-balancer-outbound-connections.md) tüm bölgeler tarafından sunulur ve bir sanal makine ortak bir Load Balancer ve bölgesel olarak yedekli ön uç ile ilişkilendirildiğinde kullanılabilirlik alanları bir bölgede otomatik olarak bölgesel olarak yedekli olur.  Giden bağlantı SNAT bağlantı noktası ayırmaları kalan bölge hataları.  

Bu durumda, VM ortak bir Load Balancer ve bir bölgesel ön ucu ile ilişkilendirilmişse giden bağlantıların tek bir bölge tarafından sunulduğunu garanti edilir.  Giden bağlantılar Fate 'yi ilgili bölgenin sistem durumuyla paylaşır.

SNAT bağlantı noktası ön tahsisi ve algoritması, bölgeleriyle veya bölgeleriyle aynı.

### <a name="health-probes"></a>Sistem durumu araştırmaları

Mevcut durum araştırma tanımlarınız Kullanılabilirlik Alanları olmadığında kalır.  Ancak sistem durumu modelini bir altyapı düzeyinde genişlettik. 

Bölgesel olarak yedekli ön uçları kullanırken Load Balancer, iç sistem durumu modelini, her bir kullanılabilirlik bölgesinden bir sanal makinenin erişilebilirliğini bağımsız olarak araştırmasını ve müşteri müdahalesi olmadan başarısız olabilecek bölgelerde yolları kapatmayı sağlar.  Belirli bir yol, bir bölgenin Load Balancer altyapısından başka bir bölgedeki bir VM 'ye yoksa, Load Balancer bu hatayı algılayabilir ve önleyebilir. Bu sanal makineye erişebilen diğer bölgeler, sanal makineyi ilgili ön uçlarından sunmaya devam edebilir.  Sonuç olarak, hata olayları sırasında her bir bölgenin, uçtan uca hizmetinizin genel sistem durumunu korurken biraz farklı akış dağıtımları olabilir.

## <a name="design"></a>Tasarım konuları

Load Balancer, Kullanılabilirlik Alanları bağlamında özellikle esnektir. Bölgelere hizalamayı seçebilirsiniz veya bölge yedekli olarak seçebilirsiniz.  Daha yüksek kullanılabilirlik, artan karmaşıklık fiyatına gelebilir ve en iyi performans için kullanılabilirliği tasarlayabilmeniz gerekir.  Bazı önemli tasarım konularına göz atalım.

### <a name="automatic-zone-redundancy"></a>Otomatik bölge-artıklık

Load Balancer, bölge yedekli ön uç olarak tek bir IP 'nin olmasını kolaylaştırır. Bölgesel olarak yedekli bir IP adresi herhangi bir bölgede güvenli bir şekilde bir bölgesel kaynağı sunabilir ve bir bölgenin bölgede sağlıklı kaldığı sürece bir veya daha fazla bölge başarısızlığının üzerinde kalabilirler. Buna karşılık, bir bölgesel ön ucu hizmetin tek bir bölgeye bir azalmasıyla birlikte Fate 'yı ilgili bölge ile paylaşır.

Bölge yedekliliği, itless DataPath veya denetim düzlemi göstermez;  açıkça veri düzledir. Bölgesel olarak yedekli akışlar herhangi bir bölgeyi kullanabilir ve bir müşterinin akışları bir bölgedeki tüm iyi alanları kullanır. Bölge hatası durumunda, zaman içinde sağlıklı bölgeleri kullanan trafik akışları etkilenmez.  Bölge hatası sırasında bölge kullanan trafik akışları etkilenebilir, ancak uygulamalar kurtarabilir. Bu akışlar, Azure 'un bölge hatası etrafında yakınsadıktan sonra yeniden aktarım veya yeniden oluşturma sırasında bölge içindeki kalan sağlıklı bölgelerde devam edebilir.

### <a name="xzonedesign"></a>Çapraz bölge sınırları

Uçtan uca bir hizmetin bölgeleri nasıl kesiştiği hakkında daha fazla bilgi için, Fate 'yi tek bir bölge değil, muhtemelen birden çok bölge ile paylaşacağınızı anlamanız önemlidir.  Sonuç olarak, uçtan uca hizmetiniz, hiç türlü dağıtım dışı dağıtımlar üzerinde kullanılabilirlik kazanmayabilir.

Kullanılabilirlik Alanları kullanırken kullanılabilirlik kazançlarını null olarak belirleyen, istenmeyen çapraz bölge bağımlılıklarından ulaşmaktan kaçının.  Uygulamanız birden çok bileşenden oluşur ve bölge hatasına dayanıklı olmak istediğinizde, bir bölgenin başarısız olması durumunda yeterli kritik bileşen olup olmadığından emin olmak için dikkatli olmanız gerekir.  Örneğin, uygulamanız için tek bir kritik bileşen yalnızca, kalan bölge (ler) in dışında bir bölgede mevcutsa uygulamanızın tamamını etkileyebilir.  Ayrıca, bölge geri yüklemesini ve uygulamanızın nasıl yakınsama olacağını da göz önünde bulundurun. Daha sonra bazı önemli noktaları incelim ve belirli senaryonuza göz önünde bulundurabileceğiniz sorulara soru-yanıt olarak kullanalım.

- Uygulamanızın bir IP adresi ve yönetilen disk içeren bir VM gibi iki bileşeni varsa ve bölge 1 ve bölge 2 ' de garanti edildiğinde bölge 1 başarısız olduğunda, bölge 1 başarısız olduğunda uçtan uca hizmetiniz devam etmez.  Potansiyel olarak tehlikeli bir hata modu oluşturmadığınızı tam olarak anlamadığınız müddetçe çapraz bölgeler kullanmayın.

- Uygulamanızın bir IP adresi ve yönetilen disk içeren bir VM gibi iki bileşeni varsa ve bunlar sırasıyla bölgesel olarak yedekli ve bölge 1 olarak garanti altına alıyorsa, uçtan uca hizmetiniz bölge 2, bölge 3 veya her ikisi de bölge 1 başarısız değilse bölge arızasından kaçınacaktır.  Ancak, tüm gözlerinizin ön ucu için ulaşılabilirlik durumunda hizmetinizin sistem durumu hakkında bir neden özelliği kaybedersiniz.  Daha kapsamlı bir sistem durumu ve kapasite modeli geliştirmeyi düşünün.  İçgörüler ve yönetilebilirlik ' i genişletmek için, bölgesel olarak yedekli ve bölgesel kavramlarını birlikte kullanabilirsiniz.

- Uygulamanızda, bölgesel olarak yedekli Load Balancer ön ucu ve üç bölgede bir çapraz bölge sanal makine ölçek kümesi gibi iki bileşen varsa, bölgelerdeki kaynaklarınız hatadan etkilenmeyen kaynaklarınız kullanılabilir olacaktır, ancak uçtan uca hizmet kapasiteniz azaltılabilir bölge hatası sırasında. Bir altyapı perspektifinden, dağıtımınız bir veya daha fazla bölge hatasından devam edebilir ve bu, aşağıdaki soruları oluşturur:
  - Uygulamanızın bu hatalara ve düşürülmüş kapasiteye ilişkin nasıl neden olduğunu anlamış musunuz?
  - Gerekirse bir bölge çiftine yük devretmeyi zorlamak için hizmetinize korumalar olması gerekiyor mu?
  - Böyle bir senaryoyu nasıl izleyebilir, algılayabilir ve azaltabilirsiniz? Uçtan uca hizmet performanlarınızın izlenmesini artırmak için Standart Load Balancer tanılamayı kullanabilirsiniz. Nelerin kullanılabilir olduğunu ve tüm resim için genişletmede nelerin gerekli olabileceğini düşünün.

- Bölgeler, hataların daha kolay anlaşılır ve dahil olmasını sağlayabilir.  Ancak, bölge hatası, zaman aşımları, denemeler ve geri alma algoritmaları gibi kavramlara geldiğinde diğer hatalardan farklı değildir. Azure Load Balancer, bölgesel olarak yedekli yollar sağlar ve hızlı bir şekilde kurtarmaya çalışır, ancak bir paket düzeyinde gerçek zamanlı olarak yeniden aktarımlar veya reestablishments, bir hatanın onın ve kesil. Yük Dengeleme şemanızın devam edecek, ancak şunları planlamanız gerekir:
  - Bir bölge başarısız olduğunda, uçtan uca hizmetiniz bunu anlamış olur ve durum kaybolduysa nasıl kurtarılır?
  - Bir bölge döndürüldüğünde, uygulamanız güvenle nasıl yakınsama yapılacağını anlasın mı?

### <a name="zonalityguidance"></a>Bölgesel olarak yedekli ve ZGen karşılaştırması

>[!IMPORTANT]
>Bölgeye özgü bilgiler de dahil olmak üzere ilgili konular için [kullanılabilirlik alanları](../availability-zones/az-overview.md) gözden geçirin.

Bölgesel olarak yedekli bölge, hizmet için tek bir IP adresi ile aynı zamanda esnek bir seçeneğe sahip olabilir.  Bu, karmaşıklığı azaltabilir.  Bölgesel olarak yedekli bölgeler arasında taşınabilirliği de vardır ve herhangi bir bölgedeki kaynaklarda güvenli bir şekilde kullanılabilir.  Ayrıca, bir bölge Kullanılabilirlik Alanları kazandıktan sonra gerekli değişiklikleri sınırlayan Kullanılabilirlik Alanları olmayan bölgelerde daha fazla kanıt vardır.  Bölgesel olarak yedekli bir IP adresi veya ön uç için yapılandırma sözdizimi, Kullanılabilirlik Alanları olmayanlar dahil olmak üzere herhangi bir bölgede başarılı olur.

Zonal, bölgenin sistem durumuyla birlikte paylaşılan bir bölgeye yönelik açık bir garanti sağlayabilir. Bir IP adresi veya bölgesel Load Balancer ön ucu ilişkilendirilmesi, özellikle iliştirilen kaynağınız aynı bölgedeki bir bölgesel VM ise, istenen veya makul bir öznitelik olabilir.  Ya da uygulamanız, bir kaynağın bulunduğu bölge hakkında açık bilgi gerektiriyor ve ayrı bölgelerde açık bir şekilde kullanılabilirlik hakkında bir neden olmak istiyorsunuz.  Bölgeler arasında dağıtılmış uçtan uca bir hizmet için birden çok zikzak ön eki kullanıma sunabilirsiniz (yani, birden fazla sanal makine ölçek kümesi için bölge başına düşen ön uçlar).  Ve, genel IP adresleri ise, bu birden fazla bölgesel ön uçlarınızın [Traffic Manager](../traffic-manager/traffic-manager-overview.md)hizmetinizi kullanıma sunmak için kullanabilirsiniz.  Ya da, üçüncü taraf izleme çözümleri aracılığıyla bölge sistem durumu ve Performans öngörüleri elde etmek ve genel hizmeti bölgesel olarak yedekli bir ön uç ile kullanıma sunmak için birden fazla bölgesel ön ucu kullanabilirsiniz. Yalnızca aynı bölgeye hizalanmış ve büyük olasılıkla olası olabilecek çapraz bölge senaryolarına karşı, yalnızca Zine ön uçlar içeren bölgesel kaynaklarına sahip olmanız gerekir.  Tek tek kaynaklar yalnızca Kullanılabilirlik Alanları var olan bölgelerde bulunur.

Hizmetin, hizmet mimarisini bilmeden daha iyi bir seçim olduğu konusunda genel bir rehberlik yoktur.

## <a name="limitations"></a>Sınırlamalar

- Veri düzlemi tamamen bölge yedekli olsa da (ZGen garantisi belirtilmediği sürece), denetim düzlemi işlemleri tam olarak bölgesel olarak yedekli değildir.

## <a name="next-steps"></a>Sonraki adımlar
- [Kullanılabilirlik alanları](../availability-zones/az-overview.md) hakkında daha fazla bilgi edinin
- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- Bir [bölge Içinde VM 'lerin yükünü bir standart Load Balancer kullanarak bir bölgede yük ön uç ile yükleme](load-balancer-standard-public-zonal-cli.md) hakkında bilgi edinin
- Bölgesel olarak [yedekli bir ön uç ile standart Load Balancer kullanarak sanal makinelerin bölge üzerinden yük dengelenmesi](load-balancer-standard-public-zone-redundant-cli.md) hakkında bilgi edinin
