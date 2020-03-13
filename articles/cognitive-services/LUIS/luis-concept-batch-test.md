---
title: Batch testi-LUSıS
titleSuffix: Azure Cognitive Services
description: Toplu test uygulama geliştirebilirsiniz ve kendi dil anlama geliştirmek için sürekli olarak çalışmak için kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: e9ad7c52af20762633c710b39a64fbebf0cf6213
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79220055"
---
# <a name="batch-testing-with-1000-utterances-in-luis-portal"></a>LUSıS portalındaki 1000 ile Batch testleri

Toplu işlem testi, etkin eğitilen sürümünüzü tahmin doğruluğunu ölçecek şekilde doğrular. Batch testi, etkin sürümünüzde her bir amaç ve varlığın doğruluğunu görüntülemenize yardımcı olur ve sonuçları grafik ile görüntüler. Uygulamanızın bir amaca göre doğru amacı belirleyemediği veya etiketlenmesini sağlamak için bir amaca daha fazla örnek ekleme gibi bir amaca yönelik daha fazla örnek ekleme gibi işlem testi sonuçlarını gözden geçirin.

## <a name="group-data-for-batch-test"></a>Toplu test grubu verileri

Toplu test etmek için kullanılan konuşma LUIS için yeni önemlidir. Bir dizi veri kümesi varsa, bu noktaları üç küme içine ayırın: bir amaca eklenen örnek, yayımlanan uç noktasından alınan utterler ve eğitilen işlem testi için kullanılan utterslar. 

## <a name="a-data-set-of-utterances"></a>Bir dizi veri kümesi

Toplu işlem testi için *veri kümesi*olarak bilinen uttersünlerdeki toplu iş dosyasını gönder. Veri kümesi, **mükerrer olmayan** bir şekilde etiketlenmiş en fazla 1.000 içeren JSON biçimli bir dosyadır. Bir uygulamada en fazla 10 veri kümesi test edebilirsiniz. Daha fazla test etmeniz gerekiyorsa, bir veri kümesini silip yeni bir tane ekleyin.

|**Kuralın**|
|--|
|\* Hiçbir yinelenen konuşma|
|Konuşma 1000 veya daha az|

\* Çoğaltmaları tam dize eşleşmeleri, ilk simgeleştirilmiş eşleşme olarak kabul edilir. 

## <a name="entities-allowed-in-batch-tests"></a>Batch testlerinde izin verilen varlıklar

Toplu dosya verilerine karşılık gelen hiçbir varlık olsa modelinde tüm özel varlıklar toplu test varlıkları filtrede görünür.

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>

## <a name="batch-file-format"></a>Toplu dosya biçimi

Toplu iş dosyası konuşma oluşur. Her bir söylemeye, algılanmak istediğiniz [makine tarafından öğrenilen varlıkların](luis-concept-entity-types.md#types-of-entities) yanında beklenen bir amaç tahmini olmalıdır. 

## <a name="batch-syntax-template-for-intents-with-entities"></a>Varlıklar içeren amaçlar için Batch sözdizimi şablonu

Toplu iş dosyasında başlatmak için aşağıdaki şablonu kullanın:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": 
    [
        {
            "entity": "entity name 1 goes here",
            "startPos": 14,
            "endPos": 23
        },
        {
            "entity": "entity name 2 goes here",
            "startPos": 14,
            "endPos": 23
        }
    ]
  }
]
```

Toplu iş dosyası, bir varlığın başlangıcını ve sonunu not etmek için **startPos** ve **endPos** özelliklerini kullanır. Değerleri sıfır tabanlı olduklarını ve başlamamalı veya boşluk ile bitemez. Bu, startIndex ve endIndex özelliklerini kullanma sorgu günlüklerinden farklıdır. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="batch-syntax-template-for-intents-without-entities"></a>Varlıklar olmadan amaçlar için Batch sözdizimi şablonu

Toplu iş dosyanızı varlıklar olmadan başlatmak için aşağıdaki şablonu kullanın:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": []
  }
]
```

Varlıkları test etmek istemiyorsanız, `entities` özelliğini dahil edin ve değeri boş bir dizi olarak ayarlayın `[]`.


## <a name="common-errors-importing-a-batch"></a>Sık karşılaşılan toplu içeri aktarma

Sık karşılaşılan hatalar şunlardır: 

> * 1\. 000'den fazla konuşma
> * Bir varlık özelliğine sahip olmayan bir utterance JSON nesnesi. Özelliği, boş bir dizi olabilir.
> * İçinde birden çok varlık etiketli sözcükler
> * Başlangıç ve bitiş boşluk varlık etiketi.

## <a name="batch-test-state"></a>Toplu test durumu

LUO, her bir veri kümesinin son testinin durumunu izler. Bu, son çalıştırma boyutu (konuşma toplu iş sayısı), tarih ve son sonucu (başarılı bir şekilde tahmin edilen konuşma sayısı) içerir.

<a name="sections-of-the-results-chart"></a>

## <a name="batch-test-results"></a>Toplu test sonuçları

Toplu test sonucu bir hata matris bilinen bir dağılım grafiği olur. Toplu iş dosyası, geçerli modelin tahmin edilen amaç ve varlıkları konuşma 4 yönlü karşılaştırması grafiğidir. 

Hatalı **pozitif** ve **yanlış negatif** bölümlerdeki veri noktaları Araştırılması gereken hataları gösterir. Tüm veri noktaları **doğru pozitif** ve **gerçek negatif** bölümlerdir, bu veri kümesinde uygulamanızın doğruluğu mükemmeldir.

![Grafik dört bölüm](./media/luis-concept-batch-test/chart-sections.png)

Bu grafik, geçerli eğitimle yanlış bağlı öngörür LUIS konuşma bulmanıza yardımcı olur. Sonuçlar, grafiğin bölge başına görüntülenir. Utterance bilgileri gözden geçirin veya bu bölgede utterance sonuçlarını gözden geçirmek için bölge adı seçmek için grafik üzerinde tek tek noktaları seçin.

![Toplu işe testi](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Hata sonuçları

Toplu test hataları, toplu iş dosyasında belirtildiği gibi tahmin değil hedefleri belirtin. Hataları, grafiğin kırmızı iki bölümde gösterilir. 

Yanlış pozitif bölümüne sahip olmamalıdır, bir utterance bir amacı ya da varlık eşleştiğini gösterir. False negatif sahip olmalıdır, bir utterance bir amacı ya da varlık eşleşmedi gösterir. 

## <a name="fixing-batch-errors"></a>Toplu iş hataları düzeltme

Hatalar varsa batch testinde, ya da bir amaç için daha fazla Konuşma ekleme ve/veya daha fazla konuşma varlıkla LUIS amaçları arasında Ayrımcılığı olun yardımcı olmak için etiket. Uttersları eklediyseniz ve bunları etiketlendirmeye devam ediyorsa ve toplu iş testinde tahmin hataları almaya devam ediyorsanız, LUL 'nın daha hızlı öğrendiğine yardımcı olması için alana özgü sözlük içeren bir [ifade listesi](luis-concept-feature.md) özelliği eklemeyi göz önünde bulundurun. 

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu işi test](luis-how-to-batch-test.md) etme hakkında bilgi edinin
