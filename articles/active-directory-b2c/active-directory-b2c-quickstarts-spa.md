---
title: Quickstart - Set up sign-in for a single-page app using Azure Active Directory B2C
description: In this Quickstart, run a sample single-page application that uses Azure Active Directory B2C to provide account sign-in.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 09/12/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7dc3a52ca9f227f8476cf74286be917b5b6ba021
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74420172"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Hızlı başlangıç: Azure Active Directory B2C'yi kullanarak tek sayfalı bir uygulama için oturum açma bilgilerini ayarlama

Azure Active Directory B2C (Azure AD B2C) provides cloud identity management to keep your application, business, and customers protected. Azure AD B2C; uygulamalarınızın, açık standart protokolleri kullanarak sosyal hesaplarda ve kurumsal hesaplarda kimlik doğrulaması gerçekleştirmesine olanak tanır. Bu hızlı başlangıçta, sosyal kimlik sağlayıcısı kullanıp Azure AD B2C korumalı web API’si çağırarak oturum açmak için bir tek sayfalı uygulama kullanırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload
- [Node.js](https://nodejs.org/en/download/)
- Social account from Facebook, Google, or Microsoft
- Code sample from GitHub: [active-directory-b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)
    
    You can [download the zip archive](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) or clone the repository:

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

1. Node.js komut isteminde şu komutları çalıştırarak sunucuyu başlatın:

    ```
    cd active-directory-b2c-javascript-msal-singlepageapp
    npm install && npm update
    node server.js
    ```

    Server.js, localhost’ta dinlediği bağlantı noktası numarasını çıkarır.

    ```
    Listening on port 6420...
    ```

2. Uygulamanın URL'sine göz atın. Örneğin, `http://localhost:6420`.

## <a name="sign-in-using-your-account"></a>Hesabınızı kullanarak oturum açın

1. İş akışını başlatmak için **Oturum aç**'a tıklayın.

    ![Single-page application sample app shown in browser](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

    Örnek, sosyal kimlik sağlayıcısı kullanma veya bir e-posta adresiyle yerel bir hesap oluşturma da dahil olmak üzere çeşitli kaydolma seçeneklerini destekler. For this quickstart, use a social identity provider account from either Facebook, Google, or Microsoft.

2. Azure AD B2C presents a sign-in page for a fictitious company called Fabrikam for the sample web application. Sosyal kimlik sağlayıcısı kullanarak kaydolmak için, kullanmak istediğiniz kimlik sağlayıcısının düğmesine tıklayın.

    ![Sign In or Sign Up page showing identity provider buttons](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

    You authenticate (sign in) using your social account credentials and authorize the application to read information from your social account. Erişim izni verdiğinizde uygulama sosyal hesabınızdan adınız ve şehriniz gibi profil bilgilerini alabilir.

3. Kimlik sağlayıcısına ilişkin oturum açma işlemini tamamlayın.

## <a name="access-a-protected-api-resource"></a>Korumalı bir API kaynağına erişme

Görünen adınızın Web API’si çağrısından JSON nesnesi olarak döndürülmesi için **Web API’si Çağrısı**'na tıklayın.

![Sample application in browser showing the web API response](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

Örnek tek sayfalı uygulama, korumalı web API’si kaynağına yönelik isteğe bir erişim belirteci ekler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C hızlı başlangıçlarını veya öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you used a sample single-page application to:

* Sign in with a custom login page
* Sign in with a social identity provider
* Create an Azure AD B2C account
* Call a web API protected by Azure AD B2C

Kendi Azure AD B2C kiracınızı oluşturarak kullanmaya başlayın.

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)
