---
title: Amazon hesabıyla kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda Amazon hesabı olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d4538c1d15aeae624f5d73e9985448bda2fd8f1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188469"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak bir Amazon hesabıyla kaydolma ve oturum açma

## <a name="create-an-amazon-application"></a>Bir Amazon uygulaması oluşturma

Bir Amazon hesabını Azure Active Directory B2C'de (Azure AD B2C) [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir Amazon hesabınız yoksa [https://www.amazon.com/](https://www.amazon.com/)kaydolabilirsiniz.

1. Amazon hesap kimlik bilgilerinizle [Amazon Geliştirici Merkezi'nde](https://login.amazon.com/) oturum açın.
1. Bunu daha önce yapmadıysanız **Kaydol'u**tıklatın, geliştirici kayıt adımlarını izleyin ve ilkeyi kabul edin.
1. **Yeni uygulamayı kaydedin.**
1. **Ad,** **Açıklama**ve **Gizlilik Bildirimi**URL'si girin ve ardından **Kaydet'i**tıklatın. Gizlilik bildirimi, kullanıcılara gizlilik bilgileri sağlayan yönettiğiniz bir sayfadır.
1. Web **Ayarları** bölümünde, **İstemci Kimliği**değerlerini kopyalayın. İstemciyi gizli hale getirmek ve sonra kopyalamak için **Gizli Göster'i** seçin. Kiracınızda bir kimlik sağlayıcısı olarak bir Amazon hesabını yapılandırmak için her ikisinin de olması gerekir. **Client Secret** önemli bir güvenlik kimlik bilgisidir.
1. Web **Ayarları** bölümünde, **Edit'i**seçin `https://your-tenant-name.b2clogin.com` ve ardından İzin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` **Verilen JavaScript Origins'e** ve İzin Verilen İade **URL'lerine**girin. Kiracınızın adıyla değiştirin. `your-tenant-name` Kiracı Azure AD B2C'de büyük harflerle tanımlanmış olsa bile, kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
1. **Kaydet**'e tıklayın.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Bir Amazon hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılar**seçin, sonra **Amazon**seçin.
1. Bir **Ad**girin. Örneğin, *Amazon*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz Amazon uygulamasının Müşteri Kimliğini girin.
1. **İstemci sırrı**için, kaydettiğiniz Müşteri Sırrı'nı girin.
1. **Kaydet'i**seçin.
