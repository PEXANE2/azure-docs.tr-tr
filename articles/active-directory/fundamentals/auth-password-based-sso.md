---
title: Azure Active Directory ile parola tabanlı kimlik doğrulaması
description: Azure Active Directory ile parola tabanlı kimlik doğrulaması elde etmeye yönelik mimari rehberlik.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74815976036abafc56f97e622351a1018fe30504
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576941"
---
# <a name="password-based-authentication-with-azure-active-directory"></a>Azure Active Directory ile parola tabanlı kimlik doğrulaması

Parola tabanlı tek Sign-On (SSO), uygulama için mevcut kimlik doğrulama işlemini kullanır. Parola tabanlı SSO 'yu etkinleştirdiğinizde Azure Active Directory (Azure AD), Kullanıcı kimlik bilgilerini dizinde toplar, şifreler ve güvenli bir şekilde depolar. Azure AD, Kullanıcı oturum açmayı denediğinde uygulamanın kullanıcı adını ve parolasını sağlar.

Bir uygulama, belirteç ve üst bilgiler yerine Kullanıcı adı ve parolayla kimlik doğrulaması gerçekleştirdiğinde parola tabanlı SSO seçin. Parola tabanlı SSO, HTML tabanlı oturum açma sayfasına sahip olan tüm bulut tabanlı uygulamaları destekler. 

## <a name="use-when"></a>Şu durumlarda kullanın

Ön kimlik doğrulaması ile korumanız gerekir ve Web uygulamalarına parola aracılığıyla SSO sağlamanız gerekir.

![mimari diyagram](./media/authentication-patterns/password-based-sso-auth.png)


## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı** : uygulamalarım ya da doğrudan siteyi ziyaret ederek oluşturulmuş tabanlı uygulamaya erişir. 

* **Web tarayıcısı** : kullanıcının uygulamanın dış URL 'sine erişmek için etkileşimde bulunduğu bileşen. Kullanıcı, form tabanlı uygulamaya Uygps uzantısı aracılığıyla erişir. 

* **Uygps uzantısı** : yapılandırılan parola tabanlı SSO uygulamasını tanımlar ve oturum açma formuna kimlik bilgilerini sağlar. Uygps uzantısı Web tarayıcısına yüklendi. 

* **Azure AD** : kullanıcının kimliğini doğrular.

## <a name="implement-password-based-sso-with-azure-ad"></a>Azure AD ile parola tabanlı SSO uygulama

* [Parola tabanlı SSO nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) 

* [Bulut uygulamaları için parola tabanlı SSO 'yu yapılandırma ](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications)

* [Uygulama proxy 'Si ile şirket içi uygulamalar için parola tabanlı SSO 'yu yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-password-vaulting)

 
