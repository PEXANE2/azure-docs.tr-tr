---
title: Sanal ağ için Azure CLI örnekleri
description: Azure CLı 'de, çok katmanlı uygulamalar için sanal ağ oluşturma da dahil olmak üzere görevleri tamamlamak için kullanabileceğiniz çeşitli örnek betikler hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
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
ms.custom: devx-track-azurecli
ms.openlocfilehash: 539d11205ffead52d7f40526f2c712e8cf8b5cdd
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501449"
---
# <a name="azure-cli-samples-for-virtual-network"></a>Sanal ağ için Azure CLI örnekleri

Aşağıdaki tablo, Azure CLI komutları ile bash betiklerine yönelik bağlantıları içerir:

| Komut Dosyası | Açıklama |
|----|----|
| [Çok katmanlı uygulamalar için sanal ağ oluşturma](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. 3306 numaralı bağlantı noktası için, ön uç alt ağına giden trafik HTTP ve SSH ile sınırlıyken, arka uç alt ağına giden trafik MySQL ile sınırlıdır. |
| [İki sanal ağı eşleme](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Aynı bölgede iki sanal ağ oluşturur ve bunları bağlar. |
| [Bir ağ sanal gereci yoluyla trafiği yönlendirme](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | İki alt ağ arasında trafiği yönlendirebilen bir sanal makine ve ön uç ve arka uç alt ağları içeren bir sanal ağ oluşturur. |
| [Gelen ve giden sanal makine ağ trafiğini filtreleme](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ön uç alt ağına gelen ağ trafiği, HTTP, HTTPS ve SSH ile sınırlıdır. Arka uç alt ağından İnternet’e giden trafiğe izin verilmez. |
|[Temel Load Balancer IPv4 + IPv6 çift yığın sanal ağını yapılandırma](./scripts/virtual-network-cli-sample-ipv6-dual-stack.md)|İki VM ile çift yığın (IPv4 + IPv6) sanal ağını ve IPv4 ve IPv6 genel IP adreslerine sahip bir Azure temel Load Balancer dağıtır. |
|[IPv4 + IPv6 çift yığın sanal ağını Standart Load Balancer ile yapılandırma](./scripts/virtual-network-cli-sample-ipv6-dual-stack-standard-load-balancer.md)|İki VM ile çift yığın (IPv4 + IPv6) sanal ağını ve IPv4 ve IPv6 genel IP adreslerine sahip bir Azure Standart Load Balancer dağıtır. |
|[Öğretici: NAT Gateway oluşturma ve test etme-Azure CLı](../virtual-network/tutorial-create-validate-nat-gateway-cli.md)|Kaynak ve hedef sanal makineyi kullanarak bir NAT ağ geçidi oluşturun ve doğrulayın. |
