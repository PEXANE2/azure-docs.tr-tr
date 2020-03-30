---
title: Azure Active Directory B2C'de kullanıcı erişimini yönetme | Microsoft Dokümanlar
description: Azure AD B2C'yi kullanarak reşit olmayanları nasıl belirleyeceklerinizi, doğum tarihini ve ülke/bölge verilerini nasıl topladığınızı ve uygulamanızdaki kullanım koşullarını nasıl kabul edebilirsiniz öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/24/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: f04a3fea3801f917a3ae4aced04ef3824d1cfa82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184528"
---
# <a name="manage-user-access-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanıcı erişimini yönetme

Bu makalede, Azure Active Directory B2C (Azure AD B2C) kullanarak uygulamalarınıza kullanıcı erişimini nasıl yönetiniz izlenebilirsiniz. Uygulamanızdaki erişim yönetimi şunları içerir:

- Reşit olmayanları tanımlamak ve uygulamanız için kullanıcı erişimini denetlemek.
- Reşit olmayanların uygulamalarınızı kullanması için ebeveyn onayı alınması gerekmektedir.
- Kullanıcılardan doğum ve ülke/bölge verilerinin toplanması.
- Kullanım koşulları sözleşmesini yakalama ve erişim.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="control-minor-access"></a>Küçük erişimi denetleme

Uygulamalar ve kuruluşlar, reşit olmayanların bu hedef kitleyi hedeflememiş uygulamaları ve hizmetleri kullanmasını engellemeye karar verebilir. Alternatif olarak, uygulamalar ve kuruluşlar reşit olmayanları kabul etmeye ve daha sonra ebeveyn onayı yönetmeye karar verebilir ve reşit olmayanlar için iş kurallarının belirlediği ve yönetmelikte izin verilen izin verilen deneyimleri sunabilir.

Bir kullanıcı reşit olmayan olarak tanımlanırsa, Azure AD B2C'deki kullanıcı akışını üç seçenekten birine ayarlayabilirsiniz:

- **İmzalı jwt id_token uygulamaya geri gönderin**: Kullanıcı dizine kaydedilir ve bir belirteç uygulamaya iade edilir. Uygulama daha sonra iş kuralları uygulayarak devam eder. Örneğin, uygulama bir ebeveyn onay süreci ile devam edebilir. Bu yöntemi kullanmak **için, uygulamadan ageGroup** ve **consentProvidedForMinor** taleplerini almayı seçin.

- **Uygulamaya imzasız bir JSON belirteci gönderin**: Azure AD B2C, kullanıcının reşit olmadığını ve kullanıcının ebeveyn onayının durumunu sağladığını uygulamayla ilgili olarak belirtir. Uygulama daha sonra iş kuralları uygulayarak devam eder. JSON belirteci, uygulamayla başarılı bir kimlik doğrulaması tamamlamaz. Uygulama, **ad,** **e-posta**, **ageGroup**ve **consentProvidedForMinor**içerebilir JSON belirteci, dahil iddialara göre kimlik doğrulamasız kullanıcı işlemelidir.

- **Kullanıcıyı engelleme**: Bir kullanıcı reşit değilse ve ebeveyn onayı sağlanmadıysa, Azure AD B2C kullanıcıya engellendiğini bildirebilir. Belirteç verilmez, erişim engellenir ve kullanıcı hesabı kayıt yolculuğu sırasında oluşturulmaz. Bu bildirimi uygulamak için, kullanıcıyı bilgilendirmek ve uygun seçenekleri sunmak için uygun bir HTML/CSS içerik sayfası sağlarsınız. Yeni kayıtlar için başvuru tarafından başka bir işlem yapılması gerekmez.

## <a name="get-parental-consent"></a>Ebeveyn onayı alın

Uygulama düzenlemesine bağlı olarak, ebeveyn izninin yetişkin olarak doğrulanan bir kullanıcı tarafından verilmesi gerekebilir. Azure AD B2C, bir kişinin yaşını doğrulamak ve doğrulanmış bir yetişkinin reşit olmayan bir çocuğa ebeveyn izni vermesini sağlamak için bir deneyim sağlamaz. Bu deneyim, uygulama veya başka bir hizmet sağlayıcısı tarafından sağlanmalıdır.

Aşağıda, ebeveyn onayı toplamak için kullanıcı akışının bir örneği verilmiştir:

1. [Microsoft Graph API](https://docs.microsoft.com/graph/use-the-api) işlemi, kullanıcıyı reşit olmayan olarak tanımlar ve kullanıcı verilerini imzasız json belirteci biçiminde uygulamaya döndürür.

2. Uygulama JSON belirteci işler ve reşit olmayan bir ekran gösterir, ebeveyn onayı gerekli olduğunu onlara bildiren ve çevrimiçi bir ebeveynin onayı talep.

3. Azure AD B2C, kullanıcının normal olarak oturum açabileceği bir oturum açma yolculuğu gösterir ve **yasalAgeGroupClassification = "minorWithParentalConsent"** içerecek şekilde ayarlanmış uygulamaya bir belirteç sağlar. Uygulama, ebeveynin e-posta adresini toplar ve ebeveynin bir yetişkin olduğunu doğrular. Bunu yapmak için, ulusal kimlik ofisi, lisans doğrulaması veya kredi kartı kanıtı gibi güvenilir bir kaynak kullanır. Doğrulama başarılı olursa, uygulama reşit olmayan olandan Azure AD B2C kullanıcı akışını kullanarak oturum açmasını ister. İzin reddedilirse (örneğin, **yasalAgeGroupClassification = "minorWithoutParentalConsent"** ise), Azure AD B2C onay işlemini yeniden başlatmak için uygulamaya bir JSON belirteci (oturum açma değil) döndürür. Reşit olmayan bir ya da yetişkinin reşit olmayan bir hesabına kayıt kodu veya kayıtlı yetişkinin e-posta adresine kayıt kodu göndererek reşit olmayan bir hesabına yeniden erişebilmeleri için kullanıcı akışını özelleştirmek isteğe bağlı olarak mümkündür.

4. Uygulama, reşit olmayan çocuğun rızasını iptal etme seçeneği sunar.

5. Reşit olmayan veya yetişkin onayı iptal ettiğinde, Microsoft Graph API onayını değiştirmek için **kullanılabilirProvidedForMinor'** un **izni reddedildi.** Alternatif olarak, uygulama, rızası iptal edilmiş bir reşit olmayanı silmeyi seçebilir. Kullanıcı akışını, kimlik doğrulaması yapılan küçük çocuğun (veya reşit olmayan çocuğun hesabını kullanan ebeveynin) onayı iptal edilebilmek için özelleştirmek isteğe bağlı olarak mümkündür. Azure AD B2C kayıtları **onayProvidedForMinor** olarak **reddedildi**.

**YasalAgeGroupClassification**hakkında daha fazla bilgi için, **consentProvidedForMinor**, ve **ageGroup**, [Bkz. Kullanıcı kaynak türü](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/user). Özel öznitelikler hakkında daha fazla bilgi için, [tüketicilerinizle ilgili bilgi toplamak için özel öznitelikleri kullanın'a](user-flow-custom-attributes.md)bakın. Genişletilmiş öznitelikleri Microsoft Graph API'sını kullanarak ele aldığınızda, *extension_18b70cf9bb834edd8f38521c2583cd86_dateOfBirth:* *2011-01-01T00:00:00Z*gibi özniteliğin uzun sürümünü kullanmanız gerekir.

## <a name="gather-date-of-birth-and-countryregion-data"></a>Doğum tarihi ve ülke/bölge verilerini toplama

Uygulamalar, kayıt sırasında tüm kullanıcılardan doğum tarihi (DOB) ve ülke/bölge bilgilerini toplamak için Azure AD B2C'ye güvenebilir. Bu bilgiler zaten yoksa, uygulama bir sonraki kimlik doğrulama (oturum açma) yolculuğu sırasında kullanıcıdan isteyebilir. Kullanıcılar DOB ve ülke/bölge bilgilerini vermeden devam edemezler. Azure AD B2C, bu bilgileri, kişinin söz ülkenin/bölgenin düzenleyici standartlarına göre reşit olmayan kabul edilip edilemeyeceğini belirlemek için kullanır.

Özelleştirilmiş bir kullanıcı akışı DOB ve ülke/bölge bilgilerini toplayabilir ve Azure AD B2C talep dönüşümlerini **kullanarak ageGroup'u** belirleyebilir ve sonucu (dob ve ülke/bölge bilgilerini doğrudan devam ettir) dizinde devam etti.

Aşağıdaki adımlar, kullanıcının doğum tarihinden **ageGroup'u** hesaplamak için kullanılan mantığı gösterir:

1. Listedeki ülke koduna göre ülkeyi bulmaya çalışın. Ülke bulunmazsa, **Varsayılan'a**geri dön.

2. Ülke öğesinde **MinorConsent** düğümü varsa:

    a. Kullanıcının yetişkin olarak kabul edilebilmek için doğum yapmış olması gereken tarihi hesaplayın. Örneğin, geçerli tarih 14 Mart 2015 ve **MinorConsent** 18 ise, doğum tarihi en geç 14 Mart 2000 olmalıdır.

    b. Minimum doğum tarihini gerçek doğum tarihiyle karşılaştırın. Minimum doğum tarihi kullanıcının doğum tarihinden önceyse, hesaplama reşit olmayan ı **yaş** grubu hesaplaması olarak döndürür.

3. Ülke öğesinde **MinorNoConsentRequired** düğüm varsa, **MinorNoConsentRequired**değerini kullanarak 2a ve 2b adımlarını yineleyin. Minimum doğum tarihi kullanıcının doğum tarihinden önce ise 2b **çıktısı MinorNoConsentGerekli** döndürür.

4. Ne hesaplama doğru döndürürse, hesaplama **Yetişkin**döndürür.

Bir uygulama DOB veya ülke/bölge verilerini diğer yöntemlerle güvenilir bir şekilde toplanmışsa, uygulama kullanıcı kaydını bu bilgilerle güncelleştirmek için Grafik API'sini kullanabilir. Örnek:

- Bir kullanıcının yetişkin olduğu biliniyorsa, **dizin**özniteliği **ageGroup'u** Yetişkin değeriyle güncelleştirin.
- Bir kullanıcı reşit olmayan biri olarak biliniyorsa, dizin özniteliği **ageGroup'u** **Minor** değeriyle güncelleştirin ve uygun şekilde **onay ını BildirinProvidedForMinor'u**ayarlayın.

DOB verilerinin toplanması hakkında daha fazla bilgi için [bkz.](basic-age-gating.md)

## <a name="capture-terms-of-use-agreement"></a>Kullanım koşullarını yakalama sözleşmesi

Uygulamanızı geliştirdiğinizde, normalde kullanıcıların uygulamaları içinde kullanım koşullarını kullanıcı dizininden hiçbir veya sadece küçük bir katılım olmadan kabul ettiğinizi yakalarsınız. Ancak, bir kullanıcının kullanım koşullarını kabul etmesini toplamak, kabul kabul edilmezse erişimi kısıtlamak ve en son kabul tarihine ve kullanım tarihine bağlı olarak kullanım şartlarında gelecekteki değişikliklerin kabul edilmesine izin vermek için bir Azure AD B2C kullanıcı akışı kullanmak mümkündür. kullanım koşullarının en son sürümü.

**Kullanım Koşulları"nın** "Verileri üçüncü taraflarla paylaşma izni" de içerebilir. Yerel düzenlemelere ve iş kurallarına bağlı olarak, bir kullanıcının her iki koşulu da bir araya getirerek kabul etmesini sağlayabilir veya kullanıcının diğerini değil, bir koşulu kabul etmesine izin verebilirsiniz.

Aşağıdaki adımlar, kullanım koşullarını nasıl yönetebileceğinizi açıklar:

1. Grafik API'sini ve genişletilmiş öznitelikleri kullanarak kullanım koşullarının ve kabul tarihini kaydedin. Bunu hem yerleşik hem de özel kullanıcı akışlarını kullanarak yapabilirsiniz. **extension_termsOfUseConsentDateTime** ve **extension_termsOfUseConsentVersion** öznitelikleri oluşturmanızı ve kullanmanızı öneririz.

2. "Kullanım Koşullarını Kabul Et" etiketli gerekli bir onay kutusu oluşturun ve kayıt sırasında sonucu kaydedin. Bunu hem yerleşik hem de özel kullanıcı akışlarını kullanarak yapabilirsiniz.

3. Azure AD B2C, kullanım koşullarını ve kullanıcının kabul ünlerini depolar. Yanıtı kaydetmek için kullanılan uzantı özniteliğini okuyarak herhangi bir kullanıcının durumunu sorgulamak için Grafik API'sini kullanabilirsiniz (örneğin, **terimleri okuyunOfUseTestUpdateDateTime).** Bunu hem yerleşik hem de özel kullanıcı akışlarını kullanarak yapabilirsiniz.

4. Kabul tarihini kullanım koşullarının en son sürümünün tarihiyle karşılaştırarak güncelleştirilmiş kullanım koşullarının kabul edilmesini gerektirir. Tarihleri yalnızca özel bir kullanıcı akışı kullanarak karşılaştırabilirsiniz. Genişletilmiş özniteliği **extension_termsOfUseConsentDateTime**kullanın ve değeri **termsOfUseTextUpdateDateTime iddiasıyla**karşılaştırın. Kabul eskiyse, kendi kendini öne süren bir ekran görüntüleyerek yeni bir kabul ezorla. Aksi takdirde, ilke mantığını kullanarak erişimi engelleyin.

5. Kabulün sürüm numarasını en son kabul edilen sürüm numarasıyla karşılaştırarak güncelleştirilmiş kullanım koşullarının kabul edilmesini gerektirir. Sürüm numaralarını yalnızca özel bir kullanıcı akışı kullanarak karşılaştırabilirsiniz. Genişletilmiş özniteliği **extension_termsOfUseConsentDateTime**kullanın ve değeri **extension_termsOfUseConsentVersion**iddiasıyla karşılaştırın. Kabul eskiyse, kendi kendini öne süren bir ekran görüntüleyerek yeni bir kabul ezorla. Aksi takdirde, ilke mantığını kullanarak erişimi engelleyin.

Aşağıdaki senaryolar altında kullanım kabul koşullarını yakalayabilirsiniz:

- Yeni bir kullanıcı kaydoluyor. Kullanım koşulları görüntülenir ve kabul sonucu depolanır.
- Bir kullanıcı, en son veya etkin kullanım koşullarını daha önce kabul eden kişilerle oturum açtır. Kullanım koşulları görüntülenmez.
- En son veya etkin kullanım koşullarını zaten kabul etmemiş olan bir kullanıcı oturum açtır. Kullanım koşulları görüntülenir ve kabul sonucu depolanır.
- Bir kullanıcı, kullanım koşullarının artık en son sürüme güncelleştirilen eski bir sürümünü zaten kabul eden kullanıcıyla oturum açmaktadır. Kullanım koşulları görüntülenir ve kabul sonucu depolanır.

Aşağıdaki resim önerilen kullanıcı akışını gösterir:

![Önerilen kabul kullanıcı akışını gösteren akış şeması diyagramı](./media/manage-user-access/user-flow.png)

Aşağıda, bir talepte DateTime tabanlı kullanım izni koşullarına bir örnek verilmiştir:

```xml
<ClaimsTransformations>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentDateTime" TransformationMethod="GetCurrentDateTime">
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="currentDateTime" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequired" TransformationMethod="IsTermsOfUseConsentRequired">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="termsOfUseConsentDateTime" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="termsOfUseTextUpdateDateTime" DataType="dateTime" Value="2098-01-30T23:03:45" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="result" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Aşağıda, bir talepte Sürüm tabanlı kullanım izni koşullarına bir örnek verilmiştir:

```xml
<ClaimsTransformations>
  <ClaimsTransformation Id="GetEmptyTermsOfUseConsentVersionForNewUser" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value=""/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentVersion" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value="V1"/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="compareTo" DataType="string" Value="V1" />
      <InputParameter Id="operator" DataType="string" Value="not equal" />
      <InputParameter Id="ignoreCase" DataType="string" Value="true" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcı verilerini nasıl siler ve [Manage user data](manage-user-data.md)dışa aktarın, bkz.
- Bir kullanım istemi uygulayan örnek bir özel ilke için Bkz. [A B2C IEF Özel İlkesi - 'Kullanım Koşulları' istemiyle Kaydolun ve Oturum Açın.](https://github.com/azure-ad-b2c/samples/tree/master/policies/sign-in-sign-up-versioned-tou)
