---
title: Web API 'Lerini çağıran masaüstü uygulamalarını yapılandırma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir masaüstü uygulamasının kodunu yapılandırmayı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev, tracking-python
ms.openlocfilehash: f197dd5063f8584968277d8d55298c03d9d71ea6
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84558839"
---
# <a name="desktop-app-that-calls-web-apis-code-configuration"></a>Web API 'Lerini çağıran masaüstü uygulaması: kod yapılandırması

Uygulamanızı oluşturduğunuza göre, kodun uygulamanın koordinatlarıyla nasıl yapılandırılacağını öğreneceksiniz.

## <a name="microsoft-authentication-libraries"></a>Microsoft kimlik doğrulama kitaplıkları

Aşağıdaki Microsoft kimlik doğrulama kitaplıkları (MSALs) masaüstü uygulamalarını destekler.

  Microsoft Authentication Library | Description
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | , Linux, Windows ve macOS gibi birden çok platformda masaüstü uygulaması oluşturmayı destekler.
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL Python | , Bir masaüstü uygulamasının birden çok platformda oluşturulmasını destekler.
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL Java | , Bir masaüstü uygulamasının birden çok platformda oluşturulmasını destekler.
  ![MSAL iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL iOS | Yalnızca macOS 'ta çalışan masaüstü uygulamalarını destekler.

## <a name="public-client-application"></a>Ortak istemci uygulaması

Bir görünüm noktasından, masaüstü uygulamaları ortak istemci uygulamalardır. Yapılandırma etkileşimli kimlik doğrulaması kullanmanıza bakılmaksızın bir bit farklı olacaktır.

# <a name="net"></a>[.NET](#tab/dotnet)

MSAL.NET oluşturmanız ve güncelleştirmeniz gerekir `IPublicClientApplication` .

![IPublicClientApplication](media/scenarios/public-client-application.png)

### <a name="exclusively-by-code"></a>Yalnızca kodla

Aşağıdaki kod, bir ortak istemci uygulaması başlatır ve Microsoft Azure genel buluttaki kullanıcılara bir iş veya okul hesabı ya da kişisel Microsoft hesabı işaretler.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

Daha önce görüldüğü gibi etkileşimli kimlik doğrulama veya cihaz kod akışı kullanmayı düşünüyorsanız, `.WithRedirectUri` değiştiricisini kullanın.

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="use-configuration-files"></a>Yapılandırma dosyalarını kullanma

Aşağıdaki kod, bir yapılandırma nesnesinden program aracılığıyla doldurulabilen veya bir yapılandırma dosyasından okunan bir yapılandırma nesnesinden ortak bir istemci uygulaması başlatır.

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="more-elaborated-configuration"></a>Daha fazla ayrıntılı yapılandırma

Birçok değiştirici ekleyerek uygulama oluşturmayı ayrıntılı hale getirebilirsiniz. Örneğin, uygulamanızın ulusal bir bulutta, burada gösterilen ABD devleti gibi çok kiracılı bir uygulama olmasını istiyorsanız şunu yazabilirsiniz:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment,
                         AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

MSAL.NET ayrıca Active Directory Federasyon Hizmetleri (AD FS) 2019 için bir değiştirici içerir:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Son olarak, bir Azure Active Directory (Azure AD) B2C kiracısı için belirteçler edinmek istiyorsanız, aşağıdaki kod parçacığında gösterildiği gibi kiracınızı belirtin:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

### <a name="learn-more"></a>Daha fazla bilgi edinin

MSAL.NET masaüstü uygulamasını yapılandırma hakkında daha fazla bilgi edinmek için:

- Üzerinde kullanılabilen tüm değiştiricilerin bir listesi için `PublicClientApplicationBuilder` bkz. Reference documentation [Publicclientapplicationbuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods).
- İçinde gösterilen tüm seçeneklerin bir açıklaması için `PublicClientApplicationOptions` başvuru belgelerindeki [Publicclientapplicationoptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions) bölümüne bakın.

### <a name="complete-example-with-configuration-options"></a>Yapılandırma seçenekleriyle birlikte tüm örnek

Aşağıdaki yapılandırma dosyasına sahip bir .NET Core konsol uygulaması düşünün `appsettings.json` :

```json
{
  "Authentication": {
    "AzureCloudInstance": "AzurePublic",
    "AadAuthorityAudience": "AzureAdMultipleOrgs",
    "ClientId": "ebe2ab4d-12b3-4446-8480-5c3828d04c50"
  },

  "WebAPI": {
    "MicrosoftGraphBaseEndpoint": "https://graph.microsoft.com"
  }
}
```

Kullanarak bu dosyada okunması gereken küçük bir koddur. NET tarafından sağlanmış yapılandırma çerçevesi:

```csharp
public class SampleConfiguration
{
 /// <summary>
 /// Authentication options
 /// </summary>
 public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

 /// <summary>
 /// Base URL for Microsoft Graph (it varies depending on whether the application runs
 /// in Microsoft Azure public clouds or national or sovereign clouds)
 /// </summary>
 public string MicrosoftGraphBaseEndpoint { get; set; }

 /// <summary>
 /// Reads the configuration from a JSON file
 /// </summary>
 /// <param name="path">Path to the configuration json file</param>
 /// <returns>SampleConfiguration as read from the json file</returns>
 public static SampleConfiguration ReadFromJsonFile(string path)
 {
  // .NET configuration
  IConfigurationRoot Configuration;
  var builder = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile(path);
  Configuration = builder.Build();

  // Read the auth and graph endpoint configuration
  SampleConfiguration config = new SampleConfiguration()
  {
   PublicClientApplicationOptions = new PublicClientApplicationOptions()
  };
  Configuration.Bind("Authentication", config.PublicClientApplicationOptions);
  config.MicrosoftGraphBaseEndpoint =
  Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
  return config;
 }
}
```

Şimdi, uygulamanızı oluşturmak için aşağıdaki kodu yazın:

```csharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .WithDefaultRedirectUri()
           .Build();
```

Yönteme çağrıdan önce `.Build()` , `.WithXXX` daha önce görüldüğü gibi yöntemlere yapılan çağrılar ile yapılandırmanızı geçersiz kılabilirsiniz.

# <a name="java"></a>[Java](#tab/java)

Örnekleri yapılandırmak için MSAL Java geliştirme örneklerinde kullanılan sınıf şunlardır: [TestData](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/).

```Java
PublicClientApplication pca = PublicClientApplication.builder(CLIENT_ID)
        .authority(AUTHORITY)
        .build();
```

# <a name="python"></a>[Python](#tab/python)

```Python
config = json.load(open(sys.argv[1]))

app = msal.PublicClientApplication(
    config["client_id"], authority=config["authority"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="macos"></a>[MacOS](#tab/macOS)

Aşağıdaki kod, bir ortak istemci uygulaması başlatır ve Microsoft Azure genel buluttaki kullanıcılara bir iş veya okul hesabı ya da kişisel Microsoft hesabı işaretler.

### <a name="quick-configuration"></a>Hızlı yapılandırma

Amaç-C:

```objc
NSError *msalError = nil;

MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

SWIFT
```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

### <a name="more-elaborated-configuration"></a>Daha fazla ayrıntılı yapılandırma

Birçok değiştirici ekleyerek uygulama oluşturmayı ayrıntılı hale getirebilirsiniz. Örneğin, uygulamanızın ulusal bir bulutta, burada gösterilen ABD devleti gibi çok kiracılı bir uygulama olmasını istiyorsanız şunu yazabilirsiniz:

Amaç-C:

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

SWIFT

```swift
let authority = try? MSALAADAuthority(cloudInstance: .usGovernmentCloudInstance, audienceType: .azureADMultipleOrgsAudience, rawTenant: nil)

let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>", redirectUri: "<your-redirect-uri-here>", authority: authority)
if let application = try? MSALPublicClientApplication(configuration: config) { /* Use application */}
```
---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması için belirteç alma](scenario-desktop-acquire-token.md)
