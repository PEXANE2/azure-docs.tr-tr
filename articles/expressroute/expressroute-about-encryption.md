---
title: 'Azure ExpressRoute: şifreleme hakkında'
description: ExpressRoute şifrelemesi hakkında bilgi edinin.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/12/2020
ms.author: duau
ms.openlocfilehash: 83fa16265106e1033cb77ab4175b606714ec66d7
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91996840"
---
# <a name="expressroute-encryption"></a>ExpressRoute şifreleme
 
ExpressRoute, ağınız ile Microsoft ağı arasında geçiş yapılan verilerin gizliliğini ve bütünlüğünü sağlamak için birkaç şifreleme teknolojisini destekler.

## <a name="point-to-point-encryption-by-macsec-faq"></a>MACsec 'e göre noktadan noktaya şifreleme SSS
MACsec, bir [IEEE standardıdır](https://1.ieee802.org/security/802-1ae/). Verileri medya erişim denetimi (MAC) düzeyinde veya ağ katmanı 2 ' de şifreler. [ExpressRoute Direct](expressroute-erdirect-about.md)aracılığıyla Microsoft 'a bağlandığınızda ağ cihazlarınızla Microsoft 'un ağ cihazları arasındaki fiziksel bağlantıları şifrelemek Için MACsec kullanabilirsiniz. MACsec, ExpressRoute doğrudan bağlantı noktalarında varsayılan olarak devre dışıdır. Şifreleme için kendi MACsec anahtarınızı getirip [Azure Key Vault](../key-vault/general/overview.md)' de saklayın. Anahtarın ne zaman döndürüleceğini karar verirsiniz. Aşağıdaki diğer SSS bölümüne bakın.
### <a name="can-i-enable-macsec-on-my-expressroute-circuit-provisioned-by-an-expressroute-provider"></a>ExpressRoute sağlayıcısı tarafından sağlanan ExpressRoute bağlantı hattı üzerinde MACsec 'i etkinleştirebilir miyim?
Hayır. MACsec bir fiziksel bağlantıdaki tüm trafiği bir varlığa ait bir anahtarla şifreler (örn. müşteri). Bu nedenle, yalnızca ExpressRoute doğrudan kullanılabilir.
### <a name="can-i-encrypt-some-of-the-expressroute-circuits-on-my-expressroute-direct-ports-and-leave-other-circuits-on-the-same-ports-unencrypted"></a>ExpressRoute doğrudan bağlantı noktalarında ExpressRoute devrelerinin bazılarını şifreleyebilir ve aynı bağlantı noktalarında diğer devreleri şifrelenmemiş olarak bırakabilir miyim? 
Hayır. MACsec 'in tüm ağ denetim trafiği etkinleştirildikten sonra, örneğin BGP veri trafiği ve müşteri veri trafiği şifrelenir. 
### <a name="when-i-enabledisable-macsec-or-update-macsec-key-will-my-on-premises-network-lose-connectivity-to-microsoft-over-expressroute"></a>MACsec 'i etkinleştirdiğimde/devre dışı bırakadım veya MACsec anahtarını güncelleştirme, şirket içi ağı ExpressRoute üzerinden Microsoft 'a bağlantı kaybetmem gerekir mi?
Evet. MACsec yapılandırması için, yalnızca önceden paylaşılan anahtar modunu destekliyoruz. Bu, hem cihazlarınızda hem de Microsoft 'un (API 'imiz aracılığıyla) anahtarı güncelleştirmeniz gerektiği anlamına gelir. Bu değişiklik atomik değildir, bu nedenle iki kenarı arasında bir anahtar uyumsuzluğu olduğunda bağlantıyı kaybedersiniz. Yapılandırma değişikliği için bir bakım penceresi zamanlamanızı önemle tavsiye ederiz. Kapalı kalma süresini en aza indirmek için, ağ trafiğinizi başka bir bağlantıya değiştirdikten sonra tek seferde ExpressRoute Direct 'in bir bağlantısındaki yapılandırmayı güncelleştirmenizi öneririz.  
### <a name="will-traffic-continue-to-flow-if-theres-a-mismatch-in-macsec-key-between-my-devices-and-microsofts"></a>Akışlarım ile Microsoft 'un arasında MACsec anahtarında uyuşmazlık olursa trafik akışa devam eder mi?
Hayır. MACsec yapılandırıldıysa ve bir anahtar uyumsuzluğu oluşursa, Microsoft bağlantısını kaybedersiniz. Diğer bir deyişle, şifrelenmemiş bir bağlantıya geri dönemeyecek ve verilerinizi kullanıma sunmayacağız. 
### <a name="will-enabling-macsec-on-expressroute-direct-degrade-network-performance"></a>ExpressRoute Direct üzerinde MACsec 'i etkinleştirmek, ağ performansının düşmesine neden olacak mı?
MACsec şifreleme ve şifre çözme, kullandığımız yönlendiricilerin donanımında oluşur. Bizim tarafımızda performans etkisi yoktur. Ancak, kullandığınız cihazların ağ satıcısına danışmanız ve MACsec 'in herhangi bir performansa sahip olup olmadığını görmeniz gerekir.
### <a name="which-cipher-suites-are-supported-for-encryption"></a>Şifreleme için hangi şifre paketleri destekleniyor?
Yalnızca AES128 ve AES256 [genişletilmiş paket numaralandırma](https://1.ieee802.org/security/802-1aebw/) sürümünü destekliyoruz. Ayrıca, lütfen cihazınızdaki MACsec yapılandırmasındaki [güvenli kanal tanımlayıcısını (SCı)](https://en.wikipedia.org/wiki/IEEE_802.1AE) devre dışı bırakın. 

## <a name="end-to-end-encryption-by-ipsec-faq"></a>IPSec ile uçtan uca şifreleme hakkında SSS
IPSec bir [IETF standardıdır](https://tools.ietf.org/html/rfc6071). Internet Protokolü (IP) düzeyinde veya ağ katmanı 3 ' teki verileri şifreler. Azure 'da şirket içi ağınız ile sanal ağınız (VNET) arasında uçtan uca bir bağlantıyı şifrelemek için IPSec kullanabilirsiniz. Aşağıdaki diğer SSS bölümüne bakın.
### <a name="can-i-enable-ipsec-in-addition-to-macsec-on-my-expressroute-direct-ports"></a>ExpressRoute doğrudan bağlantı noktalarında MACsec 'e ek olarak IPSec 'i etkinleştirebilir miyim?
Evet. MACsec, siz ve Microsoft arasındaki fiziksel bağlantıları korur. IPSec, sizin ve sanal ağlarınızla Azure 'daki uçtan uca bağlantının güvenliğini sağlar. Bunları bağımsız olarak etkinleştirebilirsiniz. 
### <a name="can-i-use-azure-vpn-gateway-to-set-up-the-ipsec-tunnel-over-azure-private-peering"></a>Azure özel eşleme üzerinden IPSec tüneli ayarlamak için Azure VPN ağ geçidini kullanabilir miyim?
Evet. Azure sanal WAN 'ı benimsediğinizde, uçtan uca bağlantıyı şifrelemek için [aşağıdaki adımları](../virtual-wan/vpn-over-expressroute.md) izleyebilirsiniz. Normal Azure VNET 'iniz varsa, Azure VPN ağ geçidi ve şirket içi VPN ağ geçidiniz arasında bir IPSec tüneli oluşturmak için [Bu adımları](../vpn-gateway/site-to-site-vpn-private-peering.md) izleyebilirsiniz.
### <a name="what-is-the-throughput-i-will-get-after-enabling-ipsec-on-my-expressroute-connection"></a>ExpressRoute bağlantınızda IPSec 'i etkinleştirdikten sonra alacağınız aktarım hızı nedir?
Azure VPN Gateway kullanılıyorsa, [burada performans numaralarını](../vpn-gateway/vpn-gateway-about-vpngateways.md)kontrol edin. Üçüncü taraf bir VPN ağ geçidi kullanılıyorsa, performans numaraları için satıcıya başvurun.

## <a name="next-steps"></a>Sonraki adımlar
MACsec yapılandırması hakkında daha fazla bilgi için bkz. [MACsec](expressroute-howto-macsec.md) yapılandırma.

IPSec yapılandırması hakkında daha fazla bilgi için bkz. [IPSec yapılandırma](site-to-site-vpn-over-microsoft-peering.md) .
