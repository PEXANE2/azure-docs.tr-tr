---
title: Bileşik varlık öğreticisi-LUSıS
titleSuffix: Azure Cognitive Services
description: Çeşitli türlerde ayıklanan verileri içeren tek bir varlığa paket için bileşik bir varlık ekleyin. İstemci uygulama, verileri paketleme tarafından farklı veri türlerinde ilgili verileri kolayca ayıklayabilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 698635b3f216c556e1e36a033703b8786a028e38
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946127"
---
# <a name="tutorial-group-and-extract-related-data"></a>Öğretici: İlişkili verileri gruplandırma ve ayıklama
Bu öğreticide, bileşik bir varlık içeren tek bir varlığa çeşitli türlerde ayıklanan veri paketi ekleyin. İstemci uygulama, verileri paketleme tarafından farklı veri türlerinde ilgili verileri kolayca ayıklayabilirsiniz.

Bileşik varlık amacı bir üst kategori varlığa ilgili varlıkları grup. Bir bileşik oluşturulmadan önce bilgi ayrı varlıklar olarak bulunmaktadır. 

Bileşik varlık olduğundan bu veri türü için uygun olan veri:

* Birbiriyle ilgilidir. 
* Varlık türleri çeşitli kullanın.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içeri aktar
> * Amaç oluşturma
> * Bileşik varlık ekleme 
> * Eğitim
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Örnek uygulamayı içeri aktar

1.  [Uygulama json dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/build-app/tutorial_list.json) liste varlık öğreticiden indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `composite` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="composite-entity"></a>Bileşik varlık

Bu uygulamada, departman adı **Departman** listesi varlığında tanımlanmıştır ve eş anlamlılar içerir. 

**Transferemployeetodepartment** amacı, bir çalışanın yeni bir departmana taşınmasını istemek için örnek bir örnektir. 

Bu amaca yönelik örnek bildirimler şunlardır:

|Örnek konuşmalar|
|--|
|John W. Smith 'i muhasebe departmanına taşı|
|Jill Jones 'ten R & D 'ye aktar|
 
Taşıma isteği, Bölüm adı ve çalışan adı ' nı içermelidir. 

## <a name="add-the-personname-prebuilt-entity-to-help-with-common-data-type-extraction"></a>Ortak veri türü ayıklamasıyla ilgili yardım için PersonName önceden oluşturulmuş varlığını ekleyin

LUIS, ortak veri ayıklama işlemi için önceden oluşturulmuş birkaç varlık sunar. 

1. Üstteki gezinmede **Oluştur** ' u seçin ve ardından sol gezinti menüsünden **varlıklar** ' ı seçin.

1. **Önceden oluşturulmuş varlıkları yönet** düğmesini seçin.

1. Önceden oluşturulmuş varlıklar listesinden **[PersonName](luis-reference-prebuilt-person.md)** ' i seçin ve **bitti**' yi seçin.

    ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-tutorial-composite-entity/add-personname-prebuilt-entity.png)

    Bu varlık, istemci uygulamanıza ad tanıma eklemenize yardımcı olur.

## <a name="create-composite-entity-from-example-utterances"></a>Örnek utbotlardan Bileşik varlık oluşturma

1. Sol gezinti bölmesinden **Intents** (Amaçlar) öğesini seçin.

1. Amaçlar listesinden **Transferemployeetodepartment** öğesini seçin.

1. Bu arada, kişi adı `John Jackson`varlığını seçin ve ardından açılan menü listesinde aşağıdaki söylük için **bileşik varlıkta Wrap** ' ı seçin. `place John Jackson in engineering` 

    ![Açılan kutudan kaydırmayı kaydır iletişim kutusunun ekran görüntüsü](./media/luis-tutorial-composite-entity/hr-create-composite-entity-1.png)

1. Son varlık hemen ardından `engineering` utterance içinde. Bileşik bir varlığı gösteren sözcüklerin altında yeşil bir çubuk çizilir. Açılır menüde, bileşik adını `TransferEmployeeInfo` seçin ENTER. 

    ![Açılan liste iletişim kutusunda bileşik ad girme ekran görüntüsü](./media/luis-tutorial-composite-entity/hr-create-composite-entity-2.png)

1. **Ne tür bir varlık oluşturmak istiyorsunuz?** , gereken tüm alanlar listede: `personName` ve. `Department` **Done** (Bitti) öğesini seçin. Önceden oluşturulmuş varlığın, personName, bileşik varlığa eklendiğini unutmayın. Başlangıç ve bitiş belirteçleri bileşik bir varlığın arasında görünür önceden oluşturulmuş bir varlık olabilir, önceden oluşturulmuş bu varlıkların bileşik varlığı içermelidir. Önceden oluşturulmuş varlıklar dahil edilmez, bileşik bir varlık değil doğru şekilde tahmin edildiğinde ancak her tek öğedir.

    ![Açılan liste iletişim kutusunda bileşik ad girme ekran görüntüsü](./media/luis-tutorial-composite-entity/hr-create-composite-entity-3.png)

## <a name="label-example-utterances-with-composite-entity"></a>Bileşik varlıkla etiket örnek konuşma

1. Her örnek utterance içinde bileşik içinde olması gereken en soldaki varlığı seçin. Ardından **kaydırma bileşik varlıkta**.

1. Bileşik varlıktaki son sözcüğü seçin ve ardından açılan menüden **Transferemployeeınfo** öğesini seçin. 

1. Tüm konuşma amacı, bileşik bir varlık ile etiketlenmiş doğrulayın. 

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulamaya yapılan değişikliklerin test edilebilir olması için uygulamayı eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktadan sorgulanabilir olması için uygulamayı yayımlayın

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktadan amacı ve varlık tahminini alın 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Move Jill Jones to DevOps` yazın. Son sorgu dizesi parametresi `q`, utterance sorgu. 

    Bileşik doğru bir şekilde ayıklandıktan doğrulamak için bu test olduğundan, bir test ya da mevcut bir örnek utterance veya yeni bir utterance içerebilir. Bileşik varlıktaki tüm alt varlıklar eklemek iyi bir testtir.

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

   Bu utterance bir bileşik varlıkları dizisi döndürür. Her varlık türü ve değeri verilir. Her bir alt varlık için daha fazla duyarlık bulmak için karşılık gelen öğe varlıkları dizide bulmak için bileşik bir dizi öğesi türü ve bir kombinasyonu kullanın.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık öğreticisini listeleme](luis-quickstart-intents-only.md)
* [Bileşik varlık](luis-concept-entity-types.md) kavramsal bilgileri
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, var olan varlıkları kapsüllemek için bileşik bir varlık oluşturuldu. Bu konuşmaya devam etmek için farklı veri türleri ilgili bir veri grubu bulmak istemci uygulaması sağlar. Bu İnsan Kaynakları uygulamasında, bir istemci uygulamanın hangi gün ve saat taşıma başlamalı ve bitmelidir gerekiyor sorabilirsiniz. Ayrıca, fiziksel telefon gibi diğer bir taşıma için başka bir lojisinden da sorabilir. 

> [!div class="nextstepaction"] 
> [Bir ifade listesi tek bir varlığın eklemeyi öğrenin](luis-quickstart-primary-and-secondary-data.md)  
