---
title: REST ile içerik anahtarı yetkilendirme ilkesini yapılandırma - Azure | Microsoft Dokümanlar
description: Media Services REST API'yi kullanarak bir içerik anahtarı için yetkilendirme ilkesini nasıl yapılandırabileceğinizi öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7af5f9e2-8ed8-43f2-843b-580ce8759fd4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 8942ad8bdc4f9fc37a88d09871c983f63cd8c1b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773689"
---
# <a name="dynamic-encryption-configure-a-content-key-authorization-policy"></a>Dinamik şifreleme: İçerik anahtarı yetkilendirme ilkesini yapılandırma  
[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
 128 bit şifreleme anahtarları ve PlayReady veya Widevine dijital haklar yönetimi (DRM) kullanarak Gelişmiş Şifreleme Standardı (AES) ile içeriğinizi şifreli (dinamik olarak) sunmak için Azure Medya Hizmetleri'ni kullanabilirsiniz. Medya Hizmetleri ayrıca yetkili müşterilere anahtar ve PlayReady/Widevine lisansları teslim etmek için bir hizmet de sunmaktadır.

Medya Hizmetleri'nin bir varlığı şifrelemesini istiyorsanız, bir şifreleme anahtarını (CommonEncryption veya EnvelopeEncryption) varlıkla ilişkilendirmeniz gerekir. Daha fazla bilgi için [bkz.](media-services-rest-create-contentkey.md) Ayrıca anahtar için yetkilendirme ilkelerini yapılandırmanız gerekir (bu makalede açıklandığı gibi).

Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri AES veya PlayReady şifrelemesi kullanarak içeriğinizi dinamik olarak şifrelemek için belirtilen anahtarı kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarı almaya yetkili olup olmadığını belirlemek için, hizmet anahtar için belirlediğiniz yetkilendirme ilkelerini değerlendirir.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesi, açık veya belirteç kısıtlamasını kullanarak bir veya daha fazla yetkilendirme kısıtlamasına sahip olabilir. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Medya Hizmetleri, basit web belirteci[(SWT)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)ve JSON Web Belirteci (JWT) biçimlerindeki belirteçleri destekler.

Medya Hizmetleri STS sağlamaz. Özel bir STS oluşturabilir veya belirteçleri vermek için Azure Etkin Dizini (Azure AD) kullanabilirsiniz. STS belirtilmiş anahtar ve sorun iddiaları belirtdiğiniz belirtilmiş bir belirteç oluşturmak için yapılandırılmalıdır (bu makalede açıklandığı gibi). Belirteç geçerliyse ve belirteçteki talepler içerik anahtarı için yapılandırılanlarla eşleşirse, Medya Hizmetleri anahtar teslim hizmeti şifreleme anahtarını istemciye döndürür.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [JWT belirteç kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
- [Azure Medya Hizmetleri OWIN MVC tabanlı bir uygulamayı Azure Active Directory ile tümleştirin ve JWT iddialarına göre içerik anahtarı teslimini kısıtlayın](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

### <a name="some-considerations-apply"></a>Bazı hususlar geçerlidir
* Dinamik paketleme ve dinamik şifreleme kullanmak için, içeriğinizi aktarmak istediğiniz akış bitiş noktasının "Çalışan" durumunda olduğundan emin olun.
* Kıymetiniz bir dizi uyarlanabilir bitrate MP4 veya uyarlanabilir bithızı Düzgün Akış dosyaları içermelidir. Daha fazla bilgi için [bkz.](media-services-encode-asset.md)
* AssetCreationOptions.StorageEncrypted seçeneğini kullanarak varlıklarınızı yükleyin ve kodlayın.
* Aynı ilke yapılandırmasını gerektiren birden çok içerik anahtarına sahip olmayı planlıyorsanız, tek bir yetkilendirme ilkesi oluşturmanızı ve birden çok içerik anahtarıyla yeniden kullanmanızı öneririz.
* Anahtar teslim hizmeti ContentKeyAuthorizationPolicy ve ilgili nesneleri (ilke seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alınr. ContentKeyAuthorizationPolicy oluşturabilir ve belirterek belirterek belirterek belirterek belirtebilir, test edebilir ve sonra açık kısıtlamayla ilkeyi güncelleştirebilirsiniz. Bu işlem, ilkeğin ilkenin açık sürümüne geçmeden önce yaklaşık 15 dakika sürer.
* Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, varsa mevcut bulucuyu silip yeni bir bulucu oluşturmanız gerekir.
* Şu anda, aşamalı indirmeleri şifreleyebilirsiniz.
* Medya Hizmetleri akışı bitiş noktası, ön kontrol yanıtında CORS Access-Control-Allow-Origin üstbilgisinin değerini joker karakter olarak\*ayarlar. Bu değer, Azure Media Player, Roku ve JWPlayer ve diğerleri dahil olmak üzere çoğu oyuncuda iyi çalışır. Ancak, dash.js kullanan bazı oyuncular çalışmaz, çünkü kimlik bilgileri modu "dahil etmek" için ayarlanmış, XMLHttpRequest kendi\*dash.js joker " " " Erişim-Denetim-İzin-Origin değeri olarak izin vermez. Dash.js'deki bu sınırlamaya geçici çözüm olarak, istemcinizi tek bir etki alanından barındırıyorsanız, Medya Hizmetleri bu etki alanını ön uçuş yanıtı üstbilgisinde belirtebilir. Yardım için Azure portalı üzerinden bir destek bileti açın.

## <a name="aes-128-dynamic-encryption"></a>AES-128 dinamik şifreleme
> [!NOTE]
> Medya Hizmetleri REST API ile çalışırken, aşağıdaki hususlar geçerlidir.
> 
> Medya Hizmetleri'ndeki varlıklara erişdiğinizde, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri ayarlamanız gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API geliştirme kurulumuna](media-services-rest-how-to-use.md)bakın.
> 
> 
> 

### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama, sistemin anahtarı önemli bir istekte bulunan herkese teslim etmesi anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, açık bir yetkilendirme ilkesi oluşturur ve içerik anahtarına ekler.

#### <a name="create-contentkeyauthorizationpolicies"></a><a id="ContentKeyAuthorizationPolicies"></a>İçerik OluşturmaKeyAuthorizationPolicies
İstek:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: d732dbfa-54fc-474c-99d6-9b46a006f389
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 36

    {"Name":"Open Authorization Policy"}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 211
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3Adb4593da-f4d1-4cc5-a92a-d20eacbabee4')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d732dbfa-54fc-474c-99d6-9b46a006f389
    request-id: aabfa731-e884-4bf3-8314-492b04747ac4
    x-ms-request-id: aabfa731-e884-4bf3-8314-492b04747ac4
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 08:25:56 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicies/@Element","Id":"nb:ckpid:UUID:db4593da-f4d1-4cc5-a92a-d20eacbabee4","Name":"Open Authorization Policy"}

#### <a name="create-contentkeyauthorizationpolicyoptions"></a><a id="ContentKeyAuthorizationPolicyOptions"></a>İçerik OluşturmaKeyAuthorizationPolicyOptions
İstek:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: d225e357-e60e-4f42-add8-9d93aba1409a
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 168

    {"Name":"policy","KeyDeliveryType":2,"KeyDeliveryConfiguration":"","Restrictions":[{"Name":"HLS Open Authorization Policy","KeyRestrictionType":0,"Requirements":null}]}

Yanıt:    

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 349
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A57829b17-1101-4797-919b-f816f4a007b7')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d225e357-e60e-4f42-add8-9d93aba1409a
    request-id: 81bcad37-295b-431f-972f-b23f2e4172c9
    x-ms-request-id: 81bcad37-295b-431f-972f-b23f2e4172c9
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 08:56:40 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:57829b17-1101-4797-919b-f816f4a007b7","Name":"policy","KeyDeliveryType":2,"KeyDeliveryConfiguration":"","Restrictions":[{"Name":"HLS Open Authorization Policy","KeyRestrictionType":0,"Requirements":null}]}

#### <a name="link-contentkeyauthorizationpolicies-with-options"></a><a id="LinkContentKeyAuthorizationPoliciesWithOptions"></a>Link İçerikKeyAuthorizationPolicies seçenekleri ile
İstek:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3A0baa438b-8ac2-4c40-a53c-4d4722b78715')/$links/Options HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 9847f705-f2ca-4e95-a478-8f823dbbaa29
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 154

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A57829b17-1101-4797-919b-f816f4a007b7')"}

Yanıt:

    HTTP/1.1 204 No Content

#### <a name="add-an-authorization-policy-to-the-content-key"></a><a id="AddAuthorizationPolicyToKey"></a>İçerik anahtarına yetkilendirme ilkesi ekleme
İstek:

    PUT https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A2e6d36a7-a17c-4e9a-830d-eca23ad1a6f9') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: e613efff-cb6a-41b4-984a-f4f8fb6e76a4
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 78

    {"AuthorizationPolicyId":"nb:ckpid:UUID:c06cebb8-c4f0-4d1a-ba00-3273fb2bc3ad"}

Yanıt:

    HTTP/1.1 204 No Content

### <a name="token-restriction"></a>Belirteç kısıtlaması
Bu bölümde, içerik anahtarı yetkilendirme ilkesinin nasıl oluşturulup içerik anahtarıyla ilişkilendirilen açıklanmaktadır. Yetkilendirme ilkesi, kullanıcının anahtarı almaya yetkili olup olmadığını belirlemek için hangi yetkilendirme gereksinimlerinin karşılanması gerektiğini açıklar. Örneğin, doğrulama anahtar listesi belirteçle imzalandığı anahtarı içeriyor mu?

Belirteç kısıtlama seçeneğini yapılandırmak için, belirteç yetkilendirme gereksinimlerini açıklamak için bir XML kullanmanız gerekir. Belirteç kısıtlama yapılandırması XML aşağıdaki XML şemasına uygun olmalıdır:


#### <a name="token-restriction-schema"></a><a id="schema"></a>Belirteç kısıtlama şeması
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

Belirteç kısıtlamalı ilkeyi yapılandırdığınızda, birincil doğrulama anahtarını, vereni ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı, belirteçle imzalandığı anahtarı içerir. İhraççı, belirteci veren STS'dir. Hedef kitle (bazen kapsam olarak adlandırılır), belirteci veya belirteç erişim eizin kaynağının amacını açıklar. Medya Hizmetleri anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştirdiğini doğrular.

Aşağıdaki örnek, belirteç kısıtlaması içeren bir yetkilendirme ilkesi oluşturur. Bu örnekte, istemci nin imza anahtarını (VerificationKey), bir belirteç vereni ve gerekli talepleri içeren bir belirteci sunması gerekir.

### <a name="create-contentkeyauthorizationpolicies"></a>İçerik OluşturmaKeyAuthorizationPolicies
["ContentKeyAuthorizationPolicies oluştur"](#ContentKeyAuthorizationPolicies)bölümünde gösterildiği gibi bir belirteç kısıtlama ilkesi oluşturun.

### <a name="create-contentkeyauthorizationpolicyoptions"></a>İçerik OluşturmaKeyAuthorizationPolicyOptions
İstek:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    x-ms-client-request-id: 2643d836-bfe7-438e-9ba2-bc6ff28e4a53
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 1079

    {"Name":"Token option for HLS","KeyDeliveryType":2,"KeyDeliveryConfiguration":null,"Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>BklyAFiPTQsuJNKriQJBZHYaKM2CkCTDQX2bw9sMYuvEC9sjW0W7GUIBygQL/+POEeUqCYPnmEU2g0o1GW2Oqg==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testissuer.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>E5BUHiN4vBdzUzdP0IWaHFMMU3D1uRZgF16TOhSfwwHGSw+Kbf0XqsHzEIYk11M372viB9vbiacsdcQksA0ftw==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

Yanıt:    

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1260
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3Ae1ef6145-46e8-4ee6-9756-b1cf96328c23')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 2643d836-bfe7-438e-9ba2-bc6ff28e4a53
    request-id: 2310b716-aeaa-421e-913e-3ce2f6f685ca
    x-ms-request-id: 2310b716-aeaa-421e-913e-3ce2f6f685ca
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:10:37 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:e1ef6145-46e8-4ee6-9756-b1cf96328c23","Name":"Token option for HLS","KeyDeliveryType":2,"KeyDeliveryConfiguration":null,"Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>BklyAFiPTQsuJNKriQJBZHYaKM2CkCTDQX2bw9sMYuvEC9sjW0W7GUIBygQL/+POEeUqCYPnmEU2g0o1GW2Oqg==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testissuer.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>E5BUHiN4vBdzUzdP0IWaHFMMU3D1uRZgF16TOhSfwwHGSw+Kbf0XqsHzEIYk11M372viB9vbiacsdcQksA0ftw==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

#### <a name="link-contentkeyauthorizationpolicies-with-options"></a>Link İçerikKeyAuthorizationPolicies seçenekleri ile
Link ContentKeyAuthorizationPolicies seçenekleri ile, bölümünde gösterildiği gibi "[İçerikKeyAuthorizationPolicies oluşturun](#ContentKeyAuthorizationPolicies)."

#### <a name="add-an-authorization-policy-to-the-content-key"></a>İçerik anahtarına yetkilendirme ilkesi ekleme
ContentKey'e Yetkilendirme Politikası ekle "[İçerik anahtarına yetkilendirme ilkesi ekleyin."](#AddAuthorizationPolicyToKey)bölümünde gösterildiği gibi.

## <a name="playready-dynamic-encryption"></a>PlayReady dinamik şifreleme
Bir kullanıcı korumalı içeriği oynatmaya çalıştığında PlayReady DRM çalışma zamanının uygulanmasını istediğiniz hakları ve kısıtlamaları yapılandırmak için Medya Hizmetlerini kullanabilirsiniz. 

İçeriğinizi PlayReady ile koruduğunuzda, yetkilendirme politikanızda belirtmeniz gereken şeylerden biri [PlayReady lisans şablonunu](media-services-playready-license-template-overview.md)tanımlayan bir XML dizesidir. 

### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama, sistemin anahtarı önemli bir istekte bulunan herkese teslim etmesi anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, açık bir yetkilendirme ilkesi oluşturur ve içerik anahtarına ekler.

#### <a name="create-contentkeyauthorizationpolicies"></a><a id="ContentKeyAuthorizationPolicies2"></a>İçerik OluşturmaKeyAuthorizationPolicies
İstek:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 9e7fa407-f84e-43aa-8f05-9790b46e279b
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 58

    {"Name":"Deliver Common Content Key"}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 233
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3Acc3c64a8-e2fc-4e09-bf60-ac954251a387')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 9e7fa407-f84e-43aa-8f05-9790b46e279b
    request-id: b3d33c1b-a9cb-4120-ac0c-18f64846c147
    x-ms-request-id: b3d33c1b-a9cb-4120-ac0c-18f64846c147
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:26:00 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicies/@Element","Id":"nb:ckpid:UUID:cc3c64a8-e2fc-4e09-bf60-ac954251a387","Name":"Deliver Common Content Key"}


#### <a name="create-contentkeyauthorizationpolicyoptions"></a>İçerik OluşturmaKeyAuthorizationPolicyOptions
İstek:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: f160ad25-b457-4bc6-8197-315604c5e585
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 593

    {"Name":"","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Open","KeyRestrictionType":0,"Requirements":null}]}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 774
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A1052308c-4df7-4fdb-8d21-4d2141fc2be0')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: f160ad25-b457-4bc6-8197-315604c5e585
    request-id: 563f5a42-50a4-4c4a-add8-a833f8364231
    x-ms-request-id: 563f5a42-50a4-4c4a-add8-a833f8364231
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:23:24 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:1052308c-4df7-4fdb-8d21-4d2141fc2be0","Name":"","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Open","KeyRestrictionType":0,"Requirements":null}]}

#### <a name="link-contentkeyauthorizationpolicies-with-options"></a>Link İçerikKeyAuthorizationPolicies seçenekleri ile
Link ContentKeyAuthorizationPolicies seçenekleri ile, bölümünde gösterildiği gibi "[İçerikKeyAuthorizationPolicies oluşturun](#ContentKeyAuthorizationPolicies)."

#### <a name="add-an-authorization-policy-to-the-content-key"></a>İçerik anahtarına yetkilendirme ilkesi ekleme
ContentKey'e Yetkilendirme Politikası ekle "[İçerik anahtarına yetkilendirme ilkesi ekleyin."](#AddAuthorizationPolicyToKey)bölümünde gösterildiği gibi.

### <a name="token-restriction"></a>Belirteç kısıtlaması
Belirteç kısıtlama seçeneğini yapılandırmak için, belirteç yetkilendirme gereksinimlerini açıklamak için bir XML kullanmanız gerekir. XML belirteç kısıtlama yapılandırması "[Belirteç kısıtlama şeması](#schema)" bölümünde gösterilen XML şemasına uygun olmalıdır."

#### <a name="create-contentkeyauthorizationpolicies"></a>İçerik OluşturmaKeyAuthorizationPolicies
ContentKeyAuthorizationPolicies oluşturun, bölümünde gösterildiği gibi "[İçerik OluşturmaKeyAuthorizationPolicies](#ContentKeyAuthorizationPolicies2)."

#### <a name="create-contentkeyauthorizationpolicyoptions"></a>İçerik OluşturmaKeyAuthorizationPolicyOptions
İstek:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: ab079b0e-2ba9-4cf1-b549-a97bfa6cd2d3
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 1525

    {"Name":"Token option","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>w52OyHVqXT8aaupGxuJ3NGt8M6opHDOtx132p4r6q4hLI6ffnLusgEGie1kedUewVoIe1tqDkVE6xsIV7O91KA==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testissuer.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>dYwLKIEMBljLeY9VM7vWdlhps31Fbt0XXhqP5VyjQa33bJXleBtkzQ6dF5AtwI9gDcdM2dV2TvYNhCilBKjMCg==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1706
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3Ae42bbeae-de42-4077-90e9-a844f297ef70')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: ab079b0e-2ba9-4cf1-b549-a97bfa6cd2d3
    request-id: ccf8a4ba-731e-4124-8192-079592c251cc
    x-ms-request-id: ccf8a4ba-731e-4124-8192-079592c251cc
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:58:47 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:e42bbeae-de42-4077-90e9-a844f297ef70","Name":"Token option","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"https://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>w52OyHVqXT8aaupGxuJ3NGt8M6opHDOtx132p4r6q4hLI6ffnLusgEGie1kedUewVoIe1tqDkVE6xsIV7O91KA==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testissuer.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>dYwLKIEMBljLeY9VM7vWdlhps31Fbt0XXhqP5VyjQa33bJXleBtkzQ6dF5AtwI9gDcdM2dV2TvYNhCilBKjMCg==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

#### <a name="link-contentkeyauthorizationpolicies-with-options"></a>Link İçerikKeyAuthorizationPolicies seçenekleri ile
Link ContentKeyAuthorizationPolicies seçenekleri ile, bölümünde gösterildiği gibi "[İçerikKeyAuthorizationPolicies oluşturun](#ContentKeyAuthorizationPolicies)."

#### <a name="add-an-authorization-policy-to-the-content-key"></a>İçerik anahtarına yetkilendirme ilkesi ekleme
ContentKey'e Yetkilendirme Politikası ekle "[İçerik anahtarına yetkilendirme ilkesi ekleyin."](#AddAuthorizationPolicyToKey)bölümünde gösterildiği gibi.

## <a name="types-used-when-you-define-contentkeyauthorizationpolicy"></a><a id="types"></a>ContentKeyAuthorizationPolicy'yi tanımlarken kullanılan türler
### <a name="contentkeyrestrictiontype"></a><a id="ContentKeyRestrictionType"></a>İçerikKeyRestrictionType
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1, 
        IPRestricted = 2, // IP restriction on content key is not currently supported, reserved for future.
    }


> [!NOTE]
> İçerik anahtarı yetkilendirme ilkelerine ilişkin IP kısıtlaması henüz hizmette mevcut değildir.


### <a name="contentkeydeliverytype"></a><a id="ContentKeyDeliveryType"></a>İçerikKeyDeliveryType
    public enum ContentKeyDeliveryType
    {
        None = 0,
        PlayReadyLicense = 1,
        BaselineHttp = 2,
        Widevine = 3
    }

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık bir içerik anahtarının yetkilendirme ilkesini yapılandırdığınıza göre, [bkz.](media-services-rest-configure-asset-delivery-policy.md)

