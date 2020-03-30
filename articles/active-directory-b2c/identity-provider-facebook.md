---
title: Facebook hesabıyla kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda Facebook hesabı olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cd0e19de88a6a65d72a2e7e19f7fca2a94d8da55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188282"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak bir Facebook hesabıyla kaydolma ve oturum açma

## <a name="create-a-facebook-application"></a>Facebook uygulaması oluşturma

Bir Facebook hesabını Azure Active Directory B2C'de (Azure AD B2C) [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir Facebook hesabınız yoksa, 'ye [https://www.facebook.com/](https://www.facebook.com/)kaydolabilirsiniz.

1. Facebook hesap kimlik bilgilerinize sahip [geliştiriciler için Facebook'ta](https://developers.facebook.com/) oturum açın.
1. Bunu zaten yapmadıysanız, bir Facebook geliştiricisi olarak kaydolmanız gerekir. Bunu yapmak için sayfanın sağ üst köşesinde **Başlat'ı** seçin, Facebook'un politikalarını kabul edin ve kayıt adımlarını tamamlayın.
1. **Uygulamalarımı** Seçin ve ardından **Uygulama Oluştur'u oluşturun.**
1. Bir **Görüntü Adı** ve geçerli bir **Kişi E-postası**girin.
1. **Uygulama Kimliği Oluştur'u**seçin. Bu, Facebook platform politikalarını kabul etmenizi ve çevrimiçi güvenlik denetimini tamamlamanızı gerektirebilir.
1. **Ayarlar** > **Temel'i**seçin.
1. Bir **Kategori**seçin `Business and Pages`, örneğin . Bu değer Facebook tarafından gereklidir, ancak Azure AD B2C için kullanılmaz.
1. Sayfanın alt kısmında **Platform Ekle'yi**seçin ve ardından **Web Sitesi'ni**seçin.
1. Site URL'sinde, `your-tenant-name` kiracınızın adıyla **Site URL** `https://your-tenant-name.b2clogin.com/` değiştirin. `http://www.contoso.com`Örneğin, Gizlilik **Politikası URL'si**için bir URL girin. İlke URL'si, uygulamanız için gizlilik bilgileri sağlamak için koruduğunuz bir sayfadır.
1. **Değişiklikleri Kaydet'i**seçin.
1. Sayfanın üst kısmında, **App ID**değerini kopyalayın.
1. **App Secret**değerini **göster'i** seçin ve kopyalayın. Her ikisini de Facebook'u kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için kullanırsınız. **App Secret** önemli bir güvenlik kimlik bilgisidir.
1. **ÜRÜNLERİn**yanındaki artı işaretini seçin ve **ardından Facebook Giriş'in**altında **Ayarla'yı** seçin.
1. **Facebook Giriş**altında , **Ayarlar**seçin.
1. **Geçerli OAuth yönlendirme URI,** girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Kiracınızın adıyla değiştirin. `your-tenant-name` Sayfanın altındaki **Değişiklikleri Kaydet'i** seçin.
1. Facebook uygulamanızı Azure AD B2C'de kullanılabilir hale getirmek için, sayfanın sağ üst kısmındaki Durum seçicisini seçin ve Uygulamayı herkese açık hale getirmek için **açın** ve ardından **Geçiş Modu'nu**seçin.  Bu noktada Durum **Geliştirme'den** **Canlı'ya**değişmeli.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Bir Facebook hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılarını**seçin, ardından **Facebook'u**seçin.
1. Bir **Ad**girin. Örneğin, *Facebook*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz Facebook uygulamasının Uygulama Kimliğini girin.
1. Müşteri **sırrı**için, kaydettiğiniz App Secret'ı girin.
1. **Kaydet'i**seçin.
