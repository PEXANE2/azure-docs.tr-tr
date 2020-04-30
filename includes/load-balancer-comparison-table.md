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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78202546"
---
| | Standart Load Balancer | Temel Load Balancer |
| --- | --- | --- |
| [Arka uç havuzu boyutu](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) | En fazla 1000 örneği destekler. | En fazla 300 örneği destekler. |
| Arka uç havuzu uç noktaları | Tek bir sanal ağdaki herhangi bir sanal makine veya sanal makine ölçek kümesi. | Tek bir kullanılabilirlik kümesindeki veya sanal makine ölçek kümesindeki sanal makineler. |
| [Sistem durumu araştırmaları](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Durum araştırma azaltma davranışı](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP bağlantıları bir örnek araştırması üzerinde __ve__ tüm yoklamalar üzerinde etkin kalır. | TCP bağlantıları bir örnek araştırmasına etkin kalır. Tüm yoklamalar kapatıldığında tüm TCP bağlantıları sonlandırılır. |
| Kullanılabilirlik Alanları | Gelen ve giden trafik için bölge yedekli ve zikzak ön uçları. | Kullanılamaz |
| Tanılama | [Azure Izleyici çok boyutlu ölçümleri](../articles/load-balancer/load-balancer-standard-diagnostics.md) | [Azure İzleyici günlükleri](../articles/load-balancer/load-balancer-monitor-log.md) |
| HA bağlantı noktaları | [Iç Load Balancer için kullanılabilir](../articles/load-balancer/load-balancer-ha-ports-overview.md) | Kullanılamaz |
| Varsayılan olarak güvenli | Ağ güvenlik grubu tarafından izin verilmediği takdirde gelen akışlar için kapatıldı. Lütfen VNet 'ten iç yük dengeleyiciye iç trafiğe izin verildiğini unutmayın. | Varsayılan olarak açın. Ağ güvenlik grubu isteğe bağlı. |
| Giden Kuralları | [Bildirim temelli giden NAT yapılandırması](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Kullanılamaz |
| Boşta durumunda TCP sıfırlaması | [Herhangi bir kuralda kullanılabilir](../articles/load-balancer/load-balancer-tcp-reset.md) | Kullanılamaz |
| [Birden çok ön uç](../articles/load-balancer/load-balancer-multivip-overview.md) | Gelen ve [giden](../articles/load-balancer/load-balancer-outbound-connections.md) | Yalnızca gelen |
| Yönetim Işlemleri | Çoğu işlem 30 saniye < | 60-90 + saniye tipik |
| SLA | [% 99,99](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/) | Kullanılamaz | 
