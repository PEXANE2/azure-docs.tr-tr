---
title: Koşullu Erişim yalnızca rapor modu nedir? - Azure Etkin Dizin
description: Koşullu Erişim ilkesi dağıtımında yalnızca rapor modu nasıl yardımcı olabilir?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd41e79a1e08c57e806f6ada32faccfa5fdf5792
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295295"
---
# <a name="what-is-conditional-access-report-only-mode"></a>Koşullu Erişim yalnızca rapor modu nedir?

Koşullu Erişim, doğru koşullarda doğru erişim denetimlerini uygulayarak müşterilerimiz tarafından güvenli kalmak için yaygın olarak kullanılır. Ancak, kuruluşunuzda Koşullu Erişim ilkesinin dağıtılmasının zorluklarından biri, son kullanıcılar üzerindeki etkiyi belirlemektir. Eski kimlik doğrulamasını engelleme, kullanıcı nüfusu için çok faktörlü kimlik doğrulaması gerektirme veya oturum açma risk ilkeleri uygulama gibi yaygın dağıtım girişimlerinden etkilenen kullanıcıların sayısını ve adlarını tahmin etmek zor olabilir. 

Yalnızca rapor modu, yöneticilerin ortamlarında etkinleştirmeden önce Koşullu Erişim ilkelerinin etkisini değerlendirmelerine olanak tanıyan yeni bir Koşullu Erişim ilkesi durumudur.  Yalnızca rapor modunun yayınlanmasıyla:

- Koşullu Erişim ilkeleri yalnızca rapor modunda etkinleştirilebilir.
- Oturum açma sırasında, yalnızca rapor modundaki ilkeler değerlendirilir, ancak uygulanmaz.
- Sonuçlar, Oturum Açma günlüğü ayrıntılarının **Koşullu Erişim** ve **Yalnızca Rapor (Önizleme)** sekmelerinde günlüğe kaydedilir.
- Azure Monitör aboneliği olan müşteriler, Koşullu Erişim öngörüleri çalışma kitabını kullanarak Koşullu Erişim ilkelerinin etkisini izleyebilir.

> [!WARNING]
> Uyumlu aygıtlar gerektiren yalnızca rapor modundaki ilkeler, aygıt uyumluluğu zorlanmasa bile, Mac, iOS ve Android'deki kullanıcıları ilke değerlendirmesi sırasında bir aygıt sertifikası seçmeye zorlayabilir. Bu istemler, aygıt uyumlu hale gelene kadar yinelenebilir. Oturum açma sırasında son kullanıcıların istemleri almasını önlemek için, aygıt platformları Mac, iOS ve Android'i aygıt uyumluluk denetimleri gerçekleştiren yalnızca rapor ilkelerinden hariç tutar.

![Azure AD oturum açma günlüğünde yalnızca rapor sekmesi](./media/concept-conditional-access-report-only/report-only-detail-in-sign-in-log.png)

## <a name="policy-results"></a>İlke sonuçları

Yalnızca rapor modundaki bir ilke belirli bir oturum açma için değerlendirildiğinde, dört yeni olası sonuç değeri vardır:

| Sonuç | Açıklama |
| --- | --- |
| Yalnızca Rapor: Başarı | Tüm yapılandırılan ilke koşulları, gerekli etkileşimli olmayan hibe denetimleri ve oturum denetimleri karşılandı. Örneğin, çok faktörlü bir kimlik doğrulama gereksinimi, belirteçte zaten mevcut olan bir MFA talebiyle karşılanır veya uyumlu bir aygıt ilkesi uyumlu bir aygıt üzerinde aygıt denetimi gerçekleştirerek karşılanır. |
| Yalnızca Rapor: Hata | Tüm yapılandırılan ilke koşulları karşılandı, ancak gerekli tüm etkileşimli olmayan hibe denetimleri veya oturum denetimleri yerine getirilmedi. Örneğin, bir ilke, blok denetiminin yapılandırıldığı veya aygıtın uyumlu bir aygıt ilkesinde başarısız olduğu bir kullanıcıya uygulanır. |
| Yalnızca rapor: Kullanıcı eylemi gereklidir | Yapılandırılan tüm ilke koşulları karşılandı, ancak gerekli hibe denetimlerini veya oturum denetimlerini karşılamak için kullanıcı eylemi gerekli olacaktır. Yalnızca rapor moduyla, kullanıcıdan gerekli denetimleri karşılaması istenmez. Örneğin, kullanıcılardan çok faktörlü kimlik doğrulama zorlukları veya kullanım koşulları istenmez.   |
| Yalnızca Rapor: Uygulanmadı | Tüm yapılandırılan ilke koşulları yerine getirilmedi. Örneğin, kullanıcı ilkenin dışında tutulur veya ilke yalnızca belirli güvenilen adlandırılmış konumlar için geçerlidir. |

## <a name="conditional-access-insights-workbook"></a>Koşullu Erişim Öngörüleri çalışma kitabı

Yöneticiler yalnızca rapor modunda birden çok ilke oluşturma yeteneğine sahiptir, bu nedenle hem her ilkenin bireysel etkisini hem de birlikte değerlendirilen birden çok ilkenin birleşik etkisini anlamak gerekir. Yeni Koşullu Erişim Öngörüleri çalışma kitabı, yöneticilerin Koşullu Erişim sorgularını görselleştirmelerine ve belirli bir zaman aralığı, uygulama kümesi ve kullanıcılar için bir ilkenin etkisini izlemelerine olanak tanır. 
 
## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ilkesinde yalnızca rapor modunu yapılandırma](howto-conditional-access-report-only.md)
