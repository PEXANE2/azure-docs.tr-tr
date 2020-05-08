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
ms.date: 04/28/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 61c0202d12756201d45fe829078d84382e44b54e
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584304"
---
# <a name="authentication-vs-authorization"></a>Kimlik doğrulama ve yetkilendirme karşılaştırması

Bu makalede kimlik doğrulaması ve yetkilendirme tanımlanmaktadır ve Web uygulamalarınızda, Web API 'Lerinde veya korumalı Web API 'Leri çağıran uygulamalarda kullanıcıların kimliğini doğrulamak ve yetkilendirmek için Microsoft Identity platformunu nasıl kullanabileceğiniz kısaca ele alınmaktadır. Bildiğiniz bir terimi görürseniz, temel kavramları kapsayan [Sözlüğümüzü](developer-glossary.md) veya [Microsoft Identity platform videolarımızı](identity-videos.md) deneyin.

## <a name="authentication"></a>Kimlik Doğrulaması

**Kimlik doğrulama** , sizin olduğunu söylediğinizde sizin olduğunu kanıtlama işlemidir. Kimlik doğrulaması bazen AuthN şeklinde kısaltılabilir. Microsoft Identity platform kimlik doğrulamasını işlemek için [OpenID Connect](https://openid.net/connect/) protokolünü uygular.

## <a name="authorization"></a>Yetkilendirme

**Yetkilendirme** , kimliği doğrulanmış bir tarafın bir şeyi yapmak için izin verme işlemidir. Hangi verilerin erişebileceğini ve bu verilerle yapabileceklerinizi belirtir. Yetkilendirme bazen AuthZ şeklinde kısaltılabilir. Microsoft Identity platform yetkilendirmeyi işlemek için [OAuth 2,0](https://oauth.net/2/) protokolünü uygular.

## <a name="authentication-and-authorization-using-the-microsoft-identity-platform"></a>Microsoft Identity platformunu kullanarak kimlik doğrulaması ve yetkilendirme

Her birinin kendi Kullanıcı adı ve parola bilgilerini korumalarına veya birden çok uygulama arasında Kullanıcı eklemeniz veya kaldırmanız gerektiğinde yüksek bir yönetim yükü sunan uygulamalar oluşturmak yerine, uygulamalar bu sorumluluğu merkezi bir kimlik sağlayıcısına devredebilir.

Azure Active Directory (Azure AD), bulutta merkezi bir kimlik sağlayıcıdır. Kimlik doğrulama ve yetkilendirmeyi yetkilendirme, bir kullanıcının belirli bir konumda olmasını gerektiren koşullu erişim ilkeleri, Multi-Factor Authentication 'ın kullanılması ve bir kullanıcının bir kez oturum açmasını ve ardından aynı merkezi dizini paylaşan tüm Web uygulamalarında otomatik olarak oturum açmasını sağlar. Bu yetenek, **Çoklu oturum açma (SSO)** olarak adlandırılır.

Microsoft Identity platform; OAuth 2,0 ve OpenID Connect gibi sektör standardı protokoller desteğiyle ve kodlamaya hızlı bir şekilde başlamanıza yardımcı olması için farklı platformlar için açık kaynak kitaplıkların yanı sıra, uygulama geliştiricileri için kimlik doğrulama ve yetkilendirmeyi basitleştirir. Geliştiricilerin tüm Microsoft kimliklerinden oturum açmasını, [Microsoft Graph](https://developer.microsoft.com/graph/), diğer Microsoft API 'lerini veya geliştiricilerin oluşturduğu API 'leri çağıracağınız belirteçler almasını sağlar. Daha fazla bilgi için bkz. [Microsoft Identity platform 'un gelişi](about-microsoft-identity-platform.md).

## <a name="next-steps"></a>Sonraki adımlar

Kimlik doğrulama ve yetkilendirme temellerini kapsayan diğer konular için:

* Kimlik doğrulama ve yetkilendirme için erişim belirteçlerinin, yenileme belirteçlerinin ve KIMLIK belirteçlerinin nasıl kullanıldığını öğrenmek için [güvenlik belirteçlerine](security-tokens.md) bakın.
* Uygulamanızı Microsoft Identity platform ile tümleştirilebilen şekilde kaydetme süreci hakkında bilgi edinmek için bkz. [uygulama modeli](application-model.md) .
* Microsoft Identity platformunda Web, masaüstü ve mobil uygulamaların oturum açma akışı hakkında bilgi edinmek için bkz. [uygulama oturum açma akışı](app-sign-in-flow.md) .

Microsoft Identity platform 'un uyguladığı protokoller hakkında daha fazla bilgi edinmek için:

* OpenID Connect ve OAuth 2,0 standartları hakkında daha fazla bilgi için bkz. [Microsoft Identity platformunda oauth 2,0 ve OpenID Connect protokolleri](active-directory-v2-protocols.md) .
* Microsoft Identity platformunun çoklu oturum açmayı nasıl desteklediği hakkında daha fazla bilgi için bkz. [Çoklu oturum açma SAML Protokolü](single-sign-on-saml-protocol.md) .
