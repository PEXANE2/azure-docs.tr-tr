---
title: Azure Load Balancer nedir?
titleSuffix: Azure Load Balancer
description: Azure Load Balancer özelliklerine, mimarisine ve uygulamasına genel bakış. Load Balancer nasıl çalıştığını ve bulutta nasıl kullanacağınızı öğrenin.
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
ms.date: 1/14/2019
ms.author: allensu
ms.openlocfilehash: dc986d40d50b93720c87ba36d265ed3044b0abc9
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045380"
---
# <a name="what-is-azure-load-balancer"></a>Azure Load Balancer nedir?

*Yük Dengeleme* , bir arka uç kaynak veya sunucu grubu genelinde yük devretme yükünü (gelen ağ trafiği) eşit bir şekilde dağıtmaktadır. Azure, ihtiyaya göre seçebileceğiniz [çeşitli yük dengeleme seçenekleri](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) sunar. Bu belgede Azure Load Balancer yer almaktadır.

Azure Load Balancer, açık sistemler arası (OSı) modelin dört katmanında çalışır. Bu, istemcilerle ilgili tek iletişim noktasıdır. Yük dengeleyici, yük dengeleyicinin ön ucuna arka uç havuz örneklerine ulaşan yeni gelen akışları dağıtır. Bu akışlar yapılandırılmış Yük Dengeleme kurallarına ve sistem durumu araştırmalara göre yapılır. Arka uç havuzu örnekleri, bir sanal makine ölçek kümesindeki Azure sanal makineleri veya örnekleri olabilir.


**[Ortak yük dengeleyici](./concepts-limitations.md#publicloadbalancer)** , sanal ağınız içindeki sanal makineler (VM) için giden bağlantılar sağlayabilir. Bu bağlantılar, özel IP adresleri genel IP adreslerine çevrilirken gerçekleştirilir. Ortak yük dengeleyiciler, sanal makinelerinize internet trafiğinin yükünü dengelemek için kullanılır.

Yalnızca ön uç üzerinde özel IP 'Lerin gerekli olduğu bir **[iç (veya özel) yük dengeleyici](./concepts-limitations.md#internalloadbalancer)** kullanılır. İç yük dengeleyiciler, bir sanal ağ içindeki trafiğin yükünü dengelemek için kullanılır. Bir yük dengeleyici ön uca bir karma senaryoda şirket içi ağdan erişilebilir.

<div align="center">
  <img src='./media/load-balancer-overview/IC744147.png'>
</div>

*Şekil: çok katmanlı uygulamaları hem genel hem de dahili Load Balancer kullanarak Dengeleme*

Ayrı yük dengeleyici bileşenleri hakkında daha fazla bilgi için bkz. [Azure Load Balancer bileşenleri ve sınırlamaları](./concepts-limitations.md)

>[!NOTE]
> Microsoft [Standart Load Balancer](./load-balancer-standard-overview.md)önerir.
Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Load Balancer ve genel IP adresi SKU 'SU ortak IP adresleriyle kullandığınızda aynı olmalıdır. Load Balancer ve genel IP SKU 'Ları değişebilir değildir.

## <a name="why-use-azure-load-balancer"></a>Neden Azure Load Balancer kullanmalıyım?
Azure Load Balancer, uygulamalarınızı ölçeklendirebilir ve yüksek oranda kullanılabilir hizmetler oluşturabilirsiniz. Yük dengeleyici hem gelen hem de giden senaryoları destekler. Yük dengeleyici, düşük gecikme süresi ve yüksek aktarım hızı sağlar ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklendirir.

Azure Load Balancer kullanarak gerçekleştirebileceğiniz önemli senaryolar şunlardır:

- Azure sanal makinelerine **[iç](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)** ve **[dış](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-internal-portal)** trafik yükünü dengeleyin.

- **[Kaynakları bölgeler](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zonal-portal)** arasında ve bölgeler **[arasında](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal)** dağıtarak kullanılabilirliği artırın.

- Azure sanal makineleri için **[giden bağlantıyı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)** yapılandırın.

- Yük dengeli kaynakları izlemek için **[sistem durumu araştırmalarını](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** kullanın.

- Bir sanal ağdaki sanal makinelere genel IP adresi ve bağlantı noktasıyla erişmek için **[bağlantı noktası iletme](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** işlemini yapın.

- **[IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)** **[Yük Dengelemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell)** için desteği etkinleştirin.

- **[Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview)** ile Azure Load Balancer yönelik **[ölçülerden ve tanılamalarından](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)** yararlanın.

- **[Birden çok bağlantı noktası, birden çok IP adresi veya her ikisinde](https://docs.microsoft.com/azure/load-balancer/load-balancer-multivip-overview)** Yük Dengeleme Hizmetleri.

- Azure bölgeleri arasında **[iç](https://docs.microsoft.com/azure/load-balancer/move-across-regions-internal-load-balancer-portal)** ve **[dış](https://docs.microsoft.com/azure/load-balancer/move-across-regions-external-load-balancer-portal)** yük dengeleyici kaynaklarını taşıyın.

- Tüm bağlantı noktalarında TCP ve UDP Flow 'u aynı anda **[ha bağlantı noktalarını](https://docs.microsoft.com/azure/load-balancer/load-balancer-ha-ports-overview)** kullanarak yük dengeleyin.

## <a name="pricing"></a>Fiyatlandırma

Standart Load Balancer kullanım ücretlendirilir.

* Yapılandırılmış yük dengeleme ve giden kural sayısı. Gelen NAT kuralları toplam kural sayısı içinde sayılmaz.
* Gelen ve giden kurallardan bağımsız olarak işlenen veri miktarı.

Standart Load Balancer fiyatlandırma bilgileri için bkz. [Load Balancer fiyatlandırması](https://azure.microsoft.com/pricing/details/load-balancer/).

Temel Load Balancer ücretsiz olarak sunulur.

## <a name="sla"></a>SLA

SLA Standart Load Balancer hakkında daha fazla bilgi için bkz. [Load Balancer Için SLA](https://aka.ms/lbsla).

## <a name="next-steps"></a>Sonraki adımlar

Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) .

Azure Load Balancer sınırlamaları ve bileşenleri hakkında daha fazla bilgi için bkz. [Azure Load Balancer kavramlar ve sınırlamalar](./concepts-limitations.md)
