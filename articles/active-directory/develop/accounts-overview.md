---
title: Android 'de kiracı profillerini & Microsoft Identity platform hesapları | Mavisi
description: Android için Microsoft Identity platform hesaplarına genel bakış
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.devlang: java
ms.date: 09/14/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman
ms.openlocfilehash: d0497ad68e7b29e6d8c83dd860ba8f509e229579
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77611874"
---
# <a name="accounts--tenant-profiles-android"></a>Hesaplar ve kiracı profilleri (Android)

Bu makalede `account` , Microsoft Identity platformunda ne olduğuna ilişkin bir genel bakış sunulmaktadır.

Microsoft kimlik doğrulama kitaplığı (MSAL) API 'SI, *Kullanıcı* terimi *hesabını hesaba*koyar. Bir nedeni, bir kullanıcının (insan veya yazılım Aracısı) birden çok hesabı olabileceği ya da kullanabileceği bir nedendir. Bu hesaplar kullanıcının kendi kuruluşunda ve/veya kullanıcının üyesi olduğu diğer kuruluşlarda yer alabilir.

Microsoft Identity platformunda bir hesap aşağıdakilerden oluşur:

- Benzersiz bir tanımlayıcı.  
- Hesabın sahipliğini/denetimini göstermek için kullanılan bir veya daha fazla kimlik bilgisi.
- Öznitelikleri içeren bir veya daha fazla profil:
  - Resim, verilen ad, Aile adı, başlık, ofis konumu
- Hesap, bir yetki kaynağına veya kayıt sistemine sahiptir. Bu, hesabın oluşturulduğu ve bu hesapla ilişkili kimlik bilgilerinin depolandığı sistemdir. Microsoft Identity platformu gibi çok kiracılı sistemlerde, kayıt sistemi `tenant` hesabın oluşturulduğu yerdir. Bu kiracıya da denir `home tenant` .
- Microsoft Identity platform 'daki hesaplara aşağıdaki kayıt sistemleri sahiptir:
  - Azure Active Directory B2C dahil Azure Active Directory.
  - Microsoft hesabı (canlı).
- Microsoft Identity platformu dışındaki kayıt sistemlerinden gelen hesaplar, aşağıdakiler de dahil olmak üzere Microsoft Identity platformu dahilinde temsil edilir:
  - bağlı şirket içi dizinlerden kimlikler (Windows Server Active Directory)
  - LinkedIn, GitHub vb. kaynaklı dış kimlikler.
  Bu durumlarda, bir hesabın hem kaynak sistemi hem de Microsoft Identity platformu içindeki bir kayıt sistemi vardır.
- Microsoft Identity platformu, birden fazla kuruluşa (Azure Active Directory kiracılar) ait kaynaklara erişmek için bir hesabın kullanılmasına izin verir.
  - Bir kayıt sisteminden (AAD kiracı A) bir hesabın, başka bir kayıt sistemindeki (AAD kiracısı B) bir kaynağa erişimi olduğunu kaydetmek için, hesabın, kaynağın tanımlandığı kiracıda temsil edilebilmesi gerekir. Bu, sistem B 'deki sistem A 'dan hesabın yerel bir kaydı oluşturularak yapılır.
  - Hesabın temsili olan bu yerel kayıt, özgün hesaba bağlanır.
  - MSAL bu yerel kaydı bir olarak gösterir `Tenant Profile` .
  - Kiracı profili, Iş unvanı, ofis konumu, Iletişim bilgileri vb. gibi yerel bağlamla ilgili farklı özniteliklere sahip olabilir.
- Bir hesap bir veya daha fazla kiracıda mevcut olabileceğinden, hesap birden fazla profile sahip olabilir.

> [!NOTE]
> MSAL, Microsoft hesabı sistemine (canlı, MSA) Microsoft Identity platform içinde başka bir kiracı olarak davranır. Microsoft hesabı kiracının Kiracı kimliği:`9188040d-6c67-4c5b-b112-36a304b66dad`

## <a name="account-overview-diagram"></a>Hesaba genel bakış Diyagramı

![Hesaba genel bakış Diyagramı](./media/accounts-overview/accounts-overview.svg)

Yukarıdaki diyagramda:

- Hesap, Şirket `bob@contoso.com` Içi Windows Server Active Directory (kaynak şirket içi kayıt sisteminde) oluşturulur.
- Hesap `tom@live.com` Microsoft hesabı kiracısında oluşturulur.
- `bob@contoso.com`, aşağıdaki Azure Active Directory kiracılarda en az bir kaynağa erişebilir:
  - contoso.com (kaydın bulut sistemi-şirket içi kayıt sistemine bağlı)
  - fabrikam.com
  - woodgrovebank.com
  - `bob@contoso.com`Bu kiracıların her birinde için bir kiracı profili bulunur.
- `tom@live.com`, aşağıdaki Microsoft kiracılarındaki kaynaklara erişebilir:
  - contoso.com
  - fabrikam.com
  - `tom@live.com`Bu kiracıların her birinde için bir kiracı profili bulunur.
- Diğer kiracılardaki Tom ve Bob hakkındaki bilgiler, kayıt sisteminde farklılık gösterebilir. Bunlar, Iş unvanı, ofis konumu vb. gibi özniteliklere göre farklılık gösterebilir. Gruplar ve/veya rollerinin üyeleri (Azure Active Directory kiracısı) olabilir. Bu bilgilere kiracı profili olarak değintik bob@contoso.com .

Diyagramda bob@contoso.com ve tom@live.com farklı Azure Active Directory kiracılarındaki kaynaklara erişimi vardır. Daha fazla bilgi için [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleme](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator)bölümüne bakın.

## <a name="accounts-and-single-sign-on-sso"></a>Hesaplar ve çoklu oturum açma (SSO)

MSAL belirteç önbelleği, hesap başına *tek bir yenileme belirteci* depolar. Bu yenileme belirteci, birden çok Microsoft kimlik platformu kiracısından sessizce erişim belirteçleri istemek için kullanılabilir. Bir aracı bir cihaza yüklendiğinde, hesap aracı tarafından yönetilir ve cihaz genelinde çoklu oturum açma mümkün olur.

> [!IMPORTANT]
> İşletmeden müşteriye (B2C) hesabı ve yenileme belirteci davranışı, Microsoft Identity platformunun geri kalanından farklıdır. Daha fazla bilgi için bkz. [B2C ilkeleri & hesapları](#b2c-policies--accounts).

## <a name="account-identifiers"></a>Hesap tanımlayıcıları

MSAL hesap KIMLIĞI, bir hesap nesne KIMLIĞI değil. Microsoft Identity platform içinde benzersizlik dışında herhangi bir şeyi iletmek için ayrıştırılıp/veya bir şekilde güvenilmez.

Azure AD kimlik doğrulama kitaplığı (ADAL) ile uyumluluk ve ADAL 'den MSAL 'e geçiş kolaylığı sağlamak için MSAL, MSAL önbelleğinde bulunan hesap için geçerli bir tanımlayıcı kullanarak hesapları arayabilir.  Örneğin, tom@live.com her bir tanımlayıcı geçerli olduğundan aşağıdakiler için her zaman aynı hesap nesnesini alır:

```java
// The following would always retrieve the same account object for tom@live.com because each identifier is valid

IAccount account = app.getAccount("<tome@live.com msal account id>");
IAccount account = app.getAccount("<tom@live.com contoso user object id>");
IAccount account = app.getAccount("<tom@live.com woodgrovebank user object id>");
```

## <a name="accessing-claims-about-an-account"></a>Hesap hakkındaki taleplere erişme

Bir erişim belirteci istemenin yanı sıra, MSAL her bir kiracıdan bir KIMLIK belirteci de ister. Bunu her zaman aşağıdaki kapsamları isteyerek yapar:

- OpenID
- profil

KIMLIK belirteci, taleplerin bir listesini içerir. `Claims`hesap hakkında ad/değer çiftleridir ve isteği yapmak için kullanılır.

Daha önce belirtildiği gibi, bir hesabın mevcut olduğu her kiracı, hesap hakkında: iş unvanı, ofis konumu vb. dahil olmak üzere farklı bilgileri depolayabileceği, ancak bunlarla sınırlı olmamak üzere.

Bir hesap birden fazla kuruluşta üye veya Konuk olabileceğinden, MSAL, hesabın üyesi olduğu kiracıların bir listesini almak üzere bir hizmeti sorgulayamaz. Bunun yerine, MSAL, yapılmış olan belirteç isteklerinin bir sonucu olarak hesabın bulunduğu kiracıların bir listesini oluşturur.

Hesap nesnesi üzerinde sunulan talepler her zaman bir hesap için ' ana kiracı '/{Authority} ' deki taleplerdir. Bu hesap, ana kiracının bir belirtecini istemek için kullanılmıyorsa, MSAL hesap nesnesi aracılığıyla talep sağlayamaz.  Örneğin:

```java
// Psuedo Code
IAccount account = getAccount("accountid");

String username = account.getClaims().get("preferred_username");
String tenantId = account.getClaims().get("tid"); // tenant id
String objectId = account.getClaims().get("oid"); // object id
String issuer = account.getClaims().get("iss"); // The tenant specific authority that issued the id_token
```

> [!TIP]
> Hesap nesnesinden kullanılabilen taleplerin listesini görmek için [bir id_token talepler](https://docs.microsoft.com/azure/active-directory/develop/id-tokens#claims-in-an-id_token) bölümüne bakın

> [!TIP]
> İd_token ek talepler eklemek için, [nasıl yapılır: Azure AD uygulamanıza isteğe bağlı talepler sağlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims) bölümündeki isteğe bağlı talep belgelerine bakın

### <a name="access-tenant-profile-claims"></a>Kiracı profili taleplerine erişin

Başka kiracılarda göründüğü gibi bir hesap hakkındaki taleplere erişmek için, önce hesap nesnesini ' e atamalısınız `IMultiTenantAccount` . Tüm hesaplar çok kiracılı olabilir, ancak MSAL aracılığıyla kullanılabilir kiracı profillerinin sayısı, geçerli hesabı kullanarak belirteçleri istediğiniz kiracılara göre belirlenir.  Örneğin:

```java
// Psuedo Code
IAccount account = getAccount("accountid");
IMultiTenantAccount multiTenantAccount = (IMultiTenantAccount)account;

multiTenantAccount.getTenantProfiles().get("tenantid for fabrikam").getClaims().get("family_name");
multiTenantAccount.getTenantProfiles().get("tenantid for contoso").getClaims().get("family_name");
```

## <a name="b2c-policies--accounts"></a>B2C ilkeleri & hesapları

Hesap için belirteçleri Yenile, B2C ilkeleri arasında paylaşılmaz. Sonuç olarak, belirteçler kullanan çoklu oturum açma mümkün değildir. Bunun anlamı çoklu oturum açma mümkün değildir. Çoklu oturum açma 'nın çoklu oturum açmayı etkinleştirmek için bir tanımlama bilgisinin kullanılabildiği etkileşimli bir deneyim kullanması gerektiği anlamına gelir.

Bu Ayrıca, MSAL durumunda farklı B2C ilkeleri kullanarak belirteçler elde ediyorsanız, bunların her biri kendi tanımlayıcısına sahip ayrı hesaplar olarak değerlendirilir. Kullanarak bir belirteç istemek için bir hesap kullanmak istiyorsanız `acquireTokenSilent` , belirteç isteğiyle kullanmakta olduğunuz ilkeyle eşleşen hesap listesinden hesabı seçmeniz gerekir. Örneğin:

```java
// Get Account For Policy

String policyId = "SignIn";
IAccount signInPolicyAccount = getAccountForPolicyId(app, policyId);

private IAccount getAccountForPolicy(IPublicClientApplication app, String policyId)
{
    List<IAccount> accounts = app.getAccounts();

    foreach(IAccount account : accounts)
   {
        if (account.getClaims().get("tfp").equals(policyId))
        {
            return account;
        }
    }

    return null;
}
```
