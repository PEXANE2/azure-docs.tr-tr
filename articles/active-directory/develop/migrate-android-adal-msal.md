---
title: Android için ADAL -MSAL geçiş kılavuzu | Azure
description: Azure Active Directory Authentication Library (ADAL) Android uygulamanızı Microsoft Kimlik Doğrulama Kitaplığı'na (MSAL) nasıl geçirebilirsiniz öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.tgt_pltfrm: Android
ms.workload: identity
ms.date: 09/6/2019
ms.author: marsma
ms.reviewer: shoatman
ms.custom: aaddev
ms.openlocfilehash: 21866bb7dab3d5a093ffc4655161b80853eadfc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084061"
---
# <a name="adal-to-msal-migration-guide-for-android"></a>Android için ADAL-MSAL geçiş kılavuzu

Bu makalede, Microsoft Kimlik Doğrulama Kitaplığını (MSAL) kullanmak için Azure Etkin Dizin Kimlik Doğrulama Kitaplığı 'nı (ADAL) kullanan bir uygulamayı geçirmek için yapmanız gereken değişiklikler vurgulanır.

## <a name="difference-highlights"></a>Fark vurguları

ADAL, Azure Active Directory v1.0 bitiş noktası ile çalışır. Microsoft Kimlik Doğrulama Kitaplığı (MSAL), eskiden Azure Active Directory v2.0 bitiş noktası olarak bilinen Microsoft kimlik platformuyla çalışır. Microsoft kimlik platformu, Azure Active Directory v1.0'dan farklıdır:

Destekle -yen:
  - Kuruluş Kimliği (Azure Etkin Dizini)
  - Outlook.com, Xbox Live gibi kuruluş dışı kimlikler ve benzeri
  - (Sadece B2C) Google, Facebook, Twitter ve Amazon ile federe giriş

- Standartlar ile uyumludur:
  - OAuth v2.0
  - OpenID Bağlantı (OIDC)

MSAL kamu API'si aşağıdakiler dahil olmak üzere önemli değişiklikler sunar:

- Jetonlara erişmek için yeni bir model:
  - ADAL, sunucuyu temsil eden `AuthenticationContext`belirteçlere erişim sağlar. MSAL, istemciyi temsil eden `PublicClientApplication`tokenlere erişim sağlar. İstemci geliştiricilerin etkileşimde `PublicClientApplication` olmaları gereken her Kurum için yeni bir örnek oluşturmaları gerekmez. Yalnızca `PublicClientApplication` bir yapılandırma gereklidir.
  - Kaynak tanımlayıcılarına ek olarak kapsamları kullanarak erişim belirteçleri istemek için destek.
  - Artımlı onay desteği. Kullanıcı uygulamada uygulama kaydı sırasında dahil olmayanlar da dahil olmak üzere giderek daha fazla işlevsellik erişirken geliştiriciler kapsam isteyebilir.
  - Yetkililer artık çalışma zamanında doğrulanmadı. Bunun yerine, geliştirici geliştirme sırasında 'bilinen yetkililerin' bir listesini bildirir.
- Belirteç API değişiklikleri:
  - ADAL'da, `AcquireToken()` önce sessiz bir istekte bulunmak. Aksi takdirde, etkileşimli bir istekte bulunmak. Bu davranış, bazı geliştiricilerin `AcquireToken`yalnızca , kullanıcının zaman zaman kimlik bilgileri için beklenmeyen bir şekilde istenmesi ile sonuçlandı. MSAL, geliştiricilerin kullanıcının kullanıcı tarafından kullanıcı tarafından kullanıcı tarafından ne zaman bir kullanıcı lİstesi alacağı konusunda kasıtlı olmasını gerektirir.
    - `AcquireTokenSilent`her zaman başarılı ya da başarısız sessiz bir istek le sonuçlanır.
    - `AcquireToken`kullanıcıyı Kullanıcı Web'den isteyen bir istekle sonuçlanır.
- MSAL, varsayılan tarayıcıdan veya katıştırılmış bir web görünümünden oturum açmayı destekler:
  - Varsayılan olarak, aygıttaki varsayılan tarayıcı kullanılır. Bu, MSAL'ın hesaplarda zaten imzalanmış bir veya daha fazla hesap için mevcut olabilecek kimlik doğrulama durumunu (tanımlama bilgilerini) kullanmasına olanak tanır. Kimlik doğrulama durumu yoksa, MSAL üzerinden yetkilendirme sırasında kimlik doğrulama durumu (çerezler) aynı tarayıcıda kullanılacak diğer web uygulamaları nın yararına oluşturulmakta olan kimlik doğrulama durumu (çerezler) ile kimlik doğrulama sı.
- Yeni özel durum Modeli:
  - Özel durumlar, oluşan hata türünü ve geliştiricinin bunu çözmek için ne yapması gerektiğini daha açık bir şekilde tanımlar.
- MSAL parametre nesneleri `AcquireToken` destekler `AcquireTokenSilent` ve çağırır.
- MSAL bildirimsel yapılandırmayı destekler:
  - Müşteri Kimliği, URI'yi Yeniden Yönlendir.
  - Gömülü vs Varsayılan Tarayıcı
  - Yetkili
  - Okuma ve bağlantı zaman sonu gibi HTTP ayarları

## <a name="your-app-registration-and-migration-to-msal"></a>Uygulama kaydınız ve MSAL'a geçiş

MSAL'ı kullanmak için mevcut uygulama kaydınızı değiştirmeniz gerekmez. Artımlı/aşamalı onaydan yararlanmak istiyorsanız, artımlı olarak talep etmek istediğiniz belirli kapsamları belirlemek için kaydı gözden geçirmeniz gerekebilir. Kapsamlar ve artımlı rıza hakkında daha fazla bilgi aşağıdaki gibidir.

Portaldaki uygulama kaydınızda bir **API izinleri** sekmesi görürsünüz. Bu, uygulamanızın şu anda erişim istemek üzere yapılandırılan API'lerin ve izinlerin (kapsamların) bir listesini sağlar. Ayrıca, her API izniyle ilişkili kapsam adlarının listesini de gösterir.

### <a name="user-consent"></a>Kullanıcı izni

ADAL ve AAD v1 bitiş noktası ile, ilk kullanımda sahip oldukları kaynaklara kullanıcı onayı verilir. MSAL ve Microsoft kimlik platformu ile onay aşamalı olarak istenebilir. Artımlı onay, bir kullanıcının yüksek ayrıcalık olarak kabul edebileceği izinler için yararlıdır veya iznin neden gerekli olduğuna dair net bir açıklama sağlanmazsa başka bir şekilde sorgulanabilir. ADAL'da, bu izinler kullanıcının uygulamanızda oturum açmayı bırakmasına neden olmuş olabilir.

> [!TIP]
> Uygulamanızın neden izin alması gerektiği konusunda kullanıcınıza ek içerik sağlamanız gereken senaryolarda artımlı onay kullanılmasını öneririz.

### <a name="admin-consent"></a>Yönetici onayı

Kuruluş yöneticileri, uygulamanızın tüm kuruluş üyeleri adına gerektirdiği izinleri onaylayabilir. Bazı kuruluşlar yalnızca yöneticilerin uygulamalara onay almasına izin verir. Yönetici onayı, uygulamanız tarafından kullanılan tüm API izinlerini ve kapsamlarını uygulama kaydınıza eklemenizi gerektirir.

> [!TIP]
> Uygulama kaydınızda yer almayan bir şey için MSAL kullanarak kapsam talep edebilirsiniz, ancak uygulama kaydınızı kullanıcının izin verebileceği tüm kaynakları ve kapsamları içerecek şekilde güncellemenizi öneririz.

## <a name="migrating-from-resource-ids-to-scopes"></a>Kaynak işleri kapsamlarından kapsamlara geçiş

### <a name="authenticate-and-request-authorization-for-all-permissions-on-first-use"></a>İlk kullanımdaki tüm izinler için kimlik doğrulaması ve yetkilendirme isteği

Şu anda ADAL kullanıyorsanız ve artımlı onay kullanmanız gerekmiyorsa, MSAL'ı kullanmaya `acquireToken` başlamanın en `AcquireTokenParameter` basit yolu yeni nesneyi kullanarak bir istekte bulunmak ve kaynak kimliği değerini ayarlamaktır.

> [!CAUTION]
> Hem kapsamları hem de kaynak kimliğini ayarlamak mümkün değildir. Her ikisini de ayarlamaya `IllegalArgumentException`çalışmak bir .

 Bu, kullandığınız aynı v1 davranışıyla sonuçlanır. Uygulama kaydınızda istenen tüm izinler, ilk etkileşimleri sırasında kullanıcıdan istenir.

### <a name="authenticate-and-request-permissions-only-as-needed"></a>Yalnızca gerektiğinde izinleri doğrula ve isteyin

Artımlı onaydan yararlanmak için, uygulamanızın uygulama kaydınızdan kullandığı izinlerin (kapsamların) bir listesini yapın ve bunları şu temele göre iki liste halinde düzenleyin:

- Oturum açma sırasında kullanıcının uygulamanızla ilk etkileşimi sırasında hangi kapsamları talep etmek istediğiniz.
- Uygulamanızın önemli bir özelliğiyle ilişkili olan ve kullanıcıya da açıklamanız gereken izinler.

Kapsamları düzenledikten sonra, belirteç istediğiniz kaynağın (API) hangi tarafından listeyi düzenleyin. Kullanıcının aynı anda yetkilendirmesini istediğiniz diğer kapsamların yanı sıra.

MSAL destekler için isteğinizi yapmak için kullanılan parametreler nesnesi:

- `Scope`: Erişim jetonu için yetkilendirme istemek ve almak istediğiniz kapsamların listesi.
- `ExtraScopesToConsent`: Başka bir kaynak için erişim jetonu isterken yetkilendirme istemek istediğiniz kapsamların ek listesi. Bu kapsam listesi, kullanıcı yetkilendirmesi istemeniz gereken kaç kez en aza indirmenize olanak tanır. Bu da daha az kullanıcı yetkilendirmesi veya onay istemi anlamına gelir.

## <a name="migrate-from-authenticationcontext-to-publicclientapplications"></a>Kimlik Doğrulama Bağlamından PublicClientApplications'a geçiş

### <a name="constructing-publicclientapplication"></a>PublicClientApplication Oluşturma

MSAL kullandığınızda, bir `PublicClientApplication`. Bu nesne uygulama kimliğinizi modeller ve bir veya daha fazla yetkiliye istekte bulunmak için kullanılır. Bu nesne ile istemci kimliğinizi yapılandıracak, URI'yi yeniden yönlendirecek, varsayılan yetkiyi, aygıt tarayıcısını katıştırılmış web görünümüne, günlük düzeyine ve daha fazlasını kullanıp kullanmayacağınız.

Bu nesneyi, dosya olarak sağladığınız veya APK'niz içinde kaynak olarak depoladığınız JSON ile bildirimsel olarak yapılandırabilirsiniz.

Bu nesne bir singleton olmamasına rağmen, `Executors` dahili olarak hem etkileşimli hem de sessiz istekler için paylaşılan kullanır.

### <a name="business-to-business"></a>İşletmeden İşletmeye

ADAL'da, erişim belirteçleri istediğiniz her kuruluş ayrı `AuthenticationContext`bir örneğini gerektirir. MSAL'da bu artık bir gereklilik değildir. Sessiz veya etkileşimli isteğinizin bir parçası olarak belirtebilmek istediğiniz yetkiyi belirtebilirsiniz.

### <a name="migrate-from-authority-validation-to-known-authorities"></a>Yetki doğrulamadan bilinen makamlara geçiş

MSAL'ın yetki doğrulaması etkinleştirmek veya devre dışı bırakmanız için bir bayrağı yoktur. Yetki doğrulaması, ADAL'da ve MSAL'ın ilk sürümlerinde, kodunuzu kötü amaçlı olabilecek bir yetkiliden belirteç istemesini engelleyen bir özelliktir. MSAL şimdi Microsoft tarafından bilinen yetkililerin listesini alır ve bu listeyi yapılandırmanızda belirttiğiniz yetkililerle birleştirir.

> [!TIP]
> Azure İşletmeden Tüketiciye (B2C) kullanıcıysanız, bu yetki doğrulaması artık devre dışı bırakmanız gerekolmadığı anlamına gelir. Bunun yerine, desteklenen Azure AD B2C ilkelerinizin her birini MSAL yapılandırmanızda yetkili olarak ekleyin.

Microsoft tarafından tanınmayan ve yapılandırmanıza dahil olmayan bir yetkisini kullanmaya çalışırsanız, bir `UnknownAuthorityException`.

### <a name="logging"></a>Günlüğe Kaydetme
Artık yapılandırmanızın bir parçası olarak günlük işlemlerini şu şekilde yapılandırabilirsiniz:

 ```
 "logging": {
    "pii_enabled": false,
    "log_level": "WARNING",
    "logcat_enabled": true
  }
  ```

## <a name="migrate-from-userinfo-to-account"></a>UserInfo'dan Hesaba geçiş

ADAL'da, `AuthenticationResult` kimlik `UserInfo` doğrulaması yapılan hesap hakkında bilgi almak için kullanılan bir nesne sağlar. İnsan veya yazılım aracısı anlamına gelen "kullanıcı" terimi, bazı uygulamaların birden fazla hesabı olan tek bir kullanıcıyı (insan veya yazılım aracısı olsun) desteklediğini bildirmeyi zorlaştıracak şekilde uygulanmıştır.

Bir banka hesabı düşünün. Birden fazla finans kurumunda birden fazla hesabınız olabilir. Bir hesap açtığınızda, size (kullanıcıya) her hesap için bakiyenize, fatura ödemelerinize vb. erişmek için kullanılan ATM Kartı & PIN gibi kimlik bilgileri verilir. Bu kimlik bilgileri yalnızca bunları veren finans kurumunda kullanılabilir.

Benzer şekilde, bir finans kurumundaki hesaplar gibi, Microsoft kimlik platformundaki hesaplara kimlik bilgileri kullanılarak erişilir. Bu kimlik bilgileri Microsoft'a kaydedilir veya Microsoft tarafından verilir. Veya bir kuruluş adına Microsoft tarafından.

Microsoft kimlik platformunun bir finans kurumundan farklı olduğu durumlarda, bu benzetmede, Microsoft kimlik platformu, bir kullanıcının bir hesabı ve ilişkili kimlik bilgilerini kullanmasına izin veren bir çerçeve sağlayarak, birden fazla kişi ve kuruluş. Bu, bir banka tarafından verilen bir kartı başka bir finans kurumunda kullanabilmek gibidir. Bu, söz konusu kuruluşların tümü, bir hesabın birden çok kuruluş ta kullanılmasına olanak tanıyan Microsoft kimlik platformlarını kullandığından çalışır. Bir örneği aşağıda verilmiştir:

Sam Contoso.com için çalışır, ancak Fabrikam.com ait Azure sanal makinelerini yönetir. Sam'in Fabrikam'ın sanal makinelerini yönetebilmesi için onlara erişmesi için yetkiye ihtiyacı var. Bu erişim, Sam'in hesabını Fabrikam.com ekleyerek ve hesabına sanal makinelerle çalışmasını sağlayan bir rol vererek verilebilir. Bu, Azure portalı ile yapılır.

Sam'in Contoso.com hesabının Fabrikam.com üyesi olarak eklenmesi, Fabrikam.com'un Sam için Azure Active Directory'sinde yeni bir kayıt oluşturulmasıyla sonuçlanır. Sam'in Azure Etkin Dizin'deki kaydı kullanıcı nesnesi olarak bilinir. Bu durumda, bu kullanıcı nesnesi Contoso.com'da Sam'in kullanıcı nesnesine işaret eder. Sam'in Fabrikam kullanıcı nesnesi Sam'in yerel gösterimidir ve Fabrikam.com bağlamında Sam ile ilişkili hesap la ilgili bilgileri depolamak için kullanılır. Contoso.com, Sam'in unvanı Kıdemli DevOps Danışmanıdır. Fabrikam'da Sam'in unvanı Yüklenici-Sanal Makineler' dir. Contoso.com sam, sanal makineleri yönetmekle yükümlü veya yetkili değildir. Fabrikam.com'da tek işi bu. Yine de Sam'in izlendiği yalnızca bir kimlik bilgileri vardır ve bunlar Contoso.com tarafından verilen kimlik bilgileridir.

Başarılı `acquireToken` bir arama yapıldıktan sonra, sonraki `IAccount` `acquireTokenSilent` isteklerde kullanılabilecek bir nesneye başvuruda bulunulacağını görürsünüz.

### <a name="imultitenantaccount"></a>IMultiTenantHesabı

Hesabın temsil edildiği kiracıların her birinden bir hesapla ilgili iddialara erişen bir uygulamanız varsa, `IAccount` nesneleri . `IMultiTenantAccount` Bu `ITenantProfiles`arabirim, geçerli hesaba göre bir belirteç talep ettiğiniz kiracıların her birinde hesaba ait olan taleplere erişmenizi sağlayan kiracı kimliğine göre anahtarlı bir harita sağlar.

Kökünde iddiaları `IAccount` ve `IMultiTenantAccount` her zaman ev kiracı iddiaları içerir. Henüz ev kiracı içinde bir belirteç için bir istekte bulunmadıysanız, bu koleksiyon boş olacaktır.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="use-the-new-authenticationcallback"></a>Yeni AuthenticationCallback kullanın

```java
// Existing ADAL Interface
public interface AuthenticationCallback<T> {

    /**
     * This will have the token info.
     *
     * @param result returns <T>
     */
    void onSuccess(T result);

    /**
     * Sends error information. This can be user related error or server error.
     * Cancellation error is AuthenticationCancelError.
     *
     * @param exc return {@link Exception}
     */
    void onError(Exception exc);
}
```

```java
// New Interface for Interactive AcquireToken
public interface AuthenticationCallback {

    /**
     * Authentication finishes successfully.
     *
     * @param authenticationResult {@link IAuthenticationResult} that contains the success response.
     */
    void onSuccess(final IAuthenticationResult authenticationResult);

    /**
     * Error occurs during the authentication.
     *
     * @param exception The {@link MsalException} contains the error code, error message and cause if applicable. The exception
     *                  returned in the callback could be {@link MsalClientException}, {@link MsalServiceException}
     */
    void onError(final MsalException exception);

    /**
     * Will be called if user cancels the flow.
     */
    void onCancel();
}

// New Interface for Silent AcquireToken
public interface SilentAuthenticationCallback {

    /**
     * Authentication finishes successfully.
     *
     * @param authenticationResult {@link IAuthenticationResult} that contains the success response.
     */
    void onSuccess(final IAuthenticationResult authenticationResult);

    /**
     * Error occurs during the authentication.
     *
     * @param exception The {@link MsalException} contains the error code, error message and cause if applicable. The exception
     *                  returned in the callback could be {@link MsalClientException}, {@link MsalServiceException} or
     *                  {@link MsalUiRequiredException}.
     */
    void onError(final MsalException exception);
}


```

## <a name="migrate-to-the-new-exceptions"></a>Yeni özel durumlara geçiş

ADAL'de, `AuthenticationException` `ADALError` enum değerini almak için bir yöntem içeren bir tür özel durum vardır.
MSAL'da, özel durumlar hiyerarşisi vardır ve her birinin kendi ilişkili belirli hata kodları kümesi vardır.

MSAL İstisnaları Listesi

|Özel durum  | Açıklama  |
|---------|---------|
| `MsalException`     | MSAL tarafından atılan varsayılan denetlenen özel durum.  |
| `MsalClientException`     | Hata istemci tarafındaysa atılır. |
| `MsalArgumentException`     | Bir veya daha fazla giriş bağımsız değişkeni geçersizse atılır. |
| `MsalClientException`     | Hata istemci tarafındaysa atılır. |
| `MsalServiceException`     | Hata sunucu tarafında ise atılır. |
| `MsalUserCancelException`     | Kullanıcı kimlik doğrulama akışını iptal ettiyse atılır.  |
| `MsalUiRequiredException`     | Belirteç sessizce yenilenmiyorsa atılır.  |
| `MsalDeclinedScopeException`     | Sunucu tarafından bir veya daha fazla istenen kapsam reddedildiyse atılan.  |
| `MsalIntuneAppProtectionPolicyRequiredException` | Kaynak MAMCA koruma ilkesi etkinse atıldı. |

### <a name="adalerror-to-msalexception-errorcode"></a>ADALError to MsalException ErrorCode

### <a name="adal-logging-to-msal-logging"></a>ADAL MSAL Günlüğü'ne Giriş

```java
// Legacy Interface
    StringBuilder logs = new StringBuilder();
    Logger.getInstance().setExternalLogger(new ILogger() {
            @Override
            public void Log(String tag, String message, String additionalMessage, LogLevel logLevel, ADALError errorCode) {
                logs.append(message).append('\n');
            }
        });
```

```java
// New interface
  StringBuilder logs = new StringBuilder();
  Logger.getInstance().setExternalLogger(new ILoggerCallback() {
            @Override
            public void log(String tag, Logger.LogLevel logLevel, String message, boolean containsPII) {
                logs.append(message).append('\n');
            }
        });

// New Log Levels:
public enum LogLevel
{
        /**
         * Error level logging.
         */
        ERROR,
        /**
         * Warning level logging.
         */
        WARNING,
        /**
         * Info level logging.
         */
        INFO,
        /**
         * Verbose level logging.
         */
        VERBOSE
}
```
