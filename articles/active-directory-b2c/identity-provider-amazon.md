---
title: Bir Amazon hesabı ile kaydolma ve oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda Amazon hesaplarına sahip olan ve müşterilere kaydolma ve oturum açma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d1a771cb13fcfa76449500ad71c67dcf7c446fa4
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85388451"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Amazon hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-an-app-in-the-amazon-developer-console"></a>Amazon geliştirici konsolunda uygulama oluşturma

Azure Active Directory B2C (Azure AD B2C) ' de bir Amazon hesabını federal kimlik sağlayıcısı olarak kullanmak için, [Amazon Developer Services ve teknolojilerinde](https://developer.amazon.com)bir uygulama oluşturmanız gerekir. Zaten bir Amazon hesabınız yoksa kaydolabilirsiniz [https://www.amazon.com/](https://www.amazon.com/) .

> [!NOTE]  
> Aşağıdaki **8. adımdaki** URL 'leri kullanarak `your-tenant-name` kiracınızın adıyla değiştirin. Kiracı adınızı girerken, kiracı, Azure AD B2C büyük harfle tanımlansa bile, tüm küçük harfleri kullanın.
> - **Izin verilen çıkış noktaları**için şunu girin`https://your-tenant-name.b2clogin.com` 
> - **Izin verilen dönüş URL 'leri**için şunu girin`https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`

[!INCLUDE [identity-provider-amazon-idp-register.md](../../includes/identity-provider-amazon-idp-register.md)]

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Bir Amazon hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Amazon**' ı seçin.
1. Bir **ad**girin. Örneğin, *Amazon*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz Amazon UYGULAMASıNıN istemci kimliğini girin.
1. **İstemci parolası**için, kaydettiğiniz istemci gizli anahtarını girin.
1. **Kaydet**’i seçin.
