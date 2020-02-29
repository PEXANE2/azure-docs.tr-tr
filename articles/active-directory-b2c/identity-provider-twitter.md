---
title: Twitter hesabı ile kaydolma ve oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda Twitter hesapları bulunan müşterilere kaydolma ve oturum açma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: bd538529756270053351e4c2d85f5761b198eeff
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78187891"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Twitter hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-an-application"></a>Uygulama oluşturma

Twitter 'ı Azure AD B2C bir kimlik sağlayıcısı olarak kullanmak için, bir Twitter uygulaması oluşturmanız gerekir. Zaten bir Twitter hesabınız yoksa, [https://twitter.com/signup](https://twitter.com/signup)' de kaydolabilirsiniz.

1. Twitter hesabı kimlik bilgilerinizle [Twitter geliştiricileri](https://developer.twitter.com/en/apps) Web sitesinde oturum açın.
1. **Uygulama oluştur**' u seçin.
1. Uygulama **adı** ve **uygulama açıklaması**girin.
1. **Web sitesi URL 'si**' nde `https://your-tenant.b2clogin.com`girin. `your-tenant`, kiracınızın adıyla değiştirin. Örneğin, https://contosob2c.b2clogin.com.
1. **Geri arama URL 'si**için `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp`girin. `your-tenant`, kiracı adınızın adı ve `your-user-flow-Id` Kullanıcı akışınız tanımlayıcısıyla değiştirin. Örneğin, `b2c_1A_signup_signin_twitter`. Azure AD B2C büyük harfle tanımlansalar bile kiracı adınızı ve Kullanıcı akış kimliğinizi girerken tüm küçük harfleri kullanmanız gerekir.
1. Sayfanın alt kısmındaki koşulları okuyup kabul edin ve **Oluştur**' u seçin.
1. **Uygulama ayrıntıları** sayfasında **Düzenle > Ayrıntıları Düzenle**' yi seçin, **Twitter ile oturum açmayı etkinleştir**onay kutusunu işaretleyin ve ardından **Kaydet**' i seçin.
1. **Anahtarlar ve belirteçler** seçin, daha sonra kullanılacak **Tüketici API anahtarını** ve **Tüketici API 'sinin gizli anahtar** değerlerini kaydedin.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Twitter 'ı kiracınızda kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı ve ardından **Twitter**' ı seçin.
1. Bir **ad**girin. Örneğin, *Twitter*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz Twitter UYGULAMASıNıN tüketici API anahtarını girin.
1. **İstemci parolası**için, kaydettiğiniz tüketici API 'si gizli anahtarını girin.
1. **Kaydet**’i seçin.
