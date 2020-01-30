---
title: Bir WeChat hesabı ile kaydolma ve oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda WeChat hesabı bulunan müşterilere kaydolma ve oturum açma sağlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7cbc41247d7b99eb63730ae4326808e64c663e8f
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76849532"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir WeChat hesabı ile kaydolma ve oturum açma ayarlama

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-wechat-application"></a>WeChat uygulaması oluşturma

Azure Active Directory B2C (Azure AD B2C) içinde bir kimlik sağlayıcısı olarak WeChat hesabı kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir WeChat hesabınız yoksa [https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html)bilgi edinebilirsiniz.

### <a name="register-a-wechat-application"></a>WeChat uygulamasını kaydetme

1. [https://open.weixin.qq.com/](https://open.weixin.qq.com/) Için, Wechat kimlik bilgilerinizle oturum açın.
1. **管理中心**(Yönetim Merkezi) öğesini seçin.
1. Yeni bir uygulamayı kaydetmek için adımları izleyin.
1. **授权回调域**(GERI arama URL) içinde `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` girin. Örneğin, kiracı adınız contoso ise, URL 'YI `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`olarak ayarlayın.
1. **Uygulama kimliği** ve **uygulama anahtarı**' nı kopyalayın. Kimlik sağlayıcısını kiracınıza eklemek için bunlara ihtiyacınız olacaktır.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat 'i kiracınızda kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Wechat (Önizleme)** öğesini seçin.
1. Bir **ad**girin. Örneğin, *Wechat*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz Wechat UYGULAMASıNıN uygulama kimliğini girin.
1. **İstemci parolası**için, KAYDETTIĞINIZ uygulama anahtarını girin.
1. **Kaydet**’i seçin.
