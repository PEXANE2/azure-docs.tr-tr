---
title: Özel Konuşma Tanıma konuşma hizmeti için bir modeli geliştirme
titleSuffix: Azure Cognitive Services
description: İnsan tarafından etiketlenmiş belirli tür dökümler ve ilgili metinler, konuşma senaryosuna dayalı olarak konuşmadan metne bir model için tanıma doğruluğunu iyileştirebilir.
services: cognitive-services
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: v-demjoh
ms.openlocfilehash: bb904482f3cb5900b724803816269f1b10ab3720
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727883"
---
# <a name="improve-custom-speech-accuracy"></a>Özel Konuşma Tanıma doğruluğu geliştirme

Bu makalede, ses, insan etiketli dökümü ve ilgili metni ekleyerek özel modelinizin kalitesini nasıl artıracağınızı öğreneceksiniz.

## <a name="accuracy-in-different-scenarios"></a>Farklı senaryolarda doğruluk

Konuşma tanıma senaryoları ses kalitesine ve dile (sözlük ve konuşma stili) göre farklılık gösterir. Aşağıdaki tabloda dört yaygın senaryo incelenir:

| Senaryo | Ses kalitesi | Sözlük | Konuşma stili |
|----------|---------------|------------|----------------|
| Çağrı merkezi | Düşük, 8 kHz, 1 ses kanalında 2 insana olabilir, sıkıştırılabilir | Dar, benzersiz etki alanı ve ürünler | Konuşma, gevşek yapılandırılmış |
| Ses Yardımcısı (Cortana veya bir sürücü aracılığıyla bir pencere) | Yüksek, 16 kHz | Varlık ağır (şarkı başlıkları, ürünler, konumlar) | Açıkça belirtilen sözcükler ve ifadeler |
| Dikte (anında ileti, notlar, arama) | Yüksek, 16 kHz | Değiştirilecek | Göz önünde bulundurularak |
| Video kapalı açıklamalı altyazı | Değişen mikrofon kullanımı, müzik eklendi gibi değişen | Toplantılar, yeniden dağıtılmış konuşma, müzik sözleri | Okuma, hazır veya gevşek yapılandırılmış |

Farklı senaryolar farklı kalite sonuçları üretir. Aşağıdaki tabloda, bu dört senaryodan alınan içeriğin, [sözcük hata oranı 'nda (WER)](how-to-custom-speech-evaluate-data.md)nasıl hızlarının yapılacağı incelenir. Tablo, her senaryo için en yaygın olarak kullanılan hata türlerini gösterir.

| Senaryo | Konuşma tanıma kalitesi | Ekleme hataları | Silme hataları | Değiştirme hataları |
|----------|----------------------------|------------------|-----------------|---------------------|
| Çağrı merkezi | Orta (<% 30 WER) | Düşük, diğer kişilerin arka planda konuştuğu durumlar dışında | Yüksek olabilir. Çağrı merkezleri gürültülü olabilir ve çakışan hoparlörler modeli karıştırabilirler | Orta. Ürünlerin ve kişilerin adları bu hatalara neden olabilir |
| Ses asistanı | Yüksek (<% 10 WER) | Düşük | Düşük | Orta, şarkı başlıkları, ürün adları veya konumlar nedeniyle |
| Etme | Yüksek (<% 10 WER) | Düşük | Düşük | Yüksek |
| Video kapalı açıklamalı altyazı | Video türüne bağlıdır (<% 50) | Düşük | Müzik, önemli, mikrofon kalitesi nedeniyle yüksek olabilir | Jargon bu hatalara neden olabilir |

WER bileşenlerinin belirlenmesi (ekleme, silme ve değiştirme hatalarının sayısı) modeli geliştirmek için hangi tür verilerin ekleneceğini belirlemeye yardımcı olur. Temel bir modelin kalitesini görüntülemek için [özel konuşma tanıma portalını](https://speech.microsoft.com/customspeech) kullanın. Portal, WER kalite ücretine bağlı ekleme, değiştirme ve silme hata oranlarını raporlar.

## <a name="improve-model-recognition"></a>Model tanımayı geliştirme

[Özel konuşma tanıma portalına](https://speech.microsoft.com/customspeech)eğitim verileri ekleyerek, tanınma hatalarını azaltabilirsiniz. 

Kaynak malzemeleri düzenli aralıklarla ekleyerek özel modelinizin korunmasını planlayın. Özel modelinize, varlıklarınızda yapılan değişiklikleri haberdar etmek için ek eğitim gerekir. Örneğin, ürün adlarına, şarkı adlarına veya yeni hizmet konumlarına yönelik güncelleştirmelere ihtiyacınız olabilir.

Aşağıdaki bölümlerde her bir ek eğitim verisinin hata düzeyini nasıl azaltabileceğini açıklamaktadır.

### <a name="add-related-text-sentences"></a>İlgili metin cümleleri ekleyin

Ek ilgili metin cümleleri öncelikle, ortak sözcüklerin ve etki alanına özgü sözcüklerin yanlış tanınmasına ilişkin değiştirme hatalarını, bunları bağlamında göstererek azaltabilirsiniz. Etki alanına özgü sözcükler seyrek olabilir veya bir sözcük oluşturabilir, ancak telaffuz tanınmak için basit olmalıdır.

> [!NOTE]
> Tanınmayan karakterler veya sözcükler gibi gürültü içeren ilgili metin cümlelerini önleyin.

### <a name="add-audio-with-human-labeled-transcripts"></a>İnsan etiketli yazılı betikler ile ses ekleme

İnsan etiketli dökümü olan ses, ses hedef kullanım durumundan geliyorsa en büyük doğruluk geliştirmelerini sunar. Örnekler, tam konuşma kapsamını kapsamalıdır. Örneğin, bir perakende mağaza için çağrı merkezi, yaz ayları sırasında swimwear ve güneşlük hakkındaki çağrıların çoğunu alır. Örneğin, algılamak istediğiniz tam konuşma kapsamını içerdiğinden emin olmanız gerekir.

Bu ayrıntıları göz önünde bulundurun:

* Özel Konuşma Tanıma, ekleme veya silme hatalarını değil, değiştirme hatalarını azaltmak için yalnızca kelime bağlamını yakalayabilir.
* Döküm hataları içeren örneklerden kaçının, ancak ses kalitesi çeşitliliğe sahiptir.
* Sorun etki alanınız ile ilgili olmayan cümlelerden kaçının. İlişkisiz cümleler modelinize zarar verebilir.
* Döküm kalitesi farklılık gösterir, ağırlığın artması için, hariç tutulan ve çok iyi cümleler (anahtar tümceleri içeren harika döküm gibi) çoğaltabilirsiniz.

### <a name="add-new-words-with-pronunciation"></a>Telaffuz ile yeni sözcükler ekleme

Oluşturulan veya yüksek ölçüde özelleştirilmiş kelimelerin benzersiz söylenişleri olabilir. Bu sözcükler, sözcüğün pronounce daha küçük bir sözcüğe bölüneceği durumlarda tanınabilmesi için tanınabilir. Örneğin, **Xbox**, Pronounce as **X kutusunu**tanımak için. Bu yaklaşım genel doğruluğu artırmaz, ancak bu anahtar sözcüklerin tanınmasını artırabilir.

> [!NOTE]
> Bu teknik, şu anda yalnızca bazı diller için kullanılabilir. Ayrıntılar için [konuşmayı metne](language-support.md) dönüştürme bölümüne bakın.

## <a name="sources-by-scenario"></a>Senaryoya göre kaynaklar

Aşağıdaki tabloda, ses tanıma senaryoları gösterilmektedir ve yukarıda listelenen üç eğitim içeriği kategorisi dahilinde göz önünde bulundurmanız gereken kaynak malzemeler listelenmiştir.

| Senaryo | İlgili metin cümleleri | Ses + insan etiketli yazılı betikler | Söylenişi olan yeni sözcükler |
|----------|------------------------|------------------------------|------------------------------|
| Çağrı merkezi             | pazarlama belgeleri, Web sitesi, Çağrı Merkezi etkinliğiyle ilgili ürün incelemeleri | Arama Merkezi, insanların yaptığı çağrıları | belirsiz söylenişleri olan terimler (yukarıdaki Xbox 'a bakın) |
| Ses asistanı         | komutların ve varlıkların tüm birleşimlerini kullanarak cümleleri listeleyin | Seslendirmeyi bir cihaza kaydetme ve metin yerleştirme | benzersiz söylenişleri olan adlar (Filmler, şarkılar, ürünler) |
| Etme               | anlık iletiler veya e-postalar gibi yazılı giriş | yukarıdakine benzer | yukarıdakine benzer |
| Video kapalı açıklamalı altyazı | TV görüntüleme betikleri, Filmler, pazarlama içeriği, video özetleri | videoların tam yazılı betikleri | yukarıdakine benzer |

## <a name="next-steps"></a>Sonraki adımlar

- [Modelinizi eğitme](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

- [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)