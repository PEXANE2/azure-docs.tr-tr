---
title: Bir Uygulama Proxy uygulamasında tek oturum açma nasıl yapılandırılır?
description: Uygulama proxy uygulamanızda tek oturum açmayı hızlı bir şekilde nasıl yapılandırabilirsiniz?
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: mimart
ms.reviewer: japere, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 589b3e51f27147f0a0432b61c22a024c202e388b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76712024"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Bir Uygulama Proxy uygulamasına tek oturum açma nasıl yapılandırılır?

Tek oturum açma (SSO), kullanıcılarınızın birden çok kez kimlik doğrulamayapmadan bir uygulamaya erişmesine olanak tanır. Azure Active Directory'ye karşı tek kimlik doğrulamasının bulutta oluşmasına izin verir ve hizmetin veya Bağlayıcı'nın uygulamadan ek kimlik doğrulama zorluklarını tamamlamak için kullanıcının kimliğine bürünmesine olanak tanır.

## <a name="how-to-configure-single-sign-on"></a>Tek oturum açma üzerinde yapılandırma
SSO'yu yapılandırmak için öncelikle uygulamanızın Azure Etkin Dizini aracılığıyla Kimlik Öncesi Için yapılandırıldığından emin olun. Bu yapılandırmayı yapmak için **Azure Active Directory**  - &gt; **Enterprise Applications**  - &gt; **All Applications**  - &gt; Tüm Uygulamalar ** - &gt; Uygulama Nız A.Ş.'ye**gidin. Bu sayfada, "Kimlik Doğrulama Öncesi" alanını görürsünüz ve bunun "Azure Etkin Dizini" olarak ayarlandığınızdan emin olun. 

Kimlik Doğrulama Öncesi yöntemleri hakkında daha fazla bilgi için [uygulama yayımlama belgesinin](application-proxy-add-on-premises-application.md)adım 4'e bakın.

   ![Azure portalında ön kimlik doğrulama yöntemi](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Uygulama Proxy Uygulamaları için tek oturum açma modlarını yapılandırma
Belirli bir oturum açma türünü yapılandırın. Oturum açma yöntemleri, arka uç uygulamasının ne tür kimlik doğrulamasını kullandığına göre sınıflandırılır. App Proxy uygulamaları üç tür oturum açmayı destekler:

-   **Parola tabanlı Oturum Açma**: Oturum açmaiçin kullanıcı adı ve parola alanlarını kullanan tüm uygulamalarda parola tabanlı oturum açma kullanılabilir. Yapılandırma adımları, [Azure AD galerisi uygulaması için yapılandırma parolası Tek oturum](configure-password-single-sign-on-non-gallery-applications.md)açma'dadır.

-   **Tümleşik Windows Kimlik Doğrulama**: Tümleşik Windows Kimlik Doğrulama (IWA) kullanan uygulamalarda, Kerberos Kısıtlı Temsilciliği (KCD) aracılığıyla tek oturum açma etkinleştirilir. Bu yöntem, Active Directory'deki Uygulama Proxy Bağlayıcıları'na kullanıcıların kimliğine bürünme ve onlar adına belirteçler gönderme ve alma izni verir. KCD yapılandırma ile ilgili ayrıntılar [KCD belgeleri ile Tek Sign-On](application-proxy-configure-single-sign-on-with-kcd.md)bulunabilir.

-   **Üstbilgi tabanlı Oturum**Açma : Üstbilgi tabanlı oturum açma, bir ortaklık aracılığıyla etkinleştirilir ve bazı ek yapılandırma gerektirir. Ortaklık ve kimlik doğrulama için üstbilgileri kullanan bir uygulamada tek oturum açma yapılandırmaya yönelik adım adım yönergeler hakkında ayrıntılı bilgi için [Azure REKLAM belgeleri için PingAccess'e](application-proxy-configure-single-sign-on-with-ping-access.md)bakın.

-   **SAML tek oturum açma**: SAML tek oturum açma ile Azure AD, kullanıcının Azure AD hesabını kullanarak uygulamaya kimlik doğrulaması sağlar. Azure AD, oturum açma bilgilerini bir bağlantı protokolü aracılığıyla uygulamaya iletir. SAML tabanlı tek oturum açma ile, kullanıcıları SAML taleplerinizde tanımladığınız kurallara göre belirli uygulama rolleriyle eşleyebilirsiniz. SAML tek oturum açma hakkında bilgi için Uygulama [Proxy ile tek oturum açma için SAML'ye](application-proxy-configure-single-sign-on-on-premises-apps.md)bakın.

Bu seçeneklerin her birini "Kurumsal Uygulamalar" uygulamasından giderek ve sol menüdeki **Tek Oturum Açma** sayfasını açarak bulabilirsiniz. Uygulamanız eski portalda oluşturulduysa, tüm bu seçenekleri göremeyebilirsiniz.

Bu sayfada, ek bir Oturum Açma seçeneği de görürsünüz: Bağlantılı Oturum Açma. Bu seçenek, Uygulama Proxy tarafından da desteklenir. Ancak, bu seçenek uygulamaya tek oturum açma eklemez. Bu uygulama zaten Aktif Dizin Federasyon Hizmetleri gibi başka bir hizmet kullanılarak uygulanan tek oturum açma olabileceğini söyledi. 

Bu seçenek, bir yöneticinin, kullanıcıların uygulamaya erişirken ilk olarak üzerine inen bir uygulamaya bağlantı oluşturmasına olanak tanır. Örneğin, Active Directory Federation Services 2.0'ı kullanarak kullanıcıların kimliğini doğrulamak üzere yapılandırılan bir uygulama varsa, yönetici erişim panelinde bu bağlantı oluşturmak için "Bağlantılı Oturum Açma" seçeneğini kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar
- [Application Proxy ile tek oturum açma için şifre atlama](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Uygulama Proxy ile tek oturum açma için Kerberos Kısıtlı Delegasyonu](application-proxy-configure-single-sign-on-with-kcd.md)
- [Uygulama Proxy ile tek oturum açma için üstbilgi tabanlı kimlik doğrulama](application-proxy-configure-single-sign-on-with-ping-access.md) 
- [Uygulama Proxy ile tek oturum açma için SAML.](application-proxy-configure-single-sign-on-on-premises-apps.md)
