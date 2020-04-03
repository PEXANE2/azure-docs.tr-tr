---
title: NAT ağ geçidi kaynaklarıyla Sanal ağlar tasarlama
titleSuffix: Azure Virtual Network NAT
description: NAT ağ geçidi kaynaklarıyla sanal ağları nasıl tasarlayabilirsiniz öğrenin.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to learn more about how to design virtual networks with NAT gateway resources.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: allensu
ms.openlocfilehash: 405d9bc09462f2940567080ec86775baf066d70d
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80584563"
---
# <a name="designing-virtual-networks-with-nat-gateway-resources"></a>NAT ağ geçidi kaynaklarıyla sanal ağlar tasarlama

NAT ağ geçidi kaynakları [Sanal Ağ NAT'Nin](nat-overview.md) bir parçasıdır ve sanal bir ağın bir veya daha fazla alt ağı için giden Internet bağlantısı sağlar. NAT ağ geçidinin kullanılacağı sanal ağ durumlarının alt ağı. NAT, bir alt ağ için kaynak ağ adresi çevirisi (SNAT) sağlar.  NAT ağ geçidi kaynakları, giden akışlar oluştururken sanal makinelerin hangi statik IP adreslerini kullandığını belirtir. Statik IP adresleri ortak IP adresi kaynaklarından, genel IP öneki kaynaklarından veya her ikisinden gelir. Bir NAT ağ geçidi kaynağı, her ikisinden de en fazla 16 statik IP adresi kullanabilir.


<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Internet'e giden ler için Sanal Ağ NAT">
</p>

*Şekil: Internet'e giden sanal ağ NAT*

## <a name="how-to-deploy-nat"></a>NAT nasıl dağıtılır?

NAT ağ geçidinin yapılandırılması ve kullanılması kasıtlı olarak basithale getirilir:  

NAT ağ geçidi kaynağı:
- Bölgesel veya bölgeli (bölge yalıtılmış) NAT ağ geçidi kaynağı oluşturmak,
- IP adresleri atamak,
- Gerekirse, TCP boşta zaman azamanı (isteğe bağlı) değiştirin.  Varsayılanı değiştirmeden <ins>önce</ins> [zamanlayıcıları](#timers) gözden geçirin.

Sanal ağ:
- Sanal ağ alt netini NAT ağ geçidi kullanacak şekilde yapılandırın.

Kullanıcı tanımlı rotalar gerekli değildir.

## <a name="resource"></a>Kaynak

Kaynak, şablon benzeri bir biçimde aşağıdaki Azure Kaynak Yöneticisi örneğinden görebileceğiniz gibi basit olacak şekilde tasarlanmıştır.  Bu şablon benzeri biçim, kavramları ve yapıyı göstermek için burada gösterilmiştir.  Gereksinimleriniz için örneği değiştirin.  Bu belge öğretici olarak tasarlanmamıştır.

Aşağıdaki diyagram, farklı Azure Kaynak Yöneticisi kaynakları arasında yazılabilir başvuruları gösterir.  Ok, yazılabilir olduğu yerden kaynaklanan başvurunun yönünü gösterir. Gözden geçirme 

<p align="center">
  <img src="media/nat-overview/flow-map.svg" width="256" title="Sanal Ağ NAT nesne modeli">
</p>

*Şekil: Sanal Ağ NAT nesne modeli*

[Havuz tabanlı Yük Dengeleyici giden bağlantıbelirli](../load-balancer/load-balancer-outbound-connections.md)bir bağımlılık yoksa NAT çoğu iş yükleri için önerilir.  

[Giden kurallar](../load-balancer/load-balancer-outbound-rules-overview.md)da dahil olmak üzere standart yük dengeleyici senaryolarından NAT ağ geçidine geçiş yapabilirsiniz. Geçiş yapmak için, genel ip ve genel ip önek kaynaklarını yük dengeleyici ön uçlarından NAT ağ geçidine taşıyın. NAT ağ geçidi için yeni IP adresleri gerekmez. Toplam 16 IP adresini aşmadığı sürece standart genel IP ve öneki yeniden kullanılabilir. Geçiş sırasında hizmet kesintisi göz önünde bulundurularak geçiş planı.  İşlemi otomatikleştirerek kesintiyi en aza indirebilirsiniz. Önce bir evreleme ortamında geçiştest edin.  Geçiş sırasında, gelen kaynaklı akışlar etkilenmez.

Aşağıdaki örnek, Azure Kaynak Yöneticisi şablonundan bir kesittir.  Bu şablon, nat ağ geçidi de dahil olmak üzere çeşitli kaynaklar dağılar.  Şablon bu örnekte aşağıdaki parametrelere sahiptir:

- **natgatewayname** - NAT ağ geçidinin adı.
- **konum** - Kaynağın bulunduğu Azure bölgesi.
- **publicipname** - NAT ağ geçidi ile ilişkili giden genel IP adı.
- **vnetname** - Sanal ağın adı.
- **subnetname** - NAT ağ geçidiyle ilişkili alt ağın adı.

Tüm IP adresi ve önek kaynakları tarafından sağlanan toplam IP adresi sayısı toplam 16 IP adresini geçemez. 1 ile 16 arasında herhangi bir sayıda IP adresine izin verilir.

:::code language="json" source="~/quickstart-templates/101-nat-gateway-vnet/azuredeploy.json" range="81-96":::

NAT ağ geçidi kaynağı oluşturulduğunda, sanal bir ağın bir veya daha fazla alt ağında kullanılabilir. Bu NAT ağ geçidi kaynağını hangi alt ağların kullanacağını belirtin. Bir NAT ağ geçidi birden fazla sanal ağa yayılamaz. Aynı NAT ağ geçidini sanal ağın tüm alt ağlarına atamanız gerekmez. Tek tek alt ağlar farklı NAT ağ geçidi kaynaklarıyla yapılandırılabilir.

Kullanılabilirlik bölgelerini kullanmayan senaryolar bölgesel (bölge belirtilmemiş) olacaktır. Kullanılabilirlik bölgelerini kullanıyorsanız, NAT'yi belirli bir bölgeye yalıtmak için bir bölge belirtebilirsiniz. Bölge artıklığı desteklenmez. NAT [kullanılabilirlik bölgelerini](#availability-zones)gözden geçirin.

:::code language="json" source="~/quickstart-templates/101-nat-gateway-vnet/azuredeploy.json" range="1-146" highlight="81-96":::

NAT ağ geçitleri, sanal ağ daki bir alt ağdaki bir özellik ile tanımlanır. Sanal ağ **vnetname** **alt net adı** üzerinde sanal makineler tarafından oluşturulan akışları NAT ağ geçidi kullanır. Tüm giden bağlantı kaynak IP adresi olarak **natgatewayname** ile ilişkili IP adreslerini kullanır.

Bu örnekte kullanılan Azure Kaynak Yöneticisi şablonu hakkında daha fazla bilgi için bkz:

- [Hızlı başlangıç: NAT ağ geçidi oluşturma - Kaynak Yöneticisi şablonu](quickstart-create-nat-gateway-template.md)
- [Sanal Ağ NAT](https://azure.microsoft.com/resources/templates/101-nat-gateway-1-vm/)

## <a name="design-guidance"></a>Tasarım Rehberliği

NAT ile sanal ağlar tasarlamak için dikkat edilmesi gereken hususları tanımak için bu bölümü inceleyin.  

1. [Maliyet optimizasyonu](#cost-optimization)
1. [Gelen ve gidenin birlikte liği](#coexistence-of-inbound-and-outbound)
2. [Temel kaynakları yönetme](#managing-basic-resources)
3. [Kullanılabilirlik Bölgeleri](#availability-zones)

### <a name="cost-optimization"></a>Maliyet iyileştirmesi

[Hizmet bitiş noktaları](virtual-network-service-endpoints-overview.md) ve [özel bağlantı,](../private-link/private-link-overview.md) maliyeti optimize etmek için göz önünde bulundurulması gereken seçeneklerdir. NAT bu hizmetler için gerekli değildir. Hizmet bitiş noktalarına veya özel bağlantıya yönlendirilen trafik sanal ağın NAT'sı tarafından işlenmez.  

Hizmet bitiş noktaları Azure hizmet kaynaklarını sanal ağınıza bağlar ve Azure hizmet kaynaklarınıza erişimi denetler. Örneğin, Azure depolama alanına erişdiğinizde, veri işlenmiş NAT ücretlerinden kaçınmak için depolama için bir hizmet bitiş noktası kullanın. Hizmet bitiş noktaları ücretsizdir.

Özel bağlantı, Azure PaaS hizmetini (veya özel bağlantıyla barındırılan diğer hizmetleri) sanal ağ içinde özel bir bitiş noktası olarak ortaya çıkarır.  Özel bağlantı, süreye ve işlenen verilere göre faturalandırılır.

Bu yaklaşımlardan birinin veya her ikisinin senaryonuza uygun olup olmadığını değerlendirin ve gerektiğinde kullanın.

### <a name="coexistence-of-inbound-and-outbound"></a>Gelen ve gidenin birlikte liği

NAT ağ geçidi şu ile uyumludur:

 - Standart yük dengeleyicisi
 - Standart genel IP
 - Standart genel IP öneki

Yeni bir dağıtım geliştirirken standart STU'larla başlayın.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Internet'e giden ler için Sanal Ağ NAT">
</p>

*Şekil: Internet'e giden sanal ağ NAT*

NAT ağ geçidi tarafından sağlanan Yalnızca Internet giden senaryo Internet işlevselliği gelen ile genişletilebilir. Her kaynak, bir akışın kaynağının farkındadır. NAT ağ geçidi olan bir alt ağda, Internet senaryolarına giden tüm bunların yerini NAT ağ geçidi ne göre tamamlar. Internet senaryolarından gelen ilgili kaynak tarafından sağlanır.

#### <a name="nat-and-vm-with-instance-level-public-ip"></a>NAT ve VM örnek düzeyinde Public IP ile

<p align="center">
  <img src="media/nat-overview/flow-direction2.svg" width="300" title="Örnek düzeyde Public IP ile Sanal Ağ NAT ve VM">
</p>

*Şekil: Örnek düzeyinde Public IP ile Sanal Ağ NAT ve VM*

| Yön | Kaynak |
|:---:|:---:|
| Gelen | Örnek düzeyde Public IP ile VM |
| Giden | NAT Gateway |

VM giden için NAT ağ geçidi kullanır.  Gelen kaynaklı etkilenmez.

#### <a name="nat-and-vm-with-public-load-balancer"></a>Kamu Yük Dengeleyicisi ile NAT ve VM

<p align="center">
  <img src="media/nat-overview/flow-direction3.svg" width="350" title="Genel Yük Dengeleyiciile Sanal Ağ NAT ve VM">
</p>

*Şekil: Kamu Yük Dengeleyici ile Sanal Ağ NAT ve VM*

| Yön | Kaynak |
|:---:|:---:|
| Gelen | kamu Yük Dengeleyici |
| Giden | NAT Gateway |

Bir yük dengeleme kuralından veya giden kurallardan gelen herhangi bir giden yapılandırman yerini NAT ağ geçidi ayarı ayarı ayarı adatır.  Gelen kaynaklı etkilenmez.

#### <a name="nat-and-vm-with-instance-level-public-ip-and-public-load-balancer"></a>NAT ve VM örnek düzeyinde kamu IP ve kamu Yük Dengeleyici ile

<p align="center">
  <img src="media/nat-overview/flow-direction4.svg" width="425" title="Örnek düzeyde genel IP ve genel Yük Dengeleyiciile Sanal Ağ NAT ve VM">
</p>

*Şekil: Örnek düzeyinde genel IP ve genel Yük Dengeleyici ile Sanal Ağ NAT ve VM*

| Yön | Kaynak |
|:---:|:---:|
| Gelen | Örnek düzeyinde genel IP ve genel Yük Dengeleyicisi ile VM |
| Giden | NAT Gateway |

Bir yük dengeleme kuralından veya giden kurallardan gelen herhangi bir giden yapılandırman yerini NAT ağ geçidi ayarı ayarı ayarı adatır.  VM ayrıca giden için NAT ağ geçidi kullanır.  Gelen kaynaklı etkilenmez.

### <a name="managing-basic-resources"></a>Temel kaynakları yönetme

Standart yük dengeleyicisi, genel IP ve genel IP öneki NAT ağ geçidi ile uyumludur. NAT ağ geçitleri bir alt ağ kapsamında çalışır. Bu hizmetlerin temel SKU'su NAT ağ geçidi olmayan bir alt ağ üzerinde dağıtılmalıdır. Bu ayrım, her iki SKU varyantı aynı sanal ağda bir arada bulunmanızı sağlar.

NAT ağ geçitleri, alt ağın giden senaryolarından önce gelir. Temel yük dengeleyicisi veya genel IP (ve onlarla oluşturulmuş herhangi bir yönetilen hizmet) doğru çevirilerle ayarlanamıyor. NAT ağ geçidi, bir alt ağüzerinde Internet trafiğine giden giden kontrolü alır. Temel yük dengeleyicisine ve genel ip'ye gelen trafik kullanılamıyor. Temel bir yük dengeleyicisine gelen trafik ve VM üzerinde yapılandırılan genel bir ip kullanılamaz.

### <a name="availability-zones"></a>Kullanılabilirlik Alanları

#### <a name="zone-isolation-with-zonal-stacks"></a>Bölge yığınları ile bölge yalıtımı

<p align="center">
  <img src="media/nat-overview/az-directions.svg" width="425" title="Bölge yalıtımlı Sanal Ağ NAT, birden çok "zonal stacks"">
</p>

*Şekil: Bölge yalıtımlı Sanal Ağ NAT, birden çok "bölge yığınları" oluşturarak*

Kullanılabilirlik bölgeleri olmasa bile, NAT esnektir ve birden çok altyapı bileşeni hatasından kurtulabilir.  Kullanılabilirlik bölgeleri, NAT için bölge yalıtım senaryoları ile bu esneklik üzerine inşa edilir.

Sanal ağlar ve alt ağları bölgesel yapılardır.  Alt ağlar sadece bir bölgeyle sınırlı değildir.

NAT ağ geçidi kaynağı kullanan sanal bir makine örneği NAT ağ geçidi kaynağı ve ortak IP adresleriyle aynı bölgede olduğunda bölge yalıtımı için bir zonal söz vardır. Bölge yalıtımı için kullanmak istediğiniz desen, kullanılabilirlik bölgesi başına bir "bölge yığını" oluşturuyor.  Bu "bölge yığını" sanal makine örnekleri, NAT ağ geçidi kaynakları, genel IP adresi ve/veya yalnızca aynı bölgeye hizmet verdiği varsayılan bir alt ağüzerindeki önek kaynaklarından oluşur.   Kontrol düzlemi işlemleri ve veri düzlemi daha sonra hizalanır ve belirtilen bölgeye sınırlanır. 

Senaryonuzun var olduğu bölge dışındaki bir bölgede hatanın NAT'ye etkisi nin bulunmaması beklenir. Aynı bölgedeki sanal makinelerden gelen giden trafik, bölge yalıtımı nedeniyle başarısız olur.  

#### <a name="integrating-inbound-endpoints"></a>Gelen uç noktaları tümleştirme

Senaryonuz gelen uç noktaları gerektiriyorsa, iki seçeneğiniz vardır:

| Seçenek | Desen | Örnek | Pro | Con |
|---|---|---|---|---|
| (1) | Gelen uç noktaları, giden için oluşturduğunuz ilgili **zonal yığınlarla** **hizala.** | Zonal frontend ile standart bir yük dengeleyici oluşturun. | Gelen ve giden ler için aynı sistem durumu modeli ve hata modu. Çalışması daha kolay. | Bölge başına tek tek IP adreslerinin ortak bir DNS adı ile maskelenilmesi gerekebilir. |
| (2) | Bölge yığınlarını **çapraz bölge** gelen bitiş noktasıyla yerle bir **edin.** | Bölge yedekli ön uçlu standart bir yük dengeleyicioluşturun. | Gelen uç nokta için tek IP adresi. | Gelen ve giden ler için değişen sistem durumu modeli ve hata modları.  Çalışması daha karmaşık. |

>[!NOTE]
> Bölge yalıtılmış NAT ağ geçidi, Ip adreslerinin NAT ağ geçidinin bölgesiyle eşleşmesini gerektirir. Farklı bir bölgeden veya bölge olmadan IP adreslerine sahip NAT ağ geçidi kaynaklarına izin verilmez.

#### <a name="cross-zone-outbound-scenarios-not-supported"></a>Çapraz bölge giden senaryolar desteklenmiyor

<p align="center">
  <img src="media/nat-overview/az-directions2.svg" width="425" title="Sanal Ağ NAT bölge yayılan alt ağ ile uyumlu değil">
</p>

*Şekil: Sanal Ağ NAT bölge yayılan alt ağ ile uyumlu değil*

Sanal makine örnekleri aynı alt ağ içinde birden çok bölgede dağıtıldığında NAT ağ geçidi kaynaklarıyla zonal bir söz elde edemezsiniz.   Ve bir alt ağa bağlı birden çok zonal NAT ağ geçidi olsa bile, sanal makine örneği hangi NAT ağ geçidi kaynağını seçmeyeceğini bilemez.

A) sanal makine örneğinin bölgesi ve zonal NAT ağ geçidi bölgeleri hizalanmadığında veya b) bölgesel bir NAT ağ geçidi kaynağı bölge sanal makine örnekleriyle kullanıldığında bir zonal söz yoktur.

Senaryo işe yarıyor gibi görünse de, sistem durumu modeli ve hata modu kullanılabilirlik bölgesi açısından tanımsızdır. Bölge yığınları veya yerine tüm bölgesel ile gidiş düşünün.

>[!NOTE]
>NAT ağ geçidi kaynağının bölge özelliği değişmez değildir.  NAT ağ geçidi kaynağını amaçlanan bölgesel veya bölge tercihiyle yeniden dağıtın.

>[!NOTE] 
>Bölge belirtilmemişse, IP adresleri tek başlarına bölge gereksiz değildir.  Belirli bir bölgede bir IP adresi oluşturulmazsa, Standart Yük Dengeleyicisinin ön ucu [bölge yedeklidir.](../load-balancer/load-balancer-standard-availability-zones.md#frontend)  Bu NAT için geçerli değil.  Yalnızca bölgesel veya bölge yalıtımı desteklenir.

## <a name="source-network-address-translation"></a>Kaynak Ağı Adres Çevirisi

Kaynak ağ adresi çevirisi (SNAT), bir akışın kaynağını farklı bir IP adresinden kaynaklandıracak şekilde yeniden yazar.  NAT ağ geçidi kaynakları, genellikle bağlantı noktası adresi çevirisine (PAT) atıfta bulunulan SNAT'ın bir varyantını kullanır. PAT kaynak adresi ve kaynak bağlantı noktasını yeniden yazar. SNAT ile, özel adreslerin sayısı ile çevrilen ortak adresleri arasında sabit bir ilişki yoktur.  

### <a name="fundamentals"></a>Temel Konular

Temel kavramı açıklamak için dört akış örneğine bakalım.  NAT ağ geçidi genel IP adresi kaynağı 65.52.0.2 kullanıyor.

| Akış | Kaynak tuple | Hedef tuple |
|:---:|:---:|:---:|
| 1 | 192.168.0.16:4283 | 65.52.0.1:80 |
| 2 | 192.168.0.16:4284 | 65.52.0.1:80 |
| 3 | 192.168.0.17.5768 | 65.52.0.1:80 |
| 4 | 192.168.0.16:4285 | 65.52.0.2:80 |

PAT gerçekleştikten sonra bu akışları bu gibi görünebilir:

| Akış | Kaynak tuple | SNAT'ed kaynak tuple | Hedef tuple | 
|:---:|:---:|:---:|:---:|
| 1 | 192.168.0.16:4283 | 65.52.0.2:234 | 65.52.0.1:80 |
| 2 | 192.168.0.16:4284 | 65.52.0.2:235 | 65.52.0.1:80 |
| 3 | 192.168.0.17.5768 | 65.52.0.2:236 | 65.52.0.1:80 |
| 4 | 192.168.0.16:4285 | 65.52.0.2:237 | 65.52.0.2:80 |

Hedef, atanan bağlantı noktası gösterilen 65.52.0.2 (SNAT kaynak tuple) olarak akış kaynağı göreceksiniz.  Pat önceki tabloda gösterildiği gibi de snat maskeleme bağlantı noktası denir.  Birden çok özel kaynak, bir IP ve bağlantı noktasının arkasında kılık da şıttır.

Kaynak bağlantı noktalarının atandığı belirli bir şekilde bağımlılık yapmayın.  Önceki sadece temel kavramın bir örneğidir.

NAT tarafından sağlanan SNAT çeşitli yönleriyle [Yük Dengeleyici](../load-balancer/load-balancer-outbound-connections.md) farklıdır.

### <a name="on-demand"></a>İsteğe bağlı

NAT, yeni giden trafik akışları için isteğe bağlı SNAT bağlantı noktaları sağlar. Stoktaki tüm kullanılabilir SNAT bağlantı noktaları, NAT ile yapılandırılan alt ağlardaki herhangi bir sanal makine tarafından kullanılır. 

<p align="center">
  <img src="media/nat-overview/lb-vnnat-chart.svg" width="550" title="Sanal Ağ NAT on-demand giden SNAT">
</p>

*Şekil: Sanal Ağ NAT on-demand giden SNAT*

Sanal bir makinenin herhangi bir IP yapılandırması gerektiğinde isteğe bağlı olarak giden akışlar oluşturabilir.  Ön ayırma, örnek başına en kötü durum aşırı sağlama dahil olmak üzere örnek planlama başına, gerekli değildir.  

<p align="center">
  <img src="media/nat-overview/exhaustion-threshold.svg" width="550" title="Tükenme senaryolarında farklılıklar">
</p>

*Şekil: Tükenme senaryolarında farklılıklar*

Bir SNAT bağlantı noktası serbest bıraktıktan sonra, NAT ile yapılandırılan alt ağlarda herhangi bir sanal makine tarafından kullanılabilir.  İsteğe bağlı ayırma, alt ağ(lar) üzerindeki dinamik ve farklı iş yüklerinin SNAT bağlantı noktalarını gerektiği gibi kullanmasına olanak tanır.  SNAT bağlantı noktası envanteri mevcut olduğu sürece, SNAT akışları başarılı olacaktır. Bunun yerine Daha büyük stoktan SNAT bağlantı noktası etkin noktaları yararlanır. SNAT bağlantı noktaları, etkin olarak ihtiyaç duymayan sanal makineler için kullanılmaz bırakılmaz.

### <a name="scaling"></a>Ölçeklendirme

Ölçekleme NAT öncelikle paylaşılan, kullanılabilir SNAT bağlantı noktası envanterini yönetme işlevidir. NAT, nat ağ geçidi kaynağına bağlı tüm alt ağlar için beklenen en yüksek giden akışlar için yeterli SNAT bağlantı noktası envanterine ihtiyaç duyar.  Ortak IP adresi kaynaklarını, genel IP öneki kaynaklarını veya her ikisini de SNAT bağlantı noktası envanteri oluşturmak için kullanabilirsiniz.

SNAT, özel adresleri bir veya daha fazla genel IP adresiyle eşler, işlemlerdeki kaynak adresi ve kaynak bağlantı noktasını yeniden yazar. Bir NAT ağ geçidi kaynağı, bu çeviri için yapılandırılan genel IP adresi başına 64.000 bağlantı noktası (SNAT bağlantı noktası) kullanır. NAT ağ geçidi kaynakları 16 IP adresine ve 1M SNAT bağlantı noktalarına kadar ölçeklenebilir. Genel bir IP öneki kaynağı sağlanmışsa, önek içindeki her IP adresi SNAT bağlantı noktası envanteri sağlar. Ve daha fazla genel IP adresi eklemek kullanılabilir stok SNAT bağlantı noktalarını artırır. TCP ve UDP ayrı SNAT bağlantı noktası stokları ve ilgisiz.

NAT ağ geçidi kaynakları fırsatçı bir şekilde kaynak bağlantı noktalarını yeniden kullanır. Ölçeklendirme amacıyla, her akışın yeni bir SNAT bağlantı noktası gerektirdiğini varsaymalı ve giden trafik için kullanılabilir IP adreslerinin toplam sayısını ölçeklendirmelisiniz.

### <a name="protocols"></a>Protokoller

NAT ağ geçidi kaynakları, UDP ve TCP akışlarının IP ve IP aktarım başlıklarıyla etkileşime girilir ve uygulama katmanı yüklerine agnostiktir.  Diğer IP protokolleri desteklenmez.

### <a name="timers"></a>Zamanlayıcılar

>[!IMPORTANT]
>Uzun boşta zamanlayıcı gereksiz yere SNAT tükenmesi olasılığını artırabilir. Zamanlayıcının süresini uzatırsanız, NAT snat bağlantı noktalarını sonunda zaman aşımına kadar basılı tutar. Akışlarınız boşta kalıyorsa, yine de başarısız olurlar ve gereksiz yere SNAT bağlantı noktası envanterini tüketirler.  2 saat içinde başarısız olan akışlar varsayılan 4 dakikada da başarısız olur. Boşta zaman aşımını artırmak, dikkatli bir şekilde kullanılması gereken son çare seçeneğidir. Bir akış asla boşta gitmezse, boşta zamanlayıcı tarafından etkilenmez.

TCP boşta zaman ayarı tüm akışlar için 4 dakikadan (varsayılan olarak) 120 dakikaya (2 saat) ayarlanabilir.  Ayrıca, akış üzerinde trafik ile boşta zamanlayıcı sıfırlayabilirsiniz.  Uzun boşta kalan bağlantıları ve bitiş noktası canlılık algılamayı yenilemek için önerilen desen TCP keepalives'dır.  TCP, son noktalara yinelenen AK'lar olarak görünür, düşük ek yükü vardır ve uygulama katmanına görünmezdir.

Aşağıdaki zamanlayıcılar SNAT bağlantı noktası sürümü için kullanılır:

| Zamanlayıcı | Değer |
|---|---|
| TCP FIN | 60 saniye |
| TCP RST | 10 saniye |
| TCP yarı açık | 30 saniye |

Bir SNAT bağlantı noktası, 5 saniye sonra aynı hedef IP adresine ve hedef bağlantı noktasına yeniden kullanılabilir.

>[!NOTE] 
>Bu zamanlayıcı ayarları değiştirilebilir. Değerler sorun gidermeye yardımcı olmak için sağlanır ve şu anda belirli zamanlayıcılara bağımlı olmamalısınız.

## <a name="limitations"></a>Sınırlamalar

- NAT standart SKU genel IP, genel IP öneki ve yük dengeleyici kaynakları ile uyumludur.   Temel kaynaklar (örneğin temel yük dengeleyicisi) ve bunlardan türetilen ürünler NAT ile uyumlu değildir.  Temel kaynaklar NAT ile yapılandırılan bir alt ağa yerleştirilmelidir.
- IPv4 adres ailesi desteklenir.  NAT, IPv6 adres ailesiyle etkileşime girmez.  NAT, IPv6 öneki olan bir alt ağ üzerinde dağıtılamaz.
- NAT kullanırken NSG akış günlüğü desteklenmez.
- NAT birden çok sanal ağa yayAmaz.


## <a name="feedback"></a>Geri Bildirim

Hizmeti nasıl geliştirebileceğimizi bilmek istiyoruz. Bir yeteneği eksik mi? NAT için [UserVoice'ta](https://aka.ms/natuservoice)bir sonraki oluşturmamız gerekenler için durumunuzu hazır bulun.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal [ağ NAT](nat-overview.md)hakkında bilgi edinin.
* [NAT ağ geçidi kaynakları için ölçümler ve uyarılar](nat-metrics.md)hakkında bilgi edinin.
* [NAT ağ geçidi kaynaklarını sorun giderme](troubleshoot-nat.md)hakkında bilgi edinin.
* NAT Ağ Geçidi'ni doğrulamak için öğretici
  - [Azure CLI](tutorial-create-validate-nat-gateway-cli.md)
  - [PowerShell](tutorial-create-validate-nat-gateway-cli.md)
  - [Portal](tutorial-create-validate-nat-gateway-cli.md)
* NAT ağ geçidi kaynağı dağıtmak için hızlı başlatma
  - [Azure CLI](./quickstart-create-nat-gateway-cli.md)
  - [PowerShell](./quickstart-create-nat-gateway-powershell.md)
  - [Portal](./quickstart-create-nat-gateway-portal.md)
  - [Şablon](./quickstart-create-nat-gateway-template.md)
* NAT ağ geçidi kaynağı API'si hakkında bilgi edinin
  - [REST API](https://docs.microsoft.com/rest/api/virtualnetwork/natgateways)
  - [Azure CLI](https://docs.microsoft.com/cli/azure/network/nat/gateway?view=azure-cli-latest)
  - [PowerShell](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway)
* [Kullanılabilirlik bölgeleri](../availability-zones/az-overview.md)hakkında bilgi edinin.
* Standart [yük dengeleyicisi](../load-balancer/load-balancer-standard-overview.md)hakkında bilgi edinin.
* [Kullanılabilirlik bölgeleri ve standart yük dengeleyicisi](../load-balancer/load-balancer-standard-availability-zones.md)hakkında bilgi edinin.
* [UserVoice Sanal Ağ NAT için sonraki oluşturmak için ne söyle.](https://aka.ms/natuservoice)


