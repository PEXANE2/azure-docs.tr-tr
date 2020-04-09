---
title: 'Quickstart: Tek sayfalık bir uygulama (SPA) için oturum açma'
titleSuffix: Azure AD B2C
description: Bu Quickstart'ta, hesap oturum açma sağlamak için Azure Active Directory B2C kullanan tek sayfalık bir örnek uygulama çalıştırın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 04/04/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0a754873aeafe8d4e7b48d49647469874ff40f7e
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875904"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Hızlı başlangıç: Azure Active Directory B2C'yi kullanarak tek sayfalı bir uygulama için oturum açma bilgilerini ayarlama

Azure Active Directory B2C (Azure AD B2C), uygulamanızı, işletmenizi ve müşterilerinizi korumak için bulut kimlik yönetimi sağlar. Azure AD B2C; uygulamalarınızın, açık standart protokolleri kullanarak sosyal hesaplarda ve kurumsal hesaplarda kimlik doğrulaması gerçekleştirmesine olanak tanır. Bu hızlı başlangıçta, sosyal kimlik sağlayıcısı kullanıp Azure AD B2C korumalı web API’si çağırarak oturum açmak için bir tek sayfalı uygulama kullanırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio Code](https://code.visualstudio.com/)
- [Node.js](https://nodejs.org/en/download/)
- Facebook, Google veya Microsoft'tan sosyal hesap
- GitHub kod örneği: [active-directory-b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)

    Zip [arşivini indirebilir](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya depoyu klonlayabilirsiniz:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

1. Node.js komut isteminde şu komutları çalıştırarak sunucuyu başlatın:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    npm install && npm update
    npm start
    ```

    *server.js* tarafından başlatılan sunucu, dinlediği bağlantı noktasını görüntüler:

    ```console
    Listening on port 6420...
    ```

1. Uygulamanın URL'sine göz atın. Örneğin, `http://localhost:6420`.

    ![Tarayıcıda gösterilen tek sayfalık uygulama örnek uygulaması](./media/quickstart-single-page-app/sample-app-spa.png)

## <a name="sign-in-using-your-account"></a>Hesabınızı kullanarak oturum açın

1. Kullanıcı yolculuğuna başlamak için **Oturum Aç'ı** seçin.
1. Azure AD B2C, örnek web uygulaması için Fabrikam adlı hayali bir şirket için bir oturum açma sayfası sunar. Sosyal kimlik sağlayıcısı kullanarak kaydolmak için kullanmak istediğiniz kimlik sağlayıcısının düğmesini seçin.

    ![Kimlik sağlayıcı düğmelerini gösteren Oturum Aç veya Kaydol sayfası](./media/quickstart-single-page-app/sign-in-or-sign-up-spa.png)

    Sosyal hesap kimlik bilgilerinizi kullanarak kimlik doğrulaması (oturum açma) ve sosyal hesabınızdaki bilgileri okumak için başvuruyunu yetkilendiriyorsunuz. Erişim izni verdiğinizde uygulama sosyal hesabınızdan adınız ve şehriniz gibi profil bilgilerini alabilir.

1. Kimlik sağlayıcısına ilişkin oturum açma işlemini tamamlayın.

## <a name="access-a-protected-api-resource"></a>Korumalı bir API kaynağına erişme

Görüntü adınızın web API'sinden JSON nesnesi olarak döndürülmesini sağlamak için **Çağrı API'sini** seçin.

![Web API yanıtını gösteren tarayıcıda örnek uygulama](./media/quickstart-single-page-app/call-api-spa.png)

Örnek tek sayfalı uygulama, korumalı web API’si kaynağına yönelik isteğe bir erişim belirteci ekler.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, örnek tek sayfalık bir uygulama kullandınız:

- Sosyal kimlik sağlayıcısıyla oturum açın
- Azure AD B2C kullanıcı hesabı oluşturma (oturum açma sırasında otomatik olarak oluşturulur)
- Azure AD B2C tarafından korunan bir web API'sını arama

Kendi Azure AD B2C kiracınızı oluşturarak kullanmaya başlayın.

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)
