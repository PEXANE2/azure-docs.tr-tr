---
title: Azure AD uygulamalarına isteğe bağlı talepler sağlama | Azure
titleSuffix: Microsoft identity platform
description: Azure Active Directory tarafından verilen SAML 2.0 ve JSON Web Belirteçleri (JWT) belirteçlerine özel veya ek talepler ekleme.
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 3/11/2020
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, keyam
ms.custom: aaddev
ms.openlocfilehash: 23d83b59c510f2565b2f66f78dad56c9c9592dd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136526"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app"></a>Nasıl yapilir: Azure AD uygulamanızda isteğe bağlı talepler sağlayın

Uygulama geliştiricileri, uygulamalarına gönderilen jetonlarda hangi talepleri istediklerini belirtmek için Azure AD uygulamalarında isteğe bağlı talepleri kullanabilir. 

İsteğe bağlı talepleri şu şekilde kullanabilirsiniz:

- Uygulamanızın belirteçlerine eklemek için ek talepler seçin.
- Azure AD'nin belirteçlerle döndürdettiği bazı iddiaların davranışını değiştirin.
- Uygulamanız için özel talepler ekleyin ve erişin.

Standart talep listeleri için [erişim belirteci](access-tokens.md) ve [id_token](id-tokens.md) talep belgelerine bakın. 

İsteğe bağlı talepler hem v1.0 hem de v2.0 biçim belirteçlerinde ve SAML belirteçlerinde desteklenirken, v1.0'dan v2.0'a taşınırken değerinin çoğunu sağlarlar. [v2.0 Microsoft kimlik platformu bitiş noktasının](active-directory-appmodel-v2-overview.md) hedeflerinden biri, istemciler tarafından en iyi performansı sağlamak için daha küçük belirteç boyutlarıdır. Sonuç olarak, daha önce erişim ve kimlik belirteçleri dahil çeşitli iddialar artık v2.0 belirteçleri mevcut değildir ve özellikle uygulama başına bazında istenmelidir.

**Tablo 1: Uygulanabilirlik**

| Hesap Türü | v1.0 belirteçleri | v2.0 belirteçleri  |
|--------------|---------------|----------------|
| Kişisel Microsoft hesabı  | Yok  | Destekleniyor |
| Azure AD hesabı      | Destekleniyor | Destekleniyor |

## <a name="v10-and-v20-optional-claims-set"></a>v1.0 ve v2.0 isteğe bağlı talepler kümesi

Kullanılacak uygulamalar için varsayılan olarak kullanılabilen isteğe bağlı talep kümesi aşağıda listelenmiştir. Uygulamanız için özel isteğe bağlı talepler eklemek için aşağıdaki [Dizin Uzantıları'na](#configuring-directory-extension-optional-claims)bakın. **Erişim jetonuna**talep eklerken, talepler uygulama için istenen belirteçlere (web API) erişmek *için* geçerlidir, uygulama *tarafından* talep edilen talepler için geçerli değildir. İstemci API'nize nasıl erişirse erişsin, API'nize karşı kimlik doğrulaması yapmak için kullanılan erişim belirtecinde doğru veriler bulunur.

> [!NOTE]
> Bu taleplerin çoğu, Token Type sütununda belirtilen durumlar dışında, v1.0 ve v2.0 belirteçleri için JWT'lere dahil edilebilir, ancak SAML belirteçleri dahil edilemez. Tüketici hesapları, "Kullanıcı Türü" sütununda işaretlenen bu taleplerin bir alt kümesini destekler.  Listelenen taleplerin çoğu tüketici kullanıcıları için geçerli değildir (kiracıları yoktur, bu yüzden `tenant_ctry` hiçbir değeri yoktur).  

**Tablo 2: v1.0 ve v2.0 isteğe bağlı talep seti**

| Adı                       |  Açıklama   | Belirteç Türü | Kullanıcı Türü | Notlar  |
|----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Kullanıcının en son kimlik doğrulaması olduğu saat. Bkz. OpenID Connect spec.| Jwt        |           |  |
| `tenant_region_scope`      | Kaynak kiracının bölgesi | Jwt        |           | |
| `home_oid`                 | Konuk kullanıcılar için, kullanıcının ev kiracısında kullanıcının nesne kimliği.| Jwt        |           | |
| `sid`                      | Oturum başına kullanıcı oturumu için kullanılan Oturum Kimliği. | Jwt        |  Kişisel ve Azure REKLAM hesapları.   |         |
| `platf`                    | Cihaz platformu    | Jwt        |           | Aygıt türünü doğrulayabilen yönetilen aygıtlar ile sınırlıdır.|
| `verified_primary_email`   | Kullanıcının PrimaryAuthoritativeEmail kaynaklı      | Jwt        |           |         |
| `verified_secondary_email` | Kullanıcının SecondaryAuthoritativeEmail kaynaklarından   | Jwt        |           |        |
| `enfpolids`                | Zorunlu ilke işlemi Tütünleri. Geçerli kullanıcı için değerlendirilen ilke işleminin listesi. | Jwt |  |  |
| `vnet`                     | VNET belirtici bilgi. | Jwt        |           |      |
| `fwd`                      | IP adresi.| Jwt    |   | İstenen istemcinin orijinal IPv4 adresini ekler (Bir VNET'in içindeyken) |
| `ctry`                     | Kullanıcının ülkesi | Jwt |  | Azure AD, `ctry` varsa isteğe bağlı talebi döndürür ve talebin değeri FR, JP, SZ gibi standart iki harfli bir ülke kodudur. |
| `tenant_ctry`              | Kaynak kiracının ülkesi | Jwt | | |
| `xms_pdl`          | Tercih edilen veri konumu   | Jwt | | Multi-Geo kiracılar için tercih edilen veri konumu, kullanıcının bulunduğu coğrafi bölgeyi gösteren üç harfli koddur. Daha fazla bilgi için, [tercih edilen veri konumu yla ilgili Azure AD Connect belgelerine](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation)bakın.<br/>Örneğin: `APC` Asya Pasifik için. |
| `xms_pl`                   | Kullanıcı tercih edilen dil  | Jwt ||Kullanıcının tercih ettiği dil, eğer ayarlanırsa. Konuk erişim senaryolarında ev kiracısından temin ediliyor. Biçimlendirilmiş LL-CC ("en-us"). |
| `xms_tpl`                  | Kiracı tercih edilen dil| Jwt | | Kaynak kiracının tercih ettiği dil, eğer ayarlanırsa. Biçimlendirilmiş LL ("en"). |
| `ztdid`                    | Sıfır dokunuşlu Dağıtım Kimliği | Jwt | | [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) için kullanılan aygıt kimliği |
| `email`                    | Kullanıcıda varsa, bu kullanıcı için adreslenebilir e-posta.  | JWT, SAML | MSA, Azure AD | Kullanıcı kiracıda konuksa, bu değer varsayılan olarak dahil edilir.  Yönetilen kullanıcılar (kiracının içindeki kullanıcılar) için, bu isteğe bağlı talep yoluyla veya yalnızca V2.0'da OpenID kapsamıyla talep edilmelidir.  Yönetilen kullanıcılar için e-posta adresi [Office yönetici portalında](https://portal.office.com/adminportal/home#/users)ayarlanmalıdır.| 
| `groups`| Grup talepleri için isteğe bağlı biçimlendirme |JWT, SAML| |[Uygulama bildiriminde](reference-app-manifest.md)GroupMembershipClaims ayarı ile birlikte kullanılır , hangi de ayarlanmalıdır. Ayrıntılar için aşağıdaki [Grup iddialarına](#configuring-groups-optional-claims) bakın. Grup talepleri hakkında daha fazla bilgi için [bkz.](../hybrid/how-to-connect-fed-group-claims.md)
| `acct`             | Kullanıcılar kiracıdaki hesap durumu. | JWT, SAML | | Kullanıcı kiracının bir üyesiyse, değer `0`. Eğer misafirlerse, `1`değeri. |
| `upn`                      | UserPrincipalName iddiası. | JWT, SAML  |           | Bu talep otomatik olarak dahil edilse de, konuk kullanıcı durumundaki davranışını değiştirmek için ek özellikler eklemek için isteğe bağlı bir talep olarak belirtebilirsiniz.  |

## <a name="v20-specific-optional-claims-set"></a>v2.0'a özel isteğe bağlı talepler kümesi

Bu talepler her zaman v1.0 Azure AD belirteçlerine dahil edilir, ancak istenmedikçe v2.0 belirteçlerine dahil edilmez. Bu talepler yalnızca JWT'ler (kimlik belirteçleri ve Erişim Belirteçleri) için geçerlidir. 

**Tablo 3: v2.0-sadece isteğe bağlı talepler**

| JWT İddiası     | Adı                            | Açıklama                                | Notlar |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | IP Adresi                      | İstemcinin oturum açtığı IP adresi.   |       |
| `onprem_sid`  | Şirket Içi Güvenlik Tanımlayıcısı |                                             |       |
| `pwd_exp`     | Parola Son Kullanma Tarihi        | Parolanın süresinin dolduğu tarih. |       |
| `pwd_url`     | Parola URL'si Değiştir             | Kullanıcının parolasını değiştirmek için ziyaret edebileceği bir URL.   |   |
| `in_corp`     | İç Şirket Ağı        | İstemci şirket ağından oturum açıyorsa sinyaller. Eğer değilse, talep dahil değildir.   |  MFA'daki [güvenilir IP](../authentication/howto-mfa-mfasettings.md#trusted-ips) ayarlarına göre.    |
| `nickname`    | Takma                        | Kullanıcı için ek bir ad. Takma ad veya soyad ayrıdır. `profile` Kapsamı gerektirir.| 
| `family_name` | Soyadı                       | Kullanıcı nesnesinde tanımlandığı şekilde kullanıcının soyadını, soyadını veya soyadını sağlar. <br>"family_name":"Miller" | MSA ve Azure AD'de desteklenir. `profile` Kapsamı gerektirir.   |
| `given_name`  | Ad                      | Kullanıcı nesnesi üzerinde ayarlanan kullanıcının ilk veya "verilen" adını sağlar.<br>"given_name": "Frank"                   | MSA ve Azure AD'de desteklenir.  `profile` Kapsamı gerektirir. |
| `upn`         | Kullanıcı Asıl Adı | username_hint parametresi ile kullanılabilen kullanıcı için bir identifer.  Kullanıcı için dayanıklı bir tanımlayıcı değildir ve anahtar veriler için kullanılmamalıdır. | Talebin yapılandırması için aşağıdaki [ek özelliklere](#additional-properties-of-optional-claims) bakın. `profile` Kapsamı gerektirir.|

### <a name="additional-properties-of-optional-claims"></a>İsteğe bağlı taleplerin ek özellikleri

Bazı isteğe bağlı talepler, talebin döndürülme şeklini değiştirecek şekilde yapılandırılabilir. Bu ek özellikler çoğunlukla farklı veri beklentileri olan şirket içi uygulamaların geçişine yardımcı olmak için kullanılır `include_externally_authenticated_upn_without_hash` `#`(örneğin, UPN'de karma işaretleri işleyemeyen istemcilere yardımcı olur)

**Tablo 4: İsteğe bağlı talepleri yapılandırmak için değerler**

| Özellik adı  | Ek Özellik adı | Açıklama |
|----------------|--------------------------|-------------|
| `upn`          |                          | Hem SAML hem de JWT yanıtları ve v1.0 ve v2.0 belirteçleri için kullanılabilir. |
|                | `include_externally_authenticated_upn`  | Kaynak kiracısında depolanan konuk UPN'yi içerir. Örneğin, `foo_hometenant.com#EXT#@resourcetenant.com` |             
|                | `include_externally_authenticated_upn_without_hash` | Yukarıdaki gibi, karma işaretleri (`#`) alt çizer (`_`), örneğin değiştirilir dışında`foo_hometenant.com_EXT_@resourcetenant.com` |

#### <a name="additional-properties-example"></a>Ek özellikler örneği

    ```json
        "optionalClaims": 
         {
             "idToken": [ 
            { 
                      "name": "upn", 
                      "essential": false,
                  "additionalProperties": [ "include_externally_authenticated_upn"]  
                    }
                 ]
        }
    ```

Bu İsteğe Bağlı Talepler nesnesi, istemciye iade edilen kimlik belirtecinin ek ev kiracısı ve kaynak kiracı bilgileriyle birlikte bir upn talep eklemesine neden olur. Talep `upn` yalnızca kullanıcı kiracıda konuksa (kimlik doğrulaması için farklı bir IDP kullanıyorsa) belirteçte değiştirilir. 

## <a name="configuring-optional-claims"></a>İsteğe bağlı talepleri yapılandırma

> [!IMPORTANT]
> Erişim belirteçleri **her zaman** istemci değil, kaynağın bildirimi kullanılarak oluşturulur.  Yani istek `...scope=https://graph.microsoft.com/user.read...` kaynak Microsoft Graph API olduğunu.  Bu nedenle, erişim belirteci istemcinin bildirimi ni değil, Microsoft Graph API bildirimi kullanılarak oluşturulur.  Uygulamanızın bildirimini değiştirmek, Microsoft Graph API'sinin farklı görünmesine neden olmaz.  Değişikliklerinizin `accessToken` geçerli olduğunu doğrulamak için, başka bir uygulama için değil, uygulamanız için bir belirteç isteyin.  


Kullanıcı Arabirimi veya uygulama bildirimi aracılığıyla uygulamanız için isteğe bağlı talepleri yapılandırabilirsiniz.

1. [Azure portalına](https://portal.azure.com)gidin. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet** bölümünden **Uygulama kayıtlarını**seçin.
1. Listede isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı seçin.

**İsteğe bağlı talepleri UI üzerinden yapılandırma:**

[![UI kullanarak isteğe bağlı taleplerin nasıl yapılandırılabildiğini gösterir](./media/active-directory-optional-claims/token-configuration.png)](./media/active-directory-optional-claims/token-configuration.png)

1. **Yönet** bölümünden **Token yapılandırmasını (önizleme)** seçin.
2. **İsteğe bağlı talep ekle'yi**seçin.
3. Yapılandırmak istediğiniz belirteç türünü seçin.
4. Eklemek için isteğe bağlı talepleri seçin.
5. **Ekle**’ye tıklayın.

**İsteğe bağlı taleplerin uygulama bildirimi aracılığıyla yapılandırılması:**

[![Uygulama bildirimini kullanarak isteğe bağlı taleplerin nasıl yapılandırılabildiğini gösterir](./media/active-directory-optional-claims/app-manifest.png)](./media/active-directory-optional-claims/app-manifest.png)

1. **Yönet** bölümünden **Manifest'i**seçin. Web tabanlı bir manifesto düzenleyicisi açılır ve manifestoyu yeniden doldurmanıza olanak sağlar. İsteğe bağlı olarak **İndir** seçeneğini belirleyip bildirimi yerel ortamda düzenledikten sonra **Yükle** seçeneğiyle uygulamanıza yeniden uygulayabilirsiniz. Uygulama bildirimi hakkında daha fazla bilgi için Azure [REKLAM uygulama bildirimini anlama makalesine](reference-app-manifest.md)bakın.

    Aşağıdaki uygulama bildirimi girişi, kimlik, erişim ve SAML belirteçlerine auth_time, ipaddr ve upn isteğe bağlı talepleri ekler.

    ```json
        "optionalClaims":  
           {
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

2. İşlemi tamamladıktan sonra **Kaydet**’e tıklayın. Artık belirtilen isteğe bağlı talepler, başvurunuzun belirteçlerine eklenecektir.    


### <a name="optionalclaims-type"></a>OptionalClaims türü

Bir uygulama tarafından istenen isteğe bağlı talepleri bildirir. Bir uygulama, güvenlik belirteci hizmetinden alabileceği üç belirteç türü (kimlik belirteci, erişim belirteci, SAML 2 belirteci) her birinde döndürülecek isteğe bağlı talepleri yapılandırabilir. Uygulama, her belirteç türünde döndürülecek farklı bir isteğe bağlı talep kümesini yapılandırabilir. Uygulama varlığının İsteğe Bağlı Talepler özelliği, Isteğe Bağlı Talepler nesnesidir.

**Tablo 5: OptionalClaims türü özellikleri**

| Adı        | Tür                       | Açıklama                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Toplama (İsteğe Bağlı Talep) | İsteğe bağlı talepler JWT kimlik belirteci döndürülür. |
| `accessToken` | Toplama (İsteğe Bağlı Talep) | İsteğe bağlı talepler JWT erişim belirteci döndürülür. |
| `saml2Token`  | Toplama (İsteğe Bağlı Talep) | İsteğe bağlı talepler SAML belirteci döndürülür.   |

### <a name="optionalclaim-type"></a>İsteğe BağlıTalep türü

Bir uygulama veya hizmet ilkesiyle ilişkili isteğe bağlı bir talep içerir. [OptionalClaims](https://docs.microsoft.com/graph/api/resources/optionalclaims?view=graph-rest-1.0) türünün idToken, accessToken ve saml2Token özellikleri İsteğe Bağlı Talep koleksiyonudur.
Belirli bir talep tarafından desteklenirse, Ek Özellikler alanını kullanarak İsteğe Bağlı Talep'in davranışını da değiştirebilirsiniz.

**Tablo 6: İsteğe Bağlı Talep türü özellikleri**

| Adı                 | Tür                    | Açıklama                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | İsteğe bağlı talebin adı.                                                                                                                                                                                                                                                                           |
| `source`               | Edm.String              | Talebin kaynağı (dizin nesnesi). Uzantı özelliklerinden önceden tanımlanmış talepler ve kullanıcı tanımlı talepler vardır. Kaynak değeri null ise, talep önceden tanımlanmış isteğe bağlı bir taleptir. Kaynak değer kullanıcıysa, ad özelliğindeki değer kullanıcı nesnesinin uzantı özelliğidir. |
| `essential`            | Edm.Boolean             | Değer doğruysa, son kullanıcı tarafından istenen belirli bir görev için düzgün bir yetkilendirme deneyimi sağlamak için istemci tarafından belirtilen talep gereklidir. Varsayılan değer false'tur.                                                                                                             |
| `additionalProperties` | Koleksiyon (Edm.String) | Talebin ek özellikleri. Bu koleksiyonda bir özellik varsa, ad özelliğinde belirtilen isteğe bağlı talebin davranışını değiştirir.                                                                                                                                               |
## <a name="configuring-directory-extension-optional-claims"></a>Dizin uzantısı isteğe bağlı talepleri yapılandırma

Standart isteğe bağlı talep kümesine ek olarak, belirteçleri uzantıları içerecek şekilde de yapılandırabilirsiniz. Daha fazla bilgi için [Microsoft Graph extensionProperty belgelerine](https://docs.microsoft.com/graph/api/resources/extensionproperty?view=graph-rest-1.0) bakın - şema ve açık uzantıların isteğe bağlı taleplerle desteklenmediğini, yalnızca AAD-Graph stili dizini uzantılarıtarafından desteklenmediğini unutmayın. Bu özellik, uygulamanızın kullanabileceği ek kullanıcı bilgilerini eklemek için yararlıdır (örneğin, kullanıcının belirlediği ek bir tanımlayıcı veya önemli yapılandırma seçeneği. Örnek olarak bu sayfanın altına bakın.

> [!NOTE]
> - Dizin şeması uzantıları yalnızca Azure AD özelliğidir, bu nedenle uygulama bildiriminiz özel bir uzantı talep ederse ve bir MSA kullanıcısı uygulamanızda oturum açarsa, bu uzantılar döndürülmez.

### <a name="directory-extension-formatting"></a>Dizin uzantısı biçimlendirme

Uygulama bildirimini kullanarak dizin uzantısı isteğe bağlı talepleri yapılandırırken, uzantının tam adını kullanın (biçimde: `extension_<appid>_<attributename>`). Talepte `<appid>` bulunulan başvurunun kimliğiyle eşleşmelidir. 

JWT içinde, bu iddialar aşağıdaki ad biçimi ile `extn.<attributename>`yayılan olacaktır: .

SAML belirteçleri içinde, bu iddialar aşağıdaki URI biçimi ile yayılacaktır:`http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="configuring-groups-optional-claims"></a>Grupları isteğe bağlı talepleri yapılandırma

   > [!NOTE]
   > Şirket içinde senkronize edilen kullanıcılar ve gruplar için grup adları yayılabilme özelliği Genel Önizleme'dir.

Bu bölüm, varsayılan grup objectID'den şirket içi Windows Active Directory'den eşitlenen özniteliklere grup taleplerinde kullanılan grup özniteliklerini değiştirmek için isteğe bağlı talepler altında yapılandırma seçeneklerini kapsar. Kullanıcı Arabirimi veya uygulama bildirimi aracılığıyla uygulamanız için isteğe bağlı talepleri gruplandırırsınız.

> [!IMPORTANT]
> Şirket içi özniteliklerden gelen grup taleplerinin genel önizlemesi için önemli uyarılar da dahil olmak üzere daha fazla ayrıntı için Bkz. [Azure AD'li uygulamalar için grup taleplerini yapılandırın.](../hybrid/how-to-connect-fed-group-claims.md)

**Grupları isteğe bağlı talepleri UI üzerinden yapılandırma:**
1. [Azure portalında](https://portal.azure.com) oturum açın
1. Kimlik doğrulaması yaptıktan sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin
1. Sol menüden **Azure Etkin Dizini'ni** seçin
1. **Yönet** bölümünde, **Uygulama kayıtlarını** seçin
1. Listedeki isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı seçin
1. **Yönet** bölümünde, **Token yapılandırmasını seçin (önizleme)**
2. **Gruplar ait ekle'yi** seçin
3. Döndürülecek grup türlerini seçin **(Tüm Gruplar,** **SecurityGroup**veya **DirectoryRole).** **Tüm Gruplar** seçeneği **SecurityGroup,** **DirectoryRole**ve **DistributionList'i** içerir
4. İsteğe bağlı: grup talep değerini şirket içi grup özniteliklerini içerecek şekilde değiştirmek veya talep türünü bir role değiştirmek için belirli belirteç türü özelliklerini tıklatın
5. **Kaydet'i** tıklatın

**Uygulama bildirimi aracılığıyla grupları isteğe bağlı talepleri yapılandırma:**
1. [Azure portalında](https://portal.azure.com) oturum açın
1. Kimlik doğrulaması yaptıktan sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin
1. Sol menüden **Azure Etkin Dizini'ni** seçin
1. Listedeki isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı seçin
1. **Yönet** bölümünde, **Bildirim'i** seçin
3. Bildirim düzenleyicisini kullanarak aşağıdaki girişi ekleyin:

   Geçerli değerler şunlardır:

   - "Tümü" (bu seçenek SecurityGroup, DirectoryRole ve DistributionList içerir)
   - "Güvenlik Grubu"
   - "DizinRolü"

   Örnek:

    ```json
        "groupMembershipClaims": "SecurityGroup"
    ```

   Varsayılan olarak Grup ObjectIDs grup talep değeri yayılacak.  Talep değerini şirket grup özniteliklerini içerecek şekilde değiştirmek veya talep türünü role değiştirmek için İsteğe Bağlı Talepler yapılandırmasını aşağıdaki gibi kullanın:

3. Grup adı yapılandırması isteğe bağlı talepleri ayarlayın.

   İsteğe bağlı talepler bölümünde şirket içi AD grubu özniteliklerini içerecek şekilde belirtilecek şekilde belirtin, isteğe bağlı talep, istenen isteğe bağlı talep adı ve istenen ek özellikleri belirtin.  Birden çok belirteç türü listelenebilir:

   - OIDC kimlik belirteci için idToken
   - oAuth erişim jetonu için accessToken
   - SAML belirteçleri için Saml2Token.

   > [!NOTE]
   > Saml2Token türü hem SAML1.1 hem de SAML2.0 biçim belirteçleri için geçerlidir

   İlgili her belirteç türü için, bildirimdeki İsteğe Bağlı Talepler bölümünü kullanma iddiasını grupları değiştirin. İsteğe Bağlı İddialar şema aşağıdaki gibidir:

    ```json
       {
       "name": "groups",
       "source": null,
       "essential": false,
       "additionalProperties": []
       }
    ```

   | İsteğe bağlı iddialar şema | Değer |
   |----------|-------------|
   | **Adı:** | "Gruplar" olmalıdır |
   | **Kaynak:** | Kullanılmadı. Geçersiz kılın |
   | **Temel:** | Kullanılmadı. Yanlışı atla veya belirt |
   | **ek Özellikler:** | Ek özelliklerin listesi.  Geçerli seçenekler "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles" |

   Ek Özellikler'de yalnızca "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" gerekir.  Birden fazla varsa, ilk kullanılır ve diğerleri göz ardı.

   Bazı uygulamalar, rol talebindeki kullanıcı hakkında grup bilgileri gerektirir.  Talep türünü bir grup talebinden bir rol talebine değiştirmek için ek özelliklere "emit_as_roles" ekleyin.  Grup değerleri rol talebinde yayılacaktır.

   > [!NOTE]
   > Kullanıcının atandığı yapılandırılan herhangi bir Uygulama Rolü "emit_as_roles" kullanılırsa, rol iddiasında görünmez

**Örnekler:**

1) Grupları OAuth erişim belirteçleri olarak grup adları olarak dnsDomainName\sAMAccountName biçiminde yaslar

    
    **UI yapılandırması:**

    [![UI kullanarak isteğe bağlı taleplerin nasıl yapılandırılabildiğini gösterir](./media/active-directory-optional-claims/groups-example-1.png)](./media/active-directory-optional-claims/groups-example-1.png)


    **Başvuru bildirimi girişi:**
    ```json
        "optionalClaims": {
            "accessToken": [{
            "name": "groups",
            "additionalProperties": ["dns_domain_and_sam_account_name"]
            }]
        }
    ```

 
    
2) Saml ve OIDC ID Belirteçleri rolleri iddia olarak netbiosDomain\sAMAccountName formatında iade edilecek grup adları yamit

    **UI yapılandırması:**

    [![UI kullanarak isteğe bağlı taleplerin nasıl yapılandırılabildiğini gösterir](./media/active-directory-optional-claims/groups-example-2.png)](./media/active-directory-optional-claims/groups-example-2.png)

    **Başvuru bildirimi girişi:**
    
    ```json
        "optionalClaims": {
        "saml2Token": [{
            "name": "groups",
            "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
        }],
        "idToken": [{
            "name": "groups",
            "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
        }]
    ``` 
     

## <a name="optional-claims-example"></a>İsteğe bağlı talepler örneği

Bu bölümde, uygulamanız için isteğe bağlı talepler özelliğini nasıl kullanabileceğinizi görmek için bir senaryoyu gözden geçirebilirsiniz.
İsteğe bağlı talepleri etkinleştirmek ve yapılandırmak için bir uygulamanın kimlik yapılandırmasındaki özellikleri güncelleştirmek için birden çok seçenek vardır:
-    **Jeton yapılandırmasını (önizleme)** UI'yi kullanabilirsiniz (aşağıdaki örneğe bakın)
-    **Manifesto'yu** kullanabilirsiniz (aşağıdaki örneğe bakın). Bildirime giriş için önce [Azure AD uygulama bildirim belgesini anlama'yı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) okuyun.
-   Uygulamanızı güncelleştirmek için Microsoft Graph API kullanan bir uygulama da yazmak [mümkündür.](https://docs.microsoft.com/graph/use-the-api?context=graph%2Fapi%2F1.0&view=graph-rest-1.0) Microsoft Graph API başvuru kılavuzundaki [İsteğe Bağlı Talepler](https://docs.microsoft.com/graph/api/resources/optionalclaims?view=graph-rest-1.0) türü, isteğe bağlı talepleri yapılandırmanıza yardımcı olabilir.

**Örnek:** Aşağıdaki örnekte, uygulamanız için amaçlanan erişim, kimlik ve SAML belirteçlerine isteğe bağlı talepler eklemek için **Token yapılandırma (önizleme)** Kullanıcı Arabirimi ve **Bildirimi'ni** kullanırsınız. Uygulamanın alabileceği her belirteç türüne farklı isteğe bağlı talepler eklenir:
-    Kimlik belirteçleri artık federe kullanıcılar için upn tam`<upn>_<homedomain>#EXT#@<resourcedomain>`formda () içerecektir.
-    Diğer istemcilerin bu uygulama için talep ettiği erişim belirteçleri artık auth_time
-    SAML belirteçleri artık skypeId dizin şeması uzantısını içerecektir (bu örnekte, bu uygulamanın uygulama kimliği ab603c56068041afb2f6832e2a17e237). SAML belirteçleri Skype Kimliğini `extension_skypeId`.

**UI yapılandırması:**

1. [Azure portalında](https://portal.azure.com) oturum açın

1. Kimliğini doğruladıktan sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin.

1. Sol menüden **Azure Etkin Dizini'ni** seçin.

1. **Yönet** bölümünde, **Uygulama kayıtlarını**seçin.

1. Listede isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı bulun ve üzerine tıklayın.

1. **Yönet** bölümünde, **Token yapılandırmasını (önizleme)** tıklatın.

1. **İsteğe bağlı talep ekle'yi**seçin, **kimlik** belirteci türünü seçin, talepler listesinden **upn'ı** seçin ve sonra **Ekle'yi**tıklatın.

1. **İsteğe bağlı talep ekle'yi**seçin, **Access** token türünü seçin, talepler listesinden **auth_time** seçin ve sonra **Ekle'yi**tıklatın.

1. Token Yapılandırmagenel bakış ekranından, **upn**yanındaki kalem simgesine tıklayın , **Harici kimlik doğrulamalı** geçiş tıklatın ve sonra **Kaydet'i**tıklatın.

1. **İsteğe bağlı talep ekle'yi**seçin, **SAML** belirteci türünü seçin, talep listesinden **extn.skypeID'yi** seçin (yalnızca skypeID adında bir Azure REKLAM kullanıcı nesnesi oluşturduysanız geçerlidir) ve sonra **Ekle'yi**tıklatın.

    [![UI kullanarak isteğe bağlı taleplerin nasıl yapılandırılabildiğini gösterir](./media/active-directory-optional-claims/token-config-example.png)](./media/active-directory-optional-claims/token-config-example.png)

**Manifest yapılandırması:**
1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Kimliğini doğruladıktan sonra, sayfanın sağ üst köşesinden seçerek Azure AD kiracınızı seçin.
1. Sol menüden **Azure Etkin Dizini'ni** seçin.
1. Listede isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı bulun ve üzerine tıklayın.
1. **Yönet** bölümünde, satır altı bildirim düzenleyicisini açmak için **Bildirim'i** tıklatın.
1. Bu düzenleyiciyi kullanarak bildirimi doğrudan edinebilirsiniz. Bildirim, [Uygulama varlığı](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)için şema izler ve kaydedildikten sonra bildirimi otomatik olarak biçimlendirin. `OptionalClaims` Özelliğe yeni öğeler eklenecektir.

    ```json
            "optionalClaims": {
                "idToken": [ 
                     { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
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
        ``` 


1. When you're finished updating the manifest, click **Save** to save the manifest.

## Next steps

Learn more about the standard claims provided by Azure AD.

- [ID tokens](id-tokens.md)
- [Access tokens](access-tokens.md)
