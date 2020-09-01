---
title: Azure kanıtlama talep kuralı dilbilgisi
description: Azure kanıtlama ilkesi talepleri ve talep kuralları hakkında ayrıntılar.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: a0f3e517e60037de6456bc3a549e072e58e2fa67
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89244747"
---
# <a name="claim-and-claim-rules"></a>Talep ve talep kuralları

Talep kurallarının dilbilgisini anlamak için öncelikle kanıtlama ilkesi taleplerini anlamanız gerekir.

## <a name="claim"></a>İste

Talep, ilgili bilgileri sağlamak için birlikte gruplanmış bir özellikler kümesidir. Azure kanıtlama için bir talep aşağıdaki özellikleri içerir:

- **tür**: talebin türünü temsil eden bir dize değeri.
- **değer**: talebin değerini temsil eden bir Boole, tamsayı veya dize değeri.
- **ValueType**: değer özelliğinde depolanan bilgilerin veri türü. Desteklenen türler dize, tamsayı ve Boole. Tanımlı değilse, varsayılan değer "String" olacaktır.
- **veren**: talebin verenle ilgili bilgiler. Veren aşağıdaki türlerden biri olacaktır:
  - **AttestationService**: belirli talepler Azure kanıtlama tarafından kullanılabilir hale getirilir ve bu ilke, uygun ilkeyi yapmak için kanıtlama ilkesi yazarı tarafından kullanılabilir.
  - **AttestationPolicy**: ilke (yönetici tarafından tanımlanan), işlem sırasında gelen kanıta talepler ekleyebilir. Bu durumda veren "AttestationPolicy" olarak ayarlanır.
  - **Customclaım**: ATTESTOR (istemci), kanıtlama kanıasında ek talepler de ekleyebilir. Bu durumda veren "CustomClaim" olarak ayarlanır.

Tanımlı değilse. Varsayılan değer "CustomClaim" olacaktır.

## <a name="claim-rule"></a>Talep kuralı

Gelen talep kümesi, kanıtlama sonucunu hesaplamak için ilke altyapısı tarafından kullanılır. Talep kuralı, gelen talepleri doğrulamak ve tanımlanan eylemi gerçekleştirmek için kullanılan bir koşullar kümesidir.

```
Conditions list => Action (Claim)
```

Bir talep kuralının Azure kanıtlama değerlendirmesi aşağıdaki adımları içerir:

- Koşullar listesi yoksa, eylemi belirtilen talebe göre yürütün 
- Aksi takdirde, koşullar listesinden koşulları değerlendirin.
- Koşullar listesi false olarak değerlendirilirse durdur. Aksi takdirde, devam edin.

Bir talep kuralındaki koşullar, eylemin yürütülmesi gerekip gerekmediğini belirlemekte kullanılır. Koşullar listesi, "&&" işleci ile ayrılmış koşulların bir dizisidir.

Koşullar listesi şu şekilde yapılandırılır:

```
Condition && Condition && ...
```

Koşul şu şekilde yapılandırılır:

```
Identifier:[ClaimPropertyCondition, ClaimPropertyCondition,…]
```

Koşul, bir talebin çeşitli özelliklerindeki tek tek koşullardan oluşur. Bir koşul, koşulu karşılayan talebi/öğeleri belirtmek için kullanılabilecek isteğe bağlı bir tanımlayıcıya sahip olabilir. Bu başvuru, diğer koşullarda veya aynı kuraldaki eylemde kullanılabilir.

Örneğin:

```
F1:[type=="OSName" , issuer=="CustomClaim"] && 
[type=="OSName" , issuer=="AttestationService", value== F1.value ] 
=> issueproperty(type="report_validity_in_minutes", value=1440);

F1:[type=="OSName" , issuer=="CustomClaim"] && 
C2:[type=="OSName" , issuer=="AttestationService", value== F1.value ] 
=> issue(claim = C2);
```

Koşulları denetlemek için kullanılabilen işleçler aşağıda verilmiştir:

| ValueType | Desteklenen işlemler |
|--|--|
| Tamsayı | = = (Equals), \! = (eşit değil), <= (küçüktür veya eşittir), < (küçüktür), >= (büyüktür veya eşittir), > (büyüktür) |
| Dize | = = (eşittir), \! = (eşit değil) |
| Boole | = = (eşittir), \! = (eşit değil) |

Koşullar listesinin değerlendirmesi:

- "&&" işlecinin varlığı, bir koşul listesinin yalnızca listedeki koşulların tümü doğru olarak değerlendiriliyorsa doğru olarak değerlendirildiğini belirtir.
- Bir koşul, talepler kümesindeki filtre ölçütlerini temsil eder. Koşulu karşılayan en az bir talep bulunursa, koşulun kendisi doğru olarak değerlendirilir.
- Bir talep, her birinin, koşuldaki karşılık gelen talep özelliği koşullarını karşılıyorsa koşulun gösterdiği filtreleme ölçütünü karşılayarak kabul edilir.  

İlkede izin verilen eylemler kümesi aşağıda açıklanmıştır.

| Eylem fiili | Description | Bunların uygulandığı ilke bölümleri |
|--|--|--|
| izin ver () | Gelen talep kümesi **ıssuancerules**'yi hesaplamak için kullanılabilir. Parametre olarak hiçbir talep almaz | **authorizationrules** |
| Deny () | Gelen talep kümesi, **ıssuancerules** 'in parametre olarak hiçbir talep almıyor olması için kullanılmamalıdır | **authorizationrules** |
| Ekle (talep) | Talepleri gelen talep kümesine ekler. Gelen talep kümesine eklenen herhangi bir talep, sonraki talep kuralları için de kullanılabilir. |**authorizationrules**, **ıssuancerules** |
| sorun (talep) | Gelen ve giden talep kümesine talebi ekler | **ıssuancerules** |
| ıssueproperty (talep) | Talebi gelen ve özellik talep kümesine ekler | **ıssuancerules**

## <a name="next-steps"></a>Sonraki adımlar

- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)

