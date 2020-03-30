---
title: WeChat hesabıyla kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda WeChat hesapları olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c53210939358255b20d0e976df9c4bff88580a80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184446"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak bir WeChat hesabıyla kaydolma ve oturum açma

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-wechat-application"></a>WeChat uygulaması oluşturma

Bir WeChat hesabını Azure Active Directory B2C'de (Azure AD B2C) kimlik sağlayıcısı olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir WeChat hesabınız yoksa, 'den [https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html)bilgi alabilirsiniz.

### <a name="register-a-wechat-application"></a>WeChat uygulamasını kaydedin

1. WeChat [https://open.weixin.qq.com/](https://open.weixin.qq.com/) kimlik bilgilerinizle oturum açın.
1. Yönetim **管理中心** merkezini seçin.
1. Yeni bir uygulamayı kaydetmek için adımları izleyin.
1. (Geri `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` arama URL'si) **girin.** Örneğin, kiracı adınız contoso ise, URL'yi `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. APP **Kimliğini** ve **UYGULAMA ANAHTARINI**kopyalayın. Kimlik sağlayıcınızı kiracınıza eklemek için bunlara ihtiyacınız olacaktır.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat'i kiracınızda kimlik sağlayıcısı olarak yapılandırın

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılarını**seçin, ardından **WeChat (Önizleme) seçeneğini belirleyin.**
1. Bir **Ad**girin. Örneğin, *WeChat*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz WeChat uygulamasının APP Kimliğini girin.
1. **İstemci sırrı**için, kaydettiğiniz APP KEY'i girin.
1. **Kaydet'i**seçin.
