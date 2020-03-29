---
title: Azure AD FS desteği (MSAL Python)
titleSuffix: Microsoft identity platform
description: Python için Microsoft Kimlik Doğrulama Kitaplığı'nda Active Directory Federation Services (AD FS) desteği hakkında bilgi edinin
services: active-directory
author: abhidnya13
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/23/2019
ms.author: abpati
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 01d4cb626aabc83117e864b75b49eec63a6c0af0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76699555"
---
# <a name="active-directory-federation-services-support-in-msal-for-python"></a>Python için MSAL Aktif Dizin Federasyon Hizmetleri desteği

Windows Server'daki Active Directory Federation Services (AD FS), Python için Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL) kullanarak uygulamalarınız için OpenID Connect ve OAuth 2.0 tabanlı kimlik doğrulama ve yetkilendirme eklemenize olanak tanır. Python kitaplığı için MSAL'ı kullanarak uygulamanız kullanıcıları doğrudan AD FS'ye karşı doğrulayabilir. Senaryolar hakkında daha fazla bilgi için [Geliştiriciler için AD FS Senaryolarına](/windows-server/identity/ad-fs/ad-fs-development)bakın.

AD FS'ye karşı kimlik doğrulamanın genellikle iki yolu vardır:

- MSAL Python, diğer kimlik sağlayıcılarıyla birlikte federated olan Azure Active Directory ile konuşur. Federasyon AD FS ile olur. MSAL Python, Azure AD'de (yönetilen kullanıcılar) yönetilen kullanıcılarda veya AD FS (federe kullanıcılar) gibi başka bir kimlik sağlayıcısı tarafından yönetilen kullanıcılarda işaretleyen Azure AD'ye bağlanır. MSAL Python bir kullanıcının federe olduğunu bilmiyor. Azure AD ile sadece konuşur. Bu durumda kullandığınız [yetki](msal-client-application-configuration.md#authority) olağan makamdır (yetkili kişi adı + kiracı, ortak veya kuruluşlar).
- MSAL Python doğrudan bir AD FS yetkilisiile konuşur. Bu sadece AD FS 2019 ve sonrası tarafından desteklenir.

## <a name="connect-to-active-directory-federated-with-ad-fs"></a>AD FS ile federe Aktif Dizine bağlanın

### <a name="acquire-a-token-interactively-for-a-federated-user"></a>Federe bir kullanıcı için etkileşimli bir belirteç edinme

Aşağıdakiler, doğrudan Active Directory Federation Services (AD FS) veya Active Directory aracılığıyla bağlantı kurmanız için geçerlidir.

`acquire_token_by_authorization_code` Aradığınızda `acquire_token_by_device_flow`veya , kullanıcı deneyimi genellikle aşağıdaki gibidir:

1. Kullanıcı hesap kimliğini girer.
2. Azure AD, "Sizi kuruluşunuzun sayfasına götürüyoruz" iletisini kısaca görüntüler ve kullanıcı kimlik sağlayıcısının oturum açma sayfasına yönlendirilir. Oturum açma sayfası genellikle kuruluşun logosuyla özelleştirilmiştir.

Bu federe senaryoda desteklenen AD FS sürümleri şunlardır:
- Aktif Dizin Federasyon Hizmetleri FS v2
- Aktif Dizin Federasyon Hizmetleri v3 (Windows Server 2012 R2)
- Aktif Dizin Federasyon Hizmetleri v4 (AD FS 2016)

### <a name="acquire-a-token-via-username-and-password"></a>Kullanıcı adı ve parola ile belirteç edinme

Aşağıdakiler, doğrudan Active Directory Federation Services (AD FS) veya Active Directory aracılığıyla bağlantı kurmanız için geçerlidir.

Bir belirteç kullanarak `acquire_token_by_username_password`elde ettiğinizde, MSAL Python kullanıcı adına göre kimlik sağlayıcısına başvurmasını sağlar. MSAL Python kimlik sağlayıcısından [bir SAML 1.1 belirteci](reference-saml-tokens.md) alır ve bu jetonu Azure AD'ye sağlar ve bu da JSON Web Belirteci'ni (JWT) döndürür.

## <a name="connecting-directly-to-ad-fs"></a>Doğrudan AD FS'ye bağlanma

Dizini AD FS'ye bağladığınızda, uygulamanızı oluşturmak için kullanmak isteyeceğiniz yetki`https://somesite.contoso.com/adfs/`

MSAL Python, ADFS 2019'u destekliyor.

ADFS 2016 veya ADFS v2 ile doğrudan bağlantı sağlamaz. ADFS 2016'ya doğrudan bağlantı gerektiren senaryoları desteklemeniz gerekiyorsa, ADAL Python'un en son sürümünü kullanın. Şirket içi sisteminizi ADFS 2019'a yükselttikten sonra MSAL Python'u kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Federe durumda, bir [Home Realm Discovery ilkesi ni kullanarak bir uygulama için Azure Etkin Dizin oturum](../manage-apps/configure-authentication-for-federated-users-portal.md) açma davranışına bakın