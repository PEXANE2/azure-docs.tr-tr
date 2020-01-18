---
title: Bir Google hesabı ile kaydolma ve oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda Google hesabı bulunan müşterilere kaydolma ve oturum açma sağlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 147de090411309a442ad07711ce62ec7fd64b3fa
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76261220"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Google hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-google-application"></a>Google uygulaması oluşturma

Bir Google hesabını Azure Active Directory B2C (Azure AD B2C) ' de bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) olarak kullanmak Için Google geliştiricileri konsolunuza bir uygulama oluşturmanız gerekir. Zaten bir Google hesabınız yoksa [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)için kaydolabilirsiniz.

1. Google [Developers konsolunda](https://console.developers.google.com/) Google hesabı kimlik bilgilerinizle oturum açın.
1. Sayfanın sol üst köşesinde proje listesini seçin ve ardından **Yeni proje**' yi seçin.
1. Bir **Proje adı**girin, **Oluştur**' u seçin.
1. Ekranın sol üst köşesindeki proje açılır listesini seçerek yeni projeyi kullandığınızdan emin olun, projenize ada göre seçip **Aç**' ı seçin.
1. Sol taraftaki menüden **OAuth onay ekranını** seçin, **dış**' i seçin ve ardından **Oluştur**' u seçin.
Uygulamanız için bir **ad** girin. **Yetkili etki alanları** bölümüne *B2clogin.com* girin ve **Kaydet**' i seçin.
1. Sol menüden **kimlik** bilgileri ' ni seçin ve ardından **Oauth istemci kimliği** > **kimlik bilgileri oluştur** ' u seçin.
1. **Uygulama türü**altında **Web uygulaması**' nı seçin.
1. Uygulamanız için bir **ad** girin, **yetkili JavaScript kaynakları**' na `https://your-tenant-name.b2clogin.com` girin ve **yetkili yeniden yönlendirme URI 'lerinde**`https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. `your-tenant-name`, kiracınızın adıyla değiştirin. Kiracı, Azure AD B2C büyük harfle tanımlansa bile kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
1. **Oluştur**’a tıklayın.
1. **ISTEMCI kimliği** ve **istemci parolası**değerlerini kopyalayın. Google 'ı kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için her ikisine de ihtiyacınız olacak. **İstemci parolası** önemli bir güvenlik kimlik bilgileridir.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Bir Google hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Google**' ı seçin.
1. Bir **ad**girin. Örneğin, *Google*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz Google UYGULAMASıNıN istemci kimliğini girin.
1. **İstemci parolası**için, kaydettiğiniz istemci gizli anahtarını girin.
1. **Kaydet**’i seçin.
