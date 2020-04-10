---
title: JavaScript tek sayfalık uygulama eğitimi - Microsoft kimlik platformu | Azure
description: JavaScript SPA uygulamaları, Azure Active Directory v2.0 bitiş noktasının erişim belirteçlerini gerektiren bir API'yi nasıl arayabilir?
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
ms.openlocfilehash: ec47850ce4cccb6a891c7e5aef2644550bc3e39a
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80990965"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Oturum açın ve JavaScript tek sayfalı bir uygulamadan (SPA) Microsoft Graph API'yi arayın

Bu kılavuz, bir JavaScript tek sayfalık uygulamanın (SPA) nasıl olabileceğini gösterir:
- Kişisel hesapların yanı sıra iş ve okul hesaplarında oturum açın 
- Erişim belirteci alma
- *Microsoft kimlik platformu bitiş noktasından* erişim belirteçleri gerektiren Microsoft Graph API veya diğer API'leri arayın

>[!NOTE]
> Microsoft kimlik platformunda yeniyseniz, Oturum Açma kullanıcılarıyla başlamanızı [ve JavaScript SPA quickstart'ında bir erişim jetonu almanızı](quickstart-v2-javascript.md)öneririz.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulama nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

Bu kılavuz tarafından oluşturulan örnek uygulama, JavaScript SPA'nın Microsoft Graph API'sini veya Microsoft kimlik platformu bitiş noktasından belirteçleri kabul eden bir web API'sini sorgulamasını sağlar. Bu senaryoda, bir kullanıcı giriş yaptıktan sonra, bir erişim belirteci istenir ve yetkilendirme üstbilgisi aracılığıyla HTTP isteklerine eklenir. Bu belirteç, kullanıcının profilini ve postalarını **MS Graph API**üzerinden elde etmek için kullanılacaktır. Belirteç edinme ve yenileme, **JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL)** tarafından işlenir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıklar

Bu kılavuzda aşağıdaki kitaplık kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript için Microsoft Kimlik Doğrulama Kitaplığı|

> [!NOTE]
> *Msal.js,* kişisel hesapların, okul ve iş hesaplarının oturum açmasını ve jeton edinmesini sağlayan Microsoft kimlik platformu bitiş noktasını hedefler. Microsoft kimlik platformu bitiş noktası [bazı sınırlamalar](../azuread-dev/azure-ad-endpoint-comparison.md#limitations)vardır.
> v1.0 ve v2.0 uç noktaları arasındaki farkları anlamak için [bitiş noktası karşılaştırma kılavuzuna](../azuread-dev/azure-ad-endpoint-comparison.md)bakın.

<!--end-collapse-->

## <a name="set-up-your-web-server-or-project"></a>Web sunucunuzu veya projenizi ayarlama

> Bunun yerine bu örneğin projesini indirmeyi mi tercih edersiniz? [Proje dosyalarını indirin.](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
>
> Kod örneğini yürütmeden önce yapılandırmak için [yapılandırma adımına](#register-your-application)atlayın.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğreticiyi çalıştırmak [için, Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core)veya [Visual Studio 2017](https://www.visualstudio.com/downloads/)ile IIS Express tümleştirmesi gibi yerel bir web sunucusuna ihtiyacınız vardır.

* Bu kılavuzdaki talimatlar, Düğüm.js'de yerleşik bir web sunucusuna dayanmaktadır. [Visual Studio Code'u](https://code.visualstudio.com/download) entegre geliştirme ortamınız (IDE) olarak kullanmanızı öneririz.

## <a name="create-your-project"></a>Projenizi oluşturun

[Node.js](https://nodejs.org/en/download/) yüklü olduğundan emin olun ve ardından uygulamanızı barındıracak bir klasör oluşturun. Burada, dosyanıza `index.html` hizmet etmek için basit bir [Express](https://expressjs.com/) web sunucusu uygulayacağız. 

1. Önce Visual Studio Code tümleşik terminalini kullanarak proje klasörünüzü bulun ve ardından NPM kullanarak Express'i yükleyin.

1. Ardından, .js adlı `server.js`bir dosya oluşturun ve ardından aşağıdaki kodu ekleyin:

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

Artık SPA'nıza hizmet vermek için basit bir sunucunuz var. Bu öğreticinin sonunda amaçlanan klasör yapısı aşağıdaki gibidir:

![amaçlanan SPA klasör yapısının metin tasviri](./media/tutorial-v2-javascript-spa/single-page-application-folder-structure.png)

## <a name="create-the-spa-ui"></a>SPA UI'sini oluştur

1. JavaScript `index.html` SPA için bir dosya oluşturun. Bu dosya **Bootstrap 4 Framework** ile oluşturulmuş bir ui uygular ve yapılandırma, kimlik doğrulama ve API çağrısı için komut dosyası dosyaları içeri aktarıyor.

   Dosyaya `index.html` aşağıdaki kodu ekleyin:

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
   > Önceki komut dosyasındaki MSAL.js sürümünü [MSAL.js sürümleri](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)altında en son yayımlanan sürümü ile değiştirebilirsiniz.
   
2. Şimdi, DOM öğelerine erişecek ve güncelleştirecek bir .js dosyası `ui.js`oluşturun ve aşağıdaki kodu ekleyin:

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

Kimlik doğrulama işlemine devam etmeden önce, uygulamanızı **Azure Etkin Dizini'ne kaydedin.**

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Hesabınız size birden fazla kiracıya erişim sağlıyorsa, sağ üstteki hesabı seçin ve ardından portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft kimlik [platformuna](https://go.microsoft.com/fwlink/?linkid=2083908) gidin Uygulama kayıtları sayfası.
1. **Uygulamayı kaydet** sayfası görüntülendiğinde, uygulamanız için ad girin.
1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin.
1. **URI'yi Yeniden Yönlendirme** bölümünde, açılan listeden **Web** platformunu seçin ve ardından web sunucunuzu temel alan uygulama URL'sine değeri ayarlayın.
1. **Kaydol**’u seçin.
1. Uygulamaya **Genel Bakış** sayfasında, daha sonra kullanmak üzere **Uygulama (istemci) kimlik** değerine dikkat edin.
1. Bu hızlı başlatma, [Örtülü hibe akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir. Kayıtlı uygulamanın sol bölmesinde **Kimlik Doğrulama'yı**seçin.
1. **Gelişmiş ayarlarda**, **Örtülü hibe**altında, **kimlik belirteçleri** ve **Access belirteçleri** onay kutularını seçin. Bu uygulamanın kullanıcıları oturum açması ve API'yi araması gerektiğinden, kimlik belirteçleri ve erişim belirteçleri gereklidir.
1. **Kaydet'i**seçin.

> ### <a name="set-a-redirect-url-for-nodejs"></a>Düğüm.js için yönlendirme URL'si ayarlama
>
> Node.js *için, server.js* dosyasındaki web sunucusu bağlantı noktasını ayarlayabilirsiniz. Bu öğretici port 3000 kullanır, ancak başka bir kullanılabilir bağlantı noktası kullanabilirsiniz.
>
> Uygulama kayıt bilgilerinde yeniden yönlendirme URL'si ayarlamak **için, Uygulama Kaydı** bölmesine geri dön ve aşağıdakilerden birini yapın:
>
> - Yönlendirme *`http://localhost:3000/`* **URL'si**olarak ayarlayın.
> - Özel bir TCP bağlantı noktası kullanıyorsanız, kullanın *`http://localhost:<port>/`* * \<(bağlantı noktası>* özel TCP bağlantı noktası numarasıdır).
>   1. **URL** değerini kopyalayın.
>   1. **Uygulama Kaydı** bölmesine geri dön ve kopyalanan değeri **Yeniden Yönlendirme**URL'si olarak yapıştırın.
>

### <a name="configure-your-javascript-spa"></a>JavaScript SPA'nızı yapılandırın

Kimlik doğrulama için yapılandırma `authConfig.js`parametrelerinizi içeren yeni bir .js dosyası oluşturun ve aşağıdaki kodu ekleyin:

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
 - Enter_the_Application_Id_Here>, kaydettiğiniz uygulamanın **Uygulama (istemci) kimliğidir.** * \<*
 - Enter_the_Cloud_Instance_Id_Here>, Azure bulutunun bir örneğidir. * \<* Ana veya küresel Azure bulutu *https://login.microsoftonline.com*için ' girin. **Ulusal** bulutlar (örneğin, Çin) için [Ulusal bulutlara](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud)bakın.
 - Enter_the_Tenant_info_here>aşağıdaki seçeneklerden birine ayarlanmıştır: * \<*
   - Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu değeri Kiracı Kimliği veya **Kiracı** **adı** ile değiştirin (örneğin, *contoso.microsoft.com).*
   - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları destekliyorsa,* bu değeri **kuruluşlarla**değiştirin.
   - Uygulamanız *herhangi bir kuruluş dizinindeki ve kişisel Microsoft hesaplarındaki hesapları*destekliyorsa, bu değeri **ortak**olanla değiştirin. Desteği yalnızca *kişisel Microsoft hesaplarına*kısıtlamak için, bu değeri **tüketicilerle**değiştirin.


## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Kullanıcıda oturum açabilmek için Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL) kullanın

Kimlik doğrulamanızı ve belirteç edinme mantığınızı içeren yeni bir .js dosyası `authPopup.js`oluşturun ve aşağıdaki kodu ekleyin:

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

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

Bir kullanıcı Oturum **Aç** düğmesini ilk kez `signIn` seçtikten `loginPopup` sonra, yöntem kullanıcıda oturum açmayı çağırır. Bu yöntem, kullanıcının kimlik bilgilerini istem ve doğrulamak için *Microsoft kimlik platformu bitiş noktası* ile bir açılır pencere açar. Başarılı bir oturum açmadan sonra, kullanıcı özgün *index.html* sayfasına geri yönlendirilir. Bir belirteç alınır, `msal.js`işlenir ve belirteçte bulunan bilgiler önbelleğe alınır. Bu belirteç, *kimlik belirteci* olarak bilinir ve kullanıcı görüntü adı gibi kullanıcı hakkında temel bilgileri içerir. Bu belirteç tarafından sağlanan herhangi bir veriyi herhangi bir amaç için kullanmayı planlıyorsanız, belirteçuygulamanız için geçerli bir kullanıcıya verildiğini garanti etmek için bu belirteçin arka uç sunucunuz tarafından doğrulandığınızdan emin olmanız gerekir.

Bu kılavuz tarafından oluşturulan `acquireTokenSilent` SPA, `acquireTokenPopup` kullanıcı profili bilgileri için Microsoft Graph API'yi sorgulamak için kullanılan bir erişim jetonuna sahip olmak için arama lar ve/veya erişim *jetonu* elde etmek için kullanılır. Kimlik belirteci doğrulayan bir örneğe ihtiyacınız varsa, GitHub'daki [bu](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "GitHub active-directory-javascript-singlepageapp-dotnet-webapi-v2 örnek") örnek uygulamaya bir göz atın. Örnek, belirteç doğrulaması için ASP.NET bir Web API kullanır.

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

İlk oturum açmadan sonra, kullanıcılardan kaynağa erişmek için bir belirteç istemeleri gerektiğinde yeniden kimlik doğrulamalarını istemek istemezsiniz. Yani *satınTokenSilent* belirteçleri elde etmek için çoğu zaman kullanılmalıdır. Ancak, kullanıcıları Microsoft kimlik platformu bitiş noktasıyla etkileşime girmeye zorlamanız gereken durumlar vardır. Örneklere şunlar dahildir:

- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmeleri gerekir.
- Uygulamanız bir kaynağa erişim talep ediyor ve kullanıcının onayına ihtiyacınız var.
- İki faktörlü kimlik doğrulama gereklidir.

Arama *acquireTokenPopup* bir açılır pencere açar (veya *satın TokenRedirect* microsoft kimlik platformu bitiş noktasına kullanıcıları yönlendirir). Bu pencerede, kullanıcıların kimlik bilgilerini onaylayarak, gerekli kaynağa onay vererek veya iki faktörlü kimlik doğrulamasını tamamlayarak etkileşimde olmaları gerekir.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntem, `acquireTokenSilent` herhangi bir kullanıcı etkileşimi olmadan belirteç edinimi ve yenilemeyi işler. Sonra `loginPopup` (veya) `loginRedirect`ilk kez yürütülür, `acquireTokenSilent` sonraki aramalar için korumalı kaynaklara erişmek için kullanılan belirteçleri elde etmek için yaygın olarak kullanılan yöntemdir. (Jeton istemek veya yenilemek için çağrılar sessizce yapılır.) `acquireTokenSilent` bazı durumlarda başarısız olabilir. Örneğin, kullanıcının parolasının süresi dolmuş olabilir. Uygulamanız bu özel durumu iki şekilde işleyebilir:

1. `acquireTokenPopup` Hemen bir çağrı yapın, bu da kullanıcı oturum açma istemini tetikler. Bu desen, kullanıcının kullanabileceği uygulamada kimlik doğrulaması olmayan içeriğin bulunmadığı çevrimiçi uygulamalarda yaygın olarak kullanılır. Bu kılavuzlu kurulum tarafından oluşturulan örnek bu deseni kullanır.

1. Uygulamalar ayrıca kullanıcıya etkileşimli oturum açmanın gerekli olduğunu görsel bir gösterge olarak da verebilir, böylece kullanıcı oturum `acquireTokenSilent` açmak için doğru zamanı seçebilir veya uygulama daha sonra yeniden deneyebilir. Bu genellikle, kullanıcı kesintiye uğramadan uygulamanın diğer işlevlerini kullanabildiği zaman kullanılır. Örneğin, uygulamada kimlik doğrulaması olmayan içerik olabilir. Bu durumda, kullanıcı korumalı kaynağa erişmek veya güncelliğini yitirmiş bilgileri yenilemek için ne zaman oturum açmaya karar verebilir.

> [!NOTE]
> Bu hızlı başlatma `loginPopup` `acquireTokenPopup` varsayılan olarak ve yöntemleri kullanır. Tarayıcınız olarak Internet Explorer kullanıyorsanız, Internet `loginRedirect` Explorer'ın açılır pencereleri işleme biçimiyle ilgili bilinen bir `acquireTokenRedirect` [sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) nedeniyle kullanılması ve yöntemleri kullanılması önerilir. Eğer kullanarak `Redirect methods`aynı sonucu elde etmek için nasıl görmek istiyorsanız, [bkz.](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/blob/quickstart/JavaScriptSPA/authRedirect.js) 
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-acquired"></a>Yeni edindiğiniz belirteci kullanarak Microsoft Graph API'yi arayın

1. İlk olarak, REST uç `graphConfig.js`noktalarınızı depolayacak .js adlı bir dosya oluşturun. Aşağıdaki kodu ekleyin:

   ```JavaScript
      const graphConfig = {
        graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me",
        graphMailEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me/messages"
      };
   ```

   Konumlar:
   - Enter_the_Graph_Endpoint_Here>MS Graph API örneğidir. * \<* Genel MS Grafiği API bitiş noktası için `https://graph.microsoft.com`bu dizeyi . Ulusal bulut dağıtımları için lütfen [Grafik API Belgeleri'ne](https://docs.microsoft.com/graph/deployments)bakın.

1. Ardından, Microsoft Graph API'ye REST çağrısı yapacak ve aşağıdaki kodu ekleyecek .js adlı `graph.js`bir dosya oluşturun:

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

<!--start-collapse-->

### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Korunan API'ye karşı REST çağrısı yapma hakkında daha fazla bilgi

Bu kılavuz tarafından oluşturulan örnek `callMSGraph()` uygulamada, yöntem bir `GET` belirteç gerektiren korumalı bir kaynağa karşı bir HTTP isteği yapmak için kullanılır. İstek daha sonra içeriği arayana döndürür. Bu *yöntem, http yetkilendirme üstbilgisinde*edinilen belirteci ekler. Bu kılavuz tarafından oluşturulan örnek uygulama için kaynak, kullanıcının profil bilgilerini görüntüleyen Microsoft Graph API *me* bitiş noktasıdır.

<!--end-collapse-->

## <a name="test-your-code"></a>Kodunuza test etme

1. *Sunucuyu, index.html* dosyanızın konumuna göre bir TCP bağlantı noktasını dinleyecek şekilde yapılandırın. Node.js için, uygulama klasöründen komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek için web sunucusunu başlatın:

   ```bash
   npm install
   npm start
   ```
1. Tarayıcınızda, web **http://localhost:3000** **http://localhost:{port}** sunucunuzun dinlediği *bağlantı noktasının* bulunduğu bağlantı noktasını girin veya girin. *index.html* dosyanızın içeriğini ve Oturum **Aç** düğmesini görmelisiniz.

## <a name="test-your-application"></a>Başvurunuzu test edin

Tarayıcı *index.html* dosyanızı yüklendikten sonra **Oturum Aç'ı**seçin. Microsoft kimlik platformu bitiş noktasıyla oturum açmanız istenir:

![JavaScript SPA hesap oturum açma penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlama

Başvurunuzda ilk oturum açtığınızda, bu uygulamanın profilinize erişme izni vermeniz ve oturum açmanız istenir:

!["İstenen İzinler" penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüleme

Oturum açmanızdan sonra, kullanıcı profili bilgileriniz görüntülenen Microsoft Graph API yanıtında döndürülür:

![Microsoft Graph API aramasının sonuçları](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API' si, kullanıcının profilini okuması için *kullanıcının okuma* kapsamını gerektirir. Varsayılan olarak, bu kapsam kayıt portalına kayıtlı olan her uygulamaya otomatik olarak eklenir. Microsoft Graph için diğer API'lerin yanı sıra arka uç sunucunuz için özel API'ler ek kapsamlar gerektirebilir. Örneğin, Microsoft Graph API kullanıcının postalarını listelemek için *Mail.Read* kapsamını gerektirir.

> [!NOTE]
> Kapsam sayısını artırdıkça kullanıcıdan ek izinler istenebilir.

Arka uç API'si kapsam gerektirmiyorsa (önerilmez), belirteçleri elde etmek için aramalarda *istemciyi* kapsam olarak kullanabilirsiniz.

<!--end-collapse-->

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
