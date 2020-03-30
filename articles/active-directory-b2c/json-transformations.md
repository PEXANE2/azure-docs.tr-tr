---
title: JSON özel ilkeler için dönüşüm örnekleri iddia ediyor
titleSuffix: Azure AD B2C
description: JSON, Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için dönüşüm örnekleri talep ediyor.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ad8fcf578ae1c89856a9d7929af0aec813cb4082
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187602"
---
# <a name="json-claims-transformations"></a>JSON dönüşümleri iddia ediyor

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) kimlik deneyimi çerçevesi şemasının JSON talep dönüşümlerini kullanmak için örnekler verilmektedir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="generatejson"></a>GenerateJson

Bir JSON dizesi oluşturmak için talep değerlerini veya sabitleri kullanın. Nokta işaretlerini izleyen yol dizesi, verilerin JSON dizesine nereye eklenirse eklenmeyeceğini belirtmek için kullanılır. Noktalarla bölündükten sonra, herhangi bir tamsayı JSON dizisinin dizini olarak yorumlanır ve tamsayılar olmayanlar bir JSON nesnesinin dizini olarak yorumlanır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | Nokta gösterimini izleyen herhangi bir dize | string | Talep değerinin eklendiği JSON'un JsonPath'i. |
| ınputparameter | Nokta gösterimini izleyen herhangi bir dize | string | JSON'un JsonPath'i, sabit dize değerinin eklendiği yer. |
| ÇıktılarTalep | outputClaim | string | Oluşturulan JSON dizesi. |

Aşağıdaki örnek, "e-posta" ve "otp" yanı sıra sabit dizeleri talep değerine dayalı bir JSON dize oluşturur.

```XML
<ClaimsTransformation Id="GenerateRequestBody" TransformationMethod="GenerateJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="personalizations.0.to.0.email" />
    <InputClaim ClaimTypeReferenceId="otp" TransformationClaimType="personalizations.0.dynamic_template_data.otp" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="template_id" DataType="string" Value="d-4c56ffb40fa648b1aa6822283df94f60"/>
    <InputParameter Id="from.email" DataType="string" Value="service@contoso.com"/>
    <InputParameter Id="personalizations.0.subject" DataType="string" Value="Contoso account email verification code"/>
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="requestBody" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

Aşağıdaki talepler dönüşüm çıkışları SendGrid (bir üçüncü taraf e-posta sağlayıcısı) gönderilen istek gövdesi olacak bir JSON dize iddia. JSON nesnesinin yapısı, Giriş Parametreleri'nin nokta gösterimindeki disler ve Giriş Taleplerinin DönüşümHakkı Türleri tarafından tanımlanır. Nokta gösterimindeki sayılar dizileri ima eder. Değerler, InputClaims'in değerlerinden ve InputParametrelerinin "Değer" özelliklerinden gelir.

- Giriş talepleri :
  - **e-posta**, dönüşüm talep türü **personalizations.0.to.0.email**: "someone@example.com"
  - **otp**, dönüşüm talep türü **kişiselleştirmeler.0.dynamic_template_data.otp** "346349"
- Giriş parametresi:
  - **template_id**: "d-4c56ffb40fa648b1aa6822283df94f60"
  - **from.email**:service@contoso.com" "
  - **personalizations.0.subject** "Contoso hesap e-posta doğrulama kodu"
- Çıktı talebi:
  - **requestBody**: JSON değeri

```JSON
{
  "personalizations": [
    {
      "to": [
        {
          "email": "someone@example.com"
        }
      ],
      "dynamic_template_data": {
        "otp": "346349",
        "verify-email" : "someone@example.com"
      },
      "subject": "Contoso account email verification code"
    }
  ],
  "template_id": "d-989077fbba9746e89f3f6411f596fb96",
  "from": {
    "email": "service@contoso.com"
  }
}
```

## <a name="getclaimfromjson"></a>GetClaimFromJson

JSON verilerinden belirli bir öğe alın.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputJson | string | Öğeyi almak için talep dönüştürme tarafından kullanılan ClaimTypes. |
| ınputparameter | claimToExtract | string | çıkarılacak JSON öğesinin adı. |
| ÇıktılarTalep | extractedClaim | string | Bu talep dönüştürmesinden sonra üretilen ClaimType çağrıldı, _claimToExtract_ giriş parametresinde belirtilen öğe değeri. |

Aşağıdaki örnekte, talep dönüştürme JSON verilerinden `emailAddress` öğe ayıklanır:`{"emailAddress": "someone@example.com", "displayName": "Someone"}`

```XML
<ClaimsTransformation Id="GetEmailClaimFromJson" TransformationMethod="GetClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="customUserData" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="emailAddress" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="extractedEmail" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Someone"}
- Giriş parametresi:
    - **claimToExtract**: emailAddress
- Çıktı talepleri:
  - **extractedClaim**:someone@example.com


## <a name="getclaimsfromjsonarray"></a>GetClaimsFromJsonArray

Json verilerinden belirtilen öğelerin listesini alın.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | jsonSourceClaim | string | Talepleri almak için talep dönüştürme tarafından kullanılan ClaimTypes. |
| ınputparameter | errorOnMissingClaims | boole | Taleplerden biri eksikse hata açılıp atılmayacağını belirtir. |
| ınputparameter | includeEmptyClaims | string | Boş talepler dahil edip etmeyeceğini belirtin. |
| ınputparameter | jsonSourceKeyName | string | Öğe anahtar adı |
| ınputparameter | jsonSourceValueName | string | Öğe değeri adı |
| ÇıktılarTalep | Koleksiyon | dize, int, boolean ve datetime |Çıkarılacak iddiaların listesi. Talebin adı _jsonSourceClaim_ giriş talebinde belirtilene eşit olmalıdır. |

Aşağıdaki örnekte, talep dönüştürme aşağıdaki iddiaları ayıklar: e-posta (dize), displayName (string), membershipNum (int), etkin (boolean) ve doğum tarihi (datetime) JSON verilerinden.

```JSON
[{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value":true}, {"key":"birthdate","value":"1980-09-23T00:00:00Z"}]
```

```XML
<ClaimsTransformation Id="GetClaimsFromJson" TransformationMethod="GetClaimsFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="jsonSourceClaim" TransformationClaimType="jsonSource" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="errorOnMissingClaims" DataType="boolean" Value="false" />
    <InputParameter Id="includeEmptyClaims" DataType="boolean" Value="false" />
    <InputParameter Id="jsonSourceKeyName" DataType="string" Value="key" />
    <InputParameter Id="jsonSourceValueName" DataType="string" Value="value" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="membershipNum" />
    <OutputClaim ClaimTypeReferenceId="active" />
    <OutputClaim ClaimTypeReferenceId="birthdate" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
  - **jsonSourceClaim**: [{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value": true}, {"key":"birthdate","value":"1980-09-23T00:00Z}]
- Giriş parametreleri:
    - **errorOnMissingClaims**: false
    - **includeEmptyClaims**: false
    - **jsonSourceKeyName**: anahtar
    - **jsonSourceValueName**: değer
- Çıktı talepleri:
  - **e-posta**: "someone@example.com"
  - **displayName**: "Birisi"
  - **üyelikNum**: 6353399
  - **active**: true
  - **doğum tarihi**: 1980-09-23T00:00:00Z

## <a name="getnumericclaimfromjson"></a>GetNumericClaimFromJson

JSON verilerinden belirli bir sayısal (uzun) öğe alır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputJson | string | Talep almak için talep dönüştürme tarafından kullanılan ClaimTypes. |
| ınputparameter | claimToExtract | string | Çıkarılacak JSON öğesinin adı. |
| ÇıktılarTalep | extractedClaim | long | Bu TalepDönüştürme çağrıldıktan sonra üretilen ClaimType, öğenin değeri _claimToExtract_ giriş parametrelerinde belirtilir. |

Aşağıdaki örnekte, talep dönüştürme JSON verilerinden `id` öğeayıklar.

```JSON
{
    "emailAddress": "someone@example.com",
    "displayName": "Someone",
    "id" : 6353399
}
```

```XML
<ClaimsTransformation Id="GetIdFromResponse" TransformationMethod="GetNumericClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="exampleInputClaim" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="id" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="membershipId" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Someone", "id" : 6353399}
- Giriş parametreleri
    - **claimToExtract**: id
- Çıktı talepleri:
    - **çıkarılanTalep**: 6353399

## <a name="getsinglevaluefromjsonarray"></a>GetSingleValueFromJsonArray

İlk öğeyi bir JSON veri dizisinden alır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputJsonClaim | string | Öğeyi JSON dizisinden almak için talep dönüştürme tarafından kullanılan ClaimTypes. |
| ÇıktılarTalep | extractedClaim | string | Bu ClaimsTransformation'den sonra üretilen ClaimType, JSON dizisindeki ilk öğe olan çağrıldı. |

Aşağıdaki örnekte, talep dönüştürme JSON dizisinden `["someone@example.com", "Someone", 6353399]`ilk öğeyi (e-posta adresi) ayıklar.

```XML
<ClaimsTransformation Id="GetEmailFromJson" TransformationMethod="GetSingleValueFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userData" TransformationClaimType="inputJsonClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputJsonClaim**:someone@example.com[" ", "Birisi", 6353399]
- Çıktı talepleri:
  - **extractedClaim**:someone@example.com

## <a name="xmlstringtojsonstring"></a>XmlStringToJsonString

XML verilerini JSON biçimine dönüştürür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | xml | string | Verileri XML'den JSON biçimine dönüştürmek için talep dönüştürme tarafından kullanılan ClaimTypes. |
| ÇıktılarTalep | json | string | Bu ClaimsTransformation çağrıldıktan sonra üretilen ClaimType, JSON formatında veri. |

```XML
<ClaimsTransformation Id="ConvertXmlToJson" TransformationMethod="XmlStringToJsonString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="intpuXML" TransformationClaimType="xml" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="outputJson" TransformationClaimType="json" />
  </OutputClaims>
</ClaimsTransformation>
```

Aşağıdaki örnekte, talep dönüştürme json biçimine aşağıdaki XML verileri dönüştürür.

#### <a name="example"></a>Örnek
Giriş iddiası:

```XML
<user>
  <name>Someone</name>
  <email>someone@example.com</email>
</user>
```

Çıktı talebi:

```JSON
{
  "user": {
    "name":"Someone",
    "email":"someone@example.com"
  }
}
```
