---
title: Azure Active Directory B2C içindeki kullanıcı profili öznitelikleri
description: Azure AD B2C Directory Kullanıcı profili tarafından desteklenen Kullanıcı kaynak türü öznitelikleri hakkında bilgi edinin. Yerleşik öznitelikler, Uzantılar ve özniteliklerin Microsoft Graph eşleme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1e6965e15b7482935148ae7fcd2edf0f3cc722b2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83738567"
---
# <a name="user-profile-attributes"></a>Kullanıcı profili öznitelikleri

Azure Active Directory (Azure AD) B2C Dizin Kullanıcı profiliniz, verilen ad, soyadı, şehir, posta kodu ve telefon numarası gibi yerleşik bir öznitelik kümesi ile gelir. Bir dış veri deposu gerektirmeden kullanıcı profilini kendi uygulama verilerinize genişletebilirsiniz. Azure AD B2C Kullanıcı profilleriyle kullanılabilen özniteliklerin çoğu, Microsoft Graph tarafından da desteklenir. Bu makalede, Kullanıcı profili özniteliklerinin desteklenen Azure AD B2C açıklanmaktadır. Ayrıca, Microsoft Graph tarafından desteklenmeyen özniteliklerin yanı sıra Azure AD B2C ile kullanılmamalıdır Microsoft Graph özniteliklerini de not edin.

> [!IMPORTANT]
> Hesap kimlik bilgileri, kamu kimlik numaraları, kart sahibi verileri, finansal hesap verileri, sağlık bilgileri ve hassas arka plan bilgileri gibi hassas kişisel verileri depolamak için yerleşik veya uzantı öznitelikleri kullanmamalısınız.

Ayrıca, dış sistemlerle tümleştirebilirsiniz. Örneğin, kimlik doğrulaması için Azure AD B2C kullanabilir, ancak bir dış müşteri ilişki yönetimi (CRM) veya müşteri bağlılık programı veritabanını, müşteri verilerinin yetkili kaynağı olarak temsil edebilirsiniz. Daha fazla bilgi için bkz. [uzak profil](https://github.com/azure-ad-b2c/samples/tree/master/policies/remote-profile) çözümü.

Aşağıdaki tabloda, Azure AD B2C Directory Kullanıcı profili tarafından desteklenen [Kullanıcı kaynak türü](https://docs.microsoft.com/graph/api/resources/user) öznitelikleri listelenmiştir. Her öznitelikle ilgili aşağıdaki bilgileri verir:

- Azure AD B2C tarafından kullanılan öznitelik adı (Eğer farklıysa, parantez içinde Microsoft Graph adı)
- Öznitelik veri türü
- Öznitelik açıklaması
- Azure portal öznitelik varsa
- Öznitelik bir Kullanıcı akışında kullanılıyorsa
- Öznitelik, özel bir ilkede [Azure AD teknik profilinde](active-directory-technical-profile.md) ve bu bölümde ( &lt; ınputclaim &gt; , &lt; outputclaim &gt; veya &lt; PersistedClaims &gt; ) kullanılabilir

|Name     |Tür     |Açıklama|Azure portal|Kullanıcı akışları|Özel ilke|
|---------|---------|----------|------------|----------|-------------|
|accountEnabled  |Boole|Kullanıcı hesabının etkin mi yoksa devre dışı mı olduğunu: hesap etkinse **doğru** , aksi takdirde **yanlış**olur.|Evet|Hayır|Kalıcı, çıkış|
|ageGroup        |Dize|Kullanıcının yaş grubu. Olası değerler: null, tanımsız, küçük, Yetişkin, NotAdult.|Evet|Hayır|Kalıcı, çıkış|
|Alternativesecurityıd ([kimlikler](manage-user-accounts-graph-api.md#identities-property))|Dize|Dış kimlik sağlayıcısından tek bir kullanıcı kimliği.|Hayır|Hayır|Giriş, kalıcı, çıkış|
|Alternativesecurityıds ([kimlikler](manage-user-accounts-graph-api.md#identities-property))|Alternatif Securityıd koleksiyonu|Dış kimlik sağlayıcılarından Kullanıcı kimlikleri koleksiyonu.|Hayır|Hayır|Kalıcı, çıkış|
|city            |Dize|Kullanıcının bulunduğu şehir. Maksimum uzunluk 128.|Yes|Evet|Kalıcı, çıkış|
|consentProvidedForMinor|Dize|İkincil için onay sağlanmış olup olmadığı. İzin verilen değerler: null, verildi, reddedildi veya notRequired.|Evet|Hayır|Kalıcı, çıkış|
|ülke         |Dize|Kullanıcının bulunduğu ülke/bölge. Örnek: "US" veya "UK". Maksimum uzunluk 128.|Yes|Evet|Kalıcı, çıkış|
|Saati|DateTime|Kullanıcı nesnesinin oluşturulduğu tarih. Salt okunurdur.|Hayır|Hayır|Kalıcı, çıkış|
|creationType    |Dize|Kullanıcı hesabı bir Azure Active Directory B2C kiracısı için yerel hesap olarak oluşturulduysa, değer LocalAccount veya namebir. Salt okunurdur.|Hayır|Hayır|Kalıcı, çıkış|
|Tarih Ofdoğum     |Tarih|Doğum tarihi.|Hayır|Hayır|Kalıcı, çıkış|
|bölüm      |Dize|Kullanıcının çalıştığı departmanın adı. Maksimum uzunluk 64.|Evet|Hayır|Kalıcı, çıkış|
|displayName     |Dize|Kullanıcının görünen adı. Maksimum uzunluk 256.|Yes|Evet|Kalıcı, çıkış|
|facsimileTelephoneNumber<sup>1</sup>|Dize|Kullanıcının iş Faks makinesinin telefon numarası.|Evet|Hayır|Kalıcı, çıkış|
|givenName       |Dize|Kullanıcının verilen adı (ilk adı). Maksimum uzunluk 64.|Yes|Evet|Kalıcı, çıkış|
|jobTitle        |Dize|Kullanıcının iş unvanı. Maksimum uzunluk 128.|Yes|Evet|Kalıcı, çıkış|
|ImmutableID     |Dize|Genellikle şirket içi Active Directory geçirilen kullanıcılar için kullanılan bir tanımlayıcı.|Hayır|Hayır|Kalıcı, çıkış|
|Ligalagegroupclassification|Dize|Yasal yaş grubu sınıflandırması. Bir salt okunurdur ve ageGroup ve consentProvidedForMinor özellikleri temel alınarak hesaplanır. İzin verilen değerler: null, minorWithOutParentalConsent, minorWithParentalConsent, minorNoParentalConsentRequired, notAdult ve yetişkin.|Evet|Hayır|Kalıcı, çıkış|
|legalCountry<sup>1</sup>  |Dize|Yasal amaçlar için ülke/bölge.|Hayır|Hayır|Kalıcı, çıkış|
|posta            |Dize|Kullanıcının SMTP adresi (örneğin, " bob@contoso.com "). Salt okunur.|Hayır|Hayır|Kalıcı, çıkış|
|mailNickName    |Dize|Kullanıcının posta diğer adı. Maksimum uzunluk 64.|Hayır|Hayır|Kalıcı, çıkış|
|Mobil (mobilePhone) |Dize|Kullanıcının birincil cep telefonu numarası. Maksimum uzunluk 64.|Evet|Hayır|Kalıcı, çıkış|
|NetID           |Dize|Ağ KIMLIĞI.|Hayır|Hayır|Kalıcı, çıkış|
|Uzantının        |Dize|Kullanıcının benzersiz tanımlayıcısı olan bir genel benzersiz tanımlayıcı (GUID). Örnek: 12345678-9ABC-def0-1234-56789abcde. Salt okuma, sabit.|Salt okunur|Evet|Giriş, kalıcı, çıkış|
|Diğer postalar      |Dize koleksiyonu|Kullanıcı için ek e-posta adresleri listesi. Örnek: [" bob@contoso.com ", " Robert@fabrikam.com "].|Evet (alternatif e-posta)|Hayır|Kalıcı, çıkış|
|password        |Dize|Kullanıcı oluşturma sırasında yerel hesabın parolası.|Hayır|Hayır|Kalıcı|
|passwordPolicies     |Dize|Parolanın ilkesi. Bu, virgülle ayrılmış farklı ilke adından oluşan bir dizedir. Yani "Disablepasswordexpiasyon, DisableStrongPassword".|Hayır|Hayır|Kalıcı, çıkış|
|physicalDeliveryOfficeName (officeLocation)|Dize|Kullanıcının iş yerinde Office konumu. Maksimum uzunluk 128.|Evet|Hayır|Kalıcı, çıkış|
|postalCode      |Dize|Kullanıcının posta adresi için posta kodu. Posta kodu kullanıcının ülkesine/bölgesine özeldir. America Birleşik Devletler, bu öznitelik ZIP kodunu içerir. Maksimum Uzunluk 40.|Evet|Hayır|Kalıcı, çıkış|
|preferredLanguage    |Dize|Kullanıcı için tercih edilen dil. ISO 639-1 kodunu izlemelidir. Örnek: "en-US".|Hayır|Hayır|Kalıcı, çıkış|
|refreshTokensValidFromDateTime|DateTime|Bu saatten önce verilen yenileme belirteçleri geçersiz ve uygulamalar, yeni bir erişim belirteci almak için geçersiz yenileme belirteci kullanırken bir hata alır. Bu durumda, uygulamanın yetkilendirme uç noktasına istek yaparak yeni bir yenileme belirteci edinmesi gerekir. Salt okunur.|Hayır|Hayır|Çıktı|
|Signınnames ([kimlikler](manage-user-accounts-graph-api.md#identities-property)) |Dize|Dizindeki herhangi bir türdeki yerel hesap kullanıcısının benzersiz oturum açma adı. Yerel hesap türünü belirtmeden oturum açma değeri olan bir kullanıcı almak için bunu kullanın.|Hayır|Hayır|Giriş|
|Signınnames. userName ([kimlikler](manage-user-accounts-graph-api.md#identities-property)) |Dize|Dizindeki yerel hesap kullanıcısının benzersiz Kullanıcı adı. Bunu, belirli bir oturum açma Kullanıcı adı ile Kullanıcı oluşturmak veya almak için kullanın. Bunu Patch işlemi sırasında PersistedClaims içinde belirtmek, diğer oturum adı türlerini kaldırır. Yeni bir Signınnames türü eklemek istiyorsanız, mevcut Signınnames 'leri de kalıcı hale getirmeniz gerekir.|Hayır|Hayır|Giriş, kalıcı, çıkış|
|Signınnames. phoneNumber ([kimlikler](manage-user-accounts-graph-api.md#identities-property)) |Dize|Dizindeki yerel hesap kullanıcısının benzersiz telefon numarası. Belirli bir oturum açma telefon numarası ile Kullanıcı oluşturmak veya almak için bunu kullanın. Bunu Patch işlemi sırasında PersistedClaims içinde belirtmek, diğer oturum adı türlerini kaldırır. Yeni bir Signınnames türü eklemek istiyorsanız, mevcut Signınnames 'leri de kalıcı hale getirmeniz gerekir.|Hayır|Hayır|Giriş, kalıcı, çıkış|
|Signınnames. Emaadresi ([kimlikler](manage-user-accounts-graph-api.md#identities-property))|Dize|Dizindeki yerel hesap kullanıcısının benzersiz e-posta adresi. Belirli bir oturum açma e-posta adresine sahip bir kullanıcı oluşturmak veya almak için bunu kullanın. Bunu Patch işlemi sırasında PersistedClaims içinde belirtmek, diğer oturum adı türlerini kaldırır. Yeni bir Signınnames türü eklemek istiyorsanız, mevcut Signınnames 'leri de kalıcı hale getirmeniz gerekir.|Hayır|Hayır|Giriş, kalıcı, çıkış|
|durum           |Dize|Kullanıcının adresindeki eyalet veya bölge. Maksimum uzunluk 128.|Yes|Evet|Kalıcı, çıkış|
|streetAddress   |Dize|Kullanıcının iş yeri için cadde adresi. Maksimum uzunluk 1024.|Yes|Evet|Kalıcı, çıkış|
|strongAuthentication AlternativePhoneNumber<sup>1</sup>|Dize|Kullanıcının çok faktörlü kimlik doğrulaması için kullandığı ikincil telefon numarası.|Evet|Hayır|Kalıcı, çıkış|
|Strongauthenticationemaadresi<sup>1</sup>|Dize|Kullanıcının SMTP adresi. Örnek: " bob@contoso.com " Bu öznitelik, kullanıcı e-posta adresini depolamak için Kullanıcı adı ilkesi ile oturum açma için kullanılır. Daha sonra bir parola sıfırlama akışında kullanılan e-posta adresi.|Evet|Hayır|Kalıcı, çıkış|
|strongAuthenticationPhoneNumber<sup>1</sup>|Dize|Kullanıcının çok faktörlü kimlik doğrulaması için kullanılan birincil telefon numarası.|Evet|Hayır|Kalıcı, çıkış|
|surname         |Dize|Kullanıcının Soyadı (aile adı veya soyadı). Maksimum uzunluk 64.|Yes|Evet|Kalıcı, çıkış|
|telephoneNumber (ilk departman girişi)|Dize|Kullanıcının iş yeri için birincil telefon numarası.|Evet|Hayır|Kalıcı, çıkış|
|userPrincipalName    |Dize|Kullanıcının kullanıcı asıl adı (UPN). UPN, Internet standart RFC 822 ' i temel alan Kullanıcı için Internet stili bir oturum açma adıdır. Etki alanının, kiracının doğrulanmış etki alanları koleksiyonunda mevcut olması gerekir. Bir hesap oluşturulduğunda bu özellik gereklidir. Değişmez.|Hayır|Hayır|Giriş, kalıcı, çıkış|
|usageLocation   |Dize|Ülkelerde/bölgelerde hizmetlerin kullanılabilirliğini kontrol etmek için yasal gereksinim nedeniyle lisanslar atanacak kullanıcılar için gereklidir. Null yapılabilir değil. İki harfli ülke/bölge kodu (ISO standart 3166). Örnekler: "US", "JP" ve "GB".|Evet|Hayır|Kalıcı, çıkış|
|userType        |Dize|Dizininizdeki Kullanıcı türlerini sınıflandırmak için kullanılabilen bir dize değeri. Değer üye olmalıdır. Salt okunur.|Salt okunur|Hayır|Kalıcı, çıkış|
|userState (externalUserState)<sup>2</sup>|Dize|Yalnızca Azure AD B2B hesabı için, davetin Pendingkabulünü veya kabul edilip edilmediğini belirtir.|Hayır|Hayır|Kalıcı, çıkış|
|userStateChangedOn (externalUserStateChangeDateTime)<sup>2</sup>|DateTime|UserState özelliği için en son değişikliğin zaman damgasını gösterir.|Hayır|Hayır|Kalıcı, çıkış|
|<sup>1</sup> Microsoft Graph tarafından desteklenmiyor<br><sup>2</sup> Azure AD B2C ile kullanılmamalıdır||||||


## <a name="extension-attributes"></a>Uzantı öznitelikleri

Aşağıdaki durumlarda olduğu gibi genellikle kendi öznitelerinizi oluşturmanız gerekir:

- **Loyaltynumber**gibi bir öznitelik için müşteriye yönelik bir uygulamanın kalıcı olması gerekir.
- Bir kimlik sağlayıcısı, kaydedilmesi gereken **Uniqueuserguid** gibi benzersiz bir Kullanıcı tanımlayıcısına sahiptir.
- Özel Kullanıcı yolculuğunun, bir kullanıcının durumu için **Migrationstatus**gibi kalıcı olması gerekir.

Azure AD B2C her kullanıcı hesabında depolanan özniteliklerin kümesini genişletir. Uzantı öznitelikleri, dizindeki Kullanıcı nesnelerinin [şemasını genişletir](https://docs.microsoft.com/graph/extensibility-overview#schema-extensions) . Uzantı öznitelikleri, bir kullanıcı için veri içerse de, yalnızca bir uygulama nesnesine kaydedilebilir. Extension özniteliği B2C-Extensions-App adlı uygulamaya iliştirilir. Bu uygulamayı, Kullanıcı verilerini depolamak için Azure AD B2C tarafından kullanıldığından değiştirmeyin. Bu uygulamayı, Azure Active Directory Uygulama kayıtları altında bulabilirsiniz.

> [!NOTE]
> - En fazla 100 uzantı özniteliği herhangi bir kullanıcı hesabına yazılabilir.
> - B2C-Extensions-App uygulaması silinirse, bu uzantı öznitelikleri içerdikleri tüm verilerle birlikte tüm kullanıcılardan kaldırılır.
> - Bir uzantı özniteliği uygulama tarafından silinirse, tüm kullanıcı hesaplarından kaldırılır ve değerler silinir.
> - Uzantı özniteliğinin temeldeki adı "Extension_" + uygulama KIMLIĞI + "_" + öznitelik adı biçiminde oluşturulur. Örneğin, LoyaltyNumber uzantı özniteliğini ve B2C-Extensions-App uygulama KIMLIĞI, 831374b3-bd50-41bf-aa54-263ec9e050fc ise, temel alınan uzantı öznitelik adı şu şekilde olacaktır: extension_831374b3bd5041bfaa54263ec9e050fc_LoyaltyNumber. Kullanıcı hesapları oluşturmak veya güncelleştirmek için Graph API sorguları çalıştırdığınızda temel alınan adı kullanırsınız.

Aşağıdaki veri türleri bir şema uzantısında Özellik tanımlanırken desteklenir:

|Özellik türü |Açıklamalar  |
|--------------|---------|
|Boole    | Olası değerler: **true** veya **false**. |
|DateTime   | ISO 8601 biçiminde belirtilmelidir. UTC olarak saklanacaktır.   |
|Tamsayı    | 32 bitlik değer.               |
|Dize     | en fazla 256 karakter.     |

## <a name="next-steps"></a>Sonraki adımlar
Uzantı öznitelikleri hakkında daha fazla bilgi edinin:
- [Şema uzantıları](https://docs.microsoft.com/graph/extensibility-overview#schema-extensions)
- [Kullanıcı akışıyla özel öznitelikler tanımlama](user-flow-custom-attributes.md)
- [Özel ilkeyle özel öznitelikler tanımlama](custom-policy-custom-attributes.md)
