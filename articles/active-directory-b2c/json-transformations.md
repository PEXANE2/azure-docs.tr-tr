---
title: Özel ilkeler için JSON talep dönüştürme örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için JSON talep dönüştürme örnekleri.
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
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78187602"
---
# <a name="json-claims-transformations"></a>JSON talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) öğesinde kimlik deneyimi çerçevesi şemasının JSON talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="generatejson"></a>GenerateJson

JSON dizesi oluşturmak için talep değerlerini veya sabitleri kullanın. Nokta gösterimini izleyen yol dizesi, verilerin bir JSON dizesine ekleneceği yeri belirtmek için kullanılır. Noktalarla bölündikten sonra, tüm tamsayılar JSON dizisinin dizini olarak yorumlanır ve tamsayı olmayan bir JSON nesnesinin dizini olarak yorumlanır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | Nokta gösterimini izleyen dizeler | string | Talep değerinin ekleneceği JSON 'ın JsonPath değeri. |
| InputParameter | Nokta gösterimini izleyen dizeler | string | Sabit dize değerinin ekleneceği JSON 'un JsonPath değeri. |
| OutputClaim | OutputClaim | string | Oluşturulan JSON dizesi. |

Aşağıdaki örnek, "e-posta" ve "OTP" talep değerine ve sabit dizelere göre bir JSON dizesi üretir.

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

Aşağıdaki talep dönüştürmesi, SendGrid 'e gönderilen isteğin gövdesi olacak bir JSON dize talebi verir (bir üçüncü taraf e-posta sağlayıcısı). JSON nesnesinin yapısı, InputParameters 'ın nokta gösteriminde ve ınputclaim 'nin dönüştürme Tionclaimtypes öğesindeki kimlikler tarafından tanımlanır. Nokta gösterimindeki sayılar dizileri kapsıyor. Değerler Inputclaim değerlerinin ve InputParameters ' "Value" özelliklerinden gelir.

- Giriş talepleri:
  - **e-posta**, dönüşüm talep türü **kişiselleştirmeler. 0. to. 0. email**: "someone@example.com"
  - **OTP**, dönüşüm talep türü **kişiselleştirmeler. 0. dynamic_template_data. OTP** "346349"
- Giriş parametresi:
  - **template_id**: "d-4c56ffb40fa648b1aa6822283df94f60"
  - **Kimden. e-posta**: "service@contoso.com"
  - **kişiselleştirmeler. 0. Subject** "contoso hesabı e-posta doğrulama kodu"
- Çıkış talebi:
  - **Requestbody**: JSON değeri

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

JSON verilerinden belirtilen bir öğeyi alır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputjson | string | Öğeyi almak için talep dönüştürmesi tarafından kullanılan ClaimTypes. |
| InputParameter | Claimtoayıkla | string | Ayıklanacak JSON öğesinin adı. |
| OutputClaim | extractedClaim | string | Bu talep dönüşümünde oluşturulan ClaimType, _Claimtoextract_ giriş parametresinde belirtilen öğe değeri çağırılır. |

Aşağıdaki örnekte, talep dönüştürmesi JSON verilerinden `emailAddress` öğesini ayıkladı: `{"emailAddress": "someone@example.com", "displayName": "Someone"}`

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
  - **ınputjson**: {"emadresi": "someone@example.com", "DisplayName": "birisi"}
- Giriş parametresi:
    - **Claimtoayıkla**: emaadresi
- Çıkış talepleri:
  - **Extractedclaim**: someone@example.com


## <a name="getclaimsfromjsonarray"></a>GetClaimsFromJsonArray

JSON verilerinden belirtilen öğelerin bir listesini alın.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | Jsonsourceclaım | string | Talepleri almak için talep dönüştürmesi tarafından kullanılan ClaimTypes. |
| InputParameter | Erroronmissingclaim | boole | Taleplerden biri eksikse bir hata oluşturulup oluşturulmayacağını belirtir. |
| InputParameter | ıncludeemptyclaim | string | Boş talepler eklenip eklenmeyeceğini belirtin. |
| InputParameter | jsonSourceKeyName | string | Öğe anahtarı adı |
| InputParameter | jsonSourceValueName | string | Öğe değeri adı |
| OutputClaim | Koleksiyon | String, int, Boolean ve DateTime |Ayıklanacak talepler listesi. Talebin adı, _Jsonsourceclaim_ giriş talebinde belirtilen değere eşit olmalıdır. |

Aşağıdaki örnekte, talep dönüştürmesi şu talepleri ayıklar: e-posta (dize), displayName (dize), membershipNum (int), Active (Boolean) ve Doğum tarihi (DateTime) JSON verilerinden.

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
  - **Jsonsourceclaim**: [{"Key": "e-posta", "Value": "someone@example.com"}, {"Key": "DisplayName", "Value": "birisi"}, {"Key": "membershipnum", "Value": 6353399}, {"Key": "etkin", "Value": true}, {"Key": "doğatarihi", "Value": "1980-09-23T00:00:00Z"}]
- Giriş parametreleri:
    - **Erroronmissingclaim**: false
    - **ıncludeemptyclaim**: false
    - **jsonSourceKeyName**: anahtar
    - **Jsonsourcevaluename**: değer
- Çıkış talepleri:
  - **e-posta**: "someone@example.com"
  - **DisplayName**: "birisi"
  - **Membershipnum**: 6353399
  - **etkin**: doğru
  - **Doğum tarihi**: 1980-09-23T00:00:00Z

## <a name="getnumericclaimfromjson"></a>GetNumericClaimFromJson

JSON verilerinden belirtilen bir sayısal (Long) öğeyi alır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputjson | string | Talebi almak için talep dönüştürmesi tarafından kullanılan ClaimTypes. |
| InputParameter | Claimtoayıkla | string | Ayıklanacak JSON öğesinin adı. |
| OutputClaim | extractedClaim | long | Bu Claimstranssetting çağrıldıktan sonra üretilen ClaimType, _Claimtoextract_ giriş parametrelerinde belirtilen öğenin değeri olarak çağırılır. |

Aşağıdaki örnekte, talep dönüştürmesi JSON verilerinden `id` öğesini ayıklar.

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
  - **ınputjson**: {"emadresi": "someone@example.com", "DisplayName": "birisi", "id": 6353399}
- Giriş parametreleri
    - **Claimtoayıkla**: kimlik
- Çıkış talepleri:
    - **Extractedclaim**: 6353399

## <a name="getsinglevaluefromjsonarray"></a>GetSingleValueFromJsonArray

JSON veri dizisindeki ilk öğeyi alır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputjsonclaim | string | JSON dizisinden öğeyi almak için talep dönüştürmesi tarafından kullanılan ClaimTypes. |
| OutputClaim | extractedClaim | string | Bu Claimstrans, tarafından üretilen ClaimType, JSON dizisindeki ilk öğe çağırılır. |

Aşağıdaki örnekte, talep dönüştürmesi `["someone@example.com", "Someone", 6353399]`JSON dizisinden ilk öğeyi (e-posta adresi) ayıklar.

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
  - **ınputjsonclaim**: ["someone@example.com", "birisi", 6353399]
- Çıkış talepleri:
  - **Extractedclaim**: someone@example.com

## <a name="xmlstringtojsonstring"></a>XmlStringToJsonString

XML verilerini JSON biçimine dönüştürür.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | xml | string | Verileri XML 'den JSON biçimine dönüştürmek için talep dönüştürmesi tarafından kullanılan ClaimTypes. |
| OutputClaim | nesnesinde | string | Bu Claimstransformadıktan sonra üretilen ClaimType, verileri JSON biçiminde çağırırdı. |

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

Aşağıdaki örnekte, talep dönüştürmesi aşağıdaki XML verilerini JSON biçimine dönüştürür.

#### <a name="example"></a>Örnek
Giriş talebi:

```XML
<user>
  <name>Someone</name>
  <email>someone@example.com</email>
</user>
```

Çıkış talebi:

```JSON
{
  "user": {
    "name":"Someone",
    "email":"someone@example.com"
  }
}
```
