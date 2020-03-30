---
title: Belirteç önbelleği serileştirme (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak belirteç önbelleğinin serileştirme ve müşteri serileştirmesi hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/16/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 1bd348ad27d892d0421b13c16ce81bc4f5dfb021
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262808"
---
# <a name="token-cache-serialization-in-msalnet"></a>MSAL.NET'da belirteç önbelleği serileştirme
Bir [belirteç alındıktan](msal-acquire-cache-tokens.md)sonra, Microsoft Kimlik Doğrulama Kitaplığı (MSAL) tarafından önbelleğe alınarak önbelleğe alınmıştır.  Uygulama kodu, başka bir yöntemle belirteç edinmeden önce önbellekten bir belirteç almaya çalışmalıdır.  Bu makalede, MSAL.NET'daki belirteç önbelleğinin varsayılan ve özel serileştirmesi anlatılmaktadır.

Bu makale MSAL.NET 3.x içindir. 2.x MSAL.NET ilgileniyorsanız, MSAL.NET [2.x'teki Token önbelleği serileştirmesine](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Token-cache-serialization-2x)bakın.

## <a name="default-serialization-for-mobile-platforms"></a>Mobil platformlar için varsayılan serileştirme

MSAL.NET, varsayılan olarak bellek içi belirteç önbelleği sağlanır. Platform kapsamında kullanıcı için güvenli depolamanın kullanılabildiği platformlar için varsayılan olarak serileştirme sağlanır. Bu Evrensel Windows Platformu (UWP), Xamarin.iOS ve Xamarin.Android için geçerlidir.

> [!Note]
> Bir Xamarin.Android projesini MSAL.NET 1,x'ten MSAL.NET 3,x'e geçirdiğinizde, Visual Studio dağıtımları yerel depolama nın geri yüklenmesini tetiklediğinde eski önbelleğe alınmış belirteçlerin geri gelmesini önlemek için projenize eklemek `android:allowBackup="false"` isteyebilirsiniz. [Bkz. Sorun #659](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/659#issuecomment-436181938).

## <a name="custom-serialization-for-windows-desktop-apps-and-web-appsweb-apis"></a>Windows masaüstü uygulamaları ve web uygulamaları/web API'leri için özel serileştirme

Unutmayın, özel serileştirme mobil platformlarda (UWP, Xamarin.iOS ve Xamarin.Android) kullanılamaz. MSAL zaten bu platformlar için güvenli ve performant serileştirme mekanizması tanımlar. .NET masaüstü ve .NET Core uygulamaları, ancak, çeşitli mimarileri var ve MSAL genel amaçlı bir serileştirme mekanizması uygulayamaz. Örneğin, web siteleri belirteçleri Redis önbelleğinde depolamayı seçebilir veya masaüstü uygulamaları belirteçleri şifreli bir dosyada saklayabilir. Yani serileştirme kutudan dışarı sağlanmaz. .NET masaüstünde veya .NET Core'da kalıcı bir belirteç önbelleği uygulamasına sahip olmak için serileştirmeyi özelleştirmeniz gerekir.

Belirteç önbelleği serileştirmesinde aşağıdaki sınıflar ve arabirimler kullanılır:

- `ITokenCache`, belirteç önbelleği serileştirme isteklerine abone olmak için olayları tanımlayan ve çeşitli biçimlerde önbelleği serihale veya seri dışıhale getiren yöntemler (ADAL v3.0, MSAL 2.x ve MSAL 3.x = ADAL v5.0).
- `TokenCacheCallback`serileştirmeyi işleyebilir diye olaylara geçirilen bir geri aramadır. Onlar tür `TokenCacheNotificationArgs`argümanlar ile çağrılacak.
- `TokenCacheNotificationArgs`yalnızca `ClientId` uygulamanın ve belirteç kullanılabilir kullanıcıya bir başvuru sağlar.

  ![Sınıf diyagramı](media/msal-net-token-cache-serialization/class-diagram.png)

> [!IMPORTANT]
> MSAL.NET sizin için belirteç önbellekleri `IToken` oluşturur ve bir uygulamanın `UserTokenCache` ve `AppTokenCache` özellikleri çağırdığınızda önbelleği sağlar. Arayüzü kendiniz uygulamamanız gerekiyor. Özel bir belirteç önbelleği serileştirme uyguladığınızda sorumluluğunuz şudur:
> - Tepki `BeforeAccess` ve `AfterAccess` "olaylar" (veya Onların Async tatlar). Temsilci `BeforeAccess` önbelleği deserialize etmekle sorumluyken, `AfterAccess` önbelleği seri hale getirmekten sorumludur.
> - Bu olayların bir kısmı, olay bağımsız değişkeninden istediğiniz depolama alanına geçirilen lekeleri saklar veya yükler.

Stratejiler, [bir kamu istemcisi uygulaması](msal-client-applications.md) (masaüstü) veya gizli bir istemci [uygulaması](msal-client-applications.md)) (web uygulaması / web API, daemon uygulaması) için bir belirteç önbellek serileştirme yazıyorsanız bağlı olarak farklıdır.

### <a name="token-cache-for-a-public-client"></a>Genel istemci için belirteç önbelleği 

v2.xMSAL.NET olduğundan, ortak istemcinin belirteç önbelleğini seri hale getirmek için çeşitli seçenekleriniz var. Önbelleği yalnızca MSAL.NET biçimine seri leştirebilirsiniz (birleşik biçim önbelleği MSAL ve platformlar arasında yaygındır).  Ayrıca, ADAL V3'ün [eski](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) belirteç önbelleği serileştirmesini de destekleyebilirsiniz.

belirteç önbellek serileştirme ADAL.NET 3.x, ADAL.NET 5.x arasında tek oturum açma durumunu paylaşmak için özelleştirme ve MSAL.NET aşağıdaki örnek bölümünde açıklanmıştır: [active-directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2).

> [!Note]
> 1.1.4 önizleme önbelleğe lenden biçimindeMSAL.NET artık MSAL 2.x'te desteklenmez. 1.x MSAL.NET kullanan uygulamalarınız varsa, kullanıcılarınızın yeniden oturum açmaları gerekir. Alternatif olarak, ADAL 4.x (ve 3.x) geçiş desteklenir.

#### <a name="simple-token-cache-serialization-msal-only"></a>Basit belirteç önbelleği serileştirme (yalnızca MSAL)

Aşağıda, masaüstü uygulamaları için belirteç önbelleğinin özel serileştirilmesinin safça uygulanmasına bir örnek verilmiştir. Burada, kullanıcı belirteç önbelleği uygulamayla aynı klasördeki bir dosyadır.

Uygulamayı yaptıktan sonra, yöntemi arayarak ve `TokenCacheHelper.EnableSerialization()` uygulamayı `UserTokenCache`geçirerek serileştirmeyi etkinleştirin.

```csharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

`TokenCacheHelper` Yardımcı sınıf olarak tanımlanır:

```csharp
static class TokenCacheHelper
 {
  public static void EnableSerialization(ITokenCache tokenCache)
  {
   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Path to the token cache
  /// </summary>
  public static readonly string CacheFilePath = System.Reflection.Assembly.GetExecutingAssembly().Location + ".msalcache.bin3";

  private static readonly object FileLock = new object();


  private static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeMsalV3(File.Exists(CacheFilePath)
            ? ProtectedData.Unprotect(File.ReadAllBytes(CacheFilePath),
                                      null,
                                      DataProtectionScope.CurrentUser)
            : null);
   }
  }

  private static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     // reflect changesgs in the persistent store
     File.WriteAllBytes(CacheFilePath,
                         ProtectedData.Protect(args.TokenCache.SerializeMsalV3(),
                                                 null,
                                                 DataProtectionScope.CurrentUser)
                         );
    }
   }
  }
 }
```

[Microsoft.Identity.Client.Extensions.Msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal) açık kaynak kitaplığından, ortak istemci uygulamaları (Windows, Mac ve Linux'ta çalışan masaüstü uygulamaları için) için ürün kalitesi belirteç önbelleği tabanlı bir seriizan bir önizleme sinebilirsiniz. Uygulamalarınıza aşağıdaki nuget paketinden ekleyebilirsiniz: [Microsoft.Identity.Client.Extensions.Msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

#### <a name="dual-token-cache-serialization-msal-unified-cache-and-adal-v3"></a>Çift belirteç önbelleği serileştirme (MSAL birleşik önbellek ve ADAL v3)

Token önbellek serileştirmesini hem birleşik önbellek biçimiyle (aynı platformda 4,x, MSAL.NET 2,x ve aynı nesil veya daha eski diğer MSAL'ları ADAL.NET ortak olarak) uygulamak istiyorsanız, aşağıdaki koda bir göz atın:

```csharp
string appLocation = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location;
string cacheFolder = Path.GetFullPath(appLocation) + @"..\..\..\..");
string adalV3cacheFileName = Path.Combine(cacheFolder, "cacheAdalV3.bin");
string unifiedCacheFileName = Path.Combine(cacheFolder, "unifiedCache.bin");

IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
                                    .Build();
FilesBasedTokenCacheHelper.EnableSerialization(app.UserTokenCache,
                                               unifiedCacheFileName,
                                               adalV3cacheFileName);

```

Bu kez yardımcı sınıf olarak tanımlanan:

```csharp
using System;
using System.IO;
using System.Security.Cryptography;
using Microsoft.Identity.Client;

namespace CommonCacheMsalV3
{
 /// <summary>
 /// Simple persistent cache implementation of the dual cache serialization (ADAL V3 legacy
 /// and unified cache format) for a desktop applications (from MSAL 2.x)
 /// </summary>
 static class FilesBasedTokenCacheHelper
 {
  /// <summary>
  /// Enables the serialization of the token cache
  /// </summary>
  /// <param name="adalV3CacheFileName">File name where the cache is serialized with the
  /// ADAL V3 token cache format. Can
  /// be <c>null</c> if you don't want to implement the legacy ADAL V3 token cache
  /// serialization in your MSAL 2.x+ application</param>
  /// <param name="unifiedCacheFileName">File name where the cache is serialized
  /// with the Unified cache format, common to
  /// ADAL V4 and MSAL V2 and above, and also across ADAL/MSAL on the same platform.
  ///  Should not be <c>null</c></param>
  /// <returns></returns>
  public static void EnableSerialization(ITokenCache tokenCache, string unifiedCacheFileName, string adalV3CacheFileName)
  {
   UnifiedCacheFileName = unifiedCacheFileName;
   AdalV3CacheFileName = adalV3CacheFileName;

   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// File path where the token cache is serialized with the unified cache format
  /// (ADAL.NET V4, MSAL.NET V3)
  /// </summary>
  public static string UnifiedCacheFileName { get; private set; }

  /// <summary>
  /// File path where the token cache is serialized with the legacy ADAL V3 format
  /// </summary>
  public static string AdalV3CacheFileName { get; private set; }

  private static readonly object FileLock = new object();

  public static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeAdalV3(ReadFromFileIfExists(AdalV3CacheFileName));
    try
    {
     args.TokenCache.DeserializeMsalV3(ReadFromFileIfExists(UnifiedCacheFileName));
    }
    catch(Exception ex)
    {
     // Compatibility with the MSAL v2 cache if you used one
     args.TokenCache.DeserializeMsalV2(ReadFromFileIfExists(UnifiedCacheFileName));
    }
   }
  }

  public static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     WriteToFileIfNotNull(UnifiedCacheFileName, args.TokenCache.SerializeMsalV3());
     if (!string.IsNullOrWhiteSpace(AdalV3CacheFileName))
     {
      WriteToFileIfNotNull(AdalV3CacheFileName, args.TokenCache.SerializeAdalV3());
     }
    }
   }
  }

  /// <summary>
  /// Read the content of a file if it exists
  /// </summary>
  /// <param name="path">File path</param>
  /// <returns>Content of the file (in bytes)</returns>
  private static byte[] ReadFromFileIfExists(string path)
  {
   byte[] protectedBytes = (!string.IsNullOrEmpty(path) && File.Exists(path))
       ? File.ReadAllBytes(path) : null;
   byte[] unprotectedBytes = encrypt ?
       ((protectedBytes != null) ? ProtectedData.Unprotect(protectedBytes, null, DataProtectionScope.CurrentUser) : null)
       : protectedBytes;
   return unprotectedBytes;
  }

  /// <summary>
  /// Writes a blob of bytes to a file. If the blob is <c>null</c>, deletes the file
  /// </summary>
  /// <param name="path">path to the file to write</param>
  /// <param name="blob">Blob of bytes to write</param>
  private static void WriteToFileIfNotNull(string path, byte[] blob)
  {
   if (blob != null)
   {
    byte[] protectedBytes = encrypt
      ? ProtectedData.Protect(blob, null, DataProtectionScope.CurrentUser)
      : blob;
    File.WriteAllBytes(path, protectedBytes);
   }
   else
   {
    File.Delete(path);
   }
  }

  // Change if you want to test with an un-encrypted blob (this is a json format)
  private static bool encrypt = true;
 }
}
```

### <a name="token-cache-for-a-web-app-confidential-client-application"></a>Bir web uygulaması için belirteç önbelleği (gizli istemci uygulaması)

Web uygulamalarında veya web API'lerinde önbellek oturumdan, Redis önbelleğinden veya veritabanından yararlanabilir.

Web uygulamalarında veya web API'lerinde, hesap başına bir belirteç önbelleği bulundurun.  Web uygulamaları için belirteç önbelleği hesap kimliğine göre anahtarlanmalıdır.  Web API'leri için, hesap API aramak için kullanılan belirteç karma tarafından anahtarlanmış olmalıdır. MSAL.NET ,NET Framework ve .NET Core alt platformlarında özel belirteç önbelleği serileştirme sağlar. Önbelleğe erişildiğinde olaylar ateşlenir, uygulamalar önbelleği serihale mi yoksa deserialize mi edeceğini seçebilir. Kullanıcıları işleyen gizli istemci uygulamalarında (kullanıcıları oturum açan ve web API'lerini çağıran web uygulamaları ve akış aşağı web API'lerini çağıran web API'leri), birçok kullanıcı olabilir ve kullanıcılar paralel olarak işlenebilir. Güvenlik ve performans nedenleriyle, tavsiyemiz kullanıcı başına bir önbelleği seri hale getirmektir. Serileştirme olayları, işlenen kullanıcının kimliğine dayalı bir önbellek anahtarı nı hesaplar ve bu kullanıcı için bir belirteç önbelleği serialize/deserialie.

Web uygulamaları ve web API'leri için belirteç önbelleklerinin nasıl kullanılacağına örnekler, [2-2.Aşama Önbelleğinde](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache) [ASP.NET Core web uygulaması öğreticisinde](https://ms-identity-aspnetcore-webapp-tutorial) mevcuttur. Uygulamalar için [microsoft-authentication-extensions-for-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) kitaplığında [(Microsoft.Identity.Client.Extensions.Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) klasöründe) [TokenCacheProviders](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/Microsoft.Identity.Web/TokenCacheProviders) klasörüne bir göz atın. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örnekler belirteç önbellek serileştirmegösteriz.

| Örnek | Platform | Açıklama|
| ------ | -------- | ----------- |
|[aktif-dizin-dotnet-masaüstü-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | Masaüstü (WPF) | Windows Desktop .NET (WPF) uygulaması Microsoft Graph API'yi arayarak. ![Topoloji](media/msal-net-token-cache-serialization/topology.png)|
|[aktif dizin-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2) | Masaüstü (Konsol) | Azure AD v1.0 uygulamalarının (ADAL.NET kullanarak) Azure AD v2.0 uygulamalarına geçişini gösteren ve özellikle [Token Önbellek Geçişi](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2/blob/master/TokenCacheMigration/README.md) gibi yakınsanmış uygulamalar (MSAL.NET kullanarak) olarak adlandırılan Visual Studio çözümleri kümesi|
