---
title: Weibo hesabıyla kaydolma ve kaydolma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda Weibo hesabı olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e99b5717cdcc32d8fd138d1edf5fe1bd3283c70e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187908"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak Weibo hesabıyla kaydolma ve kaydolma ayarlama

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-weibo-application"></a>Weibo uygulaması oluşturma

Bir Weibo hesabını Azure Active Directory B2C'de (Azure AD B2C) kimlik sağlayıcısı olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir Weibo hesabınız yoksa, 'ye [https://weibo.com/signup/signup.php?lang=en-us](https://weibo.com/signup/signup.php?lang=en-us)kaydolabilirsiniz.

1. Weibo hesap kimlik bilgilerinizle [Birlikte Weibo geliştirici portalında](https://open.weibo.com/) oturum açın.
1. Oturum açmadan sonra sağ üst köşede görüntü adınızı seçin.
1. Açılan açılır durumda, geliştirici bilgilerini **(geliştirici** bilgilerini düzenleme) seçeneğini belirleyin.
1. Gerekli bilgileri girin **提交** ve (gönder) seçin.
1. E-posta doğrulama işlemini tamamlayın.
1. [Kimlik doğrulama sayfasına](https://open.weibo.com/developers/identity/edit)gidin.
1. Gerekli bilgileri girin **提交** ve (gönder) seçin.

### <a name="register-a-weibo-application"></a>Weibo uygulamasını kaydedin

1. [Yeni Weibo uygulama kayıt sayfasına](https://open.weibo.com/apps/new)gidin.
1. Gerekli uygulama bilgilerini girin.
1. Seçin **(oluştur).**
1. **App Key** ve **App Secret**değerlerini kopyalayın. Kimlik sağlayıcınızı kiracınıza eklemek için bunların her ikisine de ihtiyacınız var.
1. Gerekli fotoğrafları yükleyin ve gerekli bilgileri girin.
1. Seçin **iz** (kaydet).
1. **(Gelişmiş** bilgi) seçin.
1. OAuth2.0 **授权设置** için alanın yanındaki **编辑** (url'yi yeniden yönlendir) seçin.
1. OAuth2.0 **授权设置** (URL yönlendirme) için girin. `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` Örneğin, kiracı adınız contoso ise, URL'yi `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. Seçin **提交** (gönder).

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Weibo hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılarını**seçin, ardından **Weibo'yu seçin (Önizleme)**.
1. Bir **Ad**girin. Örneğin, *Weibo.*
1. **İstemci Kimliği**için, daha önce oluşturduğunuz Weibo uygulamasının Uygulama Anahtarı'nı girin.
1. Müşteri **sırrı**için, kaydettiğiniz App Secret'ı girin.
1. **Kaydet'i**seçin.
