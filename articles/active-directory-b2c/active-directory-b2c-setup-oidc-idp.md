---
title: OpenID Connect ile kaydolma ve oturum açma ayarlama-Azure Active Directory B2C
description: Azure Active Directory B2C kullanarak, OpenID Connect ile kaydolma ve oturum açma ayarlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 720deb28ce124af23035337ac88cfb1d37fc7c53
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69509698"
---
# <a name="set-up-sign-up-and-sign-in-with-openid-connect-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak OpenID Connect ile kaydolma ve oturum açma ayarlama

[OpenID Connect](active-directory-b2c-reference-oidc.md) , OAuth 2,0 üzerinde oluşturulmuş, güvenli Kullanıcı oturumu açma için kullanılabilen bir kimlik doğrulama protokolüdür. Bu protokolü kullanan kimlik sağlayıcılarının çoğu Azure AD B2C desteklenir. Bu makalede, Kullanıcı akışlarınıza özel OpenID Connect kimlik sağlayıcılarını nasıl ekleyebileceğiniz açıklanmaktadır.

## <a name="add-the-identity-provider"></a>Kimlik sağlayıcısını ekleme

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. **Kimlik sağlayıcıları**' nı seçin ve ardından **Ekle**' ye tıklayın.
5. **Kimlik sağlayıcısı türü**Için **OpenID Connect**' i seçin.

## <a name="configure-the-identity-provider"></a>Kimlik sağlayıcısını yapılandırma

Her OpenID Connect kimlik sağlayıcısı, oturum açma işlemini gerçekleştirmek için gereken bilgilerin çoğunu içeren bir meta veri belgesi tanımlar. Bu, kullanılacak URL 'Ler ve hizmetin ortak imzalama anahtarlarının konumu gibi bilgileri içerir. OpenID Connect meta veri belgesi her zaman içinde `.well-known\openid-configuration`biten bir uç noktada bulunur. Eklemek istediğiniz OpenID Connect kimlik sağlayıcısı için, meta veri URL 'sini girin.

Kullanıcıların oturum açmalarına izin vermek için, kimlik sağlayıcısı geliştiricilerin hizmetine bir uygulama kaydetmesini gerektirir. Bu uygulamanın, **ISTEMCI kimliği** ve **istemci PAROLASı**olarak adlandırılan kimliği vardır. Bu değerleri kimlik sağlayıcısından kopyalayın ve bunlara karşılık gelen alanlara girin.

> [!NOTE]
> İstemci parolası isteğe bağlıdır. Ancak, belirteç kodunu değiştirmek için gizli dizi kullanan [yetkilendirme kodu akışını](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)kullanmak istiyorsanız bir istemci gizli anahtarı girmeniz gerekir.

Kapsam, özel kimlik sağlayıcınızdan toplamak istediğiniz bilgileri ve izinleri tanımlar. Kimlik sağlayıcısından kimlik belirtecini almak için OpenID Connect isteklerinin `openid` kapsam değerini içermesi gerekir. KIMLIK belirteci olmadan, kullanıcılar özel kimlik sağlayıcısını kullanarak Azure AD B2C oturum açamaz. Diğer kapsamlar boşlukla ayırarak eklenebilir. Diğer kapsamların kullanılabilir olduğunu görmek için özel kimlik sağlayıcısının belgelerine bakın.

Yanıt türü, özel kimlik sağlayıcısının ilk çağrısında `authorization_endpoint` ne tür bilgilerin geri gönderileceğini açıklar. Aşağıdaki yanıt türleri kullanılabilir:

- `code`: [Yetkilendirme kodu akışına](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)göre, Azure AD B2C bir kod geri döndürülür. Azure AD B2C, `token_endpoint` belirtecin kodunu değiştirmek için ' a çağrı devam eder.
- `token`: Özel kimlik sağlayıcısından Azure AD B2C bir erişim belirteci geri döndürülür.
- `id_token`: Özel kimlik sağlayıcısından Azure AD B2C bir KIMLIK belirteci geri döndürülür.

Yanıt modu, verileri özel kimlik sağlayıcısından Azure AD B2C 'e geri göndermek için kullanılması gereken yöntemi tanımlar. Aşağıdaki yanıt modları kullanılabilir:

- `form_post`: Bu yanıt modu en iyi güvenlik için önerilir. Yanıt http `POST` yöntemiyle iletilir ve `application/x-www-form-urlencoded` biçimi kullanılarak gövdede kodlanacak kodla veya belirtece sahip olur.
- `query`: Kod veya belirteç bir sorgu parametresi olarak döndürülür.

Etki alanı ipucu, kullanıcının kullanılabilir kimlik sağlayıcılarının listesi arasında seçim yapmasını sağlamak yerine, belirtilen kimlik sağlayıcısının oturum açma sayfasına doğrudan atlamak için kullanılabilir. Bu tür davranışa izin vermek için, etki alanı ipucu için bir değer girin. Özel kimlik sağlayıcısına geçmek için, oturum açma için Azure AD B2C çağırırken `domain_hint=<domain hint value>` parametresini isteğinizin sonuna ekleyin.

Özel kimlik sağlayıcısı bir KIMLIK belirtecini Azure AD B2C 'e geri gönderdikten sonra, Azure AD B2C alınan belirteçteki talepleri Azure AD B2C tanıdığı ve kullandığı taleplerle eşleyebilmelidir. Aşağıdaki eşlemelerin her biri için, kimlik sağlayıcısının belirteçlerine geri döndürülen talepleri anlamak üzere özel kimlik sağlayıcısının belgelerine bakın:

- `User ID`: Oturum açmış kullanıcı için benzersiz tanımlayıcıyı sağlayan talebi girin.
- `Display Name`: Kullanıcı için görünen adı veya tam adı sağlayan talebi girin.
- `Given Name`: Kullanıcının adını sağlayan talebi girin.
- `Surname`: Kullanıcının soyadını sağlayan talebi girin.
- `Email`: Kullanıcının e-posta adresini sağlayan talebi girin.

