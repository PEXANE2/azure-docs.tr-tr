---
title: LinkedIn hesabı ile kaydolma ve oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda LinkedIn hesapları olan kaydolma ve oturum açma bilgilerini sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/22/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10c460775bcb63028f03d0e8d0b1f7ed1507cdb4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91259467"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir LinkedIn hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-linkedin-application"></a>LinkedIn uygulaması oluşturma

Azure Active Directory B2C (Azure AD B2C) ' de bir LinkedIn hesabını [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir LinkedIn hesabınız yoksa, ' de kaydolabilirsiniz [https://www.linkedin.com/](https://www.linkedin.com/) .

1. LinkedIn hesabı kimlik bilgilerinizle [LinkedIn geliştiricileri Web sitesinde](https://www.developer.linkedin.com/) oturum açın.
1. **Uygulamalarım**' ı seçin ve ardından **uygulama oluştur**' a tıklayın.
1. **Uygulama adı**, **LinkedIn sayfası**, **Gizlilik ilkesi URL 'si**ve **uygulama logosu**girin.
1. LinkedIn **API kullanım koşullarını** kabul edin ve **uygulama oluştur**' a tıklayın.
1. **AUTH** sekmesini seçin. **Kimlik doğrulama anahtarları**' nın altında, **Istemci kimliği** ve **istemci parolası**değerlerini kopyalayın. Bunları kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için her ikisine de ihtiyacınız olacak. **Istemci parolası** önemli bir güvenlik kimlik bilgileridir.
1. **Uygulamanızın yetkili yeniden yönlendirme URL 'lerinin**yanındaki Düzenle kalem ' i seçin ve ardından **yeniden yönlendirme URL 'si Ekle**' yi seçin. `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` `your-tenant-name` Kiracınızın adıyla değiştirin. Kiracı, Azure AD B2C büyük harfle tanımlansa bile kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir. **Güncelleştir**’i seçin.
2. Varsayılan olarak, LinkedIn uygulamanız oturum açma ile ilgili kapsamlar için onaylanmamış değildir. Bir gözden geçirme istemek için, **Ürünler** sekmesini seçin ve ardından **LinkedIn ile oturum aç**' ı seçin. İnceleme tamamlandığında, uygulamanıza gereken kapsamlar eklenir.
   > [!NOTE]
   > Uygulamanız için şu anda izin verilen kapsamları, **OAuth 2,0 kapsamları** bölümündeki **AUTH** sekmesinde görebilirsiniz.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Bir LinkedIn hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve **LinkedIn**' i seçin.
1. Bir **ad**girin. Örneğin, *LinkedIn*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz LinkedIn UYGULAMASıNıN istemci kimliğini girin.
1. **İstemci parolası**için, kaydettiğiniz istemci gizli anahtarını girin.
1. **Kaydet**’i seçin.

## <a name="migration-from-v10-to-v20"></a>V 1.0 'dan v 2.0'a geçiş

LinkedIn [, API 'lerini son zamanlarda v 1.0 'dan v 2.0 'a güncelleştirmiştir](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Geçişin bir parçası olarak, Azure AD B2C yalnızca kaydolma sırasında LinkedIn kullanıcısının tam adını elde edebilir. Kayıt sırasında toplanan özniteliklerden biri e-posta adresi ise, kullanıcının e-posta adresini el ile girmesi ve doğrulaması gerekir.
