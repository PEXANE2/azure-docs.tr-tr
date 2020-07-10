---
title: Özel ilkede SAML teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede SAML teknik profili tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 724178f71befbe4eace0d3d5615871c21253c1f1
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170080"
---
# <a name="define-a-saml-identity-provider-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde bir SAML kimlik sağlayıcısı teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) SAML 2,0 kimlik sağlayıcısı için destek sağlar. Bu makalede, bu standartlaştırılmış protokolü destekleyen bir talep sağlayıcısıyla etkileşim kurmaya yönelik teknik bir profilin ayrıntıları açıklanmaktadır. SAML teknik profiliyle, [ADFS](identity-provider-adfs2016-custom.md) ve [Salesforce](identity-provider-salesforce-custom.md)gibi SAML tabanlı bir kimlik sağlayıcısıyla federasyona bağlayabilirsiniz. Bu Federasyon, kullanıcılarınızın mevcut sosyal veya kurumsal kimliklerinde oturum açmasına olanak tanır.

## <a name="metadata-exchange"></a>Meta veri değişimi

Meta veriler, bir hizmet sağlayıcı veya kimlik sağlayıcısı gibi bir SAML partisi yapılandırmasını göstermek için SAML protokolünde kullanılan bilgiler. Meta veriler, oturum açma ve oturum kapatma, sertifikalar, oturum açma yöntemi ve daha fazlası gibi hizmetlerin konumunu tanımlar. Kimlik sağlayıcısı, Azure AD B2C ile nasıl iletişim kurabileceğinizi öğrenmek için meta verileri kullanır. Meta veriler XML biçiminde yapılandırılır ve diğer tarafın meta verilerin bütünlüğünü doğrulayabilmesi için dijital imzayla imzalanmayabilir. Azure AD B2C bir SAML Identity sağlayıcısıyla federe olduğunda, bir SAML isteği başlatan ve bir SAML yanıtı bekleyen bir hizmet sağlayıcı olarak davranır. Ayrıca, bazı durumlarda, kimlik sağlayıcısı tarafından başlatılan olarak da bilinen, istenmeyen SAML kimlik doğrulaması kabul eder.

Meta veriler her iki tarafın da "statik meta veriler" veya "dinamik meta veriler" olarak yapılandırılabilir. Statik modda, tüm meta verileri bir taraftan kopyalar ve diğer tarafa ayarlarsınız. Dinamik modda, diğer taraf yapılandırmayı dinamik olarak okurken, URL 'YI meta verilere ayarlarsınız. İlkeler aynı, kimlik sağlayıcıınızda Azure AD B2C teknik profilin meta verilerini ayarlar ve Azure AD B2C kimlik sağlayıcısının meta verilerini ayarlar.

Her SAML kimlik sağlayıcısı, hizmet sağlayıcısını kullanıma sunma ve ayarlamaya yönelik farklı adımlara sahiptir, bu durumda Azure AD B2C ve kimlik sağlayıcıda Azure AD B2C meta verilerini ayarlar. Bunun nasıl yapılacağını gösteren yönergeler için kimlik sağlayıcınızın belgelerine bakın.

Aşağıdaki örnekte, bir Azure AD B2C teknik profilinin SAML meta verilerine yönelik bir URL adresi gösterilmektedir:

```
https://your-tenant-name.b2clogin.com/your-tenant-name/your-policy/samlp/metadata?idptp=your-technical-profile
```

Aşağıdaki değerleri değiştirin:

- Kiracı **adınız,** örneğin fabrikam.b2clogin.com gibi.
- ilke adınızla **ilkenize** . SAML sağlayıcısı teknik profilini yapılandırdığınız ilkeyi veya bu ilkeden devralan bir ilkeyi kullanın.
- SAML kimlik sağlayıcısı teknik profil adınızla **Teknik profiliniz** .

## <a name="digital-signing-certificates-exchange"></a>Dijital imza sertifikaları değişimi

Azure AD B2C ile SAML kimlik sağlayıcınız arasında bir güven oluşturmak için özel anahtarla geçerli bir x509 sertifikası sağlamanız gerekir. Sertifikayı özel anahtarla (. pfx dosyası) Azure AD B2C ilkesi anahtar deposuna yüklersiniz. Sağladığınız sertifikayı kullanarak SAML oturum açma isteğini dijital olarak imzalar Azure AD B2C.

Sertifika aşağıdaki yollarla kullanılır:

- Azure AD B2C, sertifikanın Azure AD B2C özel anahtarını kullanarak bir SAML isteği oluşturur ve imzalar. SAML isteği, sertifikanın Azure AD B2C ortak anahtarını kullanarak isteği doğrulayan kimlik sağlayıcısına gönderilir. Azure AD B2C ortak sertifikaya, teknik profil meta verileri aracılığıyla erişilebilir. Alternatif olarak,. cer dosyasını SAML Identity sağlayıcınıza el ile yükleyebilirsiniz.
- Kimlik sağlayıcısı, kimlik sağlayıcısının sertifikanın özel anahtarını kullanarak Azure AD B2C gönderilen verileri imzalar. Azure AD B2C, kimlik sağlayıcısının ortak sertifikasını kullanarak verileri doğrular. Her kimlik sağlayıcısı kurulum için farklı adımlara sahiptir, bunun nasıl yapılacağını gösteren yönergeler için kimlik sağlayıcınızın belgelerine bakın. Azure AD B2C, ilkenizin kimlik sağlayıcısının meta verilerini kullanarak sertifika ortak anahtarına erişmeniz gerekir.

Kendinden imzalı bir sertifika çoğu senaryo için kabul edilebilir. Üretim ortamları için, bir sertifika yetkilisi tarafından verilen bir x509 sertifikası kullanılması önerilir. Ayrıca, bu belgenin ilerleyen kısımlarında açıklandığı gibi, üretim dışı ortamlar için her iki tarafta da SAML imzalamayı devre dışı bırakabilirsiniz.

Aşağıdaki diyagramda meta veriler ve sertifika alışverişi gösterilmektedir:

![meta veriler ve sertifika değişimi](media/saml-technical-profile/technical-profile-idp-saml-metadata.png)

## <a name="digital-encryption"></a>Dijital şifreleme

SAML yanıtı onaylama 'yı şifrelemek için, kimlik sağlayıcısı her zaman bir Azure AD B2C teknik profilinde bir şifreleme sertifikasının ortak anahtarını kullanır. Azure AD B2C verilerin şifresinin çözülmesi gerektiğinde, şifreleme sertifikasının özel kısmını kullanır.

SAML yanıtı onayını şifrelemek için:

1. Azure AD B2C ilkesi anahtar deposuna özel anahtara (. pfx dosyası) sahip geçerli bir x509 sertifikası yükleyin.
2. Bir tanımlayıcılı bir **Cryptographickey** öğesini `SamlAssertionDecryption` Technical profile **cryptographickeys** koleksiyonuna ekleyin. **Storagereferenceıd** 'yi, 1. adımda oluşturduğunuz ilke anahtarının adı olarak ayarlayın.
3. Teknik profil meta verilerini **WantsEncryptedAssertions** olarak ayarlayın `true` .
4. Kimlik sağlayıcısını yeni Azure AD B2C teknik profil meta verileri ile güncelleştirin. **Anahtar tanımlayıcısını** , **use** `encryption` sertifikanızın ortak anahtarını içerecek şekilde Use özelliği ayarlanmış olarak görmeniz gerekir.

Aşağıdaki örnek, şifreleme için kullanılan SAML meta verilerinin anahtar tanımlayıcı bölümünü göstermektedir:

```xml
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

## <a name="protocol"></a>Protokol

Protokol öğesinin **Name** özniteliğinin olarak ayarlanması gerekir `SAML2` .

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, bölümünde SAML Identity provider tarafından döndürülen taleplerin bir listesini içerir `AttributeStatement` . İlkenizde tanımlanan talebin adını kimlik sağlayıcısında tanımlanan adla eşlemeniz gerekebilir. Özniteliği ayarladığınız sürece, kimlik sağlayıcısı tarafından döndürülmeyen talepleri de ekleyebilirsiniz `DefaultValue` .

### <a name="subject-name-output-claim"></a>Konu adı çıkış talebi

**KONUDAKI** SAML onaylama **NameID** 'sini normalleştirilmiş bir talep olarak okumak Için, Claim **partnerclaimtype** değerini özniteliğin değerine ayarlayın `SPNameQualifier` . `SPNameQualifier`Öznitelik sunulmadığı takdirde, Claim **partnerclaimtype** değerini özniteliğin değerine ayarlayın `NameQualifier` . 


SAML onaylama: 

```xml
<saml:Subject>
  <saml:NameID SPNameQualifier="http://your-idp.com/unique-identifier" Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient">david@contoso.com</saml:NameID>
    <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
      <SubjectConfirmationData InResponseTo="_cd37c3f2-6875-4308-a9db-ce2cf187f4d1" NotOnOrAfter="2020-02-15T16:23:23.137Z" Recipient="https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer" />
    </SubjectConfirmation>
  </saml:SubjectConfirmation>
</saml:Subject>
```

Çıkış talebi:

```xml
<OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="http://your-idp.com/unique-identifier" />
```

`SPNameQualifier` `NameQualifier` SAML onaylama işlemi içinde her iki veya öznitelik sunulmadığı takdirde, talep **partnerclaimtype** 'ı olarak ayarlayın `assertionSubjectName` . **NameID** 'nın assertion XML içindeki ilk değer olduğundan emin olun. Birden fazla onay tanımladığınızda, Azure AD B2C son onaylamanın konu değerini seçer.

Aşağıdaki örnekte, bir SAML kimlik sağlayıcısı tarafından döndürülen talepler gösterilmektedir:

- **Issueruserıd** Claim, **assertionSubjectName** talebine eşlenir.
- **First_name** **talebi, bu talep ile** eşlendi.
- **Last_name** talebi, **Soyadı** talebine eşlenir.
- **DisplayName** talebi, **ad** talebine eşlenir.
- Ad eşleştirmesi olmayan **e-posta** talebi.

Teknik profil, kimlik sağlayıcısı tarafından döndürülmeyen talepleri de döndürür:

- Kimlik sağlayıcısının adını içeren **IdentityProvider** talebi.
- Varsayılan bir **Socialidpauthentication**değeri olan **authenticationsource** talebi.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="assertionSubjectName" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email"  />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İş ortağı varlığı | Evet | SAML kimlik sağlayıcısı meta verisinin URL 'SI. Kimlik sağlayıcısı meta verilerini kopyalayın ve CDATA öğesinin içine ekleyin`<![CDATA[Your IDP metadata]]>` |
| WantsSignedRequests | Hayır | Teknik profilin tüm giden kimlik doğrulama isteklerinin imzalanmasını gerektirip gerektirmediğini belirtir. Olası değerler: `true` veya `false` . Varsayılan değer: `true`. Değer olarak ayarlandığında `true` , **Samlmessagesigning** şifreleme anahtarının belirtilmesi gerekir ve giden tüm kimlik doğrulama istekleri imzalanır. Değer olarak ayarlanırsa `false` , **sigalg** ve **imza** parametreleri (sorgu dizesi veya post parametresi) istekten çıkarılır. Bu meta veriler ayrıca kimlik sağlayıcısıyla paylaşılan Azure AD B2C teknik profilin meta verilerinde çıkış olan meta veri **AuthnRequestsSigned** özniteliğini de denetler. Teknik profil meta verilerindeki **Wantssignedrequests** değeri olarak ayarlandıysa `false` ve **Wantauthnrequestssigned** kimlik sağlayıcısı meta verileri olarak ayarlandıysa veya belirtilmemişse isteği imzalayamıyor Azure AD B2C `false` . |
| XmlSignatureAlgorithm | Hayır | Azure AD B2C SAML isteğini imzalamak için kullandığı yöntem. Bu meta veriler, SAML isteğindeki **Sigalg** parametresinin (sorgu dizesi veya post parametresi) değerini denetler. Olası değerler: `Sha256` , `Sha384` , `Sha512` veya `Sha1` . Aynı değere sahip her iki tarafta de imza algoritmasını yapılandırdığınızdan emin olun. Yalnızca sertifikanızın desteklediği algoritmayı kullanın. |
| WantsSignedAssertions | Hayır | Teknik profilin tüm gelen onayların imzalanmasını gerektirip gerektirmediğini belirtir. Olası değerler: `true` veya `false` . Varsayılan değer: `true`. Değer olarak ayarlanmışsa `true` , `saml:Assertion` kimlik sağlayıcısı tarafından Azure AD B2C gönderilen tüm onaylar bölümü imzalanmalıdır. Değer olarak ayarlanırsa `false` , kimlik sağlayıcısı onaylamaları imzalamamamalıdır, ancak bile Azure AD B2C imza doğrulamaz. Bu meta veriler ayrıca, kimlik sağlayıcısıyla paylaşılan Azure AD B2C teknik profilin meta verilerinde çıktı olan meta veri bayrağını **WantsAssertionsSigned**denetler. Onayların doğrulanmasını devre dışı bırakırsanız, yanıt imza doğrulamasını devre dışı bırakmak isteyebilirsiniz (daha fazla bilgi için bkz. **ResponsesSigned**). |
| ResponsesSigned | Hayır | Olası değerler: `true` veya `false` . Varsayılan değer: `true`. Değer olarak ayarlanırsa `false` , kimlik sağlayıcısı SAML yanıtını imzalamaz, ancak olsa bile Azure AD B2C imzayı doğrulamaz. Değer olarak ayarlanırsa `true` , kimlik sağlayıcısı tarafından Azure AD B2C GÖNDERILEN SAML yanıtı imzalanır ve doğrulanması gerekir. SAML yanıtı doğrulamasını devre dışı bırakırsanız, onaylama imza doğrulamasını devre dışı bırakmak de isteyebilirsiniz (daha fazla bilgi için bkz. **Wantssignedasttions**). |
| WantsEncryptedAssertions | Hayır | Teknik profilin tüm gelen onayların şifrelenmesini gerektirip gerektirmediğini belirtir. Olası değerler: `true` veya `false` . Varsayılan değer: `false`. Değer olarak ayarlanırsa `true` , kimlik sağlayıcısı tarafından Azure AD B2C için gönderilen onaylar imzalanmalıdır ve **SamlAssertionDecryption** şifreleme anahtarının belirtilmesi gerekir. Değer olarak ayarlanırsa `true` , Azure AD B2C teknik profilinin meta verileri **şifreleme** bölümünü içerir. Kimlik sağlayıcısı meta verileri okur ve Azure AD B2C teknik profilinin meta verilerinde belirtilen ortak anahtarla SAML yanıtı onayını şifreler. Onaylama şifrelemesini etkinleştirirseniz, yanıt imzası doğrulamasını devre dışı bırakmanız da gerekebilir (daha fazla bilgi için bkz. **ResponsesSigned**). |
| Idpınitisenabled Profiletkin | Hayır | Bir SAML kimlik sağlayıcısı profili tarafından başlatılan çoklu oturum açma oturum profilinin etkinleştirilip etkinleştirilmediğini gösterir. Olası değerler: `true` veya `false` . Varsayılan değer: `false`. Kimlik sağlayıcısı tarafından başlatılan akışta, kullanıcının kimliği dışarıdan doğrulanır ve Azure AD B2C, ardından belirteci tüketen, düzenleme adımlarını yürüten ve bağlı olan taraf uygulamasına bir yanıt gönderen bir yanıt olarak gönderilir. |
| Nameıdpolicyformat | Hayır | İstenen konuyu temsil etmek için kullanılacak ad tanımlayıcıda kısıtlamalarını belirtir. Atlanırsa, istenen konu için kimlik sağlayıcısı tarafından desteklenen herhangi bir tanımlayıcı türü kullanılabilir. Örneğin, `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`. **Nameıdpolicyformat** , **Nameıdpolicyallowcreate**ile birlikte kullanılabilir. Hangi ad KIMLIK ilkelerinin desteklendiği hakkında rehberlik için kimlik sağlayıcınızın belgelerine bakın. |
| Nameıdpolicyallowcreate | Hayır | **Nameıdpolicyformat**kullanılırken, `AllowCreate` **nameıdpolicy**özelliğini de belirtebilirsiniz. Bu meta verilerin değeri, `true` `false` oturum açma akışında kimlik sağlayıcısının yeni bir hesap oluşturmasına izin verilip verilmeyeceğini belirtir. Bunun nasıl yapılacağını gösteren yönergeler için kimlik sağlayıcınızın belgelerine bakın. |
| Authenticationrequesbir | Hayır | Azure AD BC ve kimlik sağlayıcısı arasında kabul edilen isteğe bağlı protokol iletisi uzantısı öğeleri. Uzantı XML biçiminde sunulmuştur. XML verilerini CDATA öğesinin içine eklersiniz `<![CDATA[Your IDP metadata]]>` . Uzantı öğesinin desteklenip desteklenmediğini görmek için kimlik sağlayıcınızın belgelerini denetleyin. |
| Includeauthncontextclassreferences | Hayır | Kimlik doğrulama bağlamı sınıflarını tanımlayan bir veya daha fazla URI başvurusu belirtir. Örneğin, bir kullanıcının yalnızca Kullanıcı adı ve parola ile oturum açmasını sağlamak için değerini olarak ayarlayın `urn:oasis:names:tc:SAML:2.0:ac:classes:Password` . Korumalı bir oturum (SSL/TLS) üzerinden Kullanıcı adı ve parola aracılığıyla oturum açmaya izin vermek için, belirtin `PasswordProtectedTransport` . Desteklenen **Authncontextclassref** URI 'leri hakkında rehberlik için kimlik sağlayıcınızın belgelerine bakın. Birden çok URI 'yi virgülle ayrılmış bir liste olarak belirtin. |
| Includekeyınfo | Hayır | Bağlama ayarlandığında, SAML kimlik doğrulama isteğinin sertifikanın ortak anahtarını içerip içermediğini belirtir `HTTP-POST` . Olası değerler: `true` veya `false` . |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıkış talepleri için, [talep çözümlemenin](claim-resolver-overview.md) teknik profile dahil edilip edilmeyeceğini belirtir. Olası değerler: `true` , veya `false`   (varsayılan). Teknik profilde bir talep çözümleyici kullanmak istiyorsanız, bunu olarak ayarlayın `true` . |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**Cryptographickeys** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- | ----------- |
| SamlMessageSigning |Evet | SAML iletilerini imzalamak için kullanılacak x509 sertifikası (RSA anahtar kümesi). Azure AD B2C, istekleri imzalamak ve kimlik sağlayıcısına göndermek için bu anahtarı kullanır. |
| SamlAssertionDecryption |Evet | SAML iletilerinin şifresini çözmek için kullanılacak x509 sertifikası (RSA anahtar kümesi). Bu sertifika, kimlik sağlayıcısı tarafından sağlanmalıdır. Azure AD B2C, kimlik sağlayıcısı tarafından gönderilen verilerin şifresini çözmek için bu sertifikayı kullanır. |
| MetadataSigning |Hayır | SAML meta verilerini imzalamak için kullanılacak x509 sertifikası (RSA anahtar kümesi). Azure AD B2C meta verileri imzalamak için bu anahtarı kullanır.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C 'de SAML kimlik sağlayıcılarıyla çalışma örnekleri için aşağıdaki makalelere bakın:

- [Özel ilkeler kullanarak bir SAML kimlik sağlayıcısı olarak ADFS ekleme](identity-provider-adfs2016-custom.md)
- [SAML aracılığıyla Salesforce hesaplarını kullanarak oturum açın](identity-provider-salesforce-custom.md)
