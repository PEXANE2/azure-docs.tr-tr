---
title: Azure Sanal Ağ NAT için ölçümler ve uyarılar
titleSuffix: Azure Virtual Network
description: Sanal Ağ NAT için kullanılabilen Azure Monitor ölçümlerini ve uyarılarını anlayın.
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
ms.date: 03/04/2020
ms.author: allensu
ms.openlocfilehash: 4f1760c32117b34d4d453964473cba3c7d07f725
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79202187"
---
# <a name="azure-virtual-network-nat-metrics"></a>Azure Sanal Ağ NAT ölçümleri

Azure Sanal Ağ NAT ağ geçidi kaynakları çok boyutlu ölçümler sağlar. Bu ölçümleri işlemi gözlemlemek ve [sorun giderme](nat-metrics.md)için kullanabilirsiniz.  Uyarılar, SNAT tükenmesi gibi kritik sorunlar için yapılandırılabilir.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Internet'e giden ler için Sanal Ağ NAT">
</p>

*Şekil: Internet'e giden sanal ağ NAT*

## <a name="metrics"></a>Ölçümler

NAT ağ geçidi kaynakları Azure Monitor'da aşağıdaki çok boyutlu ölçümleri sağlar:

| Ölçüm | Açıklama | Önerilen Toplama | Boyutlar |
|---|---|---|---|
| Bayt | Baytlar gelen ve giden işlenmiş | Toplam | Yön (In; Çıkış), Protokol (6 TCP; 17 UDP) |
| Paket | Gelen ve giden paketler işlenir | Toplam | Yön (In; Çıkış), Protokol (6 TCP; 17 UDP) |
| Bırakılan paketler | Paketler NAT ağ geçidi tarafından bırakılan | Toplam | / |
| SNAT Bağlantı Sayısı | Aralık başına durum geçişleri | Toplam | Bağlantı Durumu, Protokol (6 TCP; 17 UDP) |
| Toplam SNAT bağlantı sayısı | Geçerli etkin SNAT bağlantıları (~ Kullanımdaki SNAT bağlantı noktaları) | Toplam | Protokol (6 TCP; 17 UDP) |


## <a name="alerts"></a>Uyarılar

Ölçümlere yönelik uyarılar, önceki [ölçümlerin](#metrics)her biri için Azure Monitörü'nde yapılandırılabilir.

## <a name="limitations"></a>Sınırlamalar

Kaynak Durumu desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal [Ağ NAT](nat-overview.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
* Azure [Monitör](../azure-monitor/overview.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynaklarını sorun giderme](troubleshoot-nat.md)hakkında bilgi edinin.
* [UserVoice Sanal Ağ NAT için sonraki oluşturmak için ne söyle.](https://aka.ms/natuservoice)


