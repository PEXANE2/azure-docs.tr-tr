---
title: Uygulama proxy 'Si uygulamasında çoklu oturum açmayı yapılandırma
description: Nasıl çoklu oturum açma uygulama proxy uygulamanıza hızlı bir şekilde yapılandırabilirsiniz
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
ms.openlocfilehash: 8b5eca08e0b736937af42e58d81148636e3269df
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74275629"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Uygulama proxy'si uygulaması için çoklu oturum açma yapılandırma

Çoklu oturum açma (SSO) kullanıcılarınızın birden çok kez kimlik doğrulaması olmadan bir uygulamaya erişmesine izin verir. Bu bulutta, Azure Active Directory karşı gerçekleşmesi tek bir kimlik doğrulaması ve hizmet veya uygulamadaki herhangi ek bir kimlik doğrulama sınaması tamamlamak için kullanıcının kimliğine bürünmek için bağlayıcı olanak sağlar.

## <a name="how-to-configure-single-sign-on"></a>Çoklu oturum yapılandırma
SSO yapılandırmak için ilk uygulamanızı Azure Active Directory üzerinden ön kimlik doğrulaması için yapılandırılmış olduğundan emin olun. Bu yapılandırmayı yapmak için **Azure Active Directory** -&gt; **Kurumsal uygulamalar** &gt; -uygulama **ara sunucusu**&gt; uygulamanızın-**tüm uygulamalar** &gt; -gidin. Bu sayfada, "Ön kimlik doğrulaması" alanına bakın ve "Azure Active Directory için ayarlanmış olduğundan emin olun 

Ön kimlik doğrulama yöntemleri hakkında daha fazla bilgi için bkz. [uygulama yayımlama belgesi](application-proxy-add-on-premises-application.md)adım 4.

   ![Azure portalında ön kimlik doğrulama yöntemi](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Çoklu oturum açma modları için uygulama Proxy uygulamaları yapılandırma
Çoklu oturum açma belirli türünü yapılandırın. Oturum açma yöntemleri, hangi kimlik doğrulaması türü arka uç uygulamanın kullandığı şirket göre sınıflandırılır. Uygulama Proxy uygulamaları, üç tür oturum açmayı destekler:

-   **Parola tabanlı oturum**açma: parola tabanlı oturum açma, oturum açmak için Kullanıcı adı ve parola alanlarını kullanan herhangi bir uygulama için kullanılabilir. Yapılandırma adımları, [bir Azure AD Galeri uygulaması için parola yapılandırma çoklu oturum açma](configure-password-single-sign-on-gallery-applications.md)bölümünde bulunur.

-   **Tümleşik Windows kimlik doğrulaması**: tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar için, tek oturum açma, Kerberos kısıtlanmış temsili (KCD) aracılığıyla etkinleştirilir. Bu yöntem uygulama Proxy Bağlayıcılarına, kullanıcıların kimliğine bürünmek ve göndermek ve kendileri adına belirteçlerini almak için Active Directory'de sağlar. KCD 'yi yapılandırmaya ilişkin ayrıntılar, [KCD belgelerinde çoklu oturum açma](application-proxy-configure-single-sign-on-with-kcd.md)bölümünde bulunabilir.

-   **Üst bilgi tabanlı oturum**açma: üst bilgi tabanlı oturum açma, bir ortaklık aracılığıyla etkinleştirilir ve bazı ek yapılandırmalar gerektirir. Kimlik doğrulama için üst bilgiler kullanan bir uygulamada çoklu oturum açmayı yapılandırmaya yönelik iş ortaklığı ve adım adım yönergeler için bkz. [Azure AD belgeleri Için Pingaccess](application-proxy-configure-single-sign-on-with-ping-access.md).

-   **SAML çoklu oturum**açma: SAML çoklu oturum açma Ile Azure AD, kullanıcının Azure AD hesabını kullanarak uygulamanın kimliğini doğrular. Azure AD oturum açma bilgileri uygulamaya bir bağlantı protokolü üzerinden iletişim kurar. SAML tabanlı çoklu oturum açma sayesinde, SAML talepleriniz içinde tanımladığınız kurallara göre kullanıcıları belirli uygulama rolleriyle eşleyebilirsiniz. SAML çoklu oturum açmayı ayarlama hakkında daha fazla bilgi için bkz. [uygulama proxy 'si ile çoklu oturum açma Için SAML](application-proxy-configure-single-sign-on-on-premises-apps.md).

Bu seçeneklerin her biri, uygulamanıza "kurumsal uygulamalar" ile gidip Sol menüdeki **Çoklu oturum açma** sayfasını açıp bulabilirsiniz. Uygulamanız eski portalda oluşturulduysa, tüm bu seçenekleri görmeyebilirsiniz.

Bu sayfada, ayrıca bir gördüğünüz ek oturum açma seçeneği: bağlantılı oturum açma. Bu seçenek, uygulama proxy'si tarafından da desteklenir. Ancak, bu seçeneği çoklu oturum açma uygulamaya eklemez. Bu uygulamanın tek Active Directory Federasyon Hizmetleri gibi başka bir hizmet kullanılarak uygulanan oturum zaten olabilir belirtti. 

Bu seçenek, bir uygulamaya bir bağlantı, kullanıcıların ilk land uygulamaya erişirken oluşturmak için bir yönetici sağlar. Active Directory Federasyon Hizmetleri 2.0 kullanan kullanıcıların kimliğini doğrulamak üzere yapılandırılmış bir uygulama ise, örneğin, bir yönetici "bağlantılı oturum açma" seçeneği erişim panelinde bir bağlantı oluşturmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Uygulama proxy 'Si ile çoklu oturum açma için parola oluşturma](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Uygulama proxy 'Si ile çoklu oturum açma için Kerberos kısıtlanmış temsili](application-proxy-configure-single-sign-on-with-kcd.md)
- [Uygulama proxy 'Si ile çoklu oturum açma için üst bilgi tabanlı kimlik doğrulaması](application-proxy-configure-single-sign-on-with-ping-access.md) 
- [Uygulama proxy 'si ile çoklu oturum açma Için SAML](application-proxy-configure-single-sign-on-on-premises-apps.md).
