---
title: Uygulamalarda çoklu oturum açma-Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) içinde uygulama yapılandırırken çoklu oturum açma yöntemi seçme hakkında bilgi edinin. Kullanıcıların her uygulama için parolaları hatırlamaları ve hesap yönetiminin yönetimini basitleştirmek için çoklu oturum açma kullanın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79366079"
---
# <a name="single-sign-on-to-applications-in-azure-active-directory"></a>Azure Active Directory uygulamalarda çoklu oturum açma

Çoklu oturum açma (SSO), kullanıcılar Azure Active Directory (Azure AD) uygulamalarında oturum açtığında güvenlik ve kolaylık sağlar. Bu makalede, çoklu oturum açma yöntemleri açıklanmakta ve uygulamalarınızı yapılandırırken en uygun SSO yöntemini seçmenize yardımcı olur.

- **Çoklu oturum açma ile**kullanıcılar, etki alanına katılmış cihazlara, şirket kaynaklarına, hizmet olarak yazılım (SaaS) uygulamalarına ve Web uygulamalarına erişmek için bir hesapla bir kez oturum açabilirler. Oturum açtıktan sonra Kullanıcı Office 365 portalından veya Azure AD Uygulamaps erişim panelinden uygulama başlatabilir. Yöneticiler kullanıcı hesabı yönetimini merkezileştirmek ve grup üyeliğine göre uygulamalara Kullanıcı erişimini otomatik olarak ekleyebilir veya kaldırabilir.

- **Çoklu oturum açma olmadan**, kullanıcıların uygulamaya özgü parolaları hatırlamaları ve her uygulamada oturum açması gerekir. BT personelinin, Office 365, Box ve Salesforce gibi her uygulama için Kullanıcı hesapları oluşturması ve güncelleştirmesi gerekir. Kullanıcıların parolalarını hatırlamaları ve her uygulamada oturum açma süresini harcaması gerekir.

## <a name="choosing-a-single-sign-on-method"></a>Çoklu oturum açma yöntemi seçme

Bir uygulamayı çoklu oturum açma için yapılandırmanın birkaç yolu vardır. Çoklu oturum açma yönteminin seçilmesi, uygulamanın kimlik doğrulaması için nasıl yapılandırıldığına bağlıdır.

- Bulut uygulamaları, çoklu oturum açma için OpenID Connect, OAuth, SAML, parola tabanlı, bağlantılı veya devre dışı Yöntemler kullanabilir. 
- Şirket içi uygulamalar, çoklu oturum açma için parola tabanlı, tümleşik Windows kimlik doğrulaması, üst bilgi tabanlı, bağlantılı veya devre dışı Yöntemler kullanabilir. Şirket içi seçimler, uygulamalar uygulama proxy 'Si için yapılandırıldığında çalışır.

Bu akış çizelgesi, hangi çoklu oturum açma yönteminin durumunuz için en uygun olduğuna karar vermenize yardımcı olur.

![Çoklu oturum açma yöntemi için karar akış çizelgesi](./media/what-is-single-sign-on/choose-single-sign-on-method-040419.png)

Aşağıdaki tablo, çoklu oturum açma yöntemlerini özetler ve daha fazla ayrıntı için bağlantılar sağlar.

| Çoklu oturum açma yöntemi | Uygulama türleri | Kullanılması gereken durumlar |
| :------ | :------- | :----- |
| [OpenID Connect ve OAuth](#openid-connect-and-oauth) | yalnızca bulut | Yeni bir uygulama geliştirirken OpenID Connect ve OAuth kullanın. Bu protokol uygulama yapılandırmasını basitleştirir, kullanımı kolay SDK 'lara sahiptir ve uygulamanızın MS Graf kullanmasını sağlar.
| [SAML](#saml-sso) | bulut ve şirket içi | OpenID Connect veya OAuth kullanmayan mevcut uygulamalar için mümkün olduğunda SAML 'yi seçin. SAML, SAML protokollerinden birini kullanarak kimlik doğrulaması yapan uygulamalar için geçerlidir.|
| [Parola tabanlı](#password-based-sso) | bulut ve şirket içi | Uygulamanın Kullanıcı adı ve parolayla kimlik doğrulaması yaparken parola tabanlı ' yı seçin. Parola tabanlı çoklu oturum açma, güvenli uygulama parola depolaması ve bir Web tarayıcısı uzantısı ya da mobil uygulama kullanarak yeniden oynatma imkanı sunar. Bu yöntem, uygulama tarafından sağlanmış olan oturum açma işlemini kullanır, ancak yöneticinin parolaları yönetmesine olanak sağlar. |
| [Bağlandı](#linked-sign-on) | bulut ve şirket içi | Uygulama, başka bir kimlik sağlayıcısı hizmetinde çoklu oturum açma için yapılandırıldığında, bağlantılı oturum açma seçeneğini belirleyin. Bu seçenek, uygulamaya çoklu oturum açma eklemez. Ancak, uygulama zaten Active Directory Federasyon Hizmetleri (AD FS) gibi başka bir hizmet kullanılarak uygulanmış çoklu oturum açma olabilir.|
| [Devre dışı](#disabled-sso) | bulut ve şirket içi | Uygulama çoklu oturum açma için yapılandırılmaya Hazırlanmadıysa, çoklu oturum açmayı devre dışı bırak seçeneğini belirleyin. Bu mod, uygulamayı oluştururken varsayılan moddur.|
| [Tümleşik Windows kimlik doğrulaması (ıWA)](#integrated-windows-authentication-iwa-sso) | yalnızca şirket içi | [Tümleşik Windows kimlik doğrulaması (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication)veya talep kullanan uygulamalar kullanan uygulamalar için IWA çoklu oturum açma seçeneğini belirleyin. IWA için uygulama proxy bağlayıcıları, kullanıcıların uygulama kimliğini doğrulamak için Kerberos kısıtlı temsilcisini (KCD) kullanır. |
| [Üst bilgi tabanlı](#header-based-sso) | yalnızca şirket içi | Uygulama kimlik doğrulaması için üst bilgiler kullandığında üst bilgi tabanlı çoklu oturum açma kullanın. Üst bilgi tabanlı çoklu oturum açma, Azure AD için PingAccess gerektirir. Uygulama proxy 'Si, kullanıcının kimliğini doğrulamak için Azure AD 'yi kullanır ve ardından bağlayıcı hizmeti üzerinden trafiği geçirir.  |

## <a name="openid-connect-and-oauth"></a>OpenID Connect ve OAuth

Yeni uygulamalar geliştirirken, birden çok cihaz platformunda uygulamanız için en iyi çoklu oturum açma deneyimi elde etmek için OpenID Connect ve OAuth gibi modern protokolleri kullanın. OAuth, kullanıcıların veya yöneticilerin [Microsoft Graph](/graph/overview)gibi korumalı kaynaklara [izin vermesini](configure-user-consent.md) sağlar. Uygulamanız için [SDK 'ları](../develop/reference-v2-libraries.md) benimsemeyi ve ayrıca uygulamanız [Microsoft Graph](/graph/overview)kullanmaya hazırız.

Daha fazla bilgi için bkz.

- [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md)
- [OpenID Connect 1.0](../develop/v2-protocols-oidc.md)
- [Microsoft Identity platform Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

## <a name="saml-sso"></a>SAML SSO

**SAML çoklu oturum açma**Ile Azure AD, kullanıcının Azure AD hesabını kullanarak uygulamanın kimliğini doğrular. Azure AD, oturum açma bilgilerini bir bağlantı protokolü aracılığıyla uygulamayla iletişim kurar. SAML tabanlı çoklu oturum açma sayesinde, SAML talepleriniz içinde tanımladığınız kurallara göre kullanıcıları belirli uygulama rolleriyle eşleyebilirsiniz.

Uygulama destekliyorsa SAML tabanlı çoklu oturum açma seçeneğini belirleyin.

SAML tabanlı çoklu oturum açma, bu protokollerden herhangi birini kullanan uygulamalar için desteklenir:

- SAML 2.0
- WS-Federation

SAML tabanlı çoklu oturum açma için bir SaaS uygulaması yapılandırmak üzere bkz. [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md). Ayrıca, hizmet olarak yazılım (SaaS) uygulamalarında, SAML tabanlı çoklu oturum açma yapılandırmasında size adım adım kılavuzluk eden [uygulamaya özgü bir öğretici](../saas-apps/tutorial-list.md) vardır.

Bir uygulamayı WS-Federation için yapılandırmak üzere, SAML tabanlı çoklu oturum açma için uygulamayı yapılandırmak üzere aynı kılavuzu izleyin, bkz. [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md). Uygulamayı Azure AD kullanacak şekilde yapılandırma adımında, WS-Federation uç noktası `https://login.microsoftonline.com/<tenant-ID>/wsfed`IÇIN Azure AD oturum açma URL 'sini değiştirmeniz gerekir.

SAML tabanlı çoklu oturum açma için şirket içi bir uygulamayı yapılandırmak için, bkz. [uygulama proxy 'si ile şirket içi uygulamalar Için SAML çoklu oturum açma](application-proxy-configure-single-sign-on-on-premises-apps.md).

SAML protokolü hakkında daha fazla bilgi için bkz. [Çoklu oturum açma SAML Protokolü](../develop/single-sign-on-saml-protocol.md).

## <a name="password-based-sso"></a>Parola tabanlı SSO

Parola tabanlı oturum açma sayesinde, kullanıcılar ilk kez erişirken Kullanıcı adı ve parola ile uygulama üzerinde oturum açabilirler. İlk oturum açma işleminden sonra Azure AD, uygulamaya Kullanıcı adı ve parola sağlar.

Parola tabanlı çoklu oturum açma, uygulama tarafından sunulan mevcut kimlik doğrulama işlemini kullanır. Bir uygulama için parola çoklu oturum açmayı etkinleştirdiğinizde, Azure AD uygulama için Kullanıcı adlarını ve parolaları toplayıp güvenli bir şekilde depolar. Kullanıcı kimlik bilgileri, dizinde şifrelenmiş bir durumda depolanır.

Şu durumlarda parola tabanlı çoklu oturum açma seçin:

- Bir uygulama SAML çoklu oturum açma protokolünü desteklemez.
- Bir uygulama, belirteç ve üst bilgiler yerine Kullanıcı adı ve parolayla kimlik doğrular.

Parola tabanlı çoklu oturum açma, HTML tabanlı bir oturum açma sayfasına sahip olan tüm bulut tabanlı uygulamalar için desteklenir. Kullanıcı aşağıdaki tarayıcılardan herhangi birini kullanabilir:

- Windows 7 veya sonraki sürümlerde Internet Explorer 11
   > [!NOTE]
   > Internet Explorer sınırlı desteğe sahiptir ve artık yeni yazılım güncelleştirmeleri almaz. Microsoft Edge önerilen tarayıcıdır.

- Windows 10 yıldönümü sürümü veya sonraki sürümlerde Microsoft Edge
- İOS ve Android için Microsoft Edge
- Intune Yönetilen Tarayıcı
- Windows 7 veya üzeri cihazlarda ve MacOS X veya üzeri cihazlarda Chrome
- Windows XP SP2 veya sonraki sürümlerde Firefox 26,0 veya üzeri ve Mac OS X 10,6 ya da üzeri

Parola tabanlı çoklu oturum açma için bir bulut uygulaması yapılandırmak üzere, bkz. [parola çoklu oturum açmayı yapılandırma](configure-password-single-sign-on-non-gallery-applications.md).

Şirket içi bir uygulamayı uygulama proxy 'Si aracılığıyla çoklu oturum açma için yapılandırmak için bkz. [uygulama proxy 'si ile çoklu oturum açma Için parola](application-proxy-configure-single-sign-on-password-vaulting.md) oluşturma

### <a name="how-authentication-works-for-password-based-sso"></a>Kimlik doğrulama, parola tabanlı SSO için nasıl kullanılır

Bir kullanıcının bir uygulamada kimliğini doğrulamak için Azure AD, kullanıcının kimlik bilgilerini dizinden alır ve uygulamanın oturum açma sayfasına girer.  Azure AD, Kullanıcı kimlik bilgilerini bir Web tarayıcısı uzantısı veya mobil uygulama aracılığıyla güvenli bir şekilde geçirir. Bu işlem, yöneticinin kullanıcı kimlik bilgilerini yönetmesine olanak sağlar ve kullanıcıların parolalarını anımsamasını gerektirmez.

> [!IMPORTANT]
> Otomatik oturum açma işlemi sırasında kimlik bilgileri Kullanıcı tarafından karartılmış hale getirilebilir. Ancak, kimlik bilgileri Web hata ayıklama araçları kullanılarak bulunabilir. Kullanıcıların ve yöneticilerin kimlik bilgileri doğrudan Kullanıcı tarafından girilmiş gibi aynı güvenlik ilkelerini izlemesi gerekir.

### <a name="managing-credentials-for-password-based-sso"></a>Parola tabanlı SSO için kimlik bilgilerini yönetme

Her uygulama için parola, Azure AD yöneticisi veya kullanıcıları tarafından yönetilebilir.

Azure AD yöneticisi kimlik bilgilerini yönettiğinde:  

- Kullanıcının Kullanıcı adını ve parolayı sıfırlaması veya anımsaması gerekmez. Kullanıcı, erişim panelinde veya bir belirtilen bağlantı aracılığıyla uygulamaya tıklayarak uygulamaya erişebilir.
- Yönetici, kimlik bilgileri üzerinde yönetim görevleri gerçekleştirebilir. Örneğin, yönetici, uygulama erişimini Kullanıcı grubu üyeliklerine ve çalışan durumuna göre güncelleştirebilir.
- Yönetici, birçok kullanıcı arasında paylaşılan uygulamalara erişim sağlamak için yönetici kimlik bilgilerini kullanabilir. Örneğin, yönetici bir uygulamaya erişebilen herkese, sosyal medya veya belge paylaşım uygulamasına erişim izni verebilir.

Son Kullanıcı kimlik bilgilerini yönettiğinde:

- Kullanıcılar, gerektiğinde bunları güncelleştirerek veya silerek parolalarını yönetebilir.
- Yöneticiler yine de uygulama için yeni kimlik bilgileri ayarlayabiliyor.

## <a name="linked-sign-on"></a>Bağlantılı oturum açma
Bağlantılı oturum açma, Azure AD 'nin, zaten başka bir hizmette çoklu oturum açma için yapılandırılmış bir uygulamaya çoklu oturum açma imkanı sağlamasına olanak sağlar. Bağlantılı uygulama, Office 365 portalındaki veya Azure AD Uygulamaps portalındaki son kullanıcılara görünebilir. Örneğin, bir Kullanıcı Office 365 portalından Active Directory Federasyon Hizmetleri (AD FS) 2,0 ' de (AD FS) çoklu oturum açma için yapılandırılmış bir uygulamayı başlatabilir. Ayrıca, Office 365 portalından veya Azure AD Uygulamaps portalından başlatılan bağlantılı uygulamalar için ek raporlama da kullanılabilir. Bir uygulamayı bağlantılı oturum açma için yapılandırmak üzere bkz. [bağlı oturum açmayı yapılandırma](configure-linked-sign-on.md).

### <a name="linked-sign-on-for-application-migration"></a>Uygulama geçişi için bağlantılı oturum açma

Bağlantılı oturum açma, uygulamaları bir süre boyunca geçirirken tutarlı bir kullanıcı deneyimi sağlayabilir. Uygulamaları Azure Active Directory geçiriyorsanız, geçiş yapmak istediğiniz tüm uygulamaların bağlantılarını hızlıca yayımlamak için bağlantılı oturum açma ' yı kullanabilirsiniz.  Kullanıcılar, [Uygulamaps portalındaki](../user-help/active-directory-saas-access-panel-introduction.md) veya [Office 365 uygulama başlatıcısı](https://support.office.com/article/meet-the-office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a)'ndaki tüm bağlantıları bulabilir. Kullanıcılar, bağlı bir uygulamaya veya geçirilen uygulamaya erişeceğimizi bilmez.  

Bir kullanıcının kimliği, bağlantılı bir uygulamayla doğrulandıktan sonra, Son Kullanıcı çoklu oturum açma erişimi sağlanmadan önce bir hesap kaydının oluşturulması gerekir. Bu hesap kaydının sağlanması otomatik olarak yapılabilir veya bir yönetici tarafından el ile gerçekleşebilir.

## <a name="disabled-sso"></a>Devre dışı SSO

Devre dışı modu, uygulama için çoklu oturum açma kullanılmaz. Çoklu oturum açma devre dışı bırakıldığında, kullanıcıların iki kez kimlik doğrulaması yapması gerekebilir. İlk olarak, kullanıcılar Azure AD 'de kimlik doğrulaması yapabilir ve ardından uygulamada oturum açabilirler.

Devre dışı çoklu oturum açma modunu kullan:

- Bu uygulamayı Azure AD çoklu oturum açma ile tümleştirmeye hazırsanız veya
- Uygulamanın diğer yönlerini test ediyorsanız veya
- Kullanıcıların kimlik doğrulaması yapmasını gerektirmeyen şirket içi bir uygulamaya yönelik güvenlik katmanı olarak. Devre dışı bırakıldığında, kullanıcının kimlik doğrulaması gerekir.

Uygulamayı SP tarafından başlatılan SAML tabanlı çoklu oturum açma için yapılandırdıysanız ve SSO modunu devre dışı olarak değiştirirseniz, kullanıcıların Uygulamaps portalının dışında uygulamaya kaydolmasını engellemez. Bunu başarmak için [kullanıcıların oturum açabilme özelliğini devre dışı bırakmanız](disable-user-sign-in-portal.md) gerekir

## <a name="integrated-windows-authentication-iwa-sso"></a>Tümleşik Windows kimlik doğrulaması (ıWA) SSO 'SU

[Uygulama proxy 'si](application-proxy.md) , [Tümleşik Windows kimlik doğrulaması (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication)veya talep kullanan uygulamalar kullanan uygulamalar için çoklu oturum açma (SSO) sağlar. Uygulamanız ıWA kullanıyorsa, uygulama proxy 'Si, Kerberos kısıtlanmış temsili (KCD) kullanarak uygulamaya kimlik doğrular. Azure Active Directory güvenen talep kullanan bir uygulama için, kullanıcının kimliği zaten Azure AD kullanarak doğruladığından çoklu oturum açma işlemi çalışmaktadır.

IWA ile kimlik doğrulayan şirket içi bir uygulamada çoklu oturum açma sağlamak için tümleşik Windows kimlik doğrulaması çoklu oturum açma modu ' nu seçin.

IWA için şirket içi bir uygulamayı yapılandırmak için bkz. [uygulama proxy 'si ile uygulamalarınızda çoklu oturum açma Için Kerberos kısıtlanmış temsili](application-proxy-configure-single-sign-on-with-kcd.md).

### <a name="how-single-sign-on-with-kcd-works"></a>KCD ile çoklu oturum açma nasıl işe yarar?
Bu diyagramda, bir Kullanıcı ıWA kullanan bir şirket içi uygulamaya eriştiğinde akış açıklanır.

![Microsoft Azure AD kimlik doğrulaması akış diyagramı](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. Kullanıcı, uygulama proxy 'si aracılığıyla şirket içi uygulamaya erişmek için URL 'YI girer.
1. Uygulama proxy 'Si, isteği Azure AD kimlik doğrulama hizmetleri 'ne ön kimlik doğrulamasından göre yeniden yönlendirir. Bu noktada, Azure AD çok faktörlü kimlik doğrulaması gibi geçerli kimlik doğrulama ve yetkilendirme ilkelerini uygular. Kullanıcı doğrulandıktan sonra, Azure AD bir belirteç oluşturur ve kullanıcıya gönderir.
1. Kullanıcı belirteci uygulama proxy 'sine geçirir.
1. Uygulama proxy 'Si belirteci doğrular ve belirteçten Kullanıcı asıl adını (UPN) alır. Daha sonra istek, UPN ve hizmet asıl adını (SPN), kimliği doğrulanmış bir güvenli kanal aracılığıyla bağlayıcıya gönderir.
1. Bağlayıcı, şirket içi AD ile Kerberos kısıtlı temsilcisi (KCD) anlaşması kullanır ve uygulamaya bir Kerberos belirteci almak için kullanıcıyı taklit edin.
1. Active Directory, uygulamanın Kerberos belirtecini bağlayıcıya gönderir.
1. Bağlayıcı, AD 'den aldığı Kerberos belirtecini kullanarak özgün isteği uygulama sunucusuna gönderir.
1. Uygulama, yanıtı bağlayıcıya gönderir ve ardından uygulama proxy 'Si hizmetine ve son olarak kullanıcıya döndürülür.

## <a name="header-based-sso"></a>Üst bilgi tabanlı SSO

Üst bilgi tabanlı çoklu oturum açma, kimlik doğrulaması için HTTP üst bilgileri kullanan uygulamalar için geçerlidir. Bu oturum açma yöntemi, PingAccess adlı bir üçüncü taraf kimlik doğrulama hizmeti kullanır. Kullanıcının yalnızca Azure AD kimlik doğrulaması yapması gerekir.

Uygulama proxy 'Si ve PingAccess uygulama için yapılandırıldığında üst bilgi tabanlı çoklu oturum açma seçeneğini belirleyin.

Üst bilgi tabanlı kimlik doğrulamasını yapılandırmak için bkz. [uygulama proxy 'si ile çoklu oturum açma Için üst bilgi tabanlı kimlik doğrulama](application-proxy-configure-single-sign-on-with-ping-access.md).

### <a name="what-is-pingaccess-for-azure-ad"></a>Azure AD için PingAccess nedir?

Kullanıcılar, Azure AD için PingAccess 'i kullanarak, kimlik doğrulama için üst bilgileri kullanan uygulamalara erişebilir ve çoklu oturum açabilirler. Uygulama proxy 'Si, erişimi doğrulamak ve bağlayıcı hizmeti üzerinden trafiği geçirmek için Azure AD kullanarak bu uygulamaları başka bir şekilde değerlendirir. Kimlik doğrulaması gerçekleştikten sonra, PingAccess hizmeti, Azure AD erişim belirtecini uygulamaya gönderilen bir üst bilgi biçimine dönüştürür.

Kullanıcılarınız şirket uygulamalarınızı kullanmak için oturum açtıklarında farklı hiçbir şey yapmaz. Herhangi bir cihazdaki herhangi bir yerden çalışmaya devam edebilirler. Uygulama proxy bağlayıcıları, uzak trafiği tüm uygulamalara yönlendirirler ve dengeyi otomatik olarak yüklemeye devam eder.

### <a name="how-do-i-get-a-license-for-pingaccess"></a>PingAccess için lisans al Nasıl yaparım??

Bu senaryo Azure AD ve PingAccess arasındaki bir iş ortaklığı aracılığıyla sunulduğundan, her iki hizmet için de lisanslarına ihtiyacınız vardır. Ancak Azure AD Premium abonelikler, 20 ' ye kadar uygulamayı kapsayan temel bir PingAccess lisansını içerir. 20 ' den fazla üst bilgi tabanlı uygulama yayımlamanız gerekiyorsa, PingAccess 'ten ek bir lisans edinebilirsiniz.

Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md).

## <a name="related-articles"></a>İlgili makaleler:
* [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](../saas-apps/tutorial-list.md)
* [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
* [Parola tabanlı çoklu oturum açmayı yapılandırma](configure-password-single-sign-on-non-gallery-applications.md)
* [Bağlı oturum açmayı yapılandırma](configure-linked-sign-on.md)
* [Uygulamalara erişimi yönetmeye giriş](what-is-access-management.md)
* İndirme bağlantısı: [Çoklu oturum açma dağıtım planı](https://aka.ms/SSODeploymentPlan).
