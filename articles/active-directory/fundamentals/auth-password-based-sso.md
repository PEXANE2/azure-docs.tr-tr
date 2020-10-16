---
title: Azure Active Directory ile parola tabanlı kimlik doğrulaması
description: Bu kimlik doğrulama modelini elde etmek için mimari yönergeler
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
ms.openlocfilehash: f05bf8f5f7ec4907c2cd61ff48e3438dfa1e097a
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114363"
---
# <a name="password-based-authentication-with-azure-active-directory"></a>Azure Active Directory ile parola tabanlı kimlik doğrulaması

Parola tabanlı tek Sign-On (SSO), uygulama için mevcut kimlik doğrulama işlemini kullanır. Parola tabanlı SSO 'yu etkinleştirdiğinizde Azure Active Directory (Azure AD), Kullanıcı kimlik bilgilerini dizinde toplar, şifreler ve güvenli bir şekilde depolar. Azure AD, Kullanıcı oturum açmayı denediğinde uygulamanın kullanıcı adını ve parolasını sağlar.

Bir uygulama, belirteç ve üst bilgiler yerine Kullanıcı adı ve parolayla kimlik doğrulaması gerçekleştirdiğinde parola tabanlı SSO seçin. Parola tabanlı SSO, HTML tabanlı oturum açma sayfasına sahip olan tüm bulut tabanlı uygulamaları destekler. 

## <a name="use-when"></a>Şu durumlarda kullanın

Ön kimlik doğrulaması ile korumanız gerekir ve Web uygulamalarına parola aracılığıyla SSO sağlamanız gerekir.

![mimari diyagram](./media/authentication-patterns/password-based-sso-auth.png)


## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı**: uygulamalarım ya da doğrudan siteyi ziyaret ederek oluşturulmuş tabanlı uygulamaya erişir. 

* **Web tarayıcısı**: kullanıcının uygulamanın dış URL 'sine erişmek için etkileşimde bulunduğu bileşen. Kullanıcı, form tabanlı uygulamaya Uygps uzantısı aracılığıyla erişir. 

* **Uygps uzantısı**: yapılandırılan parola tabanlı SSO uygulamasını tanımlar ve oturum açma formuna kimlik bilgilerini sağlar. Uygps uzantısı Web tarayıcısına yüklendi. 

* **Azure AD**: kullanıcının kimliğini doğrular.

## <a name="implement-password-based-sso-with-azure-ad"></a>Azure AD ile parola tabanlı SSO uygulama

* [Parola tabanlı SSO nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) 

* [Bulut uygulamaları için parola tabanlı SSO 'yu yapılandırma ](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications)

* [Uygulama proxy 'Si ile şirket içi uygulamalar için parola tabanlı SSO 'yu yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-password-vaulting)

 
