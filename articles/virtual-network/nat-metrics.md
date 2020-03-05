---
title: Azure sanal ağ NAT için ölçümler ve uyarılar
titleSuffix: Azure Virtual Network
description: Azure Izleyici ölçümlerini ve sanal ağ NAT için kullanılabilir uyarıları anlayın.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to understand available Azure Monitor metrics and alerts for Virtual Network NAT.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/03/2020
ms.author: allensu
ms.openlocfilehash: 6bffa114b325e8b98f439e7ff2ce21480407a643
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274917"
---
# <a name="azure-virtual-network-nat-metrics"></a>Azure sanal ağ NAT ölçümleri

Azure sanal ağ NAT ağ geçidi kaynakları çok boyutlu ölçümler sağlar. Bu ölçümleri, işlemi gözlemlemek ve [sorun gidermek](nat-metrics.md)için kullanabilirsiniz.  Uyarılar, SNAT tükenmesi gibi kritik sorunlar için yapılandırılabilir.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Internet 'e giden sanal ağ NAT">
</p>

*Şekil: Internet 'e giden sanal ağ NAT*

>[!NOTE] 
>Sanal ağ NAT Şu anda genel önizleme olarak kullanılabilir. Şu anda yalnızca sınırlı sayıda [bölgede](nat-overview.md#region-availability)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

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

- [Sanal ağ NAT](nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
- [Azure izleyici](../azure-monitor/overview.md) hakkında bilgi edinin
