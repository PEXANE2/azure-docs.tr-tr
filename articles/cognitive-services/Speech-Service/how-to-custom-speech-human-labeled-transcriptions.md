---
title: İnsan etiketli transkripsiyon kılavuzu - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Sözcüklersilindiğinde veya yanlış değiştirildiğinde olduğu gibi konuşma tanıma doğruluğunu artırmak için ses verilerinizle birlikte insan etiketli transkripsiyonlar da kullanabilirsiniz. İnsan etiketli transkripsiyonlar, bir ses dosyasının kelime kelime nin harfi harfiyle transkripsiyonu.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: 81b4ffc8f77673e52bb78f891e3de618b67e0d1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74806071"
---
# <a name="how-to-create-human-labeled-transcriptions"></a>İnsan etiketli transkripsiyonlar nasıl oluşturulur?

Tanıma doğruluğunu artırmak istiyorsanız, özellikle sözcükler silindiğinde veya yanlış değiştirildiğinde ortaya çıkan sorunlar, ses verilerinizle birlikte insan etiketli transkripsiyonlar kullanmak istersiniz. İnsan etiketli transkripsiyonlar nelerdir? Bu çok kolay, onlar kelime kelime, bir ses dosyasının kelimesi kelimesine transkripsiyonları.

Tanımayı geliştirmek için transkripsiyon verilerinin büyük bir örneği gereklidir, 10 ila 1000 saat arasında transkripsiyon verisi sağlamanızı öneririz. Bu sayfada, yüksek kaliteli transkripsiyonlar oluşturmanıza yardımcı olmak üzere tasarlanmış yönergeleri gözden geçireceğiz. Bu kılavuz, ABD İngilizcesi, Mandarin Çincesi ve Almanca bölümleri ile yerel olarak ayrılmıştır.

## <a name="us-english-en-us"></a>ABD İngilizcesi (tr-US)

İngilizce ses için insan etiketli transkripsiyonlar, yalnızca ASCII karakterleri kullanılarak düz metin olarak sağlanmalıdır. Latin-1 veya Unicode noktalama işaretlerinin kullanılmasından kaçının. Bu karakterler genellikle bir sözcük işleme uygulamasından metin kopyalarken veya web sayfalarından veri kazırken yanlışlıkla eklenir. Bu karakterler varsa, bunları uygun ASCII ikamesiyle güncelleştirdiğinden emin olun.

İşte birkaç örnek:

| Kaçınılması gereken karakterler | Değiştirme | Notlar |
| ------------------- | ------------ | ----- |
| "Merhaba dünya" | "Merhaba dünya" | Açılış ve kapanış tırnak işaretleri uygun ASCII karakterleri ile değiştirilmiştir. |
| John'un günü | John'un günü | Keseli ile uygun ASCII karakteri değiştirilmiştir. |
| iyi oldu-hayır, harikaydı! | bu iyiydi -- hayır, harikaydı! | Em tire iki tire ile değiştirildi. |

### <a name="text-normalization-for-us-english"></a>ABD İngilizcesi için metin normalleştirme

Metin normalleştirme, sözcüklerin bir modeli eğitirken kullanılan tutarlı bir biçime dönüştürülmesidir. Metne otomatik olarak bazı normalleştirme kuralları uygulanır, ancak insan etiketli transkripsiyon verilerinizi hazırlarken bu yönergeleri kullanmanızı öneririz:

- Kısaltmaları kelimelerle yazın.
- Standart olmayan sayısal dizeleri sözcüklere (muhasebe terimleri gibi) yazın.
- Alfabetik olmayan karakterler veya karışık alfasayısal karakterler telaffuz olarak yazılmalıdır.
- Kelime olarak telaffuz edilen kısaltmalar düzenlenmemelidir ("radar", "lazer", "RAM" veya "NATO" gibi).
- Her harf bir boşlukla ayrılmış ayrı harfler olarak telaffuz edilen kısaltmaları yazın.

Transkripsiyon üzerinde gerçekleştirmeniz gereken birkaç normalleştirme örneği aşağıda verilmiştir:

| Orijinal metin               | Normalleştirme den sonra metin              |
| --------------------------- | ------------------------------------- |
| Dr. Bruce Banner            | Doktor Bruce Banner                   |
| James Bond, 007             | James Bond, çift oh yedi           |
| Ke$ha                       | Kesha                                 |
| 2x4 ne kadar         | Ne kadar süre yle iki dört           |
| Toplantı 1-3 arası. | Toplantı 1'den 3'e kadar devam ediyor. |
| Kan grubum O+         | Kan grubum O pozitif           |
| Su H20'dir                | Su H 2 O                        |
| Van Halen tarafından OU812 oyna     | Van Halen tarafından O U 8 1 2 oyna           |
| BOM ile UTF-8              | BOM ile U T F 8                      |

Aşağıdaki normalleştirme kuralları transkripsiyonlara otomatik olarak uygulanır:

- Küçük harfler kullanın.
- Sözcüklerdeki kesitler hariç tüm noktalama işaretlerini kaldırın.
- Sayıları dolar tutarları gibi sözcüklere/konuşulan forma genişletin.

Transkripsiyonda otomatik olarak gerçekleştirilen normalleştirmenin birkaç örneği aşağıda verilmiştir:

| Orijinal metin                          | Normalleştirme den sonra metin          |
| -------------------------------------- | --------------------------------- |
| "Kutsal inek!" Batman dedi.               | kutsal batman dedi              |
| "Ne?" Batman'in yardımcısı Robin dedi. | batman'in yardımcısı robin ne dedi |
| Git -em al!                            | git em olsun                         |
| Çift eklemliyim.                     | Ben çift eklemli değilim                |
| 104 Karaağaç Sokak                         | bir oh dört Elm sokak            |
| 102.7'yi dinle                          | bir oh iki nokta yedi için ayarlayın    |
| Pi yaklaşık 3.14 olduğunu                       | pi yaklaşık üç nokta bir dört  |
| Bu \$3,14 maliyeti                        | bu üç on dört maliyeti           |

## <a name="mandarin-chinese-zh-cn"></a>Mandalina Çincesi (zh-CN)

Mandarin Çince ses için insan etiketli transkripsiyonlar UTF-8 bir bayt sipariş marker ile kodlanmış olmalıdır. Yarım genişliknoktalama karakterlerinin kullanımından kaçının. Bu karakterler, bir sözcük işleme programındaki verileri hazırlarken veya web sayfalarından verileri kazıdığınızda yanlışlıkla eklenebilir. Bu karakterler varsa, bunları uygun tam genişlik değiştirmeyle güncelleştirdiğinden emin olun.

İşte birkaç örnek:

| Kaçınılması gereken karakterler | Değiştirme   | Notlar |
| ------------------- | -------------- | ----- |
| "Bu" | "Bu" | Açılış ve kapanış tırnak işaretleri uygun karakterlerle değiştirildi. |
| Bir şey yok mu? | Bir şey yok mu?| Soru işareti uygun karakterle değiştirildi. |

### <a name="text-normalization-for-mandarin-chinese"></a>Mandarin Çincesi için metin normalleştirme

Metin normalleştirme, sözcüklerin bir modeli eğitirken kullanılan tutarlı bir biçime dönüştürülmesidir. Metne otomatik olarak bazı normalleştirme kuralları uygulanır, ancak insan etiketli transkripsiyon verilerinizi hazırlarken bu yönergeleri kullanmanızı öneririz:

- Kısaltmaları kelimelerle yazın.
- Sayısal dizeleri sözlü biçimde yazın.

Transkripsiyon üzerinde gerçekleştirmeniz gereken birkaç normalleştirme örneği aşağıda verilmiştir:

| Orijinal metin | Normalleştirme den sonra metin |
| ------------- | ------------------------ |
| 21. 21 | Bu nedenle, 2006 yılında, 2006 yılında, |
| 3 . 504 | Bir anda |

Aşağıdaki normalleştirme kuralları transkripsiyonlara otomatik olarak uygulanır:

- Tüm noktalama işaretlerini kaldırma
- Sayıları konuşulan forma genişletme
- Tam genişlikli harfleri yarım genişlikli harflere dönüştürme
- Tüm İngilizce sözcükler için büyük harf kullanma

Transkripsiyonda otomatik olarak gerçekleştirilen normalleştirmenin birkaç örneği aşağıda verilmiştir:

| Orijinal metin | Normalleştirme den sonra metin |
| ------------- | ------------------------ |
| 3.1415 | Bu kadar önemli bir şey. |
| ¥ 3.5 | Bu da benim için geçerli dir. |
| w f y z | W F Y Z |
| 1992 - 8 中 8 | Bir şey yok . |
| Bu da ne demek oluyor? | Bu da 2000'li ve 2000'li ve 2 |
| Saat 05:00 | Bu yıl içinde , 2006 yılında, 2006 yılında, 2006 yılında, 2 |
| 21. Yıl 21 | Bu nedenle , 2010 yılında, 2006 yılında, 2006 yılında |

## <a name="german-de-de-and-other-languages"></a>Almanca (de-DE) ve diğer diller

Almanca ses için insan etiketli transkripsiyonlar (ve diğer İngilizce olmayan veya Mandarin Çince dilleri) UTF-8 bir bayt sipariş işaretçisi ile kodlanmış olmalıdır. Her ses dosyası için bir adet insan etiketli transkript sağlanmalıdır.

### <a name="text-normalization-for-german"></a>Almanca için metin normalleştirme

Metin normalleştirme, sözcüklerin bir modeli eğitirken kullanılan tutarlı bir biçime dönüştürülmesidir. Metne otomatik olarak bazı normalleştirme kuralları uygulanır, ancak insan etiketli transkripsiyon verilerinizi hazırlarken bu yönergeleri kullanmanızı öneririz:

- Ondalık noktaları "" olarak değil, "" olarak yazın.
- Zaman ayırıcılarını " olarak değil , ":" olarak yazın. (örneğin: 12:00 Uhr).
- "ca" gibi kısaltmalar. değiştirilmez. Tam konuşma formunu kullanmanızı öneririz.
- Dört ana matematiksel işleç (+, -, \*ve /) kaldırılır. Bunları "artı", "eksi", "mal" ve "geteilt" yazılı formla değiştirmenizi öneririz.
- Karşılaştırma işleçleri kaldırılır (=, < ve >). Onları "gleich", "kleiner als" ve "grösser als" ile değiştirmenizi öneririz.
- 3/4 gibi kesirleri yazılı olarak yazın (örneğin: 3/4 yerine "drei viertel").
- "€" sembolünü "Euro" yazılı formuyla değiştirin.

Transkripsiyon üzerinde gerçekleştirmeniz gereken birkaç normalleştirme örneği aşağıda verilmiştir:

| Orijinal metin    | Kullanıcı normalleştirmeden sonra metin | Sistem normalleştirme sonrası metin       |
| ---------------- | ----------------------------- | ------------------------------------- |
| Es ist 12.23 Uhr | Es ist 12:23 Uhr              | es ist zwölf uhr drei und zwanzig uhr |
| {12.45}          | {12,45}                       | zwölf komma vier fünf                 |
| 2 + 3 - 4        | 2 artı 3 eksi 4              | zwei artı drei eksi vier             |

Aşağıdaki normalleştirme kuralları transkripsiyonlara otomatik olarak uygulanır:

- Tüm metin için küçük harfler kullanın.
- Çeşitli tırnak işaretleri ("test", 'test', "test" ve «test» tamam) dahil olmak üzere tüm noktalama işaretlerini kaldırın.
- Bu setten herhangi bir özel karakter içeren satırları atın: ¢ ¢ ¥ ¤ ¤ © ¬ ® ° ± ² μ × ø¬¬.
- Dolar veya Euro tutarları da dahil olmak üzere sayıları konuşulan forma genişletin.
- Umlautları sadece a, o ve senin için kabul et. Diğerleri "th" ile değiştirilecek veya atılır.

Transkripsiyonda otomatik olarak gerçekleştirilen normalleştirmenin birkaç örneği aşağıda verilmiştir:

| Orijinal metin    | Normalleştirme den sonra metin |
| ---------------- | ------------------------ |
| Frankfurter Yüzük | frankfurter halka         |
| Eine Frage!     | eine frage               |
| wir, haben       | wir haben                |

## <a name="next-steps"></a>Sonraki Adımlar

- [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
- [Modelinizi eğitme](how-to-custom-speech-train-model.md)
- [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)
