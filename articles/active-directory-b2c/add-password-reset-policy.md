---
title: Parola sıfırlama akışı ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir parola sıfırlama akışını ayarlamayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/16/2020
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 6e6e4aa4ece781f415308b638323f8d4d4b34038
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618940"
---
# <a name="set-up-a-password-reset-flow-in-azure-active-directory-b2c"></a>Azure Active Directory B2C bir parola sıfırlama akışı ayarlayın

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

## <a name="password-rest-flow"></a>Parola Rest akışı

Parola sıfırlama ilkesi, kullanıcıların kendi unutulan parolalarını sıfırlamasına izin verir. Parola sıfırlama akışı aşağıdaki adımları içerir: 
1. Kullanıcı, kaydolma ve oturum açma sayfasından "parolanızı unuttum mı?" seçeneğine tıklasın. ücretsiz bir hesap oluşturabilirsiniz. Azure AD B2C, uygulamanıza AADB2C90118 hata kodunu döndürür. Uygulama, parola sıfırlama ilkesini çağırarak bu hata kodunu işler. 
1. Kullanıcılar, zaman aşımı süresi bir saat geçiş kodu ile e-postalarını sağlar ve doğrular.
1. Yeni bir parola girin.

![Parola sıfırlama akışı](./media/add-password-reset-policy/password-reset-flow.png)

## <a name="prerequisites"></a>Ön koşullar

Henüz yapmadıysanız, [bir Web uygulamasını Azure Active Directory B2C kaydedin](tutorial-register-applications.md).

::: zone pivot="b2c-user-flow"

## <a name="create-a-password-reset-user-flow"></a>Parola sıfırlama Kullanıcı akışı oluşturma

Uygulamanızın kullanıcılarının parolasını sıfırlamasına olanak tanımak için, parola sıfırlama kullanıcı akışını kullanırsınız.

1. Azure AD B2C kiracı genel bakış menüsünde **Kullanıcı akışları**' nı seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.
1. **Kullanıcı akışı oluştur** sayfasında, **parola sıfırlama** Kullanıcı akışını seçin. 
1. **Sürüm seçin** altında **Önerilen**' i seçin ve ardından **Oluştur**' u seçin.
1. Kullanıcı akışı için bir **ad** girin. Örneğin, *passwordreset1*.
1. **Kimlik sağlayıcıları** için **e-posta adresini kullanarak parolayı sıfırlamayı** etkinleştirin.
2. Uygulama talepleri bölümünde **daha fazla göster** ' e tıklayın ve uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Kullanıcının Nesne Kimliği**’ni seçin.
3. **Tamam** düğmesine tıklayın.
4. Kullanıcı akışını eklemek için **Oluştur** ' a tıklayın. *B2C_1* bir ön eki otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Genel Bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama** için, daha önce kaydettiğiniz *WebApp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır**' a tıklayın, daha önce oluşturduğunuz hesabın e-posta adresini doğrulayın ve **devam**' ı seçin.
1. Artık Kullanıcı parolasını değiştirme fırsatına sahipsiniz. Parolayı değiştirin ve **devam**' ı seçin. Belirteç öğesine döner `https://jwt.ms` ve size gösterilmesi gerekir.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-password-reset-policy"></a>Parola sıfırlama ilkesi oluşturma

Özel ilkeler, Kullanıcı, neys tanımlamak için Azure AD B2C kiracınıza yüklediğiniz XML dosyaları kümesidir. Kaydolma ve oturum açma, parola sıfırlama ve profil düzenlemesi ilkesi dahil olmak üzere önceden oluşturulmuş birkaç ilke ile başlangıç paketleri sunuyoruz. Daha fazla bilgi için, bkz. [Azure AD B2C özel ilkelerle çalışmaya başlama](custom-policy-get-started.md).

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

[Profil düzenlemesi akışı](add-profile-editing-policy.md)ayarlayın.