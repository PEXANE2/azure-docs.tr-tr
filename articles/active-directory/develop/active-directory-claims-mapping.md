---
title: Azure AD kiracı uygulama taleplerini özelleştirme (PowerShell)
titleSuffix: Microsoft identity platform
description: Bu sayfada Azure Active Directory claims eşleme açıklanmaktadır.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, jeedes, luleon
ms.openlocfilehash: 49860504da8dd2a1b994a23a24df95f59c959c90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263198"
---
# <a name="how-to-customize-claims-emitted-in-tokens-for-a-specific-app-in-a-tenant-preview"></a>Nasıl yapılır: Kiracıdaki belirli bir uygulama için belirteçlerde yayılan talepleri özelleştirme (Önizleme)

> [!NOTE]
> Bu özellik, bugün portal üzerinden sunulan [talep özelleştirmesinin](active-directory-saml-claims-customization.md) yerini alır ve bunun yerini alır. Aynı uygulamada, bu belgede ayrıntılı olarak açıklanan Grafik/PowerShell yöntemine ek olarak portalı kullanarak talepleri özelleştiriseniz, bu uygulama için verilen belirteçler portaldaki yapılandırmayı yoksayacaktır. Bu belgede ayrıntılı yöntemlerle yapılan yapılandırmalar portala yansıtılmayacaktır.

Bu özellik, kiracı yöneticileri tarafından, kiracılarında belirli bir uygulama için belirteçlerde yayılan talepleri özelleştirmek için kullanılır. Talep-eşleme ilkelerini şu şekilde kullanabilirsiniz:

- Hangi taleplerin jetonlara dahil olduğunu seçin.
- Zaten var olmayan talep türleri oluşturun.
- Belirli taleplerde yayılan verilerin kaynağını seçin veya değiştirin.

> [!NOTE]
> Bu özellik şu anda genel önizlemede. Değişiklikleri geri almaya veya kaldırmaya hazırlıklı olun. Bu özellik, genel önizleme sırasında herhangi bir Azure Etkin Dizin (Azure AD) aboneliğinde kullanılabilir. Ancak, özellik genel kullanıma sunulduğunda, özelliğin bazı yönleri bir Azure REKLAM premium aboneliği gerektirebilir. Bu özellik, WS-Fed, SAML, OAuth ve OpenID Connect protokolleri için talep eşleme ilkelerinin yapılandırılmalarını destekler.

## <a name="claims-mapping-policy-type"></a>Talepler eşleme ilkesi türü

Azure AD'de, **Bir İlke** nesnesi tek tek uygulamalarda veya bir kuruluştaki tüm uygulamalarda uygulanan bir kural kümesini temsil eder. Her tür ilke, daha sonra atandıkları nesnelere uygulanan bir dizi özellik içeren benzersiz bir yapıya sahiptir.

Talep eşleme ilkesi, belirli uygulamalar için verilen belirteçlerde yayılan talepleri değiştiren bir **İlke** nesnesidir.

## <a name="claim-sets"></a>Talep setleri

Jetonlarda nasıl ve ne zaman kullanıldıklarını tanımlayan belirli talep kümeleri vardır.

| Talep seti | Açıklama |
|---|---|
| Çekirdek talep seti | İlke ne olursa olsun her belirteç te bulunur. Bu talepler de kısıtlanmış olarak kabul edilir ve değiştirilemez. |
| Temel talep kümesi | Belirteçler için varsayılan olarak yayılan talepleri içerir (temel talep kümesine ek olarak). Talep eşleme ilkelerini kullanarak temel talepleri atlayabilir veya değiştirebilirsiniz. |
| Kısıtlı talep kümesi | İlke kullanılarak değiştirilemez. Veri kaynağı değiştirilemez ve bu talepleri oluştururken hiçbir dönüşüm uygulanmaz. |

### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Tablo 1: JSON Web Belirteci (JWT) sınırlı talep seti

| Talep türü (ad) |
| ----- |
| _claim_names |
| _claim_sources |
| access_token |
| account_type |
| acr |
| Aktör |
| aktörtoken |
| Aıo |
| altsecid |
| amr |
| app_chain |
| app_displayname |
| app_res |
| appctx |
| appctxsender |
| Appıd |
| appidacr |
| Iddia |
| at_hash |
| aud |
| auth_data |
| auth_time |
| authorization_code |
| azp |
| azpacr |
| c_hash |
| ca_enf |
| cc |
| cert_token_use |
| client_id |
| cloud_graph_host_name |
| cloud_instance_name |
| Cnf |
| kod |
| denetimler |
| credential_keys |
| Kss |
| csr_type |
| Deviceıd |
| dns_names |
| domain_dns_name |
| domain_netbios_name |
| e_exp |
| e-posta |
| endpoint |
| enfpolids |
| Exp |
| expires_on |
| grant_type |
| Grafik |
| group_sids |
| gruplar |
| hasgroups |
| hash_alg |
| home_oid |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/expired` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` |
| ıat |
| identityprovider |
| ıdp |
| in_corp |
| örnek |
| ipaddr |
| isbrowserhostedapp |
| ıss |
| jwk |
| key_id |
| Key_type |
| mam_compliance_url |
| mam_enrollment_url |
| mam_terms_of_use_url |
| mdm_compliance_url |
| mdm_enrollment_url |
| mdm_terms_of_use_url |
| nameid |
| nbf |
| netbios_name |
| Nonce |
| Oıd |
| on_prem_id |
| onprem_sam_account_name |
| onprem_sid |
| openid2_id |
| password |
| platf |
| polids |
| pop_jwk |
| preferred_username |
| previous_refresh_token |
| primary_sid |
| puid |
| pwd_exp |
| pwd_url |
| redirect_uri |
| refresh_token |
| refreshtoken |
| request_nonce |
| kaynak |
| rol |
| roles |
| scope |
| Scp |
| Sid |
| Imza |
| signin_state |
| src1 |
| src2 |
| Alt |
| tbid |
| tenant_display_name |
| tenant_region_scope |
| thumbnail_photo |
| Tid |
| tokenAutologonEtkin |
| güvenilir fordelegation |
| unique_name |
| Upn |
| user_setting_sync_url |
| kullanıcı adı |
| Uti |
| Ver |
| verified_primary_email |
| verified_secondary_email |
| wids |
| win_ver |

### <a name="table-2-saml-restricted-claim-set"></a>Tablo 2: SAML kısıtlı talep seti

| Talep türü (URI) |
| ----- |
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/expired`|
|`http://schemas.microsoft.com/identity/claims/accesstoken`|
|`http://schemas.microsoft.com/identity/claims/openid2_id`|
|`http://schemas.microsoft.com/identity/claims/identityprovider`|
|`http://schemas.microsoft.com/identity/claims/objectidentifier`|
|`http://schemas.microsoft.com/identity/claims/puid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier [MR1]`|
|`http://schemas.microsoft.com/identity/claims/tenantid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`|
|`http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/groups`|
|`http://schemas.microsoft.com/claims/groups.link`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/role`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/wids`|
|`http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant`|
|`http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown`|
|`http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged`|
|`http://schemas.microsoft.com/2014/03/psso`|
|`http://schemas.microsoft.com/claims/authnmethodsreferences`|
|`http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier`|
|`http://schemas.microsoft.com/identity/claims/scope`|

## <a name="claims-mapping-policy-properties"></a>İddialar eşleme ilkesi özellikleri

Hangi iddiaların yayıldığı ve verilerin nereden geldiğini denetlemek için, bir talep eşleme ilkesinin özelliklerini kullanın. Bir ilke ayarlanmazsa, sistem temel talep kümesini, temel talep kümesini ve uygulamanın almayı seçtiği [isteğe bağlı talepleri](active-directory-optional-claims.md) içeren belirteçleri alır.

### <a name="include-basic-claim-set"></a>Temel talep kümesini ekleme

**Dize:** IncludeBasicClaimSet

**Veri türü:** Boolean (Doğru veya Yanlış)

**Özet:** Bu özellik, temel talep kümesinin bu ilkeden etkilenen belirteçlere dahil edilip edilemeyeceğini belirler.

- True olarak ayarlanırsa, temel talep kümesindeki tüm talepler ilkeden etkilenen belirteçlerde yayılır. 
- False olarak ayarlanırsa, aynı politikanın hak iddialarına ayrı ayrı eklenmedikleri sürece, temel talep kümesindeki talepler belirteçlerde yer almaz.

> [!NOTE] 
> Temel talep kümesindeki talepler, bu özelliğin ne olarak ayarlıolduğundan bağımsız olarak her belirteçte mevcuttur. 

### <a name="claims-schema"></a>İddialar şema

**Dize:** İddialar Schema

**Veri türü:** Bir veya daha fazla iddia şema girişleri ile JSON blob

**Özet:** Bu özellik, temel talep kümesi ne kadar önemli, temel talep kümesi ve temel talep kümesine ek olarak, poliçeden etkilenen belirteçlerde hangi taleplerin mevcut olduğunu tanımlar.
Bu özellikte tanımlanan her hak talebi girişi için belirli bilgiler gereklidir. Verilerin nereden geldiğini **(Değer** veya **Kaynak/Kimlik çifti)** ve verilerin hangi talepte bulunduğuna (**Talep Türü)** belirtin.

### <a name="claim-schema-entry-elements"></a>Hak iddia şema giriş öğeleri

**Değer:** Değer öğesi, talepte yayılacak veri olarak statik bir değer tanımlar.

**Kaynak/Kimlik çifti:** Kaynak ve Kimlik öğeleri, talepteki verilerin nereden geldiğini tanımlar. 

Kaynak öğesini aşağıdaki değerlerden birine ayarlayın: 

- "kullanıcı": Talepteki veriler Kullanıcı nesnesi üzerindeki bir özelliktir. 
- "uygulama": Talepteki veriler, uygulama (istemci) hizmet sorumlusundaki bir özelliktir. 
- "kaynak": Talepteki veriler kaynak hizmeti ilkesindeki bir özelliktir.
- "hedef kitle": Talepteki veriler, hizmet ilkesindeki belirteç (istemci veya kaynak hizmeti ilkesi) olan bir özelliktir.
- "şirket": Talepteki veriler, kaynak kiracısının Şirket nesnesi üzerindeki bir özelliktir.
- "dönüşüm": Talepteki veriler talep dönüşümündendir (bu makalenin ilerleyen bölümlerinde "Talep dönüşümü" bölümüne bakın).

Kaynak dönüşümise, **TransformationID** öğesi de bu talep tanımına dahil edilmelidir.

Kimlik öğesi, kaynaktaki hangi özelliğin talep için değer sağladığını tanımlar. Aşağıdaki tabloda, Kaynak'ın her değeri için geçerli olan kimlik değerleri listelenir.

#### <a name="table-3-valid-id-values-per-source"></a>Tablo 3: Kaynak başına geçerli kimlik değerleri

| Kaynak | Kimlik | Açıklama |
|-----|-----|-----|
| Kullanıcı | surname | Aile Adı |
| Kullanıcı | givenname | Verilen Ad |
| Kullanıcı | Displayname | Görünen Ad |
| Kullanıcı | Objectıd | ObjectID |
| Kullanıcı | posta | E-posta Adresi |
| Kullanıcı | Userprincipalname | Kullanıcı Asıl Adı |
| Kullanıcı | bölüm|Bölüm|
| Kullanıcı | onpremisessamaccountname | Şirket içi SAM Hesap Adı |
| Kullanıcı | netbiosname| NetBios Adı |
| Kullanıcı | dnsdomainname | DNS Etki Alanı Adı |
| Kullanıcı | öncül güvenlik tanımlayıcı | Şirket Içi Güvenlik Tanımlayıcısı |
| Kullanıcı | Şirketadı| Organizasyon Adı |
| Kullanıcı | Streetaddress | Adres |
| Kullanıcı | Postakodu | Posta Kodu |
| Kullanıcı | tercihlanguange | Tercih Edilen Dil |
| Kullanıcı | inpremisesuserprincipalname | Şirket içi UPN |
| Kullanıcı | Mailnickname | Posta Takma Adı |
| Kullanıcı | extensionattribute1 | Uzatma Özniteliği 1 |
| Kullanıcı | extensionattribute2 | Uzatma Özniteliği 2 |
| Kullanıcı | extensionattribute3 | Uzatma Özniteliği 3 |
| Kullanıcı | extensionattribute4 | Uzatma Özniteliği 4 |
| Kullanıcı | extensionattribute5 | Uzatma Özniteliği 5 |
| Kullanıcı | extensionattribute6 | Uzatma Özniteliği 6 |
| Kullanıcı | extensionattribute7 | Uzatma Özniteliği 7 |
| Kullanıcı | extensionattribute8 | Uzatma Özniteliği 8 |
| Kullanıcı | extensionattribute9 | Uzatma Özniteliği 9 |
| Kullanıcı | extensionattribute10 | Uzatma Özniteliği 10 |
| Kullanıcı | extensionattribute11 | Uzantı Özniteliği 11 |
| Kullanıcı | extensionattribute12 | Uzantı Özniteliği 12 |
| Kullanıcı | extensionattribute13 | Uzantı Özniteliği 13 |
| Kullanıcı | extensionattribute14 | Uzantı Özniteliği 14 |
| Kullanıcı | extensionattribute15 | Uzatma Özniteliği 15 |
| Kullanıcı | othermail | Diğer Postalar |
| Kullanıcı | ülke | Ülke |
| Kullanıcı | city | Şehir |
| Kullanıcı | durum | Durum |
| Kullanıcı | iş unvanı | İş Unvanı |
| Kullanıcı | Employeeıd | Çalışan Kimlik Numarası |
| Kullanıcı | fakstelefon numarası | Faks Telefon Numarası |
| uygulama, kaynak, hedef kitle | Displayname | Görünen Ad |
| uygulama, kaynak, hedef kitle | Itiraz | ObjectID |
| uygulama, kaynak, hedef kitle | etiketler | Hizmet Müdürü Etiketi |
| Şirket | kiracı ülke | Kiracının ülkesi |

**DönüşümKimliği:** TransformationID öğesi yalnızca Kaynak öğe "dönüştürme" olarak ayarlanmışsa sağlanmalıdır.

- Bu öğe, Bu talep için verilerin nasıl oluşturulduğunu tanımlayan **ClaimsTransformation** özelliğindeki dönüşüm girişinin kimlik öğesiyle eşleşmelidir.

**Talep Türü:** **JwtClaimType** ve **SamlClaimType** öğeleri, bu iddia şeması girişinin hangi iddiayı ifade ettiğine işaret eder.

- JwtClaimType, JWT'lerde yayımlanacak iddianın adını içermelidir.
- SamlClaimType, SAML belirteçlerinde yayılan iddianın URI'sini içermelidir.

> [!NOTE]
> Kısıtlı talep kümesindeki taleplerin adları ve URI'leri, talep türü öğeleri için kullanılamaz. Daha fazla bilgi için bu makalenin ilerleyen bölümlerinde "Özel Durumlar ve kısıtlamalar" bölümüne bakın.

### <a name="claims-transformation"></a>Talepleri dönüştürme

**Dize:** İddiaDönüşümü

**Veri türü:** JSON blob, bir veya daha fazla dönüşüm girişleri ile 

**Özet:** Kaynak verilere yaygın dönüşümler uygulamak, Talep Şeması'nda belirtilen talepler için çıktı verilerini oluşturmak için bu özelliği kullanın.

**Kimlik Numarası:** TransformID Talepleri Şeması girişindeki bu dönüşüm girişine başvurmak için KIMLIK öğesini kullanın. Bu değer, bu ilke deki her dönüşüm girişi için benzersiz olmalıdır.

**Dönüşüm Yöntemi:** TransformationMethod öğesi, talep için veri oluşturmak için hangi işlemin gerçekleştirildiyi tanımlar.

Seçilen yönteme göre, bir dizi girdi ve çıktı bekleniyor. **Giriş Talepleri,** **GirişParametreleri** ve **Çıktılar Talepleri** öğelerini kullanarak giriş ve çıktıları tanımlayın.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Tablo 4: Dönüşüm yöntemleri ve beklenen girdi ve çıktılar

|Dönüşüm Yöntemi|Beklenen giriş|Beklenen çıkış|Açıklama|
|-----|-----|-----|-----|
|Birleştir|string1, string2, ayırıcı|outputClaim|Arada bir ayırıcı kullanarak giriş dizelerini birleştirir. Örneğin: string1:"foo@bar.com, string2:"sandbox" , ayırıcı:"." çıktılarıClaim:" "foo@bar.com.sandbox|
|ExtractMailPrefix|posta|outputClaim|E-posta adresinin yerel kısmını ayıklar. Örneğin: posta:"foo@bar.com" outputClaim sonuçları:"foo". Hiçbir \@ işaret yoksa, özgün giriş dizesi olduğu gibi döndürülür.|

**GirişTalepleri:** Bir inputclaims öğesini kullanarak verileri bir hak talebi şeması girişinden dönüştürmeye aktarın. İki özelliği vardır: **ClaimTypeReferenceId** ve **TransformationClaimType**.

- **ClaimTypeReferenceId** uygun giriş iddiasını bulmak için hak şema girişinin kimlik öğesi ile birleştirilir. 
- **TransformationClaimType** bu girişe benzersiz bir ad vermek için kullanılır. Bu ad, dönüştürme yöntemi için beklenen girişlerden biriyle eşleşmelidir.

**GirişParametreleri:** Dönüşüme sabit bir değer aktarmak için Bir Giriş Parametreleri öğesini kullanın. İki özelliği vardır: **Değer** ve **kimlik.**

- **Değer,** geçirilecek gerçek sabit değerdir.
- **Kimlik,** girişe benzersiz bir ad vermek için kullanılır. Ad, dönüştürme yöntemi için beklenen girişlerden biriyle eşleşmelidir.

**Çıktılar:** Dönüşüm tarafından oluşturulan verileri tutmak ve bir hak talebi şeması girişine bağlamak için bir Çıktılar öğesi kullanın. İki özelliği vardır: **ClaimTypeReferenceId** ve **TransformationClaimType**.

- **ClaimTypeReferenceId** uygun çıktı iddiasını bulmak için hak talebi şema girişinin kimliği ile birleştirilir.
- **TransformationClaimType** çıktıya benzersiz bir ad vermek için kullanılır. Ad, dönüştürme yöntemi için beklenen çıktılardan biriyle eşleşmelidir.

### <a name="exceptions-and-restrictions"></a>İstisnalar ve kısıtlamalar

**SAML NameID ve UPN:** NameID ve UPN değerlerini kaynak ladığınız öznitelikler ve izin verilen talep dönüşümleri sınırlıdır. İzin verilen değerleri görmek için tablo 5 ve tablo 6'ya bakın.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Tablo 5: SAML NameID için veri kaynağı olarak izin verilen öznitelikler

|Kaynak|Kimlik|Açıklama|
|-----|-----|-----|
| Kullanıcı | posta|E-posta Adresi|
| Kullanıcı | Userprincipalname|Kullanıcı Asıl Adı|
| Kullanıcı | onpremisessamaccountname|On Premises Sam Hesap Adı|
| Kullanıcı | Employeeıd|Çalışan Kimlik Numarası|
| Kullanıcı | extensionattribute1 | Uzatma Özniteliği 1 |
| Kullanıcı | extensionattribute2 | Uzatma Özniteliği 2 |
| Kullanıcı | extensionattribute3 | Uzatma Özniteliği 3 |
| Kullanıcı | extensionattribute4 | Uzatma Özniteliği 4 |
| Kullanıcı | extensionattribute5 | Uzatma Özniteliği 5 |
| Kullanıcı | extensionattribute6 | Uzatma Özniteliği 6 |
| Kullanıcı | extensionattribute7 | Uzatma Özniteliği 7 |
| Kullanıcı | extensionattribute8 | Uzatma Özniteliği 8 |
| Kullanıcı | extensionattribute9 | Uzatma Özniteliği 9 |
| Kullanıcı | extensionattribute10 | Uzatma Özniteliği 10 |
| Kullanıcı | extensionattribute11 | Uzantı Özniteliği 11 |
| Kullanıcı | extensionattribute12 | Uzantı Özniteliği 12 |
| Kullanıcı | extensionattribute13 | Uzantı Özniteliği 13 |
| Kullanıcı | extensionattribute14 | Uzantı Özniteliği 14 |
| Kullanıcı | extensionattribute15 | Uzatma Özniteliği 15 |

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Tablo 6: SAML NameID için izin verilen dönüştürme yöntemleri

| Dönüşüm Yöntemi | Kısıtlamalar |
| ----- | ----- |
| ExtractMailPrefix | None |
| Birleştir | Birleşen sonek, kaynak kiracısının doğrulanmış bir etki alanı olmalıdır. |

### <a name="custom-signing-key"></a>Özel imzalama anahtarı

Bir talep eşleme ilkesinin etkili olabilmesi için hizmet ana nesnesine özel bir imzalama anahtarı atanması gerekir. Bu, belirteçlerin talep eşleme ilkesinin yaratıcısı tarafından değiştirildiğinin kabul edilmesini sağlar ve uygulamaları kötü amaçlı aktörler tarafından oluşturulan talep eşleme ilkelerine karşı korur. Özel bir imzalama anahtarı eklemek için, Uygulama nesneniz için `new-azureadapplicationkeycredential` simetrik bir anahtar kimlik bilgisi oluşturmak için Azure PowerShell cmdlet'ini kullanabilirsiniz. Bu Azure PowerShell cmdlet hakkında daha fazla bilgi için [New-AzureADApplicationKeyCredential'a](https://docs.microsoft.com/powerShell/module/Azuread/New-AzureADApplicationKeyCredential?view=azureadps-2.0)bakın.

Uygulama eşleme etkin olan `appid={client_id}` [uygulamalar, OpenID Connect meta veri isteklerine](v2-protocols-oidc.md#fetch-the-openid-connect-metadata-document)ekleyerek belirteç imzalama anahtarlarını doğrulamalıdır. Kullanmanız gereken OpenID Connect meta veri belgesinin biçimi aşağıdaverilmiştir: 

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration?appid={client-id}
```

### <a name="cross-tenant-scenarios"></a>Kiracı lar arası senaryolar

Talep eşleme ilkeleri konuk kullanıcılar için geçerli değildir. Konuk kullanıcı, hizmet ilkesine atanmış bir talep eşleme ilkesiyle bir uygulamaya erişmeye çalışırsa, varsayılan belirteç verilir (politikanın etkisi yoktur).

## <a name="claims-mapping-policy-assignment"></a>İlke ataması için talepler eşleme

Talep eşleme ilkeleri yalnızca hizmet ana nesnelerine atanabilir.

### <a name="example-claims-mapping-policies"></a>Örnek talepler eşleme ilkeleri

Azure AD'de, belirli hizmet ilkeleri için belirteçlerde yayılan talepleri özelleştirebildiğinizde birçok senaryo mümkündür. Bu bölümde, talep eşleme ilkesi türünü nasıl kullanacağınızı kavramanıza yardımcı olabilecek birkaç yaygın senaryodan geçiyoruz.

#### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki örneklerde, hizmet ilkeleri için ilkeler oluşturur, günceller, bağlantı verir ve silebilirsiniz. Azure AD'de yeniyseniz, bu örneklere geçmeden önce [Azure AD kiracısını nasıl alacağınızı öğrenmenizi](quickstart-create-new-tenant.md) öneririz.

Başlamak için aşağıdaki adımları yapın:

1. En son [Azure AD PowerShell Modülü genel önizleme yayınını indirin.](https://www.powershellgallery.com/packages/AzureADPreview)
1. Azure AD yönetici hesabınızda oturum açabilmek için Bağlan komutunu çalıştırın. Yeni bir oturum başlatmak her zaman bu komutu çalıştırın.

   ``` powershell
   Connect-AzureAD -Confirm
   ```
1. Kuruluşunuzda oluşturulan tüm ilkeleri görmek için aşağıdaki komutu çalıştırın. İlkelerinizin beklendiği gibi oluşturulmakta olup olmadığını kontrol etmek için, aşağıdaki senaryolarda çoğu işlemden sonra bu komutu çalıştırmanızı öneririz.

   ``` powershell
   Get-AzureADPolicy
   ```

#### <a name="example-create-and-assign-a-policy-to-omit-the-basic-claims-from-tokens-issued-to-a-service-principal"></a>Örnek: Hizmet ilkesine verilen belirteçlerden gelen temel talepleri atlayacak bir ilke oluşturma ve atama

Bu örnekte, bağlı hizmet ilkelerine verilen belirteçlerden temel talep kümesini kaldıran bir ilke oluşturursunuz.

1. Talep eşleme ilkesi oluşturun. Belirli hizmet ilkelerine bağlı bu ilke, belirteçlerden ayarlanan temel talebi kaldırır.
   1. İlkeoluşturmak için şu komutu çalıştırın: 
    
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims" -Type "ClaimsMappingPolicy"
      ```
   2. Yeni ilkenizi görmek ve ObjectId ilkesini almak için aşağıdaki komutu çalıştırın:
    
      ``` powershell
      Get-AzureADPolicy
      ```
1. İlkeyi hizmet müdürünüze atayın. Ayrıca hizmet müdürünün ObjectId'ini de almanız gerekir.
   1. Kuruluşunuzun tüm hizmet ilkelerini görmek için [Microsoft Graph API'sini](/graph/traverse-the-graph)sorgulayabilirsiniz. Veya [Microsoft Graph Explorer'da](https://developer.microsoft.com/graph/graph-explorer)Azure REKLAM hesabınızda oturum açın.
   2. Hizmet müdürünüzin ObjectId'ini aldığınızda aşağıdaki komutu çalıştırın:  
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

#### <a name="example-create-and-assign-a-policy-to-include-the-employeeid-and-tenantcountry-as-claims-in-tokens-issued-to-a-service-principal"></a>Örnek: Bir hizmet sorumlusuna verilen belirteçlerde talep olarak EmployeeID ve TenantCountry'i içerecek bir ilke oluşturma ve atama

Bu örnekte, Bağlı hizmet ilkelerine verilen belirteçlere EmployeeID ve TenantCountry'i ekleyen bir ilke oluşturursunuz. EmployeeID, hem SAML belirteçlerinde hem de JWT'lerde ad talebi türü olarak yayımlanır. TenantCountry, hem SAML belirteçlerinde hem de JWT'lerde ülke talep türü olarak yayımlanır. Bu örnekte, belirteçlerde ayarlanan temel talepleri eklemeye devam ediyoruz.

1. Talep eşleme ilkesi oluşturun. Belirli hizmet ilkelerine bağlı bu ilke, EmployeeID ve TenantCountry taleplerini belirteçlere ekler.
   1. İlke oluşturmak için aşağıdaki komutu çalıştırın:  
     
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":"tenantcountry","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample" -Type "ClaimsMappingPolicy"
      ```
    
   2. Yeni ilkenizi görmek ve ObjectId ilkesini almak için aşağıdaki komutu çalıştırın:
     
      ``` powershell  
      Get-AzureADPolicy
      ```
1. İlkeyi hizmet müdürünüze atayın. Ayrıca hizmet müdürünün ObjectId'ini de almanız gerekir. 
   1. Kuruluşunuzun tüm hizmet ilkelerini görmek için [Microsoft Graph API'sini](/graph/traverse-the-graph)sorgulayabilirsiniz. Veya [Microsoft Graph Explorer'da](https://developer.microsoft.com/graph/graph-explorer)Azure REKLAM hesabınızda oturum açın.
   2. Hizmet müdürünüzin ObjectId'ini aldığınızda aşağıdaki komutu çalıştırın:  
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-to-a-service-principal"></a>Örnek: Hizmet ilkesine verilen belirteçlerde talep dönüşümü kullanan bir ilke oluşturma ve atama

Bu örnekte, bağlı hizmet ilkelerine verilen JWT'lere özel bir "JoinedData" iddiası yayılan bir ilke oluşturursunuz. Bu talep, kullanıcı nesnesi üzerinde extensionattribute1 özniteliği nde depolanan verileri ".sandbox" ile birleştirerek oluşturulan bir değer içerir. Bu örnekte, belirteçlerde belirlenen temel talepleri hariç tutarız.

1. Talep eşleme ilkesi oluşturun. Belirli hizmet ilkelerine bağlı bu ilke, EmployeeID ve TenantCountry taleplerini belirteçlere ekler.
   1. İlke oluşturmak için aşağıdaki komutu çalıştırın:
     
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformations":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"ID":"string2","Value":"sandbox"},{"ID":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample" -Type "ClaimsMappingPolicy"
      ```
    
   2. Yeni ilkenizi görmek ve ObjectId ilkesini almak için aşağıdaki komutu çalıştırın: 
     
      ``` powershell
      Get-AzureADPolicy
      ```
1. İlkeyi hizmet müdürünüze atayın. Ayrıca hizmet müdürünün ObjectId'ini de almanız gerekir. 
   1. Kuruluşunuzun tüm hizmet ilkelerini görmek için [Microsoft Graph API'sini](/graph/traverse-the-graph)sorgulayabilirsiniz. Veya [Microsoft Graph Explorer'da](https://developer.microsoft.com/graph/graph-explorer)Azure REKLAM hesabınızda oturum açın.
   2. Hizmet müdürünüzin ObjectId'ini aldığınızda aşağıdaki komutu çalıştırın: 
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

## <a name="see-also"></a>Ayrıca bkz.

Azure portalı aracılığıyla SAML belirtecinde verilen talepleri nasıl özelleştirebilirsiniz öğrenmek [için bkz: Kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](active-directory-saml-claims-customization.md)
