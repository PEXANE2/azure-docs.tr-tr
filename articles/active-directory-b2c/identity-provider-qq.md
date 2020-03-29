---
title: Azure Active Directory B2C'yi kullanarak QQ hesabıyla kaydolma ve oturum açma
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda QQ hesabı olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b0f15124c64e5cca54112987d486ddadaca79452
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187996"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak QQ hesabıyla kaydolma ve oturum açma

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-qq-application"></a>QQ uygulaması oluşturma

Bir QQ hesabını Azure Active Directory B2C'de (Azure AD B2C) kimlik sağlayıcısı olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir QQ hesabınız yoksa, 'de [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033)kaydolabilirsiniz.

### <a name="register-for-the-qq-developer-program"></a>QQ geliştirici programına kaydolun

1. QQ hesap kimlik bilgilerinizle [QQ geliştirici portalında](http://open.qq.com) oturum açın.
1. Oturum imzaladıktan sonra, kendinizi geliştirici olarak kaydettirmeye [https://open.qq.com/reg](https://open.qq.com/reg) gidin.
1. **(Tek** tek geliştirici) seçin.
1. Gerekli bilgileri girin ve **(sonraki** adım) seçin.
1. E-posta doğrulama işlemini tamamlayın. Geliştirici olarak kaydolduktan sonra onaylanmak için birkaç gün beklemeniz gerekir.

### <a name="register-a-qq-application"></a>QQ uygulamasını kaydetme

1. [https://connect.qq.com/index.html](https://connect.qq.com/index.html)Git.
1. **(Uygulama** yönetimi) seçeneğini belirleyin.
1. **(Uygulama** oluştur) seçin ve gerekli bilgileri girin.
1. (Geri `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp` arama URL'si) **girin.** Örneğin, contoso `tenant_name` ise, URL'yi `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. Seçin **(uygulama** oluşturun).
1. Onay sayfasında, uygulama yönetimi sayfasına dönmek için uygulama yönetimini **seçin.**
1. Oluşturduğunuz uygulamanın yanında **(görünüm)** seçin.
1. (Edit) seçin. **修改**
1. APP **Kimliğini** ve **UYGULAMA ANAHTARINI**kopyalayın. Kimlik sağlayıcınızı kiracınıza eklemek için bu değerlerin her ikisine de ihtiyacınız var.

## <a name="configure-qq-as-an-identity-provider"></a>QQ'yi kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
1. **Kimlik sağlayıcılarını**seçin, ardından **QQ (Önizleme) seçeneğini belirleyin.**
1. Bir **Ad**girin. Örneğin, *QQ*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz QQ uygulamasının APP Kimliğini girin.
1. **İstemci sırrı**için, kaydettiğiniz APP KEY'i girin.
1. **Kaydet'i**seçin.
