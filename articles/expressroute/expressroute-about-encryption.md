---
title: 'Azure ExpressRoute: Şifreleme Hakkında'
description: ExpressRoute şifrelemesi hakkında bilgi edinin.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: cherylmc
ms.openlocfilehash: 7dd9106539b6756d74629ac663241a5b5562cefb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75437056"
---
# <a name="expressroute-encryption"></a>ExpressRoute şifrelemesi
 
ExpressRoute, ağınızla Microsoft'un ağı arasında veri geçişinin gizliliğini ve bütünlüğünü sağlamak için birkaç şifreleme teknolojilerini destekler.

## <a name="point-to-point-encryption-by-macsec-faq"></a>MACsec SSS ile noktadan noktaya şifreleme
MACsec bir [IEEE standardıdır.](https://1.ieee802.org/security/802-1ae/) Medya Erişimi denetimi (MAC) düzeyindeki veya Ağ Katmanı 2'deki verileri şifreler. [ExpressRoute Direct](expressroute-erdirect-about.md)üzerinden Microsoft'a bağlandığınızda ağ aygıtlarınız ve Microsoft'un ağ aygıtları arasındaki fiziksel bağlantıları şifrelemek için MACsec'i kullanabilirsiniz. MACsec varsayılan olarak ExpressRoute Direct bağlantı noktalarında devre dışı bırakılır. Şifreleme için kendi MACsec anahtarınızı getirin ve [Azure Key Vault'ta](../key-vault/key-vault-overview.md)saklayın. Anahtarı ne zaman döndüreceğine siz karar verirsiniz. Aşağıdaki diğer SSS'lere bakın.
### <a name="can-i-enable-macsec-on-my-expressroute-circuit-provisioned-by-an-expressroute-provider"></a>ExpressRoute devremde BIR ExpressRoute sağlayıcısı tarafından sağlanan MACsec'i etkinleştirebilir miyim?
Hayır. MACsec, fiziksel bir bağlantıdaki tüm trafiği tek bir tüzel kişiye (yani müşteriye) ait bir anahtarla şifreler. Bu nedenle, yalnızca ExpressRoute Direct'te kullanılabilir.
### <a name="can-i-encrypt-some-of-the-expressroute-circuits-on-my-expressroute-direct-ports-and-leave-other-circuits-on-the-same-ports-unencrypted"></a>ExpressRoute Direct bağlantı noktalarımdaki ExpressRoute devrelerinden bazılarını şifreleyebilir ve aynı bağlantı noktalarındaki diğer devreleri şifrelenmeden bırakabilir miyim? 
Hayır. MACsec etkinleştirildiğinde, örneğin bgp veri trafiği ve müşteri veri trafiği tüm ağ kontrol trafiği şifrelenir. 
### <a name="when-i-enabledisable-macsec-or-update-macsec-key-will-my-on-premises-network-lose-connectivity-to-microsoft-over-expressroute"></a>MACsec'i etkinleştirdiğim/devre dışı kurduğumda veya MACsec tuşunu güncellediğimde şirket içi ağım ExpressRoute üzerinden Microsoft'a olan bağlantısını kaybeder mi?
Evet. MACsec yapılandırması için yalnızca önceden paylaşılan anahtar modunu destekliyoruz. Bu, anahtarı hem aygıtlarınızda hem de Microsoft'un (API'miz aracılığıyla) güncelleştirmeniz gerektiği anlamına gelir. Bu değişiklik atomik değildir, bu nedenle iki taraf arasında önemli bir uyumsuzluk olduğunda bağlantıyı kaybedersiniz. Yapılandırma değişikliği için bir bakım penceresi zamanlamanızı şiddetle öneririz. Kapalı kalma süresini en aza indirmek için, ağ trafiğinizi diğer bağlantıya geçirdikten sonra ExpressRoute Direct'in bir bağlantısındaki yapılandırmayı güncelleştirmenizi öneririz.  
### <a name="will-traffic-continue-to-flow-if-theres-a-mismatch-in-macsec-key-between-my-devices-and-microsofts"></a>Cihazlarım ve Microsoft'un cihazları arasında MACsec anahtarında bir uyumsuzluk varsa trafik akmaya devam eder mi?
Hayır. MACsec yapılandırılırsa ve önemli bir uyumsuzluk oluşursa, Microsoft'a bağlantı kaybedersiniz. Başka bir deyişle, verilerinizi açığa çıkararak şifrelenmemiş bir bağlantıya geri dönemeyiz. 
### <a name="will-enabling-macsec-on-expressroute-direct-degrade-network-performance"></a>ExpressRoute Direct'te MACsec'i etkinleştirmek ağ performansını düşürür mü?
MACsec şifreleme ve şifre çözme kullandığımız yönlendiriciler üzerinde donanım oluşur. Bizim tarafımızda hiçbir performans etkisi yok. Ancak, kullandığınız aygıtlar için ağ satıcısına danışmalısınız ve MACsec'in herhangi bir performans etkisi olup olmadığını görmeniz gerekir.
### <a name="which-cipher-suites-are-supported-for-encryption"></a>şifreleme için hangi şifreleme paketleri desteklenir?
AES128 ve AES256'yı destekliyoruz.

## <a name="end-to-end-encryption-by-ipsec-faq"></a>IPsec SSS ile uçuca şifreleme
IPsec bir [IETF standardıdır.](https://tools.ietf.org/html/rfc6071) Internet Protokolü (IP) düzeyindeveya Ağ Katmanı 3'teki verileri şifreler. IPsec'i, şirket içi ağınızla Azure'daki sanal ağınız (VNET) arasındaki uçuçbağlantıyı şifrelemek için kullanabilirsiniz. Aşağıdaki diğer SSS'lere bakın.
### <a name="can-i-enable-ipsec-in-addition-to-macsec-on-my-expressroute-direct-ports"></a>ExpressRoute Direct bağlantı noktalarımda MACsec'e ek olarak IPsec'i etkinleştirebilir miyim?
Evet. MACsec, sizinle Microsoft arasındaki fiziksel bağlantıları güvence altına alar. IPsec, Azure'daki sanal ağlarınızla aranızdaki uçlardan uca bağlantıyı güvence altına alar. Bunları bağımsız olarak etkinleştirebilirsiniz. 
### <a name="can-i-use-azure-vpn-gateway-to-set-up-the-ipsec-tunnel-between-my-on-premises-network-and-my-azure-virtual-network"></a>Şirket içi ağım ve Azure sanal ağım arasında IPsec tünelini kurmak için Azure VPN ağ geçidini kullanabilir miyim?
Evet. Bu IPsec tünelini ExpressRoute devrenizin Microsoft Peering'i üzerinden ayarlayabilirsiniz. Yapılandırma [kılavuzumuzu takip edin.](site-to-site-vpn-over-microsoft-peering.md)
### <a name="can-i-use-azure-vpn-gateway-to-set-up-the-ipsec-tunnel-over-azure-private-peering"></a>Azure Özel Eşleme üzerinde IPsec tünelini ayarlamak için Azure VPN ağ geçidini kullanabilir miyim?
Azure Virtual WAN'ı benimserseniz, uçlardan uca bağlantıyı şifrelemek için [aşağıdaki adımları](../virtual-wan/vpn-over-expressroute.md) izleyebilirsiniz. Normal Azure VNET'iniz varsa, VNET'inizde bir üçüncü taraf VPN ağ geçidi dağıtabilir ve bu ağ ile şirket içi VPN ağ geçidiniz arasında bir IPsec tüneli oluşturabilirsiniz.
### <a name="what-is-the-throughput-i-will-get-after-enabling-ipsec-on-my-expressroute-connection"></a>ExpressRoute bağlantımda IPsec'i etkinleştirdikten sonra elde edeceğim iş nedir?
Azure VPN ağ geçidi kullanılıyorsa, burada performans numaralarını kontrol [edin.](../vpn-gateway/vpn-gateway-about-vpngateways.md) Bir üçüncü taraf VPN ağ geçidi kullanılırsa, performans numaraları için satıcıya danışın.

## <a name="next-steps"></a>Sonraki adımlar
MACsec yapılandırması hakkında daha fazla bilgi için [MACsec'i Yapılandırın'](expressroute-howto-macsec.md) a bakın.

IPsec yapılandırması hakkında daha fazla bilgi için [IPsec'i Yapılandırın'](site-to-site-vpn-over-microsoft-peering.md) a bakın.
