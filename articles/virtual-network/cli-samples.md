---
title: Sanal ağ için Azure CLI örnekleri
description: Sanal ağ için Azure CLI örnekleri.
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
ms.openlocfilehash: 5ce10cf37b61b0269e6c8f2279b8814d9dc4a4f9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78271214"
---
# <a name="azure-cli-samples-for-virtual-network"></a>Sanal ağ için Azure CLI örnekleri

Aşağıdaki tablo, Azure CLI komutları ile bash betiklerine yönelik bağlantıları içerir:

| | |
|----|----|
| [Çok katmanlı uygulamalar için sanal ağ oluşturma](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. 3306 numaralı bağlantı noktası için, ön uç alt ağına giden trafik HTTP ve SSH ile sınırlıyken, arka uç alt ağına giden trafik MySQL ile sınırlıdır. |
| [İki sanal ağı eşleme](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Aynı bölgede iki sanal ağ oluşturur ve bunları bağlar. |
| [Bir ağ sanal gereci yoluyla trafiği yönlendirme](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | İki alt ağ arasında trafiği yönlendirebilen bir sanal makine ve ön uç ve arka uç alt ağları içeren bir sanal ağ oluşturur. |
| [Gelen ve giden sanal makine ağ trafiğini filtreleme](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ön uç alt ağına gelen ağ trafiği, HTTP, HTTPS ve SSH ile sınırlıdır. Arka uç alt ağından İnternet’e giden trafiğe izin verilmez. |
|[IPv4 + IPv6 çift yığın sanal ağı Temel Yük Dengeleyici ile yapılandır](./scripts/virtual-network-cli-sample-ipv6-dual-stack.md)|İki VM ve IPv4 ve IPv6 ortak IP adresine sahip bir Azure Temel Yük Dengeleyicisi ile çift yığınlı (IPv4+IPv6) sanal ağ dağıtır. |
|[IPv4 + IPv6 çift yığın sanal ağı Standart Yük Dengeleyici ile yapılandır](./scripts/virtual-network-cli-sample-ipv6-dual-stack-standard-load-balancer.md)|İki VM ve IPv4 ve IPv6 ortak IP adresine sahip bir Azure Standart Yük Dengeleyicisi ile çift yığınlı (IPv4+IPv6) sanal ağ dağıtır. |
|[Öğretici: Bir NAT ağ geçidi oluşturma ve test edin - Azure CLI](../virtual-network/tutorial-create-validate-nat-gateway-cli.md)|Kaynak ve hedef sanal makine kullanarak bir NAT ağ geçidi oluşturun ve doğrulayın. |