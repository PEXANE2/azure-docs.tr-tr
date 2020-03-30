---
title: Oturum Yönetimi - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
description: Tehdit Modelleme Aracı'nda açığa çıkan tehditler için azaltıcı etkenler
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 5d9dc1595e3cc812ba060d958b6e981867500ae2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73161501"
---
# <a name="security-frame-session-management"></a>Güvenlik Çerçevesi: Oturum Yönetimi
| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[Azure AD kullanırken ADAL yöntemlerini kullanarak uygun oturumu uygulama](#logout-adal)</li></ul> |
| IoT Cihazı | <ul><li>[Oluşturulan SaS belirteçleri için sonlu kullanım ömürleri kullanma](#finite-tokens)</li></ul> |
| **Azure Belge DB** | <ul><li>[Oluşturulan Kaynak belirteçleri için minimum belirteç ömürleri kullanma](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[ADFS kullanırken WsFederation yöntemlerini kullanarak uygun oturumu uygulama](#wsfederation-logout)</li></ul> |
| **Kimlik Sunucusu** | <ul><li>[Identity Server'ı kullanırken uygun oturumu uygulama](#proper-logout)</li></ul> |
| **Web Uygulaması** | <ul><li>[HTTPS üzerinden kullanılabilen uygulamalar güvenli tanımlama bilgileri kullanmalıdır](#https-secure-cookies)</li><li>[Tüm http tabanlı uygulama sadece çerez tanımı için http belirtmelidir](#cookie-definition)</li><li>[ASP.NET web sayfalarında KiSTe'de Arama (CSRF) saldırılarına karşı azaltma](#csrf-asp)</li><li>[Hareketsizlik süresi için oturum ayarlama](#inactivity-lifetime)</li><li>[Uygulamadan uygun oturumu uygulama](#proper-app-logout)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API'lerine yönelik Site Ötesi İstek Sahteciliği (CSRF) saldırılarına karşı azaltma](#csrf-api)</li></ul> |

## <a name="implement-proper-logout-using-adal-methods-when-using-azure-ad"></a><a id="logout-adal"></a>Azure AD kullanırken ADAL yöntemlerini kullanarak uygun oturumu uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Uygulama Azure AD tarafından verilen erişim jetonuna dayanıyorsa, oturum açma olay işleyicisi |

### <a name="example"></a>Örnek
```csharp
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Örnek
Ayrıca Session.Abandon() yöntemini arayarak kullanıcının oturumunu yok etmelidir. Aşağıdaki yöntem, kullanıcı oturumunun güvenli bir şekilde uygulanmasını gösterir:
```csharp
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <a name="use-finite-lifetimes-for-generated-sas-tokens"></a><a id="finite-tokens"></a>Oluşturulan SaS belirteçleri için sonlu kullanım ömürleri kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Azure IoT Hub'ına kimlik doğrulaması için oluşturulan SaS belirteçlerinin sonlu bir son kullanma süresi olmalıdır. Belirteçlerin tehlikeye girmeleri durumunda yeniden oynatılabilir zaman miktarını sınırlamak için SaS belirteç ömürlerini en az olarak tutun.|

## <a name="use-minimum-token-lifetimes-for-generated-resource-tokens"></a><a id="resource-tokens"></a>Oluşturulan Kaynak belirteçleri için minimum belirteç ömürleri kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Belge DB | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Kaynak belirteci zaman lığını gereken minimum değere indirin. Kaynak belirteçleri varsayılan geçerli zaman ası 1 saat vardır.|

## <a name="implement-proper-logout-using-wsfederation-methods-when-using-adfs"></a><a id="wsfederation-logout"></a>ADFS kullanırken WsFederation yöntemlerini kullanarak uygun oturumu uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | ADFS | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Uygulama ADFS tarafından verilen STS belirtecine dayanıyorsa, oturum açma olay işleyicisi kullanıcıyı oturumu açmak için WSFederationAuthenticationModule.FedereSignOut() yöntemini aramalıdır. Ayrıca geçerli oturum yok edilmeli ve oturum belirteç değeri sıfırlanıp geçersiz kılınmalıdır.|

### <a name="example"></a>Örnek
```csharp
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes the user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at the specified security token service (STS) by using the WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of the current session and raises the appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at the specified security token service (STS) by using the WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <a name="implement-proper-logout-when-using-identity-server"></a><a id="proper-logout"></a>Identity Server'ı kullanırken uygun oturumu uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik Sunucusu | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [IdentityServer3-Federe oturum açma](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Adımlar** | IdentityServer, harici kimlik sağlayıcılarla fetorate etme yeteneğini destekler. Bir kullanıcı, kullanılan protokole bağlı olarak, bir yukarı kimlik sağlayıcısından çıkış yaptığında, kullanıcı çıkış yaptığında bir bildirim almak mümkün olabilir. IdentityServer'ın istemcilerini bilgilendirmesine olanak tanır, böylece kullanıcıyı da oturum açabilirler. Uygulama ayrıntıları için başvurular bölümündeki belgeleri denetleyin.|

## <a name="applications-available-over-https-must-use-secure-cookies"></a><a id="https-secure-cookies"></a>HTTPS üzerinden kullanılabilen uygulamalar güvenli tanımlama bilgileri kullanmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvurular**              | [httpCookies Element (ASP.NET Ayarları Şema)](https://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure Özellik](https://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Adımlar** | Tanımlama bilgileri normalde yalnızca kapsama alındıkları etki alanı için erişilebilir. Ne yazık ki, "etki alanı" tanımı protokolü içermez, bu nedenle HTTPS üzerinden oluşturulan tanımlama bilgilerine HTTP üzerinden erişilebilir. "Güvenli" özniteliği tarayıcıya çerezin yalnızca HTTPS üzerinden kullanılabilir hale getirilmesi gerektiğini gösterir. HTTPS üzerinden ayarlanan tüm tanımlama bilgilerinin **güvenli** özniteliği kullandığından emin olun. Gereksinim, gereksinimi doğru olana gerekliSSL özniteliği ayarlayarak web.config dosyasında zorlanabilir. Tercih edilen yaklaşımdır, çünkü ek kod değişiklikleri yapmaya gerek kalmadan tüm geçerli ve gelecekteki tanımlama bilgileri için **güvenli** özniteliği uygular.|

### <a name="example"></a>Örnek
```csharp
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
UYGULAMAERIŞMEK İçİn HTTP kullanılsa bile ayar uygulanır. Uygulamaya erişmek için HTTP kullanılıyorsa, tanımlama bilgileri güvenli öznitelik ile ayarlandığı ve tarayıcı nın bunları uygulamaya geri göndermemesi nedeniyle ayarı uygulamayı bozar.

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Web Formları, MVC5 |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvurular**              | Yok  |
| **Adımlar** | Web uygulaması Güvenen Taraf ve IdP ADFS sunucusu olduğunda, FedAuth belirteci'nin güvenli özniteliği web.config `system.identityModel.services` bölümünde True sssl gerektirir ayarlayarak yapılandırılabilir:|

### <a name="example"></a>Örnek
```csharp
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <a name="all-http-based-application-should-specify-http-only-for-cookie-definition"></a><a id="cookie-definition"></a>Tüm http tabanlı uygulama sadece çerez tanımı için http belirtmelidir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Güvenli Çerez Özniteliği](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Adımlar** | Bir çapraz site komut dosyası (XSS) saldırısı ile bilgi açıklama riskini azaltmak için, yeni bir öznitelik - httpOnly - çerezleri tanıtıldı ve tüm büyük tarayıcılar tarafından desteklenir. Öznitelik, çerezkomut dosyası aracılığıyla erişilemeyeceğini belirtir. Bir web uygulaması, HttpOnly çerezleri kullanarak, çerezde bulunan hassas bilgilerin komut dosyası aracılığıyla çalınarak saldırganın web sitesine gönderilme olasılığını azaltır. |

### <a name="example"></a>Örnek
Çerezleri kullanan tüm HTTP tabanlı uygulamalar, web.config'de aşağıdaki yapılandırmayı uygulayarak HttpOnly'i tanımlama bilgisi tanımında belirtmelidir:
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Web Forms |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [FormsAuthentication.RequireSSL Özellik](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Adımlar** | RequireSSL özellik değeri yapılandırma öğesinin requireSSL özniteliği kullanılarak ASP.NET bir uygulama için yapılandırma dosyasında ayarlanır. ASP.NET uygulamanız için Web.config dosyasında, form kimlik doğrulama çerezinin gerekliSSSL özniteliğini ayarlayarak sunucuya döndürmek için SSL (Secure Sockets Layer) gerekip gerekmediğini belirtebilirsiniz.|

### <a name="example"></a>Örnek 
Aşağıdaki kod örneği, Web.config dosyasındaki gereksinimliSSL özniteliğini ayarlar.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC5 |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvurular**              | [Windows Identity Foundation (WIF) Yapılandırması – Bölüm II](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Adımlar** | FedAuth çerezleri için httpOnly özniteliğiayarlamak için hideFromCsript öznitelik değeri True olarak ayarlanmalıdır. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma doğru yapılandırmayı gösterir:
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <a name="mitigate-against-cross-site-request-forgery-csrf-attacks-on-aspnet-web-pages"></a><a id="csrf-asp"></a>ASP.NET web sayfalarında KiSTe'de Arama (CSRF) saldırılarına karşı azaltma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Site içi istek sahteciliği (CSRF veya XSRF), saldırganın farklı bir kullanıcının web sitesindeki yerleşik oturumunun güvenlik bağlamında eylem gerçekleştirebileceği bir saldırı türüdür. Hedeflenen web sitesi alınan isteği doğrulamak için yalnızca oturum tanımlama bilgilerine dayanıyorsa, içeriği değiştirmek veya silmektir. Saldırgan, farklı bir kullanıcının tarayıcısının, kullanıcının zaten oturum açtığı savunmasız bir siteden gelen komutla bir URL yüklemesini sağlayarak bu güvenlik açığından yararlanabilir. Bir saldırganın bunu yapması, örneğin, bir kaynağı savunmasız sunucudan yükleyen farklı bir web sitesini barındırmak veya kullanıcının bir bağlantıyı tıklatmasını sağlamak gibi birçok yol vardır. Sunucu istemciye ek bir belirteç gönderirse, istemcinin gelecekteki tüm isteklere bu belirteci eklemesini gerektiriyorsa ve gelecekteki tüm isteklerin geçerli oturumla ilgili bir belirteç içerdiğini doğrularsa, saldırı önlenebilirASP.NET AntiForgeryToken veya ViewState. |

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET MVC ve Web Sayfalarında XSRF/CSRF Önleme](https://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Adımlar** | Anti-CSRF ve ASP.NET MVC `AntiForgeryToken` formları - Görünümler üzerinde yardımcı yöntemi kullanın; forma `Html.AntiForgeryToken()` bir koymak, örneğin,|

### <a name="example"></a>Örnek
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Örnek
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Örnek
Aynı zamanda, Html.AntiForgeryToken() ziyaretçiye yukarıda gösterilen rasgele gizli değerle aynı değere sahip __RequestVerificationToken adlı bir çerez verir. Ardından, gelen bir form gönderisini doğrulamak için hedef eylem yöntemine [ValidateAntiForgeryToken] filtresini ekleyin. Örnek:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Bunu denetleyen yetkilendirme filtresi:
* Gelen istek __RequestVerificationToken adlı bir çerez vardır
* Gelen isteğin __RequestVerificationToken `Request.Form` adlı bir girişi vardır
* Bu çerez `Request.Form` ve değerler tüm iyi olduğunu varsayarak eşleşir, istek normal olarak geçer. Ama değilse, o zaman iletisi ile bir yetkilendirme hatası "Gerekli bir anti-sahtecilik belirteci sağlanmadı veya geçersiz oldu". 

### <a name="example"></a>Örnek
Anti-CSRF ve AJAX: Bir AJAX isteği HTML form verileri değil, JSON verileri gönderebilir, çünkü form belirteci AJAX istekleri için bir sorun olabilir. Bir çözüm özel bir HTTP üstbilgi belirteçleri göndermektir. Aşağıdaki kod belirteçleri oluşturmak için Razor sözdizimini kullanır ve sonra bir AJAX isteğine belirteçleri ekler. 
```csharp
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Örnek
İsteği işlediğinizde, belirteçleri istek üstbilgisinden ayıklayın. Ardından belirteçleri doğrulamak için AntiForgery.Validate yöntemini arayın. Belirteçleri geçerli değilse Doğrulama yöntemi bir özel durum atar.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Web Forms |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Web Saldırılarını Savuşturmak için ASP.NET Yerleşik Özelliklerden Yararlanın](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Adımlar** | WebForm tabanlı uygulamalardaki CSRF saldırıları, ViewStateUserKey'i her kullanıcı için değişen rasgele bir dize -kullanıcı kimliği veya daha iyisi, oturum kimliği- ayarlayarak azaltılabilir. Bir dizi teknik ve sosyal nedenden dolayı oturum kimliği çok daha uygundur, çünkü oturum kimliği tahmin edilemez, zaman dışında dır ve kullanıcı başına değişir.|

### <a name="example"></a>Örnek
Tüm sayfalarınızda olması gereken kod şunlardır:
```csharp
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a name="set-up-session-for-inactivity-lifetime"></a><a id="inactivity-lifetime"></a>Hareketsizlik süresi için oturum ayarlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [httpSessionState.Timeout Özelliği](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Adımlar** | Oturum zaman anına, kullanıcı bir süre boyunca bir web sitesinde herhangi bir eylem gerçekleştirmediğinde (web sunucusu tarafından tanımlanan) meydana gelen olayı temsil eder. Sunucu tarafında ki olay, kullanıcı oturumunun durumunu 'geçersiz' olarak değiştirir (örneğin "artık kullanılmaz") ve web sunucusuna onu yok etmesini (içerdiği tüm verileri silme) talimat verir. Aşağıdaki kod örneği, zaman ekme oturumu özniteliğini Web.config dosyasındaki 15 dakika olarak ayarlar.|

### <a name="example"></a>Örnek
```XML 
<configuration>
  <system.web>
    <sessionState mode="InProc" cookieless="true" timeout="15" />
  </system.web>
</configuration>
```

## <a name="enable-threat-detection-on-azure-sql"></a><a id="threat-detection"></a>Azure SQL'de Tehdit algılamayı etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Web Forms |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Kimlik doğrulama için formlar öğesi (ASP.NET Ayarlar Şeması)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Adımlar** | Formkimlik Doğrulama Bileti çerez zaman lamasını 15 dakikaya ayarlama|

### <a name="example"></a>Örnek
```XML
<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Web Formları, MVC5 |
| **Öznitelikler**              | EnvironmentType - OnPrem |
| **Başvurular**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Adımlar** | Web uygulaması Relying Party ve ADFS STS olduğunda, kimlik doğrulama çerezleri ömür boyu - FedAuth belirteçleri - web.config aşağıdaki yapılandırma tarafından ayarlanabilir:|

### <a name="example"></a>Örnek
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use the code below to enable encryption-decryption of claims received from ADFS. Thumbprint value varies based on the certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a>Örnek
Ayrıca ADFS saml iddia belirteç ömrü 15 dakika olarak ayarlanmalıdır, ADFS sunucusunda aşağıdaki powershell komutu çalıştırılarak yayınladı:
```csharp
Set-ADFSRelyingPartyTrust -TargetName "<RelyingPartyWebApp>" -ClaimsProviderName @("Active Directory") -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a name="implement-proper-logout-from-the-application"></a><a id="proper-app-logout"></a>Uygulamadan uygun oturumu uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Kullanıcı oturumu açma düğmesine bastığında uygulamadan düzgün Çıkış Yap. Oturum açma sırasında uygulama, kullanıcının oturumunu yok etmeli ve oturum çerez değerini sıfırlamalı ve geçersiz kılar. Ayrıca, birden çok oturum tek bir kullanıcı kimliğine bağlı olduğunda, zaman anına veya oturum adadığında sunucu tarafında topluca sonlandırılmalıdır. Son olarak, Oturum Açma işlevinin her sayfada kullanılabilir olduğundan emin olun. |

## <a name="mitigate-against-cross-site-request-forgery-csrf-attacks-on-aspnet-web-apis"></a><a id="csrf-api"></a>ASP.NET Web API'lerine yönelik Site Ötesi İstek Sahteciliği (CSRF) saldırılarına karşı azaltma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Site içi istek sahteciliği (CSRF veya XSRF), saldırganın farklı bir kullanıcının web sitesindeki yerleşik oturumunun güvenlik bağlamında eylem gerçekleştirebileceği bir saldırı türüdür. Hedeflenen web sitesi alınan isteği doğrulamak için yalnızca oturum tanımlama bilgilerine dayanıyorsa, içeriği değiştirmek veya silmektir. Saldırgan, farklı bir kullanıcının tarayıcısının, kullanıcının zaten oturum açtığı savunmasız bir siteden gelen komutla bir URL yüklemesini sağlayarak bu güvenlik açığından yararlanabilir. Bir saldırganın bunu yapması, örneğin, bir kaynağı savunmasız sunucudan yükleyen farklı bir web sitesini barındırmak veya kullanıcının bir bağlantıyı tıklatmasını sağlamak gibi birçok yol vardır. Sunucu istemciye ek bir belirteç gönderirse, istemcinin gelecekteki tüm isteklere bu belirteci eklemesini gerektiriyorsa ve gelecekteki tüm isteklerin geçerli oturumla ilgili bir belirteç içerdiğini doğrularsa, saldırı önlenebilirASP.NET AntiForgeryToken veya ViewState. |

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET Web API'sinde Siteler Arası İstek Sahteciliği (CSRF) Saldırılarını Önleme](https://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Adımlar** | Anti-CSRF ve AJAX: Bir AJAX isteği HTML form verileri değil, JSON verileri gönderebilir, çünkü form belirteci AJAX istekleri için bir sorun olabilir. Bir çözüm özel bir HTTP üstbilgi belirteçleri göndermektir. Aşağıdaki kod belirteçleri oluşturmak için Razor sözdizimini kullanır ve sonra bir AJAX isteğine belirteçleri ekler. |

### <a name="example"></a>Örnek
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Örnek
İsteği işlediğinizde, belirteçleri istek üstbilgisinden ayıklayın. Ardından belirteçleri doğrulamak için AntiForgery.Validate yöntemini arayın. Belirteçleri geçerli değilse Doğrulama yöntemi bir özel durum atar.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a>Örnek
Anti-CSRF ve ASP.NET MVC formları - Görünümlerde AntiForgeryToken yardımcı yöntemini kullanın; forma bir Html.AntiForgeryToken() koymak, örneğin,
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Örnek
Yukarıdaki örnek aşağıdaki gibi bir şey çıktı olacaktır:
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Örnek
Aynı zamanda, Html.AntiForgeryToken() ziyaretçiye yukarıda gösterilen rasgele gizli değerle aynı değere sahip __RequestVerificationToken adlı bir çerez verir. Ardından, gelen bir form gönderisini doğrulamak için hedef eylem yöntemine [ValidateAntiForgeryToken] filtresini ekleyin. Örnek:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Bunu denetleyen yetkilendirme filtresi:
* Gelen istek __RequestVerificationToken adlı bir çerez vardır
* Gelen isteğin __RequestVerificationToken `Request.Form` adlı bir girişi vardır
* Bu çerez `Request.Form` ve değerler tüm iyi olduğunu varsayarak eşleşir, istek normal olarak geçer. Ama değilse, o zaman iletisi ile bir yetkilendirme hatası "Gerekli bir anti-sahtecilik belirteci sağlanmadı veya geçersiz oldu".

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Kimlik Sağlayıcısı - ADFS, Kimlik Sağlayıcısı - Azure AD |
| **Başvurular**              | [Web API'ASP.NET Bireysel Hesaplar ve Yerel Giriş ile Web API'ASP.NET 2.2](https://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Adımlar** | Web API OAuth 2.0 kullanılarak güvenli ise, yetkilendirme isteği üstbilgisinde bir taşıyıcı belirteci bekler ve yalnızca belirteç geçerliyse isteğe erişim sağlar. Çerez tabanlı kimlik doğrulamasının aksine, tarayıcılar taşıyıcı belirteçlerini isteklere eklemez. İstenen istemcinin, taşıyıcı belirteci'ni istek üstbilgisine açıkça eklemesi gerekir. Bu nedenle, OAuth 2.0 kullanılarak korunan ASP.NET Web API'leri için taşıyıcı belirteçleri CSRF saldırılarına karşı bir savunma olarak kabul edilir. Uygulamanın MVC bölümünde kimlik doğrulama formları kullanılıyorsa (örneğin, tanımlama bilgileri kullanıyorsa), sahteciliğin önleyici belirteçlerinin MVC web uygulaması tarafından kullanılması gerektiğini lütfen unutmayın. |

### <a name="example"></a>Örnek
Web API'si, tanımlama bilgilerine değil, yalnızca taşıyıcı belirteçlerine güvenmek için bilgilendirilmelidir. Yöntemde `WebApiConfig.Register` aşağıdaki yapılandırma ile yapılabilir:

```csharp
config.SuppressDefaultHostAuthentication();
config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```

SuppressDefaultHostAuthentication yöntemi Web API istek IIS veya OWIN ara tarafından Web API ardışık ulaşmadan önce olur herhangi bir kimlik doğrulama yoksaymak için söyler. Bu şekilde, Web API'sini yalnızca taşıyıcı belirteçlerini kullanarak kimlik doğrulaması yla sınırlandırabiliriz.
