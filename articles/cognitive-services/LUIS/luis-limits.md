---
title: Limit-LUSıS
description: Bu makale, Azure bilişsel hizmetler Language Understanding (LUSıS) 'nin bilinen sınırlarını içerir. LUO 'NUN birkaç sınır alanı vardır. Model sınırı denetim amaçları, varlıklar ve LUSıS 'deki Özellikler. Anahtar türüne göre kota sınırları. Klavye birleşimi LUSıS Web sitesini denetler.
ms.topic: reference
ms.date: 05/06/2020
ms.openlocfilehash: d4a6162758fab7e5c9592b98974620bbf06ba978
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684611"
---
# <a name="limits-for-your-luis-model-and-keys"></a>LUSıS modeliniz ve anahtarlarınız için sınırlar
LUO 'NUN birkaç sınır alanı vardır. Birincisi, LUSıS 'deki amaçları, varlıkları ve özellikleri denetleyen [model limiti](#model-limits)olur. İkinci alan, anahtar türüne göre [Kota sınırlamalarıdır](#key-limits) . Limitlerin üçüncü alanı, LUSıS Web sitesini denetlemeye yönelik [klavye birleşimidir](#keyboard-controls) . Bir dördüncü alan, LUıS Authoring Web sitesi ve LUıS [uç nokta](luis-glossary.md#endpoint) API 'leri arasındaki [Dünya bölgesi eşlemesidir](luis-reference-regions.md) .

<a name="model-boundaries"></a>

## <a name="model-limits"></a>Model sınırları

Uygulamanız LUSıS model sınırlarını aşarsa, bir [lusıs dağıtım](luis-concept-enterprise.md#dispatch-tool-and-model) uygulaması kullanmayı veya bir [lusıs kapsayıcısı](luis-container-howto.md)kullanmayı düşünün.

|Alan|Sınır|
|--|:--|
| [Uygulama adı][luis-get-started-create-app] | * Varsayılan karakter en fazla |
| Uygulamalar| Azure yazma kaynağı başına 500 uygulama |
| [Toplu işe testi][batch-testing]| 10 veri kümesi, veri kümesi başına 1000 utterde|
| Açık liste | Uygulama başına 50|
| Dış varlıklar | sınırsız |
| [Hedefler][intents]|Uygulama başına 500:499 özel amaçlar ve gerekli _hiçbiri_ amacı.<br>[Dağıtım tabanlı](https://aka.ms/dispatch-tool) uygulama karşılık gelen 500 dağıtım kaynaklarına sahiptir.|
| [Liste varlıkları](./luis-concept-entity-types.md) | Üst: 50, alt: 20.000 öğeleri. Kurallı ad, en fazla * varsayılan karakter maks. Eş anlamlı değerlerde uzunluk kısıtlaması yok. |
| [makine öğrenimi varlıkları + roller](./luis-concept-entity-types.md):<br> bileşik<br>MPLE<br>varlık rolü|İlk olarak Kullanıcı isabetlerinin sınırlarını sınırlayan 100 üst varlık veya 330 varlıklarının bir sınırı. Rol, bu sınırın amacı için bir varlık olarak sayılır. Örnek, bir basit varlık olan, 2 rol içeren bir Composite örnektir: 1 bileşik + 1 basit + 2 rol = 4 varlıkların 330.<br>Alt varlıklar 5 düzeyden fazla iç içe olabilir.|
|Özellik olarak model oluşturma| Belirli bir modelde 10 model olması için bir özellik olarak kullanılabilecek maksimum model sayısı. Belirli bir modelin 10 ifade listesi olması için bir özellik olarak kullanılan en fazla tümcecik listesi sayısı.|
| [Önizleme-dinamik liste varlıkları](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|sorgu tahmin uç noktası isteği başına 2 adet ~ 1k liste|
| [Desenler](luis-concept-patterns.md)|Uygulama başına 500 desen.<br>Maksimum model uzunluğu 400 karakterdir.<br>3 model. her model için tüm varlıklar<br>Düzende en fazla 2 iç içe isteğe bağlı metin|
| [Model. any](./luis-concept-entity-types.md)|Uygulama başına 100, 3 model. her tür varlık |
| [Tümcecik listesi][phrase-list]|500 ifade listesi. bir özellik sınırı olarak model nedeniyle 10 genel ifade listesi. Değiştirilebilir olmayan tümcecik listesi en fazla 5.000 tümcecik içerir. Değiştirilebilir tümcecik listesinin en fazla 50.000 tümceciği vardır. 500.000 tümcecikte uygulama başına en fazla toplam tümcecik sayısı.|
| [Önceden oluşturulmuş varlıklar](./luis-prebuilt-entities.md) | sınır yok|
| [Normal ifade varlıkları](./luis-concept-entity-types.md)|20 varlık<br>en fazla 500 karakter. normal ifade varlık düzenine göre|
| [Roller](luis-concept-roles.md)|Uygulama başına 300 rol. varlık başına 10 rol|
| [İfade][utterances] | 500 karakter|
| [Konuşmalar][utterances] | Uygulama başına 15.000-amaç başına düşen sayıda sınır yoktur|
| [Sürümler](luis-concept-version.md)| Uygulama başına 100 sürüm |
| [Sürüm adı][luis-how-to-manage-versions] | 128 karakter |

* Varsayılan karakter en fazla 50 karakterdir.

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Ad benzersizliği

Aynı düzeyin diğer nesneleriyle karşılaştırıldığında, nesne adları benzersiz olmalıdır.

|Nesneler|Kısıtlamalar|
|--|--|
|Amaç, varlık|Tüm amaç ve varlık adları, bir uygulamanın sürümünde benzersiz olmalıdır.|
|ML varlık bileşenleri|Tüm makine öğrenimi varlık bileşenleri (alt varlıklar), aynı düzeydeki bileşenler için bu varlık dahilinde benzersiz olmalıdır.|
|Özellikler | Tümcecik listeleri gibi tüm adlandırılmış özellikler, bir uygulamanın sürümü içinde benzersiz olmalıdır.|
|Varlık rolleri|Bir varlık veya varlık bileşenindeki tüm roller, aynı varlık düzeyinde olduklarında benzersiz olmalıdır (üst, alt, alt alt, vb.).|

## <a name="object-naming"></a>Nesne adlandırma

Aşağıdaki adlarla aşağıdaki karakterleri kullanmayın.

|Nesne|Dışlama karakterleri|
|--|--|
|Amaç, varlık ve rol adları|`:`<br>`$` <br> `&`|
|Sürüm adı|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="resource-usage-and-limits"></a>Kaynak kullanımı ve sınırları

Dil anlaşılması ayrı kaynaklar, yazma için bir tür ve tahmin uç noktasını sorgulamak için bir tür içerir. Anahtar türleri arasındaki farklar hakkında daha fazla bilgi edinmek için bkz. [lusıs 'de yazma ve sorgu tahmin uç noktası anahtarları](luis-concept-keys.md).

<a name="key-limits"></a>

### <a name="authoring-resource-limits"></a>Kaynak sınırlarını yazma

_kind_ `LUIS.Authoring` Azure Portal kaynakları filtrelerken türü kullanın. LUSıS, Azure yazma kaynağı başına 500 uygulama kısıtlar.

|Kaynak yazma|Yazma TPS 'leri|
|--|--|
|Başlangıç|1 milyon/ay, 5/saniye|
|F0-ücretsiz katman |1 milyon/ay, 5/saniye|

* TPS = Işlem/saniye

[Fiyatlandırma hakkında daha fazla bilgi edinin.][pricing]

### <a name="query-prediction-resource-limits"></a>Sorgu tahmini kaynak sınırları

_kind_ `LUIS` Azure Portal kaynakları filtrelerken türü kullanın. Çalışma zamanında kullanılan LUSıS sorgu tahmini uç noktası kaynağı yalnızca Endpoint sorguları için geçerlidir.

|Sorgu tahmini kaynağı|Sorgu TPS 'leri|
|--|--|
|F0-ücretsiz katman |10 bin/ay, 5/saniye|
|S0-Standart katman|50/saniye|

### <a name="sentiment-analysis"></a>Yaklaşım analizi

Yaklaşım bilgilerini sağlayan yaklaşım [Analizi tümleştirmesi](luis-how-to-publish-app.md#enable-sentiment-analysis), başka bir Azure kaynağı gerekmeden sağlanır.

### <a name="speech-integration"></a>Konuşma tümleştirmesi

[Konuşma tümleştirmesi](../speech-service/how-to-recognize-intents-from-speech-csharp.md) birim maliyeti başına 1000 uç nokta isteği sağlar.

[Fiyatlandırma hakkında daha fazla bilgi edinin.][pricing]

## <a name="keyboard-controls"></a>Klavye denetimleri

|Klavye girişi | Açıklama |
|--|--|
|Denetim + E|utterslar listesindeki belirteçler ve varlıklar arasında geçiş yapar|

## <a name="website-sign-in-time-period"></a>Web sitesi oturum açma zamanı dönemi

Oturum açma erişiminiz **60 dakikaya**yöneliktir. Bu süre dolduktan sonra bu hatayı alırsınız. Yeniden oturum açmanız gerekiyor.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
