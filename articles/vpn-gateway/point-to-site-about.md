---
title: Azure Noktadan Siteye VPN bağlantıları hakkında | VPN Ağ Geçidi
description: Bu makale, Sayfa-To-Site bağlantıları anlamanıza yardımcı olur ve hangi P2S VPN ağ geçidi kimlik doğrulama türünü kullanacağınıza karar vermenize yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: cherylmc
ms.openlocfilehash: 381aad5d0a56362d9966ed54b931a8478f2f6bf2
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410496"
---
# <a name="about-point-to-site-vpn"></a>Noktadan Siteye VPN Hakkında

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu çözüm, Azure sanal ağlarına uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak isteyen uzaktan çalışan kişiler için kullanışlıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda S2S VPN yerine kullanabileceğiniz yararlı bir çözümüdür. Bu tablo Resource Manager dağıtım modelleri için geçerlidir.

## <a name="what-protocol-does-p2s-use"></a><a name="protocol"></a>P2S hangi protokolü kullanır?

Noktadan siteye VPN aşağıdaki protokollerden birini kullanabilir:

* **OpenVPN® Protokolü**, SSL/TLS tabanlı VPN protokolü. Bir TLS VPN çözümü güvenlik duvarlarına nüfuz edebilir, çünkü çoğu güvenlik duvarı TCP bağlantı noktası 443 giden ilerlediği ve TLS'nin kullandığı açık olduğundan. OpenVPN, Android, iOS (11.0 ve üzeri sürümler), Windows, Linux ve Mac aygıtlarından (OSX sürümleri 10.13 ve üzeri) bağlanmak için kullanılabilir.

* Güvenli Soket Tünel Protokolü (SSTP), özel bir TLS tabanlı VPN protokolü. Bir TLS VPN çözümü güvenlik duvarlarına nüfuz edebilir, çünkü çoğu güvenlik duvarı TCP bağlantı noktası 443 giden ilerlediği ve TLS'nin kullandığı açık olduğundan. SSTP yalnızca Windows aygıtlarında desteklenir. Azure, Windows'un SSTP (Windows 7 ve sonrası) tüm sürümlerini destekler.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.


>[!NOTE]
>IkEv2 ve P2S için OpenVPN yalnızca Kaynak Yöneticisi dağıtım modeli için kullanılabilir. Bunlar klasik dağıtım modeli için kullanılamaz.
>

## <a name="how-are-p2s-vpn-clients-authenticated"></a><a name="authentication"></a>P2S VPN istemcileri nasıl doğrulanır?

Azure bir P2S VPN bağlantısını kabul etmeden önce, kullanıcının önce kimlik doğrulaması gerekir. Azure'un bağlantı kuran bir kullanıcının kimliğini doğrulamak için sunduğu iki mekanizma vardır.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Yerel Azure sertifika kimlik doğrulamasını kullanarak kimlik doğrulaması

Yerel Azure sertifikası kimlik doğrulaması kullanılırken, aygıtta bulunan bir istemci sertifikası, bağlantı kullanıcının kimliğini doğrulamak için kullanılır. İstemci sertifikaları güvenilir bir kök sertifikadan oluşturulur ve sonra her istemci bilgisayara yüklenir. Kurumsal çözüm kullanılarak oluşturulan bir kök sertifika kullanabilir veya kendi imzalı bir sertifika oluşturabilirsiniz.

İstemci sertifikasının doğrulanması VPN ağ geçidi tarafından gerçekleştirilir ve P2S VPN bağlantısının kurulması sırasında gerçekleşir. Doğrulama için kök sertifika gereklidir ve Azure'a yüklenmesi gerekir.

### <a name="authenticate-using-native-azure-active-directory-authentication"></a>Yerel Azure Etkin Dizin kimlik doğrulaması kullanarak kimlik doğrulaması

Azure AD kimlik doğrulaması, kullanıcıların Azure Etkin Dizin kimlik bilgilerini kullanarak Azure'a bağlanmalarını sağlar. Yerel Azure AD kimlik doğrulaması yalnızca OpenVPN protokolü ve Windows 10 için desteklenir ve [Azure VPN İstemci'sinin](https://go.microsoft.com/fwlink/?linkid=2117554)kullanılmasını gerektirir.

Yerel Azure AD kimlik doğrulaması ile Azure AD'nin koşullu erişiminin yanı sıra VPN için Çok Faktörlü Kimlik Doğrulama (MFA) özelliklerinden de yararlanabilirsiniz.

Yüksek düzeyde, Azure AD kimlik doğrulamasını yapılandırmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. [Azure AD kiracısını yapılandırma](openvpn-azure-ad-tenant.md)

2. [Ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirme](openvpn-azure-ad-tenant.md#enable-authentication)

3. [Azure VPN İstemcisini İndirin ve yapılandırın](https://go.microsoft.com/fwlink/?linkid=2117554)


### <a name="authenticate-using-active-directory-ad-domain-server"></a>Active Directory (AD) Etki Alanı Sunucusu kullanarak kimlik doğrulaması

AD Etki Alanı kimlik doğrulaması, kullanıcıların kuruluş etki alanı kimlik bilgilerini kullanarak Azure'a bağlanmalarını sağlar. AD sunucusuyla tümleşen bir RADIUS sunucusu gerektirir. Kuruluşlar, mevcut RADIUS dağıtımından da yararlanabilirler.
  
RADIUS sunucusu şirket içinde veya Azure VNet'inizde dağıtılabilir. Kimlik doğrulama sırasında Azure VPN Ağ Geçidi, bir geçiş görevi görür ve kimlik doğrulama iletilerini RADIUS sunucusu ile bağlantı aygıtı arasında ileri geri iletir. Bu nedenle RADIUS sunucusuna ağ geçidi nin ulaşılabilirliği önemlidir. RADIUS sunucusu şirket içinde mevcutsa, ulaşılabilirlik için Azure'dan şirket içi siteye VPN S2S bağlantısı gerekir.  
  
RADIUS sunucusu, AD sertifikası hizmetleriyle de tümleştirebilirsiniz. Bu, Azure sertifika kimlik doğrulamasına alternatif olarak RADIUS sunucusunu ve P2S sertifika kimlik doğrulaması için kurumsal sertifika dağıtımınızı kullanmanıza olanak tanır. Bunun avantajı, kök sertifikaları ve iptal edilmiş sertifikaları Azure'a yüklemeniz gerekmese de.

RADIUS sunucusu diğer dış kimlik sistemleriyle de tümleşebilir. Bu, Çok faktörlü seçenekler de dahil olmak üzere P2S VPN için birçok kimlik doğrulama seçeneği sunar.

![noktadan siteye](./media/point-to-site-about/p2s.png "Noktadan Siteye")

## <a name="what-are-the-client-configuration-requirements"></a>İstemci yapılandırma gereksinimleri nelerdir?

>[!NOTE]
>Windows istemcileri için, istemci aygıtından Azure'a VPN bağlantısını başlatmak için istemci aygıtında yönetici haklarına sahip olmalısınız.
>

Kullanıcılar P2S için Windows ve Mac aygıtlarında yerel VPN istemcilerini kullanır. Azure, bu yerel istemcilerin Azure'a bağlanmak için gerekli ayarlarını içeren bir VPN istemcisi yapılandırma zip dosyası sağlar.

* Windows aygıtları için VPN istemcisi yapılandırması, kullanıcıların cihazlarına yükledikleri bir yükleyici paketinden oluşur.
* Mac aygıtları için, kullanıcıların cihazlarına yükledikleri mobileconfig dosyasından oluşur.

Zip dosyası ayrıca, Azure tarafındaki bazı önemli ayarların değerlerini bu aygıtlar için kendi profilinizi oluşturmak için kullanabilirsiniz. Değerlerden bazıları VPN ağ geçidi adresi, yapılandırılmış tünel türleri, rotalar ve ağ geçidi doğrulaması için kök sertifikaiçerir.

>[!NOTE]
>[!INCLUDE [TLS version changes](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="which-gateway-skus-support-p2s-vpn"></a><a name="gwsku"></a>Hangi ağ geçidi SUS desteği P2S VPN?

[!INCLUDE [aggregate throughput sku](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

* Ağ Geçidi SKU önerileri için [VPN Ağ Geçidi ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku)bkz.

>[!NOTE]
>Temel SKU, IKEv2 veya RADIUS kimlik doğrulamasını desteklemez.
>

## <a name="what-ikeipsec-policies-are-configured-on-vpn-gateways-for-p2s"></a><a name="IKE/IPsec policies"></a>Hangi IKE/IPsec ilkeleri P2S için VPN ağ geçitlerinde yapılandırılır?


**IKEv2**

|**Şifre** | **Bütünlük** | **Prf** | **DH Grubu** |
|---        | ---           | ---       | ---   |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_24 |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_14 |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_ECP384 |
|GCM_AES256 |   GCM_AES256  | SHA384    | GROUP_ECP256 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_24 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_14 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_ECP384 |
|GCM_AES256 |   GCM_AES256  | SHA256    | GROUP_ECP256 |
|AES256     |   SHA384      | SHA384    | GROUP_24 |
|AES256     |   SHA384      | SHA384    | GROUP_14 |
|AES256     |   SHA384      | SHA384    | GROUP_ECP384 |
|AES256     |   SHA384      | SHA384    | GROUP_ECP256 |
|AES256     |   SHA256      | SHA256    | GROUP_24 |
|AES256     |   SHA256      | SHA256    | GROUP_14 |
|AES256     |   SHA256      | SHA256    | GROUP_ECP384 |
|AES256     |   SHA256      | SHA256    | GROUP_ECP256 |
|AES256     |   SHA256      | SHA256    | GROUP_2 |

**IPsec**

|**Şifre** | **Bütünlük** | **PFS Grubu** |
|---        | ---           | ---       |
|GCM_AES256 | GCM_AES256 | GROUP_NONE |
|GCM_AES256 | GCM_AES256 | GROUP_24 |
|GCM_AES256 | GCM_AES256 | GROUP_14 |
|GCM_AES256 | GCM_AES256 | GROUP_ECP384 |
|GCM_AES256 | GCM_AES256 | GROUP_ECP256 |
| AES256    | SHA256 | GROUP_NONE |
| AES256    | SHA256 | GROUP_24 |
| AES256    | SHA256 | GROUP_14 |
| AES256    | SHA256 | GROUP_ECP384 |
| AES256    | SHA256 | GROUP_ECP256 |
| AES256    | SHA1 | GROUP_NONE |

## <a name="what-tls-policies-are-configured-on-vpn-gateways-for-p2s"></a><a name="TLS policies"></a>P2S için VPN ağ geçitlerinde hangi TLS ilkeleri yapılandırılır?
**Tls**

|**İlkeler** |
|---| 
|TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 |
|TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 |
|TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 |
|TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 |
|TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 |
|TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 |
|TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 |
|TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 |
|TLS_RSA_WITH_AES_128_GCM_SHA256 |
|TLS_RSA_WITH_AES_256_GCM_SHA384 |
|TLS_RSA_WITH_AES_128_CBC_SHA256 |
|TLS_RSA_WITH_AES_256_CBC_SHA256 |

## <a name="how-do-i-configure-a-p2s-connection"></a><a name="configure"></a>P2S bağlantısını nasıl yapılandırıyorum?

Bir P2S yapılandırması oldukça birkaç özel adım gerektirir. Aşağıdaki makaleler, P2S yapılandırması ile size yol için adımlar ve VPN istemci aygıtları yapılandırmak için bağlantılar içerir:

* [Bir P2S bağlantısını yapılandırma - RADIUS kimlik doğrulaması](point-to-site-how-to-radius-ps.md)

* [Bir P2S bağlantısını yapılandırma - Azure yerel sertifika kimlik doğrulaması](vpn-gateway-howto-point-to-site-rm-ps.md)

* [OpenVPN yapılandırması](vpn-gateway-howto-openvpn.md)

### <a name="to-remove-the-configuration-of-a-p2s-connection"></a>P2S bağlantısının yapılandırmasını kaldırmak için

Adımlar için aşağıdaki [SSS](#removeconfig)bölümüne bakın.
 
## <a name="faq-for-native-azure-certificate-authentication"></a><a name="faqcert"></a>Yerel Azure sertifika kimlik doğrulaması için SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faq-for-radius-authentication"></a><a name="faqradius"></a>RADIUS kimlik doğrulaması için SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar

* [Bir P2S bağlantısını yapılandırma - RADIUS kimlik doğrulaması](point-to-site-how-to-radius-ps.md)

* [Bir P2S bağlantısını yapılandırma - Azure yerel sertifika kimlik doğrulaması](vpn-gateway-howto-point-to-site-rm-ps.md)

**"OpenVPN", OpenVPN Inc. şirketinin ticari markasıdır.**
