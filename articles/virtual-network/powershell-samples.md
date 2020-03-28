---
title: Sanal ağ için Azure PowerShell örnekleri
description: Sanal ağ için Azure PowerShell örnekleri.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: de752cdacf17193d5be95b2b9f887938ace2d50f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74091882"
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Sanal ağ için Azure PowerShell örnekleri

Aşağıdaki tablo, Azure PowerShell betiklerinin bağlantılarını içerir:

| | |
|----|----|
| [Çok katmanlı uygulamalar için sanal ağ oluşturma](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. 1433 numaralı bağlantı noktası için, ön uç alt ağına giden trafik HTTP ile sınırlıyken, arka uç alt ağına giden trafik SQL ile sınırlıdır. |
| [İki sanal ağı eşleme](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Aynı bölgede iki sanal ağ oluşturur ve bunları bağlar. |
| [Bir ağ sanal gereci yoluyla trafiği yönlendirme](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | İki alt ağ arasında trafiği yönlendirebilen bir sanal makine ve ön uç ve arka uç alt ağları içeren bir sanal ağ oluşturur. |
| [Gelen ve giden sanal makine ağ trafiğini filtreleme](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ön uç alt ağına gelen ağ trafiği, HTTP ve HTTPS ile sınırlıdır. Arka uç alt ağından İnternet’e giden trafiğe izin verilmez. |
|[IPv4 + IPv6 çift yığın sanal ağı Temel Yük Dengeleyici ile yapılandır](./scripts/virtual-network-powershell-sample-ipv6-dual-stack.md)|İki VM ve IPv4 ve IPv6 ortak IP adresine sahip bir Azure Temel Yük Dengeleyicisi ile çift yığınlı (IPv4+IPv6) sanal ağ dağıtır. |
|[IPv4 + IPv6 çift yığın sanal ağı Standart Yük Dengeleyici ile yapılandır](./scripts/virtual-network-powershell-sample-ipv6-dual-stack-standard-load-balancer.md)|İki VM ve IPv4 ve IPv6 ortak IP adresine sahip bir Azure Standart Yük Dengeleyicisi ile çift yığınlı (IPv4+IPv6) sanal ağ dağıtır. |
