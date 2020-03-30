---
title: 'P2S ile uzaktan çalışma: Azure VPN Ağ Geçidi'
description: Bu sayfa, COVID-19 salgını nedeniyle uzaktan çalışmayı etkinleştirmek için Azure Kalesi'nden nasıl yararlanabileceğinizi açıklar.
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: alzam
ms.openlocfilehash: b751817467bd263e8b7c64ccc95ec82ef1579836
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337113"
---
# <a name="remote-work-using-azure-vpn-gateway-point-to-site"></a>Azure VPN Ağ Geçidi Sayfa-to-site kullanarak uzaktan çalışma

>[!NOTE]
>Bu makalede, COVID-19 krizi nedeniyle karşılaştığınız ağ sorunlarını uzaktan çalışmak ve ağ sorunlarını azaltmak için Azure VPN Ağ Geçidi, Azure, Microsoft ağı ve Azure iş ortağı ekosisteminden nasıl yararlanabileceğiniz açıklanmaktadır.
>

Bu makalede, kuruluşların kullanıcıları için uzaktan erişim kurulumu veya COVID-19 salgını sırasında mevcut çözümlerini ek kapasiteyle tamamlamaları için kullanabileceği seçenekler açıklanmaktadır.

Azure noktadan siteye çözüm bulut tabanlıdır ve kullanıcıların evden çalışma talebini karşılamak için hızlı bir şekilde sağlanabilir. Artırılmış kapasiteye artık ihtiyaç duyulmadığında kolayca ölçeklenebilir ve kolayca ve hızlı bir şekilde kapatılabilir.

## <a name="about-point-to-site-vpn"></a>Noktadan Siteye VPN Hakkında

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu çözüm, Azure VNet'lerine veya şirket içi veri merkezlerine evden veya konferans gibi uzak bir konumdan bağlanmak isteyen yolcular için yararlıdır. Bu makalede, kullanıcıların çeşitli senaryolara göre uzaktan çalışmasını nasıl etkinleştirin.

Aşağıdaki tabloistemci işletim sistemlerini ve bunların kullanabileceği kimlik doğrulama seçeneklerini gösterir. Zaten kullanılmakta olan istemci işletim sistemi dayalı kimlik doğrulama yöntemini seçmek yararlı olacaktır. Örneğin, bağlanması gereken istemci işletim sistemlerinin bir karışımı varsa, Sertifika tabanlı kimlik doğrulaması ile OpenVPN'i seçin. Ayrıca, siteye göre vpn'in yalnızca rota tabanlı VPN ağ geçitlerinde desteklenildiğini lütfen unutmayın.

![noktadan siteye](./media/working-remotely-support/ostable.png "İşletim Sistemi")

## <a name="scenario-1---users-need-access-to-resources-in-azure-only"></a>Senaryo 1 - Kullanıcıların yalnızca Azure'daki kaynaklara erişmeleri gerekir

Bu senaryoda, uzak kullanıcıların yalnızca Azure'da bulunan kaynaklara erişmeleri gerekir.

![noktadan siteye](./media/working-remotely-support/scenario1.png "Scanario 1")

Yüksek düzeyde, kullanıcıların Azure kaynaklarına güvenli bir şekilde bağlanabilmeleri için aşağıdaki adımlar gereklidir:

1. Sanal Ağ Ağ Ağ Geçidi Oluşturma (yoksa)
2. Ağ geçidinde noktadan siteye VPN'i yapılandırma
    3. [Sertifika kimlik doğrulaması için bu bağlantıyı takip edin.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw)
    2.  [OpenVPN için bu bağlantıyı takip edin.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-openvpn)
    3.  [Azure AD kimlik doğrulaması için bu bağlantıyı izleyin.](https://docs.microsoft.com/azure/vpn-gateway/openvpn-azure-ad-tenant)
    4.  [Sorun giderme noktası sayfa-site bağlantıları için bu bağlantıyı izleyin.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems)
3. VPN istemcisi yapılandırmasını indirin ve dağıtın
4. Sertifikaları (sertifika kimlik doğrulaması seçilirse) istemcilere dağıtma
5. Azure VPN'e bağlanın

## <a name="scenario-2---users-need-access-to-resources-in-azure-andor-on-prem-resources"></a>Senaryo 2 - Kullanıcıların Azure ve/veya ön hazırlık kaynaklarındaki kaynaklara erişmeleri gerekir

Bu senaryoda, uzak kullanıcıların Azure'da ve şirket içi veri merkezinde bulunan kaynaklara erişmeleri gerekir.

![noktadan siteye](./media/working-remotely-support/scenario2.png "Senaryo 2")

Yüksek düzeyde, kullanıcıların Azure kaynaklarına güvenli bir şekilde bağlanabilmeleri için aşağıdaki adımlar gereklidir:

1. Sanal Ağ Ağ Ağ Geçidi Oluşturma (yoksa)
2. Ağ geçidinde noktadan siteye VPN'i yapılandırın (yukarıdaki Senaryo 1'e bakın)
3. Azure Sanal Ağ Ağ Geçidi'nde BGP etkinleştirilmiş siteden siteye tüneli yapılandırma
4. Azure Sanal Ağ Ağ Geçidine bağlanmak için şirket aygıtında yapılandırma
5. Azure portalından siteye nokta profilini indirin ve istemcilere dağıtın

[Siteden siteye VPN tüneli nasıl kurulacaksını öğrenmek için bu bağlantıyı izleyin](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="faq-for-native-azure-certificate-authentication"></a><a name="faqcert"></a>Yerel Azure sertifika kimlik doğrulaması için SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faq-for-radius-authentication"></a><a name="faqradius"></a>RADIUS kimlik doğrulaması için SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar

* [Bir P2S bağlantısını yapılandırma - Azure AD kimlik doğrulaması](openvpn-azure-ad-tenant.md)

* [Bir P2S bağlantısını yapılandırma - RADIUS kimlik doğrulaması](point-to-site-how-to-radius-ps.md)

* [Bir P2S bağlantısını yapılandırma - Azure yerel sertifika kimlik doğrulaması](vpn-gateway-howto-point-to-site-rm-ps.md)

**"OpenVPN", OpenVPN Inc. şirketinin ticari markasıdır.**