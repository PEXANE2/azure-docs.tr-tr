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
ms.date: 1/14/2020
ms.author: allensu
ms.openlocfilehash: 08f7554ed9648df8b225fe9f205ee13b0f693c44
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170573"
---
# <a name="what-is-azure-load-balancer"></a>Azure Load Balancer nedir?

*Yük Dengeleme* , bir arka uç kaynak veya sunucu grubu genelinde yük devretme yükünü (gelen ağ trafiği) eşit bir şekilde dağıtmaktadır. 

Azure Load Balancer, açık sistemler arası (OSı) modelin dört katmanında çalışır. Bu, istemcilerle ilgili tek iletişim noktasıdır. Load Balancer yük dengeleyicinin ön ucuna, arka uç havuzu örneklerine ulaşan gelen akışları dağıtır. Bu akışlar, yapılandırılmış Yük Dengeleme kurallarına ve sistem durumu araştırmalara göre yapılır. Arka uç havuzu örnekleri, bir sanal makine ölçek kümesindeki Azure sanal makineleri veya örnekleri olabilir.

**[Ortak yük dengeleyici](./components.md#frontend-ip-configurations)** , sanal ağınız içindeki sanal makineler (VM) için giden bağlantılar sağlayabilir. Bu bağlantılar, özel IP adresleri genel IP adreslerine çevrilirken gerçekleştirilir. Ortak yük dengeleyiciler, sanal makinelerinize internet trafiğinin yükünü dengelemek için kullanılır.

Yalnızca ön uç üzerinde özel IP 'Lerin gerekli olduğu bir **[iç (veya özel) yük dengeleyici](./components.md#frontend-ip-configurations)** kullanılır. İç yük dengeleyiciler, bir sanal ağ içindeki trafiğin yükünü dengelemek için kullanılır. Bir yük dengeleyici ön uca bir karma senaryoda şirket içi ağdan erişilebilir.

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer.svg" width="512" title="Azure Load Balancer">
</p>

*Şekil: çok katmanlı uygulamaları hem genel hem de dahili Load Balancer kullanarak Dengeleme*

Ayrı yük dengeleyici bileşenleri hakkında daha fazla bilgi için bkz. [Azure Load Balancer Components](./components.md).

## <a name="why-use-azure-load-balancer"></a>Neden Azure Load Balancer kullanmalıyım?
Standart Load Balancer, uygulamalarınızı ölçeklendirebilir ve yüksek oranda kullanılabilir hizmetler oluşturabilirsiniz. Yük dengeleyici hem gelen hem de giden senaryoları destekler. Yük dengeleyici, düşük gecikme süresi ve yüksek aktarım hızı sağlar ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklendirir.

Standart Load Balancer kullanarak gerçekleştirebileceğiniz önemli senaryolar şunlardır:

- Azure sanal makinelerine **[iç](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-internal-portal)** ve **[dış](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)** trafik yükünü dengeleyin.

- **[Kaynakları bölgeler](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zonal-portal)** arasında ve bölgeler **[arasında](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal)** dağıtarak kullanılabilirliği artırın.

- Azure sanal makineleri için **[giden bağlantıyı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)** yapılandırın.

- Yük dengeli kaynakları izlemek için **[sistem durumu araştırmalarını](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** kullanın.

- Bir sanal ağdaki sanal makinelere genel IP adresi ve bağlantı noktasıyla erişmek için **[bağlantı noktası iletme](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** işlemini yapın.

- **[IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)** **[Yük Dengelemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell)** için desteği etkinleştirin.

- Standart Load Balancer, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview)aracılığıyla çok boyutlu ölçümler sağlar.  Bu ölçümler, belirli bir boyut için filtrelenebilir, gruplandırılabilir ve parçalanılabilir.  Bu kişiler, hizmetinizin performansı ve durumuyla ilgili güncel ve geçmiş öngörüler sağlar.  Kaynak Durumu de desteklenir. Daha fazla ayrıntı için **[Standart Load Balancer tanılamayı](load-balancer-standard-diagnostics.md)** inceleyin.

- **[Birden çok bağlantı noktası, birden çok IP adresi veya her ikisinde](https://docs.microsoft.com/azure/load-balancer/load-balancer-multivip-overview)** Yük Dengeleme Hizmetleri.

- Azure bölgeleri arasında **[iç](https://docs.microsoft.com/azure/load-balancer/move-across-regions-internal-load-balancer-portal)** ve **[dış](https://docs.microsoft.com/azure/load-balancer/move-across-regions-external-load-balancer-portal)** yük dengeleyici kaynaklarını taşıyın.

- Tüm bağlantı noktalarında TCP ve UDP Flow 'u aynı anda **[ha bağlantı noktalarını](https://docs.microsoft.com/azure/load-balancer/load-balancer-ha-ports-overview)** kullanarak yük dengeleyin.

### <a name="secure-by-default"></a><a name="securebydefault"></a>Varsayılan olarak güvenli

Standart Load Balancer, temel alınarak sıfır güvenli ağ güvenlik modelinde oluşturulmuştur. Varsayılan olarak güvenli Standart Load Balancer ve sanal ağınızın bir parçasıdır. Sanal ağ özel ve yalıtılmış bir ağ.  Bu, ağ güvenlik grupları tarafından açılmadığı müddetçe standart yük dengeleyiciler ve standart genel IP adreslerinin gelen akışlara kapalı olması anlamına gelir. NSG 'ler, izin verilen trafiğe açıkça izin vermek için kullanılır.  Sanal makine kaynağınızın bir alt ağda veya NIC 'inde bir NSG 'niz yoksa, trafiğin bu kaynağa erişmesine izin verilmez. NSG 'ler ve senaryonuz için nasıl uygulanacağı hakkında daha fazla bilgi edinmek için bkz. [ağ güvenlik grupları](../virtual-network/security-overview.md).
Temel Load Balancer varsayılan olarak Internet 'e açıktır. Ayrıca, Load Balancer müşteri verilerini depolamaz.

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

Standart Load Balancer fiyatlandırma bilgileri için bkz. [Load Balancer fiyatlandırması](https://azure.microsoft.com/pricing/details/load-balancer/).
Temel Load Balancer ücretsiz olarak sunulur.
[Load Balancer Için SLA](https://aka.ms/lbsla)konusuna bakın. Temel Load Balancer SLA 'sı yok.

## <a name="next-steps"></a>Sonraki adımlar
Temel Load Balancer Standart Load Balancer sürümüne yükseltmek için bkz. [temel Load Balancer yükseltme](upgrade-basic-standard.md) .

Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) .

Azure Load Balancer sınırlamaları ve bileşenleri hakkında daha fazla bilgi için bkz. [Azure Load Balancer bileşenler](./components.md) ve [Azure Load Balancer kavramları](./concepts.md)

Azure yük dengeleme seçenekleri karşılaştırması için bkz. [Azure 'da Yük Dengeleme seçeneklerine genel bakış](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).
