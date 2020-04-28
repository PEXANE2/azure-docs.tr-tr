---
title: JavaScript tek sayfalı uygulama öğreticisi-Microsoft Identity platform | Mavisi
description: JavaScript SPA uygulamaları, Azure Active Directory v 2.0 uç noktası tarafından erişim belirteçleri gerektiren bir API 'YI nasıl çağırabilir
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 03/20/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 6f0253490d39e69d491dd5fd3ab0d0d0a32d47bb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181571"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Kullanıcı oturum açma ve JavaScript tek sayfalı uygulamadan (SPA) Microsoft Graph API 'sini çağırma

Bu kılavuzda bir JavaScript tek sayfalı uygulamanın (SPA) nasıl kullanılabileceğini gösterilmektedir:
- Kişisel hesapların yanı sıra iş ve okul hesaplarında oturum açın
- Erişim belirteci alma
- *Microsoft Identity platform uç noktasından* erişim belirteçleri GEREKTIREN Microsoft Graph API 'sini veya diğer API 'leri çağırma

>[!NOTE]
> Microsoft Identity platform 'u kullanmaya yeni başladıysanız, [oturum açma kullanıcıları ile başlamanıza ve bir JAVASCRIPT Spa hızlı Başlangıçda erişim belirteci almanıza](quickstart-v2-javascript.md)önerilir.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulamanın nasıl çalıştığı

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

### <a name="more-information"></a>Daha fazla bilgi

Bu kılavuz tarafından oluşturulan örnek uygulama, JavaScript SPA 'nın Microsoft Graph API 'sini veya Microsoft Identity platform uç noktasından belirteçleri kabul eden bir Web API 'sini sorgulamasına olanak sağlar. Bu senaryoda, bir Kullanıcı oturum açtıktan sonra, yetkilendirme üst bilgisi aracılığıyla bir erişim belirteci istenir ve HTTP isteklerine eklenir. Bu belirteç, kullanıcının profilini ve postalarını **MS Graph API**aracılığıyla almak için kullanılacaktır. Belirteç alma ve yenileme, **JavaScript Için Microsoft kimlik doğrulama kitaplığı (msal)** tarafından işlenir.

### <a name="libraries"></a>Kitaplıklar

Bu kılavuz aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal. js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript için Microsoft kimlik doğrulama kitaplığı|

## <a name="set-up-your-web-server-or-project"></a>Web sunucunuzu veya projenizi ayarlama

> Bunun yerine bu örneğin projesini indirmeyi tercih et mi? [Proje dosyalarını indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> Yürütmeden önce kod örneğini yapılandırmak için [yapılandırma adımına](#register-your-application)atlayın.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğreticiyi çalıştırmak için [Node. js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core)veya [Visual Studio 2017](https://www.visualstudio.com/downloads/)ile IIS Express tümleştirme gibi yerel bir Web sunucusuna ihtiyacınız vardır.

* Bu kılavuzdaki yönergeler Node. js ' de yerleşik bir Web sunucusunu temel alır. Tümleşik geliştirme ortamınız (IDE) olarak [Visual Studio Code](https://code.visualstudio.com/download) kullanmanızı öneririz.

## <a name="create-your-project"></a>Projenizi oluşturma

[Node. js](https://nodejs.org/en/download/) ' nin yüklü olduğundan emin olun ve ardından uygulamanızı barındırmak için bir klasör oluşturun. Burada, `index.html` dosyanıza yönelik basit bir [Express](https://expressjs.com/) Web sunucusu uygulayacağız.

1. İlk olarak, Visual Studio Code tümleşik Terminal kullanarak, proje klasörünüzü bulun ve ardından NPM kullanarak Express 'ı yüklemeniz gerekir.

1. Ardından adlı `server.js`bir. js dosyası oluşturun ve ardından aşağıdaki kodu ekleyin:

   ```JavaScript
   const express = require('express');
   const morgan = require('morgan');
   const path = require('path');

   //initialize express.
   const app = express();

   // Initialize variables.
   const port = 3000; // process.env.PORT || 3000;

   // Configure morgan module to log all requests.
   app.use(morgan('dev'));

   // Set the front-end folder to serve public assets.
   app.use(express.static('JavaScriptSPA'))

   // Set up a route for index.html.
   app.get('*', function (req, res) {
       res.sendFile(path.join(__dirname + '/index.html'));
   });

   // Start the server.
   app.listen(port);
   console.log('Listening on port ' + port + '...');
   ```

Artık SPA 'larınızı sunacak basit bir sunucunuz var. Bu öğreticinin sonundaki amaçlanan klasör yapısı aşağıdaki gibidir:

![amaçlanan SPA klasör yapısının metin gösterimi](./media/tutorial-v2-javascript-spa/single-page-application-folder-structure.png)

## <a name="create-the-spa-ui"></a>SPA Kullanıcı arabirimini oluşturma

1. JavaScript SPA `index.html` 'niz için bir dosya oluşturun. Bu dosya, **önyükleme 4 çerçevesiyle** oluşturulmuş bir kullanıcı arabirimi uygular ve yapılandırma, kimlik doğrulama ve API çağrısı için betik dosyalarını içeri aktarır.

   `index.html` Dosyasına aşağıdaki kodu ekleyin:

   ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
       <title>Quickstart | MSAL.JS Vanilla JavaScript SPA</title>

       <!-- msal.js with a fallback to backup CDN -->
       <script type="text/javascript" src="https://alcdn.msauth.net/lib/1.2.1/js/msal.js" integrity="sha384-9TV1245fz+BaI+VvCjMYL0YDMElLBwNS84v3mY57pXNOt6xcUYch2QLImaTahcOP" crossorigin="anonymous"></script>
       <script type="text/javascript">
         if(typeof Msal === 'undefined')document.write(unescape("%3Cscript src='https://alcdn.msftauth.net/lib/1.2.1/js/msal.js' type='text/javascript' integrity='sha384-m/3NDUcz4krpIIiHgpeO0O8uxSghb+lfBTngquAo2Zuy2fEF+YgFeP08PWFo5FiJ' crossorigin='anonymous'%3E%3C/script%3E"));
       </script>

       <!-- adding Bootstrap 4 for UI components  -->
       <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
     </head>
     <body>
       <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
         <a class="navbar-brand" href="/">MS Identity Platform</a>
         <div class="btn-group ml-auto dropleft">
           <button type="button" id="signIn" class="btn btn-secondary" onclick="signIn()">Sign In</button>
           <button type="button" id="signOut" class="btn btn-success d-none" onclick="signOut()">Sign Out</button>
       </div>
       </nav>
       <br>
       <h5 class="card-header text-center">Vanilla JavaScript SPA calling MS Graph API with MSAL.JS</h5>
       <br>
       <div class="row" style="margin:auto" >
       <div id="card-div" class="col-md-3 d-none">
       <div class="card text-center">
         <div class="card-body">
           <h5 class="card-title" id="welcomeMessage">Please sign-in to see your profile and read your mails</h5>
           <div id="profile-div"></div>
           <br>
           <br>
           <button class="btn btn-primary" id="seeProfile" onclick="seeProfile()">See Profile</button>
           <br>
           <br>
           <button class="btn btn-primary d-none" id="readMail" onclick="readMail()">Read Mails</button>
         </div>
       </div>
       </div>
       <br>
       <br>
         <div class="col-md-4">
           <div class="list-group" id="list-tab" role="tablist">
           </div>
         </div>
         <div class="col-md-5">
           <div class="tab-content" id="nav-tabContent">
           </div>
         </div>
       </div>
       <br>
       <br>

       <!-- importing bootstrap.js and supporting js libraries -->
       <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
       <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
       <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>

       <!-- importing app scripts (load order is important) -->
       <script type="text/javascript" src="./authConfig.js"></script>
       <script type="text/javascript" src="./graphConfig.js"></script>
       <script type="text/javascript" src="./ui.js"></script>

       <!-- replace next line with authRedirect.js if you would like to use the redirect flow -->
       <!-- <script type="text/javascript" src="./authRedirect.js"></script>   -->
       <script type="text/javascript" src="./authPopup.js"></script>
       <script type="text/javascript" src="./graph.js"></script>
     </body>
   </html>
   ```

   > [!TIP]
   > Önceki betikteki MSAL. js sürümünü [msal. js yayınları](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)altındaki en son yayınlanan sürümle değiştirebilirsiniz.

2. Şimdi, DOM öğelerine erişen ve bunları güncelleştiren `ui.js`ve aşağıdaki kodu ekleyen adlı bir. js dosyası oluşturun:

   ```JavaScript
   // Select DOM elements to work with
   const welcomeDiv = document.getElementById("welcomeMessage");
   const signInButton = document.getElementById("signIn");
   const signOutButton = document.getElementById('signOut');
   const cardDiv = document.getElementById("card-div");
   const mailButton = document.getElementById("readMail");
   const profileButton = document.getElementById("seeProfile");
   const profileDiv = document.getElementById("profile-div");

   function showWelcomeMessage(account) {
     // Reconfiguring DOM elements
     cardDiv.classList.remove('d-none');
     welcomeDiv.innerHTML = `Welcome ${account.name}`;
     signInButton.classList.add('d-none');
     signOutButton.classList.remove('d-none');
   }

   function updateUI(data, endpoint) {
     console.log('Graph API responded at: ' + new Date().toString());

     if (endpoint === graphConfig.graphMeEndpoint) {
       const title = document.createElement('p');
       title.innerHTML = "<strong>Title: </strong>" + data.jobTitle;
       const email = document.createElement('p');
       email.innerHTML = "<strong>Mail: </strong>" + data.mail;
       const phone = document.createElement('p');
       phone.innerHTML = "<strong>Phone: </strong>" + data.businessPhones[0];
       const address = document.createElement('p');
       address.innerHTML = "<strong>Location: </strong>" + data.officeLocation;
       profileDiv.appendChild(title);
       profileDiv.appendChild(email);
       profileDiv.appendChild(phone);
       profileDiv.appendChild(address);

     } else if (endpoint === graphConfig.graphMailEndpoint) {
         if (data.value.length < 1) {
           alert("Your mailbox is empty!")
         } else {
           const tabList = document.getElementById("list-tab");
           tabList.innerHTML = ''; // clear tabList at each readMail call
           const tabContent = document.getElementById("nav-tabContent");

           data.value.map((d, i) => {
             // Keeping it simple
             if (i < 10) {
               const listItem = document.createElement("a");
               listItem.setAttribute("class", "list-group-item list-group-item-action")
               listItem.setAttribute("id", "list" + i + "list")
               listItem.setAttribute("data-toggle", "list")
               listItem.setAttribute("href", "#list" + i)
               listItem.setAttribute("role", "tab")
               listItem.setAttribute("aria-controls", i)
               listItem.innerHTML = d.subject;
               tabList.appendChild(listItem)

               const contentItem = document.createElement("div");
               contentItem.setAttribute("class", "tab-pane fade")
               contentItem.setAttribute("id", "list" + i)
               contentItem.setAttribute("role", "tabpanel")
               contentItem.setAttribute("aria-labelledby", "list" + i + "list")
               contentItem.innerHTML = "<strong> from: " + d.from.emailAddress.address + "</strong><br><br>" + d.bodyPreview + "...";
               tabContent.appendChild(contentItem);
             }
           });
         }
     }
   }
   ```

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Kimlik doğrulaması ile devam etmeden önce, **Azure Active Directory**uygulamanızı kaydedin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst köşedeki hesabı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Uygulamayı kaydet** sayfası görüntülendiğinde, uygulamanız için ad girin.
1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
1. **Yeniden yönlendirme URI 'si** bölümünde, açılan listeden **Web** platformunu seçin ve ardından değeri Web sunucunuza bağlı olan uygulama URL 'si olarak ayarlayın.
1. **Kaydol**’u seçin.
1. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
1. Bu hızlı başlangıç, [örtük izin akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir. Kayıtlı uygulamanın sol bölmesinde **kimlik doğrulaması**' nı seçin.
1. **Gelişmiş ayarlar**' da, **örtük izin**' ın altında, **Kimlik belirteçleri** ve **erişim belirteçleri** onay kutularını seçin. KIMLIK belirteçleri ve erişim belirteçleri gereklidir çünkü bu uygulamanın kullanıcıları oturum açması ve bir API çağırması gerekir.
1. **Kaydet**’i seçin.

> ### <a name="set-a-redirect-url-for-nodejs"></a>Node. js için yeniden yönlendirme URL 'SI ayarlama
>
> Node. js için, *Server. js* dosyasında Web sunucusu bağlantı noktasını ayarlayabilirsiniz. Bu öğretici 3000 numaralı bağlantı noktasını kullanır, ancak kullanılabilir başka herhangi bir bağlantı noktasını kullanabilirsiniz.
>
> Uygulama kayıt bilgilerinde bir yeniden yönlendirme URL 'SI ayarlamak için, **uygulama kayıt** bölmesine dönün ve aşağıdakilerden birini yapın:
>
> - *`http://localhost:3000/`* **Yeniden yönlendirme URL 'si**olarak ayarlayın.
> - Özel bir TCP bağlantı noktası kullanıyorsanız kullanın *`http://localhost:<port>/`* ( * \<bağlantı noktası>* özel TCP bağlantı noktası numarasıdır).
>   1. **URL** değerini kopyalayın.
>   1. **Uygulama kayıt** bölmesine dönün ve kopyalanmış değeri **yeniden yönlendirme URL 'si**olarak yapıştırın.
>

### <a name="configure-your-javascript-spa"></a>JavaScript SPA 'nizi yapılandırma

Kimlik doğrulaması için yapılandırma parametrelerinizi içeren `authConfig.js`adlı yeni bir. js dosyası oluşturun ve aşağıdaki kodu ekleyin:

```javascript
  const msalConfig = {
    auth: {
      clientId: "Enter_the_Application_Id_Here",
      authority: "Enter_the_Cloud_Instance_Id_HereEnter_the_Tenant_Info_Here",
      redirectUri: "Enter_the_Redirect_Uri_Here",
    },
    cache: {
      cacheLocation: "sessionStorage", // This configures where your cache will be stored
      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
    }
  };

  // Add here scopes for id token to be used at MS Identity Platform endpoints.
  const loginRequest = {
    scopes: ["openid", "profile", "User.Read"]
  };

  // Add here scopes for access token to be used at MS Graph API endpoints.
  const tokenRequest = {
    scopes: ["Mail.Read"]
  };
```

 Konumlar:
 - Enter_the_Application_Id_Here>, kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** . * \<*
 - Enter_the_Cloud_Instance_Id_Here>Azure bulutunun örneğidir. * \<* Ana veya küresel Azure bulutu için yalnızca girmeniz *https://login.microsoftonline.com*yeterlidir. **Ulusal** bulutlar (örneğin, Çin) için bkz. [Ulusal bulutlar](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
 - Enter_the_Tenant_info_here>aşağıdaki seçeneklerden birine ayarlanır: * \<*
   - Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adı** (örneğin, *contoso.Microsoft.com*) ile değiştirin.
   - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **kuruluşlar**ile değiştirin.
   - Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları*destekliyorsa, bu değeri **ortak**ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına*yönelik desteği kısıtlamak için bu değeri **tüketicilerle**değiştirin.


## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Kullanıcı oturumu açmak için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanın

Kimlik doğrulama ve belirteç alma mantığınızı `authPopup.js`içerecek adlı yeni bir. js dosyası oluşturun ve aşağıdaki kodu ekleyin:

   ```JavaScript
   const myMSALObj = new Msal.UserAgentApplication(msalConfig);

   function signIn() {
     myMSALObj.loginPopup(loginRequest)
       .then(loginResponse => {
         console.log('id_token acquired at: ' + new Date().toString());
         console.log(loginResponse);

         if (myMSALObj.getAccount()) {
           showWelcomeMessage(myMSALObj.getAccount());
         }
       }).catch(error => {
         console.log(error);
       });
   }

   function signOut() {
     myMSALObj.logout();
   }

   function callMSGraph(theUrl, accessToken, callback) {
       var xmlHttp = new XMLHttpRequest();
       xmlHttp.onreadystatechange = function () {
           if (this.readyState == 4 && this.status == 200) {
              callback(JSON.parse(this.responseText));
           }
       }
       xmlHttp.open("GET", theUrl, true); // true for asynchronous
       xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
       xmlHttp.send();
   }

   function getTokenPopup(request) {
     return myMSALObj.acquireTokenSilent(request)
       .catch(error => {
         console.log(error);
         console.log("silent token acquisition fails. acquiring token using popup");

         // fallback to interaction when silent call fails
           return myMSALObj.acquireTokenPopup(request)
             .then(tokenResponse => {
               return tokenResponse;
             }).catch(error => {
               console.log(error);
             });
       });
   }

   function seeProfile() {
     if (myMSALObj.getAccount()) {
       getTokenPopup(loginRequest)
         .then(response => {
           callMSGraph(graphConfig.graphMeEndpoint, response.accessToken, updateUI);
           profileButton.classList.add('d-none');
           mailButton.classList.remove('d-none');
         }).catch(error => {
           console.log(error);
         });
     }
   }

   function readMail() {
     if (myMSALObj.getAccount()) {
       getTokenPopup(tokenRequest)
         .then(response => {
           callMSGraph(graphConfig.graphMailEndpoint, response.accessToken, updateUI);
         }).catch(error => {
           console.log(error);
         });
     }
   }
   ```

### <a name="more-information"></a>Daha fazla bilgi

Kullanıcı **oturum aç** düğmesini ilk kez seçtikten sonra, `signIn` yöntemi kullanıcının oturum açmasını çağırır. `loginPopup` Bu yöntem, kullanıcının kimlik bilgilerini sormak ve doğrulamak için *Microsoft Identity platform uç noktasıyla* bir açılır pencere açar. Başarılı bir oturum açma işleminden sonra, Kullanıcı özgün *index. html* sayfasına yeniden yönlendirilir. Bir belirteç alınır, tarafından `msal.js`işlenir ve belirteçte bulunan bilgiler önbelleğe alınır. Bu belirteç, *kimlik belirteci* olarak bilinir ve Kullanıcı ile ilgili Kullanıcı görünen adı gibi temel bilgileri içerir. Herhangi bir amaçla bu belirteç tarafından verilen herhangi bir veriyi kullanmayı planlıyorsanız, belirtecin uygulamanız için geçerli bir kullanıcıya verildiğini garantilemek için bu belirtecin arka uç sunucunuz tarafından doğrulanması gerekir.

Bu kılavuz tarafından oluşturulan SPA, Kullanıcı `acquireTokenSilent` profili bilgileri IÇIN `acquireTokenPopup` Microsoft Graph API 'sini sorgulamak üzere kullanılan bir *erişim belirteci* almak için ve/veya çağırır. KIMLIK belirtecini doğrulayan bir örneğe ihtiyacınız varsa GitHub 'da [Bu](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "GitHub Active-Directory-JavaScript-singlepageapp-DotNet-WebApi-v2 örneği") örnek uygulamaya göz atın. Örnek, belirteç doğrulaması için bir ASP.NET Web API 'SI kullanır.

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

İlk oturum açma işleminden sonra, kullanıcılardan bir kaynağa erişmek için bir belirteç isteme ihtiyacı olan her seferinde yeniden kimlik doğrulaması yapmasını istemeniz gerekmez. Bu nedenle, en çok belirteçleri almak için *Acquiretokensilent* kullanılması gerekir. Ancak, kullanıcıların Microsoft Identity platform uç noktasıyla etkileşime geçmesini zorunlu hale getirmeniz gereken durumlar vardır. Örneklere şunlar dahildir:

- Parolanın süresi sona erdiği için kullanıcıların kimlik bilgilerini yeniden girmesi gerekir.
- Uygulamanız bir kaynağa erişim istiyor ve kullanıcının izni gerekiyor.
- İki öğeli kimlik doğrulaması gereklidir.

*Acquiretokenpopup* çağrısı, bir açılır pencere açar (veya *Acquiretokenredirect* kullanıcıları Microsoft Identity platform uç noktasına yönlendirir). Bu pencerede, kullanıcıların kimlik bilgilerini onaylayarak, gerekli kaynağa onay vererek veya iki öğeli kimlik doğrulamasını tamamlayarak etkileşimde olmaları gerekir.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntemi `acquireTokenSilent` , Kullanıcı etkileşimi olmadan belirteç alımı ve yenilemeyi işler. ( `loginPopup` Veya `loginRedirect`) ilk kez yürütüldükten sonra, `acquireTokenSilent` sonraki çağrılar için korunan kaynaklara erişmek üzere kullanılan belirteçleri elde etmek için yaygın olarak kullanılan yöntemdir. (Belirteçleri istek veya yenileme çağrısı sessizce yapılır.) `acquireTokenSilent` bazı durumlarda başarısız olabilir. Örneğin, kullanıcının parolasının kullanım süreniz olabilir. Uygulamanız bu özel durumu iki şekilde işleyebilir:

1. Hemen için `acquireTokenPopup` bir çağrı yapın, bu da Kullanıcı oturum açma istemi tetikler. Bu model yaygın olarak, uygulamada kullanıcının kullanabileceği kimliği doğrulanmamış içerik olmayan çevrimiçi uygulamalarda kullanılır. Bu Kılavuzlu kurulum tarafından oluşturulan örnek bu düzeni kullanır.

1. Uygulamalar, kullanıcıya etkileşimli bir oturum açma gerektiğini belirten bir görsel gösterge de oluşturabilir, böylece Kullanıcı, oturum açmak için doğru zamanı seçebilir veya uygulama daha sonra yeniden deneyebilir `acquireTokenSilent` . Bu, genellikle kullanıcı uygulamanın kesintiye uğramadan diğer işlevlerini kullanabilmesi durumunda kullanılır. Örneğin, uygulamada kimliği doğrulanmamış içerik bulunabilir. Bu durumda, Kullanıcı, korunan kaynağa erişmek veya güncel olmayan bilgileri yenilemek için ne zaman oturum açmak istediğinize karar verebilir.

> [!NOTE]
> Bu hızlı başlangıç, `loginPopup` ve `acquireTokenPopup` yöntemlerini varsayılan olarak kullanır. Tarayıcınız olarak Internet Explorer kullanıyorsanız, Internet Explorer 'ın açılır pencereleri işleme yöntemiyle ilgili bilinen `loginRedirect` bir `acquireTokenRedirect` [sorundan](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) dolayı ve yöntemlerinin kullanılması önerilir. Kullanarak `Redirect methods`aynı sonucu nasıl elde etmek istiyorsanız, lütfen [bkz](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/blob/quickstart/JavaScriptSPA/authRedirect.js)..

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-acquired"></a>Yeni aldığınız belirteci kullanarak Microsoft Graph API 'sini çağırın

1. İlk olarak, adlı `graphConfig.js`bir. js dosyası oluşturun ve bu dosyayı REST uç noktalarınızı depolayacaksınız. Aşağıdaki kodu ekleyin:

   ```JavaScript
      const graphConfig = {
        graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me",
        graphMailEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me/messages"
      };
   ```

   Konumlar:
   - Enter_the_Graph_Endpoint_Here>MS Graph API örneğidir. * \<* Genel MS Graph API uç noktası için, bu dizeyi ile `https://graph.microsoft.com`değiştirin. Ulusal bulut dağıtımları için lütfen [Graph API belgelerine](https://docs.microsoft.com/graph/deployments)bakın.

1. Ardından adlı `graph.js`bir. js dosyası oluşturun, bu, Microsoft Graph API 'sine Rest çağrısı yapar ve aşağıdaki kodu ekler:

   ```javascript
   function callMSGraph(endpoint, token, callback) {
     const headers = new Headers();
     const bearer = `Bearer ${token}`;

     headers.append("Authorization", bearer);

     const options = {
         method: "GET",
         headers: headers
     };

     console.log('request made to Graph API at: ' + new Date().toString());

     fetch(endpoint, options)
       .then(response => response.json())
       .then(response => callback(response, endpoint))
       .catch(error => console.log(error))
   }
   ```

### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Korunan bir API 'ye karşı REST çağrısı yapma hakkında daha fazla bilgi

Bu kılavuz tarafından oluşturulan örnek uygulamada, `callMSGraph()` yöntemi, bir belirteç gerektiren korumalı bir kaynağa karşı http `GET` isteği oluşturmak için kullanılır. İstek daha sonra içeriği çağırana döndürür. Bu yöntem, alınan belirteci *http yetkilendirme üst bilgisine*ekler. Bu kılavuz tarafından oluşturulan örnek uygulama için, kaynak, kullanıcının profil bilgilerini görüntüleyen Microsoft Graph API *Me* uç noktasıdır.

## <a name="test-your-code"></a>Kodunuza test etme

1. Sunucuyu, *index. html* dosyanızın konumunu temel alan bir TCP bağlantı noktasını dinleyecek şekilde yapılandırın. Node. js için, uygulama klasöründen bir komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek üzere Web sunucusunu başlatın:

   ```bash
   npm install
   npm start
   ```
1. Tarayıcınızda, veya **http://localhost:3000** **http://localhost:{port}** yazın; burada *bağlantı noktası* , Web sunucunuzun dinlediği bağlantı noktasıdır. *İndex. html* dosyanızın Içeriğini ve **oturum aç** düğmesini görmeniz gerekir.

## <a name="test-your-application"></a>Uygulamanızı test etme

Tarayıcı *index. html* dosyanızı yükledikten sonra **oturum aç**' ı seçin. Microsoft Identity platform uç noktası ile oturum açmanız istenir:

![JavaScript SPA hesabı oturum açma penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlayın

Uygulamanızda ilk kez oturum açtığınızda profilinize erişim vermeniz ve oturumunuzu açmanız istenir:

!["Izin isteniyor" penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra, Kullanıcı profili bilgileriniz görüntülenen Microsoft Graph API yanıtında döndürülür:

![Microsoft Graph API çağrısının sonuçları](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API 'SI, kullanıcının profilini okumak için *Kullanıcı. Read* kapsamını gerektirir. Varsayılan olarak, bu kapsam kayıt portalı 'nda kayıtlı her uygulamaya otomatik olarak eklenir. Microsoft Graph için diğer API 'Ler ve arka uç sunucunuza yönelik özel API 'Ler için ek kapsamlar gerekebilir. Örneğin, Microsoft Graph API 'SI, kullanıcının postalarını listelemek için *mail. Read* kapsamını gerektirir.

> [!NOTE]
> Kapsam sayısını artırdıkça kullanıcıdan ek Yarışması istenebilir.

Bir arka uç API 'SI bir kapsam gerektirmiyorsa (önerilmez), belirteçleri almak için çağrılarındaki kapsam olarak *ClientID* 'yi kullanabilirsiniz.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
