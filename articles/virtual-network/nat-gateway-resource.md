---
title: NAT ağ geçidi kaynaklarıyla sanal ağlar tasarlama
titleSuffix: Azure Virtual Network NAT
description: NAT ağ geçidi kaynaklarıyla sanal ağların nasıl tasarlanacağını öğrenin.
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
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: 0cbebf4ab2d865d49a85da2e809bdf24e7258a5f
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444392"
---
# <a name="designing-virtual-networks-with-nat-gateway-resources-public-preview"></a>NAT ağ geçidi kaynaklarıyla sanal ağlar tasarlama (Genel Önizleme)

NAT ağ geçidi kaynakları, [sanal ağ NAT](nat-overview.md) 'nin bir parçasıdır ve bir sanal ağın bir veya daha fazla alt ağı Için giden Internet bağlantısı sağlar. Sanal ağ alt ağı hangi NAT ağ geçidinin kullanılacağını belirtir. NAT, bir alt ağ için kaynak ağ adresi çevirisi (SNAT) sağlar.  NAT ağ geçidi kaynakları, dış akışlar oluştururken sanal makinelerin hangi statik IP adresleri tarafından kullanılacağını belirtir. Statik IP adresleri, genel IP adresi kaynaklarından, genel IP öneki kaynaklarından veya her ikisiyle gelir. Bir NAT ağ geçidi kaynağı, bunlardan en fazla 16 statik IP adresi kullanabilir.


<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Internet 'e giden sanal ağ NAT">
</p>

*Şekil: Internet 'e giden sanal ağ NAT*


>[!NOTE] 
>Sanal ağ NAT Şu anda genel önizleme olarak kullanılabilir. Şu anda yalnızca sınırlı sayıda [bölgede](nat-overview.md#region-availability)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.comsupport/legal/preview-supplemental-terms).

> [!IMPORTANT]
> Aboneliğinizde sanal ağ NAT [önizlemesi etkinleştirildikten](./nat-overview.md#enable-preview) sonra, portala erişmek için https://aka.ms/natportal kullanın.

## <a name="how-to-deploy-nat"></a>NAT dağıtma

NAT ağ geçidini yapılandırma ve kullanma özellikle basit hale getirilir:  

NAT ağ geçidi kaynağı:
- Bölgesel veya bölgesel (bölge yalıtılmış) NAT ağ geçidi kaynağı oluşturma,
- IP adresleri ata,
- Boşta kalma zaman aşımını değiştirme (isteğe bağlı).

Sanal ağ:
- Sanal ağ alt ağını bir NAT ağ geçidi kullanacak şekilde yapılandırın.

Kullanıcı tanımlı yollar gerekli değildir.

## <a name="resource"></a>Kaynak

Kaynak, şablon benzeri bir biçimde aşağıdaki Azure Resource Manager örneğinde görebileceğiniz kadar basit olacak şekilde tasarlanmıştır.  Bu şablon benzeri biçim, kavramları ve yapıyı göstermek için burada gösterilmiştir.  Gereksinimlerinize göre örneği değiştirin.  Bu belge bir öğretici olarak tasarlanmamıştır.

Aşağıdaki diyagramda farklı Azure Resource Manager kaynakları arasındaki yazılabilir başvurular gösterilmektedir.  Ok, başvurunun yazılabilir olduğu yerden kaynaklanan yönünün yönünü gösterir. Gözden geçirme 

<p align="center">
  <img src="media/nat-overview/flow-map.svg" width="256" title="Sanal ağ NAT nesne modeli">
</p>

*Şekil: sanal ağ NAT nesne modeli*

[Havuz tabanlı Load Balancer giden bağlantı](../load-balancer/load-balancer-outbound-connections.md)üzerinde belirli bir bağımlılığı yoksa, NAT çoğu iş yükü için önerilir.  

[Giden kuralları](../load-balancer/load-balancer-outbound-rules-overview.md)da dahil olmak üzere standart yük dengeleyici SENARYOLARıNDAN NAT ağ geçidine geçiş yapabilirsiniz. Geçirmek için genel IP ve genel IP öneki kaynaklarını yük dengeleyici ön uçları ' ndan NAT ağ geçidine taşıyın. NAT Gateway için yeni IP adresleri gerekli değildir. Standart genel IP ve ön ek, toplam 16 IP adresini aşmadığı sürece yeniden kullanılabilir. Geçiş sırasında göz önünde bulundurarak hizmet kesintisini içeren geçiş planlayın.  İşlemi otomatikleştirerek kesintiye uğramasını küçültebilirsiniz. Geçişi önce hazırlama ortamında test edin.  Geçiş sırasında gelen kaynaklı akışlar etkilenmez.

Aşağıdaki örnek, Doğu ABD 2 bölgesinde _Mynatgateway_ ADLı bir NAT ağ geçidi kaynağı oluşturur _, az_ _4 dakikalık_ boşta zaman aşımı ile 1. Belirtilen giden IP adresleri şunlardır:
- _MyIP1_ ve _myIP2_ genel IP adresi kaynakları kümesi ve 
- _MyPrefix1_ ve _myPrefix2_genel IP öneki kaynakları kümesi. 

Dört IP adresi kaynağı tarafından belirtilen toplam IP adresi sayısı 16 IP adresi toplamı aşamaz. 1 ve 16 arasında herhangi bir sayıda IP adresine izin verilir.

```json
{
"name": "myNATGateway",
   "type": "Microsoft.Network/natGateways",
   "apiVersion": "2018-11-01",
   "location": "East US 2",
   "sku": { "name": "Standard" },
   "zones": [ "1" ],
   "properties": {
      "idleTimeoutInMinutes": 4, 
      "publicIPPrefixes": [
         {
            "id": "ref to myPrefix1"
         },
         {
            "id": "ref to myPrefix2"
         }
      ],
      "publicIPAddresses": [
         {
            "id": "ref to myIP1"
         },
         {
            "id": "ref to myIP2"
         }
      ]
   }
}
```

NAT ağ geçidi kaynağı oluşturulduğunda, sanal ağın bir veya daha fazla alt ağında kullanılabilir. Hangi alt ağların bu NAT ağ geçidi kaynağını kullanmasını belirtin. Bir NAT ağ geçidi birden fazla sanal ağa yayılamaz. Aynı NAT ağ geçidini bir sanal ağın tüm alt ağlarına atamak gerekli değildir. Ayrı alt ağlar, farklı NAT ağ geçidi kaynaklarıyla yapılandırılabilir.

Kullanılabilirlik bölgelerini kullanmayan senaryolar bölgesel (bölge belirtilmemiş) olacaktır. Kullanılabilirlik alanları kullanıyorsanız, NAT 'yi belirli bir bölgeye yalıtmak için bir bölge belirtebilirsiniz. Bölge yedekliliği desteklenmez. NAT [kullanılabilirlik bölgelerini](#availability-zones)gözden geçirin.


```json
{
   "name": "myVNet",
   "apiVersion": "2018-11-01",
   "type": "Microsoft.Network/virtualNetworks",
   "location": "myRegion", 
   "properties": {
      "addressSpace": {
          "addressPrefixes": [
           "192.168.0.0/16"
          ]
      },
      "subnets": [
         {
            "name": "mySubnet1",
            "properties": {
               "addressPrefix": "192.168.0.0/24",
               "natGateway": {
                  "id": "ref to myNATGateway"
               }
            }
         } 
      ]
   }
}
```
NAT ağ geçitleri, bir sanal ağ içindeki bir alt ağda bulunan bir özellik ile tanımlanır. Sanal makineler tarafından oluşturulan sanal ağ _mySubnet1_ alt ağı üzerinde sanal makineler tarafından oluşturulan AKıŞLAR, NAT ağ _geçidini kullanacaktır._ Tüm giden bağlantılar, _Mynatgateway_ Ile ilişkili IP ADRESLERINI kaynak IP adresi olarak kullanır.


## <a name="design-guidance"></a>Tasarım Kılavuzu

NAT ile sanal ağların tasarlanmasıyla ilgili hususları öğrenmek için bu bölümü gözden geçirin.  

1. [Maliyet iyileştirmesi](#cost-optimization)
1. [Gelen ve giden birlikte bulunma](#coexistence-of-inbound-and-outbound)
2. [Temel kaynakları yönetme](#managing-basic-resources)
3. [Kullanılabilirlik Alanları](#availability-zones)

### <a name="cost-optimization"></a>Maliyet iyileştirmesi

[Hizmet uç noktaları](virtual-network-service-endpoints-overview.md) ve [özel bağlantı](../private-link/private-link-overview.md) , NAT 'nin gerekli olmadığı maliyeti iyileştirmek için göz önünde bulundurmanız gereken iki seçenek vardır.  Hizmet uç noktalarına veya özel bağlantıya yöneltilen tüm trafik, sanal ağın NAT tarafından işlenmez.  

Hizmet uç noktaları, Azure hizmet kaynaklarını sanal ağınıza bağlayabilir ve Azure hizmet kaynaklarınıza erişimi denetler. Örneğin, Azure depolama 'ya eriştiğinizde, veri işleme NAT ücretlerinden kaçınmak için depolama için bir hizmet uç noktası kullanın. Hizmet uç noktaları ücretsizdir.

Özel bağlantı, Azure PaaS hizmetini (veya özel bağlantı ile barındırılan diğer hizmetleri) bir sanal ağ içindeki özel bir uç nokta olarak kullanıma sunar.  Özel bağlantı, işlenen süreye ve verilere göre faturalandırılır.

Bu yaklaşımlardan birinin veya her ikisinin senaryonuz için uygun olup olmadığını değerlendirin ve gerektiğinde kullanın.

### <a name="coexistence-of-inbound-and-outbound"></a>Gelen ve giden birlikte bulunma

NAT ağ geçidi ile uyumludur:

 - Standart yük dengeleyici
 - Standart genel IP
 - Standart genel IP öneki

Yeni bir dağıtım geliştirilirken standart SKU 'Lar ile başlayın.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Internet 'e giden sanal ağ NAT">
</p>

*Şekil: Internet 'e giden sanal ağ NAT*

NAT ağ geçidi tarafından sunulan yalnızca Internet giden senaryosu Internet 'ten gelen bir işlevle genişletilebilir. Her kaynak, akışın kaynaklandığı yönü algılar. NAT ağ geçidine sahip bir alt ağda, Internet 'Ten gelen tüm senaryolar, NAT ağ geçidi tarafından yenisiyle değiştirilmiştir. Internet senaryolarından gelen giriş, ilgili kaynak tarafından sağlanır.

#### <a name="nat-and-vm-with-instance-level-public-ip"></a>Örnek düzeyi genel IP ile NAT ve VM

<p align="center">
  <img src="media/nat-overview/flow-direction2.svg" width="300" title="Örnek düzeyi genel IP 'si olan sanal ağ NAT ve VM">
</p>

*Şekil: sanal ağ NAT ve örnek düzeyi genel IP 'si olan VM*

| Yön | Kaynak |
|:---:|:---:|
| Gelen | Örnek düzeyi genel IP 'si olan VM |
| Giden | NAT Gateway |

VM, giden için NAT Gateway kullanır.  Gelen kaynaklı etkilenmemektedir.

#### <a name="nat-and-vm-with-public-load-balancer"></a>Ortak Load Balancer sahip NAT ve VM

<p align="center">
  <img src="media/nat-overview/flow-direction3.svg" width="350" title="Sanal ağ NAT ve ortak Load Balancer VM">
</p>

*Şekil: ortak Load Balancer sanal ağ NAT ve VM*

| Yön | Kaynak |
|:---:|:---:|
| Gelen | ortak Load Balancer |
| Giden | NAT Gateway |

Yük Dengeleme kuralı veya giden kuralların herhangi bir giden yapılandırmasının yerini NAT ağ geçidi almıştır.  Gelen kaynaklı etkilenmemektedir.

#### <a name="nat-and-vm-with-instance-level-public-ip-and-public-load-balancer"></a>Örnek düzeyi genel IP ve ortak Load Balancer sahip NAT ve VM

<p align="center">
  <img src="media/nat-overview/flow-direction4.svg" width="425" title="Örnek düzeyi genel IP ve ortak Load Balancer sanal ağ NAT ve VM">
</p>

*Şekil: örnek düzeyinde genel IP ve genel Load Balancer sanal ağ NAT ve VM*

| Yön | Kaynak |
|:---:|:---:|
| Gelen | Örnek düzeyi genel IP ve genel Load Balancer VM |
| Giden | NAT Gateway |

Yük Dengeleme kuralı veya giden kuralların herhangi bir giden yapılandırmasının yerini NAT ağ geçidi almıştır.  VM, giden için NAT ağ geçidini de kullanacaktır.  Gelen kaynaklı etkilenmemektedir.

### <a name="managing-basic-resources"></a>Temel kaynakları yönetme

Standart yük dengeleyici, genel IP ve genel IP ön eki, NAT ağ geçidiyle uyumludur. NAT ağ geçitleri bir alt ağın kapsamında çalışır. Bu hizmetlerin temel SKU 'SU bir NAT ağ geçidi olmadan bir alt ağda dağıtılmalıdır. Bu ayrım, her iki SKU çeşitlemesinin de aynı sanal ağda birlikte kullanılmasına izin verir.

NAT ağ geçitleri, alt ağın giden senaryolarından önceliklidir. Temel yük dengeleyici veya genel IP (ve ile derlenen herhangi bir yönetilen hizmet) doğru çeviriler ile ayarlanamaz. NAT ağ geçidi, bir alt ağdaki Internet trafiğine giden denetimi alır. Temel yük dengeleyiciye ve genel IP 'ye gelen trafik kullanılamıyor. Temel yük dengeleyiciye gelen trafik ve veya bir VM 'de yapılandırılmış genel IP kullanılamaz.

### <a name="availability-zones"></a>Kullanılabilirlik Alanları

Kullanılabilirlik alanları olmadan bile NAT esnektir ve birden çok altyapı bileşeni hatasını devam edebilir. Kullanılabilirlik alanları senaryonuzun bir parçasıysa, NAT 'yi belirli bir bölge için yapılandırmanız gerekir.  Denetim düzlemi işlemleri ve veri düzlemi belirtilen bölge ile kısıtlanıyor. Senaryonuzun olması dışında bir bölgede hata olması beklenen, NAT 'nin etkisi olmadan olması beklenir. Aynı bölgedeki sanal makinelerden giden trafik, bölge yalıtımı nedeniyle başarısız olur.

<p align="center">
  <img src="media/nat-overview/az-directions.svg" width="425" title="Kullanılabilirlik alanları ile sanal ağ NAT">
</p>

*Şekil: kullanılabilirlik alanları ile sanal ağ NAT*

Bölge yalıtılmış bir NAT ağ geçidi, IP adreslerinin NAT ağ geçidinin bölgesiyle eşleşmesini gerektirir. Farklı bir bölgeden veya bölge olmadan IP adreslerine sahip NAT ağ geçidi kaynakları desteklenmez.

Sanal ağlar ve alt ağlar, bölgelere göre değil, bölgesel bir bölgedir. Bir VM, giden bağlantılar için bir NAT ağ geçidiyle aynı bölgede olmalıdır. Bölge yalıtımı, kullanılabilirlik alanı başına bir bölgesel "Stack" oluşturularak oluşturulur. Bir bölgesel NAT ağ geçidinin bölgelerini veya bölgesel VM 'lerle bölgesel bir NAT ağ geçidini kullanarak bir bölgesel Promise yok.

NAT ile kullanmak üzere sanal makine ölçek kümeleri dağıttığınızda, kendi alt ağına bir bölgesel ölçek kümesi dağıtır ve eşleşen bölge NAT ağ geçidini bu alt ağa iliştirebilirsiniz. Bölge-kapsayıcı ölçek kümeleri (iki veya daha fazla bölgede bir ölçek kümesi) kullanıyorsanız, NAT bir bölgesel taahhüdüne sahip olmaz.  NAT, bölge yedekliliği desteklemez.  Yalnızca bölge veya bölge yalıtımı desteklenir.

<p align="center">
  <img src="media/nat-overview/az-directions2.svg" width="425" title="bölge ile yayılan sanal ağ NAT">
</p>

*Şekil: bölgeye yayılan sanal ağ NAT*

Zones özelliği değişebilir değildir.  NAT ağ geçidi kaynağını amaçlanan bölge veya bölge tercihi ile yeniden dağıtın.

>[!NOTE] 
>Hiçbir bölge belirtilmemişse, kendilerine göre IP adresleri bölge yedekli değildir.  Belirli bir bölgede bir IP adresi oluşturulmadıysa, bir Standart Load Balancer ön ucu [bölge yedekli olur](../load-balancer/load-balancer-standard-availability-zones.md#frontend) .  Bu, NAT için uygulanmaz.  Yalnızca bölge veya bölge yalıtımı desteklenir.

## <a name="source-network-address-translation"></a>Kaynak ağ adresi çevirisi

Kaynak ağ adresi çevirisi (SNAT), bir akışın kaynağını farklı bir IP adresinden kaynaklanan bir şekilde yeniden yazar.  NAT ağ geçidi kaynakları, bağlantı noktası adresi çevirisi (PAT) ile yaygın olarak anılan bir, SNAT değişkenini kullanır. PAT kaynak adresi ve kaynak bağlantı noktasını yeniden yazar. SNAT ile, özel adres sayısı ve bunların çevrilmiş ortak adresleri arasında sabit bir ilişki yoktur.  

### <a name="fundamentals"></a>Temel Konular

Temel kavramı açıklamak için dört akışla bir örneğe bakalım.  NAT ağ geçidi, genel IP adresi kaynağı 65.52.0.2 kullanıyor.

| Akış | Kaynak kayıt düzeni | Hedef kayıt düzeni |
|:---:|:---:|:---:|
| 1 | 192.168.0.16:4283 | 65.52.0.1:80 |
| 2 | 192.168.0.16:4284 | 65.52.0.1:80 |
| 3 | 192.168.0.17.5768 | 65.52.0.1:80 |
| 4 | 192.168.0.16:4285 | 65.52.0.2:80 |

Bu akışlar, PAT olduktan sonra şöyle görünebilir:

| Akış | Kaynak kayıt düzeni | Snattoed kaynak kayıt düzeni | Hedef kayıt düzeni | 
|:---:|:---:|:---:|:---:|
| 1 | 192.168.0.16:4283 | 65.52.0.2:234 | 65.52.0.1:80 |
| 2 | 192.168.0.16:4284 | 65.52.0.2:235 | 65.52.0.1:80 |
| 3 | 192.168.0.17.5768 | 65.52.0.2:236 | 65.52.0.1:80 |
| 4 | 192.168.0.16:4285 | 65.52.0.2:237 | 65.52.0.2:80 |

Hedef, belirtilen atanmış bağlantı noktası ile Flow kaynağını 65.52.0.2 (SNAT kaynak kayıt düzeni) olarak görür.  Yukarıdaki tabloda gösterildiği gibi PAT, bağlantı noktası aşağı olarak da adlandırılır.  Birden çok özel kaynak bir IP ve bağlantı noktasının arkasında bir şekilde kendini temel alınır.

Kaynak bağlantı noktalarının atandığı belirli şekilde bir bağımlılık yapmayın.  Yukarıdaki temel kavramın bir çizimidir.

NAT tarafından sunulan SNAT, çeşitli açılardan [Load Balancer](../load-balancer/load-balancer-outbound-connections.md) farklıdır.

### <a name="on-demand"></a>İsteğe bağlı

NAT, yeni giden trafik akışları için isteğe bağlı SNAT bağlantı noktaları sağlar. Envanterdeki tüm kullanılabilir SNAT bağlantı noktaları, NAT ile yapılandırılmış alt ağlardaki tüm sanal makineler tarafından kullanılır. 

<p align="center">
  <img src="media/nat-overview/lb-vnnat-chart.svg" width="550" title="Sanal ağ NAT istek üzerine giden SNAT">
</p>

*Şekil: sanal ağ NAT istek üzerine giden SNAT*

Bir sanal makinenin herhangi bir IP yapılandırması, gerektiğinde istek üzerine giden akışlar oluşturabilir.  Örnek başına en kötü büyük/küçük harf sağlama de dahil olmak üzere ön tahsisi, gerekli değildir.  

<p align="center">
  <img src="media/nat-overview/exhaustion-threshold.svg" width="550" title="Tükenme senaryolarında farklılıklar">
</p>

*Şekil: tükenme senaryolarında farklılıklar*

Bir SNAT bağlantı noktası yayımlandıktan sonra, NAT ile yapılandırılmış alt ağlardaki tüm sanal makineler tarafından kullanılabilir.  İsteğe bağlı ayırma, alt ağdaki dinamik ve daha fazla iş yükünün, gereken SNAT bağlantı noktalarını kullanmasına izin verir.  SNAT bağlantı noktası envanteri kullanılabilir olduğu sürece, SNAT akışları başarılı olur. SNAT bağlantı noktası etkin noktaları, bunun yerine daha büyük envanterden faydalanır. SNAT bağlantı noktaları, etkin bir şekilde ihtiyaç duymayan sanal makineler için kullanılmıyor olarak kalmadı.

### <a name="scaling"></a>Ölçeklendirme

NAT, giden tüm senaryolar için yeterli SNAT bağlantı noktası envanterine ihtiyaç duyuyor. NAT ölçeklendirme, birincil olarak paylaşılan, kullanılabilir SNAT bağlantı noktası envanterini yönetme işlevinden oluşur. Bir NAT ağ geçidi kaynağına bağlı tüm alt ağlar için en yoğun giden akışı karşılamak üzere yeterli sayım olmalıdır.

SNAT birden çok özel adresi tek bir genel adresle eşleştirir ve ölçeklendirmek için birden çok genel IP kullanır.

Bir NAT ağ geçidi kaynağı, genel bir IP adresinin 64.000 bağlantı noktasını (SNAT bağlantı noktaları) kullanır.  Bu SNAT bağlantı noktaları, özel ve ortak akış eşleştirmesi için kullanılabilir envanter olur. Ve daha fazla genel IP adresi eklemek, kullanılabilir envanter SNAT bağlantı noktalarını artırır. NAT ağ geçidi kaynakları, 16 IP adresi ve 1M SNAT bağlantı noktası kadar ölçeklenebilir.  TCP ve UDP ayrı SNAT bağlantı noktası envanteridir ve ilgisiz değildir.

NAT ağ geçidi kaynakları mümkün olduğunda kaynak bağlantı noktalarını yeniden kullanır. Ölçeklendirme amacıyla, her akışın yeni bir SNAT bağlantı noktası gerektirdiğini ve giden trafik için kullanılabilir IP adreslerinin toplam sayısını ölçeklendirmelisiniz.

### <a name="protocols"></a>Protokoller

NAT ağ geçidi kaynakları, UDP ve TCP akışlarının IP ve IP taşıma üstbilgileri ile etkileşim kurar ve uygulama katmanı yükleri için belirsizdir.  Diğer IP protokolleri desteklenmez.

### <a name="timers"></a>Zamanlayıcılar

Boşta kalma zaman aşımı, tüm akışlar için 4 dakikadan (varsayılan) 120 dakikaya (2 saat) ayarlanabilir.  Ayrıca, boşta kalma zamanlayıcısını akıştaki trafikle sıfırlayabilirsiniz.  Uzun boşta bağlantıları yenilemek için önerilen bir model ve uç nokta sahtekarlığı algılama, TCP keepcanlı hale getirilir.  TCP keepetkin noktalar, uç noktalara yinelenen bir ACK olarak görünür, düşük yüklerdir ve uygulama katmanında görünmez.

Aşağıdaki zamanlayıcılar, SNAT bağlantı noktası sürümü için kullanılır:

| Zamanlayıcı | Değer |
|---|---|
| TCP FIN | 60 saniye |
| TCP RST | 10 saniye |
| TCP yarı açık | 30 saniye |

Bir SNAT bağlantı noktası, 5 saniye sonra aynı hedef IP adresi ve hedef bağlantı noktası için yeniden kullanılabilir.

>[!NOTE] 
>Bu süreölçer ayarları değişebilir. Bu değerler, sorun gidermeye yardımcı olmak için sağlanır ve şu anda belirli zamanlayıcılara bağımlılık yapmanız gerekmez.

## <a name="limitations"></a>Sınırlamalar

- NAT, standart SKU genel IP 'si, genel IP öneki ve yük dengeleyici kaynaklarıyla uyumludur.   Temel kaynaklar (örneğin, temel yük dengeleyici) ve bunlardan türetilmiş tüm ürünler NAT ile uyumlu değildir.  Temel kaynakların NAT ile yapılandırılmamış bir alt ağa yerleştirilmesi gerekir.
- IPv4 adres ailesi destekleniyor.  NAT, IPv6 adres ailesi ile etkileşime girmez.
- Alt ağ veya NIC üzerinde NSG, NAT kullanılarak genel uç noktalara giden akışlar için kabul edilemez.
- NSG akış günlüğü, NAT kullanılırken desteklenmez.
- Bir sanal ağda birden çok alt ağ olduğunda, her alt ağda farklı bir NAT yapılandırılmış olabilir.
- NAT birden çok sanal ağa yayılamaz.

## <a name="preview-participation"></a>Önizleme katılımı

[Aboneliğinizi etkinleştirmek için yönergeleri](nat-overview.md#public-preview-participation)izleyin.

## <a name="feedback"></a>Geri Bildirim

Hizmeti nasıl geliştirebileceğimizi öğrenmek istiyoruz. [Görüşlerinizi genel önizlemede](https://aka.ms/natfeedback) bizimle paylaşabilirsiniz.  Ve [NAT Için UserVoice](https://aka.ms/natuservoice)'ta daha sonra derleme yapmanız gerekenleri de önerebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal ağ NAT](nat-overview.md)hakkında daha fazla bilgi edinin.
- NAT ağ geçidini doğrulama öğreticisi
  * [Azure CLI](tutorial-create-validate-nat-gateway-cli.md),
  * [PowerShell](tutorial-create-validate-nat-gateway-cli.md),
  * [Portal](tutorial-create-validate-nat-gateway-cli.md)
- NAT ağ geçidi kaynağı dağıtmaya yönelik hızlı başlangıç
  * [Azure CLI](./quickstart-create-nat-gateway-cli.md),
  * [PowerShell](./quickstart-create-nat-gateway-powershell.md),
  * [Portal](./quickstart-create-nat-gateway-portal.md).
- [Kullanılabilirlik alanları](../availability-zones/az-overview.md)hakkında daha fazla bilgi edinin.
- [Standart yük dengeleyici](../load-balancer/load-balancer-standard-overview.md)hakkında daha fazla bilgi edinin.
- [Kullanılabilirlik alanları ve standart yük dengeleyici](../load-balancer/load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
- NAT ağ geçidi kaynak API 'SI hakkında daha fazla bilgi
  * [REST API](https://docs.microsoft.com/rest/api/virtualnetwork/natgateways),
  * [Azure CLI](https://docs.microsoft.com/cli/azure/network/nat/gateway?view=azure-cli-latest),
  * [PowerShell](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway).
- [UserVoice 'ta daha sonra neleri derleyeceğinizi bize söyleyin](https://aka/natuservoice).
- [Genel önizleme hakkında geri bildirim sağlayın](https://aka.ms/natfeedback).
