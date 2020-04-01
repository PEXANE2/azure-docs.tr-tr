---
title: SSTP'den OpenVPN veya IKEv2'ye geçiş | Azure VPN Ağ Geçidi
description: Bu makale, SSTP'nin 128 eşzamanlı bağlantı sınırını aşmanın yollarını anlamanıza yardımcı olur.
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: alzam
ms.openlocfilehash: 5500d993a4bf3c664f14182d983f9abed8ebb08a
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398374"
---
# <a name="transition-to-openvpn-protocol-or-ikev2-from-sstp"></a>SSTP'den OpenVPN protokolüne veya IKEv2'ye geçiş

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu makale, Kaynak Yöneticisi dağıtım modeli için geçerlidir ve OpenVPN protokolüne veya IKEv2'ye geçerek SSTP'nin 128 eşzamanlı bağlantı sınırını aşmanın yolları hakkında görüşmeler yapmaktadır.

## <a name="what-protocol-does-p2s-use"></a><a name="protocol"></a>P2S hangi protokolü kullanır?

Noktadan siteye VPN aşağıdaki protokollerden birini kullanabilir:

* **OpenVPN&reg; Protokolü**, SSL/TLS tabanlı VPN protokolü. Çoğu güvenlik duvarı TCP bağlantı noktası 443 giden açık olduğundan, Bir SSL VPN çözümü güvenlik duvarları nüfuz edebilir, Hangi SSL kullanır. OpenVPN, Android, iOS (11.0 ve üzeri sürümler), Windows, Linux ve Mac aygıtlarından (OSX sürümleri 10.13 ve üzeri) bağlanmak için kullanılabilir.

* **Güvenli Soket Tünel Protokolü (SSTP),** özel bir SSL tabanlı VPN protokolü. Çoğu güvenlik duvarı TCP bağlantı noktası 443 giden açık olduğundan, Bir SSL VPN çözümü güvenlik duvarları nüfuz edebilir, Hangi SSL kullanır. SSTP yalnızca Windows aygıtlarında desteklenir. Azure, Windows'un SSTP (Windows 7 ve sonrası) tüm sürümlerini destekler. **SSTP, ağ geçidi SKU'dan bağımsız olarak yalnızca 128'e kadar eşzamanlı bağlantıyı destekler.**

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.


>[!NOTE]
>IkEv2 ve P2S için OpenVPN yalnızca Kaynak Yöneticisi dağıtım modeli için kullanılabilir. Bunlar klasik dağıtım modeli için kullanılamaz. Temel ağ geçidi SKU IKEv2 veya OpenVPN protokollerini desteklemez. Temel SKU kullanıyorsanız, bir üretim SKU Sanal Ağ Ağ Ağ Geçidi'ni silmek ve yeniden oluşturmak zorunda kaldığınız zaman.
>

## <a name="migrating-from-sstp-to-ikev2-or-openvpn"></a>SSTP'den IKEv2 veya OpenVPN'e geçiş

VPN ağ geçidine 128'den fazla eşzamanlı P2S bağlantısını desteklemek istediğiniz, ancak SSTP kullandığınız durumlar olabilir. Böyle bir durumda IKEv2 veya OpenVPN protokolüne geçmeniz gerekir.

### <a name="option-1---add-ikev2-in-addition-to-sstp-on-the-gateway"></a>Seçenek 1 - Ağ Geçidi'ndeki SSTP'ye ek olarak IKEv2 ekle

Bu en basit seçenektir. SSTP ve IKEv2 aynı ağ geçidinde bir arada bulunabilir ve size daha fazla sayıda eşzamanlı bağlantı verebilir. Mevcut ağ geçidinde IKEv2'yi etkinleştirebilir ve istemciyi yeniden indirebilirsiniz.

Mevcut bir SSTP VPN ağ geçidine IKEv2 eklemek varolan istemcileri etkilemez ve bunları IKEv2'yi küçük gruplar halinde kullanacak veya ikev2 kullanacak şekilde yapılandırabilirsiniz. Bir Windows istemcisi hem SSTP hem de IKEv2 için yapılandırılırsa, önce IKEV2 kullanarak bağlanmayı deneyecek ve bu başarısız olursa, SSTP'ye geri döner.

**IKEv2 standart olmayan UDP bağlantı noktalarını kullanır, bu nedenle bu bağlantı noktalarının kullanıcının güvenlik duvarında engellenmediğinden emin olmanız gerekir. Kullanılan bağlantı noktaları UDP 500 ve 4500'dir.**

IKEv2'yi varolan bir ağ geçidine eklemek için portaldaki Sanal Ağ Ağ Geçidi'nin altındaki "noktaya noktaya yapılandırma" sekmesine gidin ve açılan kutudan **IKEv2 ve SSTP'yi (SSL)** seçin.

![noktadan siteye](./media/ikev2-openvpn-from-sstp/sstptoikev2.png "IKEv2")


### <a name="option-2---remove-sstp-and-enable-openvpn-on-the-gateway"></a>Seçenek 2 - SSTP'yi kaldırın ve Ağ Geçidi'nde OpenVPN'i etkinleştirin

SSTP ve OpenVPN her ikisi de TLS tabanlı protokol olduğundan, aynı ağ geçidinde bir arada bulunamazlar. SSTP'den OpenVPN'e geçmeye karar verirseniz, SSTP'yi devre dışı bırakıp ağ geçidinde OpenVPN'i etkinleştirmeniz gerekir. Bu işlem, istemciüzerinde yeni profil yapılandırılına kadar varolan istemcilerin VPN ağ geçidine bağlantı kaybetmesine neden olur.

İsterseniz IKEv2 ile birlikte OpenVPN'i etkinleştirebilirsiniz. OpenVPN TLS tabanlıdır ve standart TCP 443 bağlantı noktasını kullanır. OpenVPN'e geçmek için portaldaki Sanal Ağ Ağ Ağ Geçidi'nin altındaki "noktaya yapılandırma" sekmesine gidin ve açılan kutudan **OpenVPN (SSL)** veya **IKEv2 ve OpenVPN'i (SSL) seçin.**

![noktadan siteye](./media/ikev2-openvpn-from-sstp/sstptoopenvpn.png "OpenVPN")

Ağ geçidi yapılandırıldıktan sonra, [openvpn istemcilerini dağıtıp yapılandırana](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-openvpn-clients)kadar varolan istemciler bağlanamayacaktır.

Windows 10 kullanıyorsanız, Windows için [Azure VPN İstemcisini](https://docs.microsoft.com/azure/vpn-gateway/openvpn-azure-ad-client#to-download-the-azure-vpn-client) de kullanabilirsiniz


## <a name="frequently-asked-questions"></a>Sık sorulan sorular
### <a name="what-are-the-client-configuration-requirements"></a>İstemci yapılandırma gereksinimleri nelerdir?

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

### <a name="which-gateway-skus-support-p2s-vpn"></a><a name="gwsku"></a>Hangi ağ geçidi SUS desteği P2S VPN?

[!INCLUDE [aggregate throughput sku](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

* Ağ Geçidi SKU önerileri için [VPN Ağ Geçidi ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku)bkz.

>[!NOTE]
>Temel SKU, IKEv2 veya RADIUS kimlik doğrulamasını desteklemez.
>

### <a name="what-ikeipsec-policies-are-configured-on-vpn-gateways-for-p2s"></a><a name="IKE/IPsec policies"></a>Hangi IKE/IPsec ilkeleri P2S için VPN ağ geçitlerinde yapılandırılır?


**IKEv2**

|**Şifre** | **Bütünlük** | **Prf** | **DH Grubu** |
|---        | ---            | ---        | ---     |
|GCM_AES256 |    GCM_AES256    | SHA384    | GROUP_24 |
|GCM_AES256 |    GCM_AES256    | SHA384    | GROUP_14 |
|GCM_AES256 |    GCM_AES256    | SHA384    | GROUP_ECP384 |
|GCM_AES256 |    GCM_AES256    | SHA384    | GROUP_ECP256 |
|GCM_AES256 |    GCM_AES256    | SHA256    | GROUP_24 |
|GCM_AES256 |    GCM_AES256    | SHA256    | GROUP_14 |
|GCM_AES256 |    GCM_AES256    | SHA256    | GROUP_ECP384 |
|GCM_AES256 |    GCM_AES256    | SHA256    | GROUP_ECP256 |
|AES256     |   SHA384        | SHA384    | GROUP_24 |
|AES256     |   SHA384        | SHA384    | GROUP_14 |
|AES256     |   SHA384        | SHA384    | GROUP_ECP384 |
|AES256     |   SHA384        | SHA384    | GROUP_ECP256 |
|AES256     |   SHA256        | SHA256    | GROUP_24 |
|AES256     |   SHA256        | SHA256    | GROUP_14 |
|AES256     |   SHA256        | SHA256    | GROUP_ECP384 |
|AES256     |   SHA256        | SHA256    | GROUP_ECP256 |
|AES256     |   SHA256        | SHA256    | GROUP_2 |

**IPsec**

|**Şifre** | **Bütünlük** | **PFS Grubu** |
|---        | ---            | ---        |
|GCM_AES256    | GCM_AES256 | GROUP_NONE |
|GCM_AES256    | GCM_AES256 | GROUP_24 |
|GCM_AES256    | GCM_AES256 | GROUP_14 |
|GCM_AES256    | GCM_AES256 | GROUP_ECP384 |
|GCM_AES256    | GCM_AES256 | GROUP_ECP256 |
| AES256    | SHA256 | GROUP_NONE |
| AES256    | SHA256 | GROUP_24 |
| AES256    | SHA256 | GROUP_14 |
| AES256    | SHA256 | GROUP_ECP384 |
| AES256    | SHA256 | GROUP_ECP256 |
| AES256    | SHA1 | GROUP_NONE |

### <a name="what-tls-policies-are-configured-on-vpn-gateways-for-p2s"></a><a name="TLS policies"></a>P2S için VPN ağ geçitlerinde hangi TLS ilkeleri yapılandırılır?
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

### <a name="how-do-i-configure-a-p2s-connection"></a><a name="configure"></a>P2S bağlantısını nasıl yapılandırıyorum?

Bir P2S yapılandırması oldukça birkaç özel adım gerektirir. Aşağıdaki makaleler, P2S yapılandırması ile size yol için adımlar ve VPN istemci aygıtları yapılandırmak için bağlantılar içerir:

* [Bir P2S bağlantısını yapılandırma - RADIUS kimlik doğrulaması](point-to-site-how-to-radius-ps.md)

* [Bir P2S bağlantısını yapılandırma - Azure yerel sertifika kimlik doğrulaması](vpn-gateway-howto-point-to-site-rm-ps.md)

* [OpenVPN yapılandırması](vpn-gateway-howto-openvpn.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Bir P2S bağlantısını yapılandırma - RADIUS kimlik doğrulaması](point-to-site-how-to-radius-ps.md)

* [Bir P2S bağlantısını yapılandırma - Azure yerel sertifika kimlik doğrulaması](vpn-gateway-howto-point-to-site-rm-ps.md)

**"OpenVPN", OpenVPN Inc. şirketinin ticari markasıdır.**
