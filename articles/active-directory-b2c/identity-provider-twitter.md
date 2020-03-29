---
title: Twitter hesabıyla kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda Twitter hesabı olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 62a283efb93987d3c4a6564c9b25d2031c269559
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051459"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak bir Twitter hesabıyla kaydolma ve oturum açma

## <a name="create-an-application"></a>Uygulama oluşturma

Twitter'ı Azure AD B2C'de kimlik sağlayıcısı olarak kullanmak için bir Twitter uygulaması oluşturmanız gerekir. Zaten bir Twitter hesabınız yoksa, 'ye [https://twitter.com/signup](https://twitter.com/signup)kaydolabilirsiniz.

1. Twitter hesap kimlik bilgilerinizle [Twitter Geliştiricileri](https://developer.twitter.com/en/apps) web sitesinde oturum açın.
1. **Uygulama Oluştur'u**seçin.
1. Bir **Uygulama adı** ve **Uygulama açıklaması**girin.
1. **Web Sitesi URL'sinde**, girin `https://your-tenant.b2clogin.com`. Kiracınızın adıyla değiştirin. `your-tenant` Örneğin, `https://contosob2c.b2clogin.com`.
1. Geri **Arama URL'si için**, girin. `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp` Kiracı `your-tenant` adınızı ve `your-user-flow-Id` kullanıcı akışınızın tanımlayıcısını değiştirin. Örneğin, `b2c_1A_signup_signin_twitter`. Azure AD B2C'de büyük harflerle tanımlanmış olsalar bile kiracı adınızı ve kullanıcı akış kimliğinizi girerken tüm küçük harfleri kullanmanız gerekir.
1. Sayfanın alt kısmında, koşulları okuyun ve kabul edin ve ardından **Oluştur'u**seçin.
1. Uygulama **ayrıntıları** sayfasında, **> Edit ayrıntılarını edit'i**seçin, **Twitter ile Oturum Aç'ı etkinleştir**için kutuyu işaretleyin ve ardından **Kaydet'i**seçin.
1. **Anahtarları ve belirteçleri** seçin ve daha sonra kullanılacak **Tüketici API Anahtarı** ve Tüketici **API gizli anahtar** değerlerini kaydedin.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Twitter'ı kiracınızda kimlik sağlayıcısı olarak yapılandırın

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılar**seçin, ardından **Twitter'ı**seçin.
1. Bir **Ad**girin. Örneğin, *Twitter*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz Twitter uygulamasının Tüketici API Anahtarını girin.
1. Müşteri **sırrı**için, kaydettiğiniz Tüketici API gizli anahtarını girin.
1. **Kaydet'i**seçin.
