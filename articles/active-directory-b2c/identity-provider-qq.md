---
title: Azure Active Directory B2C kullanarak bir QQ hesabı ile kaydolma ve oturum açma ayarlama
description: Azure Active Directory B2C kullanarak uygulamalarınızda QQ hesaplarına kaydolma ve oturum açma özelliği sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d230bc8a1e9bf388e1cca4e3a3a691223146d734
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85387992"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir QQ hesabı ile kaydolma ve oturum açma ayarlama

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-qq-application"></a>QQ uygulaması oluşturma

Bir QQ hesabını Azure Active Directory B2C (Azure AD B2C) ' de bir kimlik sağlayıcısı olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir QQ hesabınız yoksa, ' de kaydolabilirsiniz [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033) .

### <a name="register-for-the-qq-developer-program"></a>QQ geliştirici programına kaydolma

1. QQ hesabı kimlik bilgilerinizle [QQ Geliştirici Portalında](http://open.qq.com) oturum açın.
1. Oturum açtıktan sonra, [https://open.qq.com/reg](https://open.qq.com/reg) kendinizi bir geliştirici olarak kaydetmek için bölümüne gidin.
1. **个人**(bireysel Geliştirici) seçeneğini belirleyin.
1. Gerekli bilgileri girin ve**下一步**(sonraki adım) seçeneğini belirleyin.
1. E-posta doğrulama sürecini doldurun. Bir geliştirici olarak kaydedildikten sonra birkaç günün onaylanmasını beklemeniz gerekecektir.

### <a name="register-a-qq-application"></a>Bir QQ uygulamasını kaydetme

1. Adresine gidin [https://connect.qq.com/index.html](https://connect.qq.com/index.html) .
1. **应用管理**(uygulama yönetimi) öğesini seçin.
1. **创建应用**(uygulama oluştur) seçeneğini belirleyin ve gerekli bilgileri girin.
1. `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp`**授权回调域**(geri arama URL) girin. Örneğin, `tenant_name` contoso ise, URL 'yi olarak ayarlayın `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp` .
1. **创建应用**(uygulama oluştur) seçeneğini belirleyin.
1. Onay sayfasında, uygulama yönetimi sayfasına dönmek için**应用管理**(uygulama yönetimi) öğesini seçin.
1. Oluşturduğunuz uygulamanın yanındaki**查看**(görünüm) seçeneğini belirleyin.
1. **修改**(Düzenle) öğesini seçin.
1. **Uygulama kimliği** ve **uygulama anahtarı**' nı kopyalayın. Kimlik sağlayıcısını kiracınıza eklemek için bu değerlerin her ikisine de ihtiyacınız vardır.

## <a name="configure-qq-as-an-identity-provider"></a>QQ 'ı kimlik sağlayıcısı olarak yapılandırma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve **QQ (Önizleme)** öğesini seçin.
1. Bir **ad**girin. Örneğin, *QQ*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz QQ UYGULAMASıNıN uygulama kimliğini girin.
1. **İstemci parolası**için, KAYDETTIĞINIZ uygulama anahtarını girin.
1. **Kaydet**’i seçin.
