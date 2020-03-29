---
title: 'Azure VPN Ağ Geçidi: Paket yakalamalarını yapılandırma'
description: VPN ağ geçitlerinde kullanabileceğiniz paket yakalama işlevleri hakkında bilgi edinin.
services: vpn-gateway
author: radwiv
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: radwiv
ms.openlocfilehash: 2429a8d08baa34aed120cffa069abae1fb9a3df9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75353513"
---
# <a name="configure-packet-captures-for-vpn-gateways"></a>VPN ağ geçitleri için paket yakalamalarını yapılandırma

Bağlantı ve performansla ilgili sorunlar genellikle karmaşıktır ve sorunun nedenini daraltmak için önemli miktarda zaman ve çaba gerektirir. Paket yakalama yeteneği, sorunun ağın müşteri tarafında mı, ağın Azure tarafında mı yoksa arada bir yerde mi olduğu gibi sorunun kapsamını ağın belirli bölümlerine daraltma süresini büyük ölçüde azaltmaya yardımcı olur. Sorun daraltıldıktan sonra hata ayıklama ve düzeltici eylemde bulunma çok daha verimli olur.

Paket yakalama için yaygın olarak kullanılabilen bazı araçlar vardır. Ancak, bu araçları kullanarak ilgili paket yakalamalar almak genellikle özellikle yüksek hacimli trafik senaryoları ile çalışırken sık sık hantal. VPN ağ geçidi paket yakalama tarafından sağlanan filtreleme yetenekleri önemli bir ayırt edici olur. Yaygın olarak kullanılabilen paket yakalama araçlarına ek olarak bir VPN ağ geçidi paketi yakalama kullanabilirsiniz.

## <a name="vpn-gateway-packet-capture-filtering-capabilities"></a>VPN ağ geçidi paket yakalama filtreleme özellikleri

VPN ağ geçidi paketi yakalamaları, müşteri ihtiyaçlarına bağlı olarak ağ geçidinde veya belirli bir bağlantıda çalıştırılabilir. Paket yakalamaları aynı anda birden çok tünelde de çalıştırabilirsiniz. VPN ağ geçidinde filtrelemenin yanı sıra tek veya çift yönlü trafiği, IKE ve ESP trafiğini ve iç paketleri yakalayabilirsiniz.

5 tuples filtresi (kaynak alt ağı, hedef alt ağ, kaynak bağlantı noktası, hedef bağlantı noktası, protokol) ve TCP bayrakları (SYN, ACK, FIN, URG, PSH, RST) kullanarak yüksek hacimli trafik sorunları yalıtma yararlıdır.

Paket yakalamayı çalıştırırken özellik başına yalnızca bir seçenek kullanabilirsiniz.

## <a name="setup-packet-capture-using-powershell"></a>PowerShell kullanarak paket yakalama

Paket yakalamaları başlatmak ve durdurmak için PowerShell komutları için aşağıdaki örneklere bakın. Parametre seçenekleri (filtre oluşturma gibi) hakkında daha fazla bilgi için bu PowerShell [belgesine](https://docs.microsoft.com/powershell/module/az.network/start-azvirtualnetworkgatewaypacketcapture)bakın.

### <a name="start-packet-capture-for-a-vpn-gateway"></a>VPN ağ geçidi için paket yakalamayı başlatma

```azurepowershell-interactive
Start-AzVirtualnetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName"
```

İsteğe bağlı parametre **-FilterData** filtre uygulamak için kullanılabilir.

### <a name="stop-packet-capture-for-a-vpn-gateway"></a>VPN ağ geçidi için paket yakalamayı durdurma

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName" -SasUrl "YourSASURL"
```

### <a name="start-packet-capture-for-a-vpn-gateway-connection"></a>VPN ağ geçidi bağlantısı için paket yakalamayı başlatma

```azurepowershell-interactive
Start-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName"
```

İsteğe bağlı parametre **-FilterData** filtre uygulamak için kullanılabilir.

### <a name="stop-packet-capture-on-a-vpn-gateway-connection"></a>VPN ağ geçidi bağlantısında paket yakalamayı durdurma

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName" -SasUrl "YourSASURL"
```

## <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular

- Paket yakalamaları çalıştırmak performansı etkileyebilir. Gerekli olmadığında paket yakalamayı durdurmayı unutmayın.
- Önerilen minimum paket yakalama süresi 600 saniyedir. Paket yakalama süresinin daha kısa olması, yoldaki birden çok bileşen arasında eşitleme sorunları nedeniyle tam veri sağlamayabilir.
- Paket yakalama veri dosyaları PCAP biçiminde oluşturulur. PCAP dosyalarını açmak için Wireshark veya yaygın olarak kullanılabilen diğer uygulamaları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

VPN Ağ Geçidi hakkında daha fazla bilgi için [VPN Ağ Geçidi Hakkında](vpn-gateway-about-vpngateways.md)
