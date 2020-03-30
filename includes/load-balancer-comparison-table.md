---
title: include dosyası
description: include dosyası
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 02/21/2020
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 33e6fecafa2b9748a27794800365974ebc975d0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78202546"
---
| | Standart Yük Dengeleyici | Temel Yük Dengeleyici |
| --- | --- | --- |
| [Arka uç havuz boyutu](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) | 1000'e kadar örneği destekler. | 300'e kadar örneği destekler. |
| Arka uç havuzu bitiş noktaları | Herhangi bir sanal makine veya sanal makine ölçeği tek bir sanal ağda ayarlar. | Tek bir kullanılabilirlik kümesindeki sanal makineler veya sanal makine ölçeği kümesi. |
| [Sistem durumu araştırmaları](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Sağlık sondası aşağı davranış](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP bağlantıları bir örnek sondaaşağı __ve__ tüm sondalar aşağı canlı kalır. | TCP bağlantıları bir örnek sondaaşağı canlı kalır. Tüm sondalar çöktüğinde tüm TCP bağlantıları sonlanır. |
| Kullanılabilirlik Alanları | Gelen ve giden trafik için bölge gereksiz ve bölge cepheleri. | Kullanılamaz |
| Tanılama | [Azure Monitör çok boyutlu ölçümler](../articles/load-balancer/load-balancer-standard-diagnostics.md) | [Azure Monitör günlükleri](../articles/load-balancer/load-balancer-monitor-log.md) |
| HA Bağlantı Noktaları | [Dahili Yük Dengeleyicisi için kullanılabilir](../articles/load-balancer/load-balancer-ha-ports-overview.md) | Kullanılamaz |
| Varsayılan olarak güvenli | Bir ağ güvenlik grubu tarafından izin verilmedikçe gelen akışlara kapalı. VNet'ten dahili yük dengeleyicisine dahili trafiğe izin verildiğini lütfen unutmayın. | Varsayılan olarak açın. Ağ güvenlik grubu isteğe bağlıdır. |
| Giden Kuralları | [Bildirimsel giden NAT yapılandırması](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Kullanılamaz |
| Boşta TCP Sıfırlama | [Herhangi bir kuralda kullanılabilir](../articles/load-balancer/load-balancer-tcp-reset.md) | Kullanılamaz |
| [Birden fazla ön uç](../articles/load-balancer/load-balancer-multivip-overview.md) | Gelen ve [giden](../articles/load-balancer/load-balancer-outbound-connections.md) | Yalnızca gelen |
| Yönetim İşlemleri | Çoğu işlem 30 saniye < | 60-90+ saniye tipik |
| SLA | [99.99%](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/) | Kullanılamaz | 
