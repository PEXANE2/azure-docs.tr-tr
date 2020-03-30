---
title: ADAL - MSAL geçiş kılavuzu (MSAL iOS/macOS) | Azure
titleSuffix: Microsoft identity platform
description: iOS/macOS için MSAL ile ObjectiveC için Azure AD Kimlik Doğrulama Kitaplığı (ADAL) arasındaki farkları öğrenin. ObjC) ve iOS/macOS için MSAL'a nasıl geçirilir.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 6050bdc8c2600998b9804b04b62102e74612719f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085185"
---
# <a name="migrate-applications-to-msal-for-ios-and-macos"></a>uygulamaları iOS ve macOS için MSAL'a geçirin

Azure Active Directory Authentication Library[(ADAL Objective-C),](https://github.com/AzureAD/azure-activedirectory-library-for-objc)v1.0 bitiş noktası üzerinden Azure Active Directory hesaplarıyla çalışmak üzere oluşturulmuştur.

iOS ve macOS için Microsoft Kimlik Doğrulama Kitaplığı (MSAL), Azure Active Directory (Azure AD) hesapları, kişisel Microsoft hesapları ve Azure AD B2C hesapları gibi tüm Microsoft kimlikleriyle Çalışmak üzere oluşturulmuştür (resmi olarak). Azure AD v2.0 bitiş noktası).

Microsoft kimlik platformu, Azure Active Directory v1.0 ile birkaç önemli farka sahiptir. Bu makale, bu farklılıkları vurgular ve bir uygulamayı ADAL'dan MSAL'a geçirmek için kılavuz sağlar.

## <a name="adal-and-msal-app-capability-differences"></a>ADAL ve MSAL uygulama özelliği farklılıkları

### <a name="who-can-sign-in"></a>Kimler oturum açabilir

* ADAL yalnızca Azure AD hesapları olarak da bilinen iş ve okul hesaplarını destekler.
* MSAL, Hotmail.com, Outlook.com ve Live.com gibi kişisel Microsoft hesaplarını (MSA hesapları) destekler.
* MSAL iş ve okul hesaplarını ve Azure AD B2C hesaplarını destekler.

### <a name="standards-compliance"></a>Standartlara uygunluk

* Microsoft kimlik Platformu bitiş noktası, OAuth 2.0 ve OpenId Connect standartlarını izler.

### <a name="incremental-and-dynamic-consent"></a>Artımlı ve dinamik onay

* Azure Active Directory v1.0 bitiş noktası, başvuru kaydı sırasında tüm izinlerin önceden bildirilmesini gerektirir. Bu, bu izinlerin statik olduğu anlamına gelir.
* Microsoft kimlik platformu, izinleri dinamik olarak istemenize olanak tanır. Uygulamalar yalnızca gerektiği gibi izin isteyebilir ve uygulamanın ihtiyacı olduğu şekilde daha fazlasını isteyebilir.

Azure Active Directory v1.0 ile Microsoft kimlik platformu arasındaki farklar hakkında daha fazla [bilgi](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison)için bkz.

## <a name="adal-and-msal-library-differences"></a>ADAL ve MSAL kütüphane farklılıkları

MSAL genel API'si, Azure AD v1.0 ile Microsoft kimlik platformu arasındaki birkaç önemli farkı yansıtır.

### <a name="msalpublicclientapplication-instead-of-adauthenticationcontext"></a>ADAuthenticationContext yerine MSALPublicClientApplication

`ADAuthenticationContext`bir ADAL uygulamasının oluşturduğu ilk nesnedir. ADAL'ın anlık bir anını temsil eder. Uygulamalar, her Azure `ADAuthenticationContext` Etkin Dizin bulutu ve kiracı (yetki) birleşimi için yeni bir örnek oluşturur. Aynı `ADAuthenticationContext` birden çok ortak istemci uygulamaları için belirteçleri almak için kullanılabilir.

MSAL'da ana etkileşim, `MSALPublicClientApplication` [OAuth 2.0 Public Client'dan](https://tools.ietf.org/html/rfc6749#section-2.1)sonra modellenen bir nesne aracılığıyla dır. Bir örneği, her bir yetki için yeni bir örnek oluşturmaya gerek kalmadan birden çok AAD bulutu ve kiracıyla etkileşim kurmak için `MSALPublicClientApplication` kullanılabilir. Çoğu uygulama için `MSALPublicClientApplication` bir örnek yeterlidir.

### <a name="scopes-instead-of-resources"></a>Kaynaklar yerine kapsamlar

ADAL'da, bir uygulamanın *resource* Azure Active Directory `https://graph.microsoft.com` v1.0 bitiş noktasından belirteçler edinmegibi bir kaynak tanımlayıcısı sağlaması gerekiyordu. Bir kaynak, anladığı kapsamları veya oAuth2Permissions uygulama bildiriminde bir dizi tanımlayabilirsiniz. Bu, istemci uygulamalarının uygulama kaydı sırasında önceden tanımlanmış belirli kapsamlar kümesi için bu kaynaktan belirteçler istemesine olanak tanır.

MSAL'da, uygulamalar istek başına bir dizi kapsam sağlar. Kapsam, kaynak/izin formunda bir izin adı izleyen bir kaynak tanımlayıcısIdır. Örneğin, `https://graph.microsoft.com/user.read`

MSAL'da kapsam sağlamanın iki yolu vardır:

* Uygulamalarınızın ihtiyaç duyduğu tüm izinlerin bir listesini sağlayın. Örnek: 

    `@[@"https://graph.microsoft.com/directory.read", @"https://graph.microsoft.com/directory.write"]`

    Bu durumda, uygulama ve `directory.read` `directory.write` izinleri ister. Kullanıcıdan, bu uygulama için daha önce izin vermedikleri takdirde bu izinleri kabul etmesi istenir. Uygulama, kullanıcının uygulama için önceden onayladığı ek izinler de alabilir. Kullanıcıdan yalnızca yeni izinler veya verilmeyen izinler için onay vermesi istenir.

* `/.default` Kapsam.

Bu, her uygulama için yerleşik kapsamdır. Uygulama kaydedildiğinde yapılandırılan izinlerin statik listesini ifade eder. Davranışları, `resource`. Bu, benzer kapsamlar ve kullanıcı deneyimi kümesinin korunduğundan emin olmak için geçiş yaparken yararlı olabilir.

`/.default` Kapsamı kullanmak için kaynak `/.default` tanımlayıcısına ek. Örneğin: `https://graph.microsoft.com/.default`. Kaynağınız bir eğik`/`çizgi ile sona `/.default`ererse ( ), yine de, önde gelen ileri eğik çizgi de dahil olmak üzere, içinde çift ileri eğik çizgi ()`//`olan bir kapsam sonuçlanan ekgerekir.

Burada "/.default" [kapsamını](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#the-default-scope) kullanma hakkında daha fazla bilgi okuyabilirsiniz

### <a name="supporting-different-webview-types--browsers"></a>Tarayıcılar & farklı Web View türlerini destekleme

ADAL yalnızca iOS için UIWebView/WKWebView'ı ve macOS için WebView'ı destekler. iOS için MSAL, yetkilendirme kodu talep ederken web içeriğini görüntülemek için `UIWebView`daha fazla seçeneği destekler ve artık desteklemez; hangi kullanıcı deneyimi ve güvenliğini artırabilir.

Varsayılan olarak, iOS'taki MSAL, Apple'ın iOS 12+ cihazlarda kimlik doğrulama için önerdiği web bileşeni olan [ASWebAuthenticationSession'ı](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession?language=objc)kullanır. Uygulamalar ve Safari tarayıcısı arasında çerez paylaşımı yoluyla Tek Oturum Açma (SSO) avantajları sağlar.

Uygulama gereksinimlerine ve istediğiniz son kullanıcı deneyimine bağlı olarak farklı bir web bileşeni kullanmayı seçebilirsiniz. Daha fazla seçenek için [desteklenen web görünümü türlerine](customize-webviews.md) bakın.

ADAL'dan MSAL'a `WKWebView` geçiş yaparken, iOS ve macOS'ta ADAL'a en çok benzer kullanıcı deneyimini sağlar. Mümkünse iOS'a `ASWebAuthenticationSession` geçmenizi öneririz. macOS için, kullanmanızı `WKWebView`öneririz.

### <a name="account-management-api-differences"></a>Hesap yönetimi API farkları

ADAL `acquireToken()` yöntemlerini aradiğinizde `acquireTokenSilent()`veya , `ADUserInformation` kimlik doğrulaması yapılan `id_token` hesabı temsil eden hesaptan bir talep listesi içeren bir nesne alırsınız. Ayrıca, `ADUserInformation` `upn` iddiaya `userId` dayalı bir döndürür. İlk etkileşimli belirteç ediniminden sonra, ADAL geliştiricinin tüm sessiz aramaları sağlamasını `userId` bekler.

ADAL, bilinen kullanıcı kimliklerini almak için api sağlamaz. Bu hesapları kaydetmek ve yönetmek için uygulamaya güvenir.

MSAL, bir belirteç elde etmek zorunda kalmadan MSAL tarafından bilinen tüm hesapları listelemek için bir dizi API sağlar.

ADAL gibi, MSAL da .'dan gelen `id_token`taleplerin listesini tutan hesap bilgilerini döndürür. Nesnenin içindeki nesnenin `MSALAccount` bir `MSALResult` parçası.

MSAL, kaldırılan hesapları uygulama için erişilemez hale getirerek hesapları kaldırmak için bir dizi API sağlar. Hesap kaldırıldıktan sonra, daha sonra belirteç edinme çağrıları kullanıcıdan etkileşimli belirteç edinimi yapmasını ister. Hesap kaldırma işlemi yalnızca onu başlatan istemci uygulaması için geçerlidir ve hesabı aygıtta veya sistem tarayıcısından çalıştıran diğer uygulamalardan kaldırmaz. Bu, kullanıcının tek bir uygulamadan çıkış yaptıktan sonra bile cihazda SSO deneyimi yaşamaya devam etmesini sağlar.

Ayrıca, MSAL daha sonra sessizce bir belirteç istemek için kullanılabilecek bir hesap tanımlayıcısı döndürür. Ancak, hesap tanımlayıcısı `identifier` `MSALAccount` (nesnedeki özellik aracılığıyla erişilebilir) görüntülenemez ve ne biçimolduğunu varsayamaz ve yorumlamaya veya ayrıştırmaya çalışamazsınız.

### <a name="migrating-the-account-cache"></a>Hesap önbelleğini geçirme

ADAL'dan göç ederken, uygulamalar normalde MSAL tarafından `userId` `identifier` gerekli olmayan ADAL's, depolar. Bir kerelik geçiş adımı olarak, bir uygulama ADAL'ın userId'ini kullanarak bir MSAL hesabını aşağıdaki API ile sorgulayabilir:

`- (nullable MSALAccount *)accountForUsername:(nonnull NSString *)username error:(NSError * _Nullable __autoreleasing * _Nullable)error;`

Bu API, Hesabı ADAL userId (UPN) tarafından bulmak için hem MSAL hem de ADAL önbelleğini okur.

Hesap bulunursa, geliştirici sessiz belirteç edinimi yapmak için hesabı kullanmalıdır. İlk sessiz belirteç edinimi hesabı etkili bir şekilde yükseltecek ve geliştirici MSAL sonucu bir`identifier`MSAL uyumlu hesap tanımlayıcısı alacak ( ). Bundan sonra, `identifier` yalnızca aşağıdaki API kullanılarak hesap aramaları için kullanılmalıdır:

`- (nullable MSALAccount *)accountForIdentifier:(nonnull NSString *)identifier error:(NSError * _Nullable __autoreleasing * _Nullable)error;`

ADAL's'ı `userId` MSAL'daki tüm işlemler için kullanmaya `userId` devam etmek mümkün olsa da, UPN'ye dayandığı için, kötü bir kullanıcı deneyimiyle sonuçlanan birden çok sınırlamaya tabidir. Örneğin, UPN değişirse, kullanıcının yeniden oturum açması gerekir. Tüm uygulamaların tüm işlemler için görüntülenemeyen hesabı `identifier` kullanmasını öneririz.

[Önbellek durumu geçişi](sso-between-adal-msal-apps-macos-ios.md)hakkında daha fazla bilgi edinin.

### <a name="token-acquisition-changes"></a>Belirteç edinim değişiklikleri

MSAL bazı belirteç edinme çağrı değişiklikleri sunar:

* ADAL gibi, `acquireTokenSilent` her zaman sessiz bir istekle sonuçlanır.
* ADAL'ın `acquireToken` aksine, web görünümü veya Microsoft Authenticator uygulaması aracılığıyla her zaman kullanıcı tarafından işlem işlenebilir kullanıcı kullanıcı arama sistemi yle sonuçlanır. Web view/Microsoft Authenticator içindeki SSO durumuna bağlı olarak, kullanıcıdan kimlik bilgilerini girmeleri istenebilir.
* `acquireToken` ADAL'da, `AD_PROMPT_AUTO` ilk olarak sessiz belirteç edinimi çalışır ve yalnızca sessiz istek başarısız olursa UI'yi gösterir. MSAL'da bu mantık ilk arama `acquireTokenSilent` ve `acquireToken` yalnızca sessiz satın alma başarısız olduğunda çağrılayarak elde edilebilir. Bu, geliştiricilerin etkileşimli belirteç edinimi başlatmadan önce kullanıcı deneyimini özelleştirmelerine olanak tanır.

### <a name="error-handling-differences"></a>Hata işleme farklılıkları

MSAL, uygulamanız tarafından işlenebilen hatalarla kullanıcıtarafından müdahale gerektiren hatalar arasında daha fazla netlik sağlar. Geliştiricinin işlemesi gereken sınırlı sayıda hata vardır:

* `MSALErrorInteractionRequired`: Kullanıcı etkileşimli bir istekte bulunmalıdır. Bunun nedeni, süresi dolmuş kimlik doğrulama oturumu, Koşullu Erişim ilkesinin değişmesi, yenileme belirteci süresi dolmuş veya iptal edilmiş, önbellekte geçerli bir belirteç bulunmadığı gibi çeşitli nedenlerle ortaya çıkabilir.
* `MSALErrorServerDeclinedScopes`: İstek tam olarak tamamlanmadı ve bazı kapsamlara erişim izni verilmedi. Bunun nedeni, bir veya daha fazla kapsamda izin alan bir kullanıcının neden olabilir.

[ `MSALError` Listedeki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALError.h#L128) diğer tüm hataları işlemek isteğe bağlıdır. Kullanıcı deneyimini geliştirmek için bu hatalardaki bilgileri kullanabilirsiniz.

Bkz. MSAL hata işleme hakkında daha fazla şey için [MSAL'ı kullanarak özel durumlar ve hatalar](msal-handling-exceptions.md) işleme.

### <a name="broker-support"></a>Broker desteği

MSAL, sürüm 0.3.0 ile başlayarak, Microsoft Authenticator uygulamasını kullanarak aracılı kimlik doğrulama desteği sağlar. Microsoft Authenticator ayrıca Koşullu Erişim senaryoları için destek sağlar. Koşullu Erişim senaryolarına örnek olarak, kullanıcının cihazı Intune üzerinden kaydetmesini veya bir belirteç almak için AAD'ye kaydolmasını gerektiren aygıt uyumluluk ilkeleri verilebilir. Ve Uygulamanızın bir belirteç alabilmeleri için uyumluluk kanıtı gerektiren Mobil Uygulama Yönetimi (MAM) Koşullu Erişim ilkeleri.

Uygulamanız için aracıyı etkinleştirmek için:

1. Uygulama için broker uyumlu yeniden yönlendirme URI biçimini kaydedin. Broker uyumlu yeniden yönlendirme `msauth.<app.bundle.id>://auth`URI biçimidir. Başvurunuzun paket kimliğiyle değiştirin. `<app.bundle.id>` ADAL'den göç ediyorsanız ve başvurunuz zaten broker yeteneğine sahipse, yapmanız gereken ekstra bir şey yoktur. Önceki yeniden yönlendirme URI MSAL ile tam uyumludur, böylece adım 3 atlayabilirsiniz.

2. Uygulamanızın yeniden yönlendirme URI düzenini info.plist dosyanıza ekleyin. Varsayılan MSAL yeniden uri için, `msauth.<app.bundle.id>`biçimi. Örnek:

    ```xml
    <key>CFBundleURLSchemes</key>
    <array>
        <string>msauth.<app.bundle.id></string>
    </array>
    ```

3. LSApplicationQueriesSchemes altında uygulamanızın Info.plist'ine aşağıdaki şemaları ekleyin:

    ```xml
    <key>LSApplicationQueriesSchemes</key>
    <array>
         <string>msauthv2</string>
         <string>msauthv3</string>
    </array>
    ```

4. Geri aramaları işlemek için AppDelegate.m dosyanıza aşağıdakileri ekleyin: Objective-C:
    
    ```objc
    - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options`
    {
        return [MSALPublicClientApplication handleMSALResponse:url sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
    }
    ```
    
    Swift:
    
    ```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
    }
    ```

### <a name="business-to-business-b2b"></a>İşletmeden işletmeye (B2B)

ADAL'de, uygulamanın `ADAuthenticationContext` belirteçlerini talep ettiği her kiracı için ayrı örnekler oluşturursunuz. Bu artık MSAL'da bir gereklilik değildir. MSAL'da, Token `MSALPublicClientApplication` ve satın alma TokenSilent aramaları için farklı bir ykurum belirterek, herhangi bir AAD bulutu ve kuruluşu için tek bir örnek oluşturabilir ve kullanabilirsiniz.

## <a name="sso-in-partnership-with-other-sdks"></a>SSO diğer SDK'larla işbirliği içinde

iOS için MSAL, Aşağıdaki SDK'larla birleşik bir önbellek aracılığıyla SSO'ya ulaşabilir:

- ADAL Hedefi-C 2.7.x+
- MSAL.NET için Xamarin 2.4.x+
- Xamarin 4.4.x+ için ADAL.NET

SSO, iOS anahtarzinciri paylaşımı yoluyla sağlanır ve yalnızca aynı Apple Developer hesabından yayınlanan uygulamalar arasında kullanılabilir.

IOS anahtarlık paylaşımı yoluyla SSO sadece sessiz SSO türüdür.

macOS'ta MSAL, iOS ve macOS tabanlı uygulamalar ve ADAL Objective-C tabanlı uygulamalar için diğer MSAL ile SSO'ya ulaşabilir.

iOS'taki MSAL, diğer iki SSO türünü de destekler:

* Web tarayıcısı üzerinden SSO. iOS için MSAL, aygıttaki diğer uygulamalar ve özellikle Safari tarayıcısı arasında paylaşılan çerezler aracılığıyla SSO sağlayan destekler. `ASWebAuthenticationSession`
* Bir Kimlik Doğrulama aracısı aracılığıyla SSO. Bir iOS aygıtında, Microsoft Authenticator Kimlik Doğrulama aracısı olarak görev eder. Uyumlu bir aygıt gerektirmek gibi Koşullu Erişim ilkelerini izleyebilir ve kayıtlı aygıtlar için SSO sağlar. 0.3.0 sürümüyle başlayan MSAL SDK'lar varsayılan olarak bir aracıyı destekler.

## <a name="intune-mam-sdk"></a>Intune MAM SDK

[Intune MAM SDK,](https://docs.microsoft.com/intune/app-sdk-get-started) [sürüm 11.1.2](https://github.com/msintuneappsdk/ms-intune-app-sdk-ios/releases/tag/11.1.2) ile başlayan iOS için MSAL'ı destekler

## <a name="msal-and-adal-in-the-same-app"></a>Aynı uygulamada MSAL ve ADAL

ADAL sürüm 2.7.0 ve üzeri, aynı uygulamada MSAL ile bir arada bulunamaz. Ana nedeni paylaşılan alt modül ortak kodu nedeniyle. Objective-C ad alanlarını desteklemediği için, uygulamanıza hem ADAL hem de MSAL çerçeveleri eklerseniz, aynı sınıfın iki örneği olacaktır. Çalışma zamanında hangisinin seçilen garantisi yok. Her iki SDK çakışan sınıfın aynı sürümünü kullanıyorsa, uygulamanız çalışmaya devam edebilir. Ancak, farklı bir sürümse, uygulamanız tanılaması zor beklenmeyen çökmelerle karşılaşabilir.

ADAL ve MSAL'ın aynı üretim uygulamasında çalıştırılması desteklenmez. Ancak, kullanıcılarınızı iOS ve macOS için ADAL Objective-C'den MSAL'a sadece test ediyor ve geçiriyorsanız, [ADAL Objective-C 2.6.10'u](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases/tag/2.6.10)kullanmaya devam edebilirsiniz. Aynı uygulamada MSAL ile çalışan tek sürüm. Bu ADAL sürümü için yeni özellik güncelleştirmeleri olmayacaktır, bu nedenle yalnızca geçiş ve sınama amacıyla kullanılmalıdır. Uygulamanız ADAL ve MSAL birlikteliği uzun vadeli güvenmemelidir.

ADAL ve MSAL aynı uygulamada birlikte yaşam desteklenmez.
Birden fazla uygulama arasında ADAL ve MSAL birlikteliği tam olarak desteklenir.

## <a name="practical-migration-steps"></a>Pratik geçiş adımları

### <a name="app-registration-migration"></a>Uygulama kaydı geçişi

MSAL'a geçmek ve AAD hesaplarını etkinleştirmek için mevcut AAD uygulamanızı değiştirmeniz gerekmez. Ancak, ADAL tabanlı uygulamanız aracılı kimlik doğrulamasını desteklemiyorsa, MSAL'a geçebilmeniz için uygulama için yeni bir yeniden yönlendirme URI kaydetmeniz gerekir.

Uri yönlendirme bu biçimde olmalıdır: `msauth.<app.bundle.id>://auth`. Başvurunuzun paket kimliğiyle değiştirin. `<app.bundle.id>` [Azure portalında](https://aka.ms/MobileAppReg)URI'yi yeniden yönlendirmeyi belirtin.

Yalnızca iOS için, sertifika tabanlı kimlik doğrulamasını desteklemek için, uygulamanızda ve Azure portalında aşağıdaki biçimde ek bir yeniden yönlendirme URI'nin kaydedilmesi gerekir: `msauth://code/<broker-redirect-uri-in-url-encoded-form>`. Örneğin, `msauth://code/msauth.com.microsoft.mybundleId%3A%2F%2Fauth`

Tüm uygulamaların hem yeniden yönlendirme URI'leri kaydetmesini öneririz.

Artımlı onay için destek eklemek istiyorsanız, **API izinleri** sekmesi altında uygulama kaydınızda erişim isteğinde bulunmak üzere uygulamanızın yapılandırıldığı API'leri ve izinleri seçin.

ADAL'den geçiş yapıyorsunuz ve hem AAD hem de MSA hesaplarını desteklemek istiyorsanız, her ikisini de desteklemek için varolan uygulama kaydınızın güncellenmesi gerekir. Hem AAD'yi hem de MSA'yı hemen desteklemek için mevcut üretim uygulamanızı güncellemenizi önermiyoruz. Bunun yerine, hem AAD hem de MSA'yı sınama için destekleyen ve tüm senaryoların çalıştığını doğruladıktan sonra varolan uygulamayı güncelleştiren başka bir istemci kimliği oluşturun.

### <a name="add-msal-to-your-app"></a>Uygulamanıza MSAL ekleyin

Tercih ettiğiniz paket yönetim aracını kullanarak uygulamanıza MSAL SDK ekleyebilirsiniz. [Burada ayrıntılı talimatlara](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Installation)bakın.

### <a name="update-your-apps-infoplist-file"></a>Uygulamanızın Info.plist dosyalarını güncelleyin

Yalnızca iOS için, uygulamanızın yeniden yönlendirme URI düzenini info.plist dosyanıza ekleyin. ADAL broker uyumlu uygulamalar için, zaten orada olmalıdır. Varsayılan MSAL yeniden yönlendirme URI düzeni biçiminde `msauth.<app.bundle.id>`olacaktır: .  

```xml
<key>CFBundleURLSchemes</key>
<array>
    <string>msauth.<app.bundle.id></string>
</array>
```

Uygulamanızın Info.plist'ine `LSApplicationQueriesSchemes`aşağıdaki şemaları ekleyin.

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauthv2</string>
     <string>msauthv3</string>
</array>
```

### <a name="update-your-appdelegate-code"></a>AppDelegate kodunuzu güncelleştirin

Yalnızca iOS için AppDelegate.m dosyanıza aşağıdakileri ekleyin:

Amaç-C:

```objc
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options`
{
    return [MSALPublicClientApplication handleMSALResponse:url sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

Swift:

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}
```

**Xcode 11 kullanıyorsanız,** bunun yerine dosyaya `SceneDelegate` MSAL geri arama yerleştirmelisiniz.
Eski iOS ile uyumluluk için hem UISceneDelegate'ı hem de UIApplicationDelegate'i destekliyorsanız, MSAL geri aramanın her iki dosyaya da yerleştirilmesi gerekir.

Amaç-C:

```objc
 - (void)scene:(UIScene *)scene openURLContexts:(NSSet<UIOpenURLContext *> *)URLContexts
 {
     UIOpenURLContext *context = URLContexts.anyObject;
     NSURL *url = context.URL;
     NSString *sourceApplication = context.options.sourceApplication;
     
     [MSALPublicClientApplication handleMSALResponse:url sourceApplication:sourceApplication];
 }
```

Swift:

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        
        guard let urlContext = URLContexts.first else {
            return
        }
        
        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication
        
        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
```

Bu, MSAL'ın broker ve web bileşeninden gelen yanıtları işlemesine olanak tanır.
Bu otomatik olarak "swizzled" uygulama temsilci yöntemleri beri ADAL gerekli değildi. El ile eklemek daha az hata yatkındır ve uygulamaya daha fazla denetim sağlar.

### <a name="enable-token-caching"></a>Belirteç önbelleğe alma etkinleştirme

Varsayılan olarak, MSAL uygulamanızın belirteçlerini iOS veya macOS anahtar zincirinde önbelleğe alır. 

Belirteç önbelleğe alma etkinleştirmek için:
1. Uygulamanızın düzgün imzalandığından emin olun
2. Xcode Proje Ayarlarınıza gidin > **Özellikler sekmesi** > **Anahtar Zinciri Paylaşımını Etkinleştir**
3. Aşağıdaki **+** **Anahtarlık Grupları** girişini tıklatın ve girin: 3.a iOS için, macOS için 3.b girin `com.microsoft.adalcache``com.microsoft.identity.universalstorage`

### <a name="create-msalpublicclientapplication-and-switch-to-its-acquiretoken-and-acquiretokesilent-calls"></a>MSALPublicClientApplication oluşturun ve onun satın Token geçmek ve TokeSilent aramaları satın

Aşağıdaki kodu `MSALPublicClientApplication` kullanarak oluşturabilirsiniz:

Amaç-C:

```objc
NSError *error = nil;
MSALPublicClientApplicationConfig *configuration = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];
    
MSALPublicClientApplication *application =
[[MSALPublicClientApplication alloc] initWithConfiguration:configuration
                                                     error:&error];
```

Swift:

```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
do {
  let application = try MSALPublicClientApplication(configuration: config)
  // continue on with application
            
} catch let error as NSError {
  // handle error here
}
```

Ardından önbellekte hesap olup olmadığını görmek için hesap yönetimi API'sini arayın:

Amaç-C:

```objc
NSString *accountIdentifier = nil /*previously saved MSAL account identifier */;
NSError *error = nil;
MSALAccount *account = [application accountForIdentifier:accountIdentifier error:&error];
```

Swift:

```swift
// definitions that need to be initialized
let application: MSALPublicClientApplication!
let accountIdentifier: String! /*previously saved MSAL account identifier */

do {
  let account = try application.account(forIdentifier: accountIdentifier)
  // continue with account usage
} catch let error as NSError {
  // handle error here
}
```



veya tüm hesapları okuyun:

Amaç-C:

```objc
NSError *error = nil;
NSArray<MSALAccount *> *accounts = [application allAccounts:&error];
```

Swift:

```swift
let application: MSALPublicClientApplication!
do {
  let accounts = try application.allAccounts()
  // continue with account usage
} catch let error as NSError {
  // handle error here
}
```



Bir hesap bulunursa, MSAL `acquireTokenSilent` API'yi arayın:

Amaç-C:

```objc
MSALSilentTokenParameters *silentParameters = [[MSALSilentTokenParameters alloc] initWithScopes:@[@"<your-resource-here>/.default"] account:account];
    
[application acquireTokenSilentWithParameters:silentParameters
                              completionBlock:^(MSALResult *result, NSError *error)
{
    if (result)
    {
        NSString *accessToken = result.accessToken;
        // Use your token
    }
    else
    {
        // Check the error
        if ([error.domain isEqual:MSALErrorDomain] && error.code == MSALErrorInteractionRequired)
        {
            // Interactive auth will be required
        }
            
        // Other errors may require trying again later, or reporting authentication problems to the user
    }
}];
```

Swift:

```swift
let application: MSALPublicClientApplication!
let account: MSALAccount!
        
let silentParameters = MSALSilentTokenParameters(scopes: ["<your-resource-here>/.default"], 
                                                 account: account)
application.acquireTokenSilent(with: silentParameters) {
  (result: MSALResult?, error: Error?) in
  if let accessToken = result?.accessToken {
     // use accessToken
  }
  else {
    // Check the error
    guard let error = error else {
      assert(true, "callback should contain a valid result or error")
      return
    }
    
    let nsError = error as NSError
    if (nsError.domain == MSALErrorDomain
        && nsError.code == MSALError.interactionRequired.rawValue) {
      // Interactive auth will be required
    }
                
    // Other errors may require trying again later, or reporting authentication problems to the user
  }
}
```



## <a name="next-steps"></a>Sonraki adımlar

[Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin
