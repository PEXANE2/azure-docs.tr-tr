---
title: 'Öğretici: Kompozit varlık öğretici - LUIS'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, çıkarılan çeşitli türleri tek bir varlık içine demetlemek için bileşik bir varlık ekleyin. İstemci uygulaması, verileri biraraya getirebilir ve ilgili verileri farklı veri türlerinde kolayca ayıklayabilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: f2b2c3f52610cd9fae0845b15aebf032a088000b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75447958"
---
# <a name="tutorial-group-and-extract-related-data"></a>Öğretici: İlgili verileri gruplandırma ve ayıklama
Bu öğreticide, çıkarılan çeşitli türleri tek bir varlık içine demetlemek için bileşik bir varlık ekleyin. İstemci uygulaması, verileri biraraya getirebilir ve ilgili verileri farklı veri türlerinde kolayca ayıklayabilir.

Bileşik varlığın amacı, ilgili varlıkları bir üst kategori varlığında gruplandırmaktır. Bileşik oluşturulmadan önce bilgiler ayrı varlıklar olarak var.

Bileşik varlık bu veri türü için iyi bir uyum, çünkü veri:

* Birbirleri ile akrabalar.
* Çeşitli varlık türleri kullanın.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içe aktarma
> * Amaç oluşturma
> * Bileşik varlık ekleme
> * Eğitim
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Örnek uygulamayı içe aktarma

1.  [Uygulama JSON dosyasını](
https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/build-app/tutorial_list.json?raw=true) Liste varlık öğreticisinden indirin ve kaydedin.

2. JSON'u [LUIS portalını](https://www.luis.ai)kullanarak yeni bir uygulamaya aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `composite` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="composite-entity"></a>Bileşik varlık

Bu uygulamada, bölüm adı **Bölüm** listesi varlığında tanımlanır ve eşanlamlıları içerir.

**TransferEmployeeToDepartment** amacı, bir çalışanın yeni bir departmana taşınmasını istemek için örnek bir sözlüğe sahiptir.

Bu amaç için örnek söz:

|Örnek konuşmalar|
|--|
|John W. Smith'i muhasebe bölümüne taşıyın|
|R&D jill Jones transferi|

Taşıma isteği bölüm adını ve çalışan adını içermelidir.

## <a name="add-the-personname-prebuilt-entity-to-help-with-common-data-type-extraction"></a>Ortak veri türü çıkarma ile yardımcı olmak için PersonName önceden oluşturulmuş varlık ekleyin

LUIS, ortak veri ayıklama işlemi için önceden oluşturulmuş birkaç varlık sunar.

1. Üst gezintiden **Oluştur'u** seçin ve ardından soldaki gezinti menüsünden **Varlıklar'ı** seçin.

1. **Önceden oluşturulmuş varlıkları yönet** düğmesini seçin.

1. Önceden oluşturulmuş varlıklar listesinden **[Kişi Adı'nı](luis-reference-prebuilt-person.md)** seçin ve **ardından Bitti'yi**seçin.

    ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-tutorial-composite-entity/add-personname-prebuilt-entity.png)

    Bu varlık, istemci uygulamanıza ad tanıma eklemenize yardımcı olur.

## <a name="create-composite-entity-from-example-utterances"></a>Örnek söyleyişlerden bileşik varlık oluşturma

1. Sol gezinti bölmesinden **Intents** (Amaçlar) öğesini seçin.

1. Niyet listesinden **TransferEmployeeToDepartment'ı** seçin.

1. Söyleyiş, `place John Jackson in engineering`personName varlık seçin, `John Jackson`sonra aşağıdaki söyleyerek için açılan menü listesinde **bileşik varlık Sarın** seçin.

    ![Açılan iletişim kutusunda kaydırma bileşik seçimi ekran görüntüsü](./media/luis-tutorial-composite-entity/hr-create-composite-entity-1.png)

1. Sonra hemen son varlığı `engineering` seçin, söyleyerek. Bileşik varlığı gösteren seçili sözcüklerin altına yeşil çubuk çizilir. Açılan menüde bileşik adı `TransferEmployeeInfo` girin ve ardından enter'u seçin.

    ![Açılan iletişim kutusuna bileşik ad girme ekran görüntüsü](./media/luis-tutorial-composite-entity/hr-create-composite-entity-2.png)

1. **Ne tür bir varlık oluşturmak istiyorsunuz?** `personName` `Department` **Done** (Bitti) öğesini seçin. Önceden oluşturulmuş varlık olan personName'nin bileşik varlığa eklenmiştir. Bileşik varlığın başlangıcı ve bitiş belirteçleri arasında önceden oluşturulmuş bir varlık görüntülenebilseydiniz, bileşik varlığın bu önceden oluşturulmuş varlıkları içermesi gerekir. Önceden oluşturulmuş varlıklar dahil edilmezse, bileşik varlık doğru tahmin edilmez, ancak her bir öğe dir.

    ![Açılan iletişim kutusuna bileşik ad girme ekran görüntüsü](./media/luis-tutorial-composite-entity/hr-create-composite-entity-3.png)

## <a name="label-example-utterances-with-composite-entity"></a>Bileşik varlıklı etiket örneği söyleyemeleri

1. Her örnekte, bileşikte olması gereken en sol daki varlığı seçin. Ardından **bileşik varlıkta Sarın'ı**seçin.

1. Bileşik varlıktaki son sözcüğü seçin ve açılan menüden **TransferEmployeeInfo'yu** seçin.

1. Amaçtaki tüm tüm söyleyemelerin bileşik varlıkla etiketlendiğini doğrulayın.

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulamada yapılan değişikliklerin test edilebilmeleri için uygulamayı eğitin

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitimli modelin bitiş noktasından sorgulanabilir olması için uygulamayı yayımlayın

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Bitiş noktasından niyet ve varlık tahmini alın

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Move Jill Jones to DevOps` yazın. Son querystring parametresi , sözcük sorgusudur. `q`

    Bu test, bileşimin doğru şekilde ayıklanmış olduğunu doğrulamak için olduğundan, bir test varolan bir örnek söyleyiş veya yeni bir söyleyiş içerebilir. İyi bir test, tüm alt varlıkları bileşik varlığa dahil etmektir.

    ```json
    {
      "query": "Move Jill Jones to DevOps",
      "topScoringIntent": {
        "intent": "TransferEmployeeToDepartment",
        "score": 0.9882747
      },
      "intents": [
        {
          "intent": "TransferEmployeeToDepartment",
          "score": 0.9882747
        },
        {
          "intent": "None",
          "score": 0.00925369747
        }
      ],
      "entities": [
        {
          "entity": "jill jones",
          "type": "builtin.personName",
          "startIndex": 5,
          "endIndex": 14
        },
        {
          "entity": "devops",
          "type": "Department",
          "startIndex": 19,
          "endIndex": 24,
          "resolution": {
            "values": [
              "Development Operations"
            ]
          }
        },
        {
          "entity": "jill jones to devops",
          "type": "TransferEmployeeInfo",
          "startIndex": 5,
          "endIndex": 24,
          "score": 0.9607566
        }
      ],
      "compositeEntities": [
        {
          "parentType": "TransferEmployeeInfo",
          "value": "jill jones to devops",
          "children": [
            {
              "type": "builtin.personName",
              "value": "jill jones"
            },
            {
              "type": "Department",
              "value": "devops"
            }
          ]
        }
      ]
    }
    ```

   Bu söyleyiş, bileşik varlıklar dizini döndürür. Her varlığa bir tür ve değer verilir. Her alt varlık için daha fazla kesinlik bulmak için, varlıklar dizisinde karşılık gelen öğeyi bulmak için bileşik dizi öğesinden tür ve değer birleşimini kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık öğreticilerini listele](luis-quickstart-intents-only.md)
* [Bileşik varlık](luis-concept-entity-types.md) kavramsal bilgi
* [Nasıl eğitilir?](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUIS portalında test nasıl](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, varolan varlıkları kapsüllemek için bileşik bir varlık oluşturdu. Bu, istemci uygulamasının konuşmaya devam etmek için farklı veri türlerinde ilgili veri grubunu bulmasına olanak tanır. Bu İnsan Kaynakları uygulaması için bir istemci uygulaması, hareketin hangi gün ve saatte başlaması ve sona ermesi gerektiğini sorabilir. Ayrıca fiziksel bir telefon gibi hareket diğer lojistik hakkında sorabilir.

> [!div class="nextstepaction"]
> [Uç nokta söyleyerek emin olmayan tahminleri düzeltme](luis-tutorial-review-endpoint-utterances.md)
