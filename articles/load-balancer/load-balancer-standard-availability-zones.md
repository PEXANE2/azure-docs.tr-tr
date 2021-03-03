---
title: Azure Load Balancer ve Kullanılabilirlik Alanları
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
ms.openlocfilehash: 3c18b6d8dc44762649a9c07b88af348a18888fb5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101699122"
---
# <a name="load-balancer-and-availability-zones"></a>Load Balancer ve Kullanılabilirlik Alanları

Azure Load Balancer kullanılabilirlik alanları senaryolarını destekler. Kaynakları, bölgelere ayırarak ve bölgeler arasında dağıtım yaparak senaryonuz genelinde kullanılabilirliği artırmak için Standart Load Balancer kullanabilirsiniz.  Bu kavramları ve temel senaryo tasarım kılavuzunu anlamak için bu belgeyi gözden geçirin

Bir Load Balancer **bölge yedekli, zonal** veya **zBu olmayan** bir durumda olabilir. Yük dengeleyiciniz için bölgeyle ilgili özellikleri (yukarıda bahsedilen) yapılandırmak için gerekli olan ön uç türünü seçin.

## <a name="zone-redundant"></a>Bölge yedekli

Kullanılabilirlik Alanları bir bölgede, bir Standart Load Balancer bölge yedekli olabilir. Bu trafik, tek bir IP adresi tarafından sunulur.

Tek bir ön uç IP adresi bölge hatasını sürdüren. Ön uç IP, bölge ne olduğuna bakılmaksızın, tüm etkilenmeyen arka uç havuzu üyelerine erişmek için kullanılabilir. Bir veya daha fazla kullanılabilirlik bölgesi başarısız olabilir ve bölgedeki bir bölge sağlıklı kaldığı sürece veri yolu daha uzundur.

Ön uç 'nin IP adresi birden çok kullanılabilirlik alanında birden çok bağımsız altyapı dağıtımı tarafından aynı anda sunulur. Herhangi bir yeniden deneme veya yeniden yapılandırma, bölge hatasından etkilenmeyen diğer bölgelerde başarılı olur.

<p align="center">
  <img src="./media/az-zonal/zone-redundant-lb-1.svg" alt="Figure depicts a zone-redundant standard load balancer directing traffic in three different zones to three different subnets in a zone redundant configuration." width="512" title="Sanal ağ NAT">
</p>

*Şekil: bölge yedekli yük dengeleyici*

## <a name="zonal"></a>Bölgesel

Tek bir bölge için bir ön uç garantisini seçebilirsiniz ve bu da *Bölgesel* olarak bilinir.  Bu senaryo, herhangi bir gelen veya giden akışın bir bölgedeki tek bir bölge tarafından sunulduğunu gösterir.  Ön uç paylaşımlarınız, bölgenin sistem durumuyla birlikte fada.  Veri yolu, garantide olduğu yerde yer alan bölgelerde hatalara göre etkilenmemiştir. Kullanılabilirlik alanı başına bir IP adresi göstermek için, bölgesel ön uçları kullanabilirsiniz.  

Ayrıca, her bir bölgedeki yük dengeli uç noktalar için doğrudan bölgesel ön uçları kullanılması desteklenir. Her bölgeyi tek tek izlemek için, bu yapılandırmayı her bölge için yük dengeli uç nokta başına göstermek üzere kullanabilirsiniz. Ortak uç noktalar için, bunları [Traffic Manager](../traffic-manager/traffic-manager-overview.md) gıbı bir DNS Yük Dengeleme ürünüyle tümleştirilebilir ve tek bir DNS adı kullanabilirsiniz.


<p align="center">
  <img src="./media/az-zonal/zonal-lb-1.svg" alt="Figure depicts three zonal standard load balancers each directing traffic in a zone to three different subnets in a zonal configuration." width="512" title="Sanal ağ NAT">
</p>

*Şekil: ZGen yük dengeleyici*

Ortak yük dengeleyici ön ucu için genel IP 'ye bir **bölgeler** parametresi eklersiniz. Bu genel IP 'ye ilgili kural tarafından kullanılan ön uç IP yapılandırması tarafından başvurulur.

İç yük dengeleyici ön ucu için iç yük dengeleyici ön uç IP yapılandırmasına bir **bölgeler** parametresi ekleyin. Bir bölgesel ön ucu, alt ağdaki bir IP adresinin belirli bir bölgeye göre olmasını sağlar.

## <a name="design-considerations"></a><a name="design"></a> Tasarım konuları

Standart Load Balancer için bölgeyle ilgili özellikleri anladığınıza göre, aşağıdaki tasarım konuları yüksek kullanılabilirlik için tasarlandığınızda yardımcı olabilirler.

### <a name="tolerance-to-zone-failure"></a>Toleransı bölge hatası

- Bölgesel olarak **yedekli** Load Balancer, tek bir IP adresi ile herhangi bir bölgede bir bölgesel kaynağı sunabilir.  En az bir bölge bölge içinde sağlıklı kaldığı sürece IP bir veya daha fazla bölge hatasını daha fazla sürebilir.
- Bir **Bölgesel** ön ucu, hizmetin tek bir bölgeye bir azalmasıyla birlikte Fate 'yi ilgili bölge ile paylaşır. Dağıtımınızın bölgesi kapalıysa, dağıtımınız bu hatayla devam etmez.

Üretim iş yükleriniz için bölge yedekli Load Balancer kullanmanız önerilir.

### <a name="control-vs-data-plane-implications"></a>Denetim vs veri düzlemi etkileri

Bölge yedekliliği, itless veri düzlemi veya denetim düzlemi göstermez. Bölgesel olarak yedekli akışlar herhangi bir bölgeyi kullanabilir ve akışlarınız, bir bölgedeki tüm sağlıklı bölgeleri kullanır. Bir bölge hatasında, sağlıklı bölgeler kullanılarak trafik akışları etkilenmez.

Bölge hatası sırasında bölge kullanan trafik akışları etkilenebilir, ancak uygulamalar kurtarabilir. Trafik, Azure 'un bölge hatasını aşmak için yeniden aktarım sırasında bölge içindeki sağlıklı bölgelerde devam eder.

Uygulamanızın hata senaryolarıyla olan dayanıklılığını artırmak için [Azure bulut tasarım modellerini](/azure/architecture/patterns/) gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar
- [Kullanılabilirlik alanları](../availability-zones/az-overview.md) hakkında daha fazla bilgi edinin
- [Standart Load Balancer](./load-balancer-overview.md) hakkında daha fazla bilgi edinin
- Bir [bölge içindeki VM 'lerin yükünü bir bölgesel standart Load Balancer kullanarak yük dengelemesi](./quickstart-load-balancer-standard-public-cli.md) yapmayı öğrenin
- Bölgesel olarak [yedekli bir bölge kullanarak VM 'lerin yükünü dengelemek](./quickstart-load-balancer-standard-public-cli.md) hakkında bilgi edinin standart Load Balancer
- Uygulamanızın hata senaryolarıyla olan dayanıklılığını artırmak için [Azure bulut tasarım desenleri](/azure/architecture/patterns/) hakkında bilgi edinin.
