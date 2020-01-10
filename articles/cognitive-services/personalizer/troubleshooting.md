---
title: Sorun giderme-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Bu makale, kişiselleştirici hakkında sık sorulan sorun giderme sorularına yanıtlar içerir.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: diberry
ms.openlocfilehash: 5aeda9abcebda50cf97e1473b458d8f1f9d15970
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75832171"
---
# <a name="personalizer-troubleshooting"></a>Kişiselleştirici sorunlarını giderme

Bu makale, kişiselleştirici hakkında sık sorulan sorun giderme sorularına yanıtlar içerir.

## <a name="transaction-errors"></a>İşlem hataları

### <a name="i-get-an-http-429-too-many-requests-response-from-the-service-what-can-i-do"></a>Hizmetten bir HTTP 429 (çok fazla istek) yanıtı alıyorum. Ne yapabilirim?

Kişiselleştirici örneğini oluştururken ücretsiz bir fiyat katmanı oluşturduysanız, izin verilen derecelendirme isteği sayısında bir kota sınırı vardır. Derecelendirme API 'SI için API çağrı hızınızı gözden geçirin (kişisel kaynağınız için Azure portal ölçümler bölmesinde) ve çağrı biriminiz seçili fiyatlandırma katmanı eşiğinin ötesinde artması bekleniyorsa fiyatlandırma katmanını (Fiyatlandırma Katmanı bölmesinde) ayarlayın.

### <a name="im-getting-a-5xx-error-on-rank-or-reward-apis-what-should-i-do"></a>Derecelendirmek veya yeniden geçirdim API 'Lerinde 5 xx hatası alıyorum. Ne yapmam gerekir?

Bu sorunlar saydam olmalıdır. Devam ederse, kişiselleştirme kaynağınız için Azure portal **destek + sorun giderme** bölümünde **Yeni destek isteği** ' ni seçerek desteğe başvurun.


## <a name="learning-loop"></a>Öğrenme döngüsü

<!--

### How do I import a learning policy?


-->

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Öğrenme döngüsü öğreniyor gibi görünüyor. Bunu nasıl düzeltirim?

Öğrenme döngüsünün, derece çağrıları etkin bir şekilde önceliklendirilmesi için birkaç bin yeniden çağrısı gerekir.

Öğrenme döngünüz Şu anda çalışır durumda olduğundan emin değilseniz, [çevrimdışı bir değerlendirme](concepts-offline-evaluation.md)çalıştırın ve düzeltilen öğrenme ilkesini uygulayın.

### <a name="i-keep-getting-rank-results-with-all-the-same-probabilities-for-all-items-how-do-i-know-personalizer-is-learning"></a>Tüm öğeler için aynı olasılıkların bulunduğu derece sonuçları almaya devam ediyorum. Kişiselleştirici öğreneceğimizi Nasıl yaparım? mı?

Kişiselleştirici, yeni başlayan ve _boş_ bir modeli olan BIR derece API sonucu için aynı olasılıkların sonucunu döndürür ya da kişiselleştirici döngüsünü sıfırladığınızda ve modeliniz hala **model güncelleştirme sıklığı** döneminde.

Yeni güncelleştirme dönemi başladığında, güncelleştirilmiş model kullanılır ve bu da olasılıkların değişikliğini görürsünüz.

### <a name="the-learning-loop-was-learning-but-seems-to-not-learn-anymore-and-the-quality-of-the-rank-results-isnt-that-good-what-should-i-do"></a>Öğrenme döngüsü öğreniyor ancak artık bilgi vermiyor gibi görünüyor ve sıra sonuçlarının kalitesi iyi değil. Ne yapmam gerekir?

* Bu kişiselleştirici kaynak (öğrenme döngüsü) için Azure portal bir değerlendirmeyi tamamladığınızdan ve uyguladığınızdan emin olun.
* Tüm yeniden temeller, Reward API aracılığıyla gönderildiğinden ve işlendiğinizden emin olun.

### <a name="how-do-i-know-that-the-learning-loop-is-getting-updated-regularly-and-is-used-to-score-my-data"></a>Nasıl yaparım? öğrenme döngüsünün düzenli olarak güncelleştirildiğini ve verilerime puan vermek için kullanıldığını öğrenmek mi istiyorsunuz?

Modelin en son ne zaman güncelleştirildiğini Azure portal **model ve öğrenme ayarları** sayfasında bulabilirsiniz. Eski bir zaman damgası görürseniz, bu durum büyük olasılıkla derecelendirme ve yeniden çağrıları göndermemelidir. Hizmetin gelen verileri yoksa Öğrenimini güncelleştirmez. Öğrenme döngüsünün yeterince sık güncelleştirilmediğinden, döngünün **model güncelleştirme sıklığını**düzenleyebilirsiniz.


## <a name="offline-evaluations"></a>Çevrimdışı değerlendirmeler

### <a name="an-offline-evaluations-feature-importance-returns-a-long-list-with-hundreds-or-thousands-of-items-what-happened"></a>Çevrimdışı değerlendirme özelliğinin önem derecesi yüzlerce veya binlerce öğe içeren uzun bir liste döndürür. Ne oldu?

Bu, genellikle zaman damgalarının, kullanıcı kimliklerinin veya diğer bazı ayrıntılı özelliklerin gönderilmesi nedeniyle yapılır.

### <a name="i-created-an-offline-evaluation-and-it-succeeded-almost-instantly-why-is-that-i-dont-see-any-results"></a>Çevrimdışı bir değerlendirme oluşturdum ve neredeyse anında başarılı oldu. Bunun nedeni nedir? Hiç sonuç görmüyorum mi?

Çevrimdışı değerlendirme, bu dönemdeki olaylardaki eğitilen model verilerini kullanır. Değerlendirmenin başlangıç ve bitiş saati arasındaki zaman diliminde herhangi bir veri göndermediyseniz, bu işlem herhangi bir sonuç olmadan tamamlanır. Kişiselleştirmede bildiğiniz bir olay ile zaman aralığı seçerek yeni bir çevrimdışı değerlendirme gönderebilirsiniz.

## <a name="learning-policy"></a>Öğrenme ilkesi

### <a name="how-do-i-import-a-learning-policy"></a>Bir öğrenme ilkesini içeri Nasıl yaparım? mı?

[Öğrenme ilkesi kavramları](concept-active-learning.md#understand-learning-policy-settings) ve yeni bir öğrenme ilkesinin [nasıl uygulanacağı](how-to-learning-policy.md) hakkında daha fazla bilgi edinin. Bir öğrenme ilkesi seçmek istemiyorsanız, geçerli olaylarınız temelinde bir öğrenme ilkesi önermek için [çevrimdışı değerlendirmeyi](how-to-offline-evaluation.md) kullanabilirsiniz.


## <a name="security"></a>Güvenlik

### <a name="the-api-key-for-my-loop-has-been-compromised-what-can-i-do"></a>Döngülerimin API anahtarı tehlikeye geçti. Ne yapabilirim?

Diğer anahtarı kullanmak için istemcilerinizi değiştirdikten sonra bir anahtarı yeniden oluşturabilirsiniz. İki anahtara sahip olmak, herhangi bir kesinti olması gerekmeden anahtarı geç bir biçimde yayalmanıza olanak sağlar. Bunu bir güvenlik önlemi olarak düzenli bir döngüde yapmanızı öneririz.


## <a name="next-steps"></a>Sonraki adımlar

[Model güncelleştirme sıklığını yapılandırma](how-to-settings.md#model-update-frequency)