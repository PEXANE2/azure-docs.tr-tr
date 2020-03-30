---
title: SAML Protokolü'nde Azure Tek İmza | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizini'nde SAML protokolünde Tek Oturum
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: ad8437f5-b887-41ff-bd77-779ddafc33fb
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/19/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: cecb78a82eb2925813bdc7f6df2503fae94b6437
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262431"
---
# <a name="single-sign-on-saml-protocol"></a>Tek Oturum Açma SAML protokolü

Bu makale, Azure Etkin Dizini'nin (Azure AD) Tek Oturum Açma için desteklediği SAML 2.0 kimlik doğrulama isteklerini ve yanıtlarını kapsar.

Aşağıdaki protokol diyagramı tek oturum açma sırasını açıklar. Bulut hizmeti (hizmet sağlayıcısı), bir `AuthnRequest` (kimlik doğrulama isteği) öğesini Azure AD'ye (kimlik sağlayıcısı) geçirmek için bir HTTP Yeniden Yönlendirme bağlayıcısı kullanır. Azure AD daha sonra bir `Response` öğeyi bulut hizmetine göndermek için bir HTTP gönderisi kullanır.

![İş Akışında Tek İşaret](./media/single-sign-on-saml-protocol/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Bulut hizmetleri, kullanıcı kimlik doğrulaması `AuthnRequest` istemek için Azure AD'ye bir öğe gönderir. Bir örnek SAML `AuthnRequest` 2.0 aşağıdaki örnek gibi görünebilir:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kimlik | Gerekli | Azure AD, döndürülen yanıtın `InResponseTo` özniteliğini doldurmak için bu özelliği kullanır. Kimlik bir sayıyla başlamamalıdır, bu nedenle ortak bir strateji guid dize temsiline "id" gibi bir dize hazırlamaktır. Örneğin, `id6c1c178c166d486687be4aaf5e482730` geçerli bir kimliktir. |
| Sürüm | Gerekli | Bu parametre **2.0**olarak ayarlanmalıdır. |
| IssueInstant | Gerekli | Bu, UTC değeri ve [gidiş-dönüş biçimi ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx)olan bir DateTime dizesidir. Azure AD bu tür bir DateTime değeri bekler, ancak değeri değerlendirmez veya kullanmaz. |
| İddiaTüketiciServiceUrl | İsteğe bağlı | Sağlandığı takdirde, bu parametrenin Azure AD'deki bulut `RedirectUri` hizmetiyle eşleşmesi gerekir. |
| ForceAuthn | İsteğe bağlı | Bu bir boolean değeridir. Doğruysa, Azure AD ile geçerli bir oturumu olsa bile kullanıcının yeniden kimlik doğrulaması yapmak zorunda olacağı anlamına gelir. |
| IsPassive | İsteğe bağlı | Bu, Azure AD'nin varsa oturum çerezini kullanarak kullanıcı etkileşimi olmadan kullanıcının kimliğini sessizce doğrulayıp doğrulamayacağını belirten bir boolean değeridir. Bu doğruysa, Azure AD oturum çerezini kullanarak kullanıcının kimliğini doğrulamayı dener. |

Consent, `AuthnRequest` Destination, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex ve ProviderName gibi diğer tüm öznitelikler **yoksayılır.**

Azure AD da `Conditions` öğeyi `AuthnRequest`yoksalan .

### <a name="issuer"></a>Veren

Bir `Issuer` `AuthnRequest` öğedeki öğe, Azure AD'deki bulut hizmetindeki **ServicePrincipalNames'ten** biriyle tam olarak eşleşmelidir. Genellikle, bu uygulama kaydı sırasında belirtilen **App ID URI** olarak ayarlanır.

Öğeyi `Issuer` içeren bir SAML alıntısı aşağıdaki örneğe benzer:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolitikası

Bu öğe yanıtta belirli bir ad kimliği `AuthnRequest` biçimi ister ve Azure AD'ye gönderilen öğelerde isteğe bağlıdır.

Bir `NameIdPolicy` öğe aşağıdaki örneğe benzer:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

`NameIDPolicy` Sağlanırsa, isteğe bağlı `Format` özniteliğini ekleyebilirsiniz. Öznitelik `Format` aşağıdaki değerlerden yalnızca birine sahip olabilir; başka bir değer bir hata yla sonuçlanır.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory, NameID iddiasını çift eşli tanımlayıcı olarak yayınlar.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory, NameID iddiasını e-posta adresi biçiminde yayınlar.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Bu değer, Azure Active Directory'nin talep biçimini seçmesine izin verir. Azure Etkin Dizin, NameID'yi çift eşli tanımlayıcı olarak yayınlar.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: Azure Etkin Dizini, NameID iddiasını geçerli SSO işlemine özgü rasgele oluşturulmuş bir değer olarak yayınlar. Bu, değerin geçici olduğu ve kimlik doğrulayan kullanıcıyı tanımlamak için kullanılamayacağı anlamına gelir.

Azure AD özniteliği yoksayılsa. `AllowCreate`

### <a name="requestauthncontext"></a>İstekAuthnContext
Öğe, `RequestedAuthnContext` istenen kimlik doğrulama yöntemlerini belirtir. Azure AD'ye gönderilen öğelerde `AuthnRequest` isteğe bağlıdır. Azure AD `AuthnContextClassRef` gibi `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`değerleri destekler.

### <a name="scoping"></a>Kapsam
Kimlik `Scoping` sağlayıcılarının listesini içeren öğe, Azure `AuthnRequest` AD'ye gönderilen öğelerde isteğe bağlıdır.

Sağlandığı takdirde, desteklenmedikleri için `IDPListOption` `RequesterID` `ProxyCount` özniteliği veya öğeyi eklemeyin.

### <a name="signature"></a>İmza
Azure AD imzalı `Signature` kimlik `AuthnRequest` doğrulama isteklerini desteklemediği için öğelere bir öğe eklemeyin.

### <a name="subject"></a>Özne
Azure AD öğelerin `Subject` `AuthnRequest` öğesini yoksayılsa.

## <a name="response"></a>Yanıt
İstenen bir oturum açma başarıyla tamamlandığında, Azure AD bulut hizmetine bir yanıt gönderir. Başarılı bir oturum açma girişimine verilen yanıt aşağıdaki örneğe benzer:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Yanıt

Öğe, `Response` yetkilendirme isteğinin sonucunu içerir. Azure AD `ID` `Response` öğedeki `IssueInstant` ve `Version` değerleri ayarlar. Ayrıca aşağıdaki öznitelikleri ayarlar:

* `Destination`: Oturum açma başarıyla tamamlandığında, bu `RedirectUri` işlem servis sağlayıcısının (bulut hizmeti) ayarlanır.
* `InResponseTo`: Bu, yanıtı `ID` başlatan öğenin `AuthnRequest` özniteliğine ayarlanır.

### <a name="issuer"></a>Veren

Azure AD, `Issuer` öğeyi TenantIDGUID `https://login.microsoftonline.com/<TenantIDGUID>/`>'nin Azure AD kiracısının kiracı kimliği olduğu yer \<olarak ayarlar.

Örneğin, Veren öğesi ile bir yanıt aşağıdaki örnek gibi görünebilir:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Durum

Öğe, `Status` oturum açmanın başarısını veya başarısızlığını iletir. Bir kod `StatusCode` veya isteğin durumunu temsil eden iç içe kodlar kümesini içeren öğeyi içerir. Ayrıca, oturum `StatusMessage` açma işlemi sırasında oluşturulan özel hata iletilerini içeren öğeyi de içerir.

<!-- TODO: Add an authentication protocol error reference -->

Aşağıdaki örnek, başarısız bir oturum açma girişimine verilen SAML yanıtıdır.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Onaylama işlemi

Buna ek `ID`olarak `IssueInstant` `Version`, ve , Azure AD `Assertion` yanıt öğesiaşağıdaki öğeleri ayarlar.

#### <a name="issuer"></a>Veren

Bu, TenantIDGUID>'nin Azure AD kiracısının Kiracı Kimliği olduğu `https://sts.windows.net/<TenantIDGUID>/`yer \<olarak ayarlanır.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>İmza

Azure AD, başarılı bir oturum açma ya da bu iddiayı imzalar. Öğe, `Signature` bulut hizmetinin iddianın bütünlüğünü doğrulamak için kaynağı doğrulamak için kullanabileceği dijital imza içerir.

Bu dijital imzayı oluşturmak için Azure AD, meta veri belgesinin `IDPSSODescriptor` öğesinde imzalama anahtarını kullanır.

```
<ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Özne

Bu, iddiadaki ifadelerin konusu olan asıl lığı belirtir. Kimlik doğrulaması yapılan kullanıcıyı temsil eden bir `NameID` öğe içerir. Değer, `NameID` yalnızca belirteci için hedef kitle olan hizmet sağlayıcısına yönlendirilen hedeflenmiş bir tanımlayıcıdır. Bu kalıcıdır - iptal edilebilir, ancak yeniden atandırılmalı. Ayrıca opaktır, kullanıcı hakkında hiçbir şey göstermez ve öznitelik sorguları için tanımlayıcı olarak kullanılamaz.

Öğenin `Method` `SubjectConfirmation` özniteliği her zaman `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Koşullar

Bu öğe, SAML iddialarının kabul edilebilir kullanımını tanımlayan koşulları belirtir.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Ve `NotBefore` `NotOnOrAfter` öznitelikleri, iddianın geçerli olduğu aralığı belirtir.

* Özniteliğin `NotBefore` değeri, `IssueInstant` `Assertion` öğenin özniteliğinin değerinden daha sonra (bir saniyeden kısa) eşit veya biraz daha kısadır. Azure AD kendisi ile bulut hizmeti (servis sağlayıcısı) arasındaki zaman farkını hesaba katmaz ve bu süreye herhangi bir arabellek eklemez.
* Özniteliğin `NotOnOrAfter` değeri, özniteliğin değerinden `NotBefore` 70 dakika sonradır.

#### <a name="audience"></a>Hedef kitle

Bu, amaçlanan bir hedef kitleyi tanımlayan bir URI içerir. Azure AD, bu öğenin değerini `Issuer` oturum açma `AuthnRequest` işlemini başlatan öğenin öğesinin değerine ayarlar. `Audience` Değeri değerlendirmek için, uygulama kaydı `App ID URI` sırasında belirtilen değerin değerini kullanın.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

`Issuer` Değer gibi, `Audience` değer de Azure AD'deki bulut hizmetini temsil eden hizmet temel adlarından biriyle tam olarak eşleşmelidir. `Issuer` Ancak, öğenin değeri URI değeri değilse, `Audience` yanıttaki `Issuer` değer . `spn:`

#### <a name="attributestatement"></a>ÖznitelikBildirimi

Bu, konu veya kullanıcı yla ilgili talepleri içerir. Aşağıdaki alıntı bir `AttributeStatement` örnek öğe içerir. Elips, öğenin birden çok öznitelik ve öznitelik değerleri içerebileceğini gösterir.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```        

* **Ad Talebi** - Özniteliğin `Name` `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`değeri ( ) kimlik doğrulaması yapılan kullanıcının `testuser@managedtenant.com`kullanıcı ana adıdır.
* **ObjectIdentifier Claim** - Öznitelik `ObjectIdentifier` değeri`http://schemas.microsoft.com/identity/claims/objectidentifier`( `ObjectId` ) Azure AD'de kimlik doğrulaması yapılan kullanıcıyı temsil eden dizin nesnesidir. `ObjectId`değişmez, genel olarak benzersiz dir ve kimliği doğrulanan kullanıcının güvenli tanımlayıcısını yeniden kullanın.

#### <a name="authnstatement"></a>AuthnStatement

Bu öğe, iddia öznesinin belirli bir zamanda belirli bir araçla doğrulandığını ileri sürerek ileri süredilir.

* Öznitelik, `AuthnInstant` kullanıcının Azure AD ile kimlik doğrulamasını belirlediği zamanı belirtir.
* Öğe, `AuthnContext` kullanıcının kimliğini doğrulamak için kullanılan kimlik doğrulama bağlamını belirtir.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
