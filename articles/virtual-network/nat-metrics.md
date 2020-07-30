---
title: Azure sanal ağ NAT için ölçümler ve uyarılar
titleSuffix: Azure Virtual Network
description: Azure Izleyici ölçümlerini ve sanal ağ NAT için kullanılabilir uyarıları anlayın.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
ms.subservice: nat
Customer intent: As an IT administrator, I want to understand available Azure Monitor metrics and alerts for Virtual Network NAT.
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/04/2020
ms.author: allensu
ms.openlocfilehash: e3c47a60a6cda074eba7b5c3292577c29f50c2ab
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87424060"
---
# <a name="azure-virtual-network-nat-metrics"></a>Azure sanal ağ NAT ölçümleri

Azure sanal ağ NAT ağ geçidi kaynakları çok boyutlu ölçümler sağlar. Bu ölçümleri, işlemi gözlemlemek ve [sorun gidermek](troubleshoot-nat.md)için kullanabilirsiniz.  Uyarılar, SNAT tükenmesi gibi kritik sorunlar için yapılandırılabilir.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" alt="Figure depicts a NAT gateway resource that consumes all IP addresses for a public IP prefix and directs that traffic to and from two subnets of virtual machines and a virtual machine scale set." width="256" title="Internet 'e giden sanal ağ NAT">
</p>

*Şekil: Internet 'e giden sanal ağ NAT*

## <a name="metrics"></a>Ölçümler

NAT ağ geçidi kaynakları, Azure Izleyici 'de aşağıdaki çok boyutlu ölçümleri sağlar:

| Ölçüm | Açıklama | Önerilen toplama | Boyutlar |
|---|---|---|---|
| Bayt | Gelen ve giden işlenen bayt sayısı | Toplam | Yön (Içinde; Out), protokol (6 TCP; 17 UDP) |
| Paketleri | Gelen ve giden işlenen paketler | Toplam | Yön (Içinde; Out), protokol (6 TCP; 17 UDP) |
| Bırakılan paketler | NAT ağ geçidi tarafından bırakılan paketler | Toplam | / |
| SNAT bağlantı sayısı | Aralık başına durum geçişleri | Toplam | Bağlantı durumu, protokol (6 TCP; 17 UDP) |
| Toplam SNAT bağlantı sayısı | Geçerli etkin SNAT bağlantıları (~ SNAT bağlantı noktaları kullanımda) | Toplam | Protokol (6 TCP; 17 UDP) |


## <a name="alerts"></a>Uyarılar

Ölçüm uyarıları, yukarıdaki [ölçümlerin](#metrics)her biri Için Azure izleyici 'de yapılandırılabilir.

## <a name="limitations"></a>Sınırlamalar

Kaynak Durumu desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ NAT](nat-overview.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
* [Azure izleyici](../azure-monitor/overview.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynakları sorunlarını giderme](troubleshoot-nat.md)hakkında bilgi edinin.
* [UserVoice 'Ta sanal ağ NAT için bir sonraki derleme yapmanız gerektiğini bize söyleyin](https://aka.ms/natuservoice).


