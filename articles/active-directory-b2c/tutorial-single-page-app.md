---
title: 'Öğretici: Tek sayfalık bir uygulamada kimlik doğrulamayı etkinleştirme'
titleSuffix: Azure AD B2C
description: Bu eğitimde, JavaScript tabanlı tek sayfalı bir uygulama (SPA) için kullanıcı girişi sağlamak için Azure Active Directory B2C'yi nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/14/2019
ms.custom: mvc, seo-javascript-september2019
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 435800d9c6bfd9131d50681a9808f9836104fac0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78183355"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c-azure-ad-b2c"></a>Öğretici: Azure Active Directory B2C (Azure AD B2C) kullanarak tek sayfalık bir uygulamada kimlik doğrulamayı etkinleştirme

Bu öğretici, tek sayfalık bir uygulamada (SPA) oturum açmak ve kaydolmak için Azure Active Directory B2C'yi (Azure AD B2C) nasıl kullanacağınızı gösterir. Azure AD B2C, uygulamalarınızın açık standart protokoller kullanarak sosyal hesaplara, kurumsal hesaplara ve Azure Etkin Dizin hesaplarına kimlik doğrulamasını sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure AD B2C'deki uygulamayı güncelleştirme
> * Uygulamayı kullanacak şekilde örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolun

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımlara devam etmeden önce aşağıdaki Azure AD B2C kaynaklarına ihtiyacınız var:

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* [Kiracınızda kayıtlı başvuru](tutorial-register-applications.md)
* Kiracınızda [oluşturulan kullanıcı akışları](tutorial-create-user-flows.md)

Ayrıca, yerel geliştirme ortamınızda aşağıdakileri yapmanız gerekir:

* Kod düzenleyicisi, örneğin [Visual Studio Code](https://code.visualstudio.com/) veya Visual Studio [2019](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.2](https://dotnet.microsoft.com/download) veya sonrası
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Ön koşulların bir parçası olarak tamamladığınız ikinci öğreticide, Azure AD B2C'de bir web uygulaması kaydettiniz. Öğreticideki örnekle iletişimi etkinleştirmek için Azure AD B2C'deki uygulamaya yeniden yönlendirme URI eklemeniz gerekir.

Uygulamayı güncellemek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **Tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve ardından *webapp1* uygulamasını seçin.
1. **Yanıtla URL'nin**altında , ekle `http://localhost:6420`.
1. **Kaydet'i**seçin.
1. Özellikler **sayfasında, Uygulama Kimliği'ni**kaydedin. Tek sayfalı web uygulamasında kodu güncellediğinizde uygulama kimliğini daha sonraki bir adımda kullanırsınız.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtları (Önizleme) seçin,** **Sahip olunan uygulamalar** sekmesini seçin ve ardından *webapp1* uygulamasını seçin.
1. **Kimlik Doğrulama'yı**seçin, ardından yeni **deneyimi deneyin** 'i seçin (gösterilirse).
1. **Web'in**altında **URI ekle** `http://localhost:6420`bağlantısını seçin, enter 'u girin ve sonra **Kaydet'i**seçin.
1. **Genel Bakış**’ı seçin.
1. Tek sayfalı web uygulamasında kodu güncelleştirdiğinizde, **uygulama (istemci) kimliğini** sonraki bir adımda kullanmak üzere kaydedin.

* * *

## <a name="get-the-sample-code"></a>Örnek kodunu alma

Bu öğreticide, GitHub'dan indirdiğiniz bir kod örneğini yapılandırırsınız. Örnek, tek sayfalı bir uygulamanın kullanıcı kaydolma ve kaydolma ve korumalı web API'sini aramak için Azure AD B2C'yi nasıl kullanabileceğini göstermektedir.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Örneği güncelleştirin

Örneği aldığınıza göre, kodu Azure AD B2C kiracı adınız ve daha önceki bir adımda kaydettiğiniz uygulama kimliğiyle güncelleştirin.

1. Dosyayı `index.html` örnek dizinin kökünde açın.
1. `msalConfig` Tanımda, istemci **değeri** önceki bir adımda kaydettiğiniz Uygulama Kimliği ile değiştirin. Ardından, **yetki** URI değerini Azure AD B2C kiracı adınız ile güncelleştirin. Ayrıca URI'yi ön koşullardan birinde oluşturduğunuz kaydol/kaydol kullanıcı akışının adı ile güncelleyin (örneğin, *B2C_1_signupsignin1).*

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

    Bu öğreticide kullanılan kullanıcı akışının adı **B2C_1_signupsignin1.** Farklı bir kullanıcı akış adı kullanıyorsanız, değere `authority` bu adı belirtin.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir konsol penceresi açın ve örneği içeren dizine değiştirin. Örnek:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Aşağıdaki komutları çalıştırın:

    ```
    npm install && npm update
    node server.js
    ```

    Konsol penceresi, yerel olarak çalışan Node.js sunucusunun bağlantı noktası numarasını görüntüler:

    ```
    Listening on port 6420...
    ```

1. Uygulamayı `http://localhost:6420` görüntülemek için tarayıcınıza gidin.

Örnek, kaydolma, oturum açma, profil düzenleme ve parola sıfırlamayı destekler. Bu öğretici, bir kullanıcının e-posta adresini kullanarak nasıl kaydolduğunu vurgular.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

> [!WARNING]
> Kaydolduktan veya oturum açtıktan sonra [yetersiz izin hatası](#error-insufficient-permissions)görebilirsiniz. Kod örneğinin geçerli uygulaması nedeniyle, bu hata bekleniyor. Bu sorun, kod örneğinin gelecekteki bir sürümünde çözülür ve bu uyarı bu uyarı kaldırılır.

1. Daha önceki bir adımda belirttiğiniz *B2C_1_signupsignin1* kullanıcı akışını başlatmak için **Giriş'i** seçin.
1. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından, **şimdi kaydol** bağlantısını seçin.
1. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma kullanıcı akışı tarafından sunulan kayıt sayfası](./media/tutorial-single-page-app/azure-ad-b2c-sign-up-workflow.png)

1. Azure AD B2C dizininde yerel bir hesap oluşturmak için **Oluştur'u** seçin.

**Oluştur'u**seçtiğinizde, kayıt sayfası kapanır ve oturum açma sayfası yeniden görüntülenir.

Artık uygulamada oturum açabilmek için e-posta adresinizi ve şifrenizi kullanabilirsiniz.

### <a name="error-insufficient-permissions"></a>Hata: yetersiz izinler

Oturum açtıktan sonra, uygulama yetersiz izin hatası döndürebilir:

```Output
ServerError: AADB2C90205: This application does not have sufficient permissions against this web resource to perform the operation.
Correlation ID: ce15bbcc-0000-0000-0000-494a52e95cd7
Timestamp: 2019-07-20 22:17:27Z
```

Web uygulaması demo dizini, *fabrikamb2c*tarafından korunan bir web API erişmeye çalışıyor çünkü bu hatayı alırsınız. Erişim jetonuyalnızca Azure REKLAM dizininiz için geçerli olduğundan, API çağrısı yetkisizdir.

Bu hatayı gidermek için, dizininiz için korumalı bir web API'sı oluşturmak için serinin bir sonraki öğreticisine devam edin [(Sonraki adımlara](#next-steps)bakın).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, nasıl öğrendim:

> [!div class="checklist"]
> * Azure AD B2C'deki uygulamayı güncelleştirme
> * Uygulamayı kullanacak şekilde örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolun

Şimdi SPA'dan korunan bir web API'sine erişim sağlamak için serinin bir sonraki öğreticisine geçin:

> [!div class="nextstepaction"]
> [Öğretici: Azure AD B2C >kullanarak bir SPA'dan ASP.NET Core web API'sine erişim izni verme](tutorial-single-page-app-webapi.md)
