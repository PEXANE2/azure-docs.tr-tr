---
title: Uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin | Microsoft Docs
description: Azure Active Directory Uygulama galerisinde çoklu oturum açmayı destekleyen bir uygulamayı nasıl listeleyeceğinizi öğrenin
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/06/2019
ms.author: ryanwi
ms.reviewer: elisol, bryanla
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1cf3eead3a56297efa3bea90ef48eb464ad63da0
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70898512"
---
# <a name="how-to-list-your-application-in-the-azure-active-directory-application-gallery"></a>Nasıl yapılır: Azure Active Directory uygulama galerisinde uygulamanızı listeleme

Bu makalede, Azure AD uygulama galerisinde bir uygulamayı listeleme, çoklu oturum açma (SSO) uygulama ve listeyi yönetme işlemlerinin nasıl yapılacağı gösterilir.

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD Uygulama Galerisi nedir?

- Müşteriler olası en iyi çoklu oturum açma deneyimini bulur.
- Uygulamanın yapılandırması basit ve en düşüktür.
- Hızlı arama, uygulamanızı galeride bulur.
- Ücretsiz, temel ve Premium Azure AD müşterilerinin hepsi bu tümleştirmeyi kullanabilir.
- Karşılıklı müşteriler, adım adım bir yapılandırma öğreticisini alır.
- SCıM kullanan müşteriler aynı uygulama için sağlama kullanabilir.

## <a name="prerequisites"></a>Önkoşullar

- Federasyon uygulamaları (açık KIMLIK ve SAML/WS-beslenir) için, uygulamanın Azure AD galerisinde listelenen SaaS modelini desteklemesi gerekir. Kurumsal Galeri uygulamaları, belirli bir müşteriyi değil birden çok müşteri yapılandırmasını desteklemelidir.

- Açık KIMLIK bağlantısı için, uygulamanın çok kiracılı olması ve [Azure AD onay çerçevesinin](consent-framework.md) uygulama için düzgün şekilde uygulanması gerekir. Kullanıcı, herhangi bir müşterinin uygulamaya onay sağlayabilmesi için oturum açma isteğini ortak bir uç noktaya gönderebilir. Kullanıcı erişimini, kiracı KIMLIĞINE ve Kullanıcı belirtecine göre belirteçte alınan UPN 'yi kontrol edebilirsiniz.

- SAML 2.0/WS-beslenir için, uygulamanızın SP veya ıDP modunda SAML/WS-Beslilik Tümleştirmesi özelliğini yapması gerekir. Lütfen isteği göndermeden önce bunun doğru çalıştığından emin olun.

- Parola SSO 'SU için, lütfen uygulamanızın form kimlik doğrulamasını desteklediğinden emin olun, böylelikle çoklu oturum açma işinin beklendiği gibi çalışmasını sağlayabilirsiniz.

- Otomatik Kullanıcı sağlama istekleri için, uygulamanın SAML 2.0/WS-beslenir kullanılarak etkinleştirilmiş çoklu oturum açma özelliği ile galeride listelenmesi gerekir. Zaten listede yoksa, portalda SSO ve Kullanıcı hazırlama işlemlerini bir araya getirebilirsiniz.

- En az 2 Kullanıcı kayıtlı olan sınama için kalıcı bir hesap gerekir.

> [!NOTE]
> Çok sayıda SCıM bağlayıcı isteği ile çalışıyoruz, bu nedenle portalda yeni istekleri almayı durdurduk. Lütfen daha fazla bildirimde bulununcaya kadar isteklerinizi tutun. Bu gecikme ve bunun neden olduğu rahatsızlıktan dolayı özür dileriz.

## <a name="submit-the-request-in-the-portal"></a>İsteği portalda gönder

Uygulama tümleştirmesinin Azure AD ile çalışıp çalışmadığını test ettikten sonra, [uygulama ağ](https://microsoft.sharepoint.com/teams/apponboarding/Apps)portalımızda erişim isteğinizi iletin. Office 365 hesabınız varsa, Bu portalda oturum açmak için bu hesabı kullanın. Aksi takdirde, oturum açmak için Microsoft hesabı (Outlook veya Hotmail gibi) kullanın.

Oturum açtıktan sonra aşağıdaki sayfa görünürse [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) başvurun ve isteği göndermek için kullanmak istediğiniz e-posta hesabını belirtin. Ardından Azure AD ekibi, hesabı Microsoft uygulama ağı portalına ekler.

![SharePoint portalında erişim Isteği](./media/howto-app-gallery-listing/errorimage.png)

Hesap eklendikten sonra, Microsoft uygulama ağı portalında oturum açabilirsiniz.

Oturum açtıktan sonra aşağıdaki sayfa görünürse, metin kutusuna erişim gereksinimi için bir iş doğrulaması belirtin ve ardından **erişim iste**' yi seçin.

  ![SharePoint portalında erişim Isteği](./media/howto-app-gallery-listing/accessrequest.png)

Ekibimiz ayrıntıları inceler ve size buna göre erişim sağlar. İsteğiniz onaylandıktan sonra, portalda oturum açabilir ve giriş sayfasına, **Isteği gönder (ISV)** kutucuğuna tıklayarak isteği gönderebilirsiniz.

![SharePoint Portal giriş sayfası](./media/howto-app-gallery-listing/homepage.png)

> [!NOTE]
> Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="implementing-sso-using-federation-protocol"></a>Federasyon protokolünü kullanarak SSO uygulama

Azure AD uygulama galerisinde bir uygulamayı listelemek için, önce Azure AD tarafından desteklenen aşağıdaki Federasyon protokollerinden birini uygulamanız ve Azure AD Uygulama Galerisi hüküm ve koşulları 'nı kabul etmeniz gerekir. Azure AD uygulama galerisinin hüküm ve koşullarını [buradan](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/)okuyun.

- **OpenID Connect**: Açık KIMLIK Connect protokolünü kullanarak uygulamanızı Azure AD ile tümleştirmek için, [geliştiricilerin yönergelerini](authentication-scenarios.md)izleyin.

    ![OpenID uygulamasının galeriye bir şekilde bağlanmasını listeleme zaman çizelgesi](./media/howto-app-gallery-listing/openid.png)

    * Uygulamanızı OpenID Connect kullanarak galerinin listesine eklemek isterseniz, **OpenID connect & OAuth 2,0** ' ı yukarıdaki gibi seçin.
    * Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun. 

- **SAML 2,0** veya **WS-beslenir**: Uygulamanız SAML 2,0 ' yi destekliyorsa, [özel bir uygulama eklemek için yönergeleri](../active-directory-saas-custom-apps.md)kullanarak doğrudan BIR Azure AD kiracısı ile tümleştirebilirsiniz.

  ![SAML 2,0 veya WS-beslenir uygulamasının galeriye listelenmesi için zaman çizelgesi](./media/howto-app-gallery-listing/saml.png)

  * Uygulamanızı **saml 2,0** veya **WS-besi**kullanarak galerinin listesine eklemek Istiyorsanız, yukarıdaki gıbı **SAML 2.0/WS-** besgu seçin.
  * Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="implementing-sso-using-password-sso"></a>Parola SSO kullanarak SSO uygulama

[Parola tabanlı çoklu oturum açmayı](../manage-apps/what-is-single-sign-on.md)YAPıLANDıRMAK için HTML oturum açma sayfasına sahip bir Web uygulaması oluşturun. Parola oluşturma olarak da bilinen parola tabanlı SSO, Kimlik Federasyonu desteklemeyen Web uygulamalarına Kullanıcı erişimini ve parolalarını yönetmenizi sağlar. Ayrıca, çeşitli kullanıcıların, kuruluşunuzun sosyal medya uygulaması hesapları gibi tek bir hesabı paylaşması gereken senaryolar için de kullanışlıdır.

![Parola SSO uygulamasının galeriye listelenmesi için zaman çizelgesi](./media/howto-app-gallery-listing/passwordsso.png)

* Uygulamanızı parola SSO 'SU kullanarak galerideki listeye eklemek istiyorsanız, yukarıdaki gibi **parola SSO** 'yu seçin.
* Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun.

## <a name="updateremove-existing-listing"></a>Mevcut listeyi Güncelleştir/Kaldır

Azure AD uygulama galerisinde mevcut bir uygulamayı güncelleştirmek veya kaldırmak için, önce isteği [uygulama ağ portalında](https://microsoft.sharepoint.com/teams/apponboarding/Apps)göndermeniz gerekir. Office 365 hesabınız varsa, Bu portalda oturum açmak için bu hesabı kullanın. Aksi takdirde, oturum açmak için Microsoft hesabı (Outlook veya Hotmail gibi) kullanın.

- Aşağıdaki görüntüde gösterildiği gibi uygun seçeneği belirleyin:

    ![SAML uygulamasının galeriye listelenmesi için zaman çizelgesi](./media/howto-app-gallery-listing/updateorremove.png)

    * Mevcut bir uygulamayı güncelleştirmek istiyorsanız, **var olan uygulama listesini güncelleştir**' i seçin.
    * Mevcut bir uygulamayı Azure AD Galerisi 'nden kaldırmak istiyorsanız, **var olan uygulama listesini kaldır**' ı seçin.
    * Erişim ile ilgili herhangi bir sorununuz varsa [Azure AD SSO tümleştirme ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)başvurun. 

## <a name="listing-requests-by-customers"></a>İstekleri müşterilere göre listeleme

Müşteriler, **müşterilerin** -> **yeni istek göndermesi**için uygulama istekleri ' ne tıklayarak bir uygulamayı listeleme isteği gönderebilir.

![Müşteri tarafından istenen uygulamalar kutucuğunu gösterir](./media/howto-app-gallery-listing/customer-submit-request.png)

Aşağıda, müşteri tarafından istenen uygulamaların akışı verilmiştir.

![Müşteri tarafından istenen uygulamalar akışını gösterir](./media/howto-app-gallery-listing/customer-request.png)

## <a name="timelines"></a>Zaman çizelgeleri

Galerideki bir SAML 2,0 veya WS-Besme uygulamasını listeleme işleminin zaman çizelgesi, 7-10 iş gündür.

   ![SAML uygulamasının galeriye listelenmesi için zaman çizelgesi](./media/howto-app-gallery-listing/timeline.png)

Galerideki bir OpenID Connect uygulamasını listeleme işleminin zaman çizelgesi 2-5 iş gündür.

   ![SAML uygulamasının galeriye listelenmesi için zaman çizelgesi](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Yükseltmeleri

Tüm yürüyen değişiklikler için [Azure AD SSO tümleştirme ekibine](mailto:SaaSApplicationIntegrations@service.microsoft.com) e-posta gönderin. Bu, SaaSApplicationIntegrations@service.microsoft.com en kısa sürede yanıt vereceğiz.
