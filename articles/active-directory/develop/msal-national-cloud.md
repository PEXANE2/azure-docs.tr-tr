---
title: MSAL'ı ulusal bir bulut uygulamasında kullanın | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı (MSAL), uygulama geliştiricilerin güvenli web API'lerini aramak için belirteçler edinmelerini sağlar. Bu web API'leri Microsoft Graph, diğer Microsoft API'leri, iş ortağı web API'leri veya kendi web API'niz olabilir. MSAL birden çok uygulama mimarisini ve platformlarını destekler.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/22/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: dfca2b1311f1b55f19d5709f7c9ca7c3e366769c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76695747"
---
# <a name="use-msal-in-a-national-cloud-environment"></a>MSAL'ı ulusal bulut ortamında kullanın

Sovereign bulutları olarak da bilinen [ulusal bulutlar,](authentication-national-cloud.md)Azure'un fiziksel olarak yalıtılmış örnekleridir. Azure'un bu bölgeleri, veri ikamet, egemenlik ve uyumluluk gereksinimlerinin coğrafi sınırlar içinde yerine getirilmesini sağlamaya yardımcı olur.

Microsoft dünya çapındaki buluta ek olarak, Microsoft Kimlik Doğrulama Kitaplığı (MSAL), ulusal bulutlardaki uygulama geliştiricilerin güvenli web API'lerinin kimliğini doğrulamak ve aramak için belirteçler edinmelerini sağlar. Bu web API'leri Microsoft Graph veya diğer Microsoft API'leri olabilir.

Genel bulut da dahil olmak üzere, Azure Etkin Dizin (Azure AD) aşağıdaki ulusal bulutlarda dağıtılır:  

- Azure Kamu
- Azure Çin 21Vianet
- Azure Almanya

Bu kılavuz, çalışma ve okul hesaplarında nasıl oturum açılabildiğini, erişim jetonunu nasıl alacağımı ve [Azure Kamu bulut](https://azure.microsoft.com/global-infrastructure/government/) ortamında Microsoft Graph API'yi nasıl arayacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, bu ön koşulları yerine getirin emin olun.

### <a name="choose-the-appropriate-identities"></a>Uygun kimlikleri seçin

[Azure Kamu](https://docs.microsoft.com/azure/azure-government/) uygulamaları, kullanıcıların kimliğini doğrulamak için Azure AD Genel kimliklerini ve Azure AD Genel kimliklerini kullanabilir. Bu kimliklerden herhangi birini kullanabileceğinizden, senaryonuz için hangi yetki bitiş noktasını seçmeniz gerektiğine karar vermeniz gerekir:

- Azure AD Genel: Kuruluşunuzun Office 365 'i (Genel veya GCC) veya başka bir uygulamayı desteklemek için zaten bir Azure REKLAM Genel kiracısı varsa genellikle kullanılır.
- Azure REKLAM Yönetimi: Kuruluşunuzun Office 365'i (GCC High veya DoD) desteklemek için zaten bir Azure AD Hükümeti kiracısı varsa veya Azure AD Yönetimi'nde yeni bir kiracı oluşturuyorsa yaygın olarak kullanılır.

Karar vermenizin ardından, uygulama kaydınızı nerede gerçekleştirdiğinize özel bir husus dikkat edin. Azure Genel uygulamanız için Azure AD Genel kimliklerini seçerseniz, uygulamayı Azure AD Genel kiracınıza kaydetmeniz gerekir.

### <a name="get-an-azure-government-subscription"></a>Azure Devlet aboneliği alın

Azure Genel aboneliği almak için Azure [Genel'de Aboneliğinizi Yönetme ve aboneliğinize bağlanma 'ya](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions)bakın.

Azure Devlet aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/global-infrastructure/government/request/) oluşturun.

Belirli bir programlama diliyle ulusal bulut kullanma hakkında ayrıntılı bilgi için, dilinizle eşleşen sekmeyi seçin:

## <a name="net"></a>[.NET](#tab/donet)

MSAL.NET kullanıcıları oturum açma, belirteçler edinme ve ulusal bulutlarda Microsoft Graph API'yi aramak için kullanabilirsiniz.

Aşağıdaki öğreticiler bir .NET Core 2.2 MVC Web uygulamasının nasıl geliştirilebildiğini göstermektedir. Uygulama, ulusal bir buluta ait bir kuruluşta iş ve okul hesabı olan kullanıcıları oturum açmak için OpenID Connect'i kullanır.

- Kullanıcıları oturum altına almak ve belirteçleri edinmek için şu öğreticiye uyun: [Microsoft kimlik platformuyla bağımsız bulutlarda oturum açma kullanıcıları ASP.NET bir Çekirdek Web uygulaması oluşturun.](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform)
- Microsoft Graph API'yi aramak için şu öğreticiyi izleyin: Microsoft Ulusal [Bulut'taki çalışmalarını ve okul hesabını kullanan bir kullanıcı adına, Bir ASP.NET Core 2.x Web Uygulamasından Microsoft Graph API'sini aramak](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud)için Microsoft kimlik platformunun kullanılması.

## <a name="javascript"></a>[Javascript](#tab/javascript)

Egemen bulutlar için MSAL.js uygulamanızı etkinleştirmek için:

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

1. [Azure portalında](https://portal.azure.us/)oturum açın.
    
   Diğer ulusal bulutlar için Azure portalı bitiş noktalarını bulmak için [Uygulama kayıt bitiş noktalarına](authentication-national-cloud.md#app-registration-endpoints)bakın.

1. Hesabınız size birden fazla kiracıya erişim sağlıyorsa, sağ üst köşedeki hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft kimlik platformundaki [Uygulama kayıtları](https://aka.ms/ra/ff) sayfasına gidin.
1. **Uygulamayı kaydet** sayfası görüntülendiğinde, uygulamanız için ad girin.
1. **Desteklenen hesap türleri**altında, herhangi bir kuruluş **dizininde Hesapları**seçin.
1. **URI'yi Yeniden Yönlendirme** **bölümünde, Web** platformunu seçin ve web sunucunuza göre uygulamanın URL'sine değeri ayarlayın. Visual Studio ve Düğüm'de yönlendirme URL'sini nasıl ayarlayıp alacağınızı anlatan talimatlar için sonraki bölümlere bakın.
1. **Kaydol**’u seçin.
1. Uygulamaya **Genel Bakış** sayfasında, **Uygulama (istemci) kimlik** değerini not edin.
1. Bu [öğretici, örtülü hibe akışını](v2-oauth2-implicit-grant-flow.md)etkinleştirmenizi gerektirir. Kayıtlı uygulamanın sol bölmesinde **Kimlik Doğrulama'yı**seçin.
1. **Gelişmiş ayarlarda**, **Örtülü hibe**altında, **kimlik belirteçleri** ve **Access belirteçleri** onay kutularını seçin. Bu uygulamanın kullanıcıları oturum açması ve API'yi araması gerektiğinden, kimlik belirteçleri ve erişim belirteçleri gereklidir.
1. **Kaydet'i**seçin.

### <a name="step-2--set-up-your-web-server-or-project"></a>Adım 2: Web sunucunuzu veya projenizi ayarlama

- Düğüm gibi yerel bir web sunucusunun [proje dosyalarını karşıdan yükleyin.](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)

  or

- [Visual Studio projesini indirin.](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Ardından, kod örneğini çalıştırmadan önce yapılandırmak için [JavaScript SPA'nızı yapılandırmayı](#step-4-configure-your-javascript-spa) atlayın.

### <a name="step-3-use-the-microsoft-authentication-library-to-sign-in-the-user"></a>Adım 3: Kullanıcıda oturum açabilmek için Microsoft Kimlik Doğrulama Kitaplığını kullanın

Projenizi oluşturmak ve kullanıcıda oturum açmanız için MSAL ile tümleştirmek için [JavaScript öğreticisindeki](tutorial-v2-javascript-spa.md#create-your-project) adımları izleyin.

### <a name="step-4-configure-your-javascript-spa"></a>Adım 4: JavaScript SPA'nızı yapılandırın

Proje `index.html` kurulumu sırasında oluşturulan dosyaya, uygulama kayıt bilgilerini ekleyin. Dosyanızın `<script></script>` `index.html` gövdesindeki etiketlerin en üstüne aşağıdaki kodu ekleyin:

```javascript
const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here",
        }
}

const graphConfig = {
        graphEndpoint: "https://graph.microsoft.us",
        graphScopes: ["user.read"],
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

Bu kodda:

- `Enter_the_Application_Id_here`kaydettiğiniz uygulamanın **Uygulama (istemci) kimlik** değeridir.
- `Enter_the_Tenant_Info_Here`aşağıdaki seçeneklerden birine ayarlanır:
    - Uygulamanız **bu kuruluş dizinindeki Hesapları**destekliyorsa, bu değeri kiracı kimliği veya kiracı adı ile değiştirin (örneğin, contoso.microsoft.com).
    - Uygulamanız **hesapları herhangi bir kuruluş dizininde destekliyorsa,** bu değeri ' le `organizations`değiştirin.
    
    Tüm ulusal bulutlar için kimlik doğrulama bitiş noktalarını bulmak için [Azure AD kimlik doğrulama bitiş noktalarına](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints)bakın.

    > [!NOTE]
    > Kişisel Microsoft hesapları ulusal bulutlarda desteklenmez.
  
- `graphEndpoint`ABD hükümeti için Microsoft bulutu için Microsoft Grafiği bitiş noktasıdır.

   Tüm ulusal bulutlar için Microsoft Grafik uç noktalarını bulmak için, [ulusal bulutlardaki Microsoft Graph uç noktalarına](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints)bakın.

## <a name="python"></a>[Python](#tab/python)

Egemen bulutlar için MSAL Python uygulamanızı etkinleştirmek için:

- Uygulamanızı buluta bağlı olarak belirli bir portala kaydedin. Portalı nasıl seçeceğiniz hakkında daha fazla bilgi için [Uygulama kayıt bitiş noktalarına](authentication-national-cloud.md#app-registration-endpoints) bakın
- Repo'daki [örneklerden](https://github.com/AzureAD/microsoft-authentication-library-for-python/tree/dev/sample) herhangi birini, daha sonra belirtilen buluta bağlı olarak yapılandırmada birkaç değişiklikle kullanın.
- Uygulamayı kaydettiğiniz buluta bağlı olarak belirli bir yetki kullanın. Farklı bulutlar için yetkililer hakkında daha fazla bilgi için [Azure AD Kimlik Doğrulama bitiş noktalarına](authentication-national-cloud.md#azure-ad-authentication-endpoints)bakın.

    İşte örnek bir yetki:

    ```json
    "authority": "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here"
    ```
    
- Microsoft grafiğini aramak için, kullandığınız buluta bağlı olarak belirli bir Grafik uç noktası URL'si gerektirir. Tüm ulusal bulutlar için Microsoft Grafik uç noktalarını bulmak için [Microsoft Graph ve Graph Explorer hizmet kök uç noktalarına](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints)bakın.

    Aşağıda, kapsamı olan grafik bitiş noktası nın bir örneği verilmiştir:
    
    ```json
    "endpoint" : "https://graph.microsoft.us/v1.0/me"
    "scope": "User.Read"
    ```
    
## <a name="java"></a>[Java](#tab/java)

Egemen bulutlar için Java için MSAL uygulamanızı etkinleştirmek için:

- Uygulamanızı buluta bağlı olarak belirli bir portala kaydedin. Portalı nasıl seçeceğiniz hakkında daha fazla bilgi için [Uygulama kayıt bitiş noktalarına](authentication-national-cloud.md#app-registration-endpoints) bakın
- Repo'daki [örneklerden](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples) herhangi birini, daha sonra belirtilen buluta bağlı olarak yapılandırmada birkaç değişiklikle kullanın.
- Uygulamayı kaydettiğiniz buluta bağlı olarak belirli bir yetki kullanın. Farklı bulutlar için yetkililer hakkında daha fazla bilgi için [Azure AD Kimlik Doğrulama bitiş noktalarına](authentication-national-cloud.md#azure-ad-authentication-endpoints)bakın.

İşte örnek bir yetki:

```json
"authority": "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here"
```

- Microsoft grafiğini aramak için, kullandığınız buluta bağlı olarak belirli bir Grafik uç noktası URL'si gerektirir. Tüm ulusal bulutlar için Microsoft Grafik uç noktalarını bulmak için [Microsoft Graph ve Graph Explorer hizmet kök uç noktalarına](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints)bakın.

Aşağıda, kapsamı olan grafik bitiş noktası nın bir örneği verilmiştir:

```json
"endpoint" : "https://graph.microsoft.us/v1.0/me"
"scope": "User.Read"
```

## <a name="objective-c"></a>[Objective-C](#tab/objc)

iOS ve macOS için MSAL ulusal bulutlarda belirteçleri elde etmek `MSALPublicClientApplication`için kullanılabilir, ancak oluştururken ek yapılandırma gerektirir.

Örneğin, uygulamanızın ulusal bir bulutta (burada ABD Hükümeti) çok kiracılı bir uygulama olmasını istiyorsanız, şunları yazabilirsiniz:

```objc
MSALAADAuthority *aadAuthority =
                [[MSALAADAuthority alloc] initWithCloudInstance:MSALAzureUsGovernmentCloudInstance
                                                   audienceType:MSALAzureADMultipleOrgsAudience
                                                      rawTenant:nil
                                                          error:nil];
                                                          
MSALPublicClientApplicationConfig *config =
                [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"
                                                                redirectUri:@"<your-redirect-uri-here>"
                                                                  authority:aadAuthority];
                                                                  
NSError *applicationError = nil;
MSALPublicClientApplication *application =
                [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&applicationError];
```

## <a name="swift"></a>[Swift](#tab/swift)

iOS ve macOS için MSAL ulusal bulutlarda belirteçleri elde etmek `MSALPublicClientApplication`için kullanılabilir, ancak oluştururken ek yapılandırma gerektirir.

Örneğin, uygulamanızın ulusal bir bulutta (burada ABD Hükümeti) çok kiracılı bir uygulama olmasını istiyorsanız, şunları yazabilirsiniz:

```swift
let authority = try? MSALAADAuthority(cloudInstance: .usGovernmentCloudInstance, audienceType: .azureADMultipleOrgsAudience, rawTenant: nil)
        
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>", redirectUri: "<your-redirect-uri-here>", authority: authority)
if let application = try? MSALPublicClientApplication(configuration: config) { /* Use application */}
```

---

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Ulusal Bulutlarda Kimlik Doğrulama](authentication-national-cloud.md)
- [Azure Kamu](https://docs.microsoft.com/azure/azure-government/)
- [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Almanya](https://docs.microsoft.com/azure/germany/)
