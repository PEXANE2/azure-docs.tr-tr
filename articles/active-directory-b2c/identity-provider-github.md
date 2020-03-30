---
title: GitHub hesabıyla kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda GitHub hesapları olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a7435abf2740800184a6de1aad07bca53cd56cf8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188214"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak GitHub hesabıyla kaydolma ve oturum açma

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-github-oauth-application"></a>GitHub OAuth uygulaması oluşturma

Bir GitHub hesabını Azure Active Directory B2C'de (Azure AD B2C) [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir GitHub hesabınız yoksa, 'de [https://www.github.com/](https://www.github.com/)kaydolabilirsiniz.

1. GitHub kimlik bilgilerinizle [GitHub Geliştirici](https://github.com/settings/developers) web sitesinde oturum açın.
1. **OAuth Apps'ı** seçin ve ardından **Yeni OAuth Uygulamasını**seçin.
1. Bir **Uygulama adı** ve **Ana Sayfa URL'nizi**girin.
1. Yetkilendirme `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` **geri arama**URL'si girin. Azure `your-tenant-name` AD B2C kiracınızın adıyla değiştirin. Kiracı Azure AD B2C'de büyük harflerle tanımlanmış olsa bile, kiracı adınızı girerken tüm küçük harfleri kullanın.
1. **Uygulamayı Kaydol'u**tıklatın.
1. **İstemci Kimliği** ve **İstemci Sırrı**değerlerini kopyalayın. Kimlik sağlayıcınızı kiracınıza eklemek için her ikisinin de gerekir.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Bir GitHub hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılarını**seçin, ardından **GitHub 'ı (Önizleme)** seçin.
1. Bir **Ad**girin. Örneğin, *GitHub*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz GitHub uygulamasının Müşteri Kimliğini girin.
1. **İstemci sırrı**için, kaydettiğiniz Müşteri Sırrı'nı girin.
1. **Kaydet'i**seçin.
