---
title: Özel bir ilkede doğrulama teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki özel bir ilkede doğrulama teknik profili kullanarak talepleri doğrulayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1eaf159149bb353b1cf0474aad5bc233decddc5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481577"
---
# <a name="define-a-validation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde doğrulama teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Doğrulama teknik profili, [Azure Etkin Dizini](active-directory-technical-profile.md) veya [REST API](restful-technical-profile.md)gibi herhangi bir protokolün sıradan bir teknik profilidir. Doğrulama teknik profili çıktı taleplerini döndürür veya aşağıdaki verilerle 4xx HTTP durum kodunu döndürür. Daha fazla bilgi için [dönen hata iletisi](restful-technical-profile.md#returning-error-message)

```JSON
{
    "version": "1.0.0",
    "status": 409,
    "userMessage": "Your error message"
}
```

Doğrulama teknik profilinin çıktı taleplerinin kapsamı, doğrulama teknik profilini çağıran [kendi kendini öne süren teknik profil](self-asserted-technical-profile.md) ve doğrulama teknik profilleriile sınırlıdır. Çıktı taleplerini bir sonraki düzenleme adımında kullanmak istiyorsanız, çıktı taleplerini doğrulama teknik profilini çağıran kendi kendini öne süren teknik profile ekleyin.

Doğrulama teknik profilleri, **DoğrulamaTeknik Profiller** öğesinde göründükleri sırayla yürütülür. Doğrulama teknik profili bir hata ortaya çıkarırsa veya başarılı olursa, sonraki doğrulama teknik profillerinin yürütülmesinin devam edip etmeyeceğini bir doğrulama teknik profilinde yapılandırabilirsiniz.

Doğrulama teknik profili, **ValidationTechnicalProfile** öğesinde tanımlanan ön koşullara göre koşullu olarak yürütülebilir. Örneğin, belirli bir talebin var olup olmadığını veya bir talebin belirtilen değere eşit olup olmadığını denetleyebilirsiniz.

Kendi kendine ileri atılmış bir teknik profil, çıktı taleplerinin bir kısmını veya tamamını doğrulamak için kullanılacak bir doğrulama teknik profili tanımlayabilir. Başvurulan teknik profilin tüm giriş iddiaları, başvuru doğrulama teknik profilinin çıktı taleplerinde görünmelidir.

> [!NOTE]
> Yalnızca kendi kendini öne süren teknik profiller doğrulama teknik profillerini kullanabilir. Kendi kendine ileri edilmeyen teknik profillerden çıktı taleplerini doğrulamanız gerekiyorsa, doğrulamadan sorumlu teknik profili karşılamak için kullanıcı yolculuğunuzda ek bir düzenleme adımı kullanmayı düşünün.

## <a name="validationtechnicalprofiles"></a>DoğrulamaTeknik Profiller

**DoğrulamaTeknik Profiller** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| DoğrulamaTeknikProfil | 1:n | Başvuru teknik profilinin çıktı taleplerinin bir kısmını veya tamamını doğrulamak için kullanılacak teknik profil. |

**DoğrulamaTeknikProfil** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke veya üst ilkede zaten tanımlanmış bir teknik profilin tanımlayıcısı. |
|ContinueOnError|Hayır| Bu doğrulama teknik profil bir hata yükseltirse sonraki doğrulama teknik profillerin doğrulanması devam edip etmeyeceğini belirten. Olası `true` değerler: `false` veya (varsayılan olarak, daha fazla doğrulama profillerinin işlenmesi durdurulacak ve bir hata döndürülür). |
|ContinueOnSuccess | Hayır | Bu doğrulama teknik profilbaşarılı olursa sonraki doğrulama profillerinin doğrulanması devam edip etmeyeceğini belirten. Olası `true` değerler: `false`veya . Varsayılan, `true`diğer doğrulama profillerinin işlenmesi devam edeceği anlamına gelir. |

**DoğrulamaTeknikProfil** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Önkoşulları | 0:1 | Doğrulama teknik profilinin yürütülmesi için karşılanması gereken ön koşulların listesi. |

**Önkoşul** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Ön koşul için gerçekleştirecek denetim veya sorgu türü. `ClaimsExist` Belirtilen talepler kullanıcının geçerli talep kümesinde varsa eylemlerin gerçekleştirilmesini sağlamak için `ClaimEquals` belirtilir veya belirtilen talep varsa ve değeri belirtilen değere eşitse eylemlerin gerçekleştirilmesi gerektiği belirtilir. |
| `ExecuteActionsIf` | Evet | Test doğru veya yanlışsa ön koşuldaki eylemlerin gerçekleştirilip gerçekleştirilmeyeceğini gösterir. |

**Önkoşul** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Değer | 1:n | Denetim tarafından kullanılan veriler. Bu denetimin türü `ClaimsExist`ise, bu alan için sorgulanacak bir ClaimTypeReferenceId belirtir. Denetim türü ise, `ClaimEquals`bu alan için sorguiçin bir ClaimTypeReferenceId belirtir. Başka bir değer öğesi denetlenecek değeri içerirken.|
| Eylem | 1:1 | Bir düzenleme adımı içinde ön koşul denetimi doğruysa yapılması gereken eylem. **Eylemin** değeri ' olarak `SkipThisValidationTechnicalProfile`ayarlanır. İlişkili doğrulama teknik profilinin yürütülmemesi gerektiğini belirtir. |

### <a name="example"></a>Örnek

Aşağıdaki örnekte bu doğrulama teknik profilleri kullanır:

1. İlk doğrulama teknik profili kullanıcı kimlik bilgilerini denetler ve geçersiz kullanıcı adı veya hatalı parola gibi bir hata oluşursa devam etmez.
2. Sonraki doğrulama teknik profili, userType claim yoksa veya userType değeri ise `Partner`yürütmez . Doğrulama teknik profili, kullanıcı profilini dahili müşteri veritabanından okumaya çalışır ve REST API hizmeti nin kullanılamaması veya herhangi bir iç hata gibi bir hata oluşursa devam etmeye çalışır.
3. Son doğrulama teknik profili, userType talebi yoksa veya userType değeri ise `Customer`yürütmez. Doğrulama teknik profili kullanıcı profilini iç iş ortağı veritabanından okumaya çalışır ve REST API hizmeti nin kullanılamaması veya herhangi bir iç hata gibi bir hata oluşursa devam eder.

```XML
<ValidationTechnicalProfiles>
  <ValidationTechnicalProfile ReferenceId="login-NonInteractive" ContinueOnError="false" />
  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromCustomertsDatabase" ContinueOnError="true" >
    <Preconditions>
      <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
        <Value>userType</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
      <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
        <Value>userType</Value>
        <Value>Partner</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
    </Preconditions>
  </ValidationTechnicalProfile>
  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromPartnersDatabase" ContinueOnError="true" >
    <Preconditions>
      <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
        <Value>userType</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
      <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
        <Value>userType</Value>
        <Value>Customer</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
    </Preconditions>
  </ValidationTechnicalProfile>
</ValidationTechnicalProfiles>
```
