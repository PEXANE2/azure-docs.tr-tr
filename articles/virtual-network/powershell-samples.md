---
title: Sanal ağ için Azure PowerShell örnekleri | Microsoft Docs
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
ms.openlocfilehash: 418a72eb9a8f98bca24c863b2f953bfe720979e6
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249042"
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Sanal ağ için Azure PowerShell örnekleri

Aşağıdaki tablo, Azure PowerShell betiklerinin bağlantılarını içerir:

| | |
|----|----|
| [Çok katmanlı uygulamalar için sanal ağ oluşturma](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. 1433 numaralı bağlantı noktası için, ön uç alt ağına giden trafik HTTP ile sınırlıyken, arka uç alt ağına giden trafik SQL ile sınırlıdır. |
| [İki sanal ağı eşleme](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Aynı bölgede iki sanal ağ oluşturur ve bunları bağlar. |
| [Bir ağ sanal gereci yoluyla trafiği yönlendirme](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | İki alt ağ arasında trafiği yönlendirebilen bir sanal makine ve ön uç ve arka uç alt ağları içeren bir sanal ağ oluşturur. |
| [Gelen ve giden sanal makine ağ trafiğini filtreleme](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ön uç alt ağına gelen ağ trafiği, HTTP ve HTTPS ile sınırlıdır. Arka uç alt ağından İnternet’e giden trafiğe izin verilmez. |
|[Yapılandırma IPv4 + IPv6 ikili yığını temel Load Balancer ile sanal ağ](./scripts/virtual-network-powershell-sample-ipv6-dual-stack.md)|İkili yığın (IPv4 + IPv6) sanal ağ ile iki VM ve bir Azure temel yük dengeleyici genel IP adresleri IPv4 ve IPv6 ile dağıtır. |
|[Yapılandırma IPv4 + IPv6 ikili yığını standart Load Balancer ile sanal ağ](./scripts/virtual-network-powershell-sample-ipv6-dual-stack-standard-load-balancer.md)|İkili yığın (IPv4 + IPv6) sanal ağ ile iki VM ve IPv4 ve IPv6 genel IP adreslerine sahip Azure standart Load Balancer'ı dağıtır. |
