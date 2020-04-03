---
title: Limitler - LUIS
description: Bu makale, Azure Bilişsel Hizmetler Dil Anlayışının (LUIS) bilinen sınırlarını içerir. LUIS'in birkaç sınır alanı vardır. Model sınır, LUIS'teki amaçları, varlıkları ve özellikleri denetler. Anahtar türüne göre kota sınırları. Klavye kombinasyonu LUIS web sitesini kontrol eder.
ms.topic: reference
ms.date: 04/02/2020
ms.openlocfilehash: 4aa69cb0fd36fe5bf4ea2928022aea602b8830d6
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618862"
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
| [Tümcecik listesi][phrase-list]|500 ifade listesi. Özellik sınırı olarak model nedeniyle 10 genel ifade listesi. Değiştirilemez tümcecik listesi en fazla 5.000 tümcecike sahiptir. Değiştirilebilir tümcecik listesi en fazla 50.000 tümcecike sahiptir. 500.000 tümcecikuygulama başına toplam tümceciklerin maksimum sayısı.|
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

## <a name="resource-usage-and-limits"></a>Kaynak kullanımı ve sınırları

Language Understand ayrı kaynaklara, yazma için bir türüne ve tahmin bitiş noktasını sorgulamak için bir türüne sahiptir. Anahtar türleri arasındaki farklar hakkında daha fazla bilgi edinmek [için, LUIS'te Yazma ve sorgu tahmin bitiş noktası anahtarlarına](luis-concept-keys.md)bakın.

<a name="key-limits"></a>

### <a name="authoring-resource-limits"></a>Kaynak sınırları yazma

Azure portalındaki `LUIS.Authoring`kaynakları filtrelerken bu _tür_, kullanın. LUIS, Azure yazma kaynağı başına 500 uygulamayı sınırlandırıyor.

|Kaynak yazma|TPS yazma|
|--|--|
|Başlangıç|1 milyon/ay, 5/saniye|
|F0 - Ücretsiz katman |1 milyon/ay, 5/saniye|

* TPS = Saniyedeki işlemler

[Fiyatlandırma hakkında daha fazla bilgi edinin.][pricing]

### <a name="query-prediction-resource-limits"></a>Sorgu tahmin kaynak sınırları

Azure portalındaki `LUIS`kaynakları filtrelerken bu _tür_, kullanın. Çalışma zamanında kullanılan LUIS sorgusu tahmin bitiş noktası kaynağı yalnızca bitiş noktası sorguları için geçerlidir.

|Sorgu Tahmin kaynağı|TPS sorgula|
|--|--|
|F0 - Ücretsiz katman |10 bin/ay, 5/saniye|
|S0 - Standart katman|50/saniye|

### <a name="sentiment-analysis"></a>Yaklaşım analizi

Duyarlılık bilgisi sağlayan [duygu analizi tümleştirmesi,](luis-how-to-publish-app.md#enable-sentiment-analysis)başka bir Azure kaynağı gerektirmeden sağlanır.

### <a name="speech-integration"></a>Konuşma entegrasyonu

[Konuşma tümleştirmesi](../speech-service/how-to-recognize-intents-from-speech-csharp.md) birim maliyet başına 1bin uç nokta isteği sağlar.

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
