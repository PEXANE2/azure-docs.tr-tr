---
title: Microsoft Hesabı ile kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'yi kullanarak uygulamalarınızda Microsoft Hesapları olan müşterilere kaydolma ve kaydolma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 25784eb161a860398b0741d1d20375cabd1c4eca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188027"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak bir Microsoft hesabıyla kaydolma ve oturum açma

## <a name="create-a-microsoft-account-application"></a>Microsoft hesabı uygulaması oluşturma

Bir Microsoft hesabını Azure Active Directory B2C'de (Azure AD B2C) [kimlik sağlayıcısı](openid-connect.md) olarak kullanmak için Azure AD kiracısında bir uygulama oluşturmanız gerekir. Azure AD kiracı, Azure AD B2C kiracınızla aynı değildir. Zaten bir Microsoft hesabınız yoksa, 'den [https://www.live.com/](https://www.live.com/)bir tane alabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Yeni kayıt**seçin.
1. Başvurunuz için bir **Ad** girin. Örneğin, *MSAapp1*.
1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında (örneğin Skype, Xbox, Outlook.com) Hesapları**seçin. Bu seçenek, en geniş Microsoft kimlikleri kümesini hedefler.

   Farklı hesap türü seçimleri hakkında daha fazla bilgi için [Quickstart: Microsoft kimlik platformuna bir uygulama kaydedin.](../active-directory/develop/quickstart-register-app.md)
1. **Uri'yi Yönlendirme (isteğe bağlı)** altında **Web'i** seçin ve metin kutusuna girin. `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` Azure `your-tenant-name` AD B2C kiracı adınız ile değiştirin.
1. **Kaydol'u** Seçin
1. Uygulamaya Genel Bakış sayfasında gösterilen **Uygulama (istemci) kimliğini** kaydedin. Bir sonraki bölümde kimlik sağlayıcısını yapılandırırken buna ihtiyacınız var.
1. **Sertifikalar & sırları** seçin
1. **Yeni istemci sırrını** tıklatın
1. Gizli için **açıklama** girin, örneğin *Uygulama parolası 1*, ve sonra **Ekle'yi**tıklatın.
1. **Değer** sütununda gösterilen uygulama parolasını kaydedin. Bir sonraki bölümde kimlik sağlayıcısını yapılandırırken buna ihtiyacınız var.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak bir Microsoft hesabını yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılar**seçin, ardından **Microsoft Hesabı'nı**seçin.
1. Bir **Ad**girin. Örneğin, *MSA*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz Azure AD uygulamasının Uygulama (istemci) kimliğini girin.
1. **İstemci sırrı**için, kaydettiğiniz istemci sırrını girin.
1. **Kaydet'i**seçin.
