---
title: AD FS desteği (Java için MSAL)
titleSuffix: Microsoft identity platform
description: Java (MSAL4j) için Microsoft kimlik doğrulama kitaplığı 'nda Active Directory Federasyon Hizmetleri (AD FS) (AD FS) desteği hakkında bilgi edinin.
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
ms.custom: aaddev, devx-track-java
ms.openlocfilehash: 5d01d1143563637e21aaa06a3f997c1507e4d8f1
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88114787"
---
# <a name="active-directory-federation-services-support-in-msal-for-java"></a>Java için MSAL Active Directory Federasyon Hizmetleri (AD FS) desteği

Windows Server 'daki Active Directory Federasyon Hizmetleri (AD FS) (AD FS), Java (Java için MSAL) uygulaması için Microsoft Authentication kitaplığınıza OpenID Connect ve OAuth 2,0 tabanlı kimlik doğrulaması ve yetkilendirme eklemenize olanak sağlar. Uygulamanız tümleşik olduktan sonra Azure AD aracılığıyla federe olan AD FS kullanıcıların kimliğini doğrulayabilir. Senaryolar hakkında daha fazla bilgi için bkz. [geliştiriciler için AD FS senaryoları](/windows-server/identity/ad-fs/ad-fs-development).

Java için MSAL kullanan bir uygulama, daha sonra AD FS federasyona Azure Active Directory (Azure AD) ile konuşacaktır.

Java için MSAL, Azure AD 'de (yönetilen kullanıcılar) yönetilen kullanıcıları veya AD FS (Federe kullanıcılar) gibi başka bir kimlik sağlayıcısı tarafından yönetilen kullanıcıları oturum açan Azure AD 'ye bağlanır. Java için MSAL, bir kullanıcının federe olduğunu bilmez. Yalnızca Azure AD 'ye sahip olur.

Bu durumda kullandığınız [yetkili](msal-client-application-configuration.md#authority) , her zamanki yetkilinin (yetkili ana bilgisayar adı + kiracı, ortak veya kuruluşlar).

## <a name="acquire-a-token-interactively-for-a-federated-user"></a>Federasyon kullanıcısı için bir belirteci etkileşimli olarak alma

Ya da `ConfidentialClientApplication.AcquireToken()` `PublicClientApplication.AcquireToken()` veya ile çağırdığınızda `AuthorizationCodeParameters` `DeviceCodeParameters` , Kullanıcı deneyimi genellikle şu şekilde olur:

1. Kullanıcı hesap KIMLIĞINI girer.
2. Azure AD, "kuruluşunuzun sayfasına yönlendirilme" seçeneğini kısaca gösterir ve Kullanıcı kimlik sağlayıcısının oturum açma sayfasına yönlendirilir. Oturum açma sayfası genellikle kuruluşun logosu ile özelleştirilir.

Bu Federasyon senaryosunda desteklenen AD FS sürümleri şunlardır:
- Active Directory Federasyon Hizmetleri (AD FS) FS v2
- Active Directory Federasyon Hizmetleri (AD FS) v3 (Windows Server 2012 R2)
- Active Directory Federasyon Hizmetleri (AD FS) v4 (AD FS 2016)

## <a name="acquire-a-token-via-username-and-password"></a>Kullanıcı adı ve parola aracılığıyla belirteç alma

Or veya ile kullanarak bir belirteç `ConfidentialClientApplication.AcquireToken()` aldığınızda `PublicClientApplication.AcquireToken()` `IntegratedWindowsAuthenticationParameters` `UsernamePasswordParameters` , Java için msal, Kullanıcı adına göre iletişim kurulacak kimlik sağlayıcısını alır. Java için MSAL, kimlik sağlayıcısından bir [SAML 1,1 belirteç](reference-saml-tokens.md) belirteci alır ve bu, daha sonra JSON Web token (JWT) döndüren Azure AD 'ye sunulur.

## <a name="next-steps"></a>Sonraki adımlar

Federasyon durumu için bkz. [ana bölge bulma ilkesi kullanarak bir uygulama için Azure Active Directory oturum açma davranışını yapılandırma](../manage-apps/configure-authentication-for-federated-users-portal.md)