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
ms.openlocfilehash: 5a936552bd7d3b97e2c027f7b0d23abd7c21b6e2
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94652204"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-azure-active-directory"></a>Güvenli karma erişim: Azure Active Directory ile güvenli eski uygulamalar

Artık şirket içi ve bulut eski kimlik doğrulama uygulamalarınızı, ile Azure Active Directory (AD) ile bağlanarak koruyabilirsiniz:

- [Azure AD Uygulama Ara Sunucusu](#secure-hybrid-access-sha-through-azure-ad-application-proxy)

- [Mevcut uygulama teslim denetleyiciniz ve ağlarınız](#sha-through-networking-and-delivery-controllers)

- [Sanal özel ağ (VPN) ve Software-Defined çevre (SDP) uygulamaları](#sha-through-vpn-and-sdp-applications)

Azure AD [koşullu erişim](../conditional-access/overview.md) ve Azure AD [kimlik KORUMASı](../identity-protection/overview-identity-protection.md)gibi Azure AD özelliklerine sahip tüm uygulamalarda, boşluğu köprüleyin ve güvenlik duruşunuzu güçlendirebilirsiniz.

## <a name="secure-hybrid-access-sha-through-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu aracılığıyla güvenli karma erişimi (SHA)
  
[Uygulama proxy 'sini](./what-is-application-proxy.md) kullanarak şirket içi Web uygulamalarınıza [güvenli uzaktan erişim](./application-proxy.md) sağlayabilirsiniz. Kullanıcılarınızın VPN kullanması gerekmez. Kullanıcılar, [Çoklu oturum](./add-application-portal-setup-sso.md)açma işleminden sonra herhangi bir cihazdan uygulamalara kolayca bağlanarak faydalanır. Uygulama proxy 'Si, bir hizmet olarak uzaktan erişim sağlar ve şirket [içi uygulamalarınızı](./application-proxy-add-on-premises-application.md) şirket ağı dışındaki kullanıcılara kolayca yayımlamanıza olanak sağlar. Şirket içi uygulamalarınızı değiştirmenize gerek kalmadan bulut erişim yönetimenizi ölçeklendirmenize yardımcı olur. Bir sonraki adım olarak [bir Azure AD uygulama ara sunucusu dağıtımı planlayın](./application-proxy-deployment-plan.md) .

## <a name="azure-ad-partner-integrations"></a>Azure AD iş ortağı tümleştirmeleri

### <a name="sha-through-networking-and-delivery-controllers"></a>Ağ ve teslim denetleyicileri aracılığıyla SHA

[Azure AD uygulama ara sunucusu](./what-is-application-proxy.md)' ye ek olarak, Microsoft iş ortakları, üçüncü taraf sağlayıcılardan oluşan [sıfır güvenlik çerçevesini](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/)kullanmanıza olanak tanır. Mevcut ağ ve teslim denetleyicilerinizi kullanabilir ve iş süreçleriniz için kritik olan eski uygulamaları kolayca koruyabilirsiniz, ancak Azure AD ile önce koruyamazsınız. Bu uygulamaları korumaya başlamak için ihtiyacınız olan her şeye zaten sahipsiniz.

![Görüntü, ağ ortakları ve uygulama proxy 'si ile güvenli karma erişimi gösterir](./media/secure-hybrid-access/secure-hybrid-access.png)

Aşağıdaki ağ satıcıları, Azure AD ile tümleştirme için önceden oluşturulmuş çözümler ve ayrıntılı kılavuz sunar.

- [Akamai kurumsal uygulama erişimi (EAA)](../saas-apps/akamai-tutorial.md)

- [Citrix uygulama teslim denetleyicisi (ADC)](../saas-apps/citrix-netscaler-tutorial.md)

- [F5 Big-IP APM](../saas-apps/headerf5-tutorial.md)

- [Kemp](../saas-apps/kemp-tutorial.md)

### <a name="sha-through-vpn-and-sdp-applications"></a>VPN ve SDP uygulamaları aracılığıyla SHA

VPN ve SDP çözümlerini kullanarak, kuruluşunuzun verilerini korurken dilediğiniz zaman herhangi bir cihazdan kurumsal ağınıza güvenli erişim sağlayabilirsiniz. Azure AD 'yi bir kimlik sağlayıcısı (ıDP) olarak bulundurarak, şirket içi eski uygulamalarınızı güvenli hale getirmek için Azure AD [Çoklu oturum açma](./what-is-single-sign-on.md) ve [çok faktörlü kimlik doğrulaması](../authentication/concept-mfa-howitworks.md) gibi modern kimlik doğrulama ve yetkilendirme yöntemlerini kullanabilirsiniz.  

![Görüntü, VPN iş ortakları ve uygulama proxy 'si ile güvenli karma erişimi gösterir ](./media/secure-hybrid-access/app-proxy-vpn.png)

Aşağıdaki VPN ve SDP satıcıları, Azure AD ile tümleştirme için önceden oluşturulmuş çözümler ve ayrıntılı kılavuz sunar.

• [Cisco AnyConnect](../saas-apps/cisco-anyconnect.md)

• [Fortinet](../saas-apps/fortigate-ssl-vpn-tutorial.md)

• [F5 Big-IP APM](../saas-apps/headerf5-tutorial.md)

• [Palo Alto ağları küresel koruma](../saas-apps/paloaltoadmin-tutorial.md)

• [Zscaler özel erişimi (ZPA)](../saas-apps/zscalerprivateaccess-tutorial.md)