---
title: Azure Active Directory B2C kullanarak bir QQ hesabı ile kaydolma ve oturum açma ayarlama Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda QQ hesaplarına kaydolma ve oturum açma özelliği sağlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9679e3216184c0dc5b8dee241c30fc69d5423aeb
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68963752"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir QQ hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-qq-application"></a>QQ uygulaması oluşturma

Bir QQ hesabını Azure Active Directory (Azure AD) B2C 'de bir kimlik sağlayıcısı olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir QQ hesabınız yoksa, bu [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033)hesaba ulaşabilirsiniz.

### <a name="register-for-the-qq-developer-program"></a>QQ geliştirici programına kaydolma

1. QQ hesabı kimlik bilgilerinizle [QQ Geliştirici Portalında](http://open.qq.com) oturum açın.
2. Oturum açtıktan sonra, kendinizi bir [https://open.qq.com/reg](https://open.qq.com/reg) geliştirici olarak kaydetmek için bölümüne gidin.
3. **个人**(bireysel Geliştirici) seçeneğini belirleyin.
4. Gerekli bilgileri girin ve**下一步**(sonraki adım) seçeneğini belirleyin.
5. E-posta doğrulama sürecini doldurun. Bir geliştirici olarak kaydedildikten sonra birkaç günün onaylanmasını beklemeniz gerekecektir. 

### <a name="register-a-qq-application"></a>Bir QQ uygulamasını kaydetme

1. [https://connect.qq.com/index.html](https://connect.qq.com/index.html) kısmına gidin.
2. **应用管理**(uygulama yönetimi) öğesini seçin.
5. **创建应用**(uygulama oluştur) seçeneğini belirleyin ve gerekli bilgileri girin.
7. 授权回调域`https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp` ( geri arama URL) girin. Örneğin, `tenant_name` contoso ise, URL 'yi `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`olarak ayarlayın.
8. **创建应用**(uygulama oluştur) seçeneğini belirleyin.
9. Onay sayfasında, uygulama yönetimi sayfasına dönmek için**应用管理**(uygulama yönetimi) öğesini seçin.
10. Oluşturduğunuz uygulamanın yanındaki**查看**(görünüm) seçeneğini belirleyin.
11. **修改**(Düzenle) öğesini seçin.
12. **Uygulama kimliği** ve **uygulama anahtarı**' nı kopyalayın. Kimlik sağlayıcısını kiracınıza eklemek için bu değerlerin her ikisine de ihtiyacınız vardır.

## <a name="configure-qq-as-an-identity-provider"></a>QQ 'ı kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *QQ*girin.
6. **Kimlik sağlayıcısı türünü**seçin, **QQ (Önizleme)** öğesini seçin ve **Tamam**' a tıklayın.
7. **Bu kimlik sağlayıcısını ayarla** ' yı seçin ve daha önce **istemci KIMLIĞI** olarak kaydettiğiniz uygulama kimliğini girin ve daha önce oluşturduğunuz QQ uygulamasının **ISTEMCI gizli** anahtarı olarak kaydettiğiniz uygulama anahtarını girin.
8. **Tamam** ' a tıklayın ve sonra QQ yapılandırmanızı kaydetmek için **Oluştur** ' a tıklayın.

