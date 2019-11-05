---
title: Language Understanding için önceden oluşturulmuş modeller
titleSuffix: Azure Cognitive Services
description: LUO, yaygın olarak kullanılan, konuşma Kullanıcı senaryolarını hızlıca eklemek için önceden oluşturulmuş bir modeller kümesi içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 2e90a5f3f7cd2cfde2a2ead26674d2348a24ae6f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73507786"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Yaygın kullanım senaryoları için önceden oluşturulmuş modeller ekleyin 

LUO, genel amaçları ve kullanımları hızla eklemek için önceden oluşturulmuş etki alanlarından önceden oluşturulmuş amaçlar kümesi içerir. Bu yetenekler için modeller tasarlamaya gerek kalmadan, konuşma istemci uygulamanıza yetenekler eklemenin hızlı ve kolay bir yoludur. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="add-a-prebuilt-domain"></a>Önceden oluşturulmuş bir alan ekleme

1. **Uygulamalarım** sayfasında, uygulamanızı seçin. Bu, uygulamanızı uygulamanın **Build** bölümüne açar. 

1. **Amaçlar** sayfasında, sol taraftaki araç çubuğundan **önceden oluşturulmuş etki alanları Ekle** ' yi seçin. 

1. **Takvim** hedefini seçin ve ardından **etki alanı Ekle** düğmesini seçin.

    ![Takvim önceden oluşturulmuş etki alanı Ekle](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Takvim amaçlarını görüntülemek için sol gezinti bölmesinde **amaçlar** ' ı seçin. Bu etki alanındaki her amaç `Calendar.`ön ekine sahiptir. Diğer bir deyişle, bu etki alanı için iki varlık uygulamaya eklenir: `Calendar.Location` ve `Calendar.Subject`. 

### <a name="train-and-publish"></a>Eğitme ve yayımlama

1. Etki alanı eklendikten sonra üst ve sağ araç çubuğunda **eğitme** ' i seçerek uygulamayı eğitme. 

1. Üstteki araç çubuğunda **Yayımla**' yı seçin. **Üretimde**yayımlayın. 

1. Yeşil başarı bildirimi göründüğünde, uç noktaları görmek için **uç noktalar listesine başvurun** bağlantısı ' nı seçin.

1. Bir uç nokta seçin. Bu uç nokta için yeni bir tarayıcı sekmesi açılır. Tarayıcı sekmesini açık tutun ve **Test** bölümüne devam edin.

### <a name="test"></a>Test etme

**Q** parametresi için bir değer ekleyerek yeni hedefi uç noktada test edin: `Schedule a meeting with John Smith in Seattle next week`.

LUO, doğru amacı ve toplantı konusunu döndürür:

```json
{
  "query": "Schedule a meeting with John Smith in Seattle next week",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.824783146
  },
  "entities": [
    {
      "entity": "a meeting with john smith",
      "type": "Calendar.Subject",
      "startIndex": 9,
      "endIndex": 33,
      "score": 0.484055847
    }
  ]
}
```

## <a name="add-a-prebuilt-intent"></a>Önceden oluşturulmuş bir amaç ekleyin

1. **Uygulamalarım** sayfasında, uygulamanızı seçin. Bu, uygulamanızı uygulamanın **Build** bölümüne açar. 

1. **Amaçlar** sayfasında, amaçlar listesinin üzerindeki araç çubuğundan **önceden oluşturulmuş amaç Ekle** ' yi seçin. 

1. Açılan iletişim kutusundan **yardımcı programlar. Cancel** hedefini seçin. 

    ![Önceden oluşturulmuş amaç Ekle](./media/luis-prebuilt-intents/prebuilt-intents-ddl.png)

1. **Bitti** düğmesini seçin.

### <a name="train-and-test"></a>Eğitme ve test

1. Amaç eklendikten sonra üst, sağ taraftaki araç çubuğunda **eğit** ' i seçerek uygulamayı eğitme. 

1. Sağ araç çubuğunda **Test** ' i seçerek yeni amacı test edin. 

1. Metin kutusuna, iptal etme için uttersları girin:

    |Test söylenişi|Tahmin puanı|
    |--|:--|
    |Uçuşumu iptal etmek istiyorum.|0,67|
    |Satın alma işlemini iptal edin.|0,52|
    |Toplantıyı iptal edin.|0,56|

    ![Test önceden oluşturulmuş amacı](./media/luis-prebuilt-intents/test.png)

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleyin

1. **Uygulamalarım** sayfasında adına tıklayarak uygulamanızı açın ve ardından sol taraftaki **varlıklar** ' a tıklayın. 

1. **Varlıklar** sayfasında, **önceden oluşturulmuş varlık Ekle**' ye tıklayın.

1. **Önceden oluşturulmuş varlıklar Ekle** iletişim kutusunda, datetimeV2 önceden oluşturulmuş varlığı seçin. 

    ![Önceden oluşturulmuş varlık Ekle iletişim kutusu](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. **Done** (Bitti) öğesini seçin. Varlık eklendikten sonra, uygulamayı eğmenize gerek kalmaz. 

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Ön derlenmiş modeli tahmin uç noktasından görüntülemek için Yayımla

Önceden oluşturulmuş bir modelin değerini görüntülemenin en kolay yolu, yayımlanan uç noktadan sorgu kullanmaktır. 

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Önceden oluşturulmuş bir varlık belirtecini içeren varlıkları işaretleme
 Bir özel varlık olarak işaretlemek istediğiniz `HH-1234`gibi bir metniniz varsa _ve_ modele [önceden oluşturduğunuz sayı](luis-reference-prebuilt-number.md) eklendiyse, bu özel varlığı Luu portalında işaretleyemezsiniz. Bunu API ile işaretleyebilirsiniz. 

 Bu tür belirteci işaretlemek için, bir kısmının önceden oluşturulmuş bir varlıkla zaten işaretlenmiş olması durumunda, önceden oluşturulmuş varlığı LUıS uygulamasında kaldırın. Uygulamayı eğmenize gerek yok. Sonra belirteci kendi özel varlığınızla işaretleyin. Ardından, önceden oluşturulmuş varlığı LUSıS uygulamasına geri ekleyin.

 Başka bir örnek için, bir sınıf tercihlerinin listesi olarak göz önünde bulundurun: `I want first year spanish, second year calculus, and fourth year english lit.` LUıS uygulamasının ön derleme sırası eklenirse, `first`, `second`ve `fourth` önceden sıra sayılıklarla işaretlenir. Sıra sayısını ve sınıfı yakalamak istiyorsanız, bileşik bir varlık oluşturabilir ve bunu önceden oluşturulmuş sıra ve sınıf adı için özel varlık etrafında sarmalayın.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [REST API 'lerle. csv 'den model oluşturma](./luis-tutorial-node-import-utterances-csv.md)
