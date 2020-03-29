---
title: Özel Konuşma için bir model eğitin - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne modelini eğitmek, Microsoft'un temel modeli veya özel bir model için tanıma doğruluğunu artırabilir. Bir model insan etiketli transkripsiyonlar ve ilgili metin kullanılarak eğitilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: a2bc39a35299f56ba52a0143ce123560bd4d88fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77137763"
---
# <a name="train-a-model-for-custom-speech"></a>Özel Konuşma için bir model eğitin

Konuşmadan metne modelini eğitmek, Microsoft'un temel modeliiçin tanıma doğruluğunu artırabilir. Bir model insan etiketli transkripsiyonlar ve ilgili metin kullanılarak eğitilir. Bu veri kümeleri, daha önce yüklenen ses verileriyle birlikte, sözcükleri, ifadeleri, kısaltmaları, adları ve diğer ürüne özgü terimleri tanıyacak şekilde konuşma-metin modelini hassaslaştırmak ve eğitmek için kullanılır. Sağladığınız daha fazla etki alanı içi veri kümeleri (kullanıcıların ne söyleyeceği ve neleri tanımasını beklediğiniz ile ilgili veriler), modeliniz o kadar doğru olur ve bu da daha iyi tanıma yla sonuçlanır. İlişkisiz verileri eğitiminize aktararak modelinizin doğruluğunu azaltabileceğinizi veya zarar verebileceğinizi unutmayın.

## <a name="use-training-to-resolve-accuracy-issues"></a>Doğruluk sorunlarını çözmek için eğitimi kullanma

Modelinizde tanıma sorunlarıyla karşılaşıyorsanız, ek eğitim için insan etiketli transkriptler ve ilgili verileri kullanmak doğruluğu artırmaya yardımcı olabilir. Sorununuzu gidermek için hangi veri kümesini kullanacağınızı belirlemek için bu tabloyu kullanın:

| Kullanım örneği | Veri türü |
| -------- | --------- |
| Tıbbi terminoloji veya BT jargonu gibi sektöre özgü kelime ve dilbilgisi üzerindeki tanıma doğruluğunu artırın. | İlgili metin (cümleler/söyleyişler) |
| Ürün adları veya kısaltmalar gibi standart olmayan telaffuzu olan bir sözcüğün veya terimin fonetik ve görüntülenen biçimini tanımlayın. | İlgili metin (telaffuz) |
| Konuşma stilleri, aksanveya belirli arka plan gürültülerinde tanıma doğruluğunu artırın. | Ses + insan etiketli transkriptler |

> [!IMPORTANT]
> Veri seti yüklemediyseniz, lütfen [verilerinizi hazırlayın ve test edin.](how-to-custom-speech-test-data.md) Bu belge, veri yüklemek için yönergeler ve yüksek kaliteli veri kümeleri oluşturmak için yönergeler sağlar.

## <a name="train-and-evaluate-a-model"></a>Modeli eğitme ve değerlendirme

Bir modeli eğitmenin ilk adımı eğitim verilerini yüklemektir. İnsan etiketli transkripsiyonları ve ilgili metinleri (sözler ve telaffuzlar) hazırlamak için [verilerinizi](how-to-custom-speech-test-data.md) adım adım hazırlayın ve test edin. Eğitim verilerini yükledikten sonra, modelinizi eğitmeye başlamak için aşağıdaki yönergeleri izleyin:

1. Özel Konuşma [portalında](https://speech.microsoft.com/customspeech)oturum açın.
2. **Konuşmadan metne > Özel Konuşma > Eğitimine**gidin.
3. **Tren modeline**tıklayın.
4. Ardından, eğitimbir **Ad** ve **Açıklama**verin.
5. Senaryo **ve Taban çizgisi modeli** açılır menüsünden etki alanınıza en uygun senaryoyu seçin. Hangi senaryoyu seçeceğiniz emin değilseniz **Genel'i**seçin. Temel model eğitim için başlangıç noktasıdır. Bir tercihiniz yoksa, en son kullanın.
6. Eğitim **verilerini seç** sayfasından, eğitim için kullanmak istediğiniz bir veya birden çok ses + insan etiketli transkripsiyon veri kümelerini seçin.
7. Eğitim tamamlandıktan sonra, yeni eğitilmiş model üzerinde doğruluk testi yapmayı seçebilirsiniz. Bu adım isteğe bağlıdır.
8. Özel modelinizi oluşturmak için **Oluştur'u** seçin.

Eğitim tablosu, bu yeni oluşturulan modele karşılık gelen yeni bir giriş görüntüler. Tablo da durumu görüntüler: İşleme, Başarılı, Başarısız.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Eğitimli bir modelin doğruluğunu değerlendirme

Verileri inceleyebilir ve bu belgeleri kullanarak model doğruluğunu değerlendirebilirsiniz:

- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)

Doğruluğu test etmeyi seçtiyseniz, modelin performansını gerçekçi bir şekilde anlamak için modelinizde kullandığınızdan farklı bir akustik veri kümesi seçmek önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

- [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
- [Modelinizi eğitme](how-to-custom-speech-train-model.md)
