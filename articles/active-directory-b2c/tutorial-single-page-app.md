---
title: 'Öğretici: Tek sayfalık bir uygulamada kimlik doğrulamayı etkinleştirme'
titleSuffix: Azure AD B2C
description: Bu eğitimde, JavaScript tabanlı tek sayfalı bir uygulama (SPA) için kullanıcı girişi sağlamak için Azure Active Directory B2C'yi nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 04/04/2020
ms.custom: mvc, seo-javascript-september2019
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: d7cd437f597fc34fe83904715fc2e459dfe4550f
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875603"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-with-azure-ad-b2c"></a>Öğretici: Azure AD B2C ile tek sayfalık bir uygulamada kimlik doğrulamayı etkinleştirme

Bu öğretici, tek sayfalık bir uygulamada (SPA) kaydolmak ve kullanıcılara oturum açmak için Azure Active Directory B2C'yi (Azure AD B2C) nasıl kullanacağınızı gösterir.

Bu öğreticide, iki bölümlü bir serinin ilk:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda kayıtlı bir uygulamaya yanıt URL'si ekleme
> * GitHub'dan kod örneği indirin
> * Kiracınızla çalışacak şekilde örnek uygulamanın kodunu değiştirme
> * Kaydolma/kaydolma kullanıcı akışınızı kullanarak kaydolun

Serinin [bir sonraki öğretici](tutorial-single-page-app-webapi.md) kod örneğinin web API bölümünü sağlar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımlara devam etmeden önce aşağıdaki Azure AD B2C kaynaklarına ihtiyacınız var:

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* [Kiracınızda kayıtlı başvuru](tutorial-register-applications.md)
* Kiracınızda [oluşturulan kullanıcı akışları](tutorial-create-user-flows.md)

Ayrıca, yerel geliştirme ortamınızda aşağıdakileri yapmanız gerekir:

* [Visual Studio Code](https://code.visualstudio.com/) veya başka bir kod düzenleyicisi
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Ön koşulların bir parçası olarak tamamladığınız ikinci öğreticide, Azure AD B2C'de bir web uygulaması kaydettiniz. Bu öğreticideki kod örneğiyle iletişimi etkinleştirmek için, uygulama kaydına bir yanıt URL'si (yeniden yönlendirme URI olarak da adlandırılır) ekleyin.

Uygulamayı güncellemek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **Tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve ardından *webapp1* uygulamasını seçin.
1. **Yanıtla URL'nin**altında , ekle `http://localhost:6420`.
1. **Kaydet'i**seçin.
1. Özellikler **sayfasında, Uygulama Kimliği'ni**kaydedin. Tek sayfalı web uygulamasında kodu güncellediğinizde uygulama kimliğini daha sonraki bir adımda kullanırsınız.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtları (Önizleme) seçin,** **Sahip olunan uygulamalar** sekmesini seçin ve ardından *webapp1* uygulamasını seçin.
1. **Kimlik Doğrulama'yı**seçin, ardından yeni **deneyimi deneyin** 'i seçin (gösterilirse).
1. **Web'in**altında **URI ekle** `http://localhost:6420`bağlantısını seçin, enter 'u girin ve sonra **Kaydet'i**seçin.
1. **Genel Bakış**’ı seçin.
1. Tek sayfalı web uygulamasında kodu güncelleştirdiğinizde, **uygulama (istemci) kimliğini** sonraki bir adımda kullanmak üzere kaydedin.

* * *

## <a name="get-the-sample-code"></a>Örnek kodunu alma

Bu öğreticide, B2C kiracınızla çalışmak üzere GitHub'dan indirdiğiniz bir kod örneğini yapılandırırsınız. Örnek, tek sayfalı bir uygulamanın kullanıcı kaydolma ve kaydolma için Azure AD B2C'yi nasıl kullanabileceğini ve korumalı bir web API'sini arayarak (web API'sini serinin bir sonraki öğreticisinde etkinleştirebilirsiniz) gösterir.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Örneği güncelleştirin

Örneği aldığınıza göre, kodu Azure AD B2C kiracı adınız ve daha önceki bir adımda kaydettiğiniz uygulama kimliğiyle güncelleştirin.

1. *JavaScriptSPA* klasöründeki *authConfig.js* dosyasını açın.
1. Nesnede, `msalConfig` güncelleştirme:
    * `clientId`daha önceki bir adımda kaydettiğiniz **Uygulama (istemci) kimliği** ile değeri ile
    * `authority`Uri, Azure AD B2C kiracı adınız ve ön koşulların bir parçası olarak oluşturduğunuz kaydolma/kaydolma kullanıcı akışının adı (örneğin, *B2C_1_signupsignin1)*

    ```javascript
    const msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", // Replace this value with your Application (client) ID
            authority: "https://your-b2c-tenant.b2clogin.com/your-b2c-tenant.onmicrosoft.com/B2C_1_signupsignin1", // Update with your tenant and user flow names
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir konsol penceresi açın ve örneği içeren dizine değiştirin. Örneğin:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Aşağıdaki komutları çalıştırın:

    ```console
    npm install && npm update
    npm start
    ```

    Konsol penceresi, yerel olarak çalışan Node.js sunucusunun bağlantı noktası numarasını görüntüler:

    ```console
    Listening on port 6420...
    ```
1. Yerel makinenizde çalışan web uygulamasını görüntülemek için `http://localhost:6420` göz atın.

    :::image type="content" source="media/tutorial-single-page-app/web-app-spa-01-not-logged-in.png" alt-text="Yerel olarak çalışan tek sayfalı uygulamayı gösteren web tarayıcısı":::

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

Bu örnek uygulama kaydolma, oturum açma ve parola sıfırlamayı destekler. Bu eğitimde, bir e-posta adresi kullanarak kaydolun.

1. Daha önceki bir adımda belirttiğiniz *B2C_1_signupsignin1* kullanıcı akışını başlatmak için **Oturum Aç'ı** seçin.
1. Azure AD B2C, kaydolma bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından, **şimdi kaydol** bağlantısını seçin.
1. Kaydolma iş akışı, bir e-posta adresi kullanarak kullanıcının kimliğini toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    :::image type="content" source="media/tutorial-single-page-app/user-flow-sign-up-workflow-01.png" alt-text="Azure AD B2C kullanıcı akışına göre görüntülenen sayfayı kaydedin":::

1. Azure AD B2C dizininde yerel bir hesap oluşturmak için **Oluştur'u** seçin.

**Oluştur'u**seçtiğinizde, uygulama oturum açmış kullanıcının adını gösterir.

:::image type="content" source="media/tutorial-single-page-app/web-app-spa-02-logged-in.png" alt-text="Kullanıcı oturum açmış tek sayfalık uygulamayı gösteren web tarayıcısı":::

Oturum açma seçeneğini test etmek **istiyorsanız, Oturum Aç** düğmesini seçin ve oturum açtığınızda girdiğiniz e-posta adresi ve şifreyle Oturum **Aç'ı** seçin.

### <a name="what-about-calling-the-api"></a>API'yi aramaya ne dersin?

Oturum açtıktan sonra **Çağrı API** düğmesini seçerseniz, API çağrısının sonuçları yerine kaydol/kaydolun kullanıcı akışı sayfası sunulur. Azure AD B2C kiracınızda kayıtlı bir web API uygulamasıyla iletişim kurmak için *your* uygulamanın API bölümünü henüz yapılandırmadığınız için bu beklenir.

Bu noktada, uygulama hala demo kiracı (fabrikamb2c.onmicrosoft.com) kayıtlı API ile iletişim kurmaya çalışıyor ve bu kiracı ile kimlik doğrulaması değil çünkü, kaydolun/kaydolun sayfası görüntülenir.

Korumalı API'yi etkinleştirmek için serinin bir sonraki öğreticisine geçin [(Sonraki adımlar](#next-steps) bölümüne bakın).

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, kaydolma ve oturum açma özelliği sağlamak için Azure AD B2C kiracınızdaki bir kullanıcı akışıyla çalışacak şekilde tek sayfalık bir uygulama yapılandırıldınız. Şu adımları tamamladınız:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda kayıtlı bir uygulamaya yanıt URL'si eklendi
> * GitHub'dan kod örneği indirildi
> * Örnek uygulamanın kodunu kiracınızla çalışacak şekilde değiştirdi
> * Kaydolma/kaydolma kullanıcı akışınızı kullanarak kaydolun

Şimdi SPA'dan korunan bir web API'sine erişim sağlamak için serinin bir sonraki öğreticisine geçin:

> [!div class="nextstepaction"]
> [Öğretici: Tek sayfalık bir uygulamadan web API'sini koruma ve >](tutorial-single-page-app-webapi.md)
