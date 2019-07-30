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
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b962fc32cdcde0509cfa60d105022bb208633ae3
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639295"
---
# <a name="batch-testing-with-1000-utterances-in-luis-portal"></a>LUSıS portalındaki 1000 ile Batch testleri

Toplu test doğrular, [etkin](luis-concept-version.md#active-version) tahmin doğruluğunun ölçmek için eğitilen modeli. Batch testi, geçerli eğitilen modelinizdeki her bir amaç ve varlığın doğruluğunu görüntülemenize yardımcı olur ve sonuçları grafik ile görüntüler. Uygulamanız için doğru amacını tanımlamak sık sık başarısız olursa bir amaç için daha fazla örnek Konuşma ekleme gibi doğruluğunu, artırmak için uygun eylemde toplu test sonuçlarını gözden geçirin.

## <a name="group-data-for-batch-test"></a>Toplu test grubu verileri

Toplu test etmek için kullanılan konuşma LUIS için yeni önemlidir. Bir dizi veri kümesi varsa, bu noktaları üç küme içine ayırın: bir amaca eklenen örnek, yayımlanan uç noktasından alınan utterler ve eğitilen işlem testi için kullanılan utterslar. 

## <a name="a-data-set-of-utterances"></a>Bir dizi veri kümesi

Toplu işlem testi için *veri kümesi*olarak bilinen uttersünlerdeki toplu iş dosyasını gönder. Veri kümesi, **mükerrer olmayan** bir şekilde etiketlenmiş en fazla 1.000 içeren JSON biçimli bir dosyadır. Bir uygulamada en fazla 10 veri kümesi test edebilirsiniz. Daha fazla test etmeniz gerekiyorsa, bir veri kümesini silip yeni bir tane ekleyin.

|**kuralları**|
|--|
|\* Hiçbir yinelenen konuşma|
|Konuşma 1000 veya daha az|

\* Çoğaltmaları tam dize eşleşmeleri, ilk simgeleştirilmiş eşleşme olarak kabul edilir. 

## <a name="entities-allowed-in-batch-tests"></a>Batch testlerinde izin verilen varlıklar

Toplu dosya verilerine karşılık gelen hiçbir varlık olsa modelinde tüm özel varlıklar toplu test varlıkları filtrede görünür.

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>

## <a name="batch-file-format"></a>Toplu dosya biçimi

Toplu iş dosyası konuşma oluşur. Her utterance yanı sıra herhangi bir beklenen hedefi tahmin olmalıdır [makine öğrenilen varlıklar](luis-concept-entity-types.md#types-of-entities) algılanamayacak kadar bekler. 

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

Toplu iş dosyasını kullanan **startPos** ve **endPos** özellikleri başlangıcını ve bitişini bir varlığın unutmayın. Değerleri sıfır tabanlı olduklarını ve başlamamalı veya boşluk ile bitemez. Bu, startIndex ve endIndex özelliklerini kullanma sorgu günlüklerinden farklıdır. 

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

Varlıkları test etmek istemiyorsanız, `entities` özelliği ekleyin ve değeri boş bir `[]`dizi olarak ayarlayın.


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

Veri noktası üzerinde **yanlış pozitif** ve **False negatif** bölümleri araştırılmalıdır hataları gösterir. Tüm veri noktaları **doğru pozitif** ve **gerçek negatif** bölümlerdir, bu veri kümesinde uygulamanızın doğruluğu mükemmeldir.

![Grafik dört bölüm](./media/luis-concept-batch-test/chart-sections.png)

Bu grafik, geçerli eğitimle yanlış bağlı öngörür LUIS konuşma bulmanıza yardımcı olur. Sonuçlar, grafiğin bölge başına görüntülenir. Utterance bilgileri gözden geçirin veya bu bölgede utterance sonuçlarını gözden geçirmek için bölge adı seçmek için grafik üzerinde tek tek noktaları seçin.

![Toplu işe testi](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Hata sonuçları

Toplu test hataları, toplu iş dosyasında belirtildiği gibi tahmin değil hedefleri belirtin. Hataları, grafiğin kırmızı iki bölümde gösterilir. 

Yanlış pozitif bölümüne sahip olmamalıdır, bir utterance bir amacı ya da varlık eşleştiğini gösterir. False negatif sahip olmalıdır, bir utterance bir amacı ya da varlık eşleşmedi gösterir. 

## <a name="fixing-batch-errors"></a>Toplu iş hataları düzeltme

Hatalar varsa batch testinde, ya da bir amaç için daha fazla Konuşma ekleme ve/veya daha fazla konuşma varlıkla LUIS amaçları arasında Ayrımcılığı olun yardımcı olmak için etiket. Konuşma eklediniz ve onları ve hala get toplu test hataları tahmin etiketli olsa eklemeyi göz önünde bir [tümcecik listesi](luis-concept-feature.md) özelliğiyle daha hızlı bilgi LUIS yardımcı olmak için etki alanına özel sözlük. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [toplu test](luis-how-to-batch-test.md)
