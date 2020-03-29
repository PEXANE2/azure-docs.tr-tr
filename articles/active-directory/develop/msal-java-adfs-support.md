---
title: AD FS desteği (Java için MSAL)
titleSuffix: Microsoft identity platform
description: Java için Microsoft Kimlik Doğrulama Kitaplığı'nda (MSAL4j) Active Directory Federation Services (AD FS) desteği hakkında bilgi edinin.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/21/2019
ms.author: sagonzal
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 3d834a8d1524595304c22fed9897094622dfd93f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696223"
---
# <a name="active-directory-federation-services-support-in-msal-for-java"></a>Java için MSAL Aktif Dizin Federasyon Hizmetleri desteği

Windows Server'daki Active Directory Federation Services (AD FS), Java için Microsoft Kimlik Doğrulama Kitaplığı (Java için MSAL) uygulamanıza OpenID Connect ve OAuth 2.0 tabanlı kimlik doğrulama ve yetkilendirme eklemenize olanak tanır. Entegre edildikten sonra, uygulamanız Azure AD aracılığıyla federated AD FS kullanıcıları kimlik doğrulaması yapabilirsiniz. Senaryolar hakkında daha fazla bilgi için [Geliştiriciler için AD FS Senaryolarına](/windows-server/identity/ad-fs/ad-fs-development)bakın.

Java için MSAL kullanan bir uygulama, daha sonra AD FS'ye fetode olan Azure Active Directory (Azure AD) ile konuşur.

Java için MSAL, Azure AD'de (yönetilen kullanıcılar) yönetilen kullanıcılarda veya AD FS (federe kullanıcılar) gibi başka bir kimlik sağlayıcısı tarafından yönetilen kullanıcılarda işaretleyen Azure AD'ye bağlanır. Java için MSAL bir kullanıcı federe olduğunu bilmiyor. Azure AD ile sadece konuşur.

Bu durumda kullandığınız [yetki](msal-client-application-configuration.md#authority) olağan makamdır (yetkili kişi adı + kiracı, ortak veya kuruluşlar).

## <a name="acquire-a-token-interactively-for-a-federated-user"></a>Federe bir kullanıcı için etkileşimli bir belirteç edinme

`ConfidentialClientApplication.AcquireToken()` Aradığınızda `PublicClientApplication.AcquireToken()` veya `AuthorizationCodeParameters` `DeviceCodeParameters`kullanıcı deneyimi ile ilgili olarak genellikle:

1. Kullanıcı hesap kimliğini girer.
2. Azure AD kısaca "Sizi kuruluşunuzun sayfasına götürüyor" ifadesiyle görüntülenir ve kullanıcı kimlik sağlayıcısının oturum açma sayfasına yönlendirilir. Oturum açma sayfası genellikle kuruluşun logosuyla özelleştirilmiştir.

Bu federe senaryoda desteklenen AD FS sürümleri şunlardır:
- Aktif Dizin Federasyon Hizmetleri FS v2
- Aktif Dizin Federasyon Hizmetleri v3 (Windows Server 2012 R2)
- Aktif Dizin Federasyon Hizmetleri v4 (AD FS 2016)

## <a name="acquire-a-token-via-username-and-password"></a>Kullanıcı adı ve parola ile belirteç edinme

Java için MSAL `ConfidentialClientApplication.AcquireToken()` kullanarak `PublicClientApplication.AcquireToken()` `IntegratedWindowsAuthenticationParameters` veya `UsernamePasswordParameters`ile bir belirteç edindiğinizde, kullanıcı adına bağlı olarak kimlik sağlayıcısının iletişim kurmasını sağlar. Java için MSAL, kimlik sağlayıcısından [saml 1.1 belirteci](reference-saml-tokens.md) alır ve bu belirteçleri Azure AD'ye sağlar ve bu da JSON Web Belirteci'ni (JWT) döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Federe durumda, bir [Home Realm Discovery ilkesi ni kullanarak bir uygulama için Azure Etkin Dizin oturum](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) açma davranışına bakın