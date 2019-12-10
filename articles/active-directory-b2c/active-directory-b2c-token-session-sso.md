---
title: Oturum ve çoklu oturum açma yapılandırması
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C 'de oturum ve çoklu oturum açma (SSO) yapılandırması.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 736dd1f0490c2c9c7c4f526df96dd5ace6a1f819
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74950078"
---
# <a name="session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Azure Active Directory B2C oturum ve çoklu oturum açma yapılandırması

Bu özellik, [Kullanıcı başına akış temelinde](active-directory-b2c-reference-policies.md)ayrıntılı denetim sağlar:

- Azure AD B2C tarafından yönetilen Web uygulaması oturumlarının yaşam süreleri.
- Azure AD B2C kiracınızdaki birden çok uygulama ve Kullanıcı akışı genelinde çoklu oturum açma (SSO) davranışı.

## <a name="session-behavior"></a>Oturum davranışı

Azure AD B2C, Web uygulamalarında güvenli oturum açmayı etkinleştirmek için [OpenID Connect kimlik doğrulama protokolünü](active-directory-b2c-reference-oidc.md) destekler. Web uygulaması oturumlarını yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

- **Web uygulaması oturumu ömrü (dakika)** -başarılı kimlik doğrulaması sırasında kullanıcının tarayıcısında depolanan Azure AD B2C's oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 1440 dakika.
    - En düşük (kapsamlı) = 15 dakika.
    - Maksimum (dahil) = 1440 dakika.
- **Web uygulaması oturumu zaman aşımı** -bu anahtar **mutlak**olarak ayarlandıysa, **Web uygulaması oturumu yaşam süresi (dakika)** tarafından belirtilen süre dolduktan sonra Kullanıcı yeniden kimlik doğrulamaya zorlanır. Bu anahtar **kayan** olarak ayarlandıysa (varsayılan ayar), Kullanıcı Web uygulamanızda sürekli olarak etkin olduğu sürece Kullanıcı oturum açmış olarak kalır.

Aşağıdaki kullanım örnekleri şu özellikler kullanılarak etkinleştirilir:

- Uygun Web uygulaması oturum ömrünü ayarlayarak sektörün güvenlik ve uyumluluk gereksinimlerini karşılayın.
- Kullanıcının Web uygulamanızın yüksek güvenlikli bir bölümüyle etkileşimi sırasında ayarlanan süre dolduktan sonra kimlik doğrulamaya zorla.

Bu ayarlar, parola sıfırlama Kullanıcı akışları için kullanılamaz.

## <a name="single-sign-on-sso-configuration"></a>Çoklu oturum açma (SSO) yapılandırması

B2C kiracınızda birden çok uygulamanız ve Kullanıcı akışınız varsa, **Çoklu oturum açma yapılandırma** özelliğini kullanarak bunlar genelinde kullanıcı etkileşimini yönetebilirsiniz. Özelliğini aşağıdaki ayarlardan birine ayarlayabilirsiniz:

- **Kiracı** -Bu ayar varsayılan ayardır. Bu ayarın kullanılması, B2C kiracınızdaki birden fazla uygulamanın ve Kullanıcı akışının aynı kullanıcı oturumunu paylaşmasına izin verir. Örneğin, bir Kullanıcı bir uygulamada oturum açtıktan sonra, Kullanıcı, contoso Boğmacy ' a erişerek, daha da sorunsuz bir şekilde oturum açabilir.
- **Uygulama** -Bu ayar, başka uygulamalardan bağımsız olarak bir uygulama için Kullanıcı oturumunu korumanıza olanak sağlar. Örneğin, kullanıcının contoso sahte Macy 'de (aynı kimlik bilgileriyle) oturum açmasını isterseniz, Kullanıcı zaten contoso alışverişe kaydolsa bile, aynı B2C kiracısındaki başka bir uygulama da vardır.
- **İlke** -Bu ayar, bir kullanıcı oturumunun, kendisini kullanan uygulamalardan bağımsız olarak bir Kullanıcı akışı için bakımını yapmanıza olanak sağlar. Örneğin, Kullanıcı oturum açmışsa ve çok faktörlü kimlik doğrulaması (MFA) adımını tamamlamışsa, Kullanıcı akışına bağlı olan oturumun süresi dolana kadar, kullanıcıya birden fazla uygulamanın daha yüksek güvenlik bölümlerine erişim verilebilir.
- **Devre dışı** -Bu ayar, kullanıcıyı ilkenin her çalıştırmasında Kullanıcı akışının tamamı boyunca çalışmaya zorlar.

Bu ayarlar, parola sıfırlama Kullanıcı akışları için kullanılamaz.

