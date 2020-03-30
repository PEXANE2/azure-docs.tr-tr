---
title: Medya Hizmetleri .NET SDK kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma | Microsoft Dokümanlar
description: Medya Hizmetleri .NET SDK'yı kullanarak bir içerik anahtarı için yetkilendirme ilkesini nasıl yapılandırabileceğinizi öğrenin.
services: media-services
documentationcenter: ''
author: mingfeiy
manager: femila
editor: ''
ms.assetid: 1a0aedda-5b87-4436-8193-09fc2f14310c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 58d52cd194ca4391c61f2477189984273df1198a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251212"
---
# <a name="configure-a-content-key-authorization-policy"></a>İçerik anahtarı yetkilendirme ilkesini yapılandırma

[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
 128 bit şifreleme anahtarları veya [PlayReady dijital haklar yönetimi (DRM)](https://www.microsoft.com/playready/overview/)kullanarak Gelişmiş Şifreleme Standardı (AES) ile korunan MPEG-DASH, Sorunsuz Akış ve HTTP Canlı Akış (HLS) akışlarını sunmak için Azure Medya Hizmetlerini kullanabilirsiniz. Medya Hizmetleri ile Widevine DRM ile şifrelenmiş DASH akışlarını da teslim edebilirsiniz. PlayReady ve Widevine, ortak şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir.

Medya Hizmetleri ayrıca, müşterilerin şifrelenmiş içeriği oynatmak için AES anahtarları veya PlayReady/Widevine lisansları alabilecekleri bir anahtar/lisans teslim hizmeti de sağlar.

Medya Hizmetleri'nin bir varlığı şifrelemesini istiyorsanız, bir şifreleme anahtarını (CommonEncryption veya EnvelopeEncryption) varlıkla ilişkilendirmeniz gerekir. Daha fazla bilgi için [bkz.](media-services-dotnet-create-contentkey.md) Ayrıca anahtar için yetkilendirme ilkelerini yapılandırmanız gerekir (bu makalede açıklandığı gibi).

Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri AES veya DRM şifrelemekullanarak içeriğinizi dinamik olarak şifrelemek için belirtilen anahtarı kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarı almaya yetkili olup olmadığını belirlemek için, hizmet anahtar için belirlediğiniz yetkilendirme ilkelerini değerlendirir.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinde bir veya daha fazla yetkilendirme kısıtlaması olabilir. Seçenekler açık veya belirteç kısıtlamasI dır. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Medya Hizmetleri, basit web belirteci[(SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) biçimindeki belirteçleri ve JSON Web Belirteci[(JWT)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)biçimini destekler.

Medya Hizmetleri STS sağlamaz. Belirteçleri vermek için özel bir STS oluşturabilir veya Azure Erişim Denetim Hizmeti'ni kullanabilirsiniz. STS belirtilmiş anahtar ve sorun iddiaları belirtdiğiniz belirtilmiş bir belirteç oluşturmak için yapılandırılmalıdır (bu makalede açıklandığı gibi). Belirteç geçerliyse ve belirteçteki talepler içerik anahtarı için yapılandırılanlarla eşleşirse, Medya Hizmetleri anahtar teslim hizmeti şifreleme anahtarını istemciye döndürür.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [JWT belirteç kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
- [Azure Medya Hizmetleri OWIN MVC tabanlı uygulamayı Azure Active Directory ile tümleştirin ve JWT iddialarına göre içerik anahtarı teslimini kısıtlayın](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

### <a name="some-considerations-apply"></a>Bazı hususlar geçerlidir
* Media Services hesabınız oluşturulduğunda hesabınıza “Durdurulmuş” durumda bir varsayılan akış uç noktası eklenir. İçeriğinizi akışa başlamak ve dinamik paketleme ve dinamik şifrelemeden yararlanmak için akış bitiş noktanızın "Çalışan" durumunda olması gerekir. 
* Kıymetiniz bir dizi uyarlanabilir bitrate MP4 veya uyarlanabilir bithızı Düzgün Akış dosyaları içermelidir. Daha fazla bilgi için [bkz.](media-services-encode-asset.md)
* AssetCreationOptions.StorageEncrypted seçeneğini kullanarak varlıklarınızı yükleyin ve kodlayın.
* Aynı ilke yapılandırmasını gerektiren birden çok içerik anahtarına sahip olmayı planlıyorsanız, tek bir yetkilendirme ilkesi oluşturmanızı ve birden çok içerik anahtarıyla yeniden kullanmanızı öneririz.
* Anahtar teslim hizmeti ContentKeyAuthorizationPolicy ve ilgili nesneleri (ilke seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alınr. ContentKeyAuthorizationPolicy oluşturabilir ve belirterek belirterek belirterek belirterek belirtebilir, test edebilir ve sonra açık kısıtlamayla ilkeyi güncelleştirebilirsiniz. Bu işlem, ilkeğin ilkenin açık sürümüne geçmeden önce yaklaşık 15 dakika sürer.
* Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, varsa mevcut bulucuyu silip yeni bir bulucu oluşturmanız gerekir.
* Şu anda, aşamalı indirmeleri şifreleyebilirsiniz.
* Bir Medya Hizmetleri akışı bitiş noktası, CORS'in 'Access-Control-Allow-Origin' üstbilgisinin ön\*kontrol yanıtındaki değerini joker karakter ' olarak ayarlar. Bu değer, Azure Media Player, Roku ve JWPlayer ve diğerleri dahil olmak üzere çoğu oyuncuda iyi çalışır. Ancak, dashjkullanan bazı oyuncular çalışmaz, çünkü kimlik bilgileri modu "dahil" olarak ayarlanmışsa, dashj'lerindeki XMLHttpRequest joker "Access-Control-Allow-Origin" değeri olarak joker ""\*olarak izin vermez. Dashjs bu sınırlama için bir geçici çözüm olarak, tek bir etki alanından istemci barındırmak, Medya Hizmetleri ön uçuş yanıt üstbilgisinde bu etki alanı belirtebilirsiniz. Yardım için Azure portalı üzerinden bir destek bileti açın.

## <a name="aes-128-dynamic-encryption"></a>AES-128 dinamik şifreleme
### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama, sistemin anahtarı önemli bir istekte bulunan herkese teslim etmesi anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, açık bir yetkilendirme ilkesi oluşturur ve içerik anahtarına ekler:
```csharp
    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
        // Create ContentKeyAuthorizationPolicy with Open restrictions
        // and create authorization policy
        IContentKeyAuthorizationPolicy policy = _context.
        ContentKeyAuthorizationPolicies.
        CreateAsync("Open Authorization Policy").Result;
        
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };

        restrictions.Add(restriction);

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");

        policy.Options.Add(policyOption);

        // Add ContentKeyAuthorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }
```

### <a name="token-restriction"></a>Belirteç kısıtlaması
Bu bölümde, içerik anahtarı yetkilendirme ilkesinin nasıl oluşturulup içerik anahtarıyla ilişkilendirilen açıklanmaktadır. Yetkilendirme ilkesi, kullanıcının anahtarı almaya yetkili olup olmadığını belirlemek için hangi yetkilendirme gereksinimlerinin karşılanması gerektiğini açıklar. Örneğin, doğrulama anahtar listesi belirteçle imzalandığı anahtarı içeriyor mu?

Belirteç kısıtlama seçeneğini yapılandırmak için, belirteç yetkilendirme gereksinimlerini açıklamak için bir XML kullanmanız gerekir. Belirteç kısıtlama yapılandırması XML aşağıdaki XML şemasına uygun olmalıdır:
```csharp
#### Token restriction schema
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="https://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>
```
Belirteç kısıtlamalı ilkeyi yapılandırdığınızda, birincil doğrulama anahtarını, vereni ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı, belirteçle imzalandığı anahtarı içerir. İhraççı, belirteci veren STS'dir. Hedef kitle (bazen kapsam olarak adlandırılır), belirteci veya belirteç erişim eizin kaynağının amacını açıklar. Medya Hizmetleri anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştirdiğini doğrular.

.NET için Medya Hizmetleri SDK'sını kullandığınızda, kısıtlama belirteci oluşturmak için TokenRestrictionTemplate sınıfını kullanabilirsiniz.
Aşağıdaki örnek, belirteç kısıtlaması içeren bir yetkilendirme ilkesi oluşturur. Bu örnekte, istemci nin bir imza anahtarı (Doğrulama Anahtarı), bir belirteç veren ve gerekli talepleri içeren bir belirteci sunması gerekir.
```csharp
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };

        restrictions.Add(restriction);

        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );

        policy.Options.Add(policyOption);

        // Add ContentKeyAuthorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    }
```
#### <a name="test-token"></a>Test jetonu
Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı bir test belirteci almak için aşağıdakileri yapın:
```csharp
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();
```

## <a name="playready-dynamic-encryption"></a>PlayReady dinamik şifreleme
Bir kullanıcı korumalı içeriği oynatmaya çalıştığında PlayReady DRM çalışma zamanının uygulanmasını istediğiniz hakları ve kısıtlamaları yapılandırmak için Medya Hizmetlerini kullanabilirsiniz. 

İçeriğinizi PlayReady ile koruduğunuzda, yetkilendirme politikanızda belirtmeniz gereken şeylerden biri [PlayReady lisans şablonunu](media-services-playready-license-template-overview.md)tanımlayan bir XML dizesidir. .NET için Medya Hizmetleri SDK'sında, PlayReadyLicenseResponseTemplate ve PlayReadyLicenseTemplate sınıfları PlayReady lisans şablonu tanımlamanıza yardımcı olur.

İçeriğinizi PlayReady ve Widevine ile nasıl şifreleyeceklerinizi öğrenmek için [PlayReady ve/veya Widevine dinamik ortak şifrelemesini kullan'a](media-services-protect-with-playready-widevine.md)bakın.

### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama, sistemin anahtarı önemli bir istekte bulunan herkese teslim etmesi anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, açık bir yetkilendirme ilkesi oluşturur ve içerik anahtarına ekler:

```csharp
    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {

        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;


        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }
```

### <a name="token-restriction"></a>Belirteç kısıtlaması
Belirteç kısıtlama seçeneğini yapılandırmak için, belirteç yetkilendirme gereksinimlerini açıklamak için bir XML kullanmanız gerekir. XML belirteç kısıtlama yapılandırması, "Belirteç kısıtlama şeması" bölümünde gösterilen XML şemasına uygun olmalıdır.

```csharp
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;

        policy.Options.Add(policyOption);

        // Add ContentKeyAuthorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {

        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();


        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 

    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to play back the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }
```

Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı bir test belirteci almak için "[Test belirteci](#test-token)" bölümüne bakın. 

## <a name="types-used-when-you-define-contentkeyauthorizationpolicy"></a><a id="types"></a>ContentKeyAuthorizationPolicy'yi tanımlarken kullanılan türler
### <a name="contentkeyrestrictiontype"></a><a id="ContentKeyRestrictionType"></a>İçerikKeyRestrictionType

```csharp
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }
```

### <a name="contentkeydeliverytype"></a><a id="ContentKeyDeliveryType"></a>İçerikKeyDeliveryType

```csharp 
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }
```

### <a name="tokentype"></a><a id="TokenType"></a>Tokentype

```csharp
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }
```

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık içerik anahtarının yetkilendirme ilkesini yapılandırdığınıza [göre,](media-services-dotnet-configure-asset-delivery-policy.md)bkz.

