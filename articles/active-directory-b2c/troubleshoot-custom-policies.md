---
title: Azure Active Directory B2C özel ilkelerle ilgili sorunları giderme
description: Azure Active Directory B2C özel ilkelerle çalışırken hataları çözmeye yönelik yaklaşımlar hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 2f65e98cec04991fe9edef1b81bcb3ecc3d93d76
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78186378"
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Özel ilkelerin ve kimlik deneyimi çerçevesinin Azure AD B2C sorunlarını giderme

Azure Active Directory B2C (Azure AD B2C) özel ilkeleri kullanıyorsanız, kimlik deneyimi çerçevesini ilke dili XML biçiminde ayarlarken sorunlarla karşılaşabilirsiniz. Özel ilkeler yazmak için öğrenme, yeni bir dil öğreniyor olabilir. Bu makalede, sorunları keşfetmenize ve çözmenize yardımcı olabilecek bazı araçlar ve ipuçları anlatılmaktadır.

Bu makale Azure AD B2C özel ilke yapılandırmanızda sorun gidermeye odaklanır. Bağlı olan taraf uygulamasının veya kimlik kitaplığının kimliğini gidermez.

## <a name="xml-editing"></a>XML düzenlemesi

Özel ilkeleri ayarlamadaki en yaygın hata hatalı biçimli XML 'dir. İyi bir XML Düzenleyicisi neredeyse önemlidir. XML yerel olarak, renk kodları içeriği, önceden dolgular Ortak terimleri görüntüler, XML öğelerinin dizinlenmesini önler ve bir XML şemasına göre doğrulayabilir.

En sevdiğiniz düzenleyicilerimizin ikisi [Visual Studio Code](https://code.visualstudio.com/) ve [Notepad + +](https://notepad-plus-plus.org/).

XML şema doğrulaması, XML dosyanızı karşıya yüklemeden önce hataları tanımlar. [Başlangıç paketinin](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack)kök KLASÖRÜNDE, XML şema tanımı dosyası *TrustFrameworkPolicy_0.3.0.0. xsd*' yi alın. Düzenleyicinizde doğrulama için XSD şema dosyasını nasıl kullanacağınızı öğrenmek için, *XML araçları* ve *XML doğrulaması* ' na bakın veya düzenleyicinin belgelerinde benzer.

XML kurallarının yararlı bir incelemesini bulabilirsiniz. Azure AD B2C algıladığı XML biçimlendirme hatalarını reddeder. Bazen yanlış biçimlendirilmiş XML, yanıltıcı bir hata iletisine neden olabilir.

## <a name="upload-policies-and-policy-validation"></a>İlkeleri ve ilke doğrulamasını karşıya yükle

XML ilke dosyasının doğrulanması, karşıya yükleme sırasında otomatik olarak gerçekleştirilir. Çoğu hata yüklemenin başarısız olmasına neden olur. Doğrulama, karşıya yüklediğiniz ilke dosyasını içerir. Ayrıca, karşıya yükleme dosyasının başvurduğu dosya zincirini (bağlı olan taraf ilke dosyası, uzantılar dosyası ve temel dosya) içerir.

Yaygın doğrulama hataları şunları içerir:

> Hata parçacığı:`...makes a reference to ClaimType with id "displayName" but neither the policy nor any of its base policies contain such an element`

* ClaimType değeri yanlış yazılmış veya şemada yok olabilir.
* ClaimType değerlerinin ilkedeki dosyalardan en az birinde tanımlanması gerekir.
    Örneğin, `<ClaimType Id="issuerUserId">`
* ClaimType, uzantılar dosyasında tanımlıysa, ancak temel dosyadaki bir teknisyen dosyasında kullanılıyorsa, temel dosyayı karşıya yüklemek bir hataya neden olur.

> Hata parçacığı:`...makes a reference to a ClaimsTransformation with id...`

* Bu hatanın nedenleri, ClaimType hatası ile aynı olabilir.

> Hata parçacığı:`Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order to manage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`

* `<TrustFrameworkPolicy\>` Ve `<BasePolicy\>` öğelerindeki tenantıd değerinin hedef Azure AD B2C kiracınızla eşleşip eşleştiğinden emin olun.

## <a name="troubleshoot-the-runtime"></a>Çalışma zamanının sorunlarını giderme

* **Şimdi Çalıştır** 'ı kullanın `https://jwt.ms` ve ilkelerinizi Web veya mobil uygulamanızdan bağımsız olarak test edin. Bu web sitesi, bağlı olan taraf uygulaması gibi davranır. Azure AD B2C ilkeniz tarafından oluşturulan JSON Web belirtecinin (JWT) içeriğini görüntüler.

    Belirteç incelemesi `https://jwt.ms` için yeniden yönlendirebilmek üzere bir test uygulaması oluşturmak için:

    [!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

* İstemci tarayıcınızla Azure AD B2C ileti alışverişi izlemek için [Fiddler](https://www.telerik.com/fiddler)'ı kullanın. Bu, Kullanıcı yolculuğunun düzenleme adımlarınızın başarısız olduğunu belirten bir bildirim almanıza yardımcı olabilir.

* **Geliştirme modunda**, kimlik deneyimi çerçevesi Kullanıcı yolculuğunun etkinliğini izlemek için [Application Insights](troubleshoot-with-application-insights.md) kullanın. **Geliştirme modunda**, kimlik deneyimi çerçevesi ile kimlik SAĞLAYıCıLARı, API tabanlı hizmetler, Azure AD B2C Kullanıcı dizini ve Azure Multi-Factor Authentication gibi diğer hizmetler gibi teknik profiller tarafından tanımlanan çeşitli talep sağlayıcıları arasındaki taleplerin değişimini gözlemleyebilirsiniz.

## <a name="recommended-practices"></a>Önerilen uygulamalar

**Senaryolarınızın birden çok sürümünü saklayın. Bunları uygulamanızla birlikte bir projede gruplayın.** Temel, Uzantılar ve bağlı olan taraf dosyaları birbirlerine doğrudan bağlıdır. Onları bir grup olarak kaydedin. İlkelerinize yeni özellikler eklendikçe, ayrı çalışma sürümlerini saklayın. Kendi dosya sisteminizdeki çalışma sürümlerini, etkileşimde bulundukları uygulama koduyla birlikte aşamalandırın. Uygulamalarınız bir kiracıda birçok farklı bağlı olan taraf ilkesini çağırabilir. Azure AD B2C ilkelerinizden bekledikleri taleplere bağlı olabilirler.

**Bilinen Kullanıcı yolculukları ile teknik profiller geliştirin ve test edin.** Teknik profillerinizi ayarlamak için Sınanan Başlatıcı paketi ilkelerini kullanın. Bunları kendi Kullanıcı yolculuğa eklemeden önce ayrı olarak test edin.

**Sınanan teknik profillerle Kullanıcı yolculukları geliştirin ve test edin.** Kullanıcı yolculuğunun düzenleme adımlarını artımlı olarak değiştirin. Amaçlanan senaryolarınızı aşamalı olarak oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

GitHub 'da kullanılabilir, [Active-Directory-B2C-Custom-Policy-starterpack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) . zip arşivini indirin. Depoyu de kopyalayabilirsiniz:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
```
