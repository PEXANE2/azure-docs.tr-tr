---
title: Uygulamanıza kullanıcı akışı üzerinden bir erişim belirteci geçirin
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki bir kullanıcı akışında talep olarak OAuth 2.0 kimlik sağlayıcılarıiçin erişim jetonundan nasıl geçeceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 312d093548b6e3cf3654f45d7610e8fc474a87b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187798"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanıcı akışı ndan uygulamanız için bir erişim belirteci geçirin

Azure Active Directory B2C'deki (Azure AD B2C) [kullanıcı akışı,](user-flow-overview.md) uygulamanızın kullanıcılarına bir kimlik sağlayıcısıyla kaydolma veya oturum açma fırsatı sağlar. Yolculuk başladığında, Azure AD B2C kimlik sağlayıcısından bir [erişim jetonu](tokens-overview.md) alır. Azure AD B2C bu belirteci kullanıcı hakkında bilgi almak için kullanır. Kullanıcı akışınızdaki bir talebin, belirteci Azure AD B2C'ye kaydettiğiniz uygulamalara geçirmesini sağlarsınız.

Azure AD B2C şu anda yalnızca [Facebook](identity-provider-facebook.md) ve [Google'ı](identity-provider-google.md)içeren [OAuth 2.0](authorization-code-flow.md) kimlik sağlayıcılarının erişim jetonunu geçirmeyi desteklemektedir. Diğer tüm kimlik sağlayıcıları için talep boş döndürülür.

## <a name="prerequisites"></a>Ön koşullar

* Uygulamanız [bir v2 kullanıcı akışı](user-flow-versions.md)kullanıyor olmalıdır.
* Kullanıcı akışınız bir OAuth 2.0 kimlik sağlayıcısı yla yapılandırılır.

## <a name="enable-the-claim"></a>Talebi etkinleştirme

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. **Kullanıcı akışlarını (ilkeler)** seçin ve ardından kullanıcı akışınızı seçin. Örneğin, **B2C_1_signupsignin1.**
5. **Uygulama talepleri**’ni seçin.
6. Kimlik **Sağlayıcı Erişim Belirteci** talebini etkinleştirin.

    ![Kimlik Sağlayıcı Erişim Belirteci talebini etkinleştirme](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. Kullanıcı akışını kaydetmek için **Kaydet'i** tıklatın.

## <a name="test-the-user-flow"></a>Kullanıcı akışını test edin

Uygulamalarınızı Azure AD B2C'de sınarken, Azure AD B2C belirteci, bu uygulamadaki talepleri gözden geçirmek için `https://jwt.ms` döndürülebilir.

1. Kullanıcı akışının Genel Bakış **sayfasında, Kullanıcı akışını Çalıştır'ı**seçin.
2. **Uygulama**için, daha önce kaydolduğunuz başvurunuzu seçin. Aşağıdaki örnekte belirteç görmek için **YanıtURL'si** gösterilmelidir. `https://jwt.ms`
3. **Kullanıcı akışını çalıştır'ı**tıklatın ve ardından hesap kimlik bilgilerinizle oturum açın. **idp_access_token** talebinde kimlik sağlayıcısının erişim jetonunu görmeniz gerekir.

    Aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![idp_access_token bloğu vurgulanmış jwt.ms decoded belirteci](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD [B2C belirteçleri](tokens-overview.md)hakkında daha fazla bilgi edinin.
