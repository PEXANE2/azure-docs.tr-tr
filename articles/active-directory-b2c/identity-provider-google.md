---
title: Bir Google hesabıyla kaydolma ve oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda Google hesabı olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 48955caddb64069f897078f5e47066d9f11d119b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188149"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak bir Google hesabıyla kaydolma ve oturum açma

## <a name="create-a-google-application"></a>Google uygulaması oluşturma

Azure Active Directory B2C'de (Azure AD B2C) bir Google hesabını [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için Google Geliştiriciler Konsolunuzda bir uygulama oluşturmanız gerekir. Zaten bir Google hesabınız yoksa [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)kaydolabilirsiniz.

1. Google hesap kimlik bilgilerinizle [Google Geliştiriciler Konsolu'nda](https://console.developers.google.com/) oturum açın.
1. Sayfanın sol üst köşesinde proje listesini seçin ve ardından **Yeni Proje'yi**seçin.
1. Proje **Adı**girin , **Oluştur'u**seçin.
1. Ekranın sol üst kısmında proje açılır seçeneğini seçerek yeni projeyi kullandığınızdan emin olun, projenizi ada göre seçin ve ardından **Aç'ı**seçin.
1. Sol menüde **OAuth onay ekranını** seçin, **Harici'yi**seçin ve sonra **Oluştur'u**seçin.
Başvurunuz için bir **Ad** girin. Yetkili etki **alanları** bölümüne *b2clogin.com* girin ve **Kaydet'i**seçin.
1. Sol menüde **Kimlik Bilgileri'ni** seçin ve ardından **Kimlik Bilgileri** > **Oluştur Oauth istemci kimliğini**seçin.
1. **Uygulama türü altında,** **Web uygulamasını**seçin.
1. Uygulamanız için bir **Ad** girin, Yetkili JavaScript `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` kökenli girin ve **Yetkili yeniden yönlendirme URI'lerine**girin. **Authorized JavaScript origins** `https://your-tenant-name.b2clogin.com` Kiracınızın adıyla değiştirin. `your-tenant-name` Kiracı Azure AD B2C'de büyük harflerle tanımlanmış olsa bile, kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
1. **Oluştur'u**tıklatın.
1. **İstemci kimliği** ve **Istemci sırrı**değerlerini kopyalayın. Google'ı kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için her ikisine de ihtiyacınız olacaktır. **Müşteri sırrı** önemli bir güvenlik kimlik bilgisidir.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Bir Google hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılarını**seçin, ardından **Google'ı**seçin.
1. Bir **Ad**girin. Örneğin, *Google*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz Google uygulamasının Müşteri Kimliğini girin.
1. **İstemci sırrı**için, kaydettiğiniz Müşteri Sırrı'nı girin.
1. **Kaydet'i**seçin.
