---
title: Microsoft hesabı ile kaydolma ve oturum açma ayarlama-Azure Active Directory B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda Microsoft hesaplarıyla oturum açma ve oturum açma sağlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 14aff313ece93feabc39194ba6859e3bca6be3bc
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69622393"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Microsoft hesabı Azure Active Directory B2C kullanarak kaydolma ve oturum açma ayarlama

## <a name="create-a-microsoft-account-application"></a>Microsoft hesabı uygulaması oluşturma

Bir Microsoft hesabı Azure Active Directory (Azure AD) B2C 'de [kimlik sağlayıcısı](active-directory-b2c-reference-oidc.md) olarak kullanmak IÇIN Azure AD kiracısında bir uygulama oluşturmanız gerekir. Azure AD kiracısı Azure AD B2C kiracınızla aynı değildir. Zaten bir Microsoft hesabı yoksa bir tane [https://www.live.com/](https://www.live.com/)edinebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini SEÇIP Azure AD kiracınızı içeren dizini seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **uygulama kayıtları**' i arayıp seçin.
1. **Yeni kayıt**seçeneğini belirleyin.
1. Uygulamanız için bir **ad** girin. Örneğin, *MSAapp1*.
1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında (ör. Skype, Xbox, Outlook.com) hesaplar**' ı seçin. Bu seçenek, en geniş Microsoft kimlikleri kümesini hedefler.

   Farklı hesap türü seçimleri hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Bir uygulamayı Microsoft Identity platformu](../active-directory/develop/quickstart-register-app.md)ile kaydedin.
1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** altında **Web** ' `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` i seçin ve metin kutusuna girin. Azure AD B2C `your-tenant-name` kiracı adınızla değiştirin.
1. **Kaydol** ' u seçin
1. Uygulamaya Genel Bakış sayfasında gösterilen **uygulama (istemci) kimliğini** kaydedin. Bu, sonraki bölümde kimlik sağlayıcısını yapılandırırken gerekir.
1. **Sertifikaları & parolaları** seçin
1. **Yeni istemci parolası** ' na tıklayın
1. Gizli dizi için bir **Açıklama** girin, örneğin, *uygulama parolası 1*ve ardından **Ekle**' ye tıklayın.
1. **Değer** sütununda gösterilen uygulama parolasını kaydedin. Bu, sonraki bölümde kimlik sağlayıcısını yapılandırırken gerekir.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak Microsoft hesabı yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Microsoft hesabı**' nı seçin.
1. Bir **ad**girin. Örneğin, *MSA*.
1. **ISTEMCI kimliği**için, daha önce oluşturduğunuz Azure AD uygulamasının uygulama (ISTEMCI) kimliğini girin.
1. **İstemci parolası**için, kaydettiğiniz istemci gizli anahtarını girin.
1. **Kaydet**’i seçin.
