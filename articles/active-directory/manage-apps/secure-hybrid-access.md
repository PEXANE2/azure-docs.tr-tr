---
title: Azure AD güvenli karma erişim | Microsoft Docs
description: Bu makalede, Azure AD ile eski şirket içi, genel bulutunuzun veya özel bulut uygulamalarınızın tümleştirilmesine yönelik iş ortağı çözümleri açıklanmaktadır. Uygulama teslim denetleyicilerini veya ağlarını Azure AD 'ye bağlayarak eski uygulamalarınızın güvenliğini sağlayın.
services: active-directory
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 9/10/2020
ms.author: gasinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d64f7423d537958b6d3c388cb12f23bd2e30e36
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90087111"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-azure-active-directory"></a>Güvenli karma erişim: Azure Active Directory ile güvenli eski uygulamalar

Artık şirket içi ve bulut eski kimlik doğrulama uygulamalarınızı, ile Azure Active Directory (AD) ile bağlanarak koruyabilirsiniz:

- [Azure AD Uygulama Ara Sunucusu](#secure-hybrid-access-sha-through-azure-ad-application-proxy)

- [Mevcut uygulama teslim denetleyiciniz ve ağlarınız](#sha-through-networking-and-delivery-controllers)

- [Sanal özel ağ (VPN) ve yazılım tanımlı çevre (SDP) uygulamaları](#sha-through-vpn-and-sdp-applications)

Azure AD [koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) ve Azure AD [kimlik KORUMASı](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)gibi Azure AD özelliklerine sahip tüm uygulamalarda, boşluğu köprüleyin ve güvenlik duruşunuzu güçlendirebilirsiniz.

## <a name="secure-hybrid-access-sha-through-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu aracılığıyla güvenli karma erişimi (SHA)
  
[Uygulama proxy 'sini](https://aka.ms/whyappproxy) kullanarak şirket içi Web uygulamalarınıza [güvenli uzaktan erişim](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) sağlayabilirsiniz. Kullanıcılarınızın VPN kullanması gerekmez. Kullanıcılar, [Çoklu oturum](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal-setup-sso)açma işleminden sonra herhangi bir cihazdan uygulamalara kolayca bağlanarak faydalanır. Uygulama proxy 'Si, bir hizmet olarak uzaktan erişim sağlar ve şirket [içi uygulamalarınızı](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application) şirket ağı dışındaki kullanıcılara kolayca yayımlamanıza olanak sağlar. Şirket içi uygulamalarınızı değiştirmenize gerek kalmadan bulut erişim yönetimenizi ölçeklendirmenize yardımcı olur. Bir sonraki adım olarak [bir Azure AD uygulama ara sunucusu dağıtımı planlayın](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-deployment-plan) .

## <a name="azure-ad-partner-integrations"></a>Azure AD iş ortağı tümleştirmeleri

### <a name="sha-through-networking-and-delivery-controllers"></a>Ağ ve teslim denetleyicileri aracılığıyla SHA

[Azure AD uygulama ara sunucusu](https://aka.ms/whyappproxy)' ye ek olarak, Microsoft iş ortakları, üçüncü taraf sağlayıcılardan oluşan [sıfır güvenlik çerçevesini](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/)kullanmanıza olanak tanır. Mevcut ağ ve teslim denetleyicilerinizi kullanabilir ve iş süreçleriniz için kritik olan eski uygulamaları kolayca koruyabilirsiniz, ancak Azure AD ile önce koruyamazsınız. Bu uygulamaları korumaya başlamak için ihtiyacınız olan her şeye zaten sahipsiniz.

![Görüntü, ağ ortakları ve uygulama proxy 'si ile güvenli karma erişimi gösterir](./media/secure-hybrid-access/secure-hybrid-access.png)

Aşağıdaki ağ satıcıları, Azure AD ile tümleştirme için önceden oluşturulmuş çözümler ve ayrıntılı kılavuz sunar.

- [Akamai kurumsal uygulama erişimi (EAA)](https://docs.microsoft.com/azure/active-directory/saas-apps/akamai-tutorial)

- [Citrix uygulama teslim denetleyicisi (ADC)](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial)

- [F5 Big-IP APM](https://docs.microsoft.com/azure/active-directory/saas-apps/headerf5-tutorial)

- [Kemp](https://docs.microsoft.com/azure/active-directory/saas-apps/kemp-tutorial)

### <a name="sha-through-vpn-and-sdp-applications"></a>VPN ve SDP uygulamaları aracılığıyla SHA

VPN ve SDP çözümlerini kullanarak, kuruluşunuzun verilerini korurken dilediğiniz zaman herhangi bir cihazdan kurumsal ağınıza güvenli erişim sağlayabilirsiniz. Azure AD 'yi bir kimlik sağlayıcısı (ıDP) olarak bulundurarak, şirket içi eski uygulamalarınızı güvenli hale getirmek için Azure AD [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) ve [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) gibi modern kimlik doğrulama ve yetkilendirme yöntemlerini kullanabilirsiniz.  

![Görüntü, VPN iş ortakları ve uygulama proxy 'si ile güvenli karma erişimi gösterir ](./media/secure-hybrid-access/app-proxy-vpn.png)

Aşağıdaki VPN ve SDP satıcıları, Azure AD ile tümleştirme için önceden oluşturulmuş çözümler ve ayrıntılı kılavuz sunar.

• [Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect)

• [Fortinet](https://docs.microsoft.com/azure/active-directory/saas-apps/fortigate-ssl-vpn-tutorial)

• [F5 Big-IP APM](https://aka.ms/f5-hybridaccessguide)

• [Palo Alto ağları küresel koruma](https://docs.microsoft.com/azure/active-directory/saas-apps/paloaltoadmin-tutorial)

• [Zscaler özel erişimi (ZPA)](https://aka.ms/zscaler-hybridaccessguide)
