---
title: Uygulamanızı Azure AD uygulama galerisinde listele | Microsoft Dokümanlar
description: Azure Active Directory uygulama galerisinde tek oturum açmayı destekleyen bir uygulamayı nasıl listeleyyin
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 12/06/2019
ms.author: ryanwi
ms.reviewer: jeedes
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 969193b2c0843c4eb217d2bdb9f9ad9a40ccf9af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154976"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Azure Active Directory uygulama galerisinde uygulamanızı listeleme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Bu makalede, Azure Etkin Dizin (Azure AD) uygulama galerisinde bir uygulamanın nasıl listelenebildiğini, tek oturum açma (SSO) nasıl uygulanacağını ve girişi nasıl yönetiştiracağı gösterilmektedir.

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisi nedir?

- Müşteriler mümkün olan en iyi tek oturum açma deneyimini bulurlar.
- Uygulamanın yapılandırması basit ve minimaldir.
- Hızlı bir arama, uygulamanızı galeride bulur.
- Ücretsiz, Temel ve Premium Azure REKLAM müşterilerinin tümü bu tümleştirmeyi kullanabilir.
- Ortak müşteriler adım adım yapılandırma öğretici olsun.
- Etki alanları arası Kimlik Yönetimi[Sistemi'ni (SCIM)](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010)kullanan müşteriler, aynı uygulama için sağlama yı kullanabilir.

## <a name="prerequisites"></a>Ön koşullar

- Federe uygulamalar (Open ID ve SAML/WS-Fed) için uygulamanın Azure AD uygulama galerisinde listelenemesi için hizmet olarak yazılım (SaaS) modelini desteklemesi gerekir. Kurumsal galeri uygulamaları, belirli bir müşteriyi değil, birden çok müşteri yapılandırmasını desteklemelidir.
- Open ID Connect için uygulamanın çok kiracılı olması ve uygulama için [Azure AD onay çerçevesinin](../develop/consent-framework.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) düzgün şekilde uygulanması gerekir. Kullanıcı, herhangi bir müşterinin uygulamaya onay verebilmesi için oturum açma isteğini ortak bir bitiş noktasına gönderebilir. Kullanıcı erişimini kiracı kimliğine ve kullanıcının belirteçte alınan UPN'ine göre denetleyebilirsiniz.
- SAML 2.0/WS-Fed için, uygulamanızın SamL/WS-Fed SSO entegrasyonunu SP veya IDP modunda yapabilme yeteneğine sahip olması gerekir. İsteğe başvurmadan önce bu özelliğin doğru çalıştığından emin olun.
- Parola SSO için, parola atlamanın beklendiği gibi çalışması için tek oturum açma yapabilmesi için başvurunuzun form kimlik doğrulamasını desteklediğinden emin olun.
- Kayıtlı en az iki kullanıcıyla sınama için kalıcı bir hesaba ihtiyacınız vardır.

**Geliştiriciler için Azure AD nasıl edinilir?**

Tüm premium Azure AD özellikleriyle ücretsiz bir test hesabı alabilirsiniz - 90 gün ücretsiz ve onunla iş yaptığınız sürece uzatılabilir:https://docs.microsoft.com/office/developer-program/office-365-developer-program

## <a name="submit-the-request-in-the-portal"></a>İsteği portalda gönderin

Uygulama tümleştirmenizin Azure AD ile çalıştığını test ettikten sonra, erişim isteğinizi [Uygulama Ağı portalına](https://microsoft.sharepoint.com/teams/apponboarding/Apps)gönderin. Office 365 hesabınız varsa, bu portalda oturum açabilmek için bunu kullanın. Değilse, oturum açabilmek için Outlook veya Hotmail gibi Microsoft hesabınızı kullanın.

Oturum açmadan sonra aşağıdaki sayfa görünürse, [Azure AD SSO Tümleştirme Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun. İsteğe yönelik göndermek için kullanmak istediğiniz e-posta hesabını sağlayın. Azure AD ekibi hesabı Microsoft Application Network portalına ekler.

![SharePoint portalında istek iletisi erişim](./media/howto-app-gallery-listing/errorimage.png)

Hesap eklendikten sonra Microsoft Application Network portalında oturum açabilirsiniz.

Oturum açmadan sonra aşağıdaki sayfa görünürse, metin kutusuna erişim gereksinimi için bir iş gerekçesi sağlayın. Ardından **Erişim İste'yi**seçin.

  ![SharePoint portalında iş yaslama kutusu](./media/howto-app-gallery-listing/accessrequest.png)

Ekibimiz detayları gözden geçirir ve size buna göre erişim sağlar. İsteğiniz onaylandıktan sonra, portalda oturum açabilir ve ana sayfadaki **İstek Gönder (ISV)** döşemesini seçerek isteğinizi iletebilirsiniz.

![Giriş sayfasında İstek Gönder (ISV) döşemesi](./media/howto-app-gallery-listing/homepage.png)

## <a name="issues-on-logging-into-portal"></a>Portala giriş le ilgili sorunlar

Oturum açarken bu hatayı görüyorsanız, sorunla ilgili ayrıntılar aşağıda ve bunu nasıl düzeltebileceğiniz aşağıda açıklanmıştır.

* Oturum açma nız aşağıda gösterildiği gibi engellendiyse:

  ![galeride uygulama çözme sorunları](./media/howto-app-gallery-listing/blocked.png)

**Ne oluyor:**

Konuk kullanıcı, aynı zamanda bir Azure AD olan bir ev kiracısına federe edilir. Konuk kullanıcı yüksek risk altındadır. Microsoft, Yüksek riskli kullanıcıların kaynaklarına erişmesine izin vermez. Tüm Yüksek riskli kullanıcılar (çalışanlar veya misafirler / satıcılar) Microsoft kaynaklarına erişmek için risklerini düzeltmelidir / kapatmalıdır. Konuk kullanıcılar için bu kullanıcı riski ev kiracısından gelir ve ilke kaynak kiracısından (bu durumda Microsoft) gelir.
 
**Güvenli çözümler:**

* MFA kayıtlı konuk kullanıcılar kendi kullanıcı risklerini düzeltiyor. Bu güvenli bir şifre değişikliği veya sıfırlama (ev kiracıhttps://aka.ms/sspr) (bu ev kiracı mfa ve SSPR ihtiyacı) gerçekleştiren konuk kullanıcı tarafından yapılabilir. Güvenli parola değişikliği veya sıfırlama işlemi ön-prem'de değil, Azure AD'de başlatılmalıdır.

* Konuk kullanıcıların yöneticileri risklerini düzeltiyor. Bu durumda, yönetici bir parola sıfırlama (geçici parola oluşturma) gerçekleştirecektir. Bunun kimlik koruması gerektirmemesi gerekmez. Konuk kullanıcının yöneticisi 'Parolayı Sıfırla'ya https://aka.ms/RiskyUsers gidebilir ve tıklayabilir.

* Konuk kullanıcıların yöneticileri yakın / risklerini göz ardı. Yine, bu Kimlik Koruması gerekmez. Yönetici 'Kullanıcı https://aka.ms/RiskyUsers riskini düşür' seçeneğini tıklayabilir. Ancak, yönetici kullanıcı riskini kapatmadan önce bunun yanlış bir pozitif risk değerlendirmesi olduğundan emin olmak için durum tespitini yapmalıdır. Aksi takdirde, araştırma yapmadan bir risk değerlendirmesini bastırarak kendi ve Microsoft'un kaynaklarını riske atmaktadırlar.

> [!NOTE]
> Erişimle ilgili herhangi bir sorun varsa, [Azure AD SSO Tümleştirme Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="implement-sso-by-using-the-federation-protocol"></a>Federasyon protokolünü kullanarak SSO'yu uygulayın

Bir uygulamayı Azure AD uygulama galerisinde listelemek için öncelikle Azure AD tarafından desteklenen aşağıdaki federasyon protokollerinden birini uygulamanız gerekir. Azure AD uygulama galerisi hüküm ve koşullarını da kabul etmeniz gerekir. Azure AD uygulama galerisinin hüküm ve koşullarını [bu web sitesinde](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/)okuyun.

- **OpenID Connect**: Open ID Connect protokolünü kullanarak uygulamanızı Azure AD ile tümleştirmek için [geliştiricilerin talimatlarına](v1-authentication-scenarios.md)uyun.

    ![Galeride bir OpenID Connect uygulamasını listeleme](./media/howto-app-gallery-listing/openid.png)

    * OpenID Connect'i kullanarak uygulamanızı galerideki listeye eklemek istiyorsanız, gösterildiği gibi **OpenID Connect & OAuth 2.0'ı** seçin.
    * Erişimle ilgili herhangi bir sorun varsa, [Azure AD SSO Tümleştirme Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

- **SAML 2.0** veya **WS-Fed**: Uygulamanız SAML 2.0'ı destekliyorsa, [özel bir uygulama eklemek için yönergeleri](../active-directory-saas-custom-apps.md)izleyerek uygulamayı doğrudan bir Azure AD kiracısıyla tümleştirebilirsiniz.

  ![Galeride SAML 2.0 veya WS-Fed uygulamasını listeleme](./media/howto-app-gallery-listing/saml.png)

  * Saml **2.0** veya **WS-Fed**kullanarak uygulamanızı galerideki listeye eklemek istiyorsanız, gösterildiği gibi **SAML 2.0/WS-Fed'i** seçin.

  * Erişimle ilgili herhangi bir sorun varsa, [Azure AD SSO Tümleştirme Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="implement-sso-by-using-the-password-sso"></a>SSO parolasını kullanarak SSO'yu uygulayın

[Parola tabanlı tek oturum](../manage-apps/what-is-single-sign-on.md)açma yapılandırmak için HTML oturum açma sayfası olan bir web uygulaması oluşturun. Parola atlama olarak da adlandırılan parola tabanlı SSO, kimlik federasyonunu desteklemeyen web uygulamalarına kullanıcı erişimini ve parolalarını yönetmenize olanak tanır. Ayrıca, birden çok kullanıcının kuruluşunuzun sosyal medya uygulama hesapları gibi tek bir hesabı paylaşması gereken senaryolar için de yararlıdır.

![Galeride bir parola SSO uygulamasını listeleme](./media/howto-app-gallery-listing/passwordsso.png)

* Uygulamanızı galerideki listeye sso parolasını kullanarak eklemek istiyorsanız, gösterildiği gibi **Parola SSO'yu** seçin.
* Erişimle ilgili herhangi bir sorun varsa, [Azure AD SSO Tümleştirme Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="request-for-user-provisioning"></a>Kullanıcı sağlama talebi

Kullanıcı sağlama isteğinde bulunmak için aşağıdaki resimde gösterilen işlemi izleyin.

   ![Kullanıcı sağlama talebi](./media/howto-app-gallery-listing/user-provisioning.png)

## <a name="update-or-remove-an-existing-listing"></a>Varolan bir girişi güncelleştirme veya kaldırma

Azure AD uygulama galerisindeki mevcut bir uygulamayı güncelleştirmek veya kaldırmak için öncelikle isteği [Uygulama Ağı portalına](https://microsoft.sharepoint.com/teams/apponboarding/Apps)göndermeniz gerekir. Office 365 hesabınız varsa, bu portalda oturum açabilmek için bunu kullanın. Değilse, oturum açabilmek için Outlook veya Hotmail gibi Microsoft hesabınızı kullanın.

- Aşağıdaki resimde gösterildiği gibi uygun seçeneği seçin.

    ![Galeride bir SAML uygulamasını listeleme](./media/howto-app-gallery-listing/updateorremove.png)

    * Varolan bir uygulamayı güncelleştirmek için gereksiniminize göre uygun seçeneği seçin.
    * Varolan bir uygulamayı Azure AD uygulama galerisinden kaldırmak **için, uygulama listemi galeriden kaldır'ı**seçin.
    * Erişimle ilgili herhangi bir sorun varsa, [Azure AD SSO Tümleştirme Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="list-requests-by-customers"></a>Müşterilere göre istekleri listele

Müşteriler, **Müşterilere** > Uygulama isteklerini seçerek bir uygulamayı listelemek için istek gönderebilirler**Yeni istek gönder.**

![İstenilen uygulamalar döşemesini gösterir](./media/howto-app-gallery-listing/customer-submit-request.png)

İşte müşteri tarafından istenen uygulamaların akışı.

![İstenilen uygulamalar akışını gösterir](./media/howto-app-gallery-listing/customerrequest.png)

## <a name="timelines"></a>Zaman çizelgeleri

Galeride bir SAML 2.0 veya WS-Fed uygulaması listeleme işlemi için zaman çizelgesi 7 ila 10 iş günüdür.

  ![Galeride bir SAML uygulamasını listelemek için zaman çizelgesi](./media/howto-app-gallery-listing/timeline.png)

Galeride bir OpenID Connect uygulamasını listeleme işleminin zaman çizelgesi 2 ila 5 iş günüdür.

  ![Galeride bir OpenID Connect uygulamasını listelemek için zaman çizelgesi](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Escalations

Herhangi bir artış için [Azure AD SSO Entegrasyon Ekibine](mailto:SaaSApplicationIntegrations@service.microsoft.com) e-posta gönderin SaaSApplicationIntegrations@service.microsoft.comve en kısa sürede yanıt verelim.
