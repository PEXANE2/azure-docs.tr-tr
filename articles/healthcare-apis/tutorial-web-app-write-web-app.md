---
title: Web uygulaması öğreticisi-Web uygulamasını yazma
description: Bu öğreticide basit bir Web uygulaması dağıtma örneği gösterilmektedir. Öğreticinin bu bölümü, Web uygulamasının yazılmasına yol gösterir.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.openlocfilehash: bb9c206a17a11f0cf710ac4ee3ac7ccae0fa9eb9
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87848035"
---
# <a name="write-azure-web-application-to-read-fhir-data"></a>FHıR verilerini okumak için Azure Web uygulaması yazma
Artık FHıR sunucunuza bağlanabildiğinizi ve veri GÖNDERIYOR olduğunuza göre, FHıR verilerini okuyacak bir Web uygulaması yazmaya hazırsınız. Öğreticinin bu son adımında, Web uygulamasına yazma ve erişme konusunda adım adım göstereceğiz.

## <a name="create-web-application"></a>Web uygulaması oluştur
Azure 'da **kaynak oluştur** ' u seçin ve **Web uygulaması**' nı seçin. Web uygulamanızı istemci uygulamanızın yeniden yönlendirme URI 'sinde belirtdiklerinizi girdiğinizden emin olun veya geri dönüp yeniden yönlendirme URI 'sini yeni adla güncelleştirin. 

![Web uygulaması oluştur](media/tutorial-web-app/create-web-app.png)

Web uygulaması kullanılabilir olduğunda **kaynağa gidin**. Sağdaki geliştirme araçları altında **App Service Düzenleyicisi (Önizleme)** öğesini seçin ve ardından **Git**' i seçin. Git ' i seçtiğinizde App Service Düzenleyicisi açılır. *Keşfet* ' in altındaki gri alanına sağ tıklayın ve **index.html**adlı yeni bir dosya oluşturun.

Aşağıda **index.html**'ye giriş yapabilirsiniz. Aşağıdaki öğeleri güncelleştirmeniz gerekecektir:
* **ClientID** -ISTEMCI uygulama Kimliğinizle güncelleştirin. Bu KIMLIK, belirtecinizi alırken çekilecek KIMLIK ile aynı olacak
* **yetkili** -Azure AD kiracı Kimliğinizle güncelleştirme
* **Fhirendpoint** -fmaendpoint hizmetini fhır hizmeti adı olacak şekilde güncelleştirin
* **kapsamlar** -izleyicilerinizin tam URL 'sini yansıtacak şekilde güncelleştirin

``` HTML

<!DOCTYPE html>
<html>

<head>
    <title>FHIR Patient browser sample app</title>
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
</head>

<body>
    <div class="leftContainer">
        <p id="WelcomeMessage">Welcome to the FHIR Patient browsing sample Application</p>
        <button id="SignIn" onclick="signIn()">Sign In</button>
    </div>

    <div id="patientTable">
    </div>

    <script>
        var msalConfig = {
            auth: {
                clientId: '<CLIENT-ID>',
                authority: "https://login.microsoftonline.com/<AZURE-AD-TENANT-ID>"
            },
            cache: {
                cacheLocation: "localStorage",
                storeAuthStateInCookie: true
            }
        }

        var FHIRConfig = {
            FHIRendpoint: "https://<FHIR-SERVER-NAME>.azurehealthcareapis.com"
        }
        var requestObj = {
            scopes: ["https://<FHIR-SERVER-NAME>.azurehealthcareapis.com/user_impersonation"]
        }

        function authRedirectCallBack(error, response) {
            if (error) {
                console.log(error);
            } else {
                if (response.tokenType === "access_token") {
                    callFHIRServer(FHIRConfig.FHIRendpoint + '/Patient', 'GET', null, response.accessToken, FHIRCallback);
                }
            }
        }

        var myMSALObj = new Msal.UserAgentApplication(msalConfig);
        myMSALObj.handleRedirectCallback(authRedirectCallBack);

        function signIn() {
            myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
                showWelcomeMessage();
                acquireTokenPopupAndCallFHIRServer();
            }).catch(function (error) {
                console.log(error);
            })
        }

        function showWelcomeMessage() {
            var divWelcome = document.getElementById('WelcomeMessage');
            divWelcome.innerHTML = "Welcome " + myMSALObj.getAccount().userName + " to FHIR Patient Browsing App";
            var loginbutton = document.getElementById('SignIn');
            loginbutton.innerHTML = 'Sign Out';
            loginbutton.setAttribute('onclick', 'signOut()')
        }

        function signOut() {
            myMSALObj.logout();
        }

        function acquireTokenPopupAndCallFHIRServer() {
            myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
                callFHIRServer(FHIRConfig.FHIRendpoint + '/Patient', 'GET', null, tokenResponse.accessToken, FHIRCallback);
            }).catch(function (error) {
                console.log(error);
                if (requiresInteraction(error.errorCode)) {
                    myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
                        callFHIRServer(FHIRConfig.FHIRendpoint + '/Patient', 'GET', null, tokenResponse.accessToken, FHIRCallback);
                    }).catch(function (error) {
                        console.log(error);
                    })
                }
            });
        }

        function callFHIRServer(theUrl, method, message, accessToken, callBack) {
            var xmlHttp = new XMLHttpRequest();
            xmlHttp.onreadystatechange = function () {
                if (this.readyState == 4 && this.status == 200)
                    callBack(JSON.parse(this.responseText));
            }
            xmlHttp.open(method, theUrl, true);
            xmlHttp.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
            xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
            xmlHttp.send(message);
        }

        function FHIRCallback(data) {
            patientListHtml = '<ol>';
            data.entry.forEach(function(e) {
                patientListHtml += '<li>' + e.resource.name[0].family + ', ' + e.resource.name[0].given + ' (' + e.resource.id + ')';
            });
            patientListHtml += '</ol>';
            document.getElementById("patientTable").innerHTML = patientListHtml;
        }
    </script>
</body>

</html>
```

Buradan, Web uygulaması kaynağınız için geri dönüp Genel Bakış sayfasında bulunan URL 'YI açabilirsiniz. Daha önce oluşturduğunuz hasta James Tibertal Kirk 'ı görmek için oturum açın.

## <a name="next-steps"></a>Sonraki Adımlar
FHıR için Azure API 'SI başarıyla dağıtıldı, bir ortak istemci uygulaması kaydedildi, test edildi ve küçük bir Web uygulaması oluşturdunuz. Sonraki adım olarak desteklenen FHIR özellikleri için Azure API 'sine göz atın.

>[!div class="nextstepaction"]
>[Desteklenen özellikler](fhir-features-supported.md)





