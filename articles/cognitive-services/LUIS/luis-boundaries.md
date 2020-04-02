---
title: Limitler - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Azure Bilişsel Hizmetler Dil Anlayışının (LUIS) bilinen sınırlarını içerir. LUIS'in birkaç sınır alanı vardır. Model sınır, LUIS'teki amaçları, varlıkları ve özellikleri denetler. Anahtar türüne göre kota sınırları. Klavye kombinasyonu LUIS web sitesini kontrol eder.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 11/07/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 6c021e68f8b76d8b0d3e6e9ff21c242580f53313
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520953"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>LUIS modeliniz ve anahtarlarınız için sınırlar
LUIS'in birkaç sınır alanı vardır. Bunlardan ilki, LUIS'teki niyetleri, varlıkları ve özellikleri kontrol eden [model sınırıdır.](#model-boundaries) İkinci alan, anahtar türüne göre [kota sınırlarıdır.](#key-limits) Sınırların üçüncü bir alanı LUIS web sitesini kontrol etmek için [klavye kombinasyonudur.](#keyboard-controls) Dördüncü bir alan luis yazma web sitesi ve LUIS [uç nokta](luis-glossary.md#endpoint) API'ler arasında dünya bölgesi [haritalama](luis-reference-regions.md) olduğunu.


## <a name="model-boundaries"></a>Model sınırları

Uygulamanız LUIS model sınırlarını ve sınırlarını aşarsa, bir [LUIS gönderim](luis-concept-enterprise.md#dispatch-tool-and-model) uygulaması veya [LUIS kapsayıcısı](luis-container-howto.md)kullanmayı düşünün.

|Alan|Sınır|
|--|:--|
| [Uygulama adı][luis-get-started-create-app] | *Varsayılan karakter max |
| Uygulamalar| Azure yazma kaynağı başına 500 uygulama |
| [Toplu işe testi][batch-testing]| 10 veri kümesi, veri kümesi başına 1000 sözcük|
| Açık liste | Uygulama başına 50|
| Dış varlıklar | sınır yok |
| [Hedefler][intents]|Uygulama başına 500: 499 özel niyet ve gerekli _Hiçbiri_ niyet.<br>[Sevk tabanlı](https://aka.ms/dispatch-tool) uygulama nın karşılık gelen 500 sevk kaynağı vardır.|
| [Liste varlıkları](./luis-concept-entity-types.md) | Ebeveyn: 50, çocuk: 20.000 öğe. Kanonik ad *varsayılan karakter max'tir. Eşanlamlı değerlerin uzunluk kısıtlaması yoktur. |
| [Makinede öğrenilen varlıklar + roller:](./luis-concept-entity-types.md)<br> Kompozit<br>Basit<br>varlık rolü|100 ana varlık veya 330 varlığın sınırı( kullanıcı önce hangi sınır ahit olursa. Bir rol, bu sınırın amacı için bir varlık olarak sayılır. Bir örnek, 2 rolü olan basit bir varlık ile bir bileşiktir: 1 bileşik + 1 basit + 2 rolleri = 4 330 varlıklar.<br>Alt bileşenler 5 düzeye kadar iç içe olabilir.|
|Özellik olarak model| 10 model olmak üzere belirli bir modeliçin tanımlayıcı (özellik) olarak kullanılabilecek maksimum model sayısı. Belirli bir model için tanımlayıcı (özellik) olarak kullanılan en fazla ifade listesi sayısı 10 tümcecik listesi dir.|
| [Önizleme - Dinamik liste varlıkları](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|Sorgu başına ~1k tahmin bitiş noktası isteği 2 listeleri|
| [Desenler](luis-concept-patterns.md)|Uygulama başına 500 desen.<br>Desen maksimum uzunluğu 400 karakterdir.<br>3 Desen.desen başına herhangi bir varlıklar<br>Desende en fazla 2 iç içe isteğe bağlı metin|
| [Desen.herhangi bir](./luis-concept-entity-types.md)|Uygulama başına 100, desen başına 3 desen.desen başına herhangi bir varlık |
| [Tümcecik listesi][phrase-list]|500 ifade listesi. Özellik sınırı olarak model nedeniyle 10 genel ifade listesi. Değiştirilemez ifade listesi en fazla 5.000 tümcecike sahiptir. Değiştirilebilir İfade Listesi'nde en fazla 50.000 ifade vardır. 500.000 tümcecikuygulama başına toplam tümceciklerin maksimum sayısı.|
| [Önceden oluşturulmuş varlıklar](./luis-prebuilt-entities.md) | limitsiz|
| [Normal ifade varlıkları](./luis-concept-entity-types.md)|20 varlık<br>500 karakter max. normal ifade varlık deseni başına|
| [Roller](luis-concept-roles.md)|Uygulama başına 300 rol. Varlık başına 10 rol|
| [İfade][utterances] | 500 karakter|
| [Konuşmalar][utterances] | Uygulama başına 15.000 - niyet başına söyleyiş sayısında bir sınır yoktur|
| [Sürümler](luis-concept-version.md)| Uygulama başına 100 sürüm |
| [Sürüm adı][luis-how-to-manage-versions] | Alfanümerik ve dönemle sınırlı 10 karakter (.) |

*Varsayılan karakter max 50 karakterdir.

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Ad benzersizliği

Aşağıdaki adlandırma benzersizlik kurallarını kullanın.

Aşağıdaki bir LUIS uygulaması içinde benzersiz olmalıdır:

* sürüm adı
* amaç
* varlık
* roles

Uygulanan kapsam içinde aşağıdaki benzersiz olmalıdır:

* tümcecik listesi

## <a name="object-naming"></a>Nesne adlandırma

Aşağıdaki adlarda aşağıdaki karakterleri kullanmayın.

|Nesne|Karakterleri hariç tutma|
|--|--|
|Amaç, varlık ve rol adları|`:`<br>`$` <br> `&`|
|Sürüm adı|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="key-usage"></a>Anahtar kullanımı

Language Understand'ın ayrı anahtarları, yazmak için bir türü ve tahmin bitiş noktasını sorgulamak için bir türü vardır. Anahtar türleri arasındaki farklar hakkında daha fazla bilgi edinmek [için, LUIS'te Yazma ve sorgu tahmin bitiş noktası anahtarlarına](luis-concept-keys.md)bakın.

<a name="key-limits"></a>

## <a name="resource-key-limits"></a>Kaynak anahtar sınırları

Kaynak anahtarlarının yazma ve bitiş noktası için farklı sınırları vardır. LUIS tahmin sorgusu bitiş noktası anahtarı yalnızca bitiş noktası sorguları için geçerlidir.

* Azure yazma kaynağı başına 500 uygulama

|Anahtar|Yazma|Uç Nokta|Amaç|
|--|--|--|--|
|Başlangıç|1 milyon/ay, 5/saniye|1/ay, 5/saniye|LUIS uygulamanızı yazma|
|F0 - Ücretsiz katman |1 milyon/ay, 5/saniye|10 bin/ay, 5/saniye|LUIS bitiş noktanızı sorgulama|
|S0 - Temel katman|-|50/saniye|LUIS bitiş noktanızı sorgulama|
|S0 - Standart katman|-|50/saniye|LUIS bitiş noktanızı sorgulama|
|[Duygusal analiz entegrasyonu](luis-how-to-publish-app.md#enable-sentiment-analysis)|-|-|Başka bir Azure kaynağı gerektirmeden anahtar tümcecik veri ayıklama dahil olmak üzere duyarlılık bilgileri ekleme sağlanır. |
|[Konuşma entegrasyonu](../speech-service/how-to-recognize-intents-from-speech-csharp.md)|-|Birim maliyet başına 1 bin uç nokta isteği|Konuşulan söyleyiyi metin söyleyerek dönüştürün ve LUIS sonuçlarını döndürün|

[Fiyatlandırma hakkında daha fazla bilgi edinin.][pricing]

## <a name="keyboard-controls"></a>Klavye kontrolleri

|Klavye girişi | Açıklama |
|--|--|
|Kontrol+E|belirtmeler listesindeki belirteçler ve varlıklar arasında geçişler|

## <a name="website-sign-in-time-period"></a>Web sitesi oturum açma süresi

Oturum açma erişiminiz **60 dakika**dır. Bu süreden sonra bu hatayı alırsınız. Tekrar oturum açman gerekiyor.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
