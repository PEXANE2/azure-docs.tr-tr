---
title: "Öğretici: Azure AD B2C'yi kullanarak bir Düğüm.js web API'sini koruyun ve tek sayfalık bir uygulamaya (SPA) erişim izni vermek"
titleSuffix: Azure AD B2C
description: Bu eğitimde, Bir Düğüm.js web API'sini korumak ve tek sayfalık bir uygulamadan aramak için Active Directory B2C'yi nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 04/04/2020
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 50524159186987b7a30015c878fa3fac949afc79
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875715"
---
# <a name="tutorial-protect-and-grant-access-to-a-nodejs-web-api-from-a-single-page-application-with-azure-ad-b2c"></a>Öğretici: Azure AD B2C ile tek sayfalı bir uygulamadan Bir Düğüm web API'sini koruyun ve erişim izni

Bu öğretici, tek sayfalık bir uygulamadan Azure Active Directory B2C (Azure AD B2C) korumalı Düğüm web API'sini nasıl çağırabileceğinizi gösterir.

Bu öğreticide, iki bölümlü bir serinin ikinci:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda web API uygulama kaydı oluşturma
> * Web API'si için kapsamları yapılandırma
> * Web API'sine izin verme
> * Kiracınızla çalışmak için web API kodu örneğini değiştirme

Bu serinin [ilk öğreticisinde,](tutorial-single-page-app.md) kod örneğini indirdiniz ve Azure AD B2C kiracınızda kullanıcı akışı olan kullanıcılarda oturum açacak şekilde değiştirdiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Tutorial: Azure AD B2C ile tek sayfalık bir uygulamada kimlik doğrulamayı etkinleştirme](tutorial-single-page-app.md) adımlarını ve ön koşulları tamamlayın
* [Visual Studio Code](https://code.visualstudio.com/) veya başka bir kod düzenleyicisi
* [Node.js](https://nodejs.org/en/download/)

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide, web API'si için hem okuma hem de yazma izinlerini tanımlarsınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Tek sayfalı uygulamayı yapılandırırken, kapsamın daha sonraki bir adımda kullanAbilmek için `demo.read` **Kapsamlar** altındaki değeri kaydedin. Tam kapsam değeri `https://contosob2c.onmicrosoft.com/api/demo.read`.

## <a name="grant-permissions"></a>İzinleri verme

Başka bir uygulamadan korumalı web API'sini çağırmak için, bu uygulama izinlerini web API'sine vermeniz gerekir.

Ön koşul öğreticisinde, *webapp1*adında bir web uygulaması oluşturdunuz. Bu öğreticide, bu uygulamayı önceki bölümde oluşturduğunuz web API'sını aramak üzere yapılandırırsınız, *webapi1.*

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Tek sayfalık web uygulamanız artık belirtilen kapsamlar için korumalı web API'sine izin ler verilmiştir. Bir kullanıcı, tek sayfalı uygulamayı kullanmak için Azure AD B2C ile kimlik doğrulaması yaptı. Tek sayfalı uygulama, Azure AD B2C tarafından döndürülen bir erişim belirteciyle korumalı web API'sine erişmek için yetkilendirme izni akışını kullanır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Artık web API'si kayıtlı ve kapsamlar tanımladığınıza göre, web API kodunu Azure AD B2C kiracınızla çalışacak şekilde yapılandırın. Bu öğreticide, GitHub'dan indirdiğiniz örnek bir Node.js web API'sını yapılandırırsınız.

Bir [ \*.zip arşivi indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) veya GitHub'dan örnek web API projesini klonla. GitHub'daki [Azure Örnekleri/active-directory-b2c-javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) projesine doğrudan göz atabilirsiniz.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. Kod düzenleyicinizde *config.js* dosyasını açın.
1. Daha önce oluşturduğunuz uygulama kaydının değerlerini yansıtacak şekilde değişken değerlerini değiştirin. Ayrıca, `policyName` ön koşulların bir parçası olarak oluşturduğunuz kullanıcı akışını da güncelleyin. Örneğin, *B2C_1_signupsignin1.*

    ```javascript
    const clientID = "<your-webapi-application-ID>"; // Application (client) ID
    const b2cDomainHost = "<your-tenant-name>.b2clogin.com";
    const tenantId = "<your-tenant-ID>.onmicrosoft.com"; // Alternatively, you can use your Directory (tenant) ID (a GUID)
    const policyName = "B2C_1_signupsignin1";
    ```

#### <a name="enable-cors"></a>CORS'yi etkinleştirme

Tek sayfalı uygulamanızın Node.js web API'yi aramasına izin vermek için, WEB API'sinde [CORS'ü](https://expressjs.com/en/resources/middleware/cors.html) etkinleştirmeniz gerekir. Bir üretim uygulamasında, hangi etki alanının istekte bulunduğuna dikkat etmelisiniz, ancak bu öğretici için herhangi bir etki alanından isteklere izin vermelisiniz.

CORS'i etkinleştirmek için aşağıdaki ara yazılımı kullanın. Bu öğreticideki Node.js web API kod örneğinde, *index.js* dosyasına zaten eklenmiştir.

```javascript
app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Authorization, Origin, X-Requested-With, Content-Type, Accept");
    next();
});
```

### <a name="configure-the-single-page-application"></a>Tek sayfalı uygulamayı yapılandırma

Serinin [önceki öğreticisinden](tutorial-single-page-app.md) tek sayfalık uygulama (SPA), kullanıcı kaydolma ve kaydolma için Azure AD B2C kullanır ve varsayılan olarak *fabrikamb2c* demo kiracıtarafından korunan Node.js web API'sini çağırır.

Bu bölümde, Azure AD B2C *kiracınız* tarafından korunan (ve yerel makinenizde çalıştırdığınız) Node.js web API'sını aramak için tek sayfalık web uygulamasını güncellersiniz.

SPA'daki ayarları değiştirmek için:

1. Önceki öğreticide indirdiğiniz veya klonladığınız [active-directory-b2c-javascript-msal-singlepageapp][github-js-spa] projesinde, *JavaScriptSPA* klasöründeki *apiConfig.js* dosyasını açın.
1. Örneği daha önce oluşturduğunuz *demo.read* kapsamı ve web API'sının URL'si için URI ile yapılandırın.
    1. `apiConfig` Tanımda, `b2cScopes` *demo.read* kapsamı (daha önce kaydettiğiniz **Kapsam** değeri) için değeri tam URI ile değiştirin.
    1. Web API `webApi` uygulamasını daha önceki bir adımda kaydettiğinizde eklediğiniz yeniden yönlendirme URI değerindeki etki alanını değiştirin.

    API'ye `/hello` bitiş noktasından erişilebildiğinden, URI'ye */merhaba'yı* bırakın.

    Tanım `apiConfig` aşağıdaki kod bloğuna benzer, ancak yerine B2C kiracı nın adı `<your-tenant-name>`ile bakmak gerekir:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    const apiConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/demo.read"],
      webApi: "http://localhost:5000/hello" // '/hello' should remain in the URI
    };
    ```

## <a name="run-the-spa-and-web-api"></a>SPA ve web API'yi çalıştırın

Artık tek sayfalı uygulamanın API'ye kapsamlı erişimini sınamaya hazırsınız. Hem Node.js web API'sini hem de örnek JavaScript tek sayfalık uygulamayı yerel makinenizde çalıştırın. Ardından, tek sayfalı uygulamada oturum açın ve korumalı **API'ye** istek başlatmak için Çağrı API düğmesini seçin.

Bu öğreticiyi uyguladığınızda her iki uygulama da yerel olarak çalışıyor olsa da, güvenli kaydolma/kaydolma için Azure AD B2C'yi kullanacak ve korunan web API'sine erişim izni verecek şekilde yapılandırıldınız.

### <a name="run-the-nodejs-web-api"></a>Düğüm.js web API çalıştırın

1. Bir konsol penceresi açın ve Node.js web API örneğini içeren dizine değiştirin. Örneğin:

    ```console
    cd active-directory-b2c-javascript-nodejs-webapi
    ```

1. Aşağıdaki komutları çalıştırın:

    ```console
    npm install && npm update
    node index.js
    ```

    Konsol penceresi, uygulamanın barındırıldığı bağlantı noktası numarasını görüntüler.

    ```console
    Listening on port 5000...
    ```

### <a name="run-the-single-page-app"></a>Tek sayfalı uygulamayı çalıştırma

1. Başka bir konsol penceresi açın ve JavaScript SPA örneğini içeren dizine değiştirin. Örneğin:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```

1. Aşağıdaki komutları çalıştırın:

    ```console
    npm install && npm update
    npm start
    ```

    Konsol penceresi, uygulamanın barındırıldığı yerin bağlantı noktası numarasını görüntüler.

    ```console
    Listening on port 6420...
    ```

1. Uygulamayı `http://localhost:6420` görüntülemek için tarayıcınızda gidin.

    ![Tarayıcıda gösterilen tek sayfalık uygulama örnek uygulaması](./media/tutorial-single-page-app-webapi/tutorial-01-sample-app-browser.png)

1. [Önceki öğreticide](tutorial-single-page-app.md)kullandığınız e-posta adresini ve parolayı kullanarak oturum açın. Başarılı bir oturum açtıktan `User 'Your Username' logged-in` sonra, iletiyi görmeniz gerekir.
1. Arama **API düğmesini** seçin. SPA, Azure AD B2C'den yetkilendirme hibesi alır ve oturum açmış kullanıcının adını görüntülemek için korumalı web API'sine erişir:

    ![Kullanıcı adı JSON sonucunu gösteren tarayıcıda TEK Sayfa uygulaması API tarafından döndürülür](./media/tutorial-single-page-app-webapi/tutorial-02-call-api.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda bir web API uygulama kaydı oluşturma
> * Web API'si için yapılandırılmış kapsamlar
> * Web API'sine verilen izinler
> * Kiracınızla çalışmak için bir web API kodu örneğini değiştirdi

Artık bir SPA'nın korumalı bir web API'sinden kaynak istediğini gördüğünüze göre, bu uygulama türlerinin birbirleriyle ve Azure AD B2C ile nasıl etkileşimde bulunduğunu daha iyi anlayabilirsiniz.

> [!div class="nextstepaction"]
> [Active Directory B2C >'da kullanılabilecek uygulama türleri](application-types.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp
