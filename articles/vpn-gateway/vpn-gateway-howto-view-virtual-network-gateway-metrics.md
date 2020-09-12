---
title: Azure VPN Gateway ölçümlerini görüntüleme
description: VPN Gateway ölçümleri görüntüleme adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: alzam
ms.openlocfilehash: 4b33af3e64726e124373f57920836bce145cd891
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89443186"
---
# <a name="view-vpn-gateway-metrics"></a>VPN Gateway ölçümlerini görüntüleme

Azure Izleyici 'yi kullanarak Azure VPN ağ geçitlerini izleyebilirsiniz. Bu makalede Portal üzerinden kullanılabilen ölçümler ele alınmaktadır. Ölçümler hafif ve neredeyse gerçek zamanlı senaryoların yanı sıra uyarı ve hızlı sorun algılama için yararlı hale getirebilirsiniz.


|**Ölçüm**   | **Birim** | **Ayrıntı düzeyi** | **Açıklama** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bayt/sn  | 5 dakika| Ağ geçidinde tüm siteden siteye bağlantıların ortalama Birleşik bant genişliği kullanımı.     |
|**P2SBandwidth**| Bayt/sn  | 1 dakika  | Ağ geçidinde tüm Noktadan siteye bağlantıların ortalama Birleşik bant genişliği kullanımı.    |
|**P2SConnectionCount**| Count  | 1 dakika  | Ağ geçidinde Noktadan siteye bağlantı sayısı.   |
|**TunnelAverageBandwidth** | Bayt/sn    | 5 dakika  | Ağ geçidinde oluşturulan tünellerin ortalama bant genişliği kullanımı. |
|**TunnelEgressBytes** | Bayt | 5 dakika | Ağ geçidinde oluşturulan tünellerde giden trafik.   |
|**TunnelEgressPackets** | Count | 5 dakika | Ağ geçidinde oluşturulan Tünellerdeki giden paket sayısı.   |
|**TunnelEgressPacketDropTSMismatch** | Count | 5 dakika | Trafik seçici uyumsuzluğu nedeniyle oluşan tünellerde bırakılan giden paket sayısı. |
|**TunnelIngressBytes** | Bayt | 5 dakika | Ağ geçidinde oluşturulan tünellerde gelen trafik.   |
|**TunnelIngressPackets** | Count | 5 dakika | Ağ geçidinde oluşturulan Tünellerdeki gelen paket sayısı.   |
|**TunnelIngressPacketDropTSMismatch** | Count | 5 dakika | Trafik seçici uyumsuzluğu nedeniyle oluşan tünellerde bırakılan gelen paket sayısı. |

## <a name="the-following-steps-help-you-locate-and-view-metrics"></a>Aşağıdaki adımlar ölçümleri bulmanıza ve görüntülemenize yardımcı olur:

1. Portalda sanal ağ geçidi kaynağına gidin
2. Toplam Tünel giriş ve çıkış ölçümlerini görmek için **genel bakış** ' ı seçin.

   ![Uyarı kuralı oluşturma seçimleri](./media/vpn-gateway-howto-view-virtual-network-gateway-metrics/overview.png "Görüntüle")

3. Yukarıda listelenen diğer ölçümleri görüntülemek için. Sanal ağ geçidi kaynağınız altındaki **ölçümler** bölümüne tıklayın ve açılan listeden ölçüyü seçin.

   ![Kaynak listesinde Seç düğmesi ve VPN ağ geçidi](./media/vpn-gateway-howto-view-virtual-network-gateway-metrics/metrics.png "Şunu seçin:")

## <a name="next-steps"></a>Sonraki adımlar

Tünel ölçümlerinde uyarıları yapılandırmak için bkz. [VPN Gateway ölçümleri üzerinde uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).

Tünel kaynak günlüklerinde uyarıları yapılandırmak için bkz. [VPN Gateway kaynak günlüklerinde uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
