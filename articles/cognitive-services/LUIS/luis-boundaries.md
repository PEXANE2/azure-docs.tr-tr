---
title: Limit-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale, Azure Bilişsel hizmetler Language Understanding (LUIS) bilinen sınırları içerir. LUIS, birden fazla sınır alanlara sahip değildir. Model sınır amacı, varlıkları ve LUIS özellikleri denetler. Kota sınırları, anahtar türüne göre. Klavye birleşimi LUIS Web sitesini denetler.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 11/07/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: d584b00caef628eb9dfd085b1fdce2bb7b353988
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273511"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>LUSıS modeliniz ve anahtarlarınız için sınırlar
LUIS, birden fazla sınır alanlara sahip değildir. Birincisi, LUSıS 'deki amaçları, varlıkları ve özellikleri denetleyen [model sınırıdır](#model-boundaries). İkinci alan, anahtar türüne göre [Kota sınırlamalarıdır](#key-limits) . Bir üçüncü sınır alanı, LUSıS Web sitesini denetlemeye yönelik [klavye birleşimidir](#keyboard-controls) . Bir dördüncü alan, LUıS Authoring Web sitesi ve LUıS [uç nokta](luis-glossary.md#endpoint) API 'leri arasındaki [Dünya bölgesi eşlemesidir](luis-reference-regions.md) .


## <a name="model-boundaries"></a>Model sınırları

Uygulamanız Luo model sınırlarını ve sınırlarını aşarsa, bir [lusıs dağıtım](luis-concept-enterprise.md#dispatch-tool-and-model) uygulaması veya bir [lusıs kapsayıcısı](luis-container-howto.md)kullanmayı düşünün.

|Alan|Sınır|
|--|:--|
| [Uygulama adı][luis-get-started-create-app] | \* Max varsayılan karakter |
| Uygulamalar| Azure yazma kaynağı başına 500 uygulama |
| [Toplu işlem testi][batch-testing]| 10 veri kümeleri, veri kümesi başına 1000 konuşma|
| Açık listesi | uygulama başına 50|
| Dış varlıklar | sınırsız |
| [Hedefleri][intents]|Uygulama başına 500:499 özel amaçlar ve gerekli _hiçbiri_ amacı.<br>[Dağıtım tabanlı](https://aka.ms/dispatch-tool) uygulama karşılık gelen 500 dağıtım kaynaklarına sahiptir.|
| [Varlıkları Listele](./luis-concept-entity-types.md) | Üst: 50, alt: 20.000 öğeleri. Kurallı ad * varsayılan karakter maks. Eş anlamlı değerleri herhangi bir uzunluk sınırlaması vardır. |
| [Makine tarafından öğrenilen varlıklar + roller](./luis-concept-entity-types.md):<br> bileşik<br>MPLE<br>varlık rolü|İlk olarak Kullanıcı isabetlerinin sınırlarını sınırlayan 100 üst varlık veya 330 varlıklarının bir sınırı. Rol, bu sınırın amacı için bir varlık olarak sayılır. Örnek, bir basit varlık olan, 2 rol içeren bir Composite örnektir: 1 bileşik + 1 basit + 2 rol = 4 varlıkların 330.<br>Alt bileşenler 5 düzeye kadar iç içe olabilir.|
|Özellik olarak model oluşturma| Belirli bir modelde 10 model olması için tanımlayıcı (özellik) olarak kullanılabilecek model sayısı üst sınırı. Belirli bir model için bir tanımlayıcı (özellik) olarak kullanılan en fazla tümcecik listesi sayısı 10 ifade listesi olmalıdır.|
| [Önizleme-dinamik liste varlıkları](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|sorgu tahmin uç noktası isteği başına 2 adet ~ 1k liste|
| [Lıklarını](luis-concept-patterns.md)|uygulama başına 500 desenleri.<br>Deseni en fazla uzunluğu 400 karakter olabilir.<br>Deseni başına 3 Pattern.any varlıklar<br>En fazla 2 iç içe geçmiş isteğe bağlı metni deseninde|
| [Model. any](./luis-concept-entity-types.md)|uygulama başına 100 deseni başına 3 pattern.any varlıklar |
| [Tümcecik listesi][phrase-list]|500 ifade listesi. Değiştirilebilir olmayan PhraseList en fazla 5.000 tümcecik içerir. Değiştirilebilir PhraseList, en fazla 50.000 tümcecik içerir. 500.000 tümcecikte uygulama başına en fazla toplam tümcecik sayısı.|
| [Önceden oluşturulmuş varlıklar](./luis-prebuilt-entities.md) | bir sınır yoktur|
| [Normal ifade varlıkları](./luis-concept-entity-types.md)|20 varlıklar<br>Maksimum 500 karakter. Normal ifade varlık deseni|
| [Roller](luis-concept-roles.md)|uygulama başına 300 roller. Varlık başına 10 rolü|
| [Söylenişi][utterances] | 500 karakter|
| [En konuşma][utterances] | Uygulama başına 15.000-amaç başına düşen sayıda sınır yoktur|
| [Ün](luis-concept-version.md)| Uygulama başına 100 sürüm |
| [Sürüm adı][luis-how-to-manage-versions] | alfasayısal ve süre sınırlı 10 karakter (.) |

\* Varsayılan karakter en fazla 50 karakterdir.

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Ad benzersizliği

Aşağıdaki adlandırma benzersizlik kurallarını kullanın.

Aşağıdaki, bir LUSıS uygulaması içinde benzersiz olmalıdır:

* Sürüm adı
* varsayılandır
* varlık
* roles

Aşağıdaki, uygulanan kapsam içinde benzersiz olmalıdır:

* tümcecik listesi

## <a name="object-naming"></a>Nesne adlandırma

Aşağıdaki adlarla aşağıdaki karakterleri kullanmayın.

|Nesne|Dışlama karakterleri|
|--|--|
|Amaç, varlık ve rol adları|`:`<br>`$` <br> `&`|
|Sürüm adı|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="key-usage"></a>Anahtar kullanımı

Dil anlaşılması ayrı anahtarlar, yazma için bir tür ve tahmin uç noktasını sorgulamak için bir tür içerir. Anahtar türleri arasındaki farklar hakkında daha fazla bilgi edinmek için bkz. [lusıs 'de yazma ve sorgu tahmin uç noktası anahtarları](luis-concept-keys.md).

<a name="key-limits"></a>

## <a name="resource-key-limits"></a>Kaynak anahtarı sınırları

Kaynak anahtarlarının yazma ve uç nokta için farklı sınırları vardır. LUSıS tahmin sorgusu uç noktası anahtarı yalnızca Endpoint sorguları için geçerlidir.

* Azure yazma kaynağı başına 500 uygulama

|Anahtar|Yazma|Uç Nokta|Amaç|
|--|--|--|--|
|Başlangıç|1 milyon/ay, 5/saniye|1 bin/ay, 5/saniye|LUIS uygulamanızı yazma|
|F0-ücretsiz katman |1 milyon/ay, 5/saniye|10 bin/ay, 5/saniye|LUIS uç noktanızı sorgulama|
|S0-temel katman|-|50/saniye|LUIS uç noktanızı sorgulama|
|S0-Standart katman|-|50/saniye|LUIS uç noktanızı sorgulama|
|[Yaklaşım Analizi tümleştirmesi](luis-how-to-publish-app.md#enable-sentiment-analysis)|-|-|Anahtar tümceciği veri ayıklama dahil olmak üzere yaklaşım bilgileri ekleme, başka bir Azure kaynağı gerektirmeksizin sağlanır. |
|[Konuşma tümleştirmesi](../speech-service/how-to-recognize-intents-from-speech-csharp.md)|-|birim maliyeti başına 1000 uç nokta isteği|Konuşulan utterance dönüştürmek için metin utterance ve LUIS sonuçlar döndürebilir.|

[Fiyatlandırma hakkında daha fazla bilgi edinin.][pricing]

## <a name="keyboard-controls"></a>Klavye denetimleri

|Klavye girdisi | Açıklama |
|--|--|
|Denetim + E|belirteçler ve konuşma listesindeki varlıklar arasında geçiş yapar|

## <a name="website-sign-in-time-period"></a>Web sitesi oturum açma zamanı dönemi

Oturum açma erişiminiz **60 dakikaya**yöneliktir. Bu süre sonra bu hatayı alırsınız. Yeniden oturum açmanız gerekiyor.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
