---
title: Bir Amazon hesabı ile kaydolma ve oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda Amazon hesaplarına sahip olan ve müşterilere kaydolma ve oturum açma sağlayın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78188469"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Amazon hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-an-amazon-application"></a>Amazon uygulaması oluşturma

Azure Active Directory B2C (Azure AD B2C) ' de bir Amazon hesabını bir [kimlik sağlayıcısı](authorization-code-flow.md) olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Henüz bir Amazon hesabınız yoksa kaydolabilirsiniz [https://www.amazon.com/](https://www.amazon.com/).

1. Amazon hesabı kimlik bilgilerinizle [Amazon Geliştirici Merkezi](https://login.amazon.com/) ' nde oturum açın.
1. Daha önce yapmadıysanız, **Kaydol**' a tıklayın, geliştirici kayıt adımlarını izleyin ve ilkeyi kabul edin.
1. **Yeni uygulama kaydet**' i seçin.
1. Bir **ad**, **Açıklama**ve **Gizlilik bildirimi URL 'si**girin ve ardından **Kaydet**' e tıklayın. Gizlilik bildirimi, kullanıcılara gizlilik bilgileri sağlayan bir sayfasıdır.
1. **Web ayarları** bölümünde, **istemci kimliği**değerlerini kopyalayın. İstemci parolasını almak için **gizli göster** ' i seçin ve sonra kopyalayın. Bir Amazon hesabını kiracınızda kimlik sağlayıcısı olarak yapılandırmak için her ikisine de ihtiyacınız vardır. **Istemci parolası** önemli bir güvenlik kimlik bilgileridir.
1. **Web ayarları** bölümünde **Düzenle**' yi seçin ve izin verilen **JavaScript kaynakları** ' na ve `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` **izin verilen dönüş URL 'lerine**girin. `https://your-tenant-name.b2clogin.com` Kiracınızın adıyla değiştirin `your-tenant-name` . Kiracı, Azure AD B2C büyük harfle tanımlansa bile kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
1. **Kaydet**’e tıklayın.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Bir Amazon hesabını kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Amazon**' ı seçin.
1. Bir **ad**girin. Örneğin, *Amazon*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz Amazon UYGULAMASıNıN istemci kimliğini girin.
1. **İstemci parolası**için, kaydettiğiniz istemci gizli anahtarını girin.
1. **Kaydet**’i seçin.
