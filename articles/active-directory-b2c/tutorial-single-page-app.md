---
title: 'Öğretici: tek sayfalı bir uygulamada kimlik doğrulamasını etkinleştirme'
titleSuffix: Azure AD B2C
description: Bu öğreticide, JavaScript tabanlı tek sayfalı uygulama (SPA) için Kullanıcı oturumu açma sağlamak üzere Azure Active Directory B2C nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 04/04/2020
ms.custom: mvc, seo-javascript-september2019, devx-track-js
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 3a3eb77315953c3791e09c4326af7cc3e3231a69
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92670033"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-with-azure-ad-b2c"></a>Öğretici: Azure AD B2C ile tek sayfalı bir uygulamada kimlik doğrulamasını etkinleştirme

Bu öğreticide, OAuth 2,0 örtük izin akışı kullanılarak tek sayfalı bir uygulamada (SPA) kullanıcılara kaydolmak ve oturum açmak için Azure Active Directory B2C (Azure AD B2C) nasıl kullanılacağı gösterilmektedir.

Bu öğreticide, birincisi iki bölümlü bir seride:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda kayıtlı bir uygulamaya yanıt URL 'SI ekleme
> * GitHub 'dan bir kod örneği indirin
> * Örnek uygulamanın kodunu kiracınızla çalışacak şekilde değiştirin
> * Kaydolma/oturum açma Kullanıcı akışınızı kullanarak kaydolun

Serideki [sonraki öğretici](tutorial-single-page-app-webapi.md) , kod ÖRNEĞININ Web API bölümünü sunar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımlara devam etmeden önce aşağıdaki Azure AD B2C kaynaklara sahip olmanız gerekir:

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* Kiracınızda [kayıtlı uygulama](tutorial-register-spa.md) (örtük akış seçeneklerini kullanın)
* Kiracınızda [oluşturulan kullanıcı akışları](tutorial-create-user-flows.md)

Ayrıca, yerel geliştirme ortamınızda aşağıdakiler gereklidir:

* [Visual Studio Code](https://code.visualstudio.com/) veya başka bir kod Düzenleyicisi
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önkoşulların bir parçası olarak tamamladığınız ikinci öğreticide, Azure AD B2C bir Web uygulaması kaydettiniz. Bu öğreticide kod örneğiyle iletişimi etkinleştirmek için uygulama kaydına bir yanıt URL 'SI (yeniden yönlendirme URI 'SI olarak da bilinir) ekleyin.

Azure AD B2C kiracınızdaki bir uygulamayı güncelleştirmek için yeni Birleşik **uygulama kayıtları** deneyimimizi veya eski  **uygulamalarımız (eski)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin](https://aka.ms/b2cappregtraining).

#### <a name="app-registrations"></a>[Uygulama kayıtları](#tab/app-reg-ga/)

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C** ' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C** seçin.
1. **Uygulama kayıtları** öğesini seçin, **sahip olunan uygulamalar** sekmesini seçin ve ardından *WebApp1* uygulamasını seçin.
1. **Web** altında **URI Ekle** bağlantısını seçin, girin `http://localhost:6420` .
1. **Örtük izin** ' ın altında, **erişim belirteçleri** ve **Kimlik belirteçleri** için henüz seçili değilse onay kutularını seçin ve ardından **Kaydet** ' i seçin.
1. **Genel bakış** 'ı seçin.
1. Tek sayfalı Web uygulamasındaki kodu güncelleştirdiğinizde daha sonraki bir adımda kullanmak üzere **uygulama (istemci) kimliğini** kaydedin.

#### <a name="applications-legacy"></a>[Uygulamalar (eski)](#tab/applications-legacy/)

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve sonra **Azure AD B2C** ' i arayıp seçin.
1. **Uygulamalar (eski)** öğesini seçin ve ardından *WebApp1* uygulamasını seçin.
1. **Yanıt URL 'si** altında, ekleyin `http://localhost:6420` .
1. **Kaydet** ’i seçin.
1. Özellikler sayfasında, **uygulama kimliğini** kaydedin. Uygulama KIMLIĞI ' ni, tek sayfalı Web uygulamasındaki kodu güncelleştirdiğinizde sonraki bir adımda kullanırsınız.

* * *

## <a name="get-the-sample-code"></a>Örnek kodunu alma

Bu öğreticide, GitHub 'dan yüklediğiniz bir kod örneğini B2C kiracınızla çalışacak şekilde yapılandırırsınız. Örnek, tek sayfalı bir uygulamanın kullanıcı kaydı ve oturum açma için Azure AD B2C nasıl kullanılabileceğini ve korumalı bir Web API 'SI (serideki bir sonraki öğreticide Web API 'sini etkinleştirmenizi) nasıl çağırabileceğinizi gösterir.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Örneği güncelleştirme

Örneği edindiğinizden, kodu Azure AD B2C kiracı adınızla ve önceki bir adımda kaydettiğiniz uygulama KIMLIĞIYLE güncelleştirin.

1. *authConfig.js* dosyasını *javascriptspa* klasörü içinde açın.
1. `msalConfig`Nesnede, Güncelleştir:
    * `clientId` önceki bir adımda kaydettiğiniz **uygulama (istemci) kimliğine** sahip değer ile
    * `authority` Azure AD B2C kiracı adınızla URI ve önkoşulların bir parçası olarak oluşturduğunuz kaydolma/oturum açma Kullanıcı akışının adı (örneğin, *B2C_1_signupsignin1* )

    ```javascript
    const msalConfig = {
        auth: {
          clientId: "00000000-0000-0000-0000-000000000000", // Replace this value with your Application (client) ID
          authority: b2cPolicies.authorities.signUpSignIn.authority,
          validateAuthority: false
        },
        cache: {
          cacheLocation: "localStorage",
          storeAuthStateInCookie: true
        }
    };

    const loginRequest = {
       scopes: ["openid", "profile"],
    };

    const tokenRequest = {
      scopes: apiConfig.b2cScopes // i.e. ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"]
    };
    ```

1. *authConfig.js* dosyasını *javascriptspa* klasörü içinde açın.
1. `msalConfig`Nesnede, Güncelleştir:
    * `clientId`önceki bir adımda kaydettiğiniz **uygulama (istemci) kimliğiyle**
    * `authority` Azure AD B2C kiracı adınızla URI ve önkoşulların bir parçası olarak oluşturduğunuz kaydolma/oturum açma Kullanıcı akışının adı (örneğin, *B2C_1_signupsignin1* )
1. *policies.js* dosyasını açın.
1. Ve için girdileri bulun `names` `authorities` ve adım 2 ' de oluşturduğunuz ilkelerin adlarıyla uygun şekilde değiştirin. `fabrikamb2c.onmicrosoft.com`Örneğin, Azure AD B2C kiracınızın adıyla değiştirin `https://<your-tenant-name>.b2clogin.com/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>` .
1. *apiConfig.js* dosyasını açın.
1. Kapsamların atamasını bulun `b2cScopes` ve URL 'yi, Web API 'si için oluşturduğunuz kapsam URL 'si ile değiştirin (örneğin,) `b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/helloapi/demo.read"]` .
1. API URL 'SI için atamayı bulun `webApi` ve geçerlI URL 'yi, 4. adımda Web API 'nizi DAĞıTTıĞıNıZ URL ile değiştirin `webApi: http://localhost:5000/hello` .

Elde edilen kodunuz aşağıdaki gibi görünmelidir:

### <a name="authconfigjs"></a>authConfig.js

```javascript
const msalConfig = {
  auth: {
    clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902",
    authority: b2cPolicies.authorities.signUpSignIn.authority,
    validateAuthority: false
  },
  cache: {
    cacheLocation: "localStorage",
    storeAuthStateInCookie: true
  }
};

const loginRequest = {
  scopes: ["openid", "profile"],
};

const tokenRequest = {
  scopes: apiConfig.b2cScopes // i.e. ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"]
};
```
### <a name="policiesjs"></a>policies.js

```javascript
const b2cPolicies = {
    names: {
        signUpSignIn: "b2c_1_susi",
        forgotPassword: "b2c_1_reset",
        editProfile: "b2c_1_edit_profile"
    },
    authorities: {
        signUpSignIn: {
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi",
        },
        forgotPassword: {
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_reset",
        },
        editProfile: {
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_edit_profile"
        }
    },
}
```
### <a name="apiconfigjs"></a>apiConfig.js

```javascript
const apiConfig = {
  b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"],
  webApi: "https://fabrikamb2chello.azurewebsites.net/hello"
};
```

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir konsol penceresi açın ve örneği içeren dizine geçin. Örneğin:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Aşağıdaki komutları çalıştırın:

    ```console
    npm install && npm update
    npm start
    ```

    Konsol penceresinde yerel olarak çalışan Node.js sunucusunun bağlantı noktası numarası görüntülenir:

    ```console
    Listening on port 6420...
    ```
1. `http://localhost:6420`Yerel makinenizde çalışan Web uygulamasını görüntülemek için öğesine gidin.

    :::image type="content" source="media/tutorial-single-page-app/web-app-spa-01-not-logged-in.png" alt-text="Yerel olarak çalışan tek sayfalı uygulamayı gösteren Web tarayıcısı":::

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

Bu örnek uygulama kaydolma, oturum açma ve parola sıfırlama 'yı destekler. Bu öğreticide, bir e-posta adresi kullanarak kaydolabilirsiniz.

1. Daha önceki bir adımda belirttiğiniz *B2C_1_signupsignin1* Kullanıcı akışını başlatmak Için **oturum aç '** ı seçin.
1. Azure AD B2C, kaydolma bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından **Şimdi kaydolun** bağlantısını seçin.
1. Kaydolma iş akışı, bir e-posta adresi kullanarak kullanıcının kimliğini toplayıp doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve Kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    :::image type="content" source="media/tutorial-single-page-app/user-flow-sign-up-workflow-01.png" alt-text="Yerel olarak çalışan tek sayfalı uygulamayı gösteren Web tarayıcısı":::

1. Azure AD B2C dizininde yerel bir hesap oluşturmak için **Oluştur** ' u seçin.

**Oluştur** ' u seçtiğinizde, uygulama, oturum açmış kullanıcının adını gösterir.

:::image type="content" source="media/tutorial-single-page-app/web-app-spa-02-logged-in.png" alt-text="Yerel olarak çalışan tek sayfalı uygulamayı gösteren Web tarayıcısı":::

Oturum açma sınamasını yapmak **isterseniz oturumu Kapat düğmesini seçin** , **oturum aç** ' ı seçin ve kaydolduğunuzda girdiğiniz e-posta adresi ve parolayla oturum açın.

### <a name="what-about-calling-the-api"></a>API 'YI çağırma hakkında ne olacak?

Oturum açtıktan sonra **API çağrısı** düğmesini SEÇERSENIZ, API çağrısının sonuçları yerine kaydolma/oturum açma Kullanıcı akışı sayfası görüntülenir. Bu, uygulamanın API bölümünü Azure AD B2C *kiracınızda* kayıtlı BIR Web API uygulamasıyla iletişim kuracak şekilde yapılandırmadığınız için beklenmektedir.

Bu noktada, uygulama, tanıtım kiracısında (fabrikamb2c.onmicrosoft.com) kayıtlı olan API ile iletişim kurmaya çalışıyor ve bu kiracı ile kimlik doğrulamamış olduğunuzdan kaydolma/oturum açma sayfası görüntülenir.

Korumalı API 'yi etkinleştirmek için içindeki serideki bir sonraki öğreticiye geçin ( [sonraki adımlar](#next-steps) bölümüne bakın).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, oturum açma ve oturum açma yeteneğini sağlamak üzere Azure AD B2C kiracınızdaki bir Kullanıcı akışı ile çalışacak şekilde tek sayfalı bir uygulamayı yapılandırmış olursunuz. Şu adımları tamamladınız:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda kayıtlı bir uygulamaya yanıt URL 'SI eklendi
> * GitHub 'dan bir kod örneği indirildi
> * Örnek uygulamanın kodunu kiracınızla çalışacak şekilde değiştirdi
> * Kaydolma/oturum açma Kullanıcı akışınız kullanılarak kaydolup

Artık, SPA 'dan korumalı bir Web API 'sine erişim sağlamak için serideki bir sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Öğretici: tek sayfalı bir uygulamadan Web API 'sine koruma ve erişim izni verme >](tutorial-single-page-app-webapi.md)
