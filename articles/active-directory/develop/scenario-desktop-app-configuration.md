---
title: Web API'lerini arayan masaüstü uygulamalarını yapılandırma - Microsoft kimlik platformu | Azure
description: Web API'leri çağıran bir masaüstü uygulamasının kodunu nasıl yapılandırılacaköğrenebilirsiniz
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f6cd9e17045fc400d32e9822cef870f1763179ab
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885282"
---
# <a name="desktop-app-that-calls-web-apis-code-configuration"></a>Web API'lerini çağıran masaüstü uygulaması: Kod yapılandırması

Uygulamanızı oluşturduğunuza göre, kodu uygulamanın koordinatlarıyla nasıl yapılandıracağınız öğrenilir.

## <a name="microsoft-authentication-libraries"></a>Microsoft Kimlik Doğrulama Kitaplıkları

Aşağıdaki Microsoft Kimlik Doğrulama Kitaplıkları (MSALs) masaüstü uygulamalarını destekler.

  Microsoft Authentication Library | Açıklama
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Linux, Windows ve macOS gibi birden çok platformda masaüstü uygulaması oluşturmayı destekler.
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL Piton | Birden çok platformda masaüstü uygulaması oluşturmayı destekler.
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL Java | Birden çok platformda masaüstü uygulaması oluşturmayı destekler.
  ![MSAL iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL iOS | Yalnızca macOS'ta çalışan masaüstü uygulamalarını destekler.

## <a name="public-client-application"></a>Kamu müşteri başvurusu

Kod açısından bakıldığında, masaüstü uygulamaları genel istemci uygulamalarıdır. Yapılandırma, etkileşimli kimlik doğrulaması kullanıp kullanmadığınıza bağlı olarak biraz farklı olacaktır.

# <a name="net"></a>[.NET](#tab/dotnet)

MSAL.NET `IPublicClientApplication`oluşturmanız ve manipüle etmeniz gerekecek.

![IPublicClientApplication](media/scenarios/public-client-application.png)

### <a name="exclusively-by-code"></a>Sadece koda göre

Aşağıdaki kod, bir iş veya okul hesabı veya kişisel Microsoft hesabıyla Microsoft Azure genel bulutundaki kullanıcılardaki ortak istemci uygulamasını anında işaretler ve işaretler.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

Daha önce görüldüğü gibi etkileşimli kimlik doğrulama veya aygıt kodu `.WithRedirectUri` akışını kullanmayı planlıyorsanız, değiştiriciyi kullanın.

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="use-configuration-files"></a>Yapılandırma dosyalarını kullanma

Aşağıdaki kod, programlı olarak doldurulabilen veya yapılandırma dosyasından okunabilen bir yapılandırma nesnesinden ortak istemci uygulamasını anında başlatır.

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="more-elaborated-configuration"></a>Daha ayrıntılı yapılandırma

Bir dizi değiştirici ekleyerek uygulama binasını ayrıntılı olarak geliştirebilirsiniz. Örneğin, uygulamanızın, ABD Hükümeti gibi ulusal bir bulutta çok kiracılı bir uygulama olmasını istiyorsanız, şunları yazabilirsiniz:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment,
                         AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

MSAL.NET ayrıca Active Directory Federation Services 2019 için bir değiştirici de içerir:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Son olarak, bir Azure Etkin Dizin (Azure AD) B2C kiracısı için belirteçler edinmek istiyorsanız, kiracınızı aşağıdaki kod snippet'inde gösterildiği gibi belirtin:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

### <a name="learn-more"></a>Daha fazla bilgi edinin

MSAL.NET bir masaüstü uygulamasını yapılandırma hakkında daha fazla bilgi edinmek için:

- Tüm değiştiriciler mevcut bir `PublicClientApplicationBuilder`liste için, referans belgeleri [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods)bakın.
- Açıkta kalan `PublicClientApplicationOptions`tüm seçeneklerin açıklaması için başvuru belgelerinde [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions) bölümüne bakın.

### <a name="complete-example-with-configuration-options"></a>Yapılandırma seçenekleriyle tam örnek

Aşağıdaki `appsettings.json` yapılandırma dosyasına sahip bir .NET Core konsol uygulaması düşünün:

```JSon
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

Bu dosyada okuyacak çok az kodunuz var. NET tarafından sağlanan yapılandırma çerçevesi:

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

`.Build()` Yönteme çağrı dan önce, yapılandırmanızı daha önce `.WithXXX` görüldüğü gibi yöntemlere yapılan çağrılarla geçersiz kılabilirsiniz.

# <a name="java"></a>[Java](#tab/java)

Örnekleri yapılandırmak için MSAL Java geliştirme örneklerinde kullanılan sınıf aşağıda veda edin: [TestData](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/TestData.java).

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

# <a name="macos"></a>[Macos](#tab/macOS)

Aşağıdaki kod, bir iş veya okul hesabı veya kişisel Microsoft hesabıyla Microsoft Azure genel bulutundaki kullanıcılardaki ortak istemci uygulamasını anında işaretler ve işaretler.

### <a name="quick-configuration"></a>Hızlı yapılandırma

Amaç-C:

```objc
NSError *msalError = nil;

MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

Swift:
```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

### <a name="more-elaborated-configuration"></a>Daha ayrıntılı yapılandırma

Bir dizi değiştirici ekleyerek uygulama binasını ayrıntılı olarak geliştirebilirsiniz. Örneğin, uygulamanızın, ABD Hükümeti gibi ulusal bir bulutta çok kiracılı bir uygulama olmasını istiyorsanız, şunları yazabilirsiniz:

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

Swift:

```swift
let authority = try? MSALAADAuthority(cloudInstance: .usGovernmentCloudInstance, audienceType: .azureADMultipleOrgsAudience, rawTenant: nil)

let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>", redirectUri: "<your-redirect-uri-here>", authority: authority)
if let application = try? MSALPublicClientApplication(configuration: config) { /* Use application */}
```
---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması için belirteç edinme](scenario-desktop-acquire-token.md)
