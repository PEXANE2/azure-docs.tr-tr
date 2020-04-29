---
title: Kayıt ayarlama ve bir GitHub hesabı ile oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda GitHub hesabı bulunan müşterilere kaydolma ve oturum açma sağlayın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78188214"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir GitHub hesabı ile kaydolma ve oturum açma ayarlama

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-github-oauth-application"></a>GitHub OAuth uygulaması oluşturma

Azure Active Directory B2C (Azure AD B2C) ' de bir GitHub hesabını bir [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir GitHub hesabınız yoksa kaydolabilirsiniz [https://www.github.com/](https://www.github.com/).

1. GitHub kimlik bilgilerinizle [GitHub geliştirici](https://github.com/settings/developers) Web sitesinde oturum açın.
1. **OAuth uygulamaları** ' nı seçin ve ardından **Yeni OAuth uygulaması**' nı seçin.
1. Bir **uygulama adı** ve **giriş sayfası URL 'si**girin.
1. `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` **Yetkilendirme geri çağırma URL 'sini**girin. Azure AD B2C `your-tenant-name` kiracınızın adıyla değiştirin. Kiracı, Azure AD B2C büyük harfle tanımlansa bile kiracı adınızı girerken tüm küçük harfleri kullanın.
1. **Uygulamayı kaydet**' e tıklayın.
1. **ISTEMCI kimliği** ve **istemci parolası**değerlerini kopyalayın. Kimlik sağlayıcısını kiracınıza eklemek için her ikisine de ihtiyacınız vardır.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Bir GitHub hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı ve ardından **GitHub (Önizleme)** öğesini seçin.
1. Bir **ad**girin. Örneğin, *GitHub*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz GitHub UYGULAMASıNıN istemci kimliğini girin.
1. **İstemci parolası**için, kaydettiğiniz istemci gizli anahtarını girin.
1. **Kaydet**’i seçin.
