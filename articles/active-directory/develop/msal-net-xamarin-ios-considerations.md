---
title: Xamarin iOS konuları (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin iOS kullanma hakkında konular hakkında bilgi edinin.
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
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132465"
---
# <a name="considerations-for-using-xamarin-ios-with-msalnet"></a>MSAL.NET ile Xamarin iOS kullanma konuları
Xamarin iOS üzerinde .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullandığınızda şunları yapmalısınız: 

- `AppDelegate``OpenUrl` işlevini geçersiz kılın ve uygulayın.
- Anahtarlık gruplarını etkinleştirin.
- Belirteç önbelleği paylaşımını etkinleştirin.
- Anahtarlık erişimini etkinleştirin.
- İOS 12 ve kimlik doğrulamasıyla ilgili bilinen sorunları anlayın.

## <a name="implement-openurl"></a>OpenUrl 'Yi Uygula

`FormsApplicationDelegate` türetilmiş sınıfın `OpenUrl` yöntemini geçersiz kılın ve `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`çağırın. Bir örneği aşağıda verilmiştir:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

Aşağıdaki görevleri de yapın: 
* Bir URL şeması tanımlayın.
* Uygulamanızın başka bir uygulamayı çağırması için izin gerektir.
* Yeniden yönlendirme URL 'SI için belirli bir form gerekir.
* Yeniden yönlendirme URL 'sini [Azure Portal](https://portal.azure.com)kaydedin.

### <a name="enable-keychain-access"></a>Anahtarlık erişimini etkinleştir

Anahtarlık erişimini etkinleştirmek için, uygulamanızın bir Anahtarlık erişim grubuna sahip olduğundan emin olun. `WithIosKeychainSecurityGroup()` API 'sini kullanarak uygulamanızı oluştururken Anahtarlık erişim grubunu ayarlayabilirsiniz.

Önbellek ve çoklu oturum açma (SSO) avantajlarından yararlanmak için, Anahtarlık erişim grubunu tüm uygulamalarınızda aynı değere ayarlayın.

Kurulumun bu örneği MSAL 4. x kullanır:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

`Entitlements.plist` dosyasında Anahtarlık erişimini de etkinleştirin. Aşağıdaki erişim grubunu ya da kendi erişim grubunuzu kullanın.

```xml
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
```

`WithIosKeychainSecurityGroup()` API 'sini kullandığınızda MSAL, güvenlik grubunuzu otomatik olarak uygulamanın *Takım Kimliği* (`AppIdentifierPrefix`) sonuna ekler. MSAL, uygulamanızı Xcode 'da oluştururken aynı şekilde güvenlik grubunuzu ekler. `Entitlements.plist` dosyasındaki yetkilendirmelerin, Anahtarlık erişim grubundan önce `$(AppIdentifierPrefix)` eklemesi gerekir.

Daha fazla bilgi için bkz. [iOS yetkilendirmeleri belgeleri](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps). 

### <a name="enable-token-cache-sharing-across-ios-applications"></a>İOS uygulamaları arasında belirteç önbelleği paylaşımını etkinleştir

MSAL 2. x ' den başlayarak, belirteç önbelleğini birden çok uygulama arasında kalıcı hale getirmek için bir Anahtarlık erişim grubu belirtebilirsiniz. Bu ayar, belirteç önbelleğini aynı Anahtarlık erişim grubuna sahip çeşitli uygulamalar arasında paylaşmanızı sağlar. [Adal.net](https://aka.ms/adal-net) uygulamaları, msal.net Xamarin. iOS uygulamaları ve [adal. ObjC](https://github.com/AzureAD/azure-activedirectory-library-for-objc) veya [msal. ObjC](https://github.com/AzureAD/microsoft-authentication-library-for-objc)içinde geliştirilen Yerel iOS uygulamaları arasında bir belirteç Cash 'ı paylaşabilirsiniz.

Belirteç önbelleğini paylaşarak, aynı Anahtarlık erişim grubunu kullanan tüm uygulamalar arasında çoklu oturum açma 'ya (SSO) izin verebilirsiniz.

Bu önbellek paylaşımını etkinleştirmek için, anahtar zinciri erişim grubunu aynı önbelleği paylaşan tüm uygulamalarda aynı değere ayarlamak için `WithIosKeychainSecurityGroup()` yöntemini kullanın. Bu makaledeki ilk kod örneği, yönteminin nasıl kullanılacağını gösterir.

Bu makalenin önceki kısımlarında, `WithIosKeychainSecurityGroup()` API 'YI her kullandığınızda MSAL `$(AppIdentifierPrefix)` eklediği hakkında daha fazla öğrendiniz. MSAL bu öğeyi ekler çünkü `AppIdentifierPrefix` takım KIMLIĞI, yalnızca aynı yayımcı tarafından yapılan uygulamaların Anahtarlık erişimini paylaşabilmesini sağlar.

> [!NOTE]
> `KeychainSecurityGroup` özelliği kullanım dışıdır.
> 
> MSAL 2. x ' den başlayarak, geliştiriciler `KeychainSecurityGroup` özelliğini kullandıklarında `TeamId` önekini dahil etmek üzere zorlandı. Ancak, MSAL 2.7. x öğesinden başlayarak, yeni `iOSKeychainSecurityGroup` özelliğini kullandığınızda MSAL, çalışma zamanı sırasında `TeamId` önekini çözümler. Bu özelliği kullandığınızda, değerde `TeamId` önekini eklemeyin. Ön ek gerekli değildir.
>
> `KeychainSecurityGroup` özelliği kullanılmıyor olduğundan, `iOSKeychainSecurityGroup` özelliğini kullanın.

### <a name="use-microsoft-authenticator"></a>Microsoft Authenticator kullan

Uygulamanız, etkinleştirmek için bir aracı olarak Microsoft Authenticator kullanabilir:

- **SSO**: SSO 'yu etkinleştirdiğinizde, kullanıcılarınızın her uygulamada oturum açması gerekmez.
- **Cihaz kimliği**: cihaz sertifikasına erişerek kimlik doğrulamak için cihaz kimliğini kullanın. Bu sertifika, çalışma alanına katıldığında cihazda oluşturulur. Kiracı yöneticileri cihazlarla ilgili koşullu erişimi etkinleştirmeleri durumunda uygulamanız hazırlanacaktır.
- **Uygulama tanımlama doğrulaması**: bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir. Aracı yeniden yönlendirme URL 'sini doğrular.

Bir aracısının nasıl etkinleştirileceği hakkında ayrıntılar için bkz. [Use Microsoft Authenticator veya Microsoft Intune şirket portalı Xamarin iOS ve Android uygulamalarında](msal-net-use-brokers-with-xamarin-apps.md).

## <a name="known-issues-with-ios-12-and-authentication"></a>İOS 12 ve kimlik doğrulama ile ilgili bilinen sorunlar
Microsoft, iOS 12 ve bazı kimlik doğrulama türleri arasında uyumsuzluk hakkında bir [Güvenlik Danışmanlığı](https://github.com/aspnet/AspNetCore/issues/4647) yayımlamıştır. Uyumsuzluk sosyal, Wsbeslenir ve OıDC oturum açma işlemlerini keser. Güvenlik Danışmanlığı, geliştiricilerin iOS 12 ile uyumlu hale getirmek için uygulamalarından ASP.NET güvenlik kısıtlamalarını nasıl kaldırabileceğini anlamalarına yardımcı olur.  

Xamarin iOS üzerinde MSAL.NET uygulamaları geliştirirken, iOS 12 ' den web sitelerinde oturum açmaya çalıştığınızda sonsuz bir döngüye karşılaşabilirsiniz. Bu davranış, bu [adal sorununa](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329)benzer. 

İOS 12 Safari ile ASP.NET Core OıDC kimlik doğrulamasında bir kesme de görebilirsiniz. Daha fazla bilgi için bu [WebKit sorununa](https://bugs.webkit.org/show_bug.cgi?id=188165)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Xamarin iOS özellikleri hakkında daha fazla bilgi için, aşağıdaki örneğin README.md dosyasının [iOS 'a özgü dikkat edilecek noktalar](https://github.com/Azure-Samples/active-directory-xamarin-native-v2/tree/master/1-Basic#ios-specific-considerations) paragrafına bakın:

Örnek | Platform | Açıklama
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, Evrensel Windows Platformu (UWP) | Azure AD 2,0 uç noktası aracılığıyla Microsoft kişisel hesaplarının ve Azure AD kimlik doğrulaması için MSAL kullanmayı gösteren basit bir Xamarin Forms uygulaması. Uygulama ayrıca Microsoft Graph erişmek için elde edilen belirtecin nasıl kullanılacağını gösterir.

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->