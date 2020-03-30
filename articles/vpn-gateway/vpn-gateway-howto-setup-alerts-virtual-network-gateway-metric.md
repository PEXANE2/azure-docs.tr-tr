---
title: Azure VPN Ağ Geçidi ölçümlerinde uyarıları ayarlama
description: VPN Ağ Geçidi ölçümlerinde uyarıları yapılandırma adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: d57663f683ba4e2107ec6813a19fac7b2dcdd26a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67605225"
---
# <a name="set-up-alerts-on-vpn-gateway-metrics"></a>VPN Ağ Geçidi ölçümlerinde uyarıları ayarlama

Bu makale, Azure VPN Ağ Geçidi ölçümlerinde uyarılar ayarlamanıza yardımcı olur. Azure Monitor, Azure kaynakları için uyarılar ayarlama olanağı sağlar. "VPN" türüsanal ağ ağ geçitleri için uyarılar ayarlayabilirsiniz.


|**Ölçüm**   | **Birim** | **Ayrıntı düzeyi** | **Açıklama** | 
|---       | ---        | ---       | ---            | ---       |
|**Ortalama Bant Genişliği**| Bayt/s  | 5 dakika| Ağ geçidindeki tüm siteden siteye bağlantıların ortalama birleştirilmiş bant genişliği kullanımı.     |
|**P2SBant Genişliği**| Bayt/s  | 1 dakika  | Ağ geçidindeki tüm noktadan siteye bağlantıların ortalama birleştirilmiş bant genişliği kullanımı.    |
|**P2SConnectionCount**| Sayı  | 1 dakika  | Ağ geçidindeki noktadan siteye bağlantıların sayısı.   |
|**TünelOrtalamaBant Genişliği** | Bayt/s    | 5 dakika  | Ağ geçidinde oluşturulan tünellerin ortalama bant genişliği kullanımı. |
|**TunnelEgressBayt** | Bayt | 5 dakika | Ağ geçidinde oluşturulan tünellerde giden trafik.   |
|**TunnelEgressPackets** | Sayı | 5 dakika | Ağ geçidinde oluşturulan tünellerde giden paketlerin sayısı.   |
|**TunnelEgressPacketDropTSMismatch** | Sayı | 5 dakika | Trafik seçici uyuşmazlığı nedeniyle tünellere bırakılan giden paketlerin sayısı. |
|**TunnelIngressBayt** | Bayt | 5 dakika | Ağ geçidinde oluşturulan tünellerde gelen trafik.   |
|**TunnelIngressPaketleri** | Sayı | 5 dakika | Ağ geçidinde oluşturulan tünellerde gelen paketlerin sayısı.   |
|**Tünel IngressPacketDropTSMismatch** | Sayı | 5 dakika | Trafik seçici uyuşmazlığı nedeniyle tünellere bırakılan gelen paketlerin sayısı. |


## <a name="set-up-azure-monitor-alerts-based-on-metrics-by-using-the-azure-portal"></a><a name="setup"></a>Azure portalını kullanarak ölçümlere göre Azure Monitör uyarıları ayarlama

Aşağıdaki örnek adımlar için bir ağ geçidinde bir uyarı oluşturur:

- **Metrik:** TünelOrtalamaBant Genişliği
- **Koşul:** Bant genişliği > 10 bayt/saniye
- **Pencere:** 5 dakika
- **Uyarı eylemi:** E-posta



1. Sanal ağ ağ geçidi kaynağına gidin ve **İzleme** sekmesinden **Uyarılar'ı** seçin. Ardından yeni bir uyarı kuralı oluşturun veya varolan bir uyarı kuralını edin.

   ![Uyarı kuralı oluşturmak için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "Oluşturma")

2. Kaynak olarak VPN ağ geçidinizi seçin.

   ![Kaynaklar listesinde Seç düğmesi ve VPN ağ geçidi](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "Şunu seçin:")

3. Uyarı için yapılandırmak için bir metrik seçin.

   ![Ölçümler listesinde seçilen metrik](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "Şunu seçin:")
4. Sinyal mantığını yapılandırın. Bunun üç bileşeni vardır:

    a. **Boyutlar**: Ölçümün boyutları varsa, uyarının yalnızca o boyutun verilerini değerlendirecek şekilde belirli boyut değerlerini seçebilirsiniz. Bunlar isteğe bağlıdır.

    b. **Koşul**: Bu, metrik değeri değerlendirme işlemidir.

    c. **Saat**: Metrik verilerin taneciklerini ve uyarıyı değerlendirmek için süreyi belirtin.

   ![Sinyal mantığını yapılandırmak için ayrıntılar](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "Şunu seçin:")

5. Yapılandırılan kuralları görüntülemek için **uyarı kurallarını yönet'i**seçin.

   ![Uyarı kurallarını yönetmek için düğme](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "Şunu seçin:")

## <a name="next-steps"></a>Sonraki adımlar

Tünel tanılama günlüklerinde uyarıları yapılandırmak için VPN [Ağ Geçidi tanı günlüklerinde uyarıları ayarlama bölümüne](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md)bakın.
