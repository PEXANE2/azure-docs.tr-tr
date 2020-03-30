---
title: MSAL.NET'da AD FS desteği | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nda Active Directory Federation Services (AD FS) desteği hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 15af18177cea217612a4d5276d130abe02d339f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77160769"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>MSAL.NET'da Aktif Dizin Federasyon Hizmetleri desteği
Windows Server'daki Active Directory Federation Services (AD FS), geliştirdiğiniz uygulamalara OpenID Connect ve OAuth 2.0 tabanlı kimlik doğrulama ve yetkilendirme eklemenize olanak tanır. Bu uygulamalar, daha sonra, doğrudan AD FS karşı kullanıcıların kimlik doğrulaması yapabilirsiniz. Daha fazla bilgi için [Geliştiriciler için AD FS Senaryoları'nı](/windows-server/identity/ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios)okuyun.

.NET için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.NET), AD FS'ye karşı kimlik doğrulama için iki senaryoyu destekler:

- MSAL.NET, kendisi DE AD FS ile *beslenen* Azure Active Directory ile konuşuyor.
- MSAL.NET **doğrudan** bir ADFS yetkilisiile görüşüyor. Bu sadece AD FS 2019 ve üzeri desteklenir. Bu vurgular senaryolardan biri [Azure Yığını](https://azure.microsoft.com/overview/azure-stack/) desteği


## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL, AD FS ile beslenen Azure AD'ye bağlanır
MSAL.NET, yönetilen kullanıcılarda (Azure AD'de yönetilen kullanıcılar) veya federe kullanıcılarda (AD FS gibi başka bir kimlik sağlayıcısı tarafından yönetilen kullanıcılar) işaretleyen Azure AD'ye bağlanmayı destekler. MSAL.NET kullanıcıların federe olduğu gerçeğini bilmiyor. Azure AD ile ilgilidir.

Bu durumda kullandığınız [yetki](msal-client-application-configuration.md#authority) olağan makamdır (yetkili kişi adı + kiracı, ortak veya kuruluşlar).

### <a name="acquiring-a-token-interactively"></a>Etkileşimli olarak belirteç edinme
`AcquireTokenInteractive` Yöntemi aradiğinizde, kullanıcı deneyimi genellikle:

1. Kullanıcı hesap kimliğini girer.
2. Azure AD, "Sizi kuruluşunuzun sayfasına götürüyor" mesajını kısaca görüntüler.
3. Kullanıcı, kimlik sağlayıcısının oturum açma sayfasına yönlendirilir. Oturum açma sayfası genellikle kuruluşun logosuyla özelleştirilmiştir.

Bu federe senaryoda desteklenen AD FS sürümleri AD FS v2, AD FS v3 (Windows Server 2012 R2) ve AD FS v4 (AD FS 2016) 'dir.

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>AcquireTokenByIntegratedAuthentication veya AcquireTokenByUsernamePassword kullanarak belirteç edinme
MSAL.NET, bir belirteç `AcquireTokenByIntegratedAuthentication` `AcquireTokenByUsernamePassword` veya yöntemleri kullanarak bir belirteç edinirken, kimlik sağlayıcısının kullanıcı adına bağlı olarak iletişim kurmasını sağlar.  MSAL.NET kimlik sağlayıcısına başvurulmasından sonra [bir SAML 1.1 belirteci](reference-saml-tokens.md) alır.  MSAL.NET sonra, bir JWT'yi geri almak için kullanıcı iddiası [(akış adına](msal-authentication-flows.md#on-behalf-of)benzer) olarak Azure AD'ye SAML belirteci sağlar.

## <a name="msal-connects-directly-to-ad-fs"></a>MSAL doğrudan AD FS'ye bağlanır
MSAL.NET, Open ID Connect uyumlu olan ve PKCE ve kapsamları anlayan AD FS 2019'a bağlanmayı destekler. Bu destek, Windows Server'a bir hizmet paketi [KB 4490481](https://support.microsoft.com/en-us/help/4490481/windows-10-update-kb4490481) uygulanmasını gerektirir. Doğrudan AD FS'ye bağlanırken, uygulamanızı oluşturmak için kullanmak isteyeceğiniz `https://mysite.contoso.com/adfs/`yetki .

Şu anda, doğrudan bağlantı desteklemek için herhangi bir plan vardır:

- AD FS 16, PKCE'yi desteklemediği ve kapsam değil kaynakları hala kullandığı için
- OIDC uyumlu olmayan AD FS v2.

 AD FS 2016'ya doğrudan bağlantı gerektiren senaryoları desteklemeniz gerekiyorsa, [Azure Etkin Dizin Kimlik Doğrulama Kitaplığı'nın](../azuread-dev/active-directory-authentication-libraries.md#microsoft-supported-client-libraries)en son sürümünü kullanın. Şirket içi sisteminizi AD FS 2019'a yükselttiğinde, MSAL.NET kullanabileceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

Federe durumda, bir [Home Realm Discovery ilkesi ni kullanarak bir uygulama için Azure Etkin Dizin oturum](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) açma davranışına bakın
