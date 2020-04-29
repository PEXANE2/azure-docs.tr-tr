---
title: Özel Konuşma Tanıma konuşma hizmeti için veri hazırlama
titleSuffix: Azure Cognitive Services
description: Microsoft konuşma tanımanın doğruluğunu test ederken veya özel modellerinizi eğitmek için ses ve metin verileri gerekir. Bu sayfada, veri türlerini ve bunların nasıl kullanılacağını ve bunların nasıl yönetileceğini ele aldık.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 78857709447f99895c36f23d8760f44f8468ba7c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81402135"
---
# <a name="prepare-data-for-custom-speech"></a>Özel Konuşma için verileri hazırlama

Microsoft konuşma tanımanın doğruluğunu test ederken veya özel modellerinizi eğitmek için ses ve metin verileri gerekir. Bu sayfada, veri türlerini ve bunların nasıl kullanılacağını ve bunların nasıl yönetileceğini ele aldık.

## <a name="data-types"></a>Veri türleri

Bu tabloda, kabul edilen veri türleri, her veri türü ne zaman kullanılmalı ve önerilen miktar listelenir. Her veri türü bir model oluşturmak için gerekmez. Veri gereksinimleri, bir modeli test etme veya eğitim oluşturmaya bağlı olarak değişir.

| Veri türü | Test için kullanılan | Önerilen miktar | Eğitim için kullanılır | Önerilen miktar |
|-----------|-----------------|----------|-------------------|----------|
| [Ses](#audio-data-for-testing) | Yes<br>Görsel inceleme için kullanılır | 5 + ses dosyası | Hayır | Yok |
| [Ses + ınsan etiketli yazılı betikler](#audio--human-labeled-transcript-data-for-testingtraining) | Yes<br>Doğruluğu değerlendirmek için kullanılır | 0,5-5 saat ses | Yes | 1-1000 saat ses |
| [İlgili metin](#related-text-data-for-training) | Hayır | Yok | Yes | 1-200 MB ilgili metin |

Dosyalar bir veri kümesine türlerine göre gruplanmalı ve bir. zip dosyası olarak karşıya yüklenir. Her veri kümesi yalnızca tek bir veri türü içerebilir.

> [!TIP]
> Hızlı bir şekilde başlamak için örnek verileri kullanmayı göz önünde bulundurun. <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/sampledata/customspeech" target="_target">Örnek özel konuşma tanıma verileri <span class="docon docon-navigate-external x-hidden-focus"></span> </a> için bu GitHub deposuna bakın

## <a name="upload-data"></a>Karşıya veri yükleme

Verilerinizi karşıya yüklemek için <a href="https://speech.microsoft.com/customspeech" target="_blank">özel konuşma tanıma portalına <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>gidin. Portalda **verileri karşıya yükle** ' ye tıklayarak Sihirbazı başlatın ve ilk veri kümenizi oluşturun. Verilerinizi karşıya yüklemeye izin vermeden önce veri kümeniz için bir konuşma veri türü seçmeniz istenir.

![Konuşma portalından ses seçme](./media/custom-speech/custom-speech-select-audio.png)

Karşıya yüklediğiniz her veri kümesinin seçtiğiniz veri türü için gereksinimleri karşılaması gerekir. Verilerinizin karşıya yüklenebilmesi için doğru şekilde biçimlendirilmesi gerekir. Doğru biçimli veriler, Özel Konuşma Tanıma hizmeti tarafından doğru şekilde işlenmek üzere olur. Gereksinimler aşağıdaki bölümlerde listelenmiştir.

Veri kümeniz karşıya yüklendikten sonra, birkaç seçeneğiniz vardır:

* **Test** sekmesine gidebilir ve yalnızca ses veya sesli + insan etiketli döküm verilerini görsel olarak inceleyebilirsiniz.
* Özel bir modeli eğitebilmeniz için **eğitim** sekmesine gidebilir ve ses + insan dökümü verilerini veya ilgili metin verilerini kullanabilirsiniz.

## <a name="audio-data-for-testing"></a>Test için ses verileri

Ses verileri, Microsoft 'un temel konuşma konuşmadan metin modelinin veya özel bir modelin doğruluğunu test etmek için idealdir. Ses verilerinin, belirli bir modelin performansına göre konuşma doğruluğunu denetlemek için kullanılması gerektiğini aklınızda bulundurun. Bir modelin doğruluğunu daha fazla bulmak istiyorsanız, [Ses + insan etiketli Döküm verileri](#audio--human-labeled-transcript-data-for-testingtraining)' ni kullanın.

Ses dosyalarınızın Özel Konuşma Tanıma ile kullanım için doğru biçimlendirildiğinden emin olmak için bu tabloyu kullanın:

| Özellik                 | Değer                 |
|--------------------------|-----------------------|
| Dosya biçimi              | RIFF (WAV)            |
| Örnekleme hızı              | 8.000 Hz veya 16.000 Hz |
| Kanallar                 | 1 (mono)              |
| Ses başına maksimum uzunluk | 2 saat               |
| Örnek biçim            | PCM, 16 bit           |
| Arşiv biçimi           | .zip                  |
| Maksimum Arşiv boyutu     | 2 GB                  |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!TIP]
> Eğitim ve test verileri yüklenirken. zip dosyası boyutu 2 GB 'ı aşamaz. Eğitim için daha fazla veri gerekiyorsa, bunu birkaç. zip dosyasına bölün ve ayrı olarak karşıya yükleyin. Daha sonra, *birden çok* veri kümesinden eğiteyi seçebilirsiniz. Ancak yalnızca *tek* bir veri kümesinden test edebilirsiniz.

Ses özelliklerini doğrulamak veya var olan sesleri uygun biçimlere dönüştürmek için <a href="http://sox.sourceforge.net" target="_blank" rel="noopener">Sox <span class="docon docon-navigate-external x-hidden-focus"></span> </a> kullanın. Aşağıda, bu etkinliklerin her birinin SoX komut satırı aracılığıyla nasıl yapılabileceği hakkında bazı örnekler verilmiştir:

| Etkinlik | Açıklama | SoX komutu |
|----------|-------------|-------------|
| Ses biçimini denetle | Denetlemek için bu komutu kullanın<br>ses dosyası biçimi. | `sox --i <filename>` |
| Ses biçimini Dönüştür | Dönüştürmek için bu komutu kullanın<br>ses dosyasını tek kanala, 16 bit, 16 KHz. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Ses + insanlar etiketli, test/eğitim için yazılı Döküm verileri

Ses dosyalarınızı işlerken Microsoft 'un konuşmadan metne doğruluğu için doğruluğu ölçmek amacıyla, karşılaştırma için insan etiketli dökümlere (Word-Word) sağlamanız gerekir. İnsanlar etiketli döküm genellikle zaman alıcı olsa da doğruluğu değerlendirmek ve modeli kullanım durumlarınız için eğitmek gerekir. Göz önünde bulundurmanız durumunda, tanınma iyileştirmeleri yalnızca belirtilen veriler kadar iyi olacaktır. Bu nedenle, yalnızca yüksek kaliteli döküm dosyalarının karşıya yüklenmesi önemlidir.

| Özellik                 | Değer                               |
|--------------------------|-------------------------------------|
| Dosya biçimi              | RIFF (WAV)                          |
| Örnekleme hızı              | 8.000 Hz veya 16.000 Hz               |
| Kanallar                 | 1 (mono)                            |
| Ses başına maksimum uzunluk | 2 saat (test)/60 s (eğitim) |
| Örnek biçim            | PCM, 16 bit                         |
| Arşiv biçimi           | .zip                                |
| En büyük ZIP boyutu         | 2 GB                                |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!NOTE]
> Eğitim ve test verileri yüklenirken. zip dosyası boyutu 2 GB 'ı aşamaz. Yalnızca *tek* bir veri kümesinden test edebilirsiniz, bunu uygun dosya boyutunda tutmanız yeterlidir. Ayrıca, her eğitim dosyası 60 saniye aşılamaz, aksi takdirde hata dışarı kalır.

Sözcük silme veya değiştirme gibi sorunları gidermek için, tanımayı geliştirmek için önemli miktarda veri gerekir. Genellikle, kabaca 10 ila 1.000 saatlik ses için Word sözcük dökümü sağlamanız önerilir. Tüm WAV dosyalarının transkripsiyonları tek bir düz metin dosyasına yerleştirilmelidir. Transkripsiyon dosyasının her satırında ses dosyalarından birinin adı ve transkripsiyon bulunmalıdır. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır.

  Örneğin:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```

> [!IMPORTANT]
> Transkripsiyon UTF-8 bayt sırası işareti (BOM) ile kodlanmış olmalıdır.

Transkripsiyon metinleri sistem tarafından işlenebilmesi için normalleştirilir. Ancak, veriler konuşma Studio 'ya yüklenmeden önce gerçekleştirilmesi gereken bazı önemli normalleştirmeler vardır. Dökümü hazırlarken kullanılacak uygun dil için, bkz. [nasıl yapılır: insan etiketli döküm oluşturma](how-to-custom-speech-human-labeled-transcriptions.md)

Ses dosyalarınızı ve bunlara karşılık gelen kayıtları topladıktan sonra, <a href="https://speech.microsoft.com/customspeech" target="_blank">özel konuşma tanıma portalına <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>yüklemeden önce bunları tek bir. zip dosyası olarak paketleyin. Aşağıda üç ses dosyası ve bir insan etiketli döküm dosyası içeren örnek bir veri kümesi verilmiştir:

> [!div class="mx-imgBorder"]
> ![Konuşma portalından ses seçme](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Eğitim için ilgili metin verileri

Ürün adları veya benzersiz özellikler, eğitim için ilgili metin verilerini içermelidir. İlgili metin, doğru tanımanın sağlanmasına yardımcı olur. Tanımayı geliştirmek için iki tür ilgili metin verisi sağlanabilmesi:

| Veri türü | Bu verilerin tanınması nasıl geliştirilir |
|-----------|------------------------------------|
| Cümleler (utterlere) | Ürün adlarını veya bir cümle bağlamı içindeki sektöre özel sözlüğü yapılandırırken doğruluğu geliştirir. |
| Söylenişler | Tanımsız söylenlerdeki yaygın olmayan terimlerin, kısaltmalardan veya diğer sözcüklerin okunuşunu geliştirir. |

Cümleler, tek bir metin dosyası veya birden çok metin dosyası olarak bulunabilir. Doğruluğu artırmak için, beklenen söyleye daha yakın olan metin verilerini kullanın. Söylenişler tek bir metin dosyası olarak sağlanmalıdır. Her şey tek bir zip dosyası olarak paketlenebilir ve <a href="https://speech.microsoft.com/customspeech" target="_blank">özel konuşma tanıma portalına <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>yüklenebilir.

### <a name="guidelines-to-create-a-sentences-file"></a>Cümleler dosyası oluşturma yönergeleri

Cümleler kullanarak özel bir model oluşturmak için, örnek dıklarla bir liste sağlamanız gerekir. Söyleyler 'in tam veya dilbilgisi doğru _olması gerekmez,_ ancak üretimde bekleeceğiniz konuşulan girişi doğru şekilde yansıtmaları gerekir. Belirli koşulların daha fazla ağırlığa sahip olmasını istiyorsanız, bu belirli koşulları içeren çeşitli cümleler ekleyin.

Genel rehberlik olarak, eğitim metni üretimde beklenen gerçek metin için mümkün olduğunca yakın olduğunda model uyarlama en etkili olur. Geliştirmeye hedeflediğiniz, etki alanına özgü öğretmek ve tümceleri eğitim metnine dahil edilmelidir. Mümkün olduğunda, bir cümleden veya anahtar kelimesinin ayrı bir satıra denetlenmesini deneyin. Sizin için önemli anahtar sözcükler ve tümcecikler (örneğin, ürün adları) için, bunları birkaç kez kopyalayabilirsiniz. Ancak, çok fazla kopyalamayın, bu da genel tanıma oranını etkileyebilir.

Dikkat edilecek ilgili veri dosyanızın doğru biçimlendirildiğinden emin olmak için bu tabloyu kullanın:

| Özellik | Değer |
|----------|-------|
| Metin kodlaması | UTF-8 BOM |
| Satır başına konuşma sayısı | 1 |
| En büyük dosya boyutu | 200 MB |

Ayrıca, aşağıdaki kısıtlamaları hesaba eklemek isteyeceksiniz:

* Dört defadan fazla karakter tekrarlamadan kaçının. Örneğin: "aaaa" veya "uuuu".
* Yukarıdaki `U+00A1`özel KARAKTERLERI veya UTF-8 karakterlerini kullanmayın.
* URI 'Ler reddedilir.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Telaffuz dosyası oluşturma yönergeleri

Kullanıcılarınızın karşılaşabilmesi veya kullanması gereken standart telaffuz olmayan, sık olmayan koşullar varsa, tanımayı geliştirmek için özel bir telaffuz dosyası sağlayabilirsiniz.

> [!IMPORTANT]
> Ortak sözcüklerin okunuşunu değiştirmek için özel telaffuz dosyaları kullanılması önerilmez.

Bu, konuşulan bir utterance örnekleri ve her biri için özel bir telaffuz içerir:

| Tanınan/görüntülenmiş form | Konuşulan form |
|--------------|--------------------------|
| 3CPO | Üç c p o |
| CNTK | c n t k |
| IEEE | Ben Üçlü e |

Konuşulan biçim fonetik bir dizidir. Bu, harf, sözcük, hecelere veya üçünün birleşiminden oluşabilir.

Özelleştirilmiş telaffuz Ingilizce (`en-US`) ve Almanca (`de-DE`) ile kullanılabilir. Bu tabloda dile göre desteklenen karakterler gösterilmektedir:

| Dil | Yerel Ayar | Karakterler |
|----------|--------|------------|
| İngilizce | `en-US` | `a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |
| Almanca | `de-DE` | `ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |

Söylenişleri için ilgili veri dosyanızın doğru biçimlendirildiğinden emin olmak için aşağıdaki tabloyu kullanın. Söyleniş dosyaları küçüktür ve yalnızca birkaç kilobayt boyutunda olmalıdır.

| Özellik | Değer |
|----------|-------|
| Metin kodlaması | UTF-8 BOM (Ingilizce için de ANSI desteklenir) |
| satır başına söylenişleri sayısı | 1 |
| En büyük dosya boyutu | 1 MB (ücretsiz katman için 1 KB) |

## <a name="next-steps"></a>Sonraki adımlar

* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitme](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)
