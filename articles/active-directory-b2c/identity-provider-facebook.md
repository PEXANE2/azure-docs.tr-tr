---
title: Kayıt ayarlama ve Facebook hesabı ile oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda Facebook hesabı bulunan müşterilere kaydolma ve oturum açma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 37fc33ae8084a2b4e99e7b5dc417eac70060eef5
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97516193"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak kayıt ve Facebook hesabı ile oturum açma ayarlama

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-facebook-application"></a>Facebook uygulaması oluşturma

Azure Active Directory B2C (Azure AD B2C) ' de bir Facebook hesabını [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Henüz bir Facebook hesabınız yoksa kaydolabilirsiniz [https://www.facebook.com/](https://www.facebook.com/) .

1. Facebook hesabı kimlik bilgilerinizle [geliştiriciler Için Facebook](https://developers.facebook.com/) 'ta oturum açın.
1. Daha önce yapmadıysanız, Facebook geliştiricisi olarak kaydolmanız gerekir. Bunu yapmak için sayfanın sağ üst **köşesinden başlayın ' ı seçin,** Facebook ilkelerini kabul edin ve kayıt adımlarını uygulayın.
1. **Uygulamalarım** ' ı ve ardından **uygulama oluştur**' u seçin.
1. **Derleme bağlı deneyimleri**' nı seçin.
1. Bir **görünen ad** ve geçerli bir **Iletişim e-postası** girin.
1. **Uygulama kimliği oluştur**' u seçin. Bu, Facebook platformu ilkelerini kabul etmenizi ve çevrimiçi bir güvenlik denetimi tamamlamanızı gerektirebilir.
1. **Ayarlar**  >  **temel** öğesini seçin.
    1. Örneğin, bir **Kategori** seçin `Business and Pages` . Bu değer Facebook için gereklidir, ancak Azure AD B2C için kullanılmaz.
    1. **Hizmet koşulları URL 'si** IÇIN bir URL girin (örneğin,) `http://www.contoso.com/tos` . İlke URL 'SI, uygulamanız için hüküm ve koşullar sağlamak üzere tuttuğunuz bir sayfasıdır.
    1. **Gizlilik ILKESI URL**'si IÇIN bir URL girin, örneğin `http://www.contoso.com/privacy` . İlke URL 'SI, uygulamanız için gizlilik bilgilerini sağlamak üzere tuttuğunuz bir sayfasıdır.
1. Sayfanın alt kısmındaki **Platform Ekle**' yi ve ardından **Web sitesi**' ni seçin.
1. **Site URL 'si**' nde, Web sitenizin adresini girin, örneğin `https://contoso.com` . 
1. **Değişiklikleri Kaydet**' i seçin.
1. Sayfanın üst kısmında, **uygulama kimliği** değerini kopyalayın.
1. **Göster** ' i seçin ve **uygulama gizli anahtarı** değerini kopyalayın. Facebook 'ı kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için her ikisini de kullanabilirsiniz. **Uygulama gizli anahtarı** önemli bir güvenlik kimlik bilgileridir.
1. Menüden, **Ürünler**' in yanındaki **artı** işaretini seçin. **Uygulamanıza ürün ekleme** altında, **Facebook oturumu** altında **Ayarla** ' yı seçin.
1. Menüden **Facebook oturum açma**' yı seçin, **Ayarlar**' ı seçin.
1. **Geçerli OAuth yeniden yönlendirme URI 'lerinde**, girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` . `your-tenant-name`Kiracınızın adıyla değiştirin. Sayfanın alt kısmındaki **Değişiklikleri Kaydet** ' i seçin.
1. Facebook uygulamanızı Azure AD B2C için kullanılabilir hale getirmek için sayfanın sağ üst kısmındaki durum seçiciyi seçin ve uygulamayı ortak hale **getirmek için açın** , sonra **Mod Değiştir**' i seçin.  Bu noktada durum, **geliştirme aşamasında** **canlı** olarak değiştirilmelidir.

::: zone pivot="b2c-user-flow"

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Bir Facebook hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve **Facebook**' u seçin.
1. Bir **ad** girin. Örneğin, *Facebook*.
1. **ISTEMCI kimliği** için, daha önce oluşturduğunuz Facebook UYGULAMASıNıN uygulama kimliğini girin.
1. **İstemci parolası** için, kaydettiğiniz uygulama gizli anahtarını girin.
1. **Kaydet**’i seçin.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="add-facebook-as-an-identity-provider"></a>Bir kimlik sağlayıcısı olarak Facebook ekleme

1. `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** Dosyasında, değerini `client_id` Facebook uygulama kimliğiyle değiştirin:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-facebook-identity-provider-to-a-user-flow"></a>Bir Kullanıcı akışına Facebook kimlik sağlayıcısı ekleme 

1. Azure AD B2C kiracınızda **Kullanıcı akışları**' nı seçin.
1. Facebook kimlik sağlayıcısına istediğiniz kullanıcı akışına tıklayın.
1. **Sosyal kimlik sağlayıcıları** altında **Facebook**' u seçin.
1. **Kaydet**’i seçin.
1. İlkenizi test etmek için **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama** için, daha önce kaydettiğiniz *testapp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır** 'a tıklayın

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="upload-and-test-the-policy"></a>İlkeyi karşıya yükleme ve test etme

Oluşturduğunuz Kullanıcı yolculuğunu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin.

1. *TrustFrameworkExtensions.xml* dosyasını kiracınıza yükleyin.
1. **Özel ilkeler** altında **B2C_1A_signup_signin**' yi seçin.
1. **Uygulama Seç** için, daha önce kaydettiğiniz *testapp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Şimdi Çalıştır** ' ı seçin ve Facebook ile oturum açmak ve özel ilkeyi test etmek için Facebook ' u seçin.

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

[Facebook belirtecini uygulamanıza nasıl geçirebileceğinizi](idp-pass-through-user-flow.md)öğrenin.
