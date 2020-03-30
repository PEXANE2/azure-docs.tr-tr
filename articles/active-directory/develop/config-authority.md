---
title: Kimlik sağlayıcılarını yapılandırma (MSAL iOS/macOS) | Azure
titleSuffix: Microsoft identity platform
description: iOS ve macOS için MSAL ile B2C, egemen bulutlar ve konuk kullanıcılar gibi farklı yetkilileri nasıl kullanacağınızı öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 4810de772e44be22ee5bd4a9fb6ef0ef756e62f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085215"
---
# <a name="how-to-configure-msal-for-ios-and-macos-to-use-different-identity-providers"></a>Nasıl kullanılır: farklı kimlik sağlayıcılarını kullanmak için iOS ve macOS için MSAL'ı yapılandırın

Bu makalede, azure Active Directory (Azure AD), Business-to-Consumer (B2C), egemen bulutlar ve konuk kullanıcılar gibi farklı otoriteler için iOS ve macOS (MSAL) için Microsoft kimlik doğrulama kitaplığı uygulamanızı nasıl yapılandıracağınızı gösterecektir.  Bu makale boyunca, genellikle bir kimlik sağlayıcısı olarak bir otorite düşünebilirsiniz.

## <a name="default-authority-configuration"></a>Varsayılan yetki yapılandırması

`MSALPublicClientApplication`çoğu Azure Etkin Dizin `https://login.microsoftonline.com/common`(AAD) senaryosu için uygun olan varsayılan yetki URL'si ile yapılandırılır. Ulusal bulutlar gibi gelişmiş senaryolar uygulamadığınız veya B2C ile çalışmadığınız sürece, değiştirmeniz gerekmez.

> [!NOTE]
> Active Directory Federation Services ile kimlik sağlayıcısı (ADFS) olarak modern kimlik doğrulaması desteklenmez (ayrıntılar [için Geliştiriciler için ADFS'ye](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios) bakın). ADFS federasyon aracılığıyla desteklenir.

## <a name="change-the-default-authority"></a>Varsayılan yetkisini değiştirme

İşletmeden tüketiciye (B2C) gibi bazı senaryolarda varsayılan yetkiyi değiştirmeniz gerekebilir.

### <a name="b2c"></a>B2C

B2C ile çalışmak için [Microsoft Kimlik Doğrulama Kitaplığı (MSAL)](reference-v2-libraries.md) farklı bir yetki yapılandırması gerektirir. MSAL tek başına b2C olarak bir yetki URL biçimini tanır. Tanınan B2C yetki `https://<host>/tfp/<tenant>/<policy>`biçimi, `https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SignInPolicy`örneğin . Ancak, yetkiyi Açıkça B2C yetkilisi olarak beyan ederek desteklenen diğer B2C yetkisini de kullanabilirsiniz.

B2C için rasgele bir URL `MSALB2CAuthority` biçimini desteklemek için, aşağıdaki gibi rasgele bir URL ile ayarlanabilir:

Objective-C
```objc
NSURL *authorityURL = [NSURL URLWithString:@"arbitrary URL"];
MSALB2CAuthority *b2cAuthority = [[MSALB2CAuthority alloc] initWithURL:authorityURL
                                                                     error:&b2cAuthorityError];
```
Swift
```swift
guard let authorityURL = URL(string: "arbitrary URL") else {
    // Handle error
    return
}
let b2cAuthority = try MSALB2CAuthority(url: authorityURL)
```

Varsayılan B2C yetki biçimini kullanmayan tüm B2C yetkilileri bilinen yetkililer olarak beyan edilmelidir.

Yetkililer yalnızca politikada farklılık sayılsa bile, her bir farklı B2C yetkisini bilinen yetkililer listesine ekleyin.

Objective-C
```objc
MSALPublicClientApplicationConfig *b2cApplicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                               initWithClientId:@"your-client-id"
                                                               redirectUri:@"your-redirect-uri"
                                                               authority:b2cAuthority];
b2cApplicationConfig.knownAuthorities = @[b2cAuthority];
```
Swift
```swift
let b2cApplicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: b2cAuthority)
b2cApplicationConfig.knownAuthorities = [b2cAuthority]
```

Uygulamanız yeni bir ilke istediğinde, yetki URL'si her ilke için farklı olduğundan, yetkili URL'nin değiştirilmesi gerekir. 

B2C uygulamasını yapılandırmak için, `@property MSALAuthority *authority` oluşturmadan `MSALB2CAuthority` `MSALPublicClientApplication` `MSALPublicClientApplicationConfig` önce bir örneğini içeren olarak ayarlanır:

Objective-C
```ObjC
    // Create B2C authority URL
    NSURL *authorityURL = [NSURL URLWithString:@"https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SignInPolicy"];
    
    MSALB2CAuthority *b2cAuthority = [[MSALB2CAuthority alloc] initWithURL:authorityURL
                                                                     error:&b2cAuthorityError];
    if (!b2cAuthority)
    {
        // Handle error
        return;
    }
    
    // Create MSALPublicClientApplication configuration
    MSALPublicClientApplicationConfig *b2cApplicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                                   initWithClientId:@"your-client-id"
                                                                   redirectUri:@"your-redirect-uri"
                                                                   authority:b2cAuthority];

    // Initialize MSALPublicClientApplication
    MSALPublicClientApplication *b2cApplication =
    [[MSALPublicClientApplication alloc] initWithConfiguration:b2cApplicationConfig error:&error];
    
    if (!b2cApplication)
    {
        // Handle error
        return;
    }
```
Swift
```swift
do{
    // Create B2C authority URL
    guard let authorityURL = URL(string: "https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SignInPolicy") else {
        // Handle error
        return
    }
    let b2cAuthority = try MSALB2CAuthority(url: authorityURL)

    // Create MSALPublicClientApplication configuration
    let b2cApplicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: b2cAuthority)

    // Initialize MSALPublicClientApplication
    let b2cApplication = try MSALPublicClientApplication(configuration: b2cApplicationConfig)
} catch {
    // Handle error
}
```

### <a name="sovereign-clouds"></a>Egemen bulutlar

Uygulamanız egemen bir bulutta çalışıyorsa, 'deki yetki `MSALPublicClientApplication`URL'sini değiştirmeniz gerekebilir. Aşağıdaki örnek, Alman AAD bulutuyla çalışmak için yetki URL'sini belirler:

Objective-C
```objc
    NSURL *authorityURL = [NSURL URLWithString:@"https://login.microsoftonline.de/common"];
    MSALAuthority *sovereignAuthority = [MSALAuthority authorityWithURL:authorityURL error:&authorityError];
    
    if (!sovereignAuthority)
    {
        // Handle error
        return;
    }
    
    MSALPublicClientApplicationConfig *applicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                               initWithClientId:@"your-client-id"
                                                               redirectUri:@"your-redirect-uri"
                                                               authority:sovereignAuthority];
    
    
    MSALPublicClientApplication *sovereignApplication = [[MSALPublicClientApplication alloc] initWithConfiguration:applicationConfig error:&error];
    
    
    if (!sovereignApplication)
    {
        // Handle error
        return;
    }
```
Swift
```swift
do{
    guard let authorityURL = URL(string: "https://login.microsoftonline.de/common") else {
        //Handle error
        return
    }
    let sovereignAuthority = try MSALAuthority(url: authorityURL)
            
    let applicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: sovereignAuthority)
            
    let sovereignApplication = try MSALPublicClientApplication(configuration: applicationConfig)
} catch {
    // Handle error
}
```

Her egemen buluta farklı kapsamlar geçirmeniz gerekebilir. Hangi kapsamların gönderilen, kullandığınız kaynağa bağlıdır. Örneğin, dünya çapında `"https://graph.microsoft.com/user.read"` bulutlarda ve `"https://graph.microsoft.de/user.read"` Alman bulutlarında kullanabilirsiniz.

### <a name="signing-a-user-into-a-specific-tenant"></a>Kullanıcıyı belirli bir kiracıya imzalama

Yetkili URL `"login.microsoftonline.com/common"`ayarlandığında, kullanıcı ev kiracısına oturum açacaktır. Ancak, bazı uygulamaların kullanıcıyı farklı bir kiracıya imzalaması gerekebilir ve bazı uygulamaların yalnızca tek bir kiracıyla çalışması gerekir.

Kullanıcıyı belirli bir kiracıya imzalamak `MSALPublicClientApplication` için, belirli bir yetkiyle yapılandırın. Örnek:

`https://login.microsoftonline.com/469fdeb4-d4fd-4fde-991e-308a78e4bea4`

Aşağıda, bir kullanıcının belirli bir kiracıda nasıl oturum açılalır gösterilmektedir:

Objective-C
```objc
    NSURL *authorityURL = [NSURL URLWithString:@"https://login.microsoftonline.com/469fdeb4-d4fd-4fde-991e-308a78e4bea4"];
    MSALAADAuthority *tenantedAuthority = [[MSALAADAuthority alloc] initWithURL:authorityURL error:&authorityError];
    
    if (!tenantedAuthority)
    {
        // Handle error
        return;
    }
    
    MSALPublicClientApplicationConfig *applicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                               initWithClientId:@"your-client-id"
                                                               redirectUri:@"your-redirect-uri"
                                                               authority:tenantedAuthority];
    
    MSALPublicClientApplication *application =
    [[MSALPublicClientApplication alloc] initWithConfiguration:applicationConfig error:&error];
    
    if (!application)
    {
        // Handle error
        return;
    }
```
Swift
```swift
do{
    guard let authorityURL = URL(string: "https://login.microsoftonline.com/469fdeb4-d4fd-4fde-991e-308a78e4bea4") else {
        //Handle error
        return
    }    
    let tenantedAuthority = try MSALAADAuthority(url: authorityURL)
            
    let applicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: tenantedAuthority)
            
    let application = try MSALPublicClientApplication(configuration: applicationConfig)
} catch {
    // Handle error
}
```

## <a name="supported-authorities"></a>Desteklenen yetkililer

### <a name="msalauthority"></a>MSALAuthority

Sınıf, `MSALAuthority` MSAL yetki sınıfları için temel soyut sınıftır. Kullanarak `alloc` veya `new`bir örneğini oluşturmaya çalışmayın. Bunun yerine, alt sınıflarından birini`MSALAADAuthority` `MSALB2CAuthority`doğrudan (, ) `authorityWithURL:error:` oluşturun veya yetki URL'sini kullanarak alt sınıflar oluşturmak için fabrika yöntemini kullanın.

Normalleştirilmiş `url` bir yetki URL'si almak için özelliği kullanın. Yetkinin bir parçası olmayan ek parametreler ve yol bileşenleri veya parçaları döndürülen normalleştirilmiş yetki URL'sinde yer almaz.

Aşağıda kullanmak istediğiniz `MSALAuthority` yetkiye bağlı olarak anlık olarak kullanabileceğiniz alt sınıflar ve bu alt sınıflar verebilirsiniz.

### <a name="msalaadauthority"></a>MSALAADAuthority

`MSALAADAuthority`bir AAD otoritesini temsil eder. Yetki url'si isteğe bağlı `<port>` olarak aşağıdaki biçimde olmalıdır:`https://<host>:<port>/<tenant>`

### <a name="msalb2cauthority"></a>MSALB2CAuthority

`MSALB2CAuthority`bir B2C yetkilisini temsil eder. Varsayılan olarak, B2C yetki url'si aşağıdaki `<port>` biçimde `https://<host>:<port>/tfp/<tenant>/<policy>`olmalıdır, nerede isteğe bağlıdır: . Ancak, MSAL diğer rasgele B2C yetki biçimlerini de destekler.

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin
