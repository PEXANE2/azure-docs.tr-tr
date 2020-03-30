---
title: 'Azure VPN Ağ Geçidi: Şifreleme gereksinimleri'
description: Bu makalede, şifreleme gereksinimleri ve Azure VPN ağ geçitleri açıklanmaktadır
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/10/2020
ms.author: yushwang
ms.openlocfilehash: 93e13592d9c434b159ad4f4c10ef30328941c64e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75902833"
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında

Bu makalede, Azure'daki hem tesisler arası S2S VPN tünelleri hem de Azure içindeki VNet-to-VNet bağlantıları için şifreleme gereksinimlerinizi karşılamak için Azure VPN ağ geçitlerini nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="about-ikev1-and-ikev2-for-azure-vpn-connections"></a>Azure VPN bağlantıları için IKEv1 ve IKEv2 hakkında

Geleneksel olarak sadece Temel SUK'lar için IKEv1 bağlantılarına izin verdik ve Temel SUK'lar dışındaki tüm VPN ağ geçidi SUK'ları için IKEv2 bağlantılarına izin verdik. Temel SUK'lar yalnızca 1 bağlantısağlar ve performans gibi diğer sınırlamalarla birlikte, yalnızca IKEv1 protokollerini destekleyen eski aygıtları kullanan müşteriler sınırlı deneyime sahiptir. IKEv1 protokollerini kullanan müşterilerin deneyimini geliştirmek için, temel SKU hariç tüm VPN ağ geçidi SKU'ları için IKEv1 bağlantılarına izin vermekteyiz. Daha fazla bilgi için [VPN Ağ Geçidi SBİZ'larına](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku)bakın.

![Azure VPN Ağ Geçidi IKEv1 ve IKEv2 bağlantıları](./media/vpn-gateway-about-compliance-crypto/ikev1-ikev2-connections.png)

IKEv1 ve IKEv2 bağlantıları aynı VPN ağ geçidine uygulandığında, bu iki bağlantı arasındaki geçiş otomatik olarak etkinleştirilir.

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>Azure VPN ağ geçitleri için IPsec ve IKE ilke parametreleri hakkında

IPsec ve IKE protokol standardı çeşitli kombinasyonlarda çok çeşitli şifreleme algoritmalarını destekler. Belirli bir şifreleme algoritması ve parametre kombinasyonu istemiyorsanız, Azure VPN ağ geçitleri varsayılan öneriler kümesi kullanır. Varsayılan ilke kümeleri, varsayılan yapılandırmalarda çok çeşitli üçüncü taraf VPN aygıtlarıyla birlikte çalışabilirliği en üst düzeye çıkarmak için seçildi. Sonuç olarak, ilkeler ve teklif sayısı, kullanılabilir şifreleme algoritmalarının ve önemli güçlerin olası tüm birleşimlerini kapsayamaz.

Azure VPN ağ geçidi için varsayılan ilke kümesi makalede listelenmiştir: [Siteden Siteye VPN Ağ Geçidi bağlantıları için VPN aygıtları ve IPsec/IKE parametreleri hakkında.](vpn-gateway-about-vpn-devices.md)

## <a name="cryptographic-requirements"></a>Şifreleme gereksinimleri

Belirli şifreleme algoritmaları veya parametreleri gerektiren iletişimlerde, genellikle uyumluluk veya güvenlik gereksinimleri nedeniyle, Azure VPN ağ geçitlerini Azure varsayılan ilke kümeleri yerine belirli şifreleme algoritmaları ve önemli güçlü yönlerle özel bir IPsec/IKE ilkesi kullanacak şekilde yapılandırabilirsiniz.

Örneğin, Azure VPN ağ geçitleri için IKEv2 ana mod ilkeleri yalnızca Diffie-Hellman Group 2 (1024 bit) kullanırken, Grup 14 (2048 bit), Grup 24 (2048 bit MODP Grubu) veya ECP (eliptik eğri) gibi IKE'de kullanılacak daha güçlü gruplar belirtmeniz gerekebilir gruplar) 256 veya 384 bit (Sırasıyla Grup 19 ve Grup 20). Benzer gereksinimler IPsec hızlı mod ilkeleri için de geçerlidir.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Azure VPN ağ geçitleri ile özel IPsec/IKE ilkesi

Azure VPN ağ geçitleri artık bağlantı başına, özel IPsec/IKE ilkesini destekler. Siteden Siteye veya VNet'ten VNet'e bağlantı için, aşağıdaki örnekte gösterildiği gibi, IPsec ve IKE için istenen anahtar gücüyle belirli bir şifreleme algoritması kombinasyonunu seçebilirsiniz:

![ipsec-ike-politikası](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

Bir IPsec/IKE ilkesi oluşturabilir ve yeni veya varolan bir bağlantıya başvurabilirsiniz.

### <a name="workflow"></a>İş akışı

1. Diğer nasıl yapılan belgelerde açıklandığı gibi bağlantı topolojiniz için sanal ağları, VPN ağ geçitlerini veya yerel ağ ağ geçitlerini oluşturun
2. Bir IPsec/IKE ilkesi oluşturma
3. Bir S2S veya VNet-to-VNet bağlantısı oluşturduğunuzda ilkeyi uygulayabilirsiniz
4. Bağlantı zaten oluşturulmuşsa, ilkeyi varolan bir bağlantıya uygulayabilir veya güncelleştirebilirsiniz

## <a name="ipsecike-policy-faq"></a>IPsec/IKE ilkesi SSS

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-faq-ipsecikepolicy-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantıda özel IPsec/IKE ilkesini yapılandırmakonusunda adım adım yönergeler için [IPsec/IKE ilkesini yapılandırın.](vpn-gateway-ipsecikepolicy-rm-powershell.md)

[Ayrıca](vpn-gateway-connect-multiple-policybased-rm-ps.md) bkz.
