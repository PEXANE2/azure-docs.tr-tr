---
title: Azure çoklu oturum açma SAML Protokolü | Microsoft Docs
description: Bu makalede Azure Active Directory içindeki çoklu oturum açma SAML Protokolü açıklanmaktadır
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79262431"
---
# <a name="single-sign-on-saml-protocol"></a>Çoklu oturum açma SAML Protokolü

Bu makalede, Azure Active Directory (Azure AD) tarafından çoklu oturum açma için desteklenen SAML 2,0 kimlik doğrulama istekleri ve yanıtları ele alınmaktadır.

Aşağıdaki protokol diyagramında çoklu oturum açma sırası açıklanmaktadır. Bulut hizmeti (hizmet sağlayıcısı) bir `AuthnRequest` (kimlik doğrulama isteği) öğesini Azure AD 'ye (kimlik sağlayıcısı) geçirmek için HTTP yeniden yönlendirme bağlamasını kullanır. Daha sonra Azure AD, bulut hizmetine bir `Response` öğesi göndermek için bir HTTP POST bağlaması kullanır.

![Çoklu oturum açma Iş akışı](./media/single-sign-on-saml-protocol/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>Authbir kimlik doğrulama

Bir kullanıcı kimlik doğrulaması istemek için, bulut hizmetleri Azure AD 'ye bir `AuthnRequest` öğesi gönderir. Örnek SAML 2,0 `AuthnRequest` aşağıdaki örnekteki gibi görünebilir:

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
| Kimlik | Gerekli | Azure AD döndürülen yanıtın `InResponseTo` özniteliğini doldurmak için bu özniteliği kullanır. KIMLIK bir sayıyla başlamamalıdır, bu nedenle ortak bir strateji, bir GUID 'nin dize gösterimine "ID" gibi bir dizeyi eklemek için kullanılır. Örneğin, `id6c1c178c166d486687be4aaf5e482730` geçerli bir KIMLIK. |
| Sürüm | Gerekli | Bu parametre **2,0**olarak ayarlanmalıdır. |
| IssueInstant | Gerekli | Bu, UTC değeri ve [gidiş dönüş biçimine ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx)sahip bir tarih saat dizesidir. Azure AD bu türde bir DateTime değeri bekler, ancak bu değeri değerlendirmez veya kullanmaz. |
| AssertionConsumerServiceUrl | İsteğe bağlı | Bu parametre sağlanmışsa, Azure AD 'deki bulut hizmetinin `RedirectUri` eşleşmelidir. |
| ForceAuthn | İsteğe bağlı | Bu bir Boole değeridir. Doğru ise, kullanıcının Azure AD ile geçerli bir oturumu olsa bile yeniden kimlik doğrulaması yapması zorunlu olacaktır. |
| Ipassive | İsteğe bağlı | Bu, Azure AD 'nin Kullanıcı etkileşimi olmadan kullanıcının kimlik doğrulaması yapması gerekip gerekmediğini, varsa oturum tanımlama bilgisini kullanmasını belirten bir Boole değeridir. Bu değer doğru ise, Azure AD oturum tanımlama bilgisini kullanarak kullanıcının kimliğini doğrulamaya çalışacaktır. |

Onay, hedef, Assertionconsumerserviceındex, Attributeconsumerserviceındex ve ProviderName gibi diğer tüm `AuthnRequest` öznitelikleri **yok sayılır**.

Azure AD, `AuthnRequest``Conditions` öğesini de yoksayar.

### <a name="issuer"></a>Veren

Bir `AuthnRequest` `Issuer` öğesi, Azure AD 'deki bulut hizmetindeki **ServicePrincipalNames adlarından** birine tam olarak uymalıdır. Genellikle, bu, uygulama kaydı sırasında belirtilen **uygulama KIMLIĞI URI** 'sine ayarlanır.

`Issuer` öğesini içeren bir SAML alıntısı aşağıdaki örneğe benzer şekilde görünür:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>Nameıdpolicy

Bu öğe, yanıtta belirli bir ad KIMLIĞI biçimi ister ve Azure AD 'ye gönderilen `AuthnRequest` öğelerinde isteğe bağlıdır.

Bir `NameIdPolicy` öğesi aşağıdaki örneğe benzer şekilde görünür:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

`NameIDPolicy` sağlanmışsa, isteğe bağlı `Format` özniteliğini dahil edebilirsiniz. `Format` özniteliği aşağıdaki değerlerden yalnızca birine sahip olabilir; diğer herhangi bir değer bir hatayla sonuçlanır.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory, NameID talebini ikili bir tanımlayıcı olarak verir.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory, e-posta adresi biçiminde NameID talebini yayınlar.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Bu değer Azure Active Directory talep biçimini seçmesine izin verir. Azure Active Directory, NameID 'yi ikili bir tanımlayıcı olarak verir.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: Azure Active Directory NameID talebini, geçerli SSO işlemi için benzersiz olan rastgele oluşturulmuş bir değer olarak verir. Bu, değerin geçici olduğu ve kimliği doğrulanmış kullanıcıyı tanımlamak için kullanılamayan anlamına gelir.

Azure AD `AllowCreate` özniteliğini yoksayar.

### <a name="requestauthncontext"></a>RequestAuthnContext
`RequestedAuthnContext` öğesi istenen kimlik doğrulama yöntemlerini belirtir. Azure AD 'ye gönderilen `AuthnRequest` öğelerinde isteğe bağlıdır. Azure AD, `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`gibi `AuthnContextClassRef` değerlerini destekler.

### <a name="scoping"></a>Kapsam
Kimlik sağlayıcılarının bir listesini içeren `Scoping` öğesi, Azure AD 'ye gönderilen `AuthnRequest` öğelerinde isteğe bağlıdır.

Sağlanmışsa, `ProxyCount` özniteliği, `IDPListOption` veya `RequesterID` öğesi desteklenmez.

### <a name="signature"></a>İmza
Azure AD imzalı kimlik doğrulama isteklerini desteklemediği için `AuthnRequest` öğelerine `Signature` öğesi eklemeyin.

### <a name="subject"></a>Özne
Azure AD, `AuthnRequest` öğelerin `Subject` öğesini yoksayar.

## <a name="response"></a>Yanıt
İstenen bir oturum açma işlemi başarıyla tamamlandığında, Azure AD, bulut hizmetine bir yanıt gönderir. Başarılı bir oturum açma girişimine yanıt aşağıdaki örneğe benzer şekilde görünür:

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

`Response` öğesi, yetkilendirme isteğinin sonucunu içerir. Azure AD, `Response` öğesindeki `ID`, `Version` ve `IssueInstant` değerlerini ayarlar. Ayrıca, aşağıdaki öznitelikleri de ayarlar:

* `Destination`: oturum açma başarıyla tamamlandığında, bu hizmet sağlayıcının (bulut hizmeti) `RedirectUri` ayarlanır.
* `InResponseTo`: Bu, yanıtı Başlatan `AuthnRequest` öğesinin `ID` özniteliğine ayarlanır.

### <a name="issuer"></a>Veren

Azure AD, `Issuer` öğesini \<TenantIDGUID > Azure AD kiracısının kiracı KIMLIĞI olduğu `https://login.microsoftonline.com/<TenantIDGUID>/` olarak ayarlar.

Örneğin, veren öğesiyle bir yanıt aşağıdaki örneğe benzeyebilir:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Durum

`Status` öğesi, oturum açma başarısını veya başarısızlığını alır. Bu, bir kodu içeren `StatusCode` öğesini veya isteğin durumunu temsil eden iç içe geçmiş kodlar kümesini içerir. Ayrıca, oturum açma işlemi sırasında oluşturulan özel hata iletilerini içeren `StatusMessage` öğesi de içerir.

<!-- TODO: Add an authentication protocol error reference -->

Aşağıdaki örnek, başarısız bir oturum açma girişimine bir SAML yanıtı örneğidir.

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

`ID`, `IssueInstant` ve `Version`ek olarak, Azure AD yanıtın `Assertion` öğesinde aşağıdaki öğeleri ayarlar.

#### <a name="issuer"></a>Veren

Bu, Azure AD kiracısının kiracı KIMLIĞI olan \<TenantIDGUID > `https://sts.windows.net/<TenantIDGUID>/`olarak ayarlanır.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>İmza

Azure AD, başarılı bir oturum açmaya yanıt olarak onaylama işlemi imzalar. `Signature` öğesi, bulut hizmetinin onay bütünlüğünü doğrulamak üzere kaynağın kimliğini doğrulamak için kullanabileceği dijital bir imza içerir.

Bu dijital imzayı oluşturmak için Azure AD, meta veri belgesinin `IDPSSODescriptor` öğesinde imzalama anahtarını kullanır.

```
<ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Özne

Bu, onay içindeki deyimlerin konusu olan sorumluyu belirtir. Kimliği doğrulanmış kullanıcıyı temsil eden bir `NameID` öğesi içerir. `NameID` değeri, yalnızca belirtecin hedef kitlesi olan hizmet sağlayıcısına yönlendirilen hedeflenen bir tanıtıcıdır. Kalıcı, iptal edilebilir, ancak hiçbir şekilde yeniden atanmaz. Ayrıca, kullanıcı hakkındaki herhangi bir şeyi açığa çıkarmadığından ve öznitelik sorguları için bir tanımlayıcı olarak kullanılamaz.

`SubjectConfirmation` öğesinin `Method` özniteliği her zaman `urn:oasis:names:tc:SAML:2.0:cm:bearer`olarak ayarlanmıştır.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Koşullar

Bu öğe, SAML onaylamaları için kabul edilebilir kullanımını tanımlayan koşulları belirtir.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

`NotBefore` ve `NotOnOrAfter` öznitelikleri, onaylama işlemi için geçerli olduğu aralığı belirtir.

* `NotBefore` özniteliğin değeri, `Assertion` öğesinin `IssueInstant` özniteliği değerinden daha sonra eşittir veya biraz (küçüktür). Azure AD, kendisiyle bulut hizmeti (hizmet sağlayıcısı) arasında herhangi bir zaman farkı hesaba almaz ve bu saate herhangi bir arabellek eklemez.
* `NotOnOrAfter` özniteliğinin değeri, `NotBefore` özniteliği değerinden daha sonra 70 dakikadır.

#### <a name="audience"></a>Hedef kitle

Bu, amaçlanan bir izleyiciyi tanımlayan bir URI içerir. Azure AD, bu öğenin değerini, oturum açmayı Başlatan `AuthnRequest` `Issuer` öğesinin değerine ayarlar. `Audience` değerini değerlendirmek için, uygulama kaydı sırasında belirtilen `App ID URI` değerini kullanın.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

`Issuer` değeri gibi `Audience` değeri, Azure AD 'deki bulut hizmetini temsil eden hizmet sorumlusu adlarından biriyle tam olarak eşleşmelidir. Ancak, `Issuer` öğesinin değeri bir URI değeri değilse, yanıttaki `Audience` değeri `spn:`önekli `Issuer` değeridir.

#### <a name="attributestatement"></a>AttributeStatement

Bu konu veya Kullanıcı hakkında talepler içerir. Aşağıdaki alıntı bir örnek `AttributeStatement` öğesi içerir. Üç nokta, öğesinin birden çok öznitelik ve öznitelik değeri içerebileceğini belirtir.

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

* **Ad talebi** -`Name` özniteliğinin değeri (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`), `testuser@managedtenant.com`gibi kimliği doğrulanmış kullanıcının Kullanıcı asıl adıdır.
* **Objectıdentifier Claim** -`ObjectIdentifier` özniteliğin değeri (`http://schemas.microsoft.com/identity/claims/objectidentifier`), Azure AD 'de kimliği doğrulanmış kullanıcıyı temsil eden dizin nesnesinin `ObjectId`. `ObjectId`, sabit, genel olarak benzersiz ve kimliği doğrulanmış kullanıcının güvenli tanımlayıcısını yeniden kullanır.

#### <a name="authnstatement"></a>Authndeyim

Bu öğe, onaylama konusunun belirli bir zamanda belirli bir anlama göre doğrulandığını onaylar.

* `AuthnInstant` özniteliği, kullanıcının kimlik doğrulamasının Azure AD ile süresini belirtir.
* `AuthnContext` öğesi, kullanıcının kimliğini doğrulamak için kullanılan kimlik doğrulaması bağlamını belirtir.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
