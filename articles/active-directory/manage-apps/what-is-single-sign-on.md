---
title: Uygulamalarda tek oturum açma - Azure Active Directory | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde (Azure AD) uygulamaları yapılandırırken tek bir oturum açma yöntemini nasıl seçeceğinizi öğrenin. Kullanıcıların her uygulama için parolaları hatırlamaları ve hesap yönetiminin yönetimini basitleştirmeleri için tek oturum açma kullanın.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: mimart
ms.reviewer: arvindh, japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: f46bcf412403d8f911e484e12a9d1f421b1666f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366079"
---
# <a name="single-sign-on-to-applications-in-azure-active-directory"></a>Azure Active Directory'deki uygulamalarda tek oturum açma

Tek oturum açma (SSO), kullanıcılar Azure Active Directory (Azure AD) uygulamaları oturum açtığında güvenlik ve kolaylık sağlar. Bu makalede, tek oturum açma yöntemleri açıklanır ve uygulamalarınızı yapılandırırken en uygun SSO yöntemini seçmenize yardımcı olur.

- **Tek oturum açma ile**kullanıcılar, etki alanı yla birleştirilmiş aygıtlara, şirket kaynaklarına, hizmet olarak yazılım (SaaS) uygulamalarına ve web uygulamalarına erişmek için tek bir hesapla oturum açmaktadır. Oturum açıldıktan sonra, kullanıcı Uygulamaları Office 365 portalından veya Azure AD MyApps erişim panelinden başlatabilir. Yöneticiler kullanıcı hesabı yönetimini merkezileştirebilir ve grup üyeliğine dayalı olarak uygulamalara kullanıcı erişimini otomatik olarak ekleyebilir veya kaldırabilir.

- **Tek oturum açmadan,** kullanıcılar uygulamaya özgü parolaları hatırlamalı ve her uygulamada oturum açmalıdır. BT personelinin Office 365, Box ve Salesforce gibi her uygulama için kullanıcı hesapları oluşturması ve güncelleştirmesi gerekir. Kullanıcıların parolalarını hatırlamaları ve her uygulamada oturum açmaları için zaman harcamaları gerekir.

## <a name="choosing-a-single-sign-on-method"></a>Tek bir oturum açma yöntemi seçme

Tek oturum açma için bir uygulamayı yapılandırmanın birkaç yolu vardır. Tek bir oturum açma yöntemi seçmek, uygulamanın kimlik doğrulaması için nasıl yapılandırıldığına bağlıdır.

- Bulut uygulamaları, openid connect, OAuth, SAML, parola tabanlı, bağlantılı veya devre dışı bırakılmış yöntemleri tek oturum açmak için kullanabilir. 
- Şirket içi uygulamalar, parola tabanlı, Tümleşik Windows Kimlik Doğrulaması, üstbilgi tabanlı, bağlantılı veya devre dışı bırakılmış yöntemleri tek oturum açma için kullanabilir. Uygulamalar Uygulama Proxy'si için yapılandırıldığında şirket içi seçimler çalışır.

Bu akış şeması, durumunuz için en iyi tek oturum açma yöntemine karar vermenize yardımcı olur.

![Tek oturum açma yöntemi için karar akış şeması](./media/what-is-single-sign-on/choose-single-sign-on-method-040419.png)

Aşağıdaki tabloda tek oturum açma yöntemleri özetler ve daha fazla ayrıntıya bağlantılar.

| Tek oturum açma yöntemi | Uygulama türleri | Kullanılması gereken durumlar |
| :------ | :------- | :----- |
| [OpenID Connect ve OAuth](#openid-connect-and-oauth) | bulut sadece | Yeni bir uygulama geliştirirken OpenID Connect ve OAuth'u kullanın. Bu protokol uygulama yapılandırmasını basitleştirir, kullanımı kolay SDK'lara sahiptir ve uygulamanızın MS Graph'ı kullanmasını sağlar.
| [SAML](#saml-sso) | bulut ve şirket içi | OpenID Connect veya OAuth kullanmayan mevcut uygulamalar için mümkün olduğunca SAML'yi seçin. SAML, SAML protokollerinden birini kullanarak kimlik doğrulaması yapan uygulamalar için çalışır.|
| [Parola tabanlı](#password-based-sso) | bulut ve şirket içi | Uygulama kullanıcı adı ve parolayla kimlik doğrulaması yaptığında parola tabanlı seçeneğini belirleyin. Parola tabanlı tek oturum açma, güvenli uygulama parolası depolamave bir web tarayıcısı uzantısı veya mobil uygulama kullanarak yeniden oynatma sağlar. Bu yöntem, uygulama tarafından sağlanan varolan oturum açma işlemini kullanır, ancak bir yöneticinin parolaları yönetmesini sağlar. |
| [Bağlantılı](#linked-sign-on) | bulut ve şirket içi | Uygulama başka bir kimlik sağlayıcı hizmetinde tek oturum açma için yapılandırıldığında bağlantılı oturum açma'yı seçin. Bu seçenek uygulamaya tek oturum açma eklemez. Ancak, uygulama zaten Active Directory Federation Services gibi başka bir hizmet kullanılarak uygulanan tek oturum açma olabilir.|
| [Devre dışı](#disabled-sso) | bulut ve şirket içi | Uygulama tek oturum açma için yapılandırılacak hazır olmadığında devre dışı tek oturum açma'yı seçin. Bu mod, uygulamayı oluştururken varsayılan dır.|
| [Tümleşik Windows Kimlik Doğrulama (IWA)](#integrated-windows-authentication-iwa-sso) | yalnızca şirket içi | [Tümleşik Windows Kimlik Doğrulama (IWA) veya](/aspnet/web-api/overview/security/integrated-windows-authentication)hak taleplerine duyarlı uygulamalar kullanan uygulamalar için IWA tek oturum açma seçeneğini belirleyin. IWA için, Uygulama Proxy bağlayıcıları uygulamanın kullanıcıları doğrulamak için Kerberos Kısıtlı Temsilciliği (KCD) kullanın. |
| [Üstbilgi tabanlı](#header-based-sso) | yalnızca şirket içi | Uygulama kimlik doğrulaması için üstbilgi kullanırken üstbilgi tabanlı tek oturum açma'yı kullanın. Üstbilgi tabanlı tek oturum açma için Azure AD için PingAccess gerektirir. Application Proxy, kullanıcının kimliğini doğrulamak için Azure AD kullanır ve ardından trafiği bağlayıcı hizmetinden geçirir.  |

## <a name="openid-connect-and-oauth"></a>OpenID Connect ve OAuth

Yeni uygulamalar geliştirirken, birden çok cihaz platformunda uygulamanız için en iyi tek oturum açma deneyimini elde etmek için OpenID Connect ve OAuth gibi modern protokolleri kullanın. OAuth, kullanıcıların veya yöneticilerin [Microsoft Graph](/graph/overview)gibi korumalı kaynaklar için [izin vermesini](configure-user-consent.md) sağlar. Uygulamanız için [SDK'ları](../develop/reference-v2-libraries.md) benimsemesi kolay salıyoruz ve ayrıca uygulamanız [Microsoft Graph'ı](/graph/overview)kullanmaya hazır olacaktır.

Daha fazla bilgi için bkz.

- [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md)
- [OpenID Connect 1.0](../develop/v2-protocols-oidc.md)
- [Microsoft kimlik platformu geliştirici kılavuzu.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

## <a name="saml-sso"></a>SAML SSO

**SAML tek oturum açma**ile Azure AD, kullanıcının Azure AD hesabını kullanarak uygulamaya kimlik doğrular. Azure AD, oturum açma bilgilerini bir bağlantı protokolü aracılığıyla uygulamaya iletir. SAML tabanlı tek oturum açma ile, kullanıcıları SAML taleplerinizde tanımladığınız kurallara göre belirli uygulama rolleriyle eşleyebilirsiniz.

Uygulama desteklediğinde SAML tabanlı tek oturum açma'yı seçin.

SamL tabanlı tek oturum açma, bu protokollerden herhangi birini kullanan uygulamalar için desteklenir:

- SAML 2.0
- WS-Federation

SAML tabanlı tek oturum açma için bir SaaS uygulamasını yapılandırmak için [bkz.](configure-single-sign-on-non-gallery-applications.md) Ayrıca, birçok Hizmet Olarak Yazılım (SaaS) uygulamaları, SAML tabanlı tek oturum açma yapılandırması boyunca size adım atan uygulamaya özel bir [öğreticiye](../saas-apps/tutorial-list.md) sahiptir.

WS-Federation için bir uygulamayı yapılandırmak için, SAML tabanlı tek oturum açma uygulamasını yapılandırmak için aynı kılavuzu izleyin, [bkz.](configure-single-sign-on-non-gallery-applications.md) Uygulamayı Azure AD'yi kullanacak şekilde yapılandırmak için, WS-Federation bitiş noktası `https://login.microsoftonline.com/<tenant-ID>/wsfed`için Azure AD giriş URL'sini değiştirmeniz gerekir.

SAML tabanlı tek oturum açma için şirket içi bir uygulamayı yapılandırmak [için, Application Proxy ile şirket içi uygulamalar için SAML tek oturum](application-proxy-configure-single-sign-on-on-premises-apps.md)açma'ya bakın.

SAML protokolü hakkında daha fazla bilgi için tek [oturum açma SAML protokolüne](../develop/single-sign-on-saml-protocol.md)bakın.

## <a name="password-based-sso"></a>Parola tabanlı SSO

Parola tabanlı oturum açma ile kullanıcılar uygulamaya ilk erişlerinde bir kullanıcı adı ve parolayla oturum açarak oturum açarak uygulamaya giriş yaparlar. Azure AD, ilk oturum açmadan sonra uygulamaya kullanıcı adı ve parola sağlar.

Parola tabanlı tek oturum açma, uygulama tarafından sağlanan varolan kimlik doğrulama işlemini kullanır. Bir uygulama için parola tek oturum açma'yı etkinleştirdiğinizde, Azure AD uygulama için kullanıcı adlarını ve parolaları toplar ve güvenli bir şekilde saklar. Kullanıcı kimlik bilgileri dizinde şifreli bir durumda depolanır.

Aşağıdaki anda parola tabanlı tek oturum açma seçeneğini belirleyin:

- Bir uygulama SAML tek oturum açma protokolünü desteklemez.
- Uygulama, belirteçlere ve üstbilgilere erişmek yerine kullanıcı adı ve parolayla kimlik doğrulaması sağlar.

Html tabanlı oturum açma sayfası olan bulut tabanlı tüm uygulamalar için parola tabanlı tek oturum açma önerilir. Kullanıcı aşağıdaki tarayıcılardan herhangi birini kullanabilir:

- Windows 7 veya sonraki internet explorer 11
   > [!NOTE]
   > Internet Explorer sınırlı destek tedir ve artık yeni yazılım güncelleştirmeleri almaz. Microsoft Edge önerilen tarayıcıdır.

- Windows 10 Anniversary Edition veya sonraki sürümde Microsoft Edge
- iOS ve Android için Microsoft Edge
- Intune Yönetilen Tarayıcı
- Windows 7 veya sonraki nde ve MacOS X'te veya daha sonra Chrome
- Firefox 26.0 veya daha sonra Windows XP SP2 veya daha sonra ve Mac OS X 10.6 veya daha sonra

Parola tabanlı tek oturum açma için bulut uygulamasını yapılandırmak için [bkz.](configure-password-single-sign-on-non-gallery-applications.md)

Uygulama Proxy üzerinden tek oturum açma için şirket içi bir uygulamayı yapılandırmak [için, Application Proxy ile tek oturum açma için Parola kasasına](application-proxy-configure-single-sign-on-password-vaulting.md) bakın

### <a name="how-authentication-works-for-password-based-sso"></a>Parola tabanlı SSO için kimlik doğrulama nasıl çalışır?

Azure AD, bir uygulamanın kimliğini doğrulamak için kullanıcının kimlik bilgilerini dizinden alır ve uygulamanın oturum açma sayfasına girer.  Azure AD, kullanıcı kimlik bilgilerini bir web tarayıcısı uzantısı veya mobil uygulama aracılığıyla güvenli bir şekilde geçirir. Bu işlem, bir yöneticinin kullanıcı kimlik bilgilerini yönetmesini sağlar ve kullanıcıların parolalarını hatırlamalarını gerektirmez.

> [!IMPORTANT]
> Kimlik bilgileri, otomatik oturum açma işlemi sırasında kullanıcıdan gizlenir. Ancak, kimlik bilgileri web hata ayıklama araçları kullanılarak kullanılabilir. Kullanıcıların ve yöneticilerin kimlik bilgileri doğrudan kullanıcı tarafından girildigibi aynı güvenlik ilkelerini izlemesi gerekir.

### <a name="managing-credentials-for-password-based-sso"></a>Parola tabanlı SSO için kimlik bilgilerini yönetme

Her uygulamanın parolaları Azure AD yöneticisi veya kullanıcılar tarafından yönetilebilir.

Azure AD yöneticisi kimlik bilgilerini yönettiğinde:  

- Kullanıcının kullanıcı adını ve parolayı sıfırlaması veya hatırlaması gerekmez. Kullanıcı, erişim panelinde üzerine tıklayarak veya sağlanan bir bağlantı aracılığıyla uygulamaya erişebilir.
- Yönetici kimlik bilgileri üzerinde yönetim görevleri yapabilir. Örneğin, yönetici uygulama erişimini kullanıcı grubu üyeliklerine ve çalışan durumuna göre güncelleyebilir.
- Yönetici, birçok kullanıcı arasında paylaşılan uygulamalara erişim sağlamak için yönetim kimlik bilgilerini kullanabilir. Örneğin, yönetici bir uygulamaya erişebilen herkesin bir sosyal medyaya veya belge paylaşım uygulamasına erişmesine izin verebilir.

Son kullanıcı kimlik bilgilerini yönettiğinde:

- Kullanıcılar parolalarını gerektiği gibi güncelleyerek veya silerek yönetebilirler.
- Yöneticiler uygulama için yeni kimlik bilgileri ayarlayabilir.

## <a name="linked-sign-on"></a>Bağlantılı oturum açma
Bağlantılı oturum açma, Azure AD'nin başka bir hizmette tek oturum açma için zaten yapılandırılmış bir uygulamaya tek oturum açma sağlamasına olanak tanır. Bağlantılı uygulama, Office 365 portalındaki veya Azure AD MyApps portalındaki son kullanıcılara görünebilir. Örneğin, bir kullanıcı, Office 365 portalından Active Directory Federation Services 2.0'da (AD FS) tek oturum açma için yapılandırılan bir uygulama başlatabilir. Office 365 portalından veya Azure AD MyApps portalından başlatılan bağlantılı uygulamalar için ek raporlama da mevcuttur. Bağlantılı oturum açma için bir uygulamayı yapılandırmak için [bkz.](configure-linked-sign-on.md)

### <a name="linked-sign-on-for-application-migration"></a>Uygulama geçişi için bağlantılı oturum açma

Bağlantılı oturum açma, uygulamaları belirli bir süre boyunca geçiş yaparken tutarlı bir kullanıcı deneyimi sağlayabilir. Uygulamaları Azure Etkin Dizin'e geçiriyorsanız, geçirmek istediğiniz tüm uygulamaların bağlantılarını hızla yayınlamak için bağlantılı oturum açma'yı kullanabilirsiniz.  Kullanıcılar [MyApps portalında](../user-help/active-directory-saas-access-panel-introduction.md) veya [Office 365 uygulama başlatıcısında](https://support.office.com/article/meet-the-office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a)tüm bağlantıları bulabilirler. Kullanıcılar bağlantılı bir uygulamaya veya geçirilen bir uygulamaya eriştiklerini bilmeyecekler.  

Bir kullanıcı bağlı bir uygulamayla kimlik doğrulaması yaptıktan sonra, son kullanıcıya tek oturum açma erişimi sağlanmadan önce bir hesap kaydının oluşturulması gerekir. Bu hesap kaydının sağlanması otomatik olarak oluşabilir veya bir yönetici tarafından el ile oluşabilir.

## <a name="disabled-sso"></a>Engelli SSO

Devre dışı bırakma modu, uygulama için tek oturum açma nın kullanılmaz anlamına gelir. Tek oturum açma devre dışı bırakıldığında, kullanıcıların iki kez kimlik doğrulaması gerekebilir. Kullanıcılar önce Azure AD kimliğine kimlik talimi verir ve ardından uygulamada oturum açtır.

Devre dışı bırakılmış tek oturum açma modunu kullanın:

- Bu uygulamayı Azure AD tek oturum açma ile tümleştirmeye hazır değilseniz veya
- Uygulamanın diğer yönlerini test ediyorsanız veya
- Kullanıcıların kimlik doğrulamasını gerektirmeyen şirket içi bir uygulamaya güvenlik katmanı olarak. Devre dışı bırakıldığında, kullanıcının kimlik doğrulaması gerekir.

SP tarafından başlatılan SAML tabanlı tek oturum açma uygulamasını yapılandırmışsanız ve SSO modunu devre dışı bırakmak için değiştirirseniz, bunun kullanıcıların MyApps portalı dışındaki uygulamaya oturum açmalarını engellemeyeceğini unutmayın. Bunu başarmak için, [kullanıcıların oturum açma özelliğini devre dışı](disable-user-sign-in-portal.md)

## <a name="integrated-windows-authentication-iwa-sso"></a>Tümleşik Windows Kimlik Doğrulama (IWA) SSO

[Application Proxy,](application-proxy.md) [Tümleşik Windows Kimlik Doğrulama (IWA) veya](/aspnet/web-api/overview/security/integrated-windows-authentication)taleplere duyarlı uygulamalar kullanan uygulamalara tek oturum açma (SSO) sağlar. Uygulamanız IWA kullanıyorsa, Uygulama Proxy'si Kerberos Kısıtlı Delegasyonu (KCD) kullanarak uygulamaya kimlik doğrulaması sağlar. Azure Etkin Dizin'e güvenen taleplere duyarlı bir uygulama için, kullanıcı Azure AD kullanılarak zaten kimlik doğrulaması olduğundan tek oturum açma çalışır.

IWA ile kimlik doğrulayan bir şirket içi uygulamada tek oturum açma sağlamak için Tümleşik Windows Kimlik Doğrulama tek oturum açma modunu seçin.

IWA için şirket içi bir uygulamayı yapılandırmak için Uygulama [Proxy ile uygulamalarınızda tek oturum açma için Kerberos Kısıtlı Temsilciliği'ne](application-proxy-configure-single-sign-on-with-kcd.md)bakın.

### <a name="how-single-sign-on-with-kcd-works"></a>KCD ile tek oturum açma nasıl çalışır?
Bu diyagram, kullanıcı IWA kullanan bir şirket içi uygulamaya eriştığında akışı açıklar.

![Microsoft Azure AD kimlik doğrulama akış diyagramı](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. Kullanıcı, Uygulama Proxy aracılığıyla şirket içi uygulamaya erişmek için URL'yi girer.
1. Application Proxy, isteği Azure AD kimlik doğrulama hizmetlerine önceden kimlik doğrulamaya yönlendirir. Bu noktada, Azure AD çok faktörlü kimlik doğrulama gibi geçerli kimlik doğrulama ve yetkilendirme ilkeleri uygular. Kullanıcı doğrulanırsa, Azure AD bir belirteç oluşturur ve kullanıcıya gönderir.
1. Kullanıcı belirteci Uygulama Proxy'sine geçirir.
1. Application Proxy belirteci doğrular ve kullanıcı ana adı (UPN) belirteci alır. Daha sonra isteği, UPN'yi ve Hizmet Ana Adı'nı (SPN) usulüne uygun olarak doğrulanmış güvenli bir kanal aracılığıyla Bağlayıcı'ya gönderir.
1. Bağlayıcı, uygulama için bir Kerberos belirteci almak için kullanıcı taklit, tesislerinde AD ile Kerberos Kısıtlı Delegasyon (KCD) müzakere kullanır.
1. Active Directory, uygulama için Kerberos belirtecikonan'ı bağlayıcıya gönderir.
1. Bağlayıcı, AD'den aldığı Kerberos belirteci kullanarak orijinal isteği uygulama sunucusuna gönderir.
1. Uygulama yanıtı bağlayıcıya gönderir ve bu yanıt daha sonra Uygulama Proxy hizmetine ve son olarak kullanıcıya döndürülür.

## <a name="header-based-sso"></a>Üstbilgi tabanlı SSO

Kimlik doğrulama için HTTP üstbilgilerini kullanan uygulamalar için üstbilgi tabanlı tek oturum açma çalışmaları çalışır. Bu oturum açma yöntemi PingAccess adlı bir üçüncü taraf kimlik doğrulama hizmeti kullanır. Bir kullanıcının yalnızca Azure AD kimliğine doğrulanması gerekir.

Uygulama Proxy ve PingAccess uygulama için yapılandırıldığında üstbilgi tabanlı tek oturum açma'yı seçin.

Üstbilgi tabanlı kimlik doğrulaması yapılandırmak için, Uygulama Proxy ile [tek oturum açma için Üstbilgi tabanlı kimlik doğrulamasına](application-proxy-configure-single-sign-on-with-ping-access.md)bakın.

### <a name="what-is-pingaccess-for-azure-ad"></a>Azure AD için PingAccess nedir?

Azure AD için PingAccess'i kullanan kullanıcılar, kimlik doğrulama için üstbilgi kullanan uygulamalara erişebilir ve tek oturum açabilir. Application Proxy, erişimin kimliğini doğrulamak için Azure AD'yi kullanarak ve ardından bağlayıcı hizmetinden trafiği geçerek bu uygulamaları diğerleri gibi ele verir. Kimlik doğrulama oluştuktan sonra PingAccess hizmeti, Azure AD erişim belirteci'ni uygulamaya gönderilen üstbilgi biçimine çevirir.

Kullanıcılarınız kurumsal uygulamalarınızı kullanmak için oturum açken farklı bir şey fark etmez. Herhangi bir cihazüzerinde herhangi bir yerden çalışabilirler. Application Proxy bağlayıcıları uzak trafiği tüm uygulamalara yönlendirir ve bakiyeyi otomatik olarak yüklemeye devam ederler.

### <a name="how-do-i-get-a-license-for-pingaccess"></a>PingAccess için nasıl lisans alabilirim?

Bu senaryo Azure AD ve PingAccess arasındaki bir ortaklık aracılığıyla sunulduğundan, her iki hizmet için de lisans gerekir. Ancak Azure AD Premium abonelikleri, en fazla 20 uygulamayı kapsayan temel bir PingAccess lisansIçerir. 20'den fazla üstbilgi tabanlı uygulama yayımlamanız gerekiyorsa, PingAccess'ten ek bir lisans alabilirsiniz.

Daha fazla bilgi için [Azure Etkin Dizin sürümlerine](../fundamentals/active-directory-whatis.md)bakın.

## <a name="related-articles"></a>İlgili makaleler:
* [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](../saas-apps/tutorial-list.md)
* [SAML tabanlı tek oturum açma yapılandırma](configure-single-sign-on-non-gallery-applications.md)
* [Parola tabanlı tek oturum açma](configure-password-single-sign-on-non-gallery-applications.md)
* [Bağlantılı oturum açma yı yapılandırma](configure-linked-sign-on.md)
* [Uygulamalara Erişimi Yönetmeye Giriş](what-is-access-management.md)
* İndirme bağlantısı: [Tek oturum açma dağıtım planı.](https://aka.ms/SSODeploymentPlan)
