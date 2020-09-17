---
title: Uygulamanızı Azure AD Uygulama Galerisi 'nde listeleyin | Microsoft Docs
description: Azure Active Directory Uygulama galerisinde çoklu oturum açmayı destekleyen bir uygulamayı nasıl listeleyeceğinizi öğrenin
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: how-to
ms.workload: identity
ms.date: 08/20/2020
ms.author: ryanwi
ms.reviewer: jeedes
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 40d299049a0a1bbff81c412a5beb84f28e64ff46
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706311"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Azure Active Directory uygulama galerisinde uygulamanızı listeleme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Bu makalede, Azure Active Directory (Azure AD) uygulama galerisinde bir uygulamayı listeleme, çoklu oturum açma (SSO) uygulama ve listeyi yönetme işlemlerinin nasıl yapılacağı gösterilir.

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD Uygulama Galerisi nedir?

- Müşteriler olası en iyi çoklu oturum açma deneyimini bulur.
- Uygulamanın yapılandırması basit ve en düşüktür.
- Hızlı arama, uygulamanızı galeride bulur.
- Ücretsiz, temel ve Premium Azure AD müşterilerinin hepsi bu tümleştirmeyi kullanabilir.
- Karşılıklı müşteriler, adım adım bir yapılandırma öğreticisini alır.
- Etki alanları arası kimlik yönetimi ([SCIM](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010)) için sistemi kullanan müşteriler aynı uygulama için sağlama kullanabilir.

## <a name="prerequisites"></a>Önkoşullar

- Federasyon uygulamaları (açık KIMLIK ve SAML/WS-beslenir) için, uygulamanın Azure AD uygulama galerisinde listelenen hizmet olarak yazılım (SaaS) modelini desteklemesi gerekir. Kurumsal Galeri uygulamaları, belirli bir müşteriyi değil birden çok müşteri yapılandırmasını desteklemelidir.
- Açık KIMLIK bağlantısı için, uygulamanın çok kiracılı olması ve [Azure AD onay çerçevesinin](../develop/consent-framework.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) uygulama için uygun şekilde uygulanması gerekir. Kullanıcı, herhangi bir müşterinin uygulamaya onay sağlayabilmesi için, oturum açma isteğini ortak bir uç noktaya gönderebilir. Kullanıcı erişimini, kiracı KIMLIĞINE ve Kullanıcı belirtecine göre belirteçte alınan UPN 'yi kontrol edebilirsiniz.
- SAML 2.0/WS-beslenir için, uygulamanızın SP veya ıDP modunda SAML/WS-Beslilik tümleştirmesi tümleştirme yeteneği olmalıdır. İsteği göndermeden önce bu özelliğin düzgün çalıştığından emin olun.
- Parola SSO 'SU için, uygulamanızın form kimlik doğrulamasını desteklediğinden emin olun ve bu sayede, tek oturum açma için beklenen şekilde çalışmaya başlayın.
- En az iki Kullanıcı kayıtlı olarak test için kalıcı bir hesaba ihtiyacınız vardır.

**Geliştiriciler için Azure AD nasıl alınır?**

Tüm Premium Azure AD özellikleriyle 90 gün ücretsiz olan ücretsiz bir test hesabı alabilir ve geliştirme yaparken sizinle birlikte çalışırken genişletilebilir. https://docs.microsoft.com/office/developer-program/office-365-developer-program

## <a name="submit-the-request-in-the-portal"></a>İsteği portalda gönder

Uygulama tümleştirmesinin Azure AD ile çalışıp çalışmadığını test ettikten sonra, uygulama isteğinizi [Microsoft Application Network portalında](https://microsoft.sharepoint.com/teams/apponboarding/Apps)gönderebilirsiniz.

Oturum açtıktan sonra aşağıdaki sayfa görünürse [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun. İsteği göndermek için kullanmak istediğiniz e-posta hesabını belirtin. Gibi bir iş e-posta adresi [name@yourbusiness.com](mailto:name@yourbusiness.com) tercih edilir. Azure AD ekibi, hesabı Microsoft uygulama ağı portalına ekler.

![SharePoint portalındaki erişim isteği iletisi](./media/howto-app-gallery-listing/errorimage.png)

Hesap eklendikten sonra, Microsoft uygulama ağı portalında oturum açabilirsiniz.

Oturum açtıktan sonra aşağıdaki sayfa görünürse, metin kutusuna erişim gereksinimi için bir iş doğrulaması belirtin. Ardından **erişim iste**' yi seçin.

  ![SharePoint portalındaki iş doğrulama kutusu](./media/howto-app-gallery-listing/accessrequest.png)

Ekibimiz ayrıntıları inceler ve size buna göre erişim sağlar. İsteğiniz onaylandıktan sonra, portalda oturum açabilir ve giriş sayfasındaki **Isteği gönder (ISV)** kutucuğunu seçerek isteği gönderebilirsiniz.

![Giriş sayfasında Istek (ISV) kutucuğunu gönder](./media/howto-app-gallery-listing/homepage.png)

## <a name="issues-on-logging-into-portal"></a>Portalda oturum açma sorunları

Oturum açarken bu hatayı görüyorsanız, sorun hakkında ayrıntılı bilgi ve bu sorunu nasıl giderebilirim?

* Oturum açma bilgileriniz aşağıda gösterildiği gibi engellenmişse:

  ![Galerideki uygulamayı çözümleyen sorunlar](./media/howto-app-gallery-listing/blocked.png)

**Ne oluyor:**

Konuk Kullanıcı aynı zamanda bir Azure AD olan bir giriş kiracıya federe olur. Konuk Kullanıcı yüksek risk altında. Microsoft, yüksek riskli kullanıcıların kaynaklarına erişmelerine izin vermez. Tüm yüksek riskli kullanıcılar (çalışanlar veya konuklar/satıcılar) Microsoft kaynaklarına erişmek için riskleri düzeltmeli/kapatacaktır. Konuk kullanıcılar için bu kullanıcı riski, ana kiracıdan gelir ve ilke, kaynak kiracısından gelir (Bu durumda Microsoft).
 
**Güvenli çözümler:**

* MFA kayıtlı Konuk kullanıcılar kendi Kullanıcı riskini düzeltir. Bu işlem, Konuk Kullanıcı tarafından güvenli bir parola değişikliği veya sıfırlama (ana https://aka.ms/sspr) KIRACıSıNDA MFA ve SSPR gerekir) gerçekleştiriliyor tarafından yapılabilir. Güvenli parola değiştirme veya sıfırlama özelliği, Azure AD 'de başlatılmalıdır ve şirket içi değil.

* Konuk kullanıcıların yöneticileri, riskini düzeltir. Bu durumda, yönetici bir parola sıfırlama işlemi gerçekleştirir (geçici parola oluşturma). Bu, kimlik koruması gerektirmez. Konuk kullanıcının Yöneticisi ' ne gidebilir https://aka.ms/RiskyUsers ve ' Parolayı Sıfırla ' öğesine tıklayabilirsiniz.

* Konuk kullanıcıların yöneticileri, riskini kapatır/kapatabilir. Bu, kimlik korumasına gerek kalmaz. Yönetici öğesine gidebilir https://aka.ms/RiskyUsers ve ' Kullanıcı riskini Kapat ' seçeneğine tıklayabilir. Ancak, bu, Kullanıcı riskini kapatmadan önce bunun yanlış bir pozitif risk değerlendirmesi olduğundan emin olmak için yöneticinin, süresi dolan bir süre olması gerekir. Aksi takdirde, araştırma yapmadan risk değerlendirmesi ortadan kaldırarak ve Microsoft 'un kaynaklarını riske sokuyor.

> [!NOTE]
> Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="implement-sso-by-using-the-federation-protocol"></a>Federasyon protokolünü kullanarak SSO 'yu uygulama

Azure AD uygulama galerisinde bir uygulamayı listelemek için, önce Azure AD tarafından desteklenen aşağıdaki Federasyon protokollerinden birini uygulamanız gerekir. Ayrıca Azure AD Uygulama Galerisi hüküm ve koşullarını kabul etmeniz gerekir. [Bu Web sitesindeki](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/)Azure AD uygulama galerisinin hüküm ve koşullarını okuyun.

- **OpenID Connect**: Open ID Connect protokolünü kullanarak UYGULAMANıZı Azure AD ile tümleştirmek için [geliştiricilerin yönergelerini](v1-authentication-scenarios.md)izleyin.

    ![Galerideki bir OpenID Connect uygulamasını listeleme](./media/howto-app-gallery-listing/openid.png)

    * Uygulamanızı OpenID Connect kullanarak galerinin listesine eklemek istiyorsanız, gösterildiği gibi **OpenID connect & OAuth 2,0** ' ı seçin.
    * Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

- **Saml 2,0** veya **WS-BESLENIR**: uygulamanız SAML 2,0 ' yi destekliyorsa, [özel bir uygulama ekleme YÖNERGELERINI](../manage-apps/view-applications-portal.md)izleyerek doğrudan bir Azure AD kiracısı ile tümleştirebilirsiniz.

  ![Bir SAML 2,0 veya WS-beslenir uygulamasını galeride listeleme](./media/howto-app-gallery-listing/saml.png)

  * **Saml 2,0** veya **WS-besu**kullanarak uygulamanızı Galeriye eklemek Istiyorsanız, gösterildiği gıbı **SAML 2.0/WS-beslenir** ' i seçin.

  * Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="implement-sso-by-using-the-password-sso"></a>SSO parolasını kullanarak SSO uygulama

[Parola tabanlı çoklu oturum açmayı](../manage-apps/what-is-single-sign-on.md)YAPıLANDıRMAK için HTML oturum açma sayfasına sahip bir Web uygulaması oluşturun. Parola oluşturma olarak da bilinen parola tabanlı SSO, Kimlik Federasyonu desteklemeyen Web uygulamalarına Kullanıcı erişimini ve parolalarını yönetmenizi sağlar. Ayrıca, çeşitli kullanıcıların, kuruluşunuzun sosyal medya uygulaması hesapları gibi tek bir hesabı paylaşması gereken senaryolar için de kullanışlıdır.

![Galerideki parola SSO uygulamasını listeleme](./media/howto-app-gallery-listing/passwordsso.png)

* Uygulamanızı parola SSO kullanarak galerideki listeye eklemek istiyorsanız, gösterilen şekilde **parola SSO (Kullanıcı adı & parola)** seçeneğini belirleyin.
* Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="request-for-user-provisioning"></a>Kullanıcı hazırlama isteği

Kullanıcı sağlaması istemek için aşağıdaki görüntüde gösterilen işlemi izleyin.

   ![Kullanıcı hazırlama isteği](./media/howto-app-gallery-listing/user-provisioning.png)

## <a name="update-or-remove-an-existing-listing"></a>Mevcut bir listeyi güncelleştirme veya kaldırma

Azure AD uygulama galerisinde mevcut bir uygulamayı güncelleştirmek veya kaldırmak için, önce isteği [uygulama ağ portalında](https://microsoft.sharepoint.com/teams/apponboarding/Apps)göndermeniz gerekir. Bir iş veya okul hesabınız varsa, Bu portalda oturum açmak için bu hesabı kullanın. Aksi takdirde, oturum açmak için Outlook veya Hotmail gibi Microsoft hesabı kullanın.

- Aşağıdaki görüntüde gösterildiği gibi uygun seçeneği belirleyin.

    ![Galerideki bir SAML uygulamasını listeleme](./media/howto-app-gallery-listing/updateorremove.png)

    * Mevcut bir uygulamayı güncelleştirmek için, gereksiniminize göre uygun seçeneği belirleyin.
    * Mevcut bir uygulamayı Azure AD Uygulama Galerisi 'nden kaldırmak için **galerideki uygulama listemi kaldır**' ı seçin.
    * Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="list-requests-by-customers"></a>İstekleri müşterilere göre Listele

Müşteriler, **müşterilerin**  >  **yeni istek göndermesi**için uygulama istekleri seçerek bir uygulamayı listeleme isteği gönderebilir.

![Müşteri tarafından istenen uygulamalar kutucuğunu gösterir](./media/howto-app-gallery-listing/customer-submit-request.png)

Müşteri tarafından istenen uygulamaların akışı aşağıda verilmiştir.

![Müşteri tarafından istenen uygulamalar akışını gösterir](./media/howto-app-gallery-listing/customerrequest.png)

## <a name="timelines"></a>Zaman çizelgeleri

Galerideki bir SAML 2,0 veya WS-Besme uygulamasını listeleme işleminin zaman çizelgesi 7 ila 10 iş gününe kadar olur.

  ![Galerideki bir SAML uygulamasını listelemek için zaman çizelgesi](./media/howto-app-gallery-listing/timeline.png)

Galerideki bir OpenID Connect uygulamasını listeleme işleminin zaman çizelgesi 2 ile 5 iş gününe kadar olur.

  ![Galerideki bir OpenID Connect uygulamasını listelemek için zaman çizelgesi](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Yükseltmeleri

Tüm yürüyen değişiklikler için [Azure AD SSO tümleştirme ekibine](mailto:SaaSApplicationIntegrations@service.microsoft.com) e-posta gönderin ve en SaaSApplicationIntegrations@service.microsoft.com kısa sürede yanıt vereceğiz.