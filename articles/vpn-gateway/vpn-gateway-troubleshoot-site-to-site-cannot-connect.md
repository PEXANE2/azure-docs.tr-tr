---
title: "TitleSuffix'i bağlayamayan bir Azure siteden siteye VPN bağlantısının giderme sorunu: Azure VPN Ağ Geçidi"
description: Aniden çalışmayı durduran ve yeniden bağlanamayan siteden siteye VPN bağlantısını nasıl gidereceğinizi öğrenin.
services: vpn-gateway
author: chadmath
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 09/16/2019
ms.author: genli
ms.openlocfilehash: 4e827c5f6eedc819bc3635cb09a28f65df51312c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75862586"
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Sorun giderme: Azure sitesinden siteye VPN bağlantısı bağlanamıyor ve çalışmayı durduramıyor

Şirket içi ağ ve Azure sanal ağı arasında siteden siteye VPN bağlantısı yapılandırdıktan sonra, VPN bağlantısı aniden çalışmayı durdurur ve yeniden bağlanamaz. Bu makalede, bu sorunu çözmenize yardımcı olacak sorun giderme adımları sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Sorunu çözmek için önce [Azure VPN ağ geçidini sıfırlamayı](vpn-gateway-resetgw-classic.md) ve tüneli şirket içi VPN aygıtından sıfırlamayı deneyin. Sorun devam ederse, sorunun nedenini belirlemek için aşağıdaki adımları izleyin.

### <a name="prerequisite-step"></a>Önkoşul adımı

Azure VPN ağ geçidinin türünü kontrol edin.

1. [Azure portalına](https://portal.azure.com)gidin.

2. Tür bilgileri için VPN ağ geçidinin **Genel Bakış** sayfasını kontrol edin.
    
    ![Ağ geçidine genel bakış](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>1. Adım. Şirket içi VPN aygıtının doğrulanıp doğrulanmadığını kontrol edin

1. Doğrulanmış bir VPN aygıtı ve işletim sistemi sürümü kullanıp kullanmadığınızı kontrol [edin.](vpn-gateway-about-vpn-devices.md#devicetable) Aygıt doğrulanmış bir VPN aygıtı değilse, uyumluluk sorunu olup olmadığını görmek için aygıt üreticisine başvurmanız gerekebilir.

2. VPN aygıtının doğru şekilde yapılandırıldığından emin olun. Daha fazla bilgi için [bkz.](vpn-gateway-about-vpn-devices.md#editing)

### <a name="step-2-verify-the-shared-key"></a>2. Adım Paylaşılan anahtarı doğrulama

Şirket içi VPN aygıtının paylaşılan anahtarını, tuşların eşleştiğından emin olmak için Azure Sanal Ağ VPN ile karşılaştırın. 

Azure VPN bağlantısı için paylaşılan anahtarı görüntülemek için aşağıdaki yöntemlerden birini kullanın:

**Azure portalında**

1. Oluşturduğunuz VPN ağ geçidi siteden siteye bağlantıya gidin.

2. **Ayarlar** bölümünde Paylaşılan **tuşu**tıklatın.
    
    ![Paylaşılan anahtar](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure Kaynak Yöneticisi dağıtım modeli için:

    Get-AzVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Klasik dağıtım modeli için:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-the-vpn-peer-ips"></a>3. Adım VPN eş IP'lerini doğrulayın

-   Azure'daki **Yerel Ağ Ağ Ağ Geçidi** nesnesindeki IP tanımı, şirket içi aygıt IP ile eşleşmelidir.
-   Şirket içi aygıtta ayarlanan Azure ağ geçidi IP tanımı, Azure ağ geçidi IP'si ile eşleşmelidir.

### <a name="step-4-check-udr-and-nsgs-on-the-gateway-subnet"></a>4. Adım. Ağ geçidi alt netinde UDR ve NSG'leri kontrol edin

Ağ geçidi alt ağındaki kullanıcı tanımlı yönlendirme (UDR) veya Ağ Güvenlik Grupları 'nı (NSGs) denetleyin ve kaldırın ve sonucu test edin. Sorun çözülürse, UDR veya NSG'nin uyguladığı ayarları doğrulayın.

### <a name="step-5-check-the-on-premises-vpn-device-external-interface-address"></a>5. Adım. Şirket içi VPN aygıtının dış arayüz adresini kontrol edin

- VPN aygıtının Internet'e bakan IP adresi Azure'daki **Yerel ağ** tanımına eklenmişse, düzensiz kopukluklarla karşılaşabilirsiniz.
- Aygıtın dış arabirimi doğrudan Internet'te olmalıdır. Internet ve aygıt arasında ağ adresi çevirisi veya güvenlik duvarı olmamalıdır.
- Güvenlik duvarı kümelemesini sanal BIR IP'ye sahip olacak şekilde yapılandırmak için kümeyi kırmanız ve VPN cihazını doğrudan ağ geçidinin arabirim olabileceği ortak bir arabirime maruz bırakmanız gerekir.

### <a name="step-6-verify-that-the-subnets-match-exactly-azure-policy-based-gateways"></a>6. Adım. Alt ağların tam olarak eşleşin (Azure ilkesi tabanlı ağ geçitleri)

-   Sanal ağ adresi alanının(lar) Azure sanal ağı yla şirket içi tanımlar arasında tam olarak eşleşip eşleşmediğini doğrulayın.
-   Alt ağların Yerel Ağ **Ağ Geçidi** ile şirket içi ağ için şirket içi tanımlar arasında tam olarak eşleştiğini doğrulayın.

### <a name="step-7-verify-the-azure-gateway-health-probe"></a>7. Adım. Azure ağ geçidi sistem durumu sondasını doğrulama

1. Aşağıdaki URL'ye göz atarak sağlık soruşturmasını açın:

    `https://<YourVirtualNetworkGatewayIP>:8081/healthprobe`

2. Sertifika uyarısını tıklayın.
3. Bir yanıt alırsanız, VPN ağ geçidi sağlıklı kabul edilir. Yanıt almazsanız, ağ geçidi sağlıklı olmayabilir veya ağ geçidi alt netindeki bir NSG soruna neden olabilir. Aşağıdaki metin örnek bir yanıttır:

    ```xml
    <?xml version="1.0"?>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Primary Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6</string>
    ```

### <a name="step-8-check-whether-the-on-premises-vpn-device-has-the-perfect-forward-secrecy-feature-enabled"></a>8. Adım Şirket içi VPN aygıtının mükemmel ileri gizlilik özelliğietkin olup olmadığını kontrol edin

Mükemmel ileri gizlilik özelliği kopukluk sorunlarına neden olabilir. VPN aygıtı mükemmel ileri gizlilik etkinse, özelliği devre dışı bırakın. Ardından VPN ağ geçidi IPsec ilkesini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

-   [Siteden siteye bağlantıyı sanal ağa yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Siteden siteye VPN bağlantıları için bir IPsec/IKE ilkesini yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md)
