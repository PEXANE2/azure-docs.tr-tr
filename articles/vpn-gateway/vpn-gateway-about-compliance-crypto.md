---
title: 'Azure VPN Gateway: Şifreleme gereksinimleri'
description: Bu makalede, şifreleme gereksinimleri ve Azure VPN ağ geçitleri açıklanmaktadır
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/10/2020
ms.author: yushwang
ms.openlocfilehash: 93e13592d9c434b159ad4f4c10ef30328941c64e
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75902833"
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında

Bu makalede, Azure VPN ağ geçitlerini, hem şirket içi S2S VPN tünellerini hem de Azure 'daki VNet-VNet bağlantıları için şifreleme gereksinimlerinizi karşılayacak şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="about-ikev1-and-ikev2-for-azure-vpn-connections"></a>Azure VPN bağlantıları için IKEv1 ve IKEv2 hakkında

Geleneksel olarak yalnızca temel SKU 'Lar için IKEv1 bağlantılara ve temel SKU 'Lardan başka tüm VPN Gateway SKU 'Larına izin verilen Ikev2 bağlantılarına izin verdik. Temel SKU 'Lar yalnızca 1 bağlantıya ve performans gibi diğer kısıtlamalarla birlikte yalnızca IKEv1 protokollerini destekleyen eski cihazları kullanan müşteriler sınırlı deneyimle izin veriyor. IKEv1 protokollerini kullanan müşterilerin deneyimini geliştirmek için artık temel SKU dışında tüm VPN Gateway SKU 'Larına yönelik IKEv1 bağlantılara izin veririz. Daha fazla bilgi için bkz. [VPN Gateway SKU 'lar](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku).

![Azure VPN Gateway IKEv1 ve IKEv2 bağlantıları](./media/vpn-gateway-about-compliance-crypto/ikev1-ikev2-connections.png)

IKEv1 ve IKEv2 bağlantıları aynı VPN ağ geçidine uygulandığında, bu iki bağlantı arasındaki aktarım otomatik olarak etkinleştirilir.

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>Azure VPN ağ geçitleri için IPSec ve ıKE ilke parametreleri hakkında

IPSec ve ıKE protokol standardı çeşitli birleşimlerde çok sayıda şifreleme algoritmasını destekler. Şifreleme algoritmalarının ve parametrelerinin belirli bir birleşimini istemeyin, Azure VPN ağ geçitleri varsayılan bir teklif kümesi kullanır. Varsayılan ilke kümeleri, varsayılan yapılandırmalarda çok çeşitli üçüncü taraf VPN cihazlarıyla birlikte çalışabilirliği en üst düzeye çıkarmak üzere seçilmiştir. Sonuç olarak, ilkeler ve tekliflerin sayısı kullanılabilir şifreleme algoritmaları ve anahtar güçlerinin tüm olası birleşimlerini kapsayamaz.

Azure VPN Gateway için ayarlanan varsayılan ilke, [siteden siteye VPN Gateway bağlantıları IÇIN VPN cihazları ve IPSec/IKE parametreleri hakkında](vpn-gateway-about-vpn-devices.md)makalesinde listelenmiştir.

## <a name="cryptographic-requirements"></a>Şifreleme gereksinimleri

Genellikle uyumluluk veya güvenlik gereksinimleri nedeniyle belirli şifreleme algoritmaları veya parametreler gerektiren iletişimler için, Azure VPN ağ geçitlerini, Azure varsayılan ilke kümeleri yerine belirli şifreleme algoritmalarına ve anahtar güçlerine sahip özel bir IPSec/ıKE ilkesi kullanacak şekilde yapılandırabilirsiniz.

Örneğin, Azure VPN ağ geçitlerinin Ikev2 ana mod ilkeleri yalnızca Diffie-Hellman grubu 2 ' yi (1024 bit) kullanır, ancak örneğin 14 (2048-bit), Grup 24 (2048-bit MODP grubu) veya ECP (Eliptik Eğri) gibi ıKE 'de kullanılacak daha güçlü gruplar belirtmeniz gerekebilir. gruplar) 256 veya 384 bit (sırasıyla grup 19 ve grup 20). Benzer gereksinimler IPSec hızlı mod ilkeleri için de geçerlidir.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Azure VPN ağ geçitleri ile özel IPSec/ıKE ilkesi

Azure VPN ağ geçitleri artık bağlantı başına özel IPSec/ıKE ilkesini desteklemektedir. Siteden siteye veya VNet 'ten VNet 'e bağlantı için, aşağıdaki örnekte gösterildiği gibi, IPSec ve ıKE için istenen anahtar gücüyle belirli bir şifreleme algoritmaları birleşimini seçebilirsiniz:

![IPSec-IKE-Policy](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

Bir IPSec/ıKE ilkesi oluşturabilir ve yeni veya mevcut bir bağlantıya uygulayabilirsiniz.

### <a name="workflow"></a>İş Akışı

1. Diğer nasıl yapılır belgelerinde açıklandığı gibi, bağlantı topolojiniz için sanal ağlar, VPN ağ geçitleri veya yerel ağ geçitleri oluşturun
2. IPSec/ıKE ilkesi oluşturma
3. Bir S2S veya VNet-VNet bağlantısı oluştururken ilkeyi uygulayabilirsiniz
4. Bağlantı zaten oluşturulduysa, var olan bir bağlantıya ilke uygulayabilir veya ilkeyi güncelleştirebilirsiniz

## <a name="ipsecike-policy-faq"></a>IPSec/ıKE ilkesi hakkında SSS

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-faq-ipsecikepolicy-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir bağlantıda özel IPSec/ıKE ilkesi yapılandırma hakkında adım adım yönergeler için bkz. [IPSec/IKE Ilkesini yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md) .

Ayrıca, UsePolicyBasedTrafficSelectors seçeneği hakkında daha fazla bilgi edinmek için bkz. [birden çok ilke tabanlı VPN cihazını bağlama](vpn-gateway-connect-multiple-policybased-rm-ps.md) .
