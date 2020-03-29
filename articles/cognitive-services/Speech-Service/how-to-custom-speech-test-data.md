---
title: Özel Konuşma için test verilerini hazırlama - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Microsoft konuşma tanımanın doğruluğunu sınarken veya özel modellerinizi eğitirken ses ve metin verilerine ihtiyacınız vardır. Bu sayfada, veri türlerini, nasıl kullanılacağını ve bunları yönetmeyi ele alıyoruz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 969c1450966d2754e6e8f00126da52a1e88181fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942706"
---
# <a name="prepare-data-for-custom-speech"></a>Özel Konuşma için veri hazırlama

Microsoft konuşma tanımanın doğruluğunu sınarken veya özel modellerinizi eğitirken ses ve metin verilerine ihtiyacınız vardır. Bu sayfada, veri türlerini, nasıl kullanılacağını ve bunları yönetmeyi ele alıyoruz.

## <a name="data-types"></a>Veri türleri

Bu tablo, kabul edilen veri türlerini, her veri türünün ne zaman kullanılması gerektiğini ve önerilen miktarı listeler. Bir model oluşturmak için her veri türü gerekli değildir. Veri gereksinimleri, bir test oluşturup oluşturmadığınıza veya bir modeli eğitip eğitmenize bağlı olarak değişir.

| Veri türü | Test için kullanılır | Önerilen miktar | Eğitim için kullanılır | Önerilen miktar |
|-----------|-----------------|----------|-------------------|----------|
| [Ses](#audio-data-for-testing) | Evet<br>Görsel denetim için kullanılır | 5+ ses dosyaları | Hayır | Yok |
| [Ses + İnsan etiketli transkriptler](#audio--human-labeled-transcript-data-for-testingtraining) | Evet<br>Doğruluğu değerlendirmek için kullanılır | 0,5-5 saat ses | Evet | 1-1.000 saat ses |
| [İlgili metin](#related-text-data-for-training) | Hayır | Yok | Evet | İlgili metnin 1-200 MB |

Dosyalar bir veri kümesine göre türe göre gruplandırılmalı ve .zip dosyası olarak yüklenmelidir. Her veri kümesi yalnızca tek bir veri türü içerebilir.

> [!TIP]
> Hızlı bir şekilde başlamak için örnek verileri kullanmayı düşünün. <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/sampledata/customspeech" target="_target">Örnek Özel Konuşma verileri <span class="docon docon-navigate-external x-hidden-focus"></span> </a> için bu GitHub deposuna bakın

## <a name="upload-data"></a>Karşıya veri yükleme

Verilerinizi yüklemek için <a href="https://speech.microsoft.com/customspeech" target="_blank">Özel Konuşma <span class="docon docon-navigate-external x-hidden-focus"> </span>portalına </a>gidin. Portaldan, sihirbazı başlatmak ve ilk veri setinizi oluşturmak için **verileri yükle'yi** tıklatın. Verilerinizi yüklemenize izin vermeden önce veri setiniz için bir konuşma veri türü seçmeniz istenir.

![Konuşma Portalı'ndan ses seçin](./media/custom-speech/custom-speech-select-audio.png)

Yüklediğiniz her veri kümesi, seçtiğiniz veri türü için gereksinimleri karşılamalıdır. Verileriniz yüklenmeden önce doğru biçimlendirilmelidir. Doğru biçimlendirilmiş veriler, Özel Konuşma hizmeti tarafından doğru bir şekilde işlenmesini sağlar. Gereksinimler aşağıdaki bölümlerde listelenmiştir.

Veri kümeniz yüklendikten sonra birkaç seçeneğiniz var:

* **Test** sekmesine gidebilir ve yalnızca ses veya ses + insan etiketli transkripsiyon verilerini görsel olarak inceleyebilirsiniz.
* Özel bir model eğitmek için **Eğitim** sekmesine gidebilir ve ses + insan transkripsiyon verilerini veya ilgili metin verilerini kullanabilirsiniz.

## <a name="audio-data-for-testing"></a>Test için ses verileri

Ses verileri, Microsoft'un temel konuşmadan metne veya özel bir modelin doğruluğunu test etmek için en uygun veridir. Ses verilerinin, belirli bir modelin performansıyla ilgili konuşmanın doğruluğunu incelemek için kullanıldığını unutmayın. Bir modelin doğruluğunu ölçmek istiyorsanız, [ses + insan etiketli transkripsiyon verilerini](#audio--human-labeled-transcript-data-for-testingtraining)kullanın.

Ses dosyalarınızın Özel Konuşma ile kullanılmak üzere doğru biçimlendirilmiş olduğundan emin olmak için bu tabloyu kullanın:

| Özellik                 | Değer                 |
|--------------------------|-----------------------|
| Dosya biçimi              | RIFF (WAV)            |
| Örnekleme hızı              | 8.000 Hz veya 16.000 Hz |
| Kanallar                 | 1 (mono)              |
| Ses başına maksimum uzunluk | 2 saat               |
| Örnek format            | PCM, 16-bit           |
| Arşiv biçimi           | .zip                  |
| Maksimum arşiv boyutu     | 2 GB                  |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!TIP]
> Eğitim ve test verileri yüklerken,.zip dosya boyutu 2 GB'ı geçemez. Eğitim için daha fazla veriye ihtiyacınız varsa, bunları birkaç .zip dosyasına bölün ve bunları ayrı olarak yükleyin. Daha sonra, *birden çok* veri kümesinden eğitim seçebilirsiniz. Ancak, yalnızca *tek* bir veri kümesinden sınama yapabilirsiniz.

Ses özelliklerini doğrulamak veya varolan sesi uygun biçimlere dönüştürmek için <a href="http://sox.sourceforge.net" target="_blank" rel="noopener">SoX'u <span class="docon docon-navigate-external x-hidden-focus"></span> </a> kullanın. Aşağıda, bu etkinliklerin her birinin SoX komut satırı üzerinden nasıl yapılabileceğinin bazı örnekleri verilmiştir:

| Etkinlik | Açıklama | SoX komutu |
|----------|-------------|-------------|
| Ses biçimini kontrol edin | Denetlemek için bu komutu kullanın<br>ses dosyası biçimi. | `sox --i <filename>` |
| Ses biçimini dönüştürme | Dönüştürmek için bu komutu kullanın<br>tek kanal, 16-bit, 16 KHz ses dosyası. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Test/eğitim için ses + insan etiketli transkript verileri

Ses dosyalarınızı işlerken Microsoft'un metin konuşma doğruluğunun doğruluğunu ölçmek için karşılaştırma için insan etiketli transkripsiyonlar (kelime kelime) sağlamanız gerekir. İnsan etiketli transkripsiyon genellikle zaman alıcı olsa da, doğruluğu değerlendirmek ve kullanım durumlarınız için modeli eğitmek gerekir. Tanımadaki gelişmelerin yalnızca sağlanan veriler kadar iyi olacağını unutmayın. Bu nedenle, yalnızca yüksek kaliteli transkriptlerin yüklenmesi önemlidir.

| Özellik                 | Değer                               |
|--------------------------|-------------------------------------|
| Dosya biçimi              | RIFF (WAV)                          |
| Örnekleme hızı              | 8.000 Hz veya 16.000 Hz               |
| Kanallar                 | 1 (mono)                            |
| Ses başına maksimum uzunluk | 2 saat (test) / 60 s (eğitim) |
| Örnek format            | PCM, 16-bit                         |
| Arşiv biçimi           | .zip                                |
| Maksimum zip boyutu         | 2 GB                                |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!NOTE]
> Eğitim ve test verileri yüklerken,.zip dosya boyutu 2 GB'ı geçemez. Yalnızca *tek* bir veri kümesinden test edebilirsiniz, uygun dosya boyutu içinde tuttuğunızdan emin olun. Ayrıca, her eğitim dosyası 60 saniyeyi geçemez aksi takdirde hata olur.

Sözcük silme veya değiştirme gibi sorunları gidermek için, tanınmayı geliştirmek için önemli miktarda veri gerekir. Genellikle, yaklaşık 10 ila 1.000 saatlik ses için kelime kelime transkripsiyonları sağlaması önerilir. Tüm WAV dosyalarının transkripsiyonları tek bir düz metin dosyasına yerleştirilmelidir. Transkripsiyon dosyasının her satırında ses dosyalarından birinin adı ve transkripsiyon bulunmalıdır. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır.

  Örnek:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```

> [!IMPORTANT]
> Transkripsiyon UTF-8 bayt sırası işareti (BOM) ile kodlanmış olmalıdır.

Transkripsiyon metinleri sistem tarafından işlenebilmesi için normalleştirilir. Ancak, verileri Konuşma Stüdyosu'na yüklemeden önce yapılması gereken bazı önemli normalleştirmeler vardır. Transkripsiyonlarınızı hazırlarken kullanılacak uygun [How to create a human-labeled transcription](how-to-custom-speech-human-labeled-transcriptions.md) dil için bkz.

Ses dosyalarınızı ve ilgili transkripsiyonlarınızı topladıktan sonra, Özel Konuşma <a href="https://speech.microsoft.com/customspeech" target="_blank">portalına <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>yüklemeden önce bunları tek bir .zip dosyası olarak paketleyin. Aşağıda üç ses dosyası ve insan etiketli transkripsiyon dosyası içeren örnek bir veri kümesi verilmiştir:

> [!div class="mx-imgBorder"]
> ![Konuşma Portalı'ndan ses seçin](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Eğitim için ilgili metin verileri

Benzersiz ürün adları veya özellikleri, eğitim için ilgili metin verilerini içermelidir. İlgili metin doğru tanıma sağlamaya yardımcı olur. Tanımayı geliştirmek için iki tür ilgili metin verisi sağlanabilir:

| Veri türü | Bu veriler tanımayı nasıl iyileştirir? |
|-----------|------------------------------------|
| Cümleler (söz) | Ürün adlarını veya bir cümle bağlamında sektöre özgü kelime dağarcığını tanımada doğruluğu artırın. |
| Telaffuz | Sık rastlanmayan terimlerin, kısaltmaların veya tanımlanmamış telaffuzlarla başka sözcüklerin telaffuzunu geliştirin. |

Cümleler tek bir metin dosyası veya birden çok metin dosyası olarak sağlanabilir. Doğruluğu artırmak için, beklenen konuşulan dile retlere daha yakın metin verilerini kullanın. Telaffuzlar tek bir metin dosyası olarak sağlanmalıdır. Her şey tek bir zip dosyası olarak paketlenebilir ve <a href="https://speech.microsoft.com/customspeech" target="_blank">Özel Konuşma portalına <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>yüklenebilir.

### <a name="guidelines-to-create-a-sentences-file"></a>Cümleler dosyası oluşturma yönergeleri

Cümleleri kullanarak özel bir model oluşturmak için örnek sözcüklerin bir listesini sağlamanız gerekir. Söyleyişlerin _do not_ tam veya dilbilgisi açısından doğru olması gerekmez, ancak üretimde beklediğiniz sözlü girdiyi doğru bir şekilde yansıtmaları gerekir. Belirli terimlerin ağırlığının artmasını istiyorsanız, bu belirli terimleri içeren birkaç cümle ekleyin.

Genel kılavuz olarak, eğitim metni üretimde beklenen gerçek metne mümkün olduğunca yakın olduğunda model uyarlaması en etkili olur. Geliştirmeyi hedeflediğiniz etki alanına özgü jargon ve ifadeler eğitim metnine eklenmelidir. Mümkün olduğunda, ayrı bir satırda bir tümceveya anahtar kelimenin denetlenen olmasını deneyin. Sizin için önemli olan anahtar kelimeler ve tümcecikler (örneğin, ürün adları) için bunları birkaç kez kopyalayabilirsiniz. Ama unutmayın, çok fazla kopyalamak yok - bu genel tanıma oranını etkileyebilir.

Bu tabloyu kullanarak ilgili veri dosyanızın doğru biçimlendirilmiş olduğundan emin olun:

| Özellik | Değer |
|----------|-------|
| Metin kodlaması | UTF-8 BOM |
| Satır başına konuşma sayısı | 1 |
| En büyük dosya boyutu | 200 MB |

Ayrıca, aşağıdaki kısıtlamaları da hesaba katmak isteyebilirsiniz:

* Karakterleri dört defadan fazla yinelemekten kaçının. Örneğin: "aaaa" veya "uuuu".
* Yukarıda `U+00A1`özel karakterler veya UTF-8 karakterleri kullanmayın.
* ÜRB'ler reddedilecektir.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Telaffuz dosyası oluşturmak için yönergeler

Kullanıcılarınızın karşılaşacağı veya kullanacağı standart telaffuzlar içermeyen nadir terimler varsa, tanımayı geliştirmek için özel bir telaffuz dosyası sağlayabilirsiniz.

> [!IMPORTANT]
> Ortak sözcüklerin telaffuzunu değiştirmek için özel telaffuz dosyaları nın kullanılması önerilmez.

Bu, konuşulan bir sözcük ve her biri için özel bir telaffuz örneklerini içerir:

| Tanınan/görüntülenen form | Sözlü form |
|--------------|--------------------------|
| 3CPO | üç c p o |
| CNTK | c n t k |
| ıeee | i üçlü e |

Konuşulan form, ifade edilen fonetik dizidir. Bu harf, kelime, hece, ya da her üç bir arada oluşur.

Özelleştirilmiş telaffuz İngilizce ( )`en-US`ve Almanca`de-DE`( ) olarak mevcuttur. Bu tablo, dil lerine göre desteklenen karakterleri gösterir:

| Dil | Yerel Ayar | Karakterler |
|----------|--------|------------|
| Türkçe | `en-US` | `a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |
| Almanca | `de-DE` | `ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |

Telaffuzlar için ilgili veri dosyanızın doğru biçimlendirilmiş olduğundan emin olmak için aşağıdaki tabloyu kullanın. Telaffuz dosyaları küçüktür ve yalnızca birkaç kilobayt boyutunda olmalıdır.

| Özellik | Değer |
|----------|-------|
| Metin kodlaması | UTF-8 BOM (ANSI İngilizce olarak da desteklenir) |
| # satır başına telaffuz | 1 |
| En büyük dosya boyutu | 1 MB (ücretsiz katman için 1 KB) |

## <a name="next-steps"></a>Sonraki adımlar

* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitme](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)
