---
title: Azure Load Balancer SKU 'Ları
description: Azure Load Balancer SKU 'Larına genel bakış
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/01/2020
ms.author: allensu
ms.openlocfilehash: c7ca630b4a6a1bedeab21feacc22cd27a1a3ee7e
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82794157"
---
# <a name="azure-load-balancer-skus"></a>Azure Load Balancer SKU 'Ları

Azure Load Balancer iki türüler veya SKU 'su vardır.

## <a name="sku-comparison"></a><a name="skus"></a>SKU karşılaştırması

Yük dengeleyici hem temel hem de standart SKU 'Ları destekler. Bu SKU 'Lar senaryo ölçeğinde, özelliklerde ve fiyatlandırmaya göre farklılık gösterir. Temel yük dengeleyici ile mümkün olan herhangi bir senaryo, standart yük dengeleyici ile oluşturulabilir.

Farkları karşılaştırmak ve anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz. [Azure Standart Load Balancer genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft, standart yük dengeleyiciyi öneriyor.
Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Yük dengeleyici ve genel IP adresi SKU 'SU ortak IP adresleriyle kullandığınızda aynı olmalıdır. Yük dengeleyici ve genel IP SKU 'Ları değişebilir değildir.

| | Standart Load Balancer | Temel Load Balancer |
| --- | --- | --- |
| [Arka uç havuzu boyutu](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) | En fazla 1000 örneği destekler. | En fazla 300 örneği destekler. |
| Arka uç havuzu uç noktaları | Tek bir sanal ağdaki herhangi bir sanal makine veya sanal makine ölçek kümesi. | Tek bir kullanılabilirlik kümesindeki veya sanal makine ölçek kümesindeki sanal makineler. |
| [Sistem durumu araştırmaları](./load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Durum araştırma azaltma davranışı](./load-balancer-custom-probe-overview.md#probedown) | TCP bağlantıları bir örnek araştırması üzerinde __ve__ tüm yoklamalar üzerinde etkin kalır. | TCP bağlantıları bir örnek araştırmasına etkin kalır. Tüm yoklamalar kapatıldığında tüm TCP bağlantıları sonlandırılır. |
| Kullanılabilirlik Alanları | Gelen ve giden trafik için bölge yedekli ve zikzak ön uçları. | Kullanılamaz |
| Tanılama | [Azure Izleyici çok boyutlu ölçümleri](./load-balancer-standard-diagnostics.md) | [Azure İzleyici günlükleri](./load-balancer-monitor-log.md) |
| HA bağlantı noktaları | [Iç Load Balancer için kullanılabilir](./load-balancer-ha-ports-overview.md) | Kullanılamaz |
| Varsayılan olarak güvenli | Ağ güvenlik grubu tarafından izin verilmediği takdirde gelen akışlar için kapatıldı. Lütfen VNet 'ten iç yük dengeleyiciye iç trafiğe izin verildiğini unutmayın. | Varsayılan olarak açın. Ağ güvenlik grubu isteğe bağlı. |
| Giden Kuralları | [Bildirim temelli giden NAT yapılandırması](./load-balancer-outbound-rules-overview.md) | Kullanılamaz |
| Boşta durumunda TCP sıfırlaması | [Herhangi bir kuralda kullanılabilir](./load-balancer-tcp-reset.md) | Kullanılamaz |
| [Birden çok ön uç](./load-balancer-multivip-overview.md) | Gelen ve [giden](./load-balancer-outbound-connections.md) | Yalnızca gelen |
| Yönetim Işlemleri | Çoğu işlem 30 saniye < | 60-90 + saniye tipik |
| SLA | [% 99,99](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/) | Kullanılamaz | 

Daha fazla bilgi için bkz. [yük dengeleyici sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer). Standart Load Balancer hakkında ayrıntılı bilgi için bkz. [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing) ve [SLA](https://aka.ms/lbsla).

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) .
- [Standart Load Balancer ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- [Sistem durumu araştırmaları](load-balancer-custom-probe-overview.md)hakkında bilgi edinin.
- [Giden bağlantılar için Load Balancer](load-balancer-outbound-connections.md)kullanma hakkında bilgi edinin.
- [Ha bağlantı noktaları Yük Dengeleme kurallarıyla standart Load Balancer](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
