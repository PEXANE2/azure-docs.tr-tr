---
title: Limit-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale, Azure Bilişsel hizmetler Language Understanding (LUIS) bilinen sınırları içerir. LUIS, birden fazla sınır alanlara sahip değildir. Model sınır amacı, varlıkları ve LUIS özellikleri denetler. Kota sınırları, anahtar türüne göre. Klavye birleşimi LUIS Web sitesini denetler.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 3fd593ff199ff87b1c69e1097852a81a21adc1dd
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883952"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>LUSıS modeliniz ve anahtarlarınız için sınırlar
LUIS, birden fazla sınır alanlara sahip değildir. İlk [modeli sınır](#model-boundaries), amacı, varlıkları ve LUIS özellikleri denetler. İkinci alanı [kota sınırları](#key-limits) anahtar türüne göre. Üçüncü bir sınırları alanıdır [klavye birleşimi](#keyboard-controls) LUIS Web sitesi denetleme. Dördüncü alan [dünya bölge eşleme](luis-reference-regions.md) LUIS ile Web sitesi geliştirme LUIS arasındaki [uç nokta](luis-glossary.md#endpoint) API'leri. 


## <a name="model-boundaries"></a>Model sınırları

Uygulamanız Luo model sınırlarını ve sınırlarını aşarsa, bir [lusıs dağıtım](luis-concept-enterprise.md#dispatch-tool-and-model) uygulaması veya bir [lusıs kapsayıcısı](luis-container-howto.md)kullanmayı düşünün. 

|Alan|Sınır|
|--|:--|
| [Uygulama adı][luis-get-started-create-app] | \* Max varsayılan karakter |
| [Toplu işlem testi][batch-testing]| 10 veri kümeleri, veri kümesi başına 1000 konuşma|
| Açık listesi | uygulama başına 50|
| Dış varlıklar | sınırsız |
| [Hedefleri][intents]|Uygulama başına 500: 499 özel amaçlar ve gerekli _hiçbiri_ amacı.<br>[Dağıtım tabanlı](https://aka.ms/dispatch-tool) uygulama karşılık gelen 500 dağıtım kaynaklarına sahiptir.|
| [Varlıklar listesi](./luis-concept-entity-types.md) | Üst 50, alt öğe: 20.000 öğe. Kurallı ad * varsayılan karakter maks. Eş anlamlı değerleri herhangi bir uzunluk sınırlaması vardır. |
| [Makine tarafından öğrenilen varlıklar + roller](./luis-concept-entity-types.md):<br> bileşik<br>MPLE<br>varlık rolü|İlk olarak Kullanıcı isabetlerinin sınırlarını sınırlayan 100 üst varlık veya 330 varlıklarının bir sınırı. Rol, bu sınırın amacı için bir varlık olarak sayılır. Bir örnek, bir basit varlık olan ve 2 rol içeren bir Composite örneğidir: 1 bileşik + 1 basit + 2 rol = 4 varlık 330.|
| [Önizleme-dinamik liste varlıkları](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|sorgu tahmin uç noktası isteği başına 2 adet ~ 1k liste|
| [Desenleri](luis-concept-patterns.md)|uygulama başına 500 desenleri.<br>Deseni en fazla uzunluğu 400 karakter olabilir.<br>Deseni başına 3 Pattern.any varlıklar<br>En fazla 2 iç içe geçmiş isteğe bağlı metni deseninde|
| [Pattern.Any](./luis-concept-entity-types.md)|uygulama başına 100 deseni başına 3 pattern.any varlıklar |
| [Tümcecik listesi][phrase-list]|10 tümcecik listeler, liste başına 5.000 öğeleri|
| [Önceden oluşturulmuş varlıklar](./luis-prebuilt-entities.md) | bir sınır yoktur|
| [Normal ifade varlıkları](./luis-concept-entity-types.md)|20 varlıklar<br>Maksimum 500 karakter. Normal ifade varlık deseni|
| [Roller](luis-concept-roles.md)|uygulama başına 300 roller. Varlık başına 10 rolü|
| [Söylenişi][utterances] | 500 karakter|
| [En konuşma][utterances] | Uygulama başına 15.000-amaç başına düşen sayıda sınır yoktur|
| [Sürümleri](luis-concept-version.md)| bir sınır yoktur |
| [Sürüm adı][luis-how-to-manage-versions] | alfasayısal ve süre sınırlı 10 karakter (.) |

\* Varsayılan karakter en fazla 50 karakterdir. 

<a name="intent-and-entity-naming"></a>

## <a name="object-naming"></a>Nesne adlandırma

Aşağıdaki adlarla aşağıdaki karakterleri kullanmayın.

|Object|Dışlama karakterleri|
|--|--|
|Amaç, varlık ve rol adları|`:`<br>`$`|
|Sürüm adı|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="key-usage"></a>Anahtar kullanımı

Dil anlaşılması ayrı anahtarlar, yazma için bir tür ve tahmin uç noktasını sorgulamak için bir tür içerir. Anahtar türleri arasındaki farklar hakkında daha fazla bilgi edinmek için bkz. [lusıs 'de yazma ve sorgu tahmin uç noktası anahtarları](luis-concept-keys.md).

## <a name="key-limits"></a>Anahtar sınırları

Yazma anahtar yazma ve uç noktası için farklı sınırlara sahiptir. LUIS hizmet uç noktası anahtarı yalnızca uç nokta sorgular için geçerlidir.


|Anahtar|Yazma|Uç Nokta|Amaç|
|--|--|--|--|
|Language Understanding yazma/Başlatıcı|1 milyon/ay, 5/saniye|1 bin/ay, 5/saniye|LUIS uygulamanızı yazma|
|Language Understanding [abonelik][pricing] -F0-ücretsiz katmanı |geçersiz|10 bin/ay, 5/saniye|LUIS uç noktanızı sorgulama|
|Language Understanding [abonelik][pricing] -S0-temel katman|geçersiz|50/saniye|LUIS uç noktanızı sorgulama|
|Bilişsel hizmet [aboneliği][pricing] -S0-standart katmanı|geçersiz|50/saniye|LUIS uç noktanızı sorgulama|
|[Yaklaşım analizi tümleştirme](luis-how-to-publish-app.md#enable-sentiment-analysis)|geçersiz|Ücretsiz|Anahtar ifade veri ayıklama gibi yaklaşım bilgileri ekleme |
|[Konuşma tümleştirmesi](../speech-service/how-to-recognize-intents-from-speech-csharp.md)|geçersiz|5\.50 ABD Doları/1 bin uç nokta istekleri|Konuşulan utterance dönüştürmek için metin utterance ve LUIS sonuçlar döndürebilir.|

## <a name="keyboard-controls"></a>Klavye denetimleri

|Klavye girdisi | Açıklama | 
|--|--|
|Denetim + E|belirteçler ve konuşma listesindeki varlıklar arasında geçiş yapar|

## <a name="website-sign-in-time-period"></a>Web sitesi oturum açma zamanı dönemi

Oturum açma erişiminizi içindir **60 dakika**. Bu süre sonra bu hatayı alırsınız. Yeniden oturum açmanız gerekiyor.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
