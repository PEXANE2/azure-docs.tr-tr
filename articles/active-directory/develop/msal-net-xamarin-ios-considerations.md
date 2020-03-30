---
title: Xamarin iOS hususlar (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET için Microsoft Kimlik Doğrulama Kitaplığı ile Xamarin iOS'u kullanmak için dikkat edilmesi gereken hususlar hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76e614b605cd07cd5dc454824dd204447f806907
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262717"
---
# <a name="considerations-for-using-xamarin-ios-with-msalnet"></a>xamarin iOS'u MSAL.NET ile kullanmak için dikkat edilmesi gerekenler
Xamarin iOS'ta .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nı kullandığınızda, şunları 

- Geçersiz kılın ve `OpenUrl` işlevi `AppDelegate`uygulayın.
- Anahtarlık gruplarını etkinleştirin.
- Belirteç önbellek paylaşımını etkinleştirin.
- Anahtarlık erişimini etkinleştirin.
- iOS 12 ve kimlik doğrulama ile bilinen sorunları anlayın.

## <a name="implement-openurl"></a>OpenUrl uygula

Türemiş `OpenUrl` sınıfın `FormsApplicationDelegate` yöntemini geçersiz `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`kılın ve çağırın. Bir örneği aşağıda verilmiştir:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

Ayrıca aşağıdaki görevleri yapın: 
* BIR URL düzeni tanımlayın.
* Uygulamanızın başka bir uygulamayı araması için izinler talep edin.
* Yönlendirme URL'si için belirli bir form alalım.
* Yeniden yönlendirme URL'sini [Azure portalına](https://portal.azure.com)kaydedin.

### <a name="enable-keychain-access"></a>Anahtarlık erişimini etkinleştirme

Anahtarlık erişimini etkinleştirmek için uygulamanızın bir anahtarzinciri erişim grubuna sahip olduğundan emin olun. UYGULAMANIZI `WithIosKeychainSecurityGroup()` oluştururken, API'yi kullanarak anahtarzinciri erişim grubunu ayarlayabilirsiniz.

Önbellekten ve tek oturum açmadan (SSO) yararlanmak için, anahtarlık erişim grubunu tüm uygulamalarınızda aynı değere ayarlayın.

Kurulumun bu örneği MSAL 4.x kullanır:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Ayrıca dosyada anahtarlık `Entitlements.plist` erişimini etkinleştirin. Aşağıdaki erişim grubunu veya kendi erişim grubunuzu kullanın.

```xml
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
```

API'yi `WithIosKeychainSecurityGroup()` kullandığınızda, MSAL güvenlik grubunuzu otomatik olarak uygulamanın *takım kimliğinin* sonuna ekler (`AppIdentifierPrefix`). MSAL, uygulamanızı Xcode'da oluşturduğunuzda aynı şeyi yapacağından güvenlik grubunuzu ekler. Bu nedenle, dosyadaki `Entitlements.plist` yetkilendirmelerin anahtarlık `$(AppIdentifierPrefix)` erişim grubuna dahil etmesi gerekir.

Daha fazla bilgi için [iOS yetkilendirme belgelerine](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps)bakın. 

### <a name="enable-token-cache-sharing-across-ios-applications"></a>iOS uygulamaları arasında belirteç önbelleği paylaşımını etkinleştirme

MSAL 2.x'ten başlayarak, belirteç önbelleğini birden çok uygulamada sürdürmek için bir anahtarlık erişim grubu belirtebilirsiniz. Bu ayar, belirteç önbelleğini aynı anahtarlık erişim grubuna sahip birkaç uygulama arasında paylaşmanızı sağlar. Jeton nakitini [ADAL.NET](https://aka.ms/adal-net) uygulamalar, MSAL.NET Xamarin.iOS uygulamaları ve [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) veya [MSAL.objc'de](https://github.com/AzureAD/microsoft-authentication-library-for-objc)geliştirilen yerel iOS uygulamaları arasında paylaşabilirsiniz.

Belirteç önbelleğini paylaşarak, aynı anahtarzinciri erişim grubunu kullanan tüm uygulamalar arasında tek oturum açma (SSO) izni verirsiniz.

Bu önbellek paylaşımını etkinleştirmek için, anahtar zinciri erişim grubunu aynı önbelleği paylaşan tüm uygulamalarda aynı değere ayarlamak için `WithIosKeychainSecurityGroup()` yöntemi kullanın. Bu makaledeki ilk kod örneği yöntemin nasıl kullanılacağını gösterir.

Bu makalenin başlarında, API'yi her kullandığınızda MSAL'ın `$(AppIdentifierPrefix)` `WithIosKeychainSecurityGroup()` ekettiğini öğrenmişsinizdir. MSAL bu öğeyi ekler, çünkü takım kimliği `AppIdentifierPrefix` yalnızca aynı yayımcı tarafından yapılan uygulamaların anahtarlık erişimini paylaşabilmesini sağlar.

> [!NOTE]
> Mülk `KeychainSecurityGroup` amortismana lı.
> 
> MSAL 2.x'ten başlayarak, geliştiriciler `TeamId` `KeychainSecurityGroup` özelliği kullanırken önek eklemek zorunda kaldılar. Ancak MSAL 2.7.x'ten başlayarak, `iOSKeychainSecurityGroup` yeni özelliği kullandığınızda, MSAL çalışma süresi sırasında `TeamId` öneki çözer. Bu özelliği kullandığınızda, değere `TeamId` önek eklemeyin. Önek gerekli değildir.
>
> `KeychainSecurityGroup` Özellik eski olduğundan, `iOSKeychainSecurityGroup` özelliği kullanın.

### <a name="use-microsoft-authenticator"></a>Microsoft Authenticator'ı kullanma

Uygulamanız, şunları etkinleştirmek için Microsoft Authenticator'ı aracı olarak kullanabilir:

- **SSO**: SSO'yu etkinleştirdiğinizde, kullanıcılarınızın her uygulamada oturum açmalarına gerek yoktur.
- **Cihaz tanımlama**: Cihaz sertifikasına erişerek kimlik doğrulaması yapmak için aygıt tanımlamasını kullanın. Bu sertifika, işyerine katıldığında aygıtta oluşturulur. Kiracı yöneticiler aygıtlarla ilgili koşullu erişimi etkinleştirirlerse uygulamanız hazır olur.
- **Uygulama tanımlama doğrulaması**: Bir uygulama aracıyı aradığında, yeniden yönlendirme URL'sini geçer. Aracı, yönlendirme URL'sini doğrular.

Bir aracıyı etkinleştirme hakkında daha fazla bilgi için, [Xamarin iOS ve Android uygulamalarında Microsoft Authenticator veya Microsoft Intune Company Portalını Kullanın'a](msal-net-use-brokers-with-xamarin-apps.md)bakın.

## <a name="known-issues-with-ios-12-and-authentication"></a>iOS 12 ve kimlik doğrulama ile bilinen sorunlar
Microsoft, iOS 12 ile bazı kimlik doğrulama türleri arasındaki uyumsuzlukla ilgili bir [güvenlik danışma belgesi](https://github.com/aspnet/AspNetCore/issues/4647) yayımladı. Uyumsuzluk sosyal, WSFed ve OIDC oturum açmaları tatili. Güvenlik danışma belgesi, geliştiricilerin ASP.NET güvenlik kısıtlamalarını iOS 12 ile uyumlu hale getirmek için uygulamalarından nasıl kaldıracaklarını anlamalarına yardımcı olur.  

Xamarin iOS'ta MSAL.NET uygulamaları geliştirdiğinizde, iOS 12'den web sitelerinde oturum açmaya çalıştığınızda sonsuz bir döngü görebilirsiniz. Bu davranış, bu [ADAL sorununa](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329)benzer. 

Ayrıca, iOS 12 Safari ile Core OIDC kimlik doğrulaması ASP.NET bir kopuş da görebilirsiniz. Daha fazla bilgi için bu [WebKit sorununa](https://bugs.webkit.org/show_bug.cgi?id=188165)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Xamarin iOS özellikleri hakkında bilgi için, aşağıdaki örneğin README.md dosyasının [iOS'a özgü hususlar](https://github.com/Azure-Samples/active-directory-xamarin-native-v2/tree/master/1-Basic#ios-specific-considerations) paragrafına bakın:

Örnek | Platform | Açıklama
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, Evrensel Windows Platformu (UWP) | Azure AD 2.0 bitiş noktası üzerinden Microsoft kişisel hesaplarını ve Azure AD'yi doğrulamak için MSAL'ın nasıl kullanılacağını gösteren basit bir Xamarin Forms uygulaması. Uygulama ayrıca Microsoft Graph'a erişmek için ortaya çıkan belirteci nasıl kullanacağımı da gösterir.

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->