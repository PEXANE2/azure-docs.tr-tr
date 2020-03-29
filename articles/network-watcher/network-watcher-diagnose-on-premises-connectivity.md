---
title: VPN ağ geçidi aracılığıyla Şirket Içi bağlantı tanılama
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure Ağ İzleyicikaynak sorun giderme ile VPN ağ geçidi üzerinden şirket içi bağlantı nın nasıl tanılandığı açıklanmaktadır.
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 835b3a69e779b536961110b674ae67f4e8c13ce0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76845059"
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>VPN ağ geçitleri aracılığıyla şirket içi bağlantıyı tanılama

Azure VPN Ağ Geçidi, şirket içi ağınızla Azure sanal ağınız arasında güvenli bir bağlantı gereksinimini gideren karma çözüm oluşturmanıza olanak tanır. Gereksinimleriniz benzersiz olduğundan, şirket içi VPN cihazının seçimi de önemlidir. Azure şu anda aygıt satıcılarıyla işbirliği içinde sürekli olarak doğrulanmış [birkaç VPN aygıtını](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) destekler. Şirket içi VPN aygıtınızı yapılandırmadan önce cihaza özel yapılandırma ayarlarını gözden geçirin. Benzer şekilde, Azure VPN Ağ Geçidi, bağlantı kurmak için kullanılan [desteklenen bir iPsec parametreleri](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) kümesiyle yapılandırılır. Şu anda Azure VPN Ağ Geçidi'nden belirli bir IPsec parametre kombinasyonunu belirtmeniz veya seçmeniz için bir yol yoktur. Şirket içi ve Azure arasında başarılı bir bağlantı kurmak için şirket içi VPN aygıt ayarlarının Azure VPN Ağ Geçidi tarafından öngörülen IPsec parametrelerine uygun olması gerekir. Ayarlar yanlışsa, bağlantı kaybı vardır ve şimdiye kadar bu sorunları giderme önemsiz değildi ve genellikle sorunu tanımlamak ve düzeltmek saatler sürdü.

Azure Ağ İzleyicisi sorun giderme özelliği yle, Ağ Geçidi ve Bağlantılarınızla ilgili tüm sorunları tanılayabilir ve birkaç dakika içinde sorunu düzeltmek için bilinçli bir karar vermek için yeterli bilgiye sahip olabilirsiniz.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario"></a>Senaryo

FortiGate'i şirket içi VPN Ağ Geçidi olarak kullanarak Azure ile şirket içi arasında site bağlantısı yapılandırmak istiyorsunuz. Bu senaryoyu gerçekleştirmek için aşağıdaki kurulum gerekir:

1. Sanal Ağ Ağ Geçidi - Azure'daki VPN Ağ Geçidi
1. Yerel Ağ Ağ Geçidi - Azure bulutundaki [şirket içi (FortiGate) VPN Ağ Geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) gösterimi
1. Siteden siteye bağlantı (rota tabanlı) - [VPN Ağ Geçidi ile şirket içi yönlendirici arasındaki bağlantı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#CreateConnection)
1. [FortiGate'i Yapılandırma](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Siteden Siteye yapılandırma yapılandırmak için adım adım ayrıntılı kılavuz ziyaret ederek bulunabilir: [Azure portalını kullanarak Siteden Siteye bağlantısı olan bir VNet oluşturun.](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Kritik yapılandırma adımlarından biri, IPsec iletişim parametrelerini yapılandırmaktır, herhangi bir yanlış yapılandırma şirket içi ağ ve Azure arasında bağlantı kaybına yol açar. Şu anda Azure VPN Ağ Geçitleri, Aşama 1 için aşağıdaki IPsec parametrelerini destekleyecek şekilde yapılandırılmıştır. Aşağıdaki tabloda görebileceğiniz gibi, Azure VPN Ağ Geçidi tarafından desteklenen şifreleme algoritmaları AES256, AES128 ve 3DES'tir.

### <a name="ike-phase-1-setup"></a>IKE faz 1 kurulumu

| **Özellik** | **PolicyBased** | **RouteBased ve Standart veya Yüksek Performanslı VPN ağ geçidi** |
| --- | --- | --- |
| IKE Sürümü |IKEv1 |IKEv2 |
| Diffie-Hellman Grubu |Grup 2 (1024 bit) |Grup 2 (1024 bit) |
| Kimlik Doğrulama Yöntemi |Önceden Paylaşılan Anahtar |Önceden Paylaşılan Anahtar |
| Şifreleme Algoritmaları |AES256 AES128 3DES |AES256 3DES |
| Karma Algoritma |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Aşama 1 Güvenlik İlişkisi (SA) Yaşam Süresi (Zaman) |28.800 saniye |28.800 saniye |

Bir kullanıcı olarak FortiGate'inizi yapılandırmanız gerekir, örneğin bir yapılandırma [GitHub'da](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt)bulunabilir. Bilmeden FortiGate'inizi SHA-512'yi karma algoritma olarak kullanacak şekilde yapılandırıldınız. Bu algoritma ilke tabanlı bağlantılar için desteklenen bir algoritma olmadığı için VPN bağlantınız çalışır.

Bu sorunları gidermek zordur ve kök nedenleri genellikle sezgisel değildir. Bu durumda, sorunu çözmek için yardım almak için bir destek bileti açabilirsiniz. Ancak Azure Ağ İzleyicisi SORUN Giderme API ile bu sorunları kendi başınıza tanımlayabilirsiniz.

## <a name="troubleshooting-using-azure-network-watcher"></a>Azure Ağ İzleyicisini kullanarak sorun giderme

Bağlantınızı tanılamak için Azure PowerShell'e `Start-AzNetworkWatcherResourceTroubleshooting` bağlanın ve cmdlet'i başlatın. Bu cmdlet'i kullanarak ilgili ayrıntıları [Troubleshoot Virtual Network Gateway ve bağlantılarında](network-watcher-troubleshoot-manage-powershell.md)bulabilirsiniz - PowerShell . Bu cmdletin tamamlanması birkaç dakika kadar sürebilir.

Cmdlet tamamlandıktan sonra, konu ve günlükler hakkında ayrıntılı bilgi almak için cmdlet'te belirtilen depolama konumuna gidebilirsiniz. Azure Ağ İzleyicisi aşağıdaki günlük dosyalarını içeren bir zip klasörü oluşturur:

![1][1]

IKEErrors.txt adlı dosyayı açın ve şirket içi IKE ayarı yanlış yapılandırmasıyla ilgili bir sorunu belirten aşağıdaki hatayı görüntüler.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

Bu durumda bağlantı düzgün çalışmıyor bu yol olduğunu `ERROR_IPSEC_IKE_POLICY_MATCH` bahseder gibi, hata hakkında Scrubbed-wfpdiag.txt ayrıntılı bilgi alabilirsiniz.

Başka bir yaygın yanlış yapılandırma yanlış paylaşılan anahtarları belirtme olduğunu. Önceki örnekte farklı paylaşılan anahtarlar belirtmişseniz, IKEErrors.txt aşağıdaki `Error: Authentication failed. Check shared key`hatayı gösterir: .

Azure Ağ İzleyicisorun giderme özelliği, VPN Ağ Geçidinizi ve Bağlantınızı basit bir PowerShell cmdlet kolaylığıyla tanılamanızı ve sorun gidermenizi sağlar. Şu anda aşağıdaki koşullarıtanılama yı destekliyoruz ve daha fazla koşul eklemek için çalışıyoruz.

### <a name="gateway"></a>Ağ geçidi

| Hata Türü | Neden | Günlük|
|---|---|---|
| NoFault | Hata saptanmadı. |Evet|
| GatewayNotFound | Ağ Geçidi veya Ağ Geçidi bulunamaz, sağlanmaz. |Hayır|
| PlannedMaintenance |  Ağ geçidi örneği bakım aşamasında.  |Hayır|
| UserDrivenUpdate | Bir kullanıcı güncelleştirmesi devam ediyor. Bu bir yeniden boyutlandırma işlemi olabilir. | Hayır |
| VipUnResponsive | Ağ Geçidi'nin birincil örneğine ulaşamıyor. Bu, sistem durumu sondası başarısız olduğunda gerçekleşir. | Hayır |
| PlatformInActive | Platform ile ilgili bir sorun var. | Hayır|
| ServiceNotRunning | Temel hizmet çalışmıyor. | Hayır|
| NoConnectionsFoundForGateway | Ağ geçidinde Bağlantı yok. Bu sadece bir uyarı.| Hayır|
| BağlantılarNotConnected | Bağlantıların hiçbiri bağlı değil. Bu sadece bir uyarı.| Evet|
| Ağ GeçidiCPUUsageExceeded | Geçerli Ağ Geçidi kullanımı CPU kullanımı %95'>. | Evet |

### <a name="connection"></a>Bağlantı

| Hata Türü | Neden | Günlük|
|---|---|---|
| NoFault | Hata saptanmadı. |Evet|
| GatewayNotFound | Ağ Geçidi veya Ağ Geçidi bulunamaz, sağlanmaz. |Hayır|
| PlannedMaintenance | Ağ geçidi örneği bakım aşamasında.  |Hayır|
| UserDrivenUpdate | Bir kullanıcı güncelleştirmesi devam ediyor. Bu bir yeniden boyutlandırma işlemi olabilir.  | Hayır |
| VipUnResponsive | Ağ Geçidi'nin birincil örneğine ulaşamıyor. Sağlık sondası başarısız olduğunda olur. | Hayır |
| BağlantıEntityNotFound | Bağlantı yapılandırması eksik. | Hayır |
| Bağlantıİşaretli Kesildi | Bağlantı "bağlantısı kesildi" olarak işaretlenir. |Hayır|
| BağlantıNotConfiguredOnGateway | Temel hizmet, Bağlantı yapılandırılmış değildir. | Evet |
| BağlantıİşaretliStandby | Temel hizmet bekleme olarak işaretlenir.| Evet|
| Kimlik doğrulaması | Önceden paylaşılan Anahtar uyuşmazlığı. | Evet|
| PeerReachability | Eş ağ geçidine erişilemez. | Evet|
| IkePolicyMismatch | Eş ağ geçidinde Azure tarafından desteklenmeyen IKE ilkeleri vardır. | Evet|
| WfpParse Hatası | WFP günlüğünü ayrışdıran bir hata oluştu. |Evet|

## <a name="next-steps"></a>Sonraki adımlar

[Azure Ağ İzleyicisorun giderme ile Monitor VPN ağ geçitlerini](network-watcher-monitor-with-azure-automation.md) ziyaret ederek PowerShell ve Azure Otomasyonu ile VPN Ağ Geçidi bağlantısını kontrol etmeyi öğrenin

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
