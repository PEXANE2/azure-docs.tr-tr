---
title: Bir kimlik sağlayıcısı olarak Facebook ekleme-Azure AD
description: Dış kullanıcıların (konuklar) kendi Facebook hesaplarıyla Azure AD uygulamalarınızda oturum açmasını sağlamak için Facebook ile federasyona Federin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a92fbd254f223e2c7eb70a4e86bb7e904294395e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597712"
---
# <a name="add-facebook-as-an-identity-provider-for-external-identities"></a>Facebook 'ı dış kimlikler için kimlik sağlayıcısı olarak ekleyin

Self Servis kaydolma Kullanıcı akışlarınıza (Önizleme) Facebook ekleyebilirsiniz, böylece kullanıcılar kendi Facebook hesaplarını kullanarak uygulamalarınızda oturum açabilirler. Kullanıcıların Facebook kullanarak oturum açmalarına izin vermek için öncelikle kiracınız için [self servis kaydolma 'yı etkinleştirmeniz](self-service-sign-up-user-flow.md) gerekir. Bir kimlik sağlayıcısı olarak Facebook ekledikten sonra, uygulama için bir Kullanıcı akışı ayarlayın ve oturum açma seçeneklerinden biri olarak Facebook ' u seçin.

## <a name="create-an-app-in-the-facebook-developers-console"></a>Facebook geliştiricileri konsolunda uygulama oluşturma

Bir Facebook hesabını [kimlik sağlayıcısı](identity-providers.md)olarak kullanmak Için, Facebook geliştiricileri konsolunda bir uygulama oluşturmanız gerekir. Henüz bir Facebook hesabınız yoksa kaydolabilirsiniz [https://www.facebook.com/](https://www.facebook.com) .

> [!NOTE]  
> Aşağıdaki 3 ve 16. adımlarda aşağıdaki URL 'Leri kullanın.
> - **Site URL 'si** için girin `https://login.microsoftonline.com` .
> - **Geçerli OAuth yeniden yönlendirme URI 'leri**için girin `https://login.microsoftonline.com/te/<tenant-id>/oauth2/authresp` . `<tenant-ID>`Azure Active Directory genel bakış dikey penceresinde bulabilirsiniz.


1. Facebook hesabı kimlik bilgilerinizle [geliştiriciler Için Facebook](https://developers.facebook.com/) 'ta oturum açın.
2. Daha önce yapmadıysanız, Facebook geliştiricisi olarak kaydolmanız gerekir. Bunu yapmak için sayfanın sağ üst **köşesinden başlayın ' ı seçin,** Facebook ilkelerini kabul edin ve kayıt adımlarını uygulayın.
3. **Uygulamalarım** ' ı ve ardından **uygulama oluştur**' u seçin.
4. Bir **görünen ad** ve geçerli bir **Iletişim e-postası**girin.
5. **Uygulama kimliği oluştur**' u seçin. Bu, Facebook platformu ilkelerini kabul etmenizi ve çevrimiçi bir güvenlik denetimi tamamlamanızı gerektirebilir.
6. **Ayarlar**  >  **temel**öğesini seçin.
7. Örneğin Iş ve sayfalar gibi bir **Kategori**seçin. Bu değer Facebook için gereklidir, ancak Azure AD için kullanılmaz.
8. Sayfanın alt kısmındaki **Platform Ekle**' yi ve ardından **Web sitesi**' ni seçin.
9. **Site URL 'si**' nde, uygun URL 'yi girin (yukarıda belirtilen).
10. **Gizlilik ilkesi URL 'si**' nde, uygulamanız için gizlilik bilgilerini tuttuğunuz sayfanın URL 'sini girin (örneğin,) `http://www.contoso.com` .
11. **Değişiklikleri Kaydet**' i seçin.
12. Sayfanın üst kısmında, **uygulama kimliği**değerini kopyalayın.
13. **Göster** ' i seçin ve **uygulama gizli anahtarı**değerini kopyalayın. Facebook 'ı kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için her ikisini de kullanabilirsiniz. **Uygulama gizli anahtarı** önemli bir güvenlik kimlik bilgileridir.
14. **Ürünlerin**yanındaki artı işaretini ve ardından **Facebook oturumu**altında **Ayarla** ' yı seçin.
15. **Facebook oturum açma**bölümünde **Ayarlar**' ı seçin.
16. **Geçerli OAuth yeniden yönlendirme URI 'lerinde**, uygun URL 'yi girin (yukarıda belirtilen).
17. Sayfanın alt kısmındaki **Değişiklikleri Kaydet** ' i seçin.
18. Facebook uygulamanızı Azure AD **'de** kullanılabilir hale getirmek için sayfanın sağ üst kısmındaki durum seçicisini seçin ve uygulamayı ortak hale getirmek için açın, sonra **geçiş modu**' nu seçin. Bu noktada, durumun **geliştirme aşamasında** **canlı**olarak değiştirilmesi gerekir.
    
## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Bir Facebook hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure Portal](https://portal.azure.com) Azure AD kiracınızın genel yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Tüm kimlik sağlayıcıları**' nı seçin ve **Facebook**' u seçin.
5. **ISTEMCI kimliği**için, daha önce oluşturduğunuz Facebook UYGULAMASıNıN uygulama **kimliğini** girin.
6. **İstemci parolası**için, kaydettiğiniz **uygulama gizli** anahtarını girin.

   ![Sosyal kimlik sağlayıcısı Ekle sayfasını gösteren ekran görüntüsü](media/facebook-federation/add-social-identity-provider-page.png)

7. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Dış kullanıcıları işbirliği için davet etme](add-users-administrator.md)
- [Bir uygulamaya self servis kaydolma ekleme](self-service-sign-up-user-flow.md)
