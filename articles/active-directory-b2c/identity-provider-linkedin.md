---
title: LinkedIn hesabıyla kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda LinkedIn hesabı olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d32eb80ffe296d86164a6d27a1b7a28181357243
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188109"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak LinkedIn hesabıyla kaydolma ve oturum açma

## <a name="create-a-linkedin-application"></a>LinkedIn uygulaması oluşturma

Bir LinkedIn hesabını Azure Active Directory B2C'de (Azure AD B2C) [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir LinkedIn hesabınız yoksa, 'ye [https://www.linkedin.com/](https://www.linkedin.com/)kaydolabilirsiniz.

1. LinkedIn hesap kimlik bilgilerinizle [LinkedIn Developers web sitesinde](https://www.developer.linkedin.com/) oturum açın.
1. **Uygulamalarım'ı**seçin ve ardından **Uygulama Oluştur'u**tıklatın.
1. **Şirket Adı,** **Uygulama Adı**, **Uygulama Tanımı**, Uygulama Logosu , **Uygulama** **Kullanımı**, Web **Sitesi URL,** **İş E-postave** **İş Telefonu**girin .
1. **LinkedIn API Kullanım Koşullarını** kabul edin ve **Gönder'i**tıklatın.
1. **İstemci Kimliği** ve **İstemci Sırrı**değerlerini kopyalayın. **Kimlik Doğrulama Tuşları**altında bulabilirsiniz. LinkedIn'i kiracınızda kimlik sağlayıcısı olarak yapılandırmak için her ikisine de ihtiyacınız olacaktır. **Client Secret** önemli bir güvenlik kimlik bilgisidir.
1. Yetkili `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` **Yönlendirme URL'lerini**girin. Kiracınızın adıyla değiştirin. `your-tenant-name` Kiracı Azure AD B2C'de büyük harflerle tanımlanmış olsa bile, kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir. **Ekle'yi**seçin ve ardından **Güncelleştir'i**tıklatın.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>LinkedIn hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılarını**seçin, ardından **LinkedIn'i**seçin.
1. Bir **Ad**girin. Örneğin, *LinkedIn*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz LinkedIn uygulamasının Müşteri Kimliğini girin.
1. **İstemci sırrı**için, kaydettiğiniz Müşteri Sırrı'nı girin.
1. **Kaydet'i**seçin.

## <a name="migration-from-v10-to-v20"></a>v1.0'dan v2.0'a geçiş

LinkedIn son zamanlarda [v1.0 v2.0 kendi API'ler güncelledi.](https://engineering.linkedin.com/blog/2018/12/developer-program-updates) Geçişin bir parçası olarak, Azure AD B2C yalnızca kayıt sırasında LinkedIn kullanıcısının tam adını alabilir. Bir e-posta adresi kaydolma sırasında toplanan özelliklerden biriyse, kullanıcının e-posta adresini el ile girmeli ve doğrulamalıdır.
