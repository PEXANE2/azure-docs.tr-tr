---
title: Azure AD FS desteği (MSAL Python)
titleSuffix: Microsoft identity platform
description: Python için Microsoft kimlik doğrulama kitaplığı 'nda Active Directory Federasyon Hizmetleri (AD FS) (AD FS) desteği hakkında bilgi edinin
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
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76699555"
---
# <a name="active-directory-federation-services-support-in-msal-for-python"></a>Python için MSAL Active Directory Federasyon Hizmetleri (AD FS) desteği

Windows Server 'daki Active Directory Federasyon Hizmetleri (AD FS) (AD FS), Python için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanarak uygulamalarınıza OpenID Connect ve OAuth 2,0 tabanlı kimlik doğrulaması ve yetkilendirme eklemenize olanak sağlar. MSAL for Python kitaplığı kullanarak, uygulamanız kullanıcıların kimliğini doğrudan AD FS karşı doğrulayabilir. Senaryolar hakkında daha fazla bilgi için bkz. [geliştiriciler için AD FS senaryoları](/windows-server/identity/ad-fs/ad-fs-development).

AD FS kimlik doğrulamanın genellikle iki yolu vardır:

- MSAL Python, kendisini diğer kimlik sağlayıcılarıyla Federal olan Azure Active Directory. AD FS aracılığıyla Federasyon gerçekleşir. MSAL Python, Azure AD 'de (yönetilen kullanıcılar) yönetilen kullanıcıların veya AD FS (Federe kullanıcılar) gibi başka bir kimlik sağlayıcısı tarafından yönetilen kullanıcıların oturum açtığı Azure AD 'ye bağlanır. MSAL Python bir kullanıcının federe olduğunu bilmez. Yalnızca Azure AD 'ye sahip olur. Bu durumda kullandığınız [yetkili](msal-client-application-configuration.md#authority) , her zamanki yetkilinin (yetkili ana bilgisayar adı + kiracı, ortak veya kuruluşlar).
- MSAL Python doğrudan bir AD FS yetkilisine konuşuyor. Bu yalnızca AD FS 2019 ve üzeri sürümlerde desteklenir.

## <a name="connect-to-active-directory-federated-with-ad-fs"></a>AD FS ile federe Active Directory bağlanma

### <a name="acquire-a-token-interactively-for-a-federated-user"></a>Federasyon kullanıcısı için bir belirteci etkileşimli olarak alma

Active Directory Federasyon Hizmetleri (AD FS) (AD FS) veya Active Directory aracılığıyla doğrudan bağlanıp bağlanmayacağı aşağıda geçerlidir.

`acquire_token_by_authorization_code` veya `acquire_token_by_device_flow`çağırdığınızda, Kullanıcı deneyimi genellikle aşağıdaki gibidir:

1. Kullanıcı hesap KIMLIĞINI girer.
2. Azure AD, "kuruluşunuzun sayfasına yönlendirilme" iletisini kısaca gösterir ve Kullanıcı kimlik sağlayıcısının oturum açma sayfasına yönlendirilir. Oturum açma sayfası genellikle kuruluşun logosu ile özelleştirilir.

Bu Federasyon senaryosunda desteklenen AD FS sürümleri şunlardır:
- Active Directory Federasyon Hizmetleri (AD FS) FS v2
- Active Directory Federasyon Hizmetleri (AD FS) v3 (Windows Server 2012 R2)
- Active Directory Federasyon Hizmetleri (AD FS) v4 (AD FS 2016)

### <a name="acquire-a-token-via-username-and-password"></a>Kullanıcı adı ve parola aracılığıyla belirteç alma

Active Directory Federasyon Hizmetleri (AD FS) (AD FS) veya Active Directory aracılığıyla doğrudan bağlanıp bağlanmayacağı aşağıda geçerlidir.

`acquire_token_by_username_password`kullanarak bir belirteç aldığınızda, MSAL Python Kullanıcı adına göre iletişim kurulacak kimlik sağlayıcısını alır. MSAL Python, kimlik sağlayıcısından bir [SAML 1,1 belirteci](reference-saml-tokens.md) alır ve bu, daha sonra JSON Web token (JWT) döndüren Azure AD 'ye sunulur.

## <a name="connecting-directly-to-ad-fs"></a>AD FS doğrudan bağlanma

Dizini AD FS 'e bağladığınızda uygulamanızı oluşturmak için kullanmak istediğiniz yetkili `https://somesite.contoso.com/adfs/` gibi olacaktır.

MSAL Python, ADFS 2019 ' i destekler.

ADFS 2016 veya ADFS v2 'ye doğrudan bağlantıyı desteklemez. ADFS 2016 'e doğrudan bağlantı gerektiren senaryoları desteklemeniz gerekiyorsa, ADAL Python 'un en son sürümünü kullanın. Şirket içi sisteminizi ADFS 2019 ' e yükselttikten sonra, MSAL Python 'u kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Federasyon durumu için bkz. [ana bölge bulma ilkesi kullanarak bir uygulama için Azure Active Directory oturum açma davranışını yapılandırma](../manage-apps/configure-authentication-for-federated-users-portal.md)