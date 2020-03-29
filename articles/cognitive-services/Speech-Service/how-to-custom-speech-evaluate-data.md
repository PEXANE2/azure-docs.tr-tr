---
title: Özel Konuşma için doğruluğu değerlendirin - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu belgede, konuşmadan metne modelimizin veya özel modelinizin kalitesini nicel olarak nasıl ölçeceğinizi öğreneceksiniz. Doğruluğu test etmek için ses + insan etiketli transkripsiyon verileri gereklidir ve 30 dakika ile 5 saat arasında temsili ses sağlanmalıdır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: f710b8bfdd4dcfd3b7a63aa0b457036ab7037016
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74806105"
---
# <a name="evaluate-custom-speech-accuracy"></a>Özel Konuşma doğruluğunu değerlendirin

Bu belgede, Microsoft'un metin den metin eki modelinin veya özel modelinizin kalitesini nicel olarak nasıl ölçeceğinizi öğreneceksiniz. Doğruluğu test etmek için ses + insan etiketli transkripsiyon verileri gereklidir ve 30 dakika ile 5 saat arasında temsili ses sağlanmalıdır.

## <a name="what-is-word-error-rate-wer"></a>Sözcük Hata Oranı (WER) nedir?

Model doğruluğunu ölçmek için endüstri standardı *Word Hata Oranı* (WER) 'dir. WER tanıma sırasında tanımlanan yanlış sözcüklerin sayısını sayar, sonra insan etiketli transkriptte sağlanan toplam kelime sayısına bölünür. Son olarak, bu sayı WER hesaplamak için% 100 ile çarpılır.

![WER formülü](./media/custom-speech/custom-speech-wer-formula.png)

Yanlış tanımlanmış sözcükler üç kategoriye ayrılır:

* Ekleme (I): Hipotez dökümüne yanlış eklenen sözcükler
* Silme (D): Hipotez metninde tespit edilemeyen sözcükler
* İkame (S): Referans ve hipotez arasında yer alan sözcükler

Bir örneği aşağıda verilmiştir:

![Yanlış tanımlanmış sözcükler örneği](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>Hataları giderin ve WER'i geliştirin

Cihazınız, aracınız veya ürününüzle kullandığınız modelin kalitesini değerlendirmek için makine tanıma sonuçlarındaki WER'i kullanabilirsiniz. %5-10 oranında bir WER kaliteli olarak kabul edilir ve kullanıma hazırdır. %20'lik bir WER kabul edilebilir, ancak ek eğitim almayı düşünebilirsiniz. %30 veya daha fazla wer düşük kalite sinyalleri ve özelleştirme ve eğitim gerektirir.

Hataların nasıl dağıtıldığı önemlidir. Birçok silme hatasıyla karşılaşıldığında, bunun nedeni genellikle zayıf ses sinyali gücüdür. Bu sorunu gidermek için, kaynağa daha yakın ses verileri toplamanız gerekir. Ekleme hataları, sesin gürültülü bir ortamda kaydedildiği ve çapraz konuşmanın mevcut olabileceği ve tanıma sorunlarına neden olabileceği anlamına gelir. Etki alanına özgü terimlerin yetersiz bir örneği insan etiketli transkripsiyonlar veya ilgili metin olarak sağlandığında, ikame hatalarıyla sık sık karşılaşılır.

Tek tek dosyaları çözümleyerek, hangi hata türünün var olduğunu ve hangi hataların belirli bir dosyaya özgü olduğunu belirleyebilirsiniz. Dosya düzeyindeki sorunları anlama, geliştirmeleri hedeflemenize yardımcı olur.

## <a name="create-a-test"></a>Test oluşturma

Microsoft'un konuşmadan metne taban çizgisi veya eğittiğiniz özel bir modelin kalitesini test etmek isterseniz, doğruluğu değerlendirmek için iki modeli yan yana karşılaştırabilirsiniz. Karşılaştırma WER ve tanıma sonuçlarını içerir. Genellikle, özel bir model Microsoft'un temel modeli ile karşılaştırılır.

Modelleri yan yana değerlendirmek için:

1. Özel Konuşma [portalında](https://speech.microsoft.com/customspeech)oturum açın.
2. Özel **Konuşma > Testi> metinden metne**gidin.
3. **Test Ekle'yi**tıklatın.
4. **Doğruluğu Değerlendir'i**seçin. Teste bir ad, açıklama verin ve ses + insan etiketli transkripsiyon veri setinizi seçin.
5. Test etmek istediğiniz en fazla iki model seçin.
6. **Oluştur'u**tıklatın.

Testiniz başarıyla oluşturulduktan sonra, sonuçları yan yana karşılaştırabilirsiniz.

## <a name="side-by-side-comparison"></a>Yan yana karşılaştırma

Test tamamlandıktan sonra, *başarılı*olarak durum değişikliği ile gösterilir, testdahil her iki model için bir WER numarası bulacaksınız. Test ayrıntısı sayfasını görüntülemek için test adını tıklayın. Bu ayrıntı sayfası, gönderilen veri kümesinden transkripsiyonla birlikte iki modelin tanıma sonuçlarını gösteren veri setinizdeki tüm deyişlenleri listeler. Yan yana karşılaştırmayı denetlemeye yardımcı olmak için ekleme, silme ve değiştirme gibi çeşitli hata türlerini geçişyapabilirsiniz. İnsan etiketli transkripsiyon ve iki konuşma-metin modelinin sonuçlarını gösteren her sütundaki sesi dinleyerek ve tanıma sonuçlarını karşılaştırarak, hangi modelin ihtiyaçlarınızı karşıladığına ve ek eğitim ve iyileştirmelerin nerede olduğuna karar verebilirsiniz. Gerekli.

## <a name="next-steps"></a>Sonraki adımlar

* [Modelinizi eğitme](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
