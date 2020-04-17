---
title: Azure AD B2C (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile Azure AD B2C kullanırken belirli hususlar hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2019
ms.author: jeferrie
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: d31cf3a4e024dc59b865d096cbd0829d50f61a1a
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81533964"
---
# <a name="use-msalnet-to-sign-in-users-with-social-identities"></a>Sosyal kimliklere sahip kullanıcılarda oturum MSAL.NET kullanın

[Azure Active Directory B2C (Azure AD B2C)](https://aka.ms/aadb2c)kullanarak sosyal kimliklere sahip kullanıcılarda oturum açma MSAL.NET kullanabilirsiniz. Azure AD B2C, ilkeler kavramı etrafında oluşturulmuştür. MSAL.NET, bir ilke belirterek bir yetki sağlamak için çevirir.

- Kamu istemcisi başvurusunu anında yaptığınızda, yetkili ilkeyi belirtmeniz gerekir.
- Bir ilke uygulamak istediğinizde, bir `AcquireTokenInteractive` `authority` parametre içeren bir geçersiz kılma çağırmanız gerekir.

Bu sayfa MSAL 3.x içindir. MSAL 2.x ile ilgileniyorsanız, lütfen [MSAL 2.x'teki Azure AD B2C özelliklerine](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-Specifics-MSAL-2.x)bakın.

## <a name="authority-for-a-azure-ad-b2c-tenant-and-policy"></a>Azure AD B2C kiracıve ilke için yetki

Kullanma yetkisi aşağıdaki `https://{azureADB2CHostname}/tfp/{tenant}/{policyName}` leri

- `azureADB2CHostname`Azure AD B2C kiracısının ve ana bilgisayar `{your-tenant-name}.b2clogin.com`sahibinin adıdır (örneğin),
- `tenant`Azure AD B2C kiracısının (örneğin, `{your-tenant-name}.onmicrosoft.com`) veya kiracı için GUID'in tam adıdır,
- `policyName`uygulanacak ilke veya kullanıcı akışının adı (örneğin kaydolma/kaydolmak için "b2c_1_susi" ).

Azure AD B2C yetkilileri hakkında daha fazla bilgi için bu [belgelere](/azure/active-directory-b2c/b2clogin)bakın.

## <a name="instantiating-the-application"></a>Uygulamanın anında

Uygulamayı yaparken, yetkiyi sağlamanız gerekir.

```csharp
// Azure AD B2C Coordinates
public static string Tenant = "fabrikamb2c.onmicrosoft.com";
public static string AzureADB2CHostname = "fabrikamb2c.b2clogin.com";
public static string ClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
public static string PolicySignUpSignIn = "b2c_1_susi";
public static string PolicyEditProfile = "b2c_1_edit_profile";
public static string PolicyResetPassword = "b2c_1_reset";

public static string AuthorityBase = $"https://{AzureADB2CHostname}/tfp/{Tenant}/";
public static string Authority = $"{AuthorityBase}{PolicySignUpSignIn}";
public static string AuthorityEditProfile = $"{AuthorityBase}{PolicyEditProfile}";
public static string AuthorityPasswordReset = $"{AuthorityBase}{PolicyResetPassword}";

application = PublicClientApplicationBuilder.Create(ClientID)
               .WithB2CAuthority(Authority)
               .Build();
```

## <a name="acquire-a-token-to-apply-a-policy"></a>İlke uygulamak için bir belirteç edinme

Bir kamu istemcisi uygulamasında Azure AD B2C korumalı bir API için belirteç elde etmek, geçersiz kılmaları bir yetkiliyle kullanmanızı gerektirir:

```csharp
IEnumerable<IAccount> accounts = await application.GetAccountsAsync();
AuthenticationResult ar = await application .AcquireTokenInteractive(scopes)
                                            .WithAccount(GetAccountByPolicy(accounts, policy))
                                            .WithParentActivityOrWindow(ParentActivityOrWindow)
                                            .ExecuteAsync();
```

Yeni değer:

- `policy`önceki dizeleri (örneğin) `PolicySignUpSignIn`biri olmak.
- `ParentActivityOrWindow`Android (Etkinlik) için gereklidir ve windows'daki windows ve iOS'taki UIViewController gibi üst ui'yi destekleyen diğer platformlar için isteğe bağlıdır. Kullanıcı Bilgisi iletişim kutusu hakkında daha fazla bilgi için [bkz.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Acquiring-tokens-interactively#withparentactivityorwindow)
- `GetAccountByPolicy(IEnumerable<IAccount>, string)`belirli bir ilke için hesap bulan bir yöntemdir. Örneğin:

  ```csharp
  private IAccount GetAccountByPolicy(IEnumerable<IAccount> accounts, string policy)
  {
   foreach (var account in accounts)
   {
    string userIdentifier = account.HomeAccountId.ObjectId.Split('.')[0];
    if (userIdentifier.EndsWith(policy.ToLower()))
     return account;
   }
   return null;
  }
  ```

Bir ilke veya kullanıcı akışı uygulama (örneğin, son kullanıcının profilini düzenlemelerine veya parolasını sıfırlamalarına izin vermek) şu anda . `AcquireTokenInteractive` Bu iki ilke söz konusu olduğunda, döndürülen belirteç / kimlik doğrulama sonucunu kullanmazsınız.

## <a name="special-case-of-editprofile-and-resetpassword-policies"></a>EditProfile ve ResetPassword ilkelerinin özel durumu

Son kullanıcılarınızın sosyal bir kimlikle oturum açarak profillerini yeniden yapılandırdığı bir deneyim sağlamak ve ardından profillerini yeniden oluşturmak istediğinizde, Azure AD B2C Profili Edit ilkesini uygulamak istersiniz. Bunu yapmanın yolu, bu `AcquireTokenInteractive` ilke için belirli bir yetkiliyle `Prompt.NoPrompt` ve hesap seçimi iletişim kutusunun görüntülenmesini önlemek için bir Istem kümesini arayarak (kullanıcı zaten oturum açmış ve etkin bir çerez oturumuna sahip olduğundan) yapmaktır.

```csharp
private async void EditProfileButton_Click(object sender, RoutedEventArgs e)
{
 IEnumerable<IAccount> accounts = await app.GetAccountsAsync();
 try
 {
  var authResult = await app.AcquireToken(scopes:App.ApiScopes)
                               .WithAccount(GetUserByPolicy(accounts, App.PolicyEditProfile)),
                               .WithPrompt(Prompt.NoPrompt),
                               .WithB2CAuthority(App.AuthorityEditProfile)
                               .ExecuteAsync();
  DisplayBasicTokenInfo(authResult);
 }
 catch
 {
  . . .
 }
}
```
## <a name="resource-owner-password-credentials-ropc-with-azure-ad-b2c"></a>Azure AD B2C ile kaynak sahibi parola kimlik bilgileri (ROPC)
ROPC akışı hakkında daha fazla bilgi için lütfen bu [belgelere](v2-oauth-ropc.md)bakın.

Uygulamanız bir kullanıcıdan parolalarını sorduğundan bu akış **önerilmez.** Bu sorun hakkında daha fazla bilgi için [bu makaleye](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/)bakın.

Kullanıcı adı/şifre kullanarak, bir dizi şeyden vazgeçersiniz:
- Modern kimliğin temel ilkeleri: şifre avlanır, yeniden oynanır. Çünkü ele geçirilebilen bir paylaşım sırrı konseptine sahibiz. Bu, parolasız ile uyumsuzdur.
- MFA yapması gereken kullanıcılar oturum açamaz (etkileşim olmadığı için).
- Kullanıcılar tek bir oturum açamaz.

### <a name="configure-the-ropc-flow-in-azure-ad-b2c"></a>Azure AD B2C'deki ROPC akışını yapılandırma
Azure AD B2C kiracınızda yeni bir kullanıcı akışı oluşturun ve **ROPC kullanarak Oturum Aç'ı**seçin. Bu, kiracınız için ROPC ilkesini etkinleştirir. Bkz. Daha fazla ayrıntı için [kaynak sahibi nin parola kimlik bilgilerini yapılandırın.](/azure/active-directory-b2c/configure-ropc)

`IPublicClientApplication`bir yöntem içerir:
```csharp
AcquireTokenByUsernamePassword(
            IEnumerable<string> scopes,
            string username,
            SecureString password)
```

Bu yöntem parametre olarak alır:
- Erişim belirteci istemek için *kapsamlar.*
- Bir *kullanıcı adı*.
- Kullanıcı için SecureString *parolası.*

ROPC ilkesini içeren yetkisini kullanmayı unutmayın.

### <a name="limitations-of-the-ropc-flow"></a>ROPC akışının sınırlamaları
 - ROPC akışı **yalnızca yerel hesaplar için çalışır** (bir e-posta veya kullanıcı adı kullanarak Azure AD B2C'ye kaydolduğunuz yer). Bu akış, Azure AD B2C (Facebook, Google, vb.) tarafından desteklenen kimlik sağlayıcılarından herhangi biri için federating durumunda çalışmaz.

## <a name="google-auth-and-embedded-webview"></a>Google Auth ve Gömülü Web Görünümü

Google'ı kimlik sağlayıcısı olarak kullanan bir Azure AD B2C geliştiricisiyseniz, Google [gömülü web görünümlerinden kimlik doğrulamaya](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html)izin vermeyince sistem tarayıcısını kullanmanız ağını yeniden komutla komuta bağlıyoruz. Şu `login.microsoftonline.com` anda, Google ile güvenilir bir otoritedir. Bu yetkisini kullanarak gömülü web view ile çalışacaktır. Ancak `b2clogin.com` kullanmak Google'ın güvenilir bir otoritesi değildir, bu nedenle kullanıcılar kimlik doğrulaması yapmayacaktır.

İşler değişirse bu [soruna](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/688) bir güncelleştirme sağlarız.

## <a name="caching-with-azure-ad-b2c-in-msalnet"></a>MSAL.Net'da Azure AD B2C ile önbelleğe alma

### <a name="known-issue-with-azure-ad-b2c"></a>Azure AD B2C ile bilinen sorun

MSAL.Net [belirteç önbelleğini](/dotnet/api/microsoft.identity.client.tokencache?view=azure-dotnet)destekler. Belirteç önbelleğe alma anahtarı, Kimlik Sağlayıcı tarafından döndürülen talepleri temel alır. Şu anda MSAL.Net belirteç önbellek anahtarı oluşturmak için iki iddia gerekir:
- `tid`azure AD Kiracı kimliği ve
- `preferred_username`

Bu iddiaların her ikisi de Azure AD B2C senaryolarının çoğunda eksiktir.

Müşteri etkisi, kullanıcı adı alanını görüntülemeye çalışırken, değer olarak "Belirteç yanıtından eksik" alıyor musunuz? Bu durumda, Azure AD B2C'nin sosyal hesaplar ve dış kimlik sağlayıcıları (IDP'ler) ile sınırlamalar nedeniyle preferred_username için IdToken'de bir değer döndürmemesidir. Azure AD, kullanıcının kim olduğunu bildiği için preferred_username için bir değer döndürür, ancak Azure AD B2C için, kullanıcı yerel bir hesapla, Facebook, Google, GitHub vb. ile oturum açabildiğinden, Azure AD B2C için preferred_username için kullanılacak tutarlı bir değer yoktur. MSAL'ın ADAL ile önbellek uyumluluğunu kullanıma samasını engellemek için, IdToken preferred_username için hiçbir şey döndürmediğinde Azure AD B2C hesaplarıyla uğraşırken "Belirteç yanıtından eksik" kullanmaya karar verdik. MSAL, kitaplıklar arasında önbellek uyumluluğunu korumak için preferred_username için bir değer döndürmelidir.

### <a name="workarounds"></a>Geçici Çözümler

#### <a name="mitigation-for-the-missing-tenant-id"></a>Eksik kiracı kimliği için azaltma

Önerilen geçici çözüm, [Önbelleğe Alma'yı İlke'ye göre](#acquire-a-token-to-apply-a-policy) kullanmaktır

Alternatif olarak, `tid` [b2C özel ilkeleri](https://aka.ms/ief)kullanıyorsanız, uygulamaya ek talepler döndürme olanağı sağladığından, talebi kullanabilirsiniz. [Talep Dönüşümü](/azure/active-directory-b2c/claims-transformation-technical-profile) hakkında daha fazla bilgi edinmek için

#### <a name="mitigation-for-missing-from-the-token-response"></a>"Belirteç yanıtından eksik" için azaltma
Seçeneklerden biri, "ad" iddiasını tercih edilen kullanıcı adı olarak kullanmaktır. İşlem bu [B2C doc](../../active-directory-b2c/user-flow-overview.md) -> "İade talep sütununda, başarılı bir profil düzenleme deneyiminden sonra başvurunuza geri gönderilen yetkilendirme belirteçlerinde iade olmasını istediğiniz talepleri seçin. Örneğin, Görüntü Adı, Posta Kodu'ni seçin."

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C uygulamaları için MSAL.NET etkileşimli olarak belirteçleri edinme hakkında daha fazla bilgi aşağıdaki örnekte verilmiştir.

| Örnek | Platform | Açıklama|
|------ | -------- | -----------|
|[aktif-dizin-b2c-xamarin-yerli](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Xamarin iOS, Xamarin Android, UWP | Azure AD B2C üzerinden kullanıcıların kimliğini doğrulamak ve ortaya çıkan belirteçlerle birlikte bir web API'sine erişmek için MSAL.NET nasıl kullanılacağını gösteren basit bir Xamarin Forms uygulaması.|
