---
title: Kimlik doğrulaması ile yetkilendirme | Mavisi
titleSuffix: Microsoft identity platform
description: Microsoft Identity platformunda (v 2.0) kimlik doğrulaması ve yetkilendirmenin temelleri hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/06/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: b905b8f3fe99b83bafdd61b1daa25549354c5275
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82926791"
---
# <a name="authentication-vs-authorization"></a>Kimlik doğrulama ve yetkilendirme karşılaştırması

Bu makalede kimlik doğrulaması ve yetkilendirme tanımlanmaktadır ve Web uygulamalarınızda, Web API 'Lerinde veya korumalı Web API 'Leri çağıran uygulamalarda kullanıcıların kimliğini doğrulamak ve yetkilendirmek için Microsoft Identity platformunu nasıl kullanabileceğiniz kısaca ele alınmaktadır. Bildiğiniz bir terimi görürseniz, temel kavramları kapsayan [Sözlüğümüzü](developer-glossary.md) veya [Microsoft Identity platform videolarımızı](identity-videos.md) deneyin.

## <a name="authentication"></a>Kimlik Doğrulaması

**Kimlik doğrulama** , sizin olduğunu söylediğinizde sizin olduğunu kanıtlama işlemidir. Kimlik doğrulaması bazen AuthN şeklinde kısaltılabilir. Microsoft Identity platform kimlik doğrulamasını işlemek için [OpenID Connect](https://openid.net/connect/) ve [SAML 2,0](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html) protokollerini uygular.

## <a name="authorization"></a>Yetkilendirme

**Yetkilendirme** , kimliği doğrulanmış bir tarafın bir şeyi yapmak için izin verme işlemidir. Hangi verilerin erişebileceğini ve bu verilerle yapabileceklerinizi belirtir. Yetkilendirme bazen AuthZ şeklinde kısaltılabilir. Microsoft Identity platform yetkilendirmeyi işlemek için [OAuth 2,0](https://oauth.net/2/) protokolünü uygular.

## <a name="authentication-and-authorization-using-microsoft-identity-platform"></a>Microsoft Identity platform kullanarak kimlik doğrulaması ve yetkilendirme

Her birinin kendi Kullanıcı adı ve parola bilgilerini korumalarına veya birden çok uygulama arasında Kullanıcı eklemeniz veya kaldırmanız gerektiğinde yüksek bir yönetim yükü sunan uygulamalar oluşturmak yerine, uygulamalar bu sorumluluğu merkezi bir kimlik sağlayıcısına devredebilir.

Azure Active Directory (Azure AD), bulutta merkezi bir kimlik sağlayıcıdır. Kimlik doğrulama ve yetkilendirmeyi yetkilendirme, bir kullanıcının belirli bir konumda olmasını gerektiren koşullu erişim ilkeleri, Multi-Factor Authentication 'ın kullanılması ve bir kullanıcının bir kez oturum açmasını ve ardından aynı merkezi dizini paylaşan tüm Web uygulamalarında otomatik olarak oturum açmasını sağlar. Bu yetenek, **Çoklu oturum açma (SSO)** olarak adlandırılır.

Microsoft Identity platform; OAuth 2,0, OpenID Connect ve SAML 2,0 gibi endüstri standardı protokoller için destek sunan ve kodlamaya hızlı bir şekilde başlamanıza yardımcı olacak farklı platformlar için açık kaynak kitaplıkların yanı sıra, uygulama geliştiricileri için kimlik doğrulama ve yetkilendirmeyi basitleştirir. Geliştiricilerin tüm Microsoft kimliklerinden oturum açmasını, [Microsoft Graph](https://developer.microsoft.com/graph/), diğer Microsoft API 'lerini veya geliştiricilerin oluşturduğu API 'leri çağıracağınız belirteçler almasını sağlar. Daha fazla bilgi için bkz. [Microsoft Identity platform 'un gelişi](about-microsoft-identity-platform.md).

Aşağıda, Microsoft Identity platform tarafından kullanılan çeşitli protokollerin kısa bir karşılaştırması verilmiştir:

* **OAuth Ile OpenID Connect**: yetkilendirme için OAuth kullanılır ve OpenID Connect (OIDC) kimlik doğrulaması için kullanılır. OpenID Connect, OAuth 2,0 üzerine kurulmuştur, bu nedenle terminoloji ve akış ikisi arasında benzerdir. Her ikisi de bir kullanıcının kimliğini doğrulayabilir (OpenID Connect kullanarak) ve kullanıcının sahip olduğu korumalı bir kaynağa (OAuth 2,0 kullanarak) tek bir istekte erişim yetkisi alabilir. Daha fazla bilgi için bkz. [OAuth 2,0 ve OpenID Connect Protocols](active-directory-v2-protocols.md) ve [OpenID Connect Protocol](v2-protocols-oidc.md).
* **OAuth**ve SAML için, YETKILENDIRME ve SAML kimlik doğrulaması için kullanılır. İki protokolün bir kullanıcının kimliğini doğrulamak (SAML kullanarak) ve korunan bir kaynağa erişmek için yetkilendirme alma (OAuth 2,0 kullanarak) ile birlikte nasıl kullanılabileceği hakkında daha fazla bilgi için bkz. [Microsoft Identity platform ve OAuth 2,0 SAML taşıyıcı onaylama akışı](v2-saml-bearer-assertion.md) .
* **OpenID Connect Ile SAML**: bir kullanıcının kimliğini doğrulamak Için hem OpenID Connect hem de SAML kullanılır ve çoklu oturum açmayı etkinleştirmek için kullanılır. SAML kimlik doğrulaması, genellikle Azure AD 'ye federe Active Directory Federasyon Hizmetleri (AD FS) (ADFS) gibi kimlik sağlayıcılarıyla kullanılır ve bu nedenle kurumsal uygulamalarda sık kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Kimlik doğrulama ve yetkilendirme temellerini kapsayan diğer konular için:

* Kimlik doğrulama ve yetkilendirme için erişim belirteçlerinin, yenileme belirteçlerinin ve KIMLIK belirteçlerinin nasıl kullanıldığını öğrenmek için [güvenlik belirteçlerine](security-tokens.md) bakın.
* Uygulamanızı Microsoft Identity platform ile tümleştirilebilen şekilde kaydetme süreci hakkında bilgi edinmek için bkz. [uygulama modeli](application-model.md) .
* Microsoft Identity platformunda Web, masaüstü ve mobil uygulamaların oturum açma akışı hakkında bilgi edinmek için bkz. [uygulama oturum açma akışı](app-sign-in-flow.md) .

Microsoft Identity platform 'un uyguladığı protokoller hakkında daha fazla bilgi edinmek için:

* OpenID Connect ve OAuth 2,0 standartları hakkında daha fazla bilgi için bkz. [Microsoft Identity platformunda oauth 2,0 ve OpenID Connect protokolleri](active-directory-v2-protocols.md) .
* Microsoft Identity platformunun çoklu oturum açmayı nasıl desteklediği hakkında daha fazla bilgi için bkz. [Çoklu oturum açma SAML Protokolü](single-sign-on-saml-protocol.md) .
