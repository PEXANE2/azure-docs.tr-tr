---
title: Android için ADAL MSAL geçiş kılavuzu | Mavisi
description: Azure Active Directory kimlik doğrulaması kitaplığı (ADAL) Android uygulamanızı Microsoft kimlik doğrulama kitaplığı 'na (MSAL) geçirmeyi öğrenin.
services: active-directory
author: tylermsft
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: Android
ms.workload: identity
ms.date: 09/6/2019
ms.author: twhitney
ms.reviewer: shoatman
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d06c84e6afcabb19c985d242679d6db8616a62e2
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679769"
---
# <a name="adal-to-msal-migration-guide-for-android"></a>Android için ADAL MSAL geçiş kılavuzu

Bu makalede, Microsoft kimlik doğrulama kitaplığı 'nı (ADAL) Azure Active Directory kullanan bir uygulamayı Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanmak üzere geçirmek için gereken değişiklikler vurgulanmaktadır.

## <a name="difference-highlights"></a>Fark vurguları

ADAL Azure Active Directory v 1.0 uç noktasıyla birlikte çalışmaktadır. Microsoft kimlik doğrulama kitaplığı (MSAL), daha önce Azure Active Directory v 2.0 uç noktası olarak bilinen Microsoft Identity platformu ile birlikte kullanılır.

Microsoft Identity platform bu Azure Active Directory v 1.0 'dan farklıdır:

- Şunları destekler:
  - Kuruluş kimliği (Azure Active Directory)
  - Outlook.com, Xbox Live vb. gibi kurumsal olmayan kimlikler.
  - (Yalnızca B2C) Google, Facebook, Twitter ve Amazon ile Federe oturum açma.

- , İle uyumlu standartlara sahiptir:
  - OAuth v 2.0
  - OpenID Connect (OıDC)

MSAL public API 'SI, aşağıdakiler de dahil olmak üzere önemli kullanılabilirlik değişikliklerini tanıtır:

- Belirteçlere erişmek için yeni model:
  - ADAL, sunucuyu temsil eden `AuthenticationContext` aracılığıyla belirteçlere erişim sağlar. MSAL, istemciyi temsil eden `PublicClientApplication` yoluyla belirteçlere erişim sağlar. İstemci geliştiricilerinin, ile etkileşimde bulunmaları gereken her yetkili için yeni bir `PublicClientApplication` örneği oluşturmalarına gerek yoktur. Yalnızca bir `PublicClientApplication` yapılandırması gerekir.
  - Kaynak tanımlayıcılarına ek olarak kapsamları kullanarak erişim belirteçleri isteme desteği.
  - Artımlı izin desteği. Geliştiriciler, uygulama kaydı sırasında dahil olmayanlar da dahil olmak üzere kapsam isteğinde bulunabilir.
  - Yetkili doğrulama-> bilinen yetkililer
      * Yetkililer artık çalışma zamanında doğrulanmaz; Bunun yerine, geliştirici geliştirme sırasında ' bilinen yetkililer ' listesi bildirir.
- Belirteç API 'SI değişiklikleri:
  - ADAL içinde, `AcquireToken` ilk önce sessiz bir istek yapmaya çalışır ve bu, etkileşimli bir istek yapar. Bu davranış, bazı geliştiriciler yalnızca `AcquireToken` ' a bağlı olduğundan, bazen kullanıcı etkileşiminin beklenmedik bir anda gerçekleşecektir. MSAL, kullanıcı ARABIRIMI istemi aldığında geliştiricilerin kasıtlı olarak bir şekilde yapılmasını gerektirir.
    - `AcquireTokenSilent`, her zaman başarılı ya da başarısız olan sessiz bir istekle sonuçlanır.
    - `AcquireToken` her zaman etkileşimli (Kullanıcı ARABIRIMI ile istenir) isteğiyle sonuçlanır.
- MSAL, varsayılan tarayıcıdan veya katıştırılmış Web görünümünden oturum açma kullanıcı arabirimi etkileşimini destekler:
  - Varsayılan olarak, cihazdaki varsayılan tarayıcı kullanılır. Bu, MSAL 'in bir veya daha fazla oturum açmış hesap için zaten mevcut olabilecek kimlik doğrulama durumunu (tanımlama bilgileri) kullanmasına izin verir. Kimlik doğrulama durumu yoksa, MSAL aracılığıyla yetkilendirme sırasında kimlik doğrulaması, aynı tarayıcıda kullanılacak diğer Web uygulamalarının avantajı için oluşturulan kimlik doğrulama durumu (tanımlama bilgileri) ile sonuçlanır.
- Yeni özel durum modeli:
  - Özel durumlar oluşan özel durum türü ve geliştiricinin bunu çözmek için yapması gerekenler hakkında daha net bir durumdur
- MSAL, `AcquireToken` ve `AcquireTokenSilent` çağrıları için parametre nesnelerini destekler.
- MSAL, için bildirim temelli yapılandırmayı destekler:
  - İstemci KIMLIĞI, yeniden yönlendirme URI 'SI
  - Gömülü vs varsayılan tarayıcı
  - Kaynakça
  - Okuma ve bağlantı zaman aşımı gibi HTTP ayarları

## <a name="your-app-registration-and-migration-to-msal"></a>Uygulama kaydınız ve MSAL 'e geçiş

MSAL kullanmak için mevcut uygulama kaydlarınızın hiçbir değişiklik yapması gerekmez. Artımlı/aşamalı onay özelliğinden yararlanmak istiyorsanız, artımlı olarak istemek istediğiniz belirli kapsamları belirlemek için kaydı gözden geçirmeniz gerekebilir. Kapsamlar ve artımlı izin hakkında daha fazla bilgi aşağıda verilmiştir.

Portalda uygulamanızın kaydında bir **API izinleri** sekmesi görürsünüz. Bu, uygulamanızın Şu anda erişim isteyecek şekilde yapılandırıldığı API 'lerin ve izinlerin (kapsamların) bir listesini sağlar. Ayrıca, her API izniyle ilişkili kapsam adlarının bir listesini gösterir.

### <a name="user-consent"></a>Kullanıcı onayı

ADAL ve AAD v1 uç noktası ile, kendilerine ait oldukları kaynaklara yönelik kullanıcı onayı ilk kullanım için verilir. MSAL ve Microsoft Identity platformu ile, izin artımlı olarak istenebilir. Artımlı onay, bir kullanıcının yüksek ayrıcalığa göz önünde bulundurabilecek izinler için yararlıdır veya iznin neden gerekli olduğuna ilişkin açık bir açıklama ile sağlanmayan durumlarda soru alabilir. ADAL 'da, bu izinler kullanıcının uygulamanızda oturum açmasını terk ediyor olabilir.

> [!TIP]
> Uygulamanıza izin gereksinimi hakkında daha fazla bağlam sağlamanız gereken senaryolarda artımlı izin kullanımını öneririz.

### <a name="admin-consent"></a>Yönetici onayı

Kuruluş yöneticileri, uygulamanızın tüm üyelerinin adına gereksinim duyduğu izinleri kabul edebilir. Bazı kuruluşlar yalnızca yöneticilerin uygulamalara izin vermesini sağlar. Yönetici onayı, uygulamanız tarafından uygulama kaydıza kullanılan tüm API izinlerini ve kapsamları dahil etmeniz gerekir.

> [!TIP]
> Uygulama kaydınızda bulunmayan bir şey için MSAL kullanarak bir kapsam isteyebilse de, uygulama kaydınızı, bir kullanıcının izin verebileceği tüm kaynakları ve kapsamları içerecek şekilde güncelleştirmenizi öneririz.

## <a name="migrating-from-resource-ids-to-scopes"></a>Kaynak kimliklerinden kapsamlara geçme

### <a name="authenticate-and-request-authorization-for-all-permissions-on-first-use"></a>İlk kullanımda olan tüm izinler için kimlik doğrulaması ve yetkilendirme iste

Şu anda ADAL kullanıyorsanız ve artımlı onay kullanmanıza gerek duymuyorsanız, MSAL kullanmaya başlamanın en kolay yolu, yeni `AcquireTokenParameter` nesnesini kullanarak `acquireToken` isteği ve kaynak KIMLIĞI değerini ayarlamayı kullanmaktır.

> [!CAUTION]
> Hem kapsamları hem de bir kaynak kimliğini ayarlamak mümkün değildir. Her ikisini de ayarlamaya çalışmak, `IllegalArgumentException` ile sonuçlanır.

 Bu, kullandığınız v1 davranışına neden olur. Uygulama kaydınızdan istenen tüm izinler kullanıcıdan ilk etkileşimi sırasında istenir.

### <a name="authenticate-and-request-permissions-only-as-needed"></a>Kimlik doğrulama ve izinleri yalnızca gerektiğinde iste

Artımlı izin avantajlarından yararlanmak için uygulamanızın uygulama kaydınızdan kullandığı izinlerin (kapsamlar) bir listesini oluşturmanız ve bunları temel alarak bunları iki listede düzenlemeniz gerekir:

- Kullanıcının oturum açma sırasında uygulamanızla ilk etkileşimini istemek istediğiniz kapsamlar.
- Uygulamanızın önemli bir özelliğiyle ilişkili izinler, Ayrıca, kullanıcıya de açıklamanıza ihtiyacınız olacak.

Kapsamları organize edildikten sonra her bir listeyi bir belirteç istemek istediğiniz kaynak (API) ile düzenlemeniz gerekir. Ayrıca, kullanıcının aynı anda yetkilendirmek istediğiniz diğer kapsamların yanı sıra.

MSAL için istekte bulunan Parameters nesnesi şunları destekler:

- `Scope`: yetkilendirme isteğinde bulunmasını istediğiniz kapsamların listesi ve erişim belirteci alın.
- `ExtraScopesToConsent`: başka bir kaynak için erişim belirteci istediğinizde, yetkilendirme isteğinde bulunmasını istediğiniz kapsamların ek bir listesi. Bu kapsam listesi, kullanıcı yetkilendirmesi istemek için gereken süreyi en aza indirmenize olanak tanır. Bu, daha az kullanıcı yetkilendirmesi veya onay istemleri anlamına gelir.

## <a name="migrate-from-authenticationcontext-to-publicclientapplications"></a>AuthenticationContext 'ten PublicClientApplications 'e geçiş

### <a name="constructing-publicclientapplication"></a>PublicClientApplication oluşturma

MSAL kullandığınızda `PublicClientApplication` ' ı örnekleyebilirsiniz. Bu nesne, uygulama kimliğinizi modelleyen ve bir veya daha fazla yetkililere istek yapmak için kullanılır. Bu nesneyle, istemci kimliğinizi, yeniden yönlendirme URI 'sini, varsayılan yetkilinizi, cihaz tarayıcısı ile ekli Web görünümü, günlük düzeyi ve daha fazlasını kullanacak şekilde yapılandıracaksınız.

Bu nesneyi, bir dosya olarak ya da APK 'niz içinde kaynak olarak sağladığınız JSON ile bildirimli olarak yapılandırabilirsiniz.

Bu nesne tek olmamakla birlikte, dahili olarak hem etkileşimli hem de sessiz istekler için paylaşılan `Executors` kullanır.

### <a name="business-to-business"></a>İşletmeden Işletmeye

ADAL içinde, erişim belirteçleri istediğiniz her kuruluş, @no__t için ayrı bir örnek gerektirir. MSAL içinde, bu artık bir gereklilik değildir. Sessiz veya etkileşimli isteğiniz kapsamında belirteç istemek istediğiniz yetkiyi belirtebilirsiniz.

### <a name="migrate-from-authority-validation-to-known-authorities"></a>Yetkili doğrulamadan bilinen yetkililere geçiş yapma

MSAL, yetkilendirme doğrulamasını etkinleştirmek veya devre dışı bırakmak için bayrak içermez. Yetkili doğrulama, kodunuzun potansiyel olarak kötü amaçlı bir yetkilinden belirteç isteğinde bulunmasını önleyen ADAL ve MSAL 'in ilk sürümlerindeki bir özelliktir. MSAL artık Microsoft tarafından bilinen bir yetkililer listesini alır ve bu listeyi yapılandırmanızda belirttiğiniz yetkililer ile birleştirir.

> [!TIP]
> Bir Azure Işletmeden tüketici (B2C) kullanıcısı kullanıyorsanız, bu, artık yetkili doğrulamasını devre dışı bırakmanız gerekmediği anlamına gelir. Bunun yerine, desteklenen Azure AD B2C ilkelerinin her birini MSAL yapılandırmanızda yetkililer olarak dahil edin.

Microsoft tarafından bilinmeyen ve yapılandırmanıza dahil olmayan bir yetkiyi kullanmaya çalışırsanız, `UnknownAuthorityException` olur.

### <a name="logging"></a>Günlüğe Kaydetme
Artık, aşağıdaki gibi yapılandırmanızın bir parçası olarak günlüğe kaydetmeyi bildirimli olarak yapılandırabilirsiniz
 
 ```
 "logging": {
    "pii_enabled": false,
    "log_level": "WARNING",
    "logcat_enabled": true
  }
  ```

## <a name="migrate-from-userinfo-to-account"></a>UserInfo 'dan hesaba geçiş

ADAL içinde `AuthenticationResult` kimliği doğrulanmış hesap hakkında bilgi almak için kullanılan bir `UserInfo` nesnesi sağlar. Bir insan veya yazılım Aracısı olan "Kullanıcı" terimi, bazı uygulamaların birden çok hesabı olan tek bir kullanıcıyı (insan veya yazılım Aracısı) desteklediği şekilde iletişim kurmayı zorlaştırır.

Bir banka hesabı değerlendirin. Birden fazla mali kurum üzerinde birden fazla hesabınız olabilir. Bir hesabı açtığınızda, size (Kullanıcı), her hesap için bakiyeniz, faturanızda ve bu gibi durumlarda kullanılan bir ATM kartı & PIN kodu gibi kimlik bilgileri verilir. Bu kimlik bilgileri, yalnızca bunları veren finans kurumda kullanılabilir.

Benzerleme vurguladı tarafından finansal bir kurumdaki hesaplar gibi, Microsoft Identity platformunda hesaplara kimlik bilgileri kullanılarak erişilir. Bu kimlik bilgileri, Microsoft tarafından kaydedilir veya tarafından verilir. Ya da bir kuruluş adına Microsoft tarafından.

Microsoft Identity platformunun bir mali kuruluştan farklı olduğu durumlarda, bu benzerleme vurguladı Microsoft Identity platformunun, bir kullanıcının bir hesabı ve ilişkili kimlik bilgilerini kullanmasına izin veren bir çerçeve sağladığından, birden çok kişi ve kuruluş. Bu, henüz başka bir mali kurumda bir banka tarafından verilen kartı kullanabiliyor gibidir. Bu, söz konusu tüm kuruluşların, bir hesabın birden çok kuruluşta kullanılmasına izin veren Microsoft Identity platformunu kullandığından, bu işe yarar. Örnek buradadır:

Sam, Contoso.com için geçerlidir ancak Fabrikam.com 'e ait olan Azure sanal makinelerini yönetir. Sam 'nin fabrikam 'ın sanal makinelerini yönetmesi için onlara erişim yetkisi olması gerekir. Bu erişim, Sam hesabı Fabrikam.com 'ye eklenerek ve hesabına sanal makinelerle çalışmasına izin veren bir rol verilerek verilebilir. Bu işlem Azure portal yapılır.

Fabrikam.com üyesi olarak Sam Contoso.com hesabını eklemek, fabrikam. com ' un Sam için Azure Active Directory yeni bir kayıt oluşturulmasına neden olur. Sam Azure Active Directory içindeki kayıt, Kullanıcı nesnesi olarak bilinir. Bu durumda, bu kullanıcı nesnesi Contoso.com içinde Sam kullanıcı nesnesine geri işaret edecektir. Sam 'ın fabrikam Kullanıcı nesnesi, Sam 'ın yerel gösterimidir ve Fabrikam.com bağlamında Sam ile ilişkili hesap hakkındaki bilgileri depolamak için kullanılır. Contoso.com ' de, Sam 'nin başlığı, üst düzey DevOps danışmanıdır. Fabrikam ' ta, Sam 'nin başlığı yüklenici-sanal makinelerdir. Contoso.com ' de, sanal makineleri yönetmek için Sam sorumlu değildir ve yetkilendirilir. Fabrikam.com ' de, bu tek iş işlevidir. Hala Sam, Contoso.com tarafından verilen kimlik bilgileri olan ve izlemek için yalnızca bir kimlik bilgileri kümesine sahip.

Başarılı bir `acquireToken` çağrısı yapıldıktan sonra, daha sonra `acquireTokenSilent` isteklerinde kullanılabilecek bir `IAccount` nesnesine bir başvuru görürsünüz.

### <a name="imultitenantaccount"></a>Iultitenantaccount

Hesabın gösterdiği her kiracıdan bir hesap hakkında talebe erişen bir uygulamanız varsa, `IAccount` nesnelerini `IMultiTenantAccount` ' e çevirebilirsiniz. Bu arabirim, kiracı KIMLIĞINE göre anahtarlı `ITenantProfiles` ' ı sağlar. Bu, geçerli hesaba göre, belirteci istediğiniz kiracıların her birinde hesaba ait olan taleplere erişmenizi sağlar.

@No__t-0 ve `IMultiTenantAccount` ' in kökündeki talepler her zaman giriş kiracısından gelen talepleri içerir. Ana Kiracıdaki bir belirteç için henüz bir istek yapmadıysanız, bu koleksiyon boş olur.

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

## <a name="migrate-to-the-new-exceptions"></a>Yeni özel durumlara geçiş yap

ADAL içinde, `ADALError` sabit listesi değerini alma yöntemi içeren `AuthenticationException` olan bir özel durum türü vardır.
MSAL ' de, özel durumların bir hiyerarşisi vardır ve her biri kendi ilişkili özel hata kodları kümesine sahiptir.

MSAL özel durumlarının listesi

|Özel Durum  | Açıklama  |
|---------|---------|
| `MsalException`     | MSAL tarafından oluşturulan varsayılan özel durum.  |
| `MsalClientException`     | Hata istemci tarafı ise oluşturulur. |
| `MsalArgumentException`     | Bir veya daha fazla giriş bağımsız değişkeni geçersiz ise oluşturulur. |
| `MsalClientException`     | Hata istemci tarafı ise oluşturulur. |
| `MsalServiceException`     | Hata sunucu tarafında ise oluşturulur. |
| `MsalUserCancelException`     | Kullanıcı kimlik doğrulama akışını iptal edildiyse oluşturulur.  |
| `MsalUiRequiredException`     | Belirteç sessizce yenilenmezse oluşturulur.  |
| `MsalDeclinedScopeException`     | Bir veya daha fazla istenen kapsam sunucu tarafından reddedildiyse oluşturulur.  |
| `MsalIntuneAppProtectionPolicyRequiredException` | Kaynakta MAMCA koruma ilkesi etkinse oluşturulur. |

### <a name="adalerror-to-msalexception-errorcode"></a>MsalException hata kodu ile ADALError

### <a name="adal-logging-to-msal-logging"></a>ADAL günlüğü MSAL günlüğe kaydediliyor

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
