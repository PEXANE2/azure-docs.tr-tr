---
title: Azure Load Balancer nedir?
titleSuffix: Azure Load Balancer
description: Azure Load Balancer özelliklerine, mimarisine ve uygulamasına genel bakış. Yük Dengeleyicisinin nasıl çalıştığını ve bulutta nasıl kullanılacağını öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/14/2020
ms.author: allensu
ms.openlocfilehash: 8596b435ffa02da7daf4ef98bfe0fe7995b9270a
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768186"
---
# <a name="what-is-azure-load-balancer"></a>Azure Load Balancer nedir?

*Yük dengeleme,* yükün (gelen ağ trafiği) bir arka uç kaynağı veya sunucu lar grubuna eşit olarak dağıtılması anlamına gelir. 

Azure Yük Dengeleyici, Açık Sistemler Ara Bağlantısı (OSI) modelinin dördüncü katmanında çalışır. Müşteriler için tek temas noktası. Yük Dengeleyici, yük bakiyesinin ön ucuna gelen gelen akışları arka uç havuz örneklerine dağıtır. Bu akışlar yapılandırılmış yük dengeleme kurallarına ve sağlık sondalarına göre yapılır. Arka uç havuzu örnekleri, sanal makine ölçeği kümesindeki Azure Sanal Makineleri veya örnekleri olabilir.

**[Genel yük dengeleyicisi,](./concepts-limitations.md#publicloadbalancer)** sanal ağınızdaki sanal makineler (VM' ler) için giden bağlantılar sağlayabilir. Bu bağlantılar, özel IP adreslerini ortak IP adreslerine çevirerek gerçekleştirilir. Kamu Yük Dengeleyicileri, bakiyeli internet trafiğini VM'lerinize yüklemek için kullanılır.

Yalnızca ön uçta özel IP'lere ihtiyaç duyulduğunda **[dahili (veya özel) bir yük dengeleyici](./concepts-limitations.md#internalloadbalancer)** kullanılır. Dahili yük dengeleyicileri, sanal ağ içindeki bakiye trafiğini yüklemek için kullanılır. Bir yük dengeleyici ön ucuna karma senaryoda şirket içi bir ağdan erişilebilir.

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer.svg" width="512" title="Azure Load Balancer">
</p>

*Şekil: Hem genel hem de dahili Yük Dengeleyicisi kullanarak çok katmanlı uygulamaları dengeleme*

Tek tek yük dengeleyici bileşenleri hakkında daha fazla bilgi için [Azure Yük Dengeleyici bileşenleri ve sınırlamaları](./concepts-limitations.md)

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. Yüksek performanslı, düşük gecikme süresi, Katman-7 yük dengelemesi gerekiyorsa, [bkz.](../application-gateway/overview.md) Global DNS yük dengeleme arıyorsanız, [bkz.](../traffic-manager/traffic-manager-overview.md) Uçtan uca senaryolarınız bu çözümleri birleştirerek yararlanabilir.
>
> Azure yük dengeleme seçenekleri karşılaştırması için [Azure'da yük dengeleme seçeneklerine genel bakış'a](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview)bakın.

## <a name="why-use-azure-load-balancer"></a>Azure Yük Dengeleyicisi neden kullanıyor?
Standart Yük Dengeleyicisi ile uygulamalarınızı ölçeklendirebilir ve yüksek kullanılabilirhizmetler oluşturabilirsiniz. Yük dengeleyicisi hem gelen hem de giden senaryoları destekler. Yük dengeleyicisi düşük gecikme süresi ve yüksek iş akışı sağlar ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklenir.

Standart Yük Dengeleyicisi'ni kullanarak gerçekleştirebileceğiniz önemli senaryolar şunlardır:

- Azure sanal makinelerine **[iç](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)** ve **[dış](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-internal-portal)** trafiği yükleyin.

- Bölgeler **[içinde](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zonal-portal)** ve bölgeler **[arasında](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal)** kaynak dağıtarak kullanılabilirliği artırın.

- Azure sanal makineleri için **[giden bağlantıyı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)** yapılandırın.

- Yük dengeli kaynakları izlemek için **[sistem durumu sondalarını](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** kullanın.

- Genel IP adresi ve bağlantı noktası tarafından sanal ağdaki sanal makinelere erişmek için **[bağlantı noktası yönlendirmesini](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** çalıştırın.

- **[IPv6'nın](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)** **[yük dengelemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell)** için desteği etkinleştirin.

- Standart Yük Dengeleyici, [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)aracılığıyla çok boyutlu ölçümler sağlar.  Bu ölçümler, belirli bir boyut için filtrelenebilir, gruplandırılabilir ve kırılabilir.  Bunlar, hizmetinizin performansı ve sağlığı hakkında güncel ve tarihi bilgiler sağlar.  Kaynak Durumu da desteklenir. Daha fazla bilgi için **[Standart Yük Dengeleyici Tanılama'yı](load-balancer-standard-diagnostics.md)** gözden geçirin.

- Birden çok **[bağlantı noktası, birden çok IP adresi veya her ikisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-multivip-overview)** üzerinde yük dengesi hizmetleri.

- **[Dahili](https://docs.microsoft.com/azure/load-balancer/move-across-regions-internal-load-balancer-portal)** ve **[harici](https://docs.microsoft.com/azure/load-balancer/move-across-regions-external-load-balancer-portal)** yük dengeleyici kaynaklarını Azure bölgeleri boyunca taşıyın.

- **[Ha bağlantı noktalarını](https://docs.microsoft.com/azure/load-balancer/load-balancer-ha-ports-overview)** kullanarak tüm bağlantı noktalarında aynı anda yük dengesi TCP ve UDP akışı.

### <a name="secure-by-default"></a><a name="securebydefault"></a>Varsayılan olarak güvenli

Standart Yük Dengeleyici, özünde sıfır güven ağı güvenlik modeli üzerine kurulmuştur. Standart Yük Dengeleyicisi varsayılan olarak güvenlidir ve sanal ağınızın bir parçasıdır. Sanal ağ özel ve yalıtılmış bir ağdır.  Bu, Ağ Güvenlik Grupları tarafından açılmadığı sürece Standart Yük Dengeleyicileri ve Standart Genel IP adreslerinin gelen akışlara kapalı olduğu anlamına gelir. NSG'ler, izin verilen trafiğe açıkça izin vermek için kullanılır.  Sanal makine kaynağınızın bir alt ağına veya NIC'de NSG yoksa, trafiğin bu kaynağa ulaşmasına izin verilmez. NSG'ler ve bunları senaryonuz için nasıl uygulayacağınız hakkında daha fazla bilgi edinmek için [Ağ Güvenlik Grupları'na](../virtual-network/security-overview.md)bakın.
Temel Yük Dengeleyici varsayılan olarak internete açıktır.


## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

Standart Yük Dengeleyici fiyatlandırma bilgileri [için, Bkz. Yük Dengeleyici fiyatlandırması.](https://azure.microsoft.com/pricing/details/load-balancer/)
Temel Load Balancer ücretsiz olarak sunulur.
[Yük Dengeleyicisi için SLA'ya](https://aka.ms/lbsla)bakın. Temel Yük Dengeleyicisi'nde SLA yoktur.

## <a name="next-steps"></a>Sonraki adımlar
Temel Yük Dengeleyicisini Standart Yük Dengeleyicisine yükseltmek için [Temel Yük Dengeleyicisini Yükselt'e](upgrade-basic-standard.md) bakın.

Bkz. Yük Dengeleyicisi kullanmaya başlamak için [genel bir Standart Yük Dengeleyicisi Oluşturun.](quickstart-load-balancer-standard-public-portal.md)

Azure Yük Dengeleyici sınırlamaları ve bileşenleri hakkında daha fazla bilgi için [Azure Yük Bakiyesi kavramları ve sınırlamaları](./concepts-limitations.md) hakkında bilgi
