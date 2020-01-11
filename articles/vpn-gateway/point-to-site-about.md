---
title: Azure Noktadan siteye VPN bağlantıları hakkında | Microsoft Docs
description: Bu makale, Noktadan siteye bağlantıları anlamanıza yardımcı olur ve hangi P2S VPN Gateway kimlik doğrulama türünün kullanılması gerektiğine karar vermenize yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: cherylmc
ms.openlocfilehash: 6084eca4f7d2d6040eb3f32a9b28cfc50922d200
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75896093"
---
# <a name="about-point-to-site-vpn"></a>Noktadan siteye VPN hakkında

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu çözüm, Azure sanal ağlarına uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak isteyen uzaktan çalışan kişiler için kullanışlıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda S2S VPN yerine kullanabileceğiniz yararlı bir çözümüdür. Bu tablo Resource Manager dağıtım modelleri için geçerlidir.

## <a name="protocol"></a>P2S hangi protokol kullanır?

Noktadan siteye VPN aşağıdaki protokollerden birini kullanabilir:

* Bir SSL/TLS tabanlı VPN protokolü olan **OpenVPN® Protokolü**. Çoğu güvenlik duvarı, SSL tarafından kullanılan TCP bağlantı noktası 443 giden ' ü açık olduğundan, bir SSL VPN çözümü, güvenlik duvarlarını sızma edebilir. OpenVPN, Android, iOS (sürüm 11,0 ve üzeri), Windows, Linux ve Mac cihazları (OSX sürümleri 10,13 ve üzeri) ile bağlantı kurmak için kullanılabilir.

* Özel bir SSL tabanlı VPN protokolü olan Güvenli Yuva Tünel Protokolü (SSTP). Çoğu güvenlik duvarı, SSL tarafından kullanılan TCP bağlantı noktası 443 giden ' ü açık olduğundan, bir SSL VPN çözümü, güvenlik duvarlarını sızma edebilir. SSTP yalnızca Windows cihazlarında desteklenir. Azure, SSTP (Windows 7 ve üzeri) olan tüm Windows sürümlerini destekler.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.


>[!NOTE]
>P2S için Ikev2 ve OpenVPN yalnızca Kaynak Yöneticisi dağıtım modelinde kullanılabilir. Klasik dağıtım modeli için kullanılamaz.
>

## <a name="authentication"></a>P2S VPN istemcileri nasıl doğrulanır?

Azure, bir P2S VPN bağlantısı kabul etmeden önce, önce kullanıcının kimliği doğrulanmalıdır. Azure 'un bağlanan bir kullanıcının kimliğini doğrulamak için sunduğu iki mekanizma vardır.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Yerel Azure sertifika kimlik doğrulaması kullanarak kimlik doğrulama

Yerel Azure sertifika kimlik doğrulamasını kullanırken, bağlanan kullanıcının kimliğini doğrulamak için cihazda bulunan bir istemci sertifikası kullanılır. İstemci sertifikaları, güvenilir bir kök sertifikadan oluşturulur ve sonra her bir istemci bilgisayara yüklenir. Kurumsal çözüm kullanılarak oluşturulmuş bir kök sertifika kullanabilir veya otomatik olarak imzalanan bir sertifika oluşturabilirsiniz.

İstemci sertifikasının doğrulanması VPN Gateway tarafından gerçekleştirilir ve P2S VPN bağlantısının kurulması sırasında gerçekleşir. Kök sertifika, doğrulama için gereklidir ve Azure 'a yüklenmelidir.

### <a name="authenticate-using-native-azure-active-directory-authentication"></a>Yerel Azure Active Directory kimlik doğrulaması kullanarak kimlik doğrulama

Azure AD kimlik doğrulaması, kullanıcıların Azure Active Directory kimlik bilgilerini kullanarak Azure 'a bağlanmasına olanak sağlar. Yerel Azure AD kimlik doğrulaması yalnızca OpenVPN Protokolü ve Windows 10 için desteklenir ve [Azure VPN istemcisinin (Önizleme)](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab)kullanılmasını gerektirir.

Yerel Azure AD kimlik doğrulamasıyla, VPN için Multi-Factor Authentication (MFA) özelliklerinin yanı sıra Azure AD 'nin koşullu erişiminden de yararlanabilirsiniz.

Yüksek düzeyde, Azure AD kimlik doğrulamasını yapılandırmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. [Azure AD kiracısı yapılandırma](openvpn-azure-ad-tenant.md)

2. [Ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirme](https://docs.microsoft.com/azure/vpn-gateway/openvpn-azure-ad-tenant#enable-authentication)

3. [Azure VPN Istemcisini indirme ve yapılandırma (Önizleme)](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab)


### <a name="authenticate-using-active-directory-ad-domain-server"></a>Active Directory (AD) etki alanı sunucusu kullanarak kimlik doğrulama

AD etki alanı kimlik doğrulaması, kullanıcıların kuruluş etki alanı kimlik bilgilerini kullanarak Azure 'a bağlanmasına olanak sağlar. AD sunucusu ile tümleşen bir RADIUS sunucusu gerektirir. Kuruluşlar, var olan RADIUS dağıtımlarından da faydalanabilir.   
  
RADIUS sunucusu şirket içinde veya Azure VNET 'te dağıtılabilir. Kimlik doğrulaması sırasında Azure VPN Gateway, kimlik doğrulama iletilerini RADIUS sunucusu ile bağlanan cihaz arasında ileri ve geri iletir. Bu nedenle, RADIUS sunucusuna ağ geçidi erişilebilirliği önemli. RADIUS sunucusu şirket içinde mevcutsa, ulaşılabilirlik için Azure 'dan şirket içi siteye bir VPN S2S bağlantısı gerekir.  
  
RADIUS sunucusu, AD Sertifika Hizmetleri ile de tümleştirilebilir. Bu, Azure sertifika kimlik doğrulamasına alternatif olarak, RADIUS sunucusunu ve P2S sertifika kimlik doğrulaması için kuruluş sertifika dağıtımınızı kullanmanıza olanak sağlar. Bunun avantajı, kök sertifikaları ve iptal edilen sertifikaları Azure 'a yüklemeniz gerekmez.

Bir RADIUS sunucusu diğer dış kimlik sistemleriyle de tümleştirilebilir. Bu, Multi-Factor seçenekleri de dahil olmak üzere P2S VPN için birçok kimlik doğrulama seçeneği açar.

![Noktadan siteye](./media/point-to-site-about/p2s.png "Uçtan Siteye")

## <a name="what-are-the-client-configuration-requirements"></a>İstemci yapılandırma gereksinimleri nelerdir?

>[!NOTE]
>Windows istemcileri için istemci cihazından Azure 'a VPN bağlantısını başlatmak üzere istemci cihazda yönetici haklarına sahip olmanız gerekir.
>

Kullanıcılar, P2S için Windows ve Mac cihazlarda yerel VPN istemcilerini kullanır. Azure, bu yerel istemciler tarafından Azure 'a bağlanmak için gereken ayarları içeren bir VPN istemcisi yapılandırma zip dosyası sağlar.

* Windows cihazlarında, VPN istemci yapılandırması kullanıcıların cihazlarına yükleyen bir yükleyici paketinden oluşur.
* Mac cihazlar için, kullanıcıların cihazlarına yükleyen mobileconfig dosyasından oluşur.

ZIP dosyası Ayrıca, Azure tarafında bu cihazların kendi profilinizi oluşturmak için kullanabileceğiniz bazı önemli ayarların değerlerini de sağlar. Bazı değerler arasında VPN ağ geçidi adresi, yapılandırılmış tünel türleri, rotalar ve ağ geçidi doğrulaması için kök sertifika bulunur.

>[!NOTE]
>[!INCLUDE [TLS version changes](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="gwsku"></a>Hangi ağ geçidi SKU 'Ları P2S VPN 'yi destekliyor?

[!INCLUDE [aggregate throughput sku](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

* Ağ Geçidi SKU önerileri için bkz. [VPN Gateway ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

>[!NOTE]
>Temel SKU, IKEv2 veya RADIUS kimlik doğrulamasını desteklemez.
>

## <a name="IKE/IPsec policies"></a>P2S için VPN ağ geçitlerinde hangi ıKE/IPSec ilkeleri yapılandırılır?


**IKEv2**

|**Şifre** | **Doğruluğunu** | **PRF** | **DH grubu** |
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

**Bkz**

|**Şifre** | **Doğruluğunu** | **PFS Grubu** |
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

## <a name="TLS policies"></a>P2S için VPN ağ geçitlerinde hangi TLS ilkeleri yapılandırıldı?
**IOCTL**

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




## <a name="configure"></a>P2S bağlantısı Nasıl yaparım? mı?

Bir P2S yapılandırması, çok sayıda belirli adım gerektirir. Aşağıdaki makalelerde P2S yapılandırmasına kılavuzluk eden adımlar ve VPN istemci cihazlarını yapılandırma bağlantıları bulunur:

* [P2S bağlantısı yapılandırma-RADIUS kimlik doğrulaması](point-to-site-how-to-radius-ps.md)

* [P2S Bağlantısı Yapılandırma-Azure yerel sertifikası kimlik doğrulaması](vpn-gateway-howto-point-to-site-rm-ps.md)

* [OpenVPN 'i yapılandırma](vpn-gateway-howto-openvpn.md)

## <a name="how-do-i-remove-the-configuration-of-a-p2s-connection"></a>P2S bağlantısının yapılandırmasını kaldırmak Nasıl yaparım??

Az CLI ve aşağıdaki komut kullanılarak bir P2S yapılandırması kaldırılabilir: 

`az network vnet-gateway update --name <gateway-name> --resource-group <resource-group name> --remove "vpnClientConfiguration"`
 
## <a name="faqcert"></a>Yerel Azure sertifika kimlik doğrulaması hakkında SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>RADIUS kimlik doğrulaması için SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar

* [P2S bağlantısı yapılandırma-RADIUS kimlik doğrulaması](point-to-site-how-to-radius-ps.md)

* [P2S Bağlantısı Yapılandırma-Azure yerel sertifikası kimlik doğrulaması](vpn-gateway-howto-point-to-site-rm-ps.md)

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
