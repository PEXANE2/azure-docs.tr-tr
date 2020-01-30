---
title: Oturum davranışını Yapılandırma-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C 'de oturum davranışını yapılandırın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6147dc64833f44f24afe14cd1da34432a3735519
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840239"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'de oturum davranışını yapılandırma

Bu özellik, [Kullanıcı başına akış temelinde](user-flow-overview.md)ayrıntılı denetim sağlar:

- Azure AD B2C tarafından yönetilen Web uygulaması oturumlarının yaşam süreleri.
- Azure AD B2C kiracınızdaki birden çok uygulama ve Kullanıcı akışı genelinde çoklu oturum açma (SSO) davranışı.

Bu ayarlar, parola sıfırlama Kullanıcı akışları için kullanılamaz.

Azure AD B2C, Web uygulamalarında güvenli oturum açmayı etkinleştirmek için [OpenID Connect kimlik doğrulama protokolünü](openid-connect.md) destekler. Web uygulaması oturumlarını yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

## <a name="session-behavior-properties"></a>Oturum davranışı özellikleri

- **Web uygulaması oturumu ömrü (dakika)** -başarılı kimlik doğrulaması sırasında kullanıcının tarayıcısında depolanan Azure AD B2C's oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 1440 dakika.
    - En düşük (kapsamlı) = 15 dakika.
    - Maksimum (dahil) = 1440 dakika.
- **Web uygulaması oturumu zaman aşımı** -bu anahtar **mutlak**olarak ayarlandıysa, **Web uygulaması oturumu yaşam süresi (dakika)** tarafından belirtilen süre dolduktan sonra Kullanıcı yeniden kimlik doğrulamaya zorlanır. Bu anahtar **kayan** olarak ayarlandıysa (varsayılan ayar), Kullanıcı Web uygulamanızda sürekli olarak etkin olduğu sürece Kullanıcı oturum açmış olarak kalır.
- **Çoklu oturum açma yapılandırması** B2C kiracınızda birden çok uygulamanız ve Kullanıcı akışınız varsa, **Çoklu oturum açma yapılandırma** özelliğini kullanarak bunlar genelinde kullanıcı etkileşimini yönetebilirsiniz. Özelliğini aşağıdaki ayarlardan birine ayarlayabilirsiniz:
    - **Kiracı** -Bu ayar varsayılan ayardır. Bu ayarın kullanılması, B2C kiracınızdaki birden fazla uygulamanın ve Kullanıcı akışının aynı kullanıcı oturumunu paylaşmasına izin verir. Örneğin, bir Kullanıcı bir uygulamada oturum açtıktan sonra, Kullanıcı, contoso Boğmacy ' a erişerek, daha da sorunsuz bir şekilde oturum açabilir.
    - **Uygulama** -Bu ayar, başka uygulamalardan bağımsız olarak bir uygulama için Kullanıcı oturumunu korumanıza olanak sağlar. Örneğin, kullanıcının contoso sahte Macy 'de (aynı kimlik bilgileriyle) oturum açmasını isterseniz, Kullanıcı zaten contoso alışverişe kaydolsa bile, aynı B2C kiracısındaki başka bir uygulama da vardır.
    - **İlke** -Bu ayar, bir kullanıcı oturumunun, kendisini kullanan uygulamalardan bağımsız olarak bir Kullanıcı akışı için bakımını yapmanıza olanak sağlar. Örneğin, Kullanıcı oturum açmışsa ve çok faktörlü kimlik doğrulaması (MFA) adımını tamamlamışsa, Kullanıcı akışına bağlı olan oturumun süresi dolana kadar, kullanıcıya birden fazla uygulamanın daha yüksek güvenlik bölümlerine erişim verilebilir.
    - **Devre dışı** -Bu ayar, kullanıcıyı ilkenin her çalıştırmasında Kullanıcı akışının tamamı boyunca çalışmaya zorlar.

Aşağıdaki kullanım örnekleri şu özellikler kullanılarak etkinleştirilir:

- Uygun Web uygulaması oturum ömrünü ayarlayarak sektörün güvenlik ve uyumluluk gereksinimlerini karşılayın.
- Kullanıcının Web uygulamanızın yüksek güvenlikli bir bölümüyle etkileşimi sırasında ayarlanan süre dolduktan sonra kimlik doğrulamaya zorla.

## <a name="configure-the-properties"></a>Özellikleri yapılandırma

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip Azure AD B2C kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kullanıcı akışları (ilkeler)** seçeneğini belirleyin.
5. Daha önce oluşturduğunuz kullanıcı akışını açın.
6. **Özellikler**’i seçin.
7. **Web uygulaması oturumu yaşam süresi (dakika)** , **Web uygulaması oturumu zaman aşımı**, **Çoklu oturum açma yapılandırması**yapılandırın ve gerektiğinde **oturum kapatma isteklerinde kimlik belirteci gerektir** .

    ![Azure portal oturum davranışı özelliği ayarları](./media/session-behavior/session-behavior.png)

8. **Save (Kaydet)** düğmesine tıklayın.
