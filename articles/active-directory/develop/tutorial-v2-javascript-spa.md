---
title: Azure AD v 2.0 JavaScript tek sayfalı uygulama (SPA) destekli kurulum | Microsoft Docs
description: JavaScript SPA uygulamalarının Azure Active Directory v 2.0 uç noktası tarafından erişim belirteçleri gerektiren bir API 'YI nasıl çağırabilen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: cee0884ef20ef9cfd63d81d6f223705d34c65ccb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69511809"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Kullanıcı oturum açma ve JavaScript tek sayfalı uygulamadan (SPA) Microsoft Graph API 'sini çağırma

Bu kılavuzda, bir JavaScript tek sayfalı uygulamasının (SPA) kişisel, iş ve okul hesaplarında nasıl oturum açıp Microsoft Identity platform uç noktasından erişim belirteçleri gerektiren API 'Leri veya diğer API 'Leri nasıl Microsoft Graph arayacağı gösterilmektedir.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulamanın nasıl çalıştığı

![Bu öğreticiler tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Bu kılavuz tarafından oluşturulan örnek uygulama, JavaScript SPA 'nın Microsoft Graph API 'sini veya Microsoft Identity platform uç noktasından belirteçleri kabul eden bir Web API 'sini sorgulamasına olanak sağlar. Bu senaryo için, bir Kullanıcı oturum açtıktan sonra, yetkilendirme üst bilgisi aracılığıyla bir erişim belirteci istenir ve HTTP isteklerine eklenir. Belirteç alma ve yenileme işlemleri Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından gerçekleştirilir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıklar

Bu kılavuz aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal. js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript önizlemesi için Microsoft kimlik doğrulama kitaplığı|

> [!NOTE]
> *msal. js* , kişisel, okul ve iş hesaplarının oturum açmasını ve belirteçleri almasını sağlayan *Microsoft Identity platform uç noktasını* hedefler. *Microsoft Identity platform uç noktasında* [bazı sınırlamalar](azure-ad-endpoint-comparison.md#limitations)vardır.
> V 1.0 ve v 2.0 uç noktaları arasındaki farkları anlamak için [uç nokta karşılaştırma kılavuzunu](azure-ad-endpoint-comparison.md)okuyun.

<!--end-collapse-->

## <a name="set-up-your-web-server-or-project"></a>Web sunucunuzu veya projenizi ayarlama

> Bunun yerine bu örneğin projesini indirmeyi tercih et mi? Aşağıdakilerden birini yapın:
> 
> - Projeyi Node. js gibi yerel bir Web sunucusuyla çalıştırmak için [Proje dosyalarını indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> - Seçim Projeyi IIS sunucusuyla çalıştırmak için [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).
>
> Daha sonra kod örneğini yürütmeden önce yapılandırmak için [yapılandırma adımına](#register-your-application)atlayın.

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticiyi çalıştırmak için [Node. js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core)veya [Visual Studio 2017](https://www.visualstudio.com/downloads/)ile IIS Express tümleştirme gibi yerel bir Web sunucusuna ihtiyacınız vardır.

* Projeyi çalıştırmak için Node. js kullanıyorsanız, proje dosyalarını düzenlemek için [Visual Studio Code](https://code.visualstudio.com/download)gibi bir tümleşik geliştirme ORTAMı (IDE) yükleyebilirsiniz.

* Bu kılavuzdaki yönergeler hem Node. js hem de Visual Studio 2017 tabanlıdır, ancak başka bir geliştirme ortamı veya Web sunucusu kullanabilirsiniz.

## <a name="create-your-project"></a>Projenizi oluşturma

> ### <a name="option-1-nodejs-or-other-web-servers"></a>Seçenek 1: Node. js veya diğer Web sunucuları
> [Node. js](https://nodejs.org/en/download/)' yi yüklediğinizden emin olun ve ardından aşağıdakileri yapın:
> - Uygulamanızı barındırmak için bir klasör oluşturun.
>
> ### <a name="option-2-visual-studio"></a>Seçenek 2: Visual Studio
> Visual Studio kullanıyorsanız ve yeni bir proje oluşturuyorsanız, şunları yapın:
> 1. Visual Studio'da **dosya** > **yeni** > **proje**.
> 1. **Visual C#\Web** bölümünde **ASP.NET Web Uygulaması (.NET Framework)** girişini seçin.
> 1. Uygulamanız için bir ad girin ve ardından **Tamam**' ı seçin.
> 1. **Yeni ASP.NET Web uygulaması**altında **boş**' ı seçin.

## <a name="create-the-spa-ui"></a>SPA Kullanıcı arabirimini oluşturma
1. JavaScript SPA 'niz için bir *index. html* dosyası oluşturun. Visual Studio kullanıyorsanız, projeyi (proje kök klasörü) seçin, sağ tıklayın ve**Yeni öğe** >  **Ekle** > **HTML sayfasını**seçin ve dosya *index. html*olarak adlandırın.

1. *İndex. html* dosyasında aşağıdaki kodu ekleyin:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

   > [!TIP]
   > Önceki betikteki MSAL. js sürümünü [msal. js yayınları](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)altındaki en son yayınlanan sürümle değiştirebilirsiniz.

## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Kullanıcı oturumu açmak için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanın

1. Aşağıdaki kodu `index.html` `<script></script>` etiketleri içinde dosyanıza ekleyin:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "Enter_the_Application_Id_here"
            authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };

    var graphConfig = {
        graphMeEndpoint: "https://graph.microsoft.com/v1.0/me"
    };

    // this can be used for login or token request, however in more complex situations
    // this can have diverging options
    var requestObj = {
        scopes: ["user.read"]
    };

    var myMSALObj = new Msal.UserAgentApplication(msalConfig);
    // Register Callbacks for redirect flow
    myMSALObj.handleRedirectCallback(authRedirectCallBack);


    function signIn() {

        myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
            //Login Success
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }).catch(function (error) {
            console.log(error);
        });
    }

    function acquireTokenPopupAndCallMSGraph() {
        //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
        myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error);
            // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
            // Call acquireTokenPopup(popup window)
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
                    callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                }).catch(function (error) {
                    console.log(error);
                });
            }
        });
    }


    function graphAPICallback(data) {
        document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
    }


    function showWelcomeMessage() {
        var divWelcome = document.getElementById('WelcomeMessage');
        divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
        var loginbutton = document.getElementById('SignIn');
        loginbutton.innerHTML = 'Sign Out';
        loginbutton.setAttribute('onclick', 'signOut();');
    }


    //This function can be removed if you do not need to support IE
    function acquireTokenRedirectAndCallMSGraph() {
         //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
         myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
             callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
         }).catch(function (error) {
             console.log(error);
             // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
             // Call acquireTokenRedirect
             if (requiresInteraction(error.errorCode)) {
                 myMSALObj.acquireTokenRedirect(requestObj);
             }
         });
     }


    function authRedirectCallBack(error, response) {
        if (error) {
            console.log(error);
        }
        else {
            if (response.tokenType === "access_token") {
                callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
            } else {
                console.log("token type is:" + response.tokenType);
            }
        }
    }

    function requiresInteraction(errorCode) {
        if (!errorCode || !errorCode.length) {
            return false;
        }
        return errorCode === "consent_required" ||
            errorCode === "interaction_required" ||
            errorCode === "login_required";
    }

    // Browser check variables
    var ua = window.navigator.userAgent;
    var msie = ua.indexOf('MSIE ');
    var msie11 = ua.indexOf('Trident/');
    var msedge = ua.indexOf('Edge/');
    var isIE = msie > 0 || msie11 > 0;
    var isEdge = msedge > 0;
    //If you support IE, our recommendation is that you sign-in using Redirect APIs
    //If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
    // can change this to default an experience outside browser use
    var loginType = isIE ? "REDIRECT" : "POPUP";

    if (loginType === 'POPUP') {
        if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }
    }
    else if (loginType === 'REDIRECT') {
        document.getElementById("SignIn").onclick = function () {
            myMSALObj.loginRedirect(requestObj);
        };
        if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenRedirectAndCallMSGraph();
        }
    } else {
        console.error('Please set a valid login type');
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Bir Kullanıcı ilk kez **oturum aç** düğmesine tıkladıktan sonra, `signIn` yöntemi kullanıcının oturum açmasını çağırır. `loginPopup` Bu yöntem, kullanıcının kimlik bilgilerini sorması ve doğrulamak için *Microsoft Identity platform uç noktasıyla* bir açılan pencere açmaya neden olur. Başarılı bir oturum açma işleminin sonucu olarak, Kullanıcı özgün *Dizin. html* sayfasına yeniden yönlendirilir ve bir belirteç alınır, tarafından `msal.js` işlenir ve belirteçte bulunan bilgiler önbelleğe alınır. Bu belirteç, *kimlik belirteci* olarak bilinir ve Kullanıcı ile ilgili Kullanıcı görünen adı gibi temel bilgileri içerir. Herhangi bir amaçla bu belirteç tarafından verilen herhangi bir veriyi kullanmayı planlıyorsanız, belirtecin uygulamanız için geçerli bir kullanıcıya verildiğini garantilemek için bu belirtecin arka uç sunucunuz tarafından doğrulanması gerekir.

Bu kılavuz tarafından oluşturulan Spa, Kullanıcı `acquireTokenSilent` profili bilgileri için `acquireTokenPopup` Microsoft Graph API 'sini sorgulamak üzere kullanılan bir *erişim belirteci* almak için ve/veya çağırır. KIMLIK belirtecini doğrulayan bir örneğe ihtiyacınız varsa, GitHub 'da [Şu](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "GitHub Active-Directory-JavaScript-singlepageapp-DotNet-WebApi-v2 örnek") örnek uygulamasına göz atın; örnek, belirteç doğrulaması için bir ASP.NET Web API 'si kullanır.

#### <a name="getting-a-user-token-interactively"></a>Kullanıcı belirtecini etkileşimli olarak alma

İlk oturum açma işleminden sonra, kullanıcılardan bir kaynağa erişmek için bir belirteç isteme ihtiyacı olan her seferinde yeniden kimlik doğrulaması yapmasını istemek istemezsiniz. bu nedenle, belirteçleri almak için en fazla *Acquiretokensilent* kullanılmalıdır. Kullanıcıların Microsoft Identity platform uç noktası ile etkileşime geçmesini zorunlu kılmak için bazı durumlar vardır. bazı örnekler şunlardır:

- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
- Uygulamanız kullanıcının onaylaması gereken bir kaynağa erişim istediğinde
- İki faktörlü kimlik doğrulama gerektiğinde

*Acquiretokenpopup* ' ı çağırmak sonuçları, kullanıcıların kimlik bilgilerini onaylayarak kullanıcılara etkileşimde bulunmak gereken bir açılan pencere penceresinde (veya *Acquiretokenredirect* sonuçları, kullanıcıları Microsoft Identity platform uç noktasına yönlendirirken) gerekli kaynağa onay veya iki öğeli kimlik doğrulamayı tamamlama.

#### <a name="getting-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntemi `acquireTokenSilent` , Kullanıcı etkileşimi olmadan belirteç alma ve yenileme işlemleri gerçekleştirir. ( `loginPopup` Veya `loginRedirect`) ilk kez yürütüldükten sonra, `acquireTokenSilent` sonraki çağrılar için korunan kaynaklara erişmek üzere kullanılan belirteçleri elde etmek için kullanılan yöntem, istek veya yenileme belirteçleri sessizce yapılır.
`acquireTokenSilent`Bazı durumlarda başarısız olabilir; Örneğin, kullanıcının parolasının süresi sona erer. Uygulamanız bu özel durumu iki şekilde işleyebilir:

1. Hemen öğesine `acquireTokenPopup` çağrı yapın ve bu, kullanıcıdan oturum açmasını ister. Bu model yaygın olarak, uygulamada kullanıcının kullanabileceği kimliği doğrulanmamış içerik olmayan çevrimiçi uygulamalarda kullanılır. Bu Kılavuzlu kurulum tarafından oluşturulan örnek bu düzeni kullanır.

2. Uygulamalar, kullanıcıya etkileşimli bir oturum açma gerektiğini belirten bir görsel gösterge de oluşturabilir, böylece Kullanıcı, oturum açmak için doğru zamanı seçebilir veya uygulama daha sonra yeniden deneyebilir `acquireTokenSilent` . Bu, genellikle kullanıcı uygulamanın kesintiye uğramadan diğer işlevlerini kullanabilmesi durumunda kullanılır. Örneğin, uygulamada kimliği doğrulanmamış içerik mevcuttur. Bu durumda, Kullanıcı, korunan kaynağa erişmek veya güncel olmayan bilgileri yenilemek için ne zaman oturum açmak istediğine karar verebilir.

> [!NOTE]
> Bu hızlı başlangıç, `loginRedirect` Internet `acquireTokenRedirect` Explorer tarayıcısı tarafından açılan pencerelerin işlenmesiyle ilgili bilinen bir [sorundan](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) dolayı, kullanılan tarayıcı Internet Explorer olduğunda ve yöntemlerini kullanır.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yeni Edindiğiniz belirteci kullanarak Microsoft Graph API 'sini çağırın

Aşağıdaki kodu `index.html` `<script></script>` etiketleri içinde dosyanıza ekleyin:

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Korunan bir API 'ye karşı REST çağrısı yapma hakkında daha fazla bilgi

Bu kılavuz tarafından oluşturulan örnek uygulamada, `callMSGraph()` yöntemi, bir belirteç gerektiren korumalı bir kaynağa karşı http `GET` isteği oluşturmak için kullanılır ve sonra içeriği çağırana döndürür. Bu yöntem, alınan belirteci *http yetkilendirme üst bilgisine*ekler. Bu kılavuz tarafından oluşturulan örnek uygulama için kaynak, kullanıcının profil bilgilerini görüntüleyen Microsoft Graph API *Me* uç noktasıdır.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcının oturumunu kapatmak için bir yöntem ekleyin

Aşağıdaki kodu `index.html` `<script></script>` etiketleri içinde dosyanıza ekleyin:

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```

## <a name="register-your-application"></a>Uygulamanızı kaydedin

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanız için bir ad girin.
1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
1. **Yeniden yönlendirme URI 'si** bölümünde, açılan listede, **Web** platformunu seçin ve ardından değeri Web sunucunuza dayalı uygulama URL 'si olarak ayarlayın. 

   Visual Studio ve Node. js ' de yeniden yönlendirme URL 'sini ayarlama ve alma hakkında daha fazla bilgi için sonraki iki bölüme bakın.

1. **Kaydol**’u seçin.
1. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
1. Bu hızlı başlangıç, [örtük izin akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir. Kayıtlı uygulamanın sol bölmesinde **kimlik doğrulaması**' nı seçin.
1. **Gelişmiş ayarlar**' da, **örtük izin**' ın altında, **Kimlik belirteçleri** ve **erişim belirteçleri** onay kutularını seçin. KIMLIK belirteçleri ve erişim belirteçleri gereklidir, çünkü bu uygulamanın kullanıcıları oturum açması ve bir API çağırması gerekir.
1. **Kaydet**’i seçin.

> #### <a name="set-a-redirect-url-for-nodejs"></a>Node. js için yeniden yönlendirme URL 'SI ayarlama
> Node. js için, *Server. js* dosyasında Web sunucusu bağlantı noktasını ayarlayabilirsiniz. Bu öğretici, başvuru için 30662 numaralı bağlantı noktasını kullanır, ancak kullanılabilir başka bir bağlantı noktasını da kullanabilirsiniz. 
>
> Uygulama kayıt bilgilerinde bir yeniden yönlendirme URL 'SI ayarlamak için, **uygulama kayıt** bölmesine dönün ve aşağıdakilerden birini yapın:
>
> - *`http://localhost:30662/`* **Yeniden yönlendirme URL 'si**olarak ayarlayın.
> - Özel bir TCP bağlantı noktası kullanıyorsanız kullanın *`http://localhost:<port>/`*  *\<(bağlantı noktası >* özel TCP bağlantı noktası numarasıdır).
>
> #### <a name="set-a-redirect-url-for-visual-studio"></a>Visual Studio için yeniden yönlendirme URL 'SI ayarlama
> Visual Studio için yeniden yönlendirme URL 'sini almak için aşağıdakileri yapın:
> 1. İçinde **Çözüm Gezgini**, projeyi seçin.
>
>    **Özellikler** penceresi açılır. Açık değilse **F4**tuşuna basın.
>
>    ![JavaScriptSPA projesi Özellikler penceresi](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)
>
> 1. **URL** değerini kopyalayın.
 
> 1. **Uygulama kayıt** bölmesine dönün ve kopyalanmış değeri **yeniden yönlendirme URL 'si**olarak yapıştırın.

#### <a name="configure-your-javascript-spa"></a>JavaScript SPA 'nizi yapılandırma

1. Proje kurulumu sırasında oluşturduğunuz *index. html* dosyasında, uygulama kayıt bilgilerini ekleyin. Dosyanın üst kısmında, `<script></script>` etiketleri içinde aşağıdaki kodu ekleyin:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "<Enter_the_Application_Id_here>",
            authority: "https://login.microsoftonline.com/<Enter_the_Tenant_info_here>"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Konumlar:
    - Enter_the_Application_Id_here >, kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** .  *\<*
    - Enter_the_Tenant_info_here > aşağıdaki seçeneklerden birine ayarlanır:  *\<*
       - Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adı** (örneğin, *contoso.Microsoft.com*) ile değiştirin.
       - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **kuruluşlar**ile değiştirin.
       - Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları*destekliyorsa, bu değeri **ortak**ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına*yönelik desteği kısıtlamak için bu değeri tüketicilerle değiştirin.

## <a name="test-your-code"></a>Kodunuzu test etme

Aşağıdaki ortamlardan birini kullanarak kodunuzu test edin.

### <a name="test-with-nodejs"></a>Node. js ile test etme

Visual Studio kullanmıyorsanız, Web sunucunuzun başlatıldığından emin olun.

1. Sunucuyu, *index. html* dosyanızın konumunu temel alan bir TCP bağlantı noktasını dinleyecek şekilde yapılandırın. Node. js için, uygulama klasöründen bir komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek üzere Web sunucusunu başlatın:

    ```bash
    npm install
    node server.js
    ```
1. Tarayıcınızda, **\<http://span\<>/span > localhost: 30662** veya **http://\<span >\</span > localhost: {Port}** yazın; burada *bağlantı noktası* Web sunucunuzun bulunduğu bağlantı noktasıdır dinleniyor. *İndex. html* dosyanızın Içeriğini ve **oturum aç** düğmesini görmeniz gerekir.

### <a name="test-with-visual-studio"></a>Visual Studio ile test

Visual Studio kullanıyorsanız, proje çözümünü seçin ve ardından F5 ' i seçerek projenizi çalıştırın. Tarayıcı, http://<span></span>localhost: {Port} konumu olarak açılır ve **oturum aç** düğmesi görünür olmalıdır.

## <a name="test-your-application"></a>Uygulamanızı test edin

Tarayıcı *index. html* dosyanızı yükledikten sonra **oturum aç**' ı seçin. Microsoft Identity platform uç noktası ile oturum açmanız istenir:

![JavaScript SPA hesabı oturum açma penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlayın

Uygulamanızda ilk kez oturum açtığınızda profilinize erişim vermeniz ve oturumunuzu açmanız istenir:

!["Izin isteniyor" penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra, Kullanıcı profili bilgileriniz sayfada görüntülenen Microsoft Graph API yanıtında döndürülür.

![Microsoft Graph API çağrısının sonuçları](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API 'SI, kullanıcının profilini okumak için *Kullanıcı. Read* kapsamını gerektirir. Bu kapsam, kayıt portalı 'nda kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Microsoft Graph için diğer API 'Ler ve arka uç sunucunuza yönelik özel API 'Ler için ek kapsamlar gerekebilir. Örneğin, Microsoft Graph API 'SI, kullanıcıların takvimlerini listelemek için *takvimler. Read* Scope 'u gerektirir.

Kullanıcının takvimlerine bir uygulama bağlamında erişmek için, *takvimler.* uygulama kayıt bilgilerine, temsilci olarak oku iznini ekleyin. Ardından, *takvimlere. Read* kapsamını `acquireTokenSilent` çağrıya ekleyin.

>[!NOTE]
>Kapsam sayısını artırdıkça kullanıcıdan ek Yarışması istenebilir.

Bir arka uç API 'SI bir kapsam gerektirmiyorsa (önerilmez), belirteçleri elde etmek için aramalarındaki kapsam olarak *ClientID* 'yi kullanabilirsiniz.

<!--end-collapse-->

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)