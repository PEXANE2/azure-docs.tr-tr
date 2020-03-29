---
title: Toplu test - LUIS
titleSuffix: Azure Cognitive Services
description: Uygulamanızı hassaslaştırmak ve dil anlayışını geliştirmek için sürekli olarak uygulamanız üzerinde çalışmak için toplu test kullanın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220055"
---
# <a name="batch-testing-with-1000-utterances-in-luis-portal"></a>LUIS portalında 1000 utterances ile toplu test

Toplu işlem, tahmin doğruluğunu ölçmek için etkin eğitimli sürümünüzü doğrular. Toplu iş testi, etkin sürümünüzdeki her amaç ve varlığın doğruluğunu görüntülemenize yardımcı olur ve sonuçları bir grafikle görüntüler. Uygulamanız sık sık doğru niyeti tanımlamada başarısız olursa veya sözcük teki varlıkları etiketlemede başarısız olursa, bir amaca daha fazla örnek sözcük eklemek gibi doğruluğu artırmak için uygun eylemi yapmak için toplu iş testi sonuçlarını gözden geçirin.

## <a name="group-data-for-batch-test"></a>Toplu iş testi için grup verileri

Toplu test için kullanılan söyleyişlerin LUIS için yeni olması önemlidir. Bir veri söyleyin, söyleyerekleri üç kümeye bölün: bir niyete eklenen örnek söyleyifadeler, yayınlanan bitiş noktasından alınan söyleyifadeler ve luis'i eğitildikten sonra toplu olarak test etmek için kullanılan söyleyifadeler. 

## <a name="a-data-set-of-utterances"></a>Bir veri söyleyiş kümesi

Toplu işlem için *veri kümesi*olarak bilinen bir toplu iş dosyası gönderin. Veri kümesi, en fazla 1.000 olarak **etiketlenmiş, çoğaltılmayan** sözcükiçeren JSON biçimli bir dosyadır. Bir uygulamada en fazla 10 veri kümesini test edebilirsiniz. Daha fazla sınaması gerekiyorsa, bir veri kümesini silin ve ardından yeni bir tane ekleyin.

|**Kurallar**|
|--|
|*Yinelenen söz lerle ilgili değil|
|1000 veya daha az söz|

*Yinelenenler, ilk belirteçolan eşleşmeler değil, tam dize eşleşmeleri olarak kabul edilir. 

## <a name="entities-allowed-in-batch-tests"></a>Toplu iş testlerinde izin verilen varlıklar

Modeldeki tüm özel varlıklar, toplu iş dosyası verilerinde karşılık gelen varlıklar olmasa bile toplu iş testi varlıkları filtresinde görünür.

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>

## <a name="batch-file-format"></a>Toplu dosya biçimi

Toplu iş dosyası, sözlerden oluşur. Her söyleyiş, algılanmasını beklediğiniz [makineden öğrenilen varlıklarla](luis-concept-entity-types.md#types-of-entities) birlikte beklenen bir niyet tahminine sahip olmalıdır. 

## <a name="batch-syntax-template-for-intents-with-entities"></a>Varlıklarla olan niyetler için toplu sözdizimi şablonu

Toplu iş dosyanızı başlatmak için aşağıdaki şablonu kullanın:

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

Toplu iş dosyası, bir varlığın başlangıcını ve sonunu belirtmek için **startPos** ve **endPos** özelliklerini kullanır. Değerler sıfır tabanlıdır ve bir boşlukta başlamamalı veya bitmemelidir. Bu, startIndex ve endIndex özelliklerini kullanan sorgu günlüklerinden farklıdır. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="batch-syntax-template-for-intents-without-entities"></a>Varlıkları olmayan niyetler için toplu sözdizimi şablonu

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

Varlıkları sınamak istemiyorsanız, `entities` özelliği ekleyin ve değeri boş bir dizi `[]`olarak ayarlayın.


## <a name="common-errors-importing-a-batch"></a>Toplu iş alma da sık karşılaşılan hatalar

Sık karşılaşılan hatalar şunlardır: 

> * 1.000'den fazla söz
> * Bir varlık özelliği olmayan bir söyleyiş JSON nesnesi. Özellik boş bir dizi olabilir.
> * Birden çok varlıkta etiketlenmiş word(ler)
> * Varlık etiketi bir boşlukta başlayıp biten.

## <a name="batch-test-state"></a>Toplu test durumu

LUIS, her veri kümesinin son testinin durumunu izler. Bu boyut (toplu iş teki sözcük sayısı), son çalıştırma tarihini ve son sonucu (başarıyla tahmin edilen sözcük sayısı) içerir.

<a name="sections-of-the-results-chart"></a>

## <a name="batch-test-results"></a>Toplu test sonuçları

Toplu işlem sonucu, hata matrisi olarak bilinen bir dağılım grafiğidir. Bu grafik, toplu iş dosyasındaki ve geçerli modelin öngörülen amaç ve varlıklarının 4 yönlü bir karşılaştırmasýr. 

**Yanlış Pozitif** ve **Yanlış Negatif** bölümlerindeki veri noktaları, araştırılması gereken hataları gösterir. Tüm veri noktaları **True Positive** ve **True Negative** bölümlerindeyse, uygulamanızın doğruluğu bu veri setinde mükemmeldir.

![Grafiğin dört bölümü](./media/luis-concept-batch-test/chart-sections.png)

Bu grafik, LUIS'in mevcut eğitimine göre yanlış tahmin ettiği söyleyişleri bulmanıza yardımcı olur. Sonuçlar grafiğin bölgesine göre görüntülenir. Sözleşme bilgilerini gözden geçirmek için grafikteki tek tek noktaları veya o bölgedeki söz leşme sonuçlarını gözden geçirlemek için bölge adını seçin.

![Toplu işe testi](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Sonuçlardaki hatalar

Toplu iş testindeki hatalar, toplu iş dosyasında belirtildiği gibi öngörülmeyen amaçları gösterir. Hatalar grafiğin iki kırmızı bölümünde gösterilir. 

Yanlış pozitif bölüm, bir söyleymenin bir niyet veya varlıkla eşleşmesi gerekirken olduğunu gösterir. Yanlış negatif, bir söyleyişe sahip olması gerektiği zaman bir niyet veya varlıkla eşleşmediğini gösterir. 

## <a name="fixing-batch-errors"></a>Toplu iş hatalarını düzeltme

Toplu iş testinde hatalar varsa, luis'in niyetler arasında ayrımcılık yapmasına yardımcı olmak için bir niyete daha fazla söz ekleyebilir ve/veya varlıkla daha fazla söz etiketleyebilirsiniz. Sözcük eklediyseniz ve bunları etiketlediyseniz ve toplu iş testinde yine de tahmin hataları aldıysanız, LUIS'in daha hızlı öğrenmesine yardımcı olmak için etki alanına özgü kelime [dağarcığına](luis-concept-feature.md) sahip bir ifade listesi özelliği eklemeyi düşünün. 

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu iş bir toplu iş test](luis-how-to-batch-test.md) etmeyi öğrenin
