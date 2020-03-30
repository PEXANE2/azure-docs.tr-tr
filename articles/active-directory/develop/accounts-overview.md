---
title: Android'deki kiracı profilleri & Microsoft kimlik platformu hesapları | Azure
description: Android için Microsoft kimlik platformu hesaplarına genel bakış
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77611874"
---
# <a name="accounts--tenant-profiles-android"></a>Hesaplar ve kiracı profilleri (Android)

Bu makalede, Microsoft kimlik `account` platformunda ne olduğuna genel bir bakış sağlar.

Microsoft Kimlik Doğrulama Kitaplığı (MSAL) API terimi *kullanıcı* terimi *yerine dönem hesabı.* Bunun nedenlerinden biri, bir kullanıcının (insan veya yazılım aracısı) birden çok hesabı olması veya bunları kullanabileceğidir. Bu hesaplar, kullanıcının kendi kuruluşunda ve/veya kullanıcının üyesi olduğu diğer kuruluşlarda olabilir.

Microsoft kimlik platformundaki bir hesap aşağıdakilerden oluşur:

- Benzersiz bir tanımlayıcı.  
- Hesabın sahipliğini/denetimini göstermek için kullanılan bir veya daha fazla kimlik bilgileri.
- Aşağıdaki gibi özniteliklerden oluşan bir veya daha fazla profil:
  - Resim, Verilen Ad, Soyadı, Unvanı, Ofis Konumu
- Bir hesabın yetki kaynağı veya kayıt sistemi vardır. Bu, hesabın oluşturulduğu ve bu hesapla ilişkili kimlik bilgilerinin depolandığı sistemdir. Microsoft kimlik platformu gibi çok kiracılı sistemlerde, `tenant` kayıt sistemi hesabın oluşturulduğu yerdir. Bu kiracı da olarak `home tenant`adlandırılır.
- Microsoft kimlik platformundaki hesaplar aşağıdaki kayıt sistemlerine sahiptir:
  - Azure Etkin Dizini, Azure Etkin Dizini B2C dahil.
  - Microsoft hesabı (Live).
- Microsoft kimlik platformu dışındaki kayıt sistemlerine ait hesaplar, aşağıdakiler de dahil olmak üzere Microsoft kimlik platformunda temsil edilmektedir:
  - bağlı şirket içi dizinlerden kimlikler (Windows Server Active Directory)
  - LinkedIn, GitHub ve benzeri harici kimlikler.
  Bu gibi durumlarda, bir hesabın hem bir başlangıç kaydı sistemi hem de Microsoft kimlik platformu içinde bir kayıt sistemi vardır.
- Microsoft kimlik platformu, birden çok kuruluşa (Azure Etkin Dizin kiracıları) ait kaynaklara erişmek için bir hesabın kullanılmasına izin verir.
  - Bir kayıt sisteminden (AAD Kiracı A) bir hesabın başka bir kayıt sistemindeki (AAD Kiracı B) kaynağa erişimi olduğunu kaydetmek için, hesabın kaynağın tanımlandığı kiracıda temsil edilmesi gerekir. Bu, B sistemindeki A sisteminden hesabın yerel bir kaydını oluşturarak yapılır.
  - Hesabın temsili olan bu yerel kayıt, özgün hesaba bağlıdır.
  - MSAL bu yerel kaydı `Tenant Profile`.
  - Kiracı Profili, İş Unvanı, Ofis Konumu, İletişim Bilgileri gibi yerel içeriğe uygun farklı özniteliklere sahip olabilir.
- Bir hesap bir veya daha fazla kiracıda bulunabileceğinden, bir hesabın birden fazla profili olabilir.

> [!NOTE]
> MSAL, Microsoft hesap sistemini (Live, MSA) Microsoft kimlik platformunda başka bir kiracı olarak ele adatır. Microsoft hesabı kiracısının kiracı kimliği:`9188040d-6c67-4c5b-b112-36a304b66dad`

## <a name="account-overview-diagram"></a>Hesaba genel bakış diyagramı

![Hesaba Genel Bakış Diyagramı](./media/accounts-overview/accounts-overview.svg)

Yukarıdaki diyagramda:

- Hesap, `bob@contoso.com` şirket içi Windows Server Active Directory 'de (başlangıç şirket içi kayıt sistemi) oluşturulur.
- Hesap, `tom@live.com` Microsoft hesabı kiracısında oluşturulur.
- `bob@contoso.com`aşağıdaki Azure Etkin Dizin kiracılarından en az bir kaynağa erişebilir:
  - contoso.com (bulut kayıt sistemi - şirket içi kayıt sistemine bağlı)
  - fabrikam.com
  - woodgrovebank.com
  - Bu kiracıların `bob@contoso.com` her birinde bir kiracı profili bulunur.
- `tom@live.com`aşağıdaki Microsoft kiracılarında kaynaklara erişimi vardır:
  - contoso.com
  - fabrikam.com
  - Bu kiracıların `tom@live.com` her birinde bir kiracı profili bulunur.
- Diğer kiracılardaki Tom ve Bob hakkındaki bilgiler kayıt sistemindekinden farklı olabilir. İş başlığı, Office Konumu ve benzeri özelliklere göre farklılık gösterebilirler. Her kuruluş (Azure Etkin Dizin Kiracı) içindeki grupların ve/veya rollerin üyeleri olabilirler. Bu bilgilere kiracı bob@contoso.com profili olarak atıfta bulunuruz.

Diyagramda ve bob@contoso.com tom@live.com farklı Azure Etkin Dizin kiracılarının kaynaklarına erişin. Daha fazla bilgi için azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcıları ekle'ye](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator)bakın.

## <a name="accounts-and-single-sign-on-sso"></a>Hesaplar ve tek oturum açma (SSO)

MSAL belirteç önbelleği hesap başına tek bir *yenileme belirteci* depolar. Bu yenileme belirteci, birden çok Microsoft kimlik platformu kiracısından sessizce erişim belirteçleri istemek için kullanılabilir. Bir aracı bir aygıta yüklendiğinde, hesap broker tarafından yönetilir ve aygıt çapında tek oturum açma mümkün olur.

> [!IMPORTANT]
> Business to Consumer (B2C) hesabı ve yenileme belirteç davranışı Microsoft kimlik platformunun geri kalanından farklıdır. Daha fazla bilgi için [B2C İlkeleri & Hesapları'na](#b2c-policies--accounts)bakın.

## <a name="account-identifiers"></a>Hesap tanımlayıcıları

MSAL hesap kimliği bir hesap nesnesi kimliği değildir. Microsoft kimlik platformunda benzersizlikten başka bir şey iletmek için ayrıştıve/veya güvenilmesi amaçlanmaz.

Azure AD Kimlik Doğrulama Kitaplığı (ADAL) ile uyumluluk ve ADAL'dan MSAL'a Geçişi kolaylaştırmak için MSAL, MSAL önbelleğinde bulunan hesap için geçerli bir tanımlayıcı kullanarak hesapları arar.  Örneğin, tanımlayıcıların her biri geçerli olduğundan, aşağıdakiler her zaman aynı hesap nesnesini tom@live.com alır:

```java
// The following would always retrieve the same account object for tom@live.com because each identifier is valid

IAccount account = app.getAccount("<tome@live.com msal account id>");
IAccount account = app.getAccount("<tom@live.com contoso user object id>");
IAccount account = app.getAccount("<tom@live.com woodgrovebank user object id>");
```

## <a name="accessing-claims-about-an-account"></a>Bir hesapla ilgili taleplere erişim

MSAL, bir erişim jetonu istemenin yanı sıra her kiracıdan her zaman bir kimlik belirteci de ister. Bunu her zaman aşağıdaki kapsamları isteyerek yapar:

- Openıd
- profil

Kimlik belirteci, taleplerin listesini içerir. `Claims`hesapla ilgili ad/değer çiftleridir ve isteği yapmak için kullanılır.

Daha önce de belirtildiği gibi, bir hesabın bulunduğu her kiracı, iş unvanı, ofis konumu ve benzeri öznitelikler de dahil ancak bunlarla sınırlı olmamak üzere hesap hakkında farklı bilgiler depolayabilir.

Bir hesap birden çok kuruluşta üye veya konuk olsa da, MSAL, hesabın üyesi olduğu kiracıların listesini almak için bir hizmeti sorgulamaz. Bunun yerine, MSAL, yapılan belirteç istekleri nin bir sonucu olarak hesabın bulunduğu kiracıların bir listesini oluşturur.

Hesap nesnesi üzerinde ortaya çıkan talepler her zaman bir hesap için 'ev kiracı'/{yetkili} gelen taleplerdir. Bu hesap ev kiracıları için bir belirteç istemek için kullanılmamışsa, MSAL hesap nesnesi üzerinden talepte bulunamaz.  Örnek:

```java
// Psuedo Code
IAccount account = getAccount("accountid");

String username = account.getClaims().get("preferred_username");
String tenantId = account.getClaims().get("tid"); // tenant id
String objectId = account.getClaims().get("oid"); // object id
String issuer = account.getClaims().get("iss"); // The tenant specific authority that issued the id_token
```

> [!TIP]
> Hesap nesnesinden edinilebilen taleplerin listesini görmek için, [id_token](https://docs.microsoft.com/azure/active-directory/develop/id-tokens#claims-in-an-id_token)

> [!TIP]
> id_token ek talepler eklemek için, Azure REKLAM [uygulamanızda isteğe bağlı talep belgelerine bakın: Azure AD uygulamanızda isteğe bağlı talepler sağlayın](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims)

### <a name="access-tenant-profile-claims"></a>Kiracı profili taleplerine erişin

Bir hesapla ilgili iddialara diğer kiracılarda göründükleri gibi erişmek için `IMultiTenantAccount`öncelikle hesap nesnenizi ' e dökmeniz gerekir. Tüm hesaplar çok kiracılı olabilir, ancak MSAL aracılığıyla kullanılabilen kiracı profillerinin sayısı, geçerli hesabı kullanarak belirteçleri istediğiniz kiracı sayısına bağlıdır.  Örnek:

```java
// Psuedo Code
IAccount account = getAccount("accountid");
IMultiTenantAccount multiTenantAccount = (IMultiTenantAccount)account;

multiTenantAccount.getTenantProfiles().get("tenantid for fabrikam").getClaims().get("family_name");
multiTenantAccount.getTenantProfiles().get("tenantid for contoso").getClaims().get("family_name");
```

## <a name="b2c-policies--accounts"></a>B2C politikaları & hesapları

Bir hesabın belirteçlerini yenilemek B2C ilkeleri arasında paylaşılmaz. Sonuç olarak, belirteçleri kullanarak tek oturum açma mümkün değildir. Bu, tek bir oturum açmanın mümkün olmadığı anlamına gelmez. Bu, tek oturum açmanın tek bir oturum açmayı etkinleştirmek için bir çerezin kullanılabildiği etkileşimli bir deneyim kullanması anlamına gelir.

Bu aynı zamanda, MSAL durumunda, farklı B2C ilkeleri kullanarak belirteçleri elde ederseniz, o zaman bu ayrı hesaplar olarak kabul edilir - her biri kendi tanımlayıcısı ile anlamına gelir. Bir hesabı kullanarak `acquireTokenSilent`bir belirteç istemek için kullanmak istiyorsanız, kullandığınız ilkeyle eşleşen hesaplar listesinden hesabı belirteç isteğiyle seçmeniz gerekir. Örnek:

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
