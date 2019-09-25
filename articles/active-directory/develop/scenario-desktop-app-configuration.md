---
title: Web API 'Lerini çağıran masaüstü uygulaması (kod yapılandırması)-Microsoft Identity platform
description: Web API 'Lerini (uygulamanın kod yapılandırması) çağıran bir masaüstü uygulaması oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0926e6800dbcd81d2e542e27afe3afb1240cff22
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268395"
---
# <a name="desktop-app-that-calls-web-apis---code-configuration"></a>Web API 'Leri çağıran masaüstü uygulaması-kod yapılandırması

Uygulamanızı oluşturduğunuza göre, kodun uygulamanın koordinatlarıyla nasıl yapılandırılacağını öğreneceksiniz.

## <a name="msal-libraries"></a>MSAL kitaplıkları

Şu anda birden çok platformda masaüstü uygulamalarını destekleyen tek MSAL kitaplığı MSAL.NET.

İOS ve macOS için MSAL, yalnızca macOS 'ta çalışan masaüstü uygulamalarını destekler. 

## <a name="public-client-application-with-msalnet"></a>MSAL.NET ile ortak istemci uygulaması

Bir görünüm noktasından, masaüstü uygulamaları ortak istemci uygulamalardır ve bu nedenle MSAL.NET `IPublicClientApplication`derlemenize ve işleyebileceksiniz. Etkileşimli kimlik doğrulaması kullanmanıza bakılmaksızın bir adım daha farklı olacaktır.

![IPublicClientApplication](media/scenarios/public-client-application.png)

### <a name="exclusively-by-code"></a>Yalnızca kodla

Aşağıdaki kod, bir iş ve okul hesabı ya da kişisel Microsoft hesabı Microsoft Azure genel bulutta oturum açan kullanıcıları ortak bir istemci uygulaması için başlatır.

```CSharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

Yukarıda görüldüğü gibi etkileşimli kimlik doğrulama veya cihaz kod akışı kullanmayı düşünüyorsanız, `.WithRedirectUri` değiştiriciyi kullanmak istersiniz:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="using-configuration-files"></a>Yapılandırma dosyalarını kullanma

Aşağıdaki kod, program aracılığıyla veya bir yapılandırma dosyasından okunan bir yapılandırma nesnesinden ortak bir istemci uygulaması başlatır

```CSharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="more-elaborated-configuration"></a>Daha fazla ayrıntılı yapılandırma

Birçok değiştirici ekleyerek uygulama oluşturmayı ayrıntılı hale getirebilirsiniz. Örneğin, uygulamanızın ulusal bir bulutta (burada ABD devlet) çok kiracılı bir uygulama olmasını istiyorsanız şunu yazabilirsiniz:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment,
                         AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

MSAL.NET, ADFS 2019 için bir değiştirici de içerir:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Son olarak, bir Azure AD B2C kiracının belirteçlerini almak istiyorsanız, aşağıdaki kod parçacığında gösterildiği gibi kiracınızı belirtebilir:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

### <a name="learn-more"></a>Daha fazla bilgi edinin

MSAL.NET masaüstü uygulamasını yapılandırma hakkında daha fazla bilgi edinmek için:

- Üzerinde `PublicClientApplicationBuilder`kullanılabilen tüm değiştiricilerin listesi için bkz. Reference documentation [publicclientapplicationbuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods)
- Bkz. `PublicClientApplicationOptions` [publicclientapplicationoptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions)içinde gösterilen tüm seçeneklerin açıklaması için başvuru belgeleri

## <a name="complete-example-with-configuration-options"></a>Yapılandırma seçenekleriyle birlikte tüm örnek

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

.NET tarafından sunulan yapılandırma çerçevesini kullanarak bu dosyayı okumak için çok az kodunuz var;

```CSharp
public class SampleConfiguration
{
 /// <summary>
 /// Authentication options
 /// </summary>
 public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

 /// <summary>
 /// Base URL for Microsoft Graph (it varies depending on whether the application is ran
 /// in Microsoft Azure public clouds or national / sovereign clouds
 /// </summary>
 public string MicrosoftGraphBaseEndpoint { get; set; }

 /// <summary>
 /// Reads the configuration from a json file
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

  // Read the auth and graph endpoint config
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

Şimdi, uygulamanızı oluşturmak için yalnızca aşağıdaki kodu yazmanız gerekir:

```CSharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .WithDefaultRedirectUri()
           .Build();
```

ve `.Build()` yöntemine çağrıdan önce, daha önce görüldüğü gibi `.WithXXX` yöntemlere çağrılar ile yapılandırmanızı geçersiz kılabilirsiniz.

## <a name="public-client-application-with-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL ile ortak istemci uygulaması

Aşağıdaki kod, bir iş ve okul hesabı ya da kişisel Microsoft hesabı Microsoft Azure genel bulutta oturum açan kullanıcıları ortak bir istemci uygulaması için başlatır.

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

Birçok değiştirici ekleyerek uygulama oluşturmayı ayrıntılı hale getirebilirsiniz. Örneğin, uygulamanızın ulusal bir bulutta (burada ABD devlet) çok kiracılı bir uygulama olmasını istiyorsanız şunu yazabilirsiniz:

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması için belirteç alma](scenario-desktop-acquire-token.md)
