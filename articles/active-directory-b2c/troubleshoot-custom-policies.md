---
title: Azure Active Directory B2C'de özel ilkeleri giderme
description: Azure Active Directory B2C'de özel ilkelerle çalışırken hataları çözme yaklaşımları hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186378"
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Azure AD B2C özel ilkeleri ve Kimlik Deneyimi Çerçevesi sorun giderme

Azure Active Directory B2C (Azure AD B2C) özel ilkeleri kullanıyorsanız, Kimlik Deneyimi Çerçevesi'ni ilke dili XML biçiminde ayarlamada güçlükler yaşayabilirsiniz. Özel politikalar yazmayı öğrenmek yeni bir dil öğrenmek gibi olabilir. Bu makalede, sorunları keşfetmenize ve çözmenize yardımcı olabilecek bazı araçlar ve ipuçları açıklıyoruz.

Bu makalede, Azure AD B2C özel ilke yapılandırmanızın sorun giderme sorunu üzerinde duruluyor. Güvenen parti uygulamasına veya kimlik kitaplığına hitap etmez.

## <a name="xml-editing"></a>XML düzenleme

Özel ilkeler ayarlamada en yaygın hata yanlış biçimlendirilmiş XML olduğunu. İyi bir XML editörü neredeyse gereklidir. XML'i yerel olarak görüntüler, renk kodları içeriği, ortak terimleri önceden doldurur, XML öğelerini dizine eksitolarak tutar ve bir XML şemasına karşı doğrulayabilir.

Bizim favori editörler iki [Visual Studio Code](https://code.visualstudio.com/) ve Not Defteri [++](https://notepad-plus-plus.org/)vardır.

XML şema doğrulama, XML dosyanızı yüklemeden önce hataları tanımlar. [Başlatıcı paketinin](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack)kök klasöründe, XML şema tanım dosyası *TrustFrameworkPolicy_0.3.0.0.xsd*olsun. Editörünüzde doğrulama için XSD şema dosyasını nasıl kullanacağınızı öğrenmek için, editörün belgelerinde *XML araçlarını* ve *XML doğrulama* veya benzeri nidaları arayın.

XML kurallarının gözden geçirilmesini yararlı bulabilirsiniz. Azure AD B2C, algıladığınız XML biçimlendirme hatalarını reddeder. Bazen, yanlış biçimlendirilmiş XML yanıltıcı hata iletilerine neden olabilir.

## <a name="upload-policies-and-policy-validation"></a>Yükleme ilkeleri ve ilke doğrulaması

XML ilke dosyasının doğrulanması yükleme de otomatik olarak gerçekleştirilir. Çoğu hata yüklemenin başarısız olmasıyla sonuçlanır. Doğrulama, yüklediğiniz ilke dosyasını içerir. Ayrıca, yükleme dosyasının başladay lık dosyasının başlağitolduğu dosyalar zincirini (güvenen taraf ilkesi dosyası, uzantılar dosyası ve temel dosya) da içerir.

Yaygın doğrulama hataları şunlardır:

> Hata parçacığı:`...makes a reference to ClaimType with id "displayName" but neither the policy nor any of its base policies contain such an element`

* ClaimType değeri yanlış yazılmış olabilir veya şemada yok.
* ClaimType değerleri ilkedeki dosyalardan en az birinde tanımlanmalıdır.
    Örneğin, `<ClaimType Id="issuerUserId">`
* ClaimType uzantıları dosyasında tanımlanmışsa, ancak temel dosyadaki Teknik Profil değerinde de kullanılıyorsa, temel dosyanın yüklenmesi bir hatayla sonuçlanır.

> Hata parçacığı:`...makes a reference to a ClaimsTransformation with id...`

* Bu hatanın nedenleri ClaimType hatasıyla aynı olabilir.

> Hata parçacığı:`Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order to manage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`

* Kiracı Id değerinin ve `<TrustFrameworkPolicy\>` `<BasePolicy\>` öğelerinin hedef Azure AD B2C kiracınızla eşleşip eşleşmediğini kontrol edin.

## <a name="troubleshoot-the-runtime"></a>Çalışma süresini sorun giderme

* **Şimdi Çalıştır'ı** kullanın ve `https://jwt.ms` ilkelerinizi web veya mobil uygulamanızdan bağımsız olarak test edin. Bu web sitesi bir güvenerek parti uygulaması gibi davranır. Azure AD B2C politikanız tarafından oluşturulan JSON web belirteci (JWT) içeriğini görüntüler.

    Belirteç denetimine `https://jwt.ms` yönlendirilebilen bir test uygulaması oluşturmak için:

    [!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

* İstemci tarayıcınız ile Azure AD B2C arasındaki ileti alışverişini izlemek için [Fiddler'ı](https://www.telerik.com/fiddler)kullanın. Bu, düzenleme adımlarınızda kullanıcı yolculuğunuzun nerede başarısız olduğuna dair bir gösterge elde eve yardımcı olabilir.

* **Geliştirme modunda,** Kimlik Deneyimi Çerçevesi kullanıcı yolculuğunuzun etkinliğini izlemek için [Uygulama Öngörüleri'ni](troubleshoot-with-application-insights.md) kullanın. **Geliştirme modunda,** Kimlik Deneyimi Çerçevesi ile kimlik sağlayıcıları, API tabanlı hizmetler, Azure AD B2C kullanıcı dizini ve Azure Çok Faktörlü Kimlik Doğrulama gibi diğer hizmetler gibi teknik profiller tarafından tanımlanan çeşitli talep sağlayıcıları arasındaki talep alışverişini gözlemleyebilirsiniz.

## <a name="recommended-practices"></a>Önerilen uygulamalar

**Senaryolarınızın birden çok versiyonunu saklayın. Uygulamanızla birlikte bir projede gruplayın.** Taban, uzantılar ve güvenerek parti dosyaları doğrudan birbirine bağlıdır. Onları grup olarak kaydedin. İlkelerinize yeni özellikler eklendikçe, ayrı çalışma sürümlerini koruyun. Kendi dosya sisteminizde, etkileşimde oldukları uygulama koduyla çalışma sürümlerini aşamalı olarak düzenleyin. Uygulamalarınız, kiracıda çok farklı güvenilen parti ilkeleri çağırabilir. Azure AD B2C ilkelerinizden bekledikleri iddialara bağımlı hale gelebilirler.

**Bilinen kullanıcı yolculukları ile teknik profiller geliştirin ve test edin.** Teknik profillerinizi ayarlamak için test edilmiş başlangıç paketi ilkelerini kullanın. Kendi kullanıcı yolculuklarınıza dahil etmeden önce bunları ayrı ayrı test edin.

**Test edilmiş teknik profillerle kullanıcı yolculuklarını geliştirin ve test edin.** Kullanıcı yolculuğunun düzenleme adımlarını artımlı olarak değiştirin. Amaçlanan senaryoları aşamalı olarak oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

GitHub'da kullanılabilir, [active-directory-b2c-custom-policy-starterpack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) .zip arşivini indirin. Ayrıca depoyu da klonlayabilirsiniz:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
```
