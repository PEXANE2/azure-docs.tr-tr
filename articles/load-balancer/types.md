---
title: Azure Load Balancer türleri
description: Azure Load Balancer türlerine genel bakış
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2020
ms.author: allensu
ms.openlocfilehash: 31e2bf19967bb8870ee6ab75687bb3fcc37373f7
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82629976"
---
# <a name="azure-load-balancer-types"></a>Azure Load Balancer türleri

Azure Load Balancer iki türe ve iki SKU 'ya sahiptir.

## <a name="public-load-balancer"></a><a name = "publicloadbalancer"></a>Ortak yük dengeleyici

Ortak yük dengeleyici, gelen trafiğin genel IP ve bağlantı noktasını, sanal makinenin özel IP ve bağlantı noktasıyla eşleştirir. Yük dengeleyici trafiği VM 'den gelen yanıt trafiği için başka bir şekilde eşler. Yük Dengeleme kuralları uygulayarak, belirli trafik türlerini birden çok VM veya hizmet arasında dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.

>[!NOTE]
>Her kullanılabilirlik kümesi için yalnızca bir ortak yük dengeleyici ve bir iç yük dengeleyici uygulayabilirsiniz.

Aşağıdaki şekilde, genel ve TCP bağlantı noktası 80 için üç VM arasında paylaşılan web trafiği için yük dengeli bir uç nokta gösterilmektedir. Bu üç VM, bir yük dengeleme kümesinde bulunur.

![Ortak yük dengeleyici örneği](./media/load-balancer-overview/load-balancer.png)

*Şekil: bir genel yük dengeleyici kullanarak Web trafiğini Dengeleme*

Internet istemcileri, 80 numaralı TCP bağlantı noktasındaki bir Web uygulamasının genel IP adresine Web sayfası istekleri gönderir. Azure Load Balancer, istekleri yük dengeli küme içindeki üç sanal makineye dağıtır. Yük dengeleyici algoritmaları hakkında daha fazla bilgi için bkz. [yük dengeleyici kavramları](concepts.md).

Azure Load Balancer, ağ trafiğini varsayılan olarak birden çok VM örneği arasında eşit olarak dağıtır. Oturum benzeşimini de yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](load-balancer-distribution-mode.md).

## <a name="internal-load-balancer"></a><a name = "internalloadbalancer"></a>İç yük dengeleyici

İç yük dengeleyici, trafiği bir sanal ağ içindeki kaynaklara dağıtır. Azure, yük dengeli bir sanal ağın ön uç IP adreslerine erişimi kısıtlar. 

Ön uç IP adresleri ve sanal ağlar hiçbir şekilde doğrudan bir internet uç noktasına gösterilmez. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır.

İç yük dengeleyici, aşağıdaki yük dengeleme türlerini sunar:

* **Bir sanal ağ içinde**: sanal ağdaki VM 'lerden, aynı sanal ağda bulunan bir VM kümesine yük dengeleme.
* **Şirketler arası sanal ağ için**: şirket içi bilgisayarlardan aynı sanal ağdaki bir VM kümesine yük dengeleme.
* **Çok katmanlı uygulamalar için**: arka uç katmanlarının internet 'e açık olmadığı internet 'e yönelik çok katmanlı uygulamalar için yük dengeleme. Arka uç katmanları, internet 'e yönelik katmandan trafik yük dengelemesi gerektirir. Sonraki şekle bakın.
* **İş kolu uygulamaları için**: Ek yük dengeleyici donanım veya yazılım olmadan Azure'da barındırılan iş kolu uygulamaları için yük dengeleme. Bu senaryo, trafiği yük dengeli olan bilgisayar kümesinde bulunan şirket içi sunucuları içerir.

![İç Load Balancer örneği](./media/load-balancer-overview/load-balancer.png)

*Şekil: çok katmanlı uygulamaları hem genel hem de iç yük dengeleyiciyi kullanarak Dengeleme*

## <a name="load-balancer-sku-comparison"></a><a name="skus"></a> Load Balancer SKU karşılaştırması

Yük dengeleyici hem temel hem de standart SKU 'Ları destekler. Bu SKU 'Lar senaryo ölçeğinde, özelliklerde ve fiyatlandırmaya göre farklılık gösterir. Temel yük dengeleyici ile mümkün olan herhangi bir senaryo, standart yük dengeleyici ile oluşturulabilir.

Farkları karşılaştırmak ve anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz. [Azure Standart Load Balancer genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft, standart yük dengeleyiciyi öneriyor.
Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Yük dengeleyici ve genel IP adresi SKU 'SU ortak IP adresleriyle kullandığınızda aynı olmalıdır. Yük dengeleyici ve genel IP SKU 'Ları değişebilir değildir.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Daha fazla bilgi için bkz. [yük dengeleyici sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer). Standart Load Balancer hakkında ayrıntılı bilgi için bkz. [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing) ve [SLA](https://aka.ms/lbsla).

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) .
- [Standart Load Balancer ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- [Sistem durumu araştırmaları](load-balancer-custom-probe-overview.md)hakkında bilgi edinin.
- [Giden bağlantılar için Load Balancer](load-balancer-outbound-connections.md)kullanma hakkında bilgi edinin.
- [Ha bağlantı noktaları Yük Dengeleme kurallarıyla standart Load Balancer](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.