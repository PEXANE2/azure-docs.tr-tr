---
title: Özel bir ilkede SAML teknik profilini tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilke de BIR SAML teknik profili tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 83a13e0b1bb4d55b889d96e42c8f3f18ce0f2b73
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408934"
---
# <a name="define-a-saml-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde SAML teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), SAML 2.0 kimlik sağlayıcısı için destek sağlar. Bu makalede, bu standart protokolü destekleyen bir talep sağlayıcısı yla etkileşim kurmak için teknik profilin özellikleri açıklanmaktadır. SAML teknik profili ile [ADFS](identity-provider-adfs2016-custom.md) ve [Salesforce](identity-provider-salesforce-custom.md)gibi SAML tabanlı bir kimlik sağlayıcısıyla federate yapabilirsiniz. Bu federasyon, kullanıcılarınızın mevcut sosyal veya kurumsal kimlikleriyle oturum açmalarına olanak tanır.

## <a name="metadata-exchange"></a>Meta veri değişimi

Meta veriler, SAML protokolünde hizmet sağlayıcısı veya kimlik sağlayıcısı gibi bir SAML tarafının yapılandırmasını ortaya çıkarmak için kullanılan bilgilerdir. Meta veriler, oturum açma ve imzalama, sertifikalar, oturum açma yöntemi ve daha fazlası gibi hizmetlerin konumunu tanımlar. Kimlik sağlayıcısı, Azure AD B2C ile nasıl iletişim kuracağını bilmek için meta verileri kullanır. Meta veriler XML biçiminde yapılandırılır ve diğer tarafın meta verilerin bütünlüğünü doğrulayabilmesi için dijital imza ile imzalanabilir. Azure AD B2C bir SAML kimlik sağlayıcısıyla federates zaman, bir SAML isteği başlatan ve SAML yanıtı bekleyen bir hizmet sağlayıcısı olarak hareket eder. Ve bazı durumlarda, başlatılan kimlik sağlayıcısı olarak da bilinen istenmeyen SAML kimlik doğrulamasını kabul eder.

Meta veriler her iki tarafta da "Statik Meta veri" veya "Dinamik Meta veri" olarak yapılandırılabilir. Statik modda, tüm meta verileri bir partiden kopyalar ve diğer tarafa ayarlarsınız. Dinamik modda, diğer taraf yapılandırmayı dinamik olarak okurken URL'yi meta verilere ayarlarsınız. İlkeler aynıdır, kimlik sağlayıcınızda Azure AD B2C teknik profilinin meta verilerini ayarlar sınız ve Azure AD B2C'deki kimlik sağlayıcısının meta verilerini ayarlarsınız.

Her SAML kimlik sağlayıcısının, bu durumda Azure AD B2C'yi ortaya çıkarmak ve hizmet sağlayıcısını ayarlamak ve Kimlik sağlayıcısında Azure AD B2C meta verilerini ayarlamak için farklı adımları vardır. Bunu nasıl yapacağınıza ilişkin rehberlik için kimlik sağlayıcınızın belgelerine bakın.

Aşağıdaki örnekte, Azure AD B2C teknik profilinin SAML meta verilerine bir URL adresi gösterilmektedir:

```
https://your-tenant-name.b2clogin.com/your-tenant-name/your-policy/samlp/metadata?idptp=your-technical-profile
```

Aşağıdaki değerleri değiştirin:

- kiracı **adınızla** birlikte fabrikam.b2clogin.com.
- ilke adınız ile **politikanız.** SAML sağlayıcı teknik profilini yapılandırdığınız ilkeyi veya bu ilkeden devralan bir ilkeyi kullanın.
- SAML kimlik sağlayıcınızın teknik profil adı ile **teknik profiliniz.**

## <a name="digital-signing-certificates-exchange"></a>Dijital imza sertifikaları değişimi

Azure AD B2C ile SAML kimlik sağlayıcınız arasında güven oluşturmak için özel anahtarla birlikte geçerli bir X509 sertifikası sağlamanız gerekir. Sertifikayı özel anahtarla (.pfx dosyası) Azure AD B2C ilke anahtar deposuna yüklersiniz. Azure AD B2C, sağladığınız sertifikayı kullanarak SAML oturum açma isteğini dijital olarak imzalar.

Sertifika aşağıdaki şekillerde kullanılır:

- Azure AD B2C, sertifikanın Azure AD B2C özel anahtarını kullanarak bir SAML isteği oluşturur ve işaretler. SAML isteği, sertifikanın Azure AD B2C ortak anahtarını kullanarak isteği doğrulayan kimlik sağlayıcısına gönderilir. Azure AD B2C genel sertifikasına teknik profil meta verileri aracılığıyla erişilebilir. Alternatif olarak, .cer dosyasını SAML kimlik sağlayıcınıza el ile yükleyebilirsiniz.
- Kimlik sağlayıcısı, sertifikanın kimlik sağlayıcısının özel anahtarını kullanarak Azure AD B2C'ye gönderilen verileri imzalar. Azure AD B2C, kimlik sağlayıcısının ortak sertifikasını kullanarak verileri doğrular. Her kimlik sağlayıcısının kurulum için farklı adımları vardır, bunu nasıl yapacağına ilişkin rehberlik için kimlik sağlayıcınızın belgelerine bakın. Azure AD B2C'de, ilkenizin kimlik sağlayıcısının meta verilerini kullanarak sertifika ortak anahtarına erişmesi gerekir.

Kendi imzalı sertifika çoğu senaryo için kabul edilebilir. Üretim ortamları için, bir sertifika yetkilisi tarafından verilen bir X509 sertifikası kullanılması önerilir. Ayrıca, bu belgede daha sonra açıklandığı gibi, üretim dışı bir ortam için her iki tarafta da SAML imzasını devre dışı kullanabilirsiniz.

Aşağıdaki diyagram meta verileri ve sertifika değişimini gösterir:

![meta veri ve sertifika değişimi](media/saml-technical-profile/technical-profile-idp-saml-metadata.png)

## <a name="digital-encryption"></a>Dijital şifreleme

SAML yanıt iddiasını şifrelemek için kimlik sağlayıcısı, Azure AD B2C teknik profilinde her zaman bir şifreleme sertifikasının ortak anahtarını kullanır. Azure AD B2C verilerin şifresini çözmesi gerektiğinde, şifreleme sertifikasının özel bölümünü kullanır.

SAML yanıt iddiasını şifrelemek için:

1. Azure AD B2C ilke anahtar deposuna özel anahtarla (.pfx dosyası) sahip geçerli bir X509 sertifikası yükleyin.
2. Teknik profil **CryptographicKeys** `SamlAssertionDecryption` koleksiyonuna tanımlayıcıiçeren bir **CryptographicKey** öğesi ekleyin. **StorageReferenceId'i** adım 1'de oluşturduğunuz ilke anahtarının adına ayarlayın.
3. Teknik profil meta **verisi WantsEncryptedAssertions'ı** `true`.
4. Kimlik sağlayıcısını yeni Azure AD B2C teknik profil meta verileriyle güncelleştirin. Sertifikanızın ortak anahtarını `encryption` içeren **kullanım** özelliği ayarlı Anahtar **Tanımlayıcı'yı** görmeniz gerekir.

Aşağıdaki örnekte, meta verilerin Azure AD B2C teknik profil şifreleme bölümü gösterilmektedir:

```XML
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

## <a name="protocol"></a>Protokol

Protokol öğesinin **Ad** özniteliğinin `SAML2`.

## <a name="output-claims"></a>Çıktı talepleri

**OutputClaims** öğesi, `AttributeStatement` saml kimlik sağlayıcısı tarafından bu bölüm altında döndürülen taleplerin listesini içerir. İlkenizde tanımlanan talep adını kimlik sağlayıcısında tanımlanan adla eşlemeniz gerekebilir. Özniteliği ayarladığınız `DefaultValue` sürece kimlik sağlayıcısı tarafından döndürülen talepleri de ekleyebilirsiniz.

### <a name="subject-name-output-claim"></a>Özne adı çıktısı talebi

**Saml** iddiasını Konu'daki **NameId'i** normalleştirilmiş bir talep olarak okumak için, `SPNameQualifier` claim **PartnerClaimType'ı** özniteliğin değerine ayarlayın. `SPNameQualifier`Öznitelik sunulmazsa, claim **PartnerClaimType'ı** özniteliğin `NameQualifier` değerine ayarlayın. 


SAML iddiası: 

```XML
<saml:Subject>
  <saml:NameID SPNameQualifier="http://your-idp.com/unique-identifier" Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient">david@contoso.com</saml:NameID>
    <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
      <SubjectConfirmationData InResponseTo="_cd37c3f2-6875-4308-a9db-ce2cf187f4d1" NotOnOrAfter="2020-02-15T16:23:23.137Z" Recipient="https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer" />
    </SubjectConfirmation>
  </saml:SubjectConfirmation>
</saml:Subject>
```

Çıktı talebi:

```XML
<OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="http://your-idp.com/unique-identifier" />
```

Saml `SPNameQualifier` `NameQualifier` iddiasında her iki veya öznitelik de sunulmuyorsa, **PartnerClaimType** iddiasını `assertionSubjectName`. **NameId'in** XML iddiasındaki ilk değer olduğundan emin olun. Birden fazla iddia tanımladığınızda, Azure AD B2C son iddiadan özne değerini seçer.

Aşağıdaki örnek, bir SAML kimlik sağlayıcısı tarafından döndürülen talepleri gösterir:

- **İhraççıUserId** **iddiası, SubjectName iddiasıyla** eşlenir.
- **first_name** iddiası **givenName** iddiasına eşlenir.
- **last_name** iddiası **soyadı** iddiasına göre eşlenir.
- **DisplayName** adı eşleme olmadan talep.
- Ad eşleme olmadan **e-posta** talebi.

Teknik profil, kimlik sağlayıcısı tarafından döndürülen talepleri de döndürür:

- Kimlik **Sağlayıcı,** kimlik sağlayıcısının adını içeren talepte bulunun.
- **SocialIdpAuthentication** varsayılan değeri **socialIdpAuthentication**ile kimlik doğrulamaKaynak iddiası .

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

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Ortak Varlık | Evet | SAML kimlik sağlayıcısının meta verilerinin URL'si. Kimlik sağlayıcı meta verilerini kopyalayın ve CDATA öğesinin içine ekleyin`<![CDATA[Your IDP metadata]]>` |
| İmza İstekleri İstiyor | Hayır | Teknik profilin tüm giden kimlik doğrulama isteklerinin imzalanmasını gerektirip gerektirmediğini gösterir. Olası `true` değerler: `false`veya . Varsayılan değer: `true`. Değer `true`ayarlandığında, **SamlMessageSigning** şifreleme anahtarının belirtilmesi ve giden kimlik doğrulama isteklerinin tümü imzalanmalıdır. Değer `false` **ayarlanırsa, SigAlg** ve **İmza** parametreleri (sorgu dizesi veya posta parametresi) istekten çıkarılır. Bu meta veriler, kimlik sağlayıcısıyla paylaşılan Azure AD B2C teknik profilinin meta verilerinde çıktı olan **authnRequestsSigned** özniteliğini de denetler. Azure AD B2C, teknik profil meta verilerindeki **WantsSignedRequests'in** değeri ayarlanırsa `false` ve kimlik sağlayıcısı **WantAuthnRequestsSigned'in** belirlediği `false` veya belirtilmemiş olması durumunda isteği imzalamaz. |
| XmlSignatureAlgoritma | Hayır | Azure AD B2C'nin SAML isteğini imzalamak için kullandığı yöntem. Bu meta veri, SAML isteğindeki **SigAlg** parametresinin (sorgu dizesi veya posta parametresi) değerini denetler. Olası `Sha256`değerler: `Sha384` `Sha512`, `Sha1`, , veya . İmza algoritmasını her iki tarafta aynı değerde yapılandırdığınızdan emin olun. Yalnızca sertifikanızın desteklediği algoritmayı kullanın. |
| WantsSignedİddiaları | Hayır | Teknik profilin tüm gelen iddiaların imzalanmasını gerektirip gerektirmediğini gösterir. Olası `true` değerler: `false`veya . Varsayılan değer: `true`. Değer `true`ayarlanmışsa, kimlik sağlayıcısı `saml:Assertion` tarafından Azure AD B2C'ye gönderilen tüm iddialar bölümünün imzalanması gerekir. Değer `false`ayarlanmışsa, kimlik sağlayıcısı iddiaları imzalamamalıdır, ancak imzayı doğrulamaz. Bu meta veriler, kimlik sağlayıcısıyla paylaşılan Azure AD B2C teknik profilinin meta verilerinde çıktı olan **WantsAssertionsSigned**meta veri bayrağını da denetler. İddiadoğrulamayı devre dışı bilebilirseniz, yanıt imzası doğrulaması da devre dışı kalmak isteyebilirsiniz (daha fazla bilgi için **Yanıtlarİmzalandı).** |
| İmzalanan Yanıtlar | Hayır | Olası `true` değerler: `false`veya . Varsayılan değer: `true`. Değer `false`ayarlanmışsa, kimlik sağlayıcısı SAML yanıtını imzalamamalıdır, ancak imzayı doğrulamaz. Değer `true`ayarlanmışsa, kimlik sağlayıcısı tarafından Azure AD B2C'ye gönderilen SAML yanıtı imzalanır ve doğrulanması gerekir. SAML yanıt doğrulaması devre dışı kalırsanız, ayrıca sebebe imza doğrulaması devre dışı kalmak isteyebilirsiniz (daha fazla bilgi için **Bkz. WantsSignedAssertions).** |
| WantsEncryptedAssertions | Hayır | Teknik profilin tüm gelen iddiaların şifrelenmesini gerektirip gerektirmediğini gösterir. Olası `true` değerler: `false`veya . Varsayılan değer: `false`. Değer `true`ayarlanmışsa, kimlik sağlayıcısı tarafından Azure AD B2C'ye gönderilen iddiaların imzalanması ve **SamlAssertionDecryption** şifreleme anahtarının belirtilmesi gerekir. Değer `true`ayarlanmışsa, Azure AD B2C teknik profilinin meta verileri **şifreleme** bölümünü içerir. Kimlik sağlayıcısı meta verileri okur ve SAML yanıt iddiasını Azure AD B2C teknik profilinin meta verilerinde sağlanan ortak anahtarla şifreler. İddiaları şifrelemeyi etkinleştiriseniz, yanıt imzası doğrulaması devre dışı bırakmanız da gerekebilir (daha fazla bilgi için **Yanıtlar İmzalandı).** |
| IdpInitiatedProfileEtkin | Hayır | SAML kimlik sağlayıcısı profili tarafından başlatılan tek bir oturum açma oturumu profilinin etkinolup olmadığını gösterir. Olası `true` değerler: `false`veya . Varsayılan değer: `false`. Kimlik sağlayıcısı tarafından başlatılan akışta, kullanıcı dışarıdan kimlik doğrulanır ve istenmeyen bir yanıt Azure AD B2C'ye gönderilir ve bu yanıt daha sonra belirteci tüketir, düzenleme adımlarını yürütür ve ardından güvenilen taraf uygulamasına yanıt gönderir. |
| NameidPolicyFormat | Hayır | İstenen özneyi temsil etmek için kullanılacak ad tanımlayıcısı üzerindeki kısıtlamaları belirtir. Atlanırsa, istenen konu için kimlik sağlayıcısı tarafından desteklenen herhangi bir tanımlayıcı türü kullanılabilir. Örneğin, `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`. **NameIdPolicyFormat** **NameIdPolicyAllowCreate**ile kullanılabilir. Kimlik kimlik ilkelerinin hangi adla desteklendiğine ilişkin rehberlik için kimlik sağlayıcınızın belgelerine bakın. |
| NameidpolicyAllowCreate | Hayır | **NameIdPolicyFormat**kullanırken, `AllowCreate` **nameidpolicy**özelliğini de belirtebilirsiniz. Bu meta verilerin değeri `true` `false` veya kimlik sağlayıcısının oturum açma akışı sırasında yeni bir hesap oluşturmasına izin verilip verilmediğini belirtmek içindir. Bunu nasıl yapacağınıza ilişkin rehberlik için kimlik sağlayıcınızın belgelerine bakın. |
| Kimlik Doğrulamaİstek Uzantıları | Hayır | Azure AD BC ile kimlik sağlayıcısı arasında üzerinde anlaşmaya varılan isteğe bağlı iletişim kuralı iletisi uzantısı öğeleri. Uzantı XML formatında sunulur. XML verilerini CDATA öğesinin `<![CDATA[Your IDP metadata]]>`içine eklersiniz. Uzantılar öğesinin desteklenip desteklenmediğinizi görmek için kimlik sağlayıcınızın belgelerini denetleyin. |
| IncludeAuthnContextClassReferences | Hayır | Kimlik doğrulama bağlamı sınıflarını tanımlayan bir veya daha fazla URI başvurularını belirtir. Örneğin, bir kullanıcının yalnızca kullanıcı adı ve parolayla oturum `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`açmasına izin vermek için değeri . Korumalı bir oturum (SSL/TLS) üzerinden oturum açmaya `PasswordProtectedTransport`izin vermek için. Desteklenen **AuthnContextClassRef** URI'ler hakkında rehberlik için kimlik sağlayıcınızın belgelerine bakın. Virgülle sınırlandırılmış liste olarak birden çok URIs belirtin. |
| IncludeKeyInfo | Hayır | SAML kimlik doğrulama isteğinin, bağlama ayarlandığında sertifikanın ortak `HTTP-POST`anahtarını içerip içerdirip içerme Olası `true` değerler: `false`veya . |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıktı talepleri [için, talep çözümlübir](claim-resolver-overview.md) çözümünü teknik profilde bulunup bulunmayacağını belirtir. Olası değerler: `true` `false`  , veya (varsayılan). Teknik profilde bir talep çözümleyicisi kullanmak istiyorsanız, bunu ' ya `true`göre ayarlayın. |

## <a name="cryptographic-keys"></a>Şifreleme tuşları

**CryptographicKeys** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- | ----------- |
| SamlMessageSigning |Evet | SAML iletilerini imzalamak için kullanılacak X509 sertifikası (RSA anahtar kümesi). Azure AD B2C, istekleri imzalamak ve kimlik sağlayıcısına göndermek için bu anahtarı kullanır. |
| SamlAssertionŞifre Çözme |Evet | SAML iletilerinin şifresini çözmek için kullanılacak X509 sertifikası (RSA anahtar kümesi). Bu sertifika kimlik sağlayıcı tarafından sağlanmalıdır. Azure AD B2C, kimlik sağlayıcısı tarafından gönderilen verilerin şifresini çözmek için bu sertifikayı kullanır. |
| Meta dataSigning |Hayır | SAML meta verilerini imzalamak için kullanılacak X509 sertifikası (RSA anahtar kümesi). Azure AD B2C meta verileri imzalamak için bu anahtarı kullanır.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C'deki SAML kimlik sağlayıcılarıyla çalışma örnekleri için aşağıdaki makalelere bakın:

- [Özel ilkeler kullanarak ADFS'yi SAML kimlik sağlayıcısı olarak ekleme](identity-provider-adfs2016-custom.md)
- [SAML üzerinden Salesforce hesaplarını kullanarak oturum açın](identity-provider-salesforce-custom.md)
