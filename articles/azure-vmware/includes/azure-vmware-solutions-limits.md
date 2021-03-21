---
title: Azure VMware Çözüm sınırları
description: Azure VMware Çözüm sınırlamaları.
ms.topic: include
ms.date: 03/16/2021
ms.openlocfilehash: 0e2359d951f5348b69e95ab7fa046981b2b7b32d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103622240"
---
<!-- Used in /azure/azure-resource-manager/management/azure-subscription-service-limits.md -->

Aşağıdaki tabloda, Azure VMware çözümü için en fazla sınır açıklanmaktadır.

| **Kaynak** | **Sınırlı** |
| :-- | :-- |
| Özel bulut başına kümeler | 12 |
| Küme başına en az düğüm sayısı | 3 |
| Küme başına en fazla düğüm sayısı | 16 |
| Özel bulut başına düğümler | 64 |
| özel bulut başına vCenter | 1  |
| HCX site eşleştirmeleri | 3 Advanced Edition ile 10 Enterprise Edition |
| AVS ExpressRoute maksimum bağlı SDDC | 4 |
| AVS ExpressRoute portspeed | 10 Gbps<br />Kullanılan sanal ağ geçidi, gerçek bant genişliğini belirler. Daha ayrıntılı bilgi için bkz. [ExpressRoute sanal ağ geçitleri hakkında](../../expressroute/expressroute-about-virtual-network-gateways.md) | 
| VWAN aracılığıyla kullanıma sunulan genel IP 'Ler | 100 |
| vSAN kapasite sınırları | Toplam kullanılabilir %75 (SLA için %25 oranında kullanılabilir)  |

Diğer VMware 'e özgü sınırlar için lütfen [VMware yapılandırması en yüksek aracı](https://configmax.vmware.com/)'nı kullanın!.
