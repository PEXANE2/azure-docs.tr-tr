---
title: 'Öğretici: Tek sayfalı bir uygulamada kimlik doğrulamasını etkinleştirme-Azure Active Directory B2C'
description: Tek sayfalı uygulama (JavaScript) için Kullanıcı oturumu açma sağlamak üzere Azure Active Directory B2C nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 07/24/2019
ms.custom: mvc, seo-javascript-september2019
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 06ac81105ad8871c934715c18cd5f78fc3ea05f5
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70858523"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak tek sayfalı bir uygulamada kimlik doğrulamasını etkinleştirme

Bu öğreticide, tek sayfalı bir uygulamada (SPA) oturum açmak ve kullanıcıları kaydolmak için Azure Active Directory (Azure AD) B2C 'nin nasıl kullanılacağı gösterilmektedir. Azure AD B2C, uygulamalarınızın, açık standart protokoller kullanarak sosyal hesaplar, kurumsal hesaplar ve Azure Active Directory hesaplar için kimlik doğrulaması yapmasına olanak sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticideki adımlara devam etmeden önce aşağıdaki Azure AD B2C kaynaklara sahip olmanız gerekir:

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* Kiracınızda [kayıtlı uygulama](tutorial-register-applications.md)
* Kiracınızda [oluşturulan kullanıcı akışları](tutorial-create-user-flows.md)

Ayrıca, yerel geliştirme ortamınızda aşağıdakiler gereklidir:

* Kod Düzenleyicisi, örneğin [Visual Studio Code](https://code.visualstudio.com/) veya [Visual Studio 2019](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2,2](https://dotnet.microsoft.com/download) veya üzeri
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önkoşulların bir parçası olarak tamamladığınız ikinci öğreticide, Azure AD B2C bir Web uygulaması kaydettiniz. Öğreticideki örnekle iletişimi etkinleştirmek için, Azure AD B2C içinde uygulamaya bir yeniden yönlendirme URI 'SI eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin ve abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve sonra **Azure AD B2C**' i arayıp seçin.
1. **Uygulamalar**' ı seçin ve ardından *WebApp1* uygulamasını seçin.
1. **Yanıt URL 'si**altında, `http://localhost:6420`ekleyin.
1. **Kaydet**’i seçin.
1. Özellikler sayfasında, **uygulama kimliğini**kaydedin. Uygulama KIMLIĞI ' ni, tek sayfalı Web uygulamasındaki kodu güncelleştirdiğinizde sonraki bir adımda kullanırsınız.

## <a name="get-the-sample-code"></a>Örnek kodunu alma

Bu öğreticide, GitHub 'dan indirdiyseniz bir kod örneği yapılandırırsınız. Örnek, tek sayfalı bir uygulamanın kullanıcı kaydı ve oturum açma için Azure AD B2C nasıl kullanılabileceğini ve korumalı bir Web API 'sini çağırabileceğinizi gösterir.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Örneği güncelleştirme

Örneği edindiğinizden, kodu Azure AD B2C kiracı adınızla ve önceki bir adımda kaydettiğiniz uygulama KIMLIĞIYLE güncelleştirin.

1. `index.html` Dosyayı örnek dizinin kökünde açın.
1. Tanımda, ClientID değerini önceki bir adımda kaydettiğiniz uygulama kimliğiyle değiştirin. `msalConfig` Sonra, Azure AD B2C kiracı adınızla **yetkili** URI değerini güncelleştirin. Ayrıca, ön koşullardan birinde oluşturduğunuz kaydolma/oturum açma Kullanıcı akışının adı ile URI 'yi güncelleştirin (örneğin, *B2C_1_signupsignin1*).

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", //This is your client ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Bu öğreticide kullanılan Kullanıcı akışının adı **B2C_1_signupsignin1**' dir. Farklı bir Kullanıcı akış adı kullanıyorsanız, bu adı `authority` değerde belirtin.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir konsol penceresi açın ve örneği içeren dizine geçin. Örneğin:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Aşağıdaki komutları çalıştırın:

    ```
    npm install && npm update
    node server.js
    ```

    Konsol penceresinde yerel olarak çalışan Node. js sunucusunun bağlantı noktası numarası görüntülenir:

    ```
    Listening on port 6420...
    ```

1. `http://localhost:6420` Uygulamayı görüntülemek için tarayıcınızda sayfasına gidin.

Örnek kaydolma, oturum açma, profil düzenlemesi ve parola sıfırlamayı destekler. Bu öğretici, bir kullanıcının e-posta adresi kullanarak nasıl kaydolduğunda vurgular.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Daha önceki bir adımda belirttiğiniz *B2C_1_signupsignin1* Kullanıcı akışını başlatmak Için **oturum aç** ' ı seçin.
1. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından **Şimdi kaydolun** bağlantısını seçin.
1. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve Kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma Kullanıcı akışından sunulan kaydolma sayfası](./media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.PNG)

1. Azure AD B2C dizininde yerel bir hesap oluşturmak için **Oluştur** ' u seçin.

**Oluştur**' u seçtiğinizde, kaydolma sayfası kapanır ve oturum açma sayfası yeniden görüntülenir.

Artık uygulamada oturum açmak için e-posta adresinizi ve parolanızı kullanabilirsiniz.

### <a name="error-insufficient-permissions"></a>Hata: yetersiz izinler

Oturum açtıktan sonra, uygulama yetersiz bir izin hatası görüntülüyor-bu **beklenen**bir hatadır:

```Output
ServerError: AADB2C90205: This application does not have sufficient permissions against this web resource to perform the operation.
Correlation ID: ce15bbcc-0000-0000-0000-494a52e95cd7
Timestamp: 2019-07-20 22:17:27Z
```

Web uygulaması, *fabrikamb2c*demo dizini tarafından korunan BIR Web API 'sine erişmeye çalıştığından bu hatayı alırsınız. Erişim belirteciniz yalnızca Azure AD dizininiz için geçerli olduğundan, API çağrısı bu nedenle yetkilendirilmemiş.

Bu hatayı düzeltemedi, dizininiz için korumalı bir Web API 'SI oluşturmak üzere serideki bir sonraki öğreticiye devam edin ( [sonraki adımlara](#next-steps)bakın).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şu şekilde nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

Artık, SPA 'dan korumalı bir Web API 'sine erişim sağlamak için serideki bir sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Öğretici: Azure AD B2C kullanarak bir SPA 'dan ASP.NET Core Web API 'sine erişim izni verme >](active-directory-b2c-tutorials-spa-webapi.md)
