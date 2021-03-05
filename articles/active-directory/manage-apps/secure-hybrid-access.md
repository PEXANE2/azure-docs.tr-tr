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
ms.date: 2/16/2021
ms.author: gasinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06ed37b914c352d7162db7f8409ac09b7615da47
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102174436"
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

- [Akamai kurumsal uygulama erişimi (EAA)](https://docs.microsoft.com/azure/active-directory/saas-apps/akamai-tutorial)

- [Citrix uygulama teslim denetleyicisi (ADC)](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial)

- [F5 Big-IP APM](https://docs.microsoft.com/azure/active-directory/manage-apps/f5-aad-integration)

- [Kemp](https://docs.microsoft.com/azure/active-directory/saas-apps/kemp-tutorial)

- [Pulse Secure sanal Traffic Manager (VTM)](https://docs.microsoft.com/azure/active-directory/saas-apps/pulse-secure-virtual-traffic-manager-tutorial)

### <a name="sha-through-vpn-and-sdp-applications"></a>VPN ve SDP uygulamaları aracılığıyla SHA

VPN ve SDP çözümlerini kullanarak, kuruluşunuzun verilerini korurken dilediğiniz zaman herhangi bir cihazdan kurumsal ağınıza güvenli erişim sağlayabilirsiniz. Azure AD 'yi bir kimlik sağlayıcısı (ıDP) olarak bulundurarak, şirket içi eski uygulamalarınızı güvenli hale getirmek için Azure AD [Çoklu oturum açma](./what-is-single-sign-on.md) ve [çok faktörlü kimlik doğrulaması](../authentication/concept-mfa-howitworks.md) gibi modern kimlik doğrulama ve yetkilendirme yöntemlerini kullanabilirsiniz.  

![Görüntü, VPN iş ortakları ve uygulama proxy 'si ile güvenli karma erişimi gösterir ](./media/secure-hybrid-access/app-proxy-vpn.png)

Aşağıdaki VPN satıcıları, Azure AD ile tümleştirme için önceden oluşturulmuş çözümler ve ayrıntılı kılavuz sunar.

- [Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect)

- [Fortinet](https://docs.microsoft.com/azure/active-directory/saas-apps/fortigate-ssl-vpn-tutorial)

- [F5 Big-IP APM](https://docs.microsoft.com/azure/active-directory/manage-apps/f5-aad-password-less-vpn)

- [Palo Alto ağları küresel koruma](https://docs.microsoft.com/azure/active-directory/saas-apps/paloaltoadmin-tutorial)

- [Pulse Secure Pulse Connect güvenli (BILGISAYARLAR)](https://docs.microsoft.com/azure/active-directory/saas-apps/pulse-secure-pcs-tutorial)

Aşağıdaki SDP satıcıları, Azure AD ile tümleştirme için önceden oluşturulmuş çözümler ve ayrıntılı kılavuz sunar.

- [Datawiza erişim Aracısı](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal-setup-oidc-sso)

- [Perimeter 81](https://docs.microsoft.com/azure/active-directory/saas-apps/perimeter-81-tutorial#:~:text=For%20SSO%20to%20work,%20you%20need%20to%20establish,to%20test%20Azure%20AD%20single%20sign-on%20with%20B.Simon.)

- [Silverfort kimlik doğrulama platformu](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal-setup-oidc-sso)

- [Strata](https://docs.microsoft.com/azure/active-directory/saas-apps/maverics-identity-orchestrator-saml-connector-tutorial)

- [Zscaler Private Access (ZPA)](https://docs.microsoft.com/azure/active-directory/saas-apps/zscalerprivateaccess-tutorial)
