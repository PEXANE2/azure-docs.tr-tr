---
title: Azure Active Directory B2C'deki kullanıcı profili öznitelikleri
description: Azure AD B2C dizin kullanıcı profili tarafından desteklenen kullanıcı kaynağı türü öznitelikleri hakkında bilgi edinin. Yerleşik öznitelikler, uzantılar ve özniteliklerin Microsoft Graph ile nasıl eşene yol gösteriş leri hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e921f0a40f53b1d08831047d1cb89ca26de41402
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80057301"
---
# <a name="user-profile-attributes"></a>Kullanıcı profili öznitelikleri

Azure Etkin Dizin (Azure AD) B2C dizin kullanıcı profiliniz, verilen ad, soyad, şehir, posta kodu ve telefon numarası gibi yerleşik bir öznitelikler kümesiyle birlikte gelir. Harici bir veri deposugerektirmeden kullanıcı profilini kendi uygulama verilerinizle genişletebilirsiniz. Azure AD B2C kullanıcı profilleriyle kullanılabilecek özniteliklerin çoğu Microsoft Graph tarafından da desteklenir. Bu makalede desteklenen Azure AD B2C kullanıcı profili öznitelikleri açıklanmaktadır. Ayrıca, Microsoft Graph tarafından desteklenmeyen bu özniteliklerin yanı sıra Azure AD B2C ile kullanılmaması gereken Microsoft Graph özniteliklerini de belirtir.

> [!IMPORTANT]
> Hesap kimlik bilgileri, devlet kimlik numaraları, kart sahibi verileri, mali hesap verileri, sağlık bilgileri veya hassas arka plan bilgileri gibi hassas kişisel verileri depolamak için yerleşik veya uzantılı öznitelikleri kullanmamalısınız.

Ayrıca dış sistemlerle entegre edebilirsiniz. Örneğin, kimlik doğrulaması için Azure AD B2C'yi kullanabilirsiniz, ancak müşteri verilerinin yetkili kaynağı olarak harici bir müşteri ilişkileri yönetimi (CRM) veya müşteri sadakatveritabanına temsilci olarak temsilci lik yapabilirsiniz. Daha fazla bilgi için [uzak profil](https://github.com/azure-ad-b2c/samples/tree/master/policies/remote-profile) çözümüne bakın.

Aşağıdaki tabloda Azure AD B2C dizin kullanıcı profili tarafından desteklenen [kullanıcı kaynağı türü](https://docs.microsoft.com/graph/api/resources/user) öznitelikleri listelenir. Her öznitelik hakkında aşağıdaki bilgileri verir:

- Azure AD B2C tarafından kullanılan öznitelik adı (farklıysa parantez içinde Microsoft Graph adı)
- Öznitelik veri türü
- Öznitelik açıklaması
- Özellik Azure portalında kullanılabilirse
- Öznitelik bir kullanıcı akışında kullanılabilse
- Öznitelik özel bir ilke [Azure AD teknik profilinde](active-directory-technical-profile.md) ve&lt;hangi&gt;bölümde &lt;kullanılabilen (Giriş Talepleri , Çıktı Talepleri&gt;veya &lt;Kalıcı Talepler&gt;)

|Adı     |Tür     |Açıklama|Azure portalında|Kullanıcı akışları|Özel ilke|
|---------|---------|----------|------------|----------|-------------|
|hesapEtkin  |Boole|Kullanıcı hesabı etkin veya devre dışı olsun: hesap etkinse **doğrudur,** aksi takdirde **yanlış**.|Evet|Hayır|Kalıcı, Çıktı|
|ageGroup        |Dize|Kullanıcının yaş grubu. Olası değerler: null, Tanımlanmamış, Minör, Yetişkin, NotAdult.|Evet|Hayır|Kalıcı, Çıktı|
|alternativeSecurityId ([Kimlikler](manage-user-accounts-graph-api.md#identities-property))|Dize|Dış kimlik sağlayıcısından tek bir kullanıcı kimliği.|Hayır|Hayır|Giriş, Kalıcı, Çıktı|
|alternativeSecurityIds ([Kimlikler](manage-user-accounts-graph-api.md#identities-property))|alternatif securityId toplama|Dış kimlik sağlayıcılardan kullanıcı kimlikleri koleksiyonu.|Hayır|Hayır|Kalıcı, Çıktı|
|city            |Dize|Kullanıcının bulunduğu şehir. Maksimum uzunluk 128.|Evet|Evet|Kalıcı, Çıktı|
|rızaProvidedForMinor|Dize|Reşit olmayan bir çocuk için izin verilip verilmedi. İzin verilen değerler: geçersiz, verilen, reddedilen veya gerekli olmayan.|Evet|Hayır|Kalıcı, Çıktı|
|ülke         |Dize|Kullanıcının bulunduğu ülke/bölge. Örnek: "ABD" veya "İngiltere". Maksimum uzunluk 128.|Evet|Evet|Kalıcı, Çıktı|
|createdDateTime|DateTime|Kullanıcı nesnesinin oluşturulduğu tarih. Sadece okuyun.|Hayır|Hayır|Kalıcı, Çıktı|
|creationType    |Dize|Kullanıcı hesabı, Azure Etkin Dizin B2C kiracısı için yerel bir hesap olarak oluşturulduysa, değer LocalAccount veya nameCoexistence'dır. Sadece okuyun.|Hayır|Hayır|Kalıcı, Çıktı|
|tarihOfBirth     |Tarih|Doğum tarihi.|Hayır|Hayır|Kalıcı, Çıktı|
|bölüm      |Dize|Kullanıcının çalıştığı bölümün adı. Maksimum uzunluk 64.|Evet|Hayır|Kalıcı, Çıktı|
|displayName     |Dize|Kullanıcının görüntü adı. Maksimum uzunluk 256.|Evet|Evet|Kalıcı, Çıktı|
|faksTelefonNumarası<sup>1</sup>|Dize|Kullanıcının iş faks makinesinin telefon numarası.|Evet|Hayır|Kalıcı, Çıktı|
|givenName       |Dize|Kullanıcının verilen adı (adı). Maksimum uzunluk 64.|Evet|Evet|Kalıcı, Çıktı|
|jobTitle        |Dize|Kullanıcının iş unvanı. Maksimum uzunluk 128.|Evet|Evet|Kalıcı, Çıktı|
|değişmezId     |Dize|Genellikle şirket içi Active Directory'den geçirilen kullanıcılar için kullanılan bir tanımlayıcı.|Hayır|Hayır|Kalıcı, Çıktı|
|yasalAgeGroupClassification|Dize|Yasal yaş grubu sınıflandırması. Salt okunur ve ageGroup ve consentProvidedForMinor özelliklerine göre hesaplanır. İzin verilen değerler: null, minorWithOutParentalConsent, minorWithParentalConsent, minorNoParentalConsentRequired, notAdult ve adult.|Evet|Hayır|Kalıcı, Çıktı|
|legalÜlke<sup>1</sup>  |Dize|Yasal amaçlar için ülke.|Hayır|Hayır|Kalıcı, Çıktı|
|posta            |Dize|Örneğin, kullanıcı için SMTP adresi,bob@contoso.com" ". Salt okunur.|Hayır|Hayır|Kalıcı, Çıktı|
|mailNickName    |Dize|Kullanıcının posta takma adı. Maksimum uzunluk 64.|Hayır|Hayır|Kalıcı, Çıktı|
|cep telefonu (cep telefonu) |Dize|Kullanıcı için birincil cep telefonu numarası. Maksimum uzunluk 64.|Evet|Hayır|Kalıcı, Çıktı|
|netId           |Dize|Net kimlik.|Hayır|Hayır|Kalıcı, Çıktı|
|Objectıd        |Dize|Kullanıcı için benzersiz tanımlayıcı olan genel olarak benzersiz tanımlayıcı (GUID). Örnek: 12345678-9abc-def0-1234-56789abcde. Sadece oku, Değişmez.|Salt okunur|Evet|Giriş, Kalıcı, Çıktı|
|otherMails      |String koleksiyonu|Kullanıcı için ek e-posta adreslerinin listesi. Örnek: ["bob@contoso.com", "Robert@fabrikam.com"].|Evet (Alternatif e-posta)|Hayır|Kalıcı, Çıktı|
|password        |Dize|Kullanıcı oluşturma sırasında yerel hesabın parolası.|Hayır|Hayır|Kalıcı|
|passwordPolitikalar     |Dize|Parolanın ilkesi. Virgülle ayrılmış farklı ilke adlarından oluşan bir dize. yani "DisablePasswordExpiration, DisableStrongPassword".|Hayır|Hayır|Kalıcı, Çıktı|
|physicalDeliveryOfficeName (officeLocation)|Dize|Kullanıcının iş yerindeki ofis konumu. Maksimum uzunluk 128.|Evet|Hayır|Kalıcı, Çıktı|
|Postakodu      |Dize|Kullanıcının posta adresiiçin posta kodu. Posta kodu kullanıcının ülkesine/bölgesine özgüdür. Amerika Birleşik Devletleri'nde bu özellik posta kodunu içerir. Maksimum uzunluk 40.|Evet|Hayır|Kalıcı, Çıktı|
|tercihDil    |Dize|Kullanıcı için tercih edilen dil. ISO 639-1 Kodu'na uymalıdır. Örnek: "en-US".|Hayır|Hayır|Kalıcı, Çıktı|
|yenilemeTokensValidFromDateTime|DateTime|Bu saatten önce verilen yenileme belirteçleri geçersizdir ve yeni bir erişim belirteci elde etmek için geçersiz bir yenileme belirteci kullanırken uygulamalar bir hata alır. Bu durumda, uygulamanın yetkili bitiş noktasına istekte bulunarak yeni bir yenileme belirteci edinmesi gerekir. Salt okunur.|Hayır|Hayır|Çıktı|
|signInnames ([Kimlikler](manage-user-accounts-graph-api.md#identities-property)) |Dize|Dizindeki herhangi bir türdeki yerel hesap kullanıcısının benzersiz oturum açma adı. Yerel hesap türünü belirtmeden oturum açma değeri olan bir kullanıcıyı almak için bunu kullanın.|Hayır|Hayır|Giriş|
|signInNames.userName ([Kimlikler](manage-user-accounts-graph-api.md#identities-property)) |Dize|Dizindeki yerel hesap kullanıcısının benzersiz kullanıcı adı. Belirli bir oturum açma kullanıcı adı olan bir kullanıcı oluşturmak veya almak için bunu kullanın. Yama işlemi sırasında tek başına Kalıcı Talepler'de bunu belirtmek diğer signInNames türlerini kaldırır. Yeni bir signInNames türü eklemek istiyorsanız, varolan signInNames'i de devam ettirebilmeniz gerekir.|Hayır|Hayır|Giriş, Kalıcı, Çıktı|
|signInNames.phoneNumber ([Kimlikler](manage-user-accounts-graph-api.md#identities-property)) |Dize|Dizindeki yerel hesap kullanıcısının benzersiz telefon numarası. Belirli bir oturum açma telefon numarasına sahip bir kullanıcı oluşturmak veya almak için bunu kullanın. Yama işlemi sırasında tek başına Kalıcı Talepler'de bunu belirtmek diğer signInNames türlerini kaldırır. Yeni bir signInNames türü eklemek istiyorsanız, varolan signInNames'i de devam ettirebilmeniz gerekir.|Hayır|Hayır|Giriş, Kalıcı, Çıktı|
|signInNames.emailAddress ([Kimlikler](manage-user-accounts-graph-api.md#identities-property))|Dize|Dizindeki yerel hesap kullanıcısının benzersiz e-posta adresi. Belirli bir oturum açma e-posta adresine sahip bir kullanıcı oluşturmak veya almak için bunu kullanın. Yama işlemi sırasında tek başına Kalıcı Talepler'de bunu belirtmek diğer signInNames türlerini kaldırır. Yeni bir signInNames türü eklemek istiyorsanız, varolan signInNames'i de devam ettirebilmeniz gerekir.|Hayır|Hayır|Giriş, Kalıcı, Çıktı|
|durum           |Dize|Kullanıcının adresindeki eyalet veya bölge. Maksimum uzunluk 128.|Evet|Evet|Kalıcı, Çıktı|
|Streetaddress   |Dize|Kullanıcının iş yerinin sokak adresi. Maksimum uzunluk 1024.|Evet|Evet|Kalıcı, Çıktı|
|güçlü Kimlik Doğrulama AlternatifiTelefon Sayısı<sup>1</sup>|Dize|Çok faktörlü kimlik doğrulama için kullanılan kullanıcının ikincil telefon numarası.|Evet|Hayır|Kalıcı, Çıktı|
|güçlü AuthenticationEmailAddress<sup>1</sup>|Dize|Kullanıcı için SMTP adresi. Örnek:bob@contoso.com" " Bu öznitelik, kullanıcı e-posta adresini depolamak için kullanıcı adı ilkesiyle oturum açma için kullanılır. E-posta adresi daha sonra parola sıfırlama akışında kullanılır.|Evet|Hayır|Kalıcı, Çıktı|
|güçlü AuthenticationTelefon Numarası<sup>1</sup>|Dize|Çok faktörlü kimlik doğrulama için kullanılan kullanıcının birincil telefon numarası.|Evet|Hayır|Kalıcı, Çıktı|
|surname         |Dize|Kullanıcının soyadı (soyadı veya soyadı). Maksimum uzunluk 64.|Evet|Evet|Kalıcı, Çıktı|
|telephoneNumber (businessPhones ilk giriş)|Dize|Kullanıcının iş yerinin birincil telefon numarası.|Evet|Hayır|Kalıcı, Çıktı|
|userPrincipalName    |Dize|Kullanıcının kullanıcı asıl adı (UPN). UPN, Kullanıcı için Internet standardı RFC 822'ye dayalı internet tarzı bir giriş adıdır. Etki alanı, kiracının doğrulanmış etki alanları koleksiyonunda bulunmalıdır. Bir hesap oluşturulduğunda bu özellik gereklidir. Değişmez.|Hayır|Hayır|Giriş, Kalıcı, Çıktı|
|kullanımKonum   |Dize|Yasal zorunluluk nedeniyle lisans verilecek kullanıcılar için gerekli olan hizmetler, ülkelerdeki hizmetlerin kullanılabilirliğini kontrol etmek için gereklidir. Hükümsüz değil. İki harfli ülke kodu (ISO standardı 3166). Örnekler: "ABD", "JP" ve "GB".|Evet|Hayır|Kalıcı, Çıktı|
|Usertype        |Dize|Dizininizde kullanıcı türlerini sınıflandırmak için kullanılabilecek bir dize değeri. Değer Üye olmalıdır. Salt okunur.|Salt okunur|Hayır|Kalıcı, Çıktı|
|userState (hariciUserState)<sup>2</sup>|Dize|Yalnızca Azure AD B2B hesabı için, davetin Beklemeli Kabul veya Kabul olup olmadığını gösterir.|Hayır|Hayır|Kalıcı, Çıktı|
|userStateChangedOn (hariciUserStateChangeDateTime)<sup>2</sup>|DateTime|UserState özelliğinde yapılan en son değişikliğin zaman damgasını gösterir.|Hayır|Hayır|Kalıcı, Çıktı|
|<sup>1.1.2</sup> Microsoft Graph tarafından desteklenmiyor<br><sup>2.000</sup> Azure AD B2C ile kullanılmamalıdır||||||


## <a name="extension-attributes"></a>Uzantı öznitelikleri

Genellikle aşağıdaki durumlarda olduğu gibi kendi özniteliklerinizi oluşturmanız gerekir:

- Müşteriye yönelik bir uygulamanın **LoyaltyNumber**gibi bir öznitelik için devam etmesi gerekir.
- Bir kimlik sağlayıcısının kaydedilmesi gereken **uniqueUserGUID** gibi benzersiz bir kullanıcı tanımlayıcısı vardır.
- Özel bir kullanıcı yolculuğunun, **geçiş durumu**gibi bir kullanıcı durumu için devam etmesi gerekir.

Azure AD B2C, her kullanıcı hesabında depolanan öznitelikler kümesini genişletir. Uzantı öznitelikleri dizindeki kullanıcı nesnelerinin [şema sını genişletir.](https://docs.microsoft.com/graph/extensibility-overview#schema-extensions) Uzantı öznitelikleri, kullanıcı için veri içerse ler bile yalnızca bir uygulama nesnesi üzerinde kaydedilebilir. Uzantı özniteliği b2c-extensions-app adı verilen uygulamaya eklenir. Azure AD B2C tarafından kullanıcı verilerini depolamak için kullanıldığından bu uygulamayı değiştirmeyin. Bu uygulamayı Azure Active Directory App kayıtları altında bulabilirsiniz.

> [!NOTE]
> - Herhangi bir kullanıcı hesabına en fazla 100 uzantı öznitelikleri yazılabilir.
> - b2c uzantıları-uygulama uygulaması silinirse, bu uzantı öznitelikleri içerdikleri verilerle birlikte tüm kullanıcılardan kaldırılır.
> - Uygulama tarafından bir uzantı özniteliği silinirse, tüm kullanıcı hesaplarından kaldırılır ve değerler silinir.
> - Uzantı özniteliğinin altında yatan ad "Extension_" + Uygulama Kimliği + "_" + Öznitelik adı biçiminde oluşturulur. Örneğin, bir uzantı özniteliği LoyaltyNumber oluşturursanız ve b2c-extensions-app Application ID 831374b3-bd50-41bf-aa54-263ec9e050fc ise, altta yatan uzantı öznitelik adı olacaktır: extension_831374b3bd5041bfaa54263ec9e050fc_ Sadakat Numarası. Kullanıcı hesapları oluşturmak veya güncelleştirmek için Grafik API sorgularını çalıştırdığınızda temel adı kullanırsınız.

Şema uzantısında bir özellik tanımlanırken aşağıdaki veri türleri desteklenir:

|Özellik türü |Açıklamalar  |
|--------------|---------|
|Boole    | Olası değerler: **doğru** veya **yanlış**. |
|DateTime   | ISO 8601 formatında belirtilmelidir. UTC'de saklanır.   |
|Tamsayı    | 32 bit değerinde.               |
|Dize     | En fazla 256 karakter.     |

## <a name="next-steps"></a>Sonraki adımlar
Uzantı öznitelikleri hakkında daha fazla bilgi edinin:
- [Şema uzantıları](https://docs.microsoft.com/graph/extensibility-overview#schema-extensions)
- [Kullanıcı akışıyla özel öznitelikleri tanımlama](user-flow-custom-attributes.md)
- [Özel ilke ile özel öznitelikleri tanımlama](custom-policy-custom-attributes.md)
