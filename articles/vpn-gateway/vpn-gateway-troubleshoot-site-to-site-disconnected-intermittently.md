---
title: Sorun giderme Azure Siteden Siteye VPN aralıklı olarak keser
description: Siteden Siteye VPN bağlantısının düzenli olarak kesildiği sorunu nasıl gidereceklerini öğrenin.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: chadmath
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 82054099a5a496e99c49135ab98ee1163af19784
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75862569"
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Sorun giderme: Azure Siteden Siteye VPN aralıklı olarak bağlantıyı keser

Yeni veya varolan microsoft Azure Siteden Siteye VPN bağlantısının kararlı olmaması veya düzenli olarak bağlantının kesilmesi sorunu yla karşılaşabilirsiniz. Bu makalede, sorunun nedenini belirlemenize ve çözmenize yardımcı olacak sorun giderme adımları sağlandığı. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

### <a name="prerequisite-step"></a>Önkoşul adımı

Azure sanal ağ ağ geçidinin türünü denetleyin:

1. Azure [portalına](https://portal.azure.com)gidin.
2. Tür bilgileri için sanal ağ ağ ağ geçidinin **Genel Bakış** sayfasını kontrol edin.
    
    ![Ağ geçidine genel bakış](media/vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently/gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Adım 1 Şirket içi VPN aygıtının doğrulanıp doğrulanmadığını kontrol edin

1. Doğrulanmış bir VPN aygıtı ve işletim sistemi sürümü kullanıp kullanmadığınızı kontrol [edin.](vpn-gateway-about-vpn-devices.md#devicetable) VPN aygıtı doğrulanmadıysa, herhangi bir uyumluluk sorunu olup olmadığını görmek için aygıt üreticisine başvurmanız gerekebilir.
2. VPN aygıtının doğru şekilde yapılandırıldığından emin olun. Daha fazla bilgi için [bkz.](vpn-gateway-about-vpn-devices.md#editing)

### <a name="step-2-check-the-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>Adım 2 Güvenlik Birliği ayarlarını kontrol edin (ilke tabanlı Azure sanal ağ ağ geçitleri için)

1. Microsoft Azure'daki **Yerel ağ ağ geçidi** tanımındaki sanal ağ, alt ağlar ve aralıkların şirket içi VPN aygıtındaki yapılandırmayla aynı olduğundan emin olun.
2. Güvenlik İlişkilendirme ayarlarının eşleştin.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>Ağ Geçidi Subnet'te Kullanıcı Tanımlı Rotalar veya Ağ Güvenlik Grupları için Adım 3 Denetimi

Ağ geçidi alt netinde kullanıcı tanımlı bir rota, bazı trafiği kısıtlıyor ve diğer trafiğe izin vermiyor olabilir. Bu VPN bağlantısı bazı trafik için güvenilmez ve diğerleri için iyi görünmesini sağlar. 

### <a name="step-4-check-the-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>Adım 4 "Subnet Pair başına bir VPN Tüneli" ayarını kontrol edin (ilke tabanlı sanal ağ ağ geçitleri için)

Şirket içi VPN aygıtının, ilke tabanlı sanal ağ ağ geçitleri için **alt ağ çifti başına bir VPN tüneline** sahip olacak şekilde ayarlandığından emin olun.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>Adım 5 Güvenlik İlişkilendirme Sınırlaması denetimi (ilke tabanlı sanal ağ ağ geçitleri için)

İlke tabanlı sanal ağ ağ geçidinin 200 alt ağ Güvenlik İlişkisi çifti sınırı vardır. Azure sanal ağ alt ağlarının sayısı yerel alt ağ sayısıyla çarpılırsa, ara sıra alt ağların kesildiğini görürsünüz.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>Adım 6 Şirket içi VPN cihazı dış arayüz adresini kontrol edin

- VPN aygıtının Internet'e bakan IP adresi Azure'daki **Yerel ağ ağ geçidi** tanımına dahil edilmişse, düzensiz kopukluklarla karşılaşabilirsiniz.
- Aygıtın dış arabirimi doğrudan Internet'te olmalıdır. Internet ile aygıt arasında Ağ Adresi Çevirisi (NAT) veya güvenlik duvarı olmamalıdır.
-  Güvenlik Duvarı Kümeleme'yi sanal bir IP'ye sahip olacak şekilde yapılandırırsanız, kümeyi kırmanız ve VPN cihazını doğrudan ağ geçidinin arayüz olabileceği ortak bir arabirime maruz bırakmanız gerekir.

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Adım 7 Şirket içi VPN aygıtının Mükemmel İleri Gizlilik etkin olup olmadığını kontrol edin

**Perfect Forward Secrecy** özelliği kopukluk sorunlarına neden olabilir. VPN **aygıtında Perfect forward Secrecy** etkinse, özelliği devre dışı bırakın. Ardından [sanal ağ ağ geçidi IPsec ilkesini güncelleştirin.](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy)

## <a name="next-steps"></a>Sonraki adımlar

- [Siteden Siteye bağlantıyı sanal ağa yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Siteden Siteye VPN bağlantıları için IPsec/IKE ilkesini yapılandırın](vpn-gateway-ipsecikepolicy-rm-powershell.md)

