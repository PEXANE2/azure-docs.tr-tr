---
title: "Öğretici: Azure AD B2C kullanarak bir Node. js web API 'sini koruyun ve tek sayfalı bir uygulamaya (SPA) erişim izni verin"
titleSuffix: Azure AD B2C
description: Bu öğreticide, bir Node. js web API 'sini korumak ve tek sayfalı bir uygulamadan çağırmak için Active Directory B2C nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80875715"
---
# <a name="tutorial-protect-and-grant-access-to-a-nodejs-web-api-from-a-single-page-application-with-azure-ad-b2c"></a>Öğretici: Azure AD B2C ile tek sayfalı bir uygulamadan Node. js web API 'sine erişim izni verme

Bu öğreticide, tek sayfalı bir uygulamadan Azure Active Directory B2C (Azure AD B2C) korumalı bir Node. js web API 'sinin nasıl çağrılacağını gösterir.

Bu öğreticide, ikinci bölümlü bir serinin saniyesi:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda bir Web API uygulaması kaydı oluşturma
> * Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Kiracınızla çalışmak için bir Web API kodu örneğini değiştirme

Bu serideki [ilk öğreticide](tutorial-single-page-app.md) , kod örneğini indirmiş ve Azure AD B2C kiracınızda Kullanıcı akışı olan kullanıcıların oturum açması için değiştirmiş olursunuz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Öğreticideki adımları ve önkoşulları doldurun [: Azure AD B2C ile tek sayfalı bir uygulamada kimlik doğrulamasını etkinleştirme](tutorial-single-page-app.md)
* [Visual Studio Code](https://code.visualstudio.com/) veya başka bir kod Düzenleyicisi
* [Node.js](https://nodejs.org/en/download/)

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide Web API 'SI için hem okuma hem de yazma izinlerini tanımlarsınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Tek sayfalı uygulamayı yapılandırırken **Scopes** daha sonraki bir `demo.read` adımda kullanılacak kapsamın kapsamları altındaki değeri kaydedin. Tam kapsam değeri öğesine `https://contosob2c.onmicrosoft.com/api/demo.read`benzerdir.

## <a name="grant-permissions"></a>İzinleri verme

Başka bir uygulamadan korumalı bir Web API 'SI çağırmak için, bu uygulama izinlerini Web API 'sine vermeniz gerekir.

Önkoşul öğreticisinde, *WebApp1*adlı bir Web uygulaması oluşturdunuz. Bu öğreticide, *webapi1*önceki bir bölümünde oluşturduğunuz Web API 'sini çağırmak için bu uygulamayı yapılandırırsınız.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Tek sayfalı web uygulamanıza, belirtilen kapsamlar için korunan Web API 'sine izin verildi. Bir kullanıcı tek sayfalı uygulamayı kullanmak üzere Azure AD B2C kimliğini doğrular. Tek sayfalı uygulama, Azure AD B2C tarafından döndürülen bir erişim belirteciyle korunan Web API 'sine erişmek için yetkilendirme verme akışını kullanır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Web API 'SI kayıtlı ve kapsamları tanımladığınıza göre, Web API kodunu Azure AD B2C kiracınızla çalışacak şekilde yapılandırın. Bu öğreticide, GitHub 'dan indirdiğinizde örnek bir Node. js web API 'SI yapılandırırsınız.

[Bir \*. zip arşivi indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) veya örnek Web API projesini GitHub 'dan kopyalayın. GitHub 'da doğrudan [Azure-Samples/Active-Directory-B2C-JavaScript-NodeJS-WebApi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) projesine de gidebilirsiniz.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. *Config. js* dosyasını kod Düzenleyicinizde açın.
1. Değişken değerlerini, daha önce oluşturduğunuz uygulama kaydını yansıtacak şekilde değiştirin. Ayrıca, `policyName` önkoşullarını, önkoşulların bir parçası olarak oluşturduğunuz Kullanıcı akışıyla güncelleştirin. Örneğin, *B2C_1_signupsignin1*.

    ```javascript
    const clientID = "<your-webapi-application-ID>"; // Application (client) ID
    const b2cDomainHost = "<your-tenant-name>.b2clogin.com";
    const tenantId = "<your-tenant-ID>.onmicrosoft.com"; // Alternatively, you can use your Directory (tenant) ID (a GUID)
    const policyName = "B2C_1_signupsignin1";
    ```

#### <a name="enable-cors"></a>CORS'yi etkinleştirme

Tek sayfalı uygulamanızın Node. js web API 'sini çağırmasını sağlamak için, Web API 'sinde [CORS](https://expressjs.com/en/resources/middleware/cors.html) 'yi etkinleştirmeniz gerekir. Bir üretim uygulamasında, hangi etki alanının istek yapmakta olduğu konusunda dikkatli olmanız gerekir, ancak bu öğreticide herhangi bir etki alanından gelen isteklere izin verilir.

CORS 'yi etkinleştirmek için aşağıdaki ara yazılımı kullanın. Bu öğreticideki Node. js web API kodu örneğinde, zaten *index. js* dosyasına eklenmiştir.

```javascript
app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Authorization, Origin, X-Requested-With, Content-Type, Accept");
    next();
});
```

### <a name="configure-the-single-page-application"></a>Tek sayfalı uygulamayı yapılandırma

Serideki [önceki öğreticideki](tutorial-single-page-app.md) tek sayfalı uygulama (Spa), Kullanıcı kaydı ve oturum açma için Azure AD B2C kullanır ve varsayılan olarak, *fabrikamb2c* demo kiracısı tarafından korunan Node. js web API 'sini çağırır.

Bu bölümde, tek sayfalı Web uygulamasını, Azure AD B2C *kiracınız* tarafından korunan Node. js web API 'sini (ve yerel makinenizde çalıştırdığınız) çağırmak için güncelleşolursunuz.

SPA 'daki ayarları değiştirmek için:

1. Önceki öğreticide indirdiğiniz veya Klonladığınız [Active-Directory-B2C-JavaScript-msal-singlepageapp][github-js-spa] projesinde, *javascriptspa* klasörünün Içindeki *apiconfig. js* dosyasını açın.
1. Örneği için URI ile birlikte yapılandırın. daha önce oluşturduğunuz ve Web API 'sinin URL 'si. *okuma* kapsamı.
    1. `apiConfig` Tanımında `b2cScopes` değeri *demo. Read* Scope (daha önce KAYDETTIĞINIZ **kapsam** değeri) için tam URI ile değiştirin.
    1. Bu `webApi` değeri, önceki bir ADıMDA Web API uygulamasını kaydettiğinizde eklediğiniz yeniden yönlendirme URI 'si olarak değiştirin.

    API 'nin `/hello` uç noktada erişilebilir olduğundan, URI 'de */Hello* ' ı bırakın.

    `apiConfig` Tanım aşağıdaki kod bloğuna benzer olmalıdır, ancak bunun yerine B2C kiracınızın adı ile birlikte şöyle görünmelidir `<your-tenant-name>`:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    const apiConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/demo.read"],
      webApi: "http://localhost:5000/hello" // '/hello' should remain in the URI
    };
    ```

## <a name="run-the-spa-and-web-api"></a>SPA ve Web API 'sini çalıştırma

Artık tek sayfalı uygulamanın API 'ye kapsamlı erişimini test etmeye hazırsınız. Hem Node. js web API 'sini hem de yerel makinenizde örnek JavaScript tek sayfalı uygulamasını çalıştırın. Ardından, tek sayfalı uygulamada oturum açın ve korunan API 'ye bir istek başlatmak için **API çağrısı** düğmesini seçin.

Bu öğreticiyi izlediğinizde her iki uygulama da yerel olarak çalışıyor olsa da, bunları güvenli kaydolma/oturum açma için Azure AD B2C kullanacak ve korunan Web API 'sine erişim izni verecek şekilde yapılandırdık.

### <a name="run-the-nodejs-web-api"></a>Node. js web API 'sini çalıştırma

1. Bir konsol penceresi açın ve Node. js web API 'SI örneğini içeren dizine geçin. Örneğin:

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

1. Başka bir konsol penceresi açın ve JavaScript SPA örneğini içeren dizine geçin. Örneğin:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```

1. Aşağıdaki komutları çalıştırın:

    ```console
    npm install && npm update
    npm start
    ```

    Konsol penceresi, uygulamanın barındırıldığı bağlantı noktası numarasını görüntüler.

    ```console
    Listening on port 6420...
    ```

1. Uygulamayı görüntülemek `http://localhost:6420` için tarayıcınızda sayfasına gidin.

    ![Tarayıcıda gösterilen tek sayfalı uygulama örnek uygulaması](./media/tutorial-single-page-app-webapi/tutorial-01-sample-app-browser.png)

1. [Önceki öğreticide](tutorial-single-page-app.md)kullandığınız e-posta adresini ve parolayı kullanarak oturum açın. Başarılı oturum açma sırasında `User 'Your Username' logged-in` iletiyi görmeniz gerekir.
1. **API çağrısı** düğmesini seçin. SPA, Azure AD B2C bir yetkilendirme izni alır ve ardından, oturum açmış kullanıcının adını göstermek için korunan Web API 'sine erişir:

    ![API tarafından döndürülen Kullanıcı adı JSON sonucunu gösteren tarayıcıda tek sayfalı uygulama](./media/tutorial-single-page-app-webapi/tutorial-02-call-api.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda bir Web API uygulaması kaydı oluşturuldu
> * Web API 'SI için yapılandırılmış kapsamlar
> * Web API 'sine izin verildi
> * Kiracınızla çalışmak için bir Web API kodu örneğini değiştirme

Artık bir SPA 'nın korumalı bir Web API 'sinden kaynak isteği gördüğünüze göre, bu uygulama türlerinin birbirleriyle ve Azure AD B2C nasıl etkileşime gireceğini daha ayrıntılı bir şekilde anlayın.

> [!div class="nextstepaction"]
> [Active Directory B2C >kullanılabilecek uygulama türleri](application-types.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp
