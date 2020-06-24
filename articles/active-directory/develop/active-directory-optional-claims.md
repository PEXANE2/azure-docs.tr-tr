---
title: Azure AD uygulamalarına isteğe bağlı talepler sağlama
titleSuffix: Microsoft identity platform
description: SAML 2,0 ve Azure Active Directory tarafından verilen JSON Web belirteçleri (JWT) belirteçlerine özel veya ek talepler ekleme.
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 06/11/2020
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, keyam
ms.custom: aaddev
ms.openlocfilehash: 79f26d56b79a4622ce99b45f153685f9063b79ad
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904856"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app"></a>Nasıl yapılır: Azure AD uygulamanıza isteğe bağlı talepler sağlama

Uygulama geliştiricileri, kendi uygulamalarına hangi talepler gönderilmesini istediğinizi belirlemek için Azure AD uygulamalarında isteğe bağlı talepler kullanabilir.

İsteğe bağlı talepleri şu şekilde kullanabilirsiniz:

- Uygulamanızın belirteçlerine dahil etmek için ek talepler ' i seçin.
- Azure AD 'nin belirteçlerde döndürdüğü belirli taleplerin davranışını değiştirin.
- Uygulamanız için özel talepler ekleyin ve erişin.

Standart talepler listesi için bkz. [erişim belirteci](access-tokens.md) ve [id_token](id-tokens.md) talep belgeleri.

İsteğe bağlı talepler hem v 1.0, v 2.0 biçim belirteçlerinde hem de SAML belirteçlerinde desteklenirken, v 1.0 'dan v 2.0'a geçiş yaparken bunların büyük bir kısmını sağlarlar. [V 2.0 Microsoft Identity platform uç noktasının](active-directory-appmodel-v2-overview.md) amaçlarından biri, istemciler tarafından en iyi performansı elde etmek için daha küçük belirteç boyutlarıdır. Sonuç olarak, daha önce erişim ve KIMLIK belirteçlerine dahil edilen birkaç talep artık v 2.0 belirteçlerinde mevcut değildir ve özellikle de uygulama başına temelinde sorulmalıdır.

**Tablo 1: uygulanabilirlik**

| Hesap türü               | v 1.0 belirteçleri | v 2.0 belirteçleri |
|----------------------------|-------------|-------------|
| Kişisel Microsoft hesabı | Yok         | Destekleniyor   |
| Azure AD hesabı           | Destekleniyor   | Destekleniyor   |

## <a name="v10-and-v20-optional-claims-set"></a>v 1.0 ve v 2.0 isteğe bağlı talepler kümesi

Varsayılan olarak, uygulamaların kullanması için kullanılabilen isteğe bağlı talepler kümesi aşağıda listelenmiştir. Uygulamanıza yönelik özel isteğe bağlı talepler eklemek için aşağıdaki [Dizin uzantıları](#configuring-directory-extension-optional-claims)bölümüne bakın. **Erişim belirtecine**talepler eklenirken, talepler uygulama *tarafından* istenen talepler değil, uygulama (bir Web API) *için* istenen erişim belirteçlerine uygulanır. İstemci, API 'nize nasıl eriştiğine bakılmaksızın, API 'niz üzerinde kimlik doğrulaması yapmak için kullanılan erişim belirtecinde doğru veriler vardır.

> [!NOTE]
> Bu taleplerin çoğu, v 1.0 ve v 2.0 belirteçleri için JWTs 'ye dahil edilebilir, ancak belirteç türü sütununda belirtilenler dışında SAML belirteçleri olamaz. Tüketici hesapları, bu taleplerin bir alt kümesini destekler ve "Kullanıcı türü" sütununda işaretlenir.  Listelenen taleplerin birçoğu tüketici kullanıcılarına uygulanmaz (hiçbir kiracı yoktur, bu nedenle bir `tenant_ctry` değer yoktur).

**Tablo 2: v 1.0 ve v 2.0 isteğe bağlı talep kümesi**

| Name                       |  Description   | Belirteç türü | Kullanıcı türü | Notlar  |
|----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Kullanıcının en son kimlik doğrulamasının süresi. Bkz. OpenID Connect spec.| JWT        |           |  |
| `tenant_region_scope`      | Kaynak kiracının bölgesi | JWT        |           | |
| `home_oid`                 | Konuk kullanıcılar için kullanıcının ana kiracısındaki kullanıcının nesne KIMLIĞI.| JWT        |           | |
| `sid`                      | Oturum başına Kullanıcı oturumu kapatma için kullanılan oturum KIMLIĞI. | JWT        |  Kişisel ve Azure AD hesapları.   |         |
| `platf`                    | Cihaz platformu    | JWT        |           | Cihaz türünü doğrulayabileceği yönetilen cihazlarla kısıtlıdır.|
| `verified_primary_email`   | Kullanıcının açık olan Yauthcumtativee-postalarından kaynaklandırılmış      | JWT        |           |         |
| `verified_secondary_email` | Kullanıcının SecondaryAuthoritativeEmail kaynağı   | JWT        |           |        |
| `enfpolids`                | Zorunlu kılınan ilke kimlikleri. Geçerli Kullanıcı için değerlendirilen ilke kimliklerinin bir listesi. | JWT |  |  |
| `vnet`                     | VNET tanımlayıcı bilgileri. | JWT        |           |      |
| `fwd`                      | IP adresi.| JWT    |   | İstek sunan istemcinin özgün IPv4 adresini ekler (sanal ağ içinde) |
| `ctry`                     | Kullanıcının ülkesi/bölgesi | JWT |  | Azure AD, varsa `ctry` isteğe bağlı talebi döndürür ve talebin DEĞERI fr, JP, SZ vb. gibi standart iki harfli bir ülke/bölge kodudur. |
| `tenant_ctry`              | Kaynak kiracının ülkesi/bölgesi | JWT | | |
| `xms_pdl`             | Tercih edilen veri konumu   | JWT | | Çoklu coğrafi kiracılar için, tercih edilen veri konumu, kullanıcının bulunduğu coğrafi bölgeyi gösteren üç harfli koddur. Daha fazla bilgi için, [tercih edilen veri konumu hakkında Azure AD Connect belgelerine](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation)bakın.<br/>Örneğin: `APC` Asya Pasifik için. |
| `xms_pl`                   | Kullanıcı tarafından tercih edilen dil  | JWT ||Ayarlanmışsa kullanıcının tercih ettiği dil. Konuk erişim senaryolarında, ana kiracılarından kaynaklıdır. Biçimlendirilen LL-CC ("en-US"). |
| `xms_tpl`                  | Kiracının tercih ettiği dil| JWT | | Ayarlanırsa, kaynak kiracının tercih edilen dili. Biçimlendirildim ("en"). |
| `ztdid`                    | Sıfır dokunma dağıtım KIMLIĞI | JWT | | [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) için kullanılan cihaz kimliği |
| `email`                    | Kullanıcının bir tane varsa, bu kullanıcı için adreslenebilir e-posta.  | JWT, SAML | MSA, Azure AD | Bu değer, Kullanıcı Kiracıdaki bir konuk ise varsayılan olarak dahil edilir.  Yönetilen kullanıcılar (kiracının içindeki kullanıcılar) için, bu isteğe bağlı talep veya yalnızca v 2.0 üzerinde, OpenID kapsamı ile istenmesi gerekir.  Yönetilen kullanıcılar için, e-posta adresinin [Office Yönetim Portalı](https://portal.office.com/adminportal/home#/users)'nda ayarlanması gerekir.|
| `groups`| Grup talepleri için isteğe bağlı biçimlendirme |JWT, SAML| |[Uygulama bildiriminde](reference-app-manifest.md), aynı zamanda ayarlanması gereken Groupmembershipclaim ayarıyla birlikte kullanılır. Ayrıntılar için aşağıdaki [Grup taleplerini](#configuring-groups-optional-claims) inceleyin. Grup talepleri hakkında daha fazla bilgi için bkz. [Grup taleplerini yapılandırma](../hybrid/how-to-connect-fed-group-claims.md)
| `acct`                | Kiracıdaki Kullanıcı hesabı durumu | JWT, SAML | | Kullanıcı kiracının üyesiyse, değeri olur `0` . Bunlar bir konuğlarsa değer olur `1` . |
| `upn`                      | UserPrincipalName | JWT, SAML  |           | Bu talep otomatik olarak dahil edilse de, Konuk Kullanıcı kasasında davranışını değiştirmek üzere ek özellikler eklemek için isteğe bağlı bir talep olarak belirtebilirsiniz.  |
| `idtyp`                    | Belirteç türü   | JWT erişim belirteçleri | Özel: yalnızca yalnızca uygulama erişim belirteçlerinde |  Değer, `app` belirtecin yalnızca uygulama belirteci olduğu değerdir. Bu, bir API 'nin bir uygulama belirteci veya bir uygulama + kullanıcı belirteci olup olmadığını belirlemesi için en doğru yoldur.|

## <a name="v20-specific-optional-claims-set"></a>v 2.0'a özgü isteğe bağlı talepler kümesi

Bu talepler her zaman v 1.0 Azure AD belirteçlerine dahil edilmiştir, ancak istenmediği takdirde v 2.0 belirteçlerine dahil edilmez. Bu talepler yalnızca JWTs (KIMLIK belirteçleri ve erişim belirteçleri) için geçerlidir.

**Tablo 3: v 2.0-yalnızca isteğe bağlı talepler**

| JWT talebi     | Name                            | Description                                | Notlar |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | IP Adresi                      | İstemcinin oturum açtığı IP adresi.   |       |
| `onprem_sid`  | Şirket Içi güvenlik tanımlayıcısı |                                             |       |
| `pwd_exp`     | Parola Son Kullanma Tarihi        | Parolanın süresi dolan tarih/saat. |       |
| `pwd_url`     | Parola URL 'sini Değiştir             | Kullanıcının parolalarını değiştirmek için ziyaret edebildikleri bir URL.   |   |
| `in_corp`     | İç Şirket Ağı        | İstemci şirket ağından oturum açıyorsanız bildirir. Aksi takdirde talep dahil edilmez.   |  MFA 'daki [Güvenilen IP 'lerin](../authentication/howto-mfa-mfasettings.md#trusted-ips) ayarlarını temel alarak.    |
| `family_name` | Soyadı                       | Kullanıcı nesnesinde tanımlandığı şekilde kullanıcının soyadı, soyadı veya aile adını sağlar. <br>"family_name": "Miller" | MSA ve Azure AD 'de desteklenir. Kapsam gerektirir `profile` .   |
| `given_name`  | Ad                      | Kullanıcı nesnesinde ayarlandığı gibi, kullanıcının ilk veya "verilen" adını sağlar.<br>"given_name": "filiz"                   | MSA ve Azure AD 'de desteklenir.  Kapsam gerektirir `profile` . |
| `upn`         | Kullanıcı Asıl Adı | Kullanıcı için username_hint parametresiyle kullanılabilecek bir tanımlayıcı.  Kullanıcı için dayanıklı bir tanımlayıcı değildir ve anahtar verileri için kullanılmamalıdır. | Talebin yapılandırması için aşağıdaki [ek özelliklere](#additional-properties-of-optional-claims) bakın. Kapsam gerektirir `profile` .|

### <a name="additional-properties-of-optional-claims"></a>İsteğe bağlı taleplerin ek özellikleri

İsteğe bağlı talepler, talebin döndürdüğü yöntemi değiştirecek şekilde yapılandırılabilir. Bu ek özellikler çoğunlukla, farklı veri beklentilerine sahip şirket içi uygulamaların geçişine yardımcı olmak için kullanılır (örneğin, `include_externally_authenticated_upn_without_hash` UPN 'de karma işaretleri () işleyemeyen istemcilerde yardımcı olur `#` )

**Tablo 4: isteğe bağlı talepler yapılandırma değerleri**

| Özellik adı  | Ek özellik adı | Description |
|----------------|--------------------------|-------------|
| `upn`          |                          | Hem SAML hem de JWT yanıtları için ve v 1.0 ve v 2.0 belirteçleri için kullanılabilir. |
|                | `include_externally_authenticated_upn`  | , Kaynak kiracısında depolanan Konuk UPN 'sini içerir. Örneğin, `foo_hometenant.com#EXT#@resourcetenant.com` |
|                | `include_externally_authenticated_upn_without_hash` | Yukarıdaki gibi, karma işaretlerinin ( `#` ) alt çizgi () ile değiştirilmeleri dışında, `_` Örneğin`foo_hometenant.com_EXT_@resourcetenant.com` |

#### <a name="additional-properties-example"></a>Ek özellikler örneği

```json
"optionalClaims": {
    "idToken": [
        {
            "name": "upn",
            "essential": false,
            "additionalProperties": [
                "include_externally_authenticated_upn"
            ]
        }
    ]
}
```

Bu Optionalclaim nesnesi, istemciye döndürülen KIMLIK belirtecinin ek ana kiracı ve kaynak kiracı bilgilerine sahip bir UPN talebi içermesini sağlar. `upn`Talep yalnızca, Kullanıcı Kiracıdaki bir konuk ise (kimlik doğrulaması için farklı bır ıDP kullanır) belirteçte değiştirilir.

## <a name="configuring-optional-claims"></a>İsteğe bağlı talepler yapılandırılıyor

> [!IMPORTANT]
> Erişim belirteçleri, **her zaman** istemcinin değil kaynağın bildirimi kullanılarak oluşturulur.  Bu nedenle, `...scope=https://graph.microsoft.com/user.read...` kaynak MICROSOFT Graph API 'sidir.  Bu nedenle, erişim belirteci, istemcinin bildirimi değil Microsoft Graph API bildirimi kullanılarak oluşturulur.  Uygulamanızın bildirimini değiştirmek, Microsoft Graph API 'sinin farklı görünmesini hiçbir şekilde neden olmaz.  Değişikliklerinizin geçerli olduğunu doğrulamak için `accessToken` , başka bir uygulama değil, uygulamanız için bir belirteç isteyin.

Kullanıcı arabirimi veya uygulama bildirimi aracılığıyla uygulamanız için isteğe bağlı talepler yapılandırabilirsiniz.

1. [Azure Portal](https://portal.azure.com)gidin. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet** bölümünden **uygulama kayıtları**' yi seçin.
1. Listede için isteğe bağlı talepler yapılandırmak istediğiniz uygulamayı seçin.

**Kullanıcı arabirimi aracılığıyla isteğe bağlı talepler yapılandırma:**

[![Kullanıcı arabirimini kullanarak isteğe bağlı talepler yapılandırmayı gösterir](./media/active-directory-optional-claims/token-configuration.png)](./media/active-directory-optional-claims/token-configuration.png)

1. **Yönet** bölümünde **belirteç yapılandırması**' nı seçin.
1. **İsteğe bağlı talep Ekle**' yi seçin.
1. Yapılandırmak istediğiniz belirteç türünü seçin.
1. Eklenecek isteğe bağlı talepler ' i seçin.
1. **Ekle**'yi seçin.

**Uygulama bildirimi aracılığıyla isteğe bağlı talepler Yapılandırılıyor:**

[![Uygulama bildirimini kullanarak isteğe bağlı taleplerin nasıl yapılandırılacağını gösterir](./media/active-directory-optional-claims/app-manifest.png)](./media/active-directory-optional-claims/app-manifest.png)

1. **Yönet** bölümünde, **bildirim**' ı seçin. Web tabanlı bir bildirim Düzenleyicisi açılarak, bildirimi düzenlemenize olanak tanır. İsteğe bağlı olarak **İndir** seçeneğini belirleyip bildirimi yerel ortamda düzenledikten sonra **Yükle** seçeneğiyle uygulamanıza yeniden uygulayabilirsiniz. Uygulama bildirimi hakkında daha fazla bilgi için bkz. [Azure AD uygulama bildirimini anlama makalesi](reference-app-manifest.md).

    Aşağıdaki uygulama bildirimi girişi, KIMLIK, erişim ve SAML belirteçlerine auth_time, IPADDR ve UPN isteğe bağlı taleplerini ekler.

    ```json
    "optionalClaims": {
        "idToken": [
            {
                "name": "auth_time",
                "essential": false
            }
        ],
        "accessToken": [
            {
                "name": "ipaddr",
                "essential": false
            }
        ],
        "saml2Token": [
            {
                "name": "upn",
                "essential": false
            },
            {
                "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                "source": "user",
                "essential": false
            }
        ]
    }
    ```

2. İşiniz bittiğinde **Kaydet**' i seçin. Artık belirtilen isteğe bağlı talepler, uygulamanızın belirteçlerine dahil edilecek.

### <a name="optionalclaims-type"></a>Optionalclaim türü

Bir uygulama tarafından istenen isteğe bağlı talepleri bildirir. Bir uygulama, güvenlik belirteci hizmetinden alabileceği üç tür belirteç türünden (KIMLIK belirteci, erişim belirteci, SAML 2 belirteci) döndürülecek isteğe bağlı talepler yapılandırabilir. Uygulama, her bir belirteç türünde döndürülecek farklı bir isteğe bağlı talepler kümesi yapılandırabilir. Uygulama varlığının Optionalclaim özelliği bir Optionalclaim nesnesidir.

**Tablo 5: Optionalclaim türü özellikleri**

| Name          | Tür                       | Description                                           |
|---------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Koleksiyon (OptionalClaim) | JWT KIMLIK belirtecinde döndürülen isteğe bağlı talepler.     |
| `accessToken` | Koleksiyon (OptionalClaim) | JWT erişim belirtecinde döndürülen isteğe bağlı talepler. |
| `saml2Token`  | Koleksiyon (OptionalClaim) | SAML belirtecinde döndürülen isteğe bağlı talepler.       |

### <a name="optionalclaim-type"></a>OptionalClaim türü

Bir uygulama veya hizmet sorumlusu ile ilişkili isteğe bağlı bir talep içerir. [Optionalclaim](https://docs.microsoft.com/graph/api/resources/optionalclaims?view=graph-rest-1.0) türünün ıdtoken, accesstoken ve saml2Token özellikleri bir optionalclaim koleksiyonudur.
Belirli bir talep tarafından destekleniyorsa, Additionalclaim 'nin davranışını AdditionalProperties alanını kullanarak da değiştirebilirsiniz.

**Tablo 6: OptionalClaim türü özellikleri**

| Name                   | Tür                    | Description                                                                                                                                                                                                                                                                                                   |
|------------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | İsteğe bağlı talebin adı.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | Talebin kaynağı (Dizin nesnesi). Uzantı özelliklerinden önceden tanımlı talepler ve Kullanıcı tanımlı talepler vardır. Kaynak değeri null ise, talep önceden tanımlanmış isteğe bağlı bir talep olur. Kaynak değeri kullanıcı ise, ad özelliğindeki değer kullanıcı nesnesinden uzantı özelliğidir. |
| `essential`            | Edm.Boolean             | Değer true ise, istemci tarafından belirtilen talep, son kullanıcı tarafından istenen belirli bir görev için sorunsuz bir yetkilendirme deneyimi sağlamak için gereklidir. Varsayılan değer false'tur.                                                                                                                 |
| `additionalProperties` | Koleksiyon (EDM. String) | Talebin ek özellikleri. Bu koleksiyonda bir özellik varsa, ad özelliğinde belirtilen isteğe bağlı talebin davranışını değiştirir.                                                                                                                                                   |

## <a name="configuring-directory-extension-optional-claims"></a>Dizin uzantısı isteğe bağlı taleplerini yapılandırma

Standart isteğe bağlı talepler kümesine ek olarak, belirteçleri uzantıları içerecek şekilde de yapılandırabilirsiniz. Daha fazla bilgi için [Microsoft Graph extensionProperty belgelerine](https://docs.microsoft.com/graph/api/resources/extensionproperty?view=graph-rest-1.0)bakın.

Şema ve açık uzantılar isteğe bağlı talepler, yalnızca AAD grafik stili Dizin uzantıları tarafından desteklenmez. Bu özellik, uygulamanızın kullanabileceği ek kullanıcı bilgilerini eklemek için yararlıdır; Örneğin, kullanıcının ayarlamış olduğu ek bir tanımlayıcı veya önemli bir yapılandırma seçeneği. Bir örnek için bu sayfanın en altına bakın.

> [!NOTE]
> Dizin şeması uzantıları yalnızca Azure AD özellikli bir özelliktir. Uygulama bildiriminiz bir özel uzantı isterse ve bir MSA kullanıcısı uygulamanızda oturum açarsa, bu uzantılar döndürülmez.

### <a name="directory-extension-formatting"></a>Dizin uzantısı biçimlendirmesi

Uygulama bildirimini kullanarak dizin uzantısı isteğe bağlı taleplerini yapılandırırken, uzantının tam adını kullanın (biçiminde: `extension_<appid>_<attributename>` ). `<appid>`Talep isteyen UYGULAMANıN kimliğiyle eşleşmesi gerekir.

JWT içinde bu talepler şu ad biçimiyle yayınlanacaktır: `extn.<attributename>` .

SAML belirteçlerinde Bu talepler aşağıdaki URI biçimiyle alınacaktır:`http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="configuring-groups-optional-claims"></a>Grupları yapılandırma isteğe bağlı talepler

   > [!NOTE]
   > Şirket içinden eşitlenen kullanıcılar ve gruplar için Grup adlarını yayma özelliği genel önizlemeye sunuldu.

Bu bölüm, Grup taleplerinde kullanılan grup özniteliklerinin, şirket içi Windows Active Directory eşitlenen özniteliklere varsayılan gruptan değiştirilmesini sağlamak için isteğe bağlı talepler altındaki yapılandırma seçeneklerini anlatmaktadır. Kullanıcı arabirimi veya uygulama bildirimi aracılığıyla uygulamanız için isteğe bağlı talepler yapılandırabilirsiniz.

> [!IMPORTANT]
> Şirket içi özniteliklerdeki grup taleplerinin genel önizlemesine ilişkin önemli uyarılar dahil olmak üzere daha fazla ayrıntı için bkz. [Azure AD ile uygulamalar için grup taleplerini yapılandırma](../hybrid/how-to-connect-fed-group-claims.md).

**Grupları kullanıcı arabirimi aracılığıyla isteğe bağlı talepler yapılandırma:**

1. [Azure Portal](https://portal.azure.com) oturum açın
1. Kimlik doğrulamasından geçtikten sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin
1. Sol taraftaki menüden **Azure Active Directory** seçin
1. **Yönet** bölümünün altında **uygulama kayıtları** ' yi seçin.
1. Listede için isteğe bağlı talepler yapılandırmak istediğiniz uygulamayı seçin
1. **Yönet** bölümünde, **belirteç yapılandırması** ' nı seçin.
1. **Grup Ekle talebi** seçin
1. Döndürülecek Grup türlerini seçin (**tüm gruplar**, **SecurityGroup**veya **directoryrole**). **Tüm gruplar** seçeneği **SecurityGroup**, **directoryrole**ve **distributionlist** içerir
1. İsteğe bağlı: gruplar talep değerini, şirket içi grup özniteliklerini içerecek şekilde değiştirmek veya talep türünü bir rol olarak değiştirmek için belirli belirteç türü özelliklerini seçin
1. **Kaydet**’i seçin

**Grupları uygulama bildirimi aracılığıyla isteğe bağlı talepler yapılandırma:**

1. [Azure Portal](https://portal.azure.com) oturum açın
1. Kimlik doğrulamasından geçtikten sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin
1. Sol taraftaki menüden **Azure Active Directory** seçin
1. Listede için isteğe bağlı talepler yapılandırmak istediğiniz uygulamayı seçin
1. **Yönet** bölümünün altında **bildirim** ' ı seçin.
1. Bildirim düzenleyicisini kullanarak aşağıdaki girişi ekleyin:

   Geçerli değerler şunlardır:

   - "All" (Bu seçenek SecurityGroup, DirectoryRole ve DistributionList içerir)
   - "SecurityGroup"
   - "DirectoryRole"

   Örneğin:

    ```json
    "groupMembershipClaims": "SecurityGroup"
    ```

   Varsayılan olarak grup ObjectID 'Ler, Grup talep değeri ' ne yayılır.  Talep değerini şirket içi grup özniteliklerini içerecek şekilde değiştirmek veya talep türünü rol olarak değiştirmek için, Optionalclaim yapılandırması ' nı aşağıdaki gibi kullanın:

1. Grup adı yapılandırması isteğe bağlı taleplerini ayarlayın.

   Belirteçte isteğe bağlı talepler bölümünde şirket içi AD grubu özniteliklerini içeren gruplar istiyorsanız, isteğe bağlı talebin adı, istenen isteğe bağlı talep ve istediğiniz ek özellikleri belirtin.  Birden çok belirteç türü listelenebilir:

   - OıDC KIMLIK belirtecinin ıdtoken 'ı
   - OAuth erişim belirteci için accessToken
   - SAML belirteçleri için Saml2Token.

   > [!NOTE]
   > Saml2Token türü SAML 1.1 ve SAML 2.0 biçim belirteçleri için geçerlidir

   Her ilgili belirteç türü için, gruplar talebini, bildirimdeki Optionalclaim bölümünü kullanacak şekilde değiştirin. Optionalclaim şeması aşağıdaki gibidir:

    ```json
    {
        "name": "groups",
        "source": null,
        "essential": false,
        "additionalProperties": []
    }
    ```

   | İsteğe bağlı talepler şeması | Değer |
   |----------|-------------|
   | **ada** | "Gruplar" olmalıdır |
   | **kaynaktaki** | Kullanılmadı. Null değerini atla veya belirt |
   | **dir** | Kullanılmadı. Yoksay veya false belirt |
   | **AdditionalProperties** | Ek özelliklerin listesi.  Geçerli seçenekler şunlardır "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles" |

   AdditionalProperties içinde "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" yalnızca biri gereklidir.  Birden fazla varsa, ilki kullanılır ve diğerleri yok sayılır.

   Bazı uygulamalar, rol talebinde Kullanıcı hakkında grup bilgileri gerektirir.  Talep türünü bir grup talebine bir rol talebine değiştirmek için, ek özelliklere "emit_as_roles" ekleyin.  Grup değerleri rol talebinde yayınlanacaktır.

   > [!NOTE]
   > "Emit_as_roles" kullanılırsa, Kullanıcı atandığı yapılandırılmış herhangi bir uygulama rolü rol talebinde görünmez

**Örnekler**

1) Grupları DNSEtkiAlanıAdı sAMAccountName biçimindeki OAuth erişim belirteçlerinde grup adları olarak yay

    **Kullanıcı arabirimi yapılandırması:**

    [![Kullanıcı arabirimini kullanarak isteğe bağlı talepler yapılandırmayı gösterir](./media/active-directory-optional-claims/groups-example-1.png)](./media/active-directory-optional-claims/groups-example-1.png)

    **Uygulama bildirim girdisi:**

    ```json
    "optionalClaims": {
        "accessToken": [
            {
                "name": "groups",
                "additionalProperties": [
                    "dns_domain_and_sam_account_name"
                ]
            }
        ]
    }
    ```

2) SAML ve OıDC KIMLIK belirteçlerinde rol talebi olarak, netbiosDomain\sAMAccountName biçiminde döndürülecek grup adlarını yay

    **Kullanıcı arabirimi yapılandırması:**

    [![Kullanıcı arabirimini kullanarak isteğe bağlı talepler yapılandırmayı gösterir](./media/active-directory-optional-claims/groups-example-2.png)](./media/active-directory-optional-claims/groups-example-2.png)

    **Uygulama bildirim girdisi:**

    ```json
    "optionalClaims": {
        "saml2Token": [
            {
                "name": "groups",
                "additionalProperties": [
                    "netbios_name_and_sam_account_name",
                    "emit_as_roles"
                ]
            }
        ],
        "idToken": [
            {
                "name": "groups",
                "additionalProperties": [
                    "netbios_name_and_sam_account_name",
                    "emit_as_roles"
                ]
            }
        ]
    }
    ```

## <a name="optional-claims-example"></a>İsteğe bağlı talepler örneği

Bu bölümde, uygulamanız için isteğe bağlı talepler özelliğini nasıl kullanabileceğinizi görmek için bir senaryoya yol açabilir.
İsteğe bağlı talepler etkinleştirmek ve yapılandırmak için bir uygulamanın kimlik yapılandırmasındaki özellikleri güncelleştirmek üzere kullanılabilecek birden fazla seçenek vardır:

- **Belirteç yapılandırması** Kullanıcı arabirimini kullanabilirsiniz (aşağıdaki örneğe bakın)
- **Bildirimi** kullanabilirsiniz (aşağıdaki örneğe bakın). Bildirime giriş için önce [Azure AD uygulama bildirimi belgesini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) makalesini okuyun.
- Uygulamanızı güncelleştirmek için [MICROSOFT Graph API](https://docs.microsoft.com/graph/use-the-api?context=graph%2Fapi%2F1.0&view=graph-rest-1.0) kullanan bir uygulama yazmak da mümkündür. Microsoft Graph API başvuru kılavuzundaki [optionalclaim](https://docs.microsoft.com/graph/api/resources/optionalclaims?view=graph-rest-1.0) türü, isteğe bağlı talepleri yapılandırmanıza yardımcı olabilir.

**Örneğinde**

Aşağıdaki örnekte, uygulamanız için tasarlanan erişim, KIMLIK ve SAML belirteçlerine isteğe bağlı talepler eklemek için **belirteç yapılandırması** Kullanıcı arabirimini ve **bildirimini** kullanacaksınız. Uygulamanın alabileceği her bir belirteç türüne farklı isteğe bağlı talepler eklenecektir:

- KIMLIK belirteçleri artık, Federasyon kullanıcıları için UPN 'yi tam biçimde ( `<upn>_<homedomain>#EXT#@<resourcedomain>` ) içerir.
- Bu uygulama için diğer istemcilerin talep aldığı erişim belirteçleri artık auth_time talebi içerecektir
- SAML belirteçleri artık Sktypeınfo dizin şema uzantısını içerecektir (Bu örnekte, bu uygulamanın uygulama KIMLIĞI ab603c56068041afb2f6832e2a17e237 ' dir). SAML belirteçleri, Skype KIMLIĞINI olarak kullanıma sunacaktır `extension_skypeId` .

**Kullanıcı arabirimi yapılandırması:**

1. [Azure Portal](https://portal.azure.com) oturum açın

1. Kimlik doğrulamasından geçtikten sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin.

1. Sol taraftaki menüden **Azure Active Directory** ' yi seçin.

1. **Yönet** bölümünün altında **uygulama kayıtları**' yi seçin.

1. Listede için isteğe bağlı talepler yapılandırmak istediğiniz uygulamayı bulun ve seçin.

1. **Yönet** bölümünde, **belirteç yapılandırması**' nı seçin.

1. **İsteğe bağlı talep Ekle**' yi seçin **, kimlik** belirteci türünü seçin, talepler listesinden **UPN** ' yi seçin ve ardından **Ekle**' yi seçin.

1. **İsteğe bağlı talep Ekle**' yi seçin **, erişim** belirteci türünü seçin, talepler listesinden **auth_time** ' ı seçin ve ardından **Ekle**' yi seçin.

1. Belirteç yapılandırması genel bakış ekranından **UPN**' nin yanındaki kurşun kalem simgesini seçin, **dışarıdan kimliği doğrulanmış** geçiş ' i seçin ve ardından **Kaydet**' i seçin.

1. **İsteğe bağlı talep Ekle**' yi seçin, **SAML** belirteci türünü seçin, talepler listesinden **Extn. sktypeınfo kimliğini** seçin (yalnızca sktypeınfo adlı bir Azure AD Kullanıcı nesnesi oluşturduysanız geçerlidir) ve ardından **Ekle**' yi seçin.

    [![Kullanıcı arabirimini kullanarak isteğe bağlı talepler yapılandırmayı gösterir](./media/active-directory-optional-claims/token-config-example.png)](./media/active-directory-optional-claims/token-config-example.png)

**Bildirim yapılandırması:**

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kimlik doğrulamasından geçtikten sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin.
1. Sol taraftaki menüden **Azure Active Directory** ' yi seçin.
1. Listede için isteğe bağlı talepler yapılandırmak istediğiniz uygulamayı bulun ve seçin.
1. **Yönet** bölümünde, satır içi bildirim düzenleyicisini açmak için **bildirim** ' ı seçin.
1. Bu düzenleyiciyi kullanarak bildirimi doğrudan düzenleyebilirsiniz. Bildirim, [uygulama varlığının](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)şemasını izler ve bir kez kaydedildikten sonra bildirimi otomatik biçimlendirir. Özelliğe yeni öğeler eklenecektir `OptionalClaims` .

    ```json
    "optionalClaims": {
        "idToken": [
            {
                "name": "upn",
                "essential": false,
                "additionalProperties": [
                    "include_externally_authenticated_upn"
                ]
            }
        ],
        "accessToken": [
            {
                "name": "auth_time",
                "essential": false
            }
        ],
        "saml2Token": [
            {
                "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                "source": "user",
                "essential": true
            }
        ]
    }
    ```

1. Bildirimi güncelleştirmeyi tamamladığınızda, bildirimi kaydetmek için **Kaydet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD tarafından sunulan standart talepler hakkında daha fazla bilgi edinin.

- [Kimlik belirteçleri](id-tokens.md)
- [Erişim belirteçleri](access-tokens.md)
