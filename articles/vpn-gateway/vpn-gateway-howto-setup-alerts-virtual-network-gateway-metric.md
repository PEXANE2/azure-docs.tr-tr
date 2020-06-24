---
title: Azure VPN Gateway ölçümleri üzerinde uyarı ayarlama
description: VPN Gateway ölçümlerinde uyarıları yapılandırma adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: ce9ab972d961a9193c3aa568878f9c93ccdcf6b1
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988025"
---
# <a name="set-up-alerts-on-vpn-gateway-metrics"></a>VPN Gateway ölçümlerinde uyarıları ayarlama

Bu makale, Azure VPN Gateway ölçümlerinde uyarı ayarlamanıza yardımcı olur. Azure Izleyici, Azure kaynakları için uyarı ayarlamanıza olanak sağlar. "VPN" türünün sanal ağ geçitleri için uyarı ayarlayabilirsiniz.


|**Ölçüm**   | **Birim** | **Ayrıntı düzeyi** | **Açıklama** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bayt/sn  | 5 dakika| Ağ geçidinde tüm siteden siteye bağlantıların ortalama Birleşik bant genişliği kullanımı.     |
|**P2SBandwidth**| Bayt/sn  | 1 dakika  | Ağ geçidinde tüm Noktadan siteye bağlantıların ortalama Birleşik bant genişliği kullanımı.    |
|**P2SConnectionCount**| Sayı  | 1 dakika  | Ağ geçidinde Noktadan siteye bağlantı sayısı.   |
|**TunnelAverageBandwidth** | Bayt/sn    | 5 dakika  | Ağ geçidinde oluşturulan tünellerin ortalama bant genişliği kullanımı. |
|**TunnelEgressBytes** | Bayt | 5 dakika | Ağ geçidinde oluşturulan tünellerde giden trafik.   |
|**TunnelEgressPackets** | Sayı | 5 dakika | Ağ geçidinde oluşturulan Tünellerdeki giden paket sayısı.   |
|**TunnelEgressPacketDropTSMismatch** | Sayı | 5 dakika | Trafik seçici uyumsuzluğu nedeniyle oluşan tünellerde bırakılan giden paket sayısı. |
|**TunnelIngressBytes** | Bayt | 5 dakika | Ağ geçidinde oluşturulan tünellerde gelen trafik.   |
|**TunnelIngressPackets** | Sayı | 5 dakika | Ağ geçidinde oluşturulan Tünellerdeki gelen paket sayısı.   |
|**TunnelIngressPacketDropTSMismatch** | Sayı | 5 dakika | Trafik seçici uyumsuzluğu nedeniyle oluşan tünellerde bırakılan gelen paket sayısı. |


## <a name="set-up-azure-monitor-alerts-based-on-metrics-by-using-the-azure-portal"></a><a name="setup"></a>Azure portal kullanarak ölçümleri temel alarak Azure Izleyici uyarılarını ayarlayın

Aşağıdaki örnek adımlar, için bir ağ geçidinde uyarı oluşturur:

- **Ölçüm:** TunnelAverageBandwidth
- **Koşul:** Bant genişliği > 10 bayt/saniye
- **Pencere:** 5 dakika
- **Uyarı eylemi:** E-posta



1. Sanal ağ geçidi kaynağına gidin ve **izleme** sekmesinden **Uyarılar** ' ı seçin. Sonra yeni bir uyarı kuralı oluşturun veya var olan bir uyarı kuralını düzenleyin.

   ![Uyarı kuralı oluşturma seçimleri](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "Oluştur")

2. Kaynak olarak VPN ağ geçidinizi seçin.

   ![Kaynak listesinde Seç düğmesi ve VPN ağ geçidi](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "Şunu seçin:")

3. Uyarı için yapılandırılacak bir ölçüm seçin.

   ![Ölçüm listesinde seçili ölçüm](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "Şunu seçin:")
4. Sinyal mantığını yapılandırın. Bunun üç bileşeni vardır:

    a. **Boyutlar**: ölçümün boyutları varsa, uyarının yalnızca o boyuttaki verileri vermesini sağlamak için belirli boyut değerlerini seçebilirsiniz. Bunlar isteğe bağlıdır.

    b. **Koşul**: Bu, ölçüm değerini değerlendirmek için bir işlemdir.

    c. **Zaman**: ölçüm verilerinin ayrıntı düzeyini ve uyarının değerlendirileceği süreyi belirtin.

   ![Sinyal mantığını yapılandırmaya ilişkin ayrıntılar](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "Şunu seçin:")

5. Yapılandırılan kuralları görüntülemek için, **Uyarı kurallarını yönet**' i seçin.

   ![Uyarı kurallarını yönetmek için düğme](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "Şunu seçin:")

## <a name="next-steps"></a>Sonraki adımlar

Tünel kaynak günlüklerinde uyarıları yapılandırmak için bkz. [VPN Gateway kaynak günlüklerinde uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
