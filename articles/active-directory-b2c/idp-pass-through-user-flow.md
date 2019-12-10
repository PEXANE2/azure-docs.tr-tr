---
title: Uygulamanıza Kullanıcı akışı aracılığıyla erişim belirteci geçirme
titleSuffix: Azure AD B2C
description: OAuth 2,0 kimlik sağlayıcıları için bir erişim belirtecini Azure Active Directory B2C bir Kullanıcı akışında talep olarak nasıl geçitireceğinizi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 345924c10feabe9a7d5f50905689a4a0edb065ce
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74948820"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>Azure Active Directory B2C uygulamasında uygulamanıza bir Kullanıcı akışı aracılığıyla erişim belirteci geçirin

Azure Active Directory B2C (Azure AD B2C) bir [Kullanıcı akışı](active-directory-b2c-reference-policies.md) , uygulamanızın kullanıcılarına bir kimlik sağlayıcısına kaydolma veya oturum açma olanağı sağlar. Yolculuğun başladığı zaman, Azure AD B2C kimlik sağlayıcısından bir [erişim belirteci](active-directory-b2c-reference-tokens.md) alır. Azure AD B2C, kullanıcı hakkındaki bilgileri almak için bu belirteci kullanır. Kullanıcı akışındaki bir talebi, belirteci, Azure AD B2C kaydettiğinizde bulunan uygulamalara geçirmek için etkinleştirirsiniz.

Azure AD B2C Şu anda yalnızca [Facebook](active-directory-b2c-setup-fb-app.md) ve [Google](active-directory-b2c-setup-goog-app.md)içeren [OAuth 2,0](active-directory-b2c-reference-oauth-code.md) kimlik sağlayıcılarının erişim belirtecinin geçirilmesini destekler. Diğer tüm kimlik sağlayıcıları için talep boş döndürülür.

## <a name="prerequisites"></a>Önkoşullar

* Uygulamanızın [V2 Kullanıcı akışı](user-flow-versions.md)kullanıyor olması gerekir.
* Kullanıcı akışınız bir OAuth 2,0 kimlik sağlayıcısı ile yapılandırılır.

## <a name="enable-the-claim"></a>Talebi etkinleştir

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. **Kullanıcı akışları ' nı (ilkeler)** seçin ve ardından Kullanıcı akışınızı seçin. Örneğin, **B2C_1_signupsignin1**.
5. **Uygulama talepleri**’ni seçin.
6. **Kimlik sağlayıcısı erişim belirteci** talebini etkinleştirin.

    ![Kimlik sağlayıcısı erişim belirteci talebini etkinleştir](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. Kullanıcı akışını kaydetmek için **Kaydet** ' e tıklayın.

## <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

Azure AD B2C ' de Uygulamalarınızı sınarken, içindeki talepleri gözden geçirmek için Azure AD B2C belirtecinin `https://jwt.ms` döndürüldüğünden yararlı olabilir.

1. Kullanıcı akışının genel bakış sayfasında, **Kullanıcı akışını Çalıştır**' ı seçin.
2. **Uygulama**için, daha önce kaydetmiş olduğunuz uygulamanızı seçin. Aşağıdaki örnekteki belirteci görmek için, **yanıt URL 'si** `https://jwt.ms`göstermelidir.
3. **Kullanıcı akışını Çalıştır**' a tıklayın ve ardından hesap kimlik bilgilerinizle oturum açın. **İdp_access_token** talebinde kimlik sağlayıcısının erişim belirtecini görmeniz gerekir.

    Aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![İdp_access_token bloğu vurgulanmış şekilde jwt.ms içinde kodu çözülmüş belirteç](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.PNG)

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD B2C belirteçlerine genel bakış](active-directory-b2c-reference-tokens.md)hakkında daha fazla bilgi edinin.
