---
title: MSAL.NET'a göç etme
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile .NET (ADAL.NET) için Azure AD Kimlik Doğrulama Kitaplığı arasındaki farklar ve MSAL.NET nasıl geçirilir arasındaki farklar hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: f389943d284c573312473f426048f8aadb79088e
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81533981"
---
# <a name="migrating-applications-to-msalnet"></a>Uygulamaları MSAL.NET'a geçirme

Hem .NET için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.NET) hem de .NET için Azure AD Kimlik Doğrulama Kitaplığı (ADAL.NET) Azure AD varlıklarının kimliğini doğrulamak ve Azure AD'den belirteç istemek için kullanılır. Şimdiye kadar çoğu geliştirici, Azure AD Kimlik Doğrulama Kitaplığı 'nı (ADAL) kullanarak belirteçler isteyerek Azure AD kimliklerinin (iş ve okul hesapları) kimliğini doğrulamak için geliştiriciler platformu (v1.0) için Azure AD ile birlikte çalışmıştır. MSAL kullanma:

- Microsoft kimlik platformu bitiş noktasını kullandığından daha geniş bir Microsoft kimlik kümesini (Azure AD kimlikleri ve Microsoft hesapları ve Azure AD B2C aracılığıyla sosyal ve yerel hesaplar) doğrulayabilirsiniz,
- kullanıcılarınız en iyi tek oturum açma deneyimini yaşayacaktır.
- uygulamanız artımlı onay sağlayabilir ve Koşullu Erişimi desteklemek daha kolaydır
- yenilikten yararlanırsınız.

**MSAL.NET şimdi Microsoft kimlik platformu ile kullanmak için önerilen auth kitaplık .** ADAL.NET'da yeni özellikler uygulanmaz. Çalışmalar MSAL'ın iyileştirilmesi ne yöneliktir.

Bu makalede, .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile .NET (ADAL.NET) için Azure AD Kimlik Doğrulama Kitaplığı arasındaki farklar açıklanır ve MSAL'a geçiş yapmaya yardımcı olur.

## <a name="differences-between-adal-and-msal-apps"></a>ADAL ve MSAL uygulamaları arasındaki farklar

Çoğu durumda, MSAL.NET ve Microsoft kimlik doğrulama kitaplıklarının en son nesli olan Microsoft kimlik platformu bitiş noktasını kullanmak istiyorsunuz. MSAL.NET kullanarak, Azure AD (iş ve okul hesapları), Microsoft (kişisel) hesapları (MSA) veya Azure AD B2C ile uygulamanızda oturum açan kullanıcılar için belirteçler edinirsiniz.

Geliştiriciler için Azure REKLAM'ı (v1.0) bitiş noktası (ve ADAL.NET) zaten biliyorsanız, [Microsoft kimlik platformu (v2.0) bitiş noktası hakkında farklı olan](active-directory-v2-compare.md)nedir?

Ancak, uygulamanızın [Active Directory Federation Services 'in (ADFS)](/windows-server/identity/active-directory-federation-services)önceki sürümlerine sahip kullanıcılarda oturum açması gerekiyorsa, ADAL.NET kullanmanız gerekir. Daha fazla bilgi için [ADFS desteğine](https://aka.ms/msal-net-adfs-support)bakın.

Aşağıdaki resim, ADAL.NET ve MSAL.NET ![Yan yana kod arasındaki bazı farkları özetler](./media/msal-compare-msaldotnet-and-adaldotnet/differences.png)

### <a name="nuget-packages-and-namespaces"></a>NuGet paketleri ve İsim Boşlukları

ADAL.NET [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) NuGet paketinden tüketilir. kullanılacak ad alanı `Microsoft.IdentityModel.Clients.ActiveDirectory`.

MSAL.NET kullanmak için [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) NuGet paketini eklemeniz `Microsoft.Identity.Client` ve ad alanını kullanmanız gerekir

### <a name="scopes-not-resources"></a>Kapsamlar kaynak değil

ADAL.NET *kaynaklar*için belirteçleri satın aldı, ancak MSAL.NET *kapsamlar*için belirteçleri edinir. Bir dizi MSAL.NET EdinmeToken geçersiz kılar kapsamları`IEnumerable<string> scopes`olarak adlandırılan bir parametre gerektirir. Bu parametre, istenen izinleri ve kaynakları bildiren dizelerin basit bir listesidir. İyi bilinen kapsamlar [Microsoft Graph'ın kapsamlarıdır.](/graph/permissions-reference)

MSAL.NET v1.0 kaynaklarına erişmek de mümkündür. [V1.0 uygulaması için Kapsamlar'daki ayrıntılara](#scopes-for-a-web-api-accepting-v10-tokens)bakın.

### <a name="core-classes"></a>Çekirdek sınıflar

- ADAL.NET, Bir Kurum aracılığıyla Güvenlik Belirteç Hizmeti (STS) veya yetkilendirme sunucusuyla bağlantınızın temsili olarak [Kimlik Doğrulama Bağlamı'nı](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD) kullanır. Aksine, MSAL.NET [istemci uygulamaları](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications)etrafında tasarlanmıştır. İki ayrı sınıf `PublicClientApplication` sağlar: ve`ConfidentialClientApplication`

- Jeton Edinme: ADAL.NET ve MSAL.NET aynı kimlik`AcquireTokenAsync` doğrulama `AcquireTokenSilentAsync` çağrılarına `AcquireTokenInteractive` (ve ADAL.NET `AcquireTokenSilent` ve MSAL.NET) ancak farklı parametreler için gereklidir. Bir fark, MSAL.NET, artık her AcquireTokenXX aramauygulama geçmek zorunda `ClientID` olmasıdır. Nitekim, `ClientID` (veya)`IPublicClientApplication` `IConfidentialClientApplication`inşa ederken sadece bir kez ayarlanır.

### <a name="iaccount-not-iuser"></a>IAccount değil IUser

ADAL.NET manipüle edilmiş kullanıcılar. Ancak, bir kullanıcı bir insan veya yazılım aracısıdır, ancak Microsoft kimlik sistemindeki bir veya daha fazla hesaba (birkaç Azure AD hesabı, Azure AD B2C, Microsoft kişisel hesapları) sahip olabilir/sahip olabilir/sahip olabilir.

MSAL.NET 2.x artık Hesap kavramını (IAccount arabirimi üzerinden) tanımlar. Bu kesme değişikliği doğru anlambilimi sağlar: aynı kullanıcının farklı Azure REKLAM dizinlerinde birden fazla hesabı olması. Ayrıca MSAL.NET, ev hesabı bilgileri sağlandığı için konuk senaryolarında daha iyi bilgi sağlar.

IUser ve IAccount arasındaki farklar hakkında daha fazla bilgi için MSAL.NET [2.x'e](https://aka.ms/msal-net-2-released)bakın.

### <a name="exceptions"></a>Özel durumlar

#### <a name="interaction-required-exceptions"></a>Etkileşim gerekli özel durumlar

MSAL.NET daha açık istisnaları vardır. Örneğin, ADAL'da sessiz kimlik doğrulaması başarısız olduğunda yordam özel `user_interaction_required` durumu yakalamak ve hata kodunu aramaktır:

```csharp
catch(AdalException exception)
{
 if (exception.ErrorCode == "user_interaction_required")
 {
  try
  {“try to authenticate interactively”}}
 }
}
```

ADAL.NET içeren [bir belirteç elde etmek için önerilen desendeki](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token#recommended-pattern-to-acquire-a-token) ayrıntılara bakın

MSAL.NET kullanarak, `MsalUiRequiredException` [AcquireTokenSilent](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token)açıklandığı gibi yakalamak.

```csharp
catch(MsalUiRequiredException exception)
{
 try {“try to authenticate interactively”}
}
```

#### <a name="handling-claim-challenge-exceptions"></a>Talep meydan okuma özel durumlarını işleme

ADAL.NET, talep meydan okuma özel durumları aşağıdaki şekilde ele alınır:

- `AdalClaimChallengeException`bir kaynağın kullanıcıdan `AdalServiceException`daha fazla talep gerektirmesi durumunda (örneğin, iki faktörlü kimlik doğrulama) hizmet tarafından atılan bir özel durumdur (kaynaktan türeyen). Üye, `Claims` beklenen iddiaları ile bazı JSON parçası içerir.
- Hala ADAL.NET, bu özel durum alan kamu istemci `AcquireTokenInteractive` saki uygulaması bir talep parametresi olan geçersiz kılma aramak gerekir. Bu geçersiz `AcquireTokenInteractive` kılma, gerekli olmadığı için önbelleğe bile çarpmaya çalışmaz. Bunun nedeni, önbellekteki belirteçlerin doğru talepleri olmamasıdır `AdalClaimChallengeException` (aksi takdirde bir belirteç atılmazdı). Bu nedenle, önbelleğe bakmaya gerek yoktur. OBO `ClaimChallengeException` yapan bir WebAPI'de alınabileceğini, ancak `AcquireTokenInteractive` bu web API'yi çağıran bir kamu istemcisi uygulamasında çağrılması gerektiğini unutmayın.
- örnekler de dahil olmak üzere ayrıntılar için Bkz. [AdalClaimChallengeException](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Exceptions-in-ADAL.NET#handling-adalclaimchallengeexception)

MSAL.NET, talep itirazı özel durumları aşağıdaki şekilde işlenir:

- Bu `Claims` su yüzüne `MsalServiceException`çıktı.
- Oluşturucu `.WithClaim(claims)` için geçerli olabilecek bir yöntem vardır. `AcquireTokenInteractive`

### <a name="supported-grants"></a>Desteklenen hibeler

Tüm hibeler henüz MSAL.NET ve v2.0 bitiş noktasında desteklenir. Aşağıda ADAL.NET ve MSAL'ı karşılaştıran bir özet yer alıyor. NET'in desteklenen hibeleri.

#### <a name="public-client-applications"></a>Kamu müşteri başvuruları

İşte Masaüstü ve Mobil uygulamalar için ADAL.NET ve MSAL.NET desteklenen hibeler

İzin verme | ADAL.NET | MSAL.NET
----- |----- | -----
Etkileşimli | [İnteraktif Auth](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-interactively---Public-client-application-flows) | [MSAL.NET'da etkileşimli olarak belirteçleri edinme](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Acquiring-tokens-interactively)
Tümleşik Windows Kimlik Doğrulaması | [Windows'da entegre kimlik doğrulama (Kerberos)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-Integrated-authentication-on-Windows-(Kerberos)) | [Tümleşik Windows Kimlik Doğrulaması](msal-authentication-flows.md#integrated-windows-authentication)
Kullanıcı Adı / Şifre | [Kullanıcı adı ve parola içeren jeton edinme](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-username-and-password)| [Kullanıcı Adı Şifre Kimlik Doğrulama](msal-authentication-flows.md#usernamepassword)
Cihaz kodu akışı | [Web tarayıcısı olmayan cihazlar için aygıt profili](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Device-profile-for-devices-without-web-browsers) | [Cihaz Kodu akışı](msal-authentication-flows.md#device-code)

#### <a name="confidential-client-applications"></a>Gizli müşteri uygulamaları

Web uygulamaları, web API'leri ve daemon uygulamaları için ADAL.NET ve MSAL.NET desteklenen hibeler şunlardır:

Uygulama Türü | İzin verme | ADAL.NET | MSAL.NET
----- | ----- | ----- | -----
Web uygulaması, web API, daemon | İstemci Kimlik Bilgileri | [ADAL.NET istemci kimlik bilgileri akışları](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Client-credential-flows) | [MSAL.NET istemci kimlik bilgileri akışları](msal-authentication-flows.md#client-credentials))
Web API | Adına | [Hizmet ADAL.NET ile kullanıcı adına hizmet çağrıları](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Service-to-service-calls-on-behalf-of-the-user) | [MSAL.NET adına](msal-authentication-flows.md#on-behalf-of)
Web uygulaması | Auth Kodu | [Web uygulamalarında yetkilendirme kodları içeren jeton edinme ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-authorization-codes-on-web-apps) | [A MSAL.NET ile web uygulamalarında yetkilendirme kodları içeren jeton edinme](msal-authentication-flows.md#authorization-code)

### <a name="cache-persistence"></a>Önbellek kalıcılığı

ADAL.NET, güvenli bir `TokenCache` depolama (.NET Framework ve .NET core) `BeforeAccess` `BeforeWrite` olmadan platformlarda istenilen kalıcılık işlevini uygulamak için sınıfı genişletmenizi sağlar. Ayrıntılar için, [ADAL.NET'daki Token Önbellek Serileştirme'ye](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization)bakın.

MSAL.NET belirteç önbelleğini mühürlü bir sınıf haline getirir ve genişletme yeteneğini ortadan kaldırır. Bu nedenle, belirteç önbelleği kalıcılığı uygulamanız, mühürlü belirteç önbelleğiyle etkileşimedebilen bir yardımcı sınıf biçiminde olmalıdır. Bu etkileşim [MSAL.NET'da Token Önbellek Serileştirme'de](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/token-cache-serialization)açıklanmıştır.

## <a name="signification-of-the-common-authority"></a>Ortak otoritenin işaretlenmesi

v1.0'da, yetkiyi `https://login.microsoftonline.com/common` kullanırsanız, kullanıcıların herhangi bir AAD hesabıyla (herhangi bir kuruluş için) oturum açmalarına izin verirsiniz. [ADAL.NET'da Yetki Doğrulama'ya](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD#authority-validation) bakın

Yetkiyi `https://login.microsoftonline.com/common` v2.0'da kullanırsanız, kullanıcıların herhangi bir AAD kuruluşu veya Microsoft kişisel hesabı (MSA) ile oturum açmalarına izin verirsiniz. MSAL.NET olarak, herhangi bir AAD hesabına girişi kısıtlamak istiyorsanız (ADAL.NET ile `https://login.microsoftonline.com/organizations`aynı davranış), kullanmanız gerekir. Ayrıntılar `authority` [için, ortak istemci uygulamasındaki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#publicclientapplication)parametreye bakın.

## <a name="v10-and-v20-tokens"></a>v1.0 ve v2.0 belirteçleri

Belirteçlerin iki sürümü vardır:
- v1.0 belirteçleri
- v2.0 belirteçleri

V1.0 uç noktası (ADAL tarafından kullanılan) sadece v1.0 belirteçleri yayan.

Ancak, v2.0 bitiş noktası (MSAL tarafından kullanılan) web API kabul ettiği belirteç sürümünü yayan. Web API'sinin uygulama bildiriminin bir özelliği, geliştiricilerin belirteç sürümünün hangi sürümünün kabul edildiğini seçmelerine olanak tanır. Başvuru `accessTokenAcceptedVersion` [bildirimi](reference-app-manifest.md) başvuru belgelerine bakın.

v1.0 ve v2.0 belirteçleri hakkında daha fazla bilgi için [Azure Active Directory erişim belirteçleri](access-tokens.md)

## <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>V1.0 belirteçlerini kabul eden bir web API'si için kapsamlar

OAuth2 izinleri, v1.0 web API (kaynak) uygulamasının istemci uygulamalarına maruz kaktEttiği izin kapsamlarıdır. Bu izin kapsamları, onay sırasında istemci uygulamalarına verilebilir. [Azure Active Directory uygulama bildiriminde](active-directory-application-manifest.md)oauth2Permissions ile ilgili bölüme bakın.

### <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>V1.0 uygulamasının belirli OAuth2 izinlerine erişim talep etmek için kapsamlar

V1.0 belirteçlerini kabul eden bir uygulama için belirteçler elde etmek istiyorsanız https://graph.microsoft.com)(örneğin, bu `scopes` kaynak için istenen bir OAuth2 izniyle istediğiniz kaynak tanımlayıcısını daraltın olarak oluşturmanız gerekir.

Örneğin, app id URI olan bir v1.0 web API kullanıcı `ResourceId`adına erişmek için, kullanmak istiyorum:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

Microsoft Graph API'yi kullanarak MSAL.NET Azure Active Directoryhttps://graph.microsoft.com/)ile okumak ve yazmak istiyorsanız, aşağıdaki parçacıktaki gibi kapsamların bir listesini oluşturursunuz:

```csharp
ResourceId = "https://graph.microsoft.com/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

#### <a name="warning-should-you-have-one-or-two-slashes-in-the-scope-corresponding-to-a-v10-web-api"></a>Uyarı: V1.0 web API'sine karşılık gelen kapsamda bir veya iki kesik varsa

Azure Kaynak Yöneticisi API'sine karşılık gelen kapsamıhttps://management.core.windows.net/)yazmak istiyorsanız , aşağıdaki kapsamı istemeniz gerekir (iki eğrmeyi not edin)

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

Bunun nedeni, Kaynak Yöneticisi API'nin hedef`aud`kitle talebinde bir kesik beklemesi ve ardından API adını kapsamdan ayırmak için bir eğik çizgi olmasıdır.

Azure AD tarafından kullanılan mantık aşağıdaki gibidir:
- V1.0 erişim belirteci (mümkün olan tek), aud=kaynak ile ADAL (v1.0) uç noktası için
- MSAL (v2.0 bitiş noktası) için v2.0 belirteçlerini kabul eden bir kaynak için erişim belirteci istemek için, aud=kaynak. Appıd
- MSAL (v2.0 bitiş noktası) v1.0 erişim belirteci kabul eden bir kaynak için erişim belirteci soran (yukarıdaki durumda), Azure AD, son eğik çizgiden önce her şeyi alarak ve kaynak tanımlayıcısı olarak kullanarak istenen hedef kitleyi istenen kapsamdan ayrıştırır. Bu nedenle\/https: /database.windows.net birhttps://database.windows.net/hedef kitle bekliyorsa " ",\/https: /database.windows.net//.default kapsamını istemeniz gerekir. Ayrıca bkz: #[747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Kaynak url's sondaki eğik çizgi atlanır, hangi sql auth hatası neden #747


### <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>V1.0 uygulamasının tüm izinlerine erişim isteğinde bulunmak için kapsamlar

Örneğin, bir v1.0 uygulamasının tüm statik kapsamları için bir belirteç elde etmek istiyorsanız,

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="scopes-to-request-in-the-case-of-client-credential-flow--daemon-app"></a>İstenecek kapsamlar istemci kimlik bilgisi akışı / daemon uygulaması durumunda

İstemci kimlik bilgisi akışı söz konusu olduğunda, geçecek `/.default`kapsam da . Bu kapsam Azure AD'ye şunları söyler: "Yöneticinin uygulama kaydında onay verdiği tüm uygulama düzeyindeizinler.

## <a name="adal-to-msal-migration"></a>ADAL'dan MSAL göçüne

ADAL.NET v2.'de. X, yenileme belirteçleri, önbelleğe alarak ve ADAL 2.x tarafından sağlanan `AcquireTokenByRefreshToken` yöntemleri kullanarak bu belirteçlerin kullanımı etrafında çözümler geliştirmenize olanak sağlayan maruz kalmıştır.
Bu çözümlerden bazıları şu senaryolarda kullanılmıştır:
* Kullanıcılar adına yenileyici panolar da dahil olmak üzere eylemlerde bulunmak ta dahil olmak üzere uzun süren hizmetler, ancak kullanıcılar artık bağlı değildir.
* İstemcinin RT'yi web hizmetine getirmesini etkinleştirmek için WebFarm senaryoları (önbelleğe alma istemci tarafı, şifreli çerez değil, sunucu tarafı yapılır)

MSAL.NET, güvenlik nedenleriyle yenileme belirteçlerini ortaya çıkarmaz: MSAL sizin için yenileyici belirteçleri işler.

Neyse ki, MSAL.NET şimdi önceki yenileme belirteçleri (ADAL ile edinilen) içine `IConfidentialClientApplication`göç etmenizi sağlayan bir API vardır:

```csharp
/// <summary>
/// Acquires an access token from an existing refresh token and stores it and the refresh token into
/// the application user token cache, where it will be available for further AcquireTokenSilent calls.
/// This method can be used in migration to MSAL from ADAL v2 and in various integration
/// scenarios where you have a RefreshToken available.
/// (see https://aka.ms/msal-net-migration-adal2-msal2)
/// </summary>
/// <param name="scopes">Scope to request from the token endpoint.
/// Setting this to null or empty will request an access token, refresh token and ID token with default scopes</param>
/// <param name="refreshToken">The refresh token from ADAL 2.x</param>
IByRefreshToken.AcquireTokenByRefreshToken(IEnumerable<string> scopes, string refreshToken);
```

Bu yöntemle, daha önce kullanılan yenileme belirteci ile birlikte istediğiniz kapsamları (kaynaklar) sağlayabilirsiniz. Yenileme belirteci yeni bir belirteç için değiştirilir ve uygulamanız içine önbelleğe alınacaktır.

Bu yöntem tipik olmayan senaryolar için tasarlanmıştır, ilk döküm `IConfidentialClientApplication` olmadan kolayca erişilebilir `IByRefreshToken`değildir.

Bu kod snippet gizli bir istemci uygulamasında bazı geçiş kodu gösterir. `GetCachedRefreshTokenForSignedInUser`ADAL 2.x'ten yararlanmak için kullanılan uygulamanın önceki bir sürümü tarafından bazı depolama alanında depolanan yenileme belirteci'ni alın. `GetTokenCacheForSignedInUser`oturum açmış kullanıcı için bir önbellek deserialize (gizli istemci uygulamaları kullanıcı başına bir önbellek olmalıdır).

```csharp
TokenCache userCache = GetTokenCacheForSignedInUser();
string rt = GetCachedRefreshTokenForSignedInUser();

IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(clientId)
 .WithAuthority(Authority)
 .WithRedirectUri(RedirectUri)
 .WithClientSecret(ClientSecret)
 .Build();
IByRefreshToken appRt = app as IByRefreshToken;

AuthenticationResult result = await appRt.AcquireTokenByRefreshToken(null, rt)
                                         .ExecuteAsync()
                                         .ConfigureAwait(false);
```

Yeni yenileme belirteciniz önbellekte depolanırken, Kimlik Doğrulama Sonucunuzda bir erişim belirteci ve kimlik belirteci döndürülür görmezsiniz.

Bu yöntemi, kullanılabilir yenibir belirteç bulunan çeşitli tümleştirme senaryoları için de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Kapsamlar, izinler ve onay](v2-permissions-and-consent.md) kapsamları hakkında daha fazla bilgiyi Microsoft kimlik platformu bitiş noktasında bulabilirsiniz
