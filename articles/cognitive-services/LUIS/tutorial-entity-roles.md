---
title: 'Öğretici: Roller ile bağlamsal veri - LUIS'
titleSuffix: Azure Cognitive Services
description: İlgili verileri içeriğe göre bulun. Örneğin, bir bina ya da ofisten başka bir bina ya da ofise fiziksel olarak taşınmada çıkış ve varış konumları.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: cd646ef061a0be06a9b1a56b72a4f35d9796aa63
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75447841"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Öğretici: Bir söyleyişten bağlamsal olarak ilgili verileri ayıklama

Bu öğreticide bağlama göre ilgili veri parçalarını bulacaksınız. Örneğin, bir şehirden diğerine aktarım için bir başlangıç ve hedef konumları. Her iki veri parçası gerekli olabilir ve birbirleriyle ilişkilidir.

Bir rol önceden oluşturulmuş veya özel varlık türüyle kullanılabilir ve hem örnek söz hem de desenlerde kullanılabilir.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni uygulama oluşturma
> * Amaç ekleme
> * Rolleri kullanarak başlangıç ve hedef bilgilerini alma
> * Eğitim
> * Yayımlama
> * Son noktadan niyet ve varlık rolleri alın

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>İlgili veriler

Bu uygulama, bir çalışanın başlangıç şehrinden hedef şehre nereye taşınacağını belirler. Şehir adlarını tanımlamak için geographyv2 önceden oluşturulmuş bir varlık kullanır ve söyleyiş içindeki konum türlerini (başlangıç ve hedef) belirlemek için rolleri kullanır.

Bir rol, varlık verilerini ayıklamak için kullanıldığında kullanılmalıdır:

* Söyleyiş bağlamında birbiriyle ilişkilidir.
* Her rolü belirtmek için belirli sözcük seçimi kullanır. Bu sözcüklere örnekler şunlardır: from/to, leaving/headed to, away from/toward (çıkış/varış, ayrılıyor/gidiyor, kaynaktan/hedefe doğru)
* Her iki rol de sık sık aynı söyleyiş içindedir ve LUIS'in bu sık bağlamsal kullanımdan ders almasına olanak sağlar.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

1. URL'si ile önizleme LUIS [https://preview.luis.ai](https://preview.luis.ai)portalında oturum açın.

1. **Yeni uygulama oluştur'u** `HumanResources` seçin, adı girin ve varsayılan kültür, **İngilizce**tutun. Açıklamayı boş bırakın.

1. **Done** (Bitti) öğesini seçin.

## <a name="create-an-intent-to-move-employees-between-cities"></a>Çalışanları şehirler arasında taşıma niyeti oluşturma

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin.

1. Açılan iletişim kutusuna `MoveEmployeeToCity` girip **Done** (Bitti) öğesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Create new intent (Yeni amaç oluştur) iletişim kutusunun ekran görüntüsü](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |Hareket John W. Smith Orlando başlı Seattle bırakarak|
    |Jill Jones'u Seattle'dan Kahire'ye transfer etmek|
    |John Jackson'ı Tampa'dan uzaklaştırın, Atlanta'ya geliyor. |
    |Debra Doughtery'yi Chicago'dan Tulsa'ya taşıyın|
    |mv Jill Jones Kahire'den ayrılarak Tampa'ya doğru yola çıktı|
    |Shift Alice Anderson Oakland Redmond gelen|
    |San Francisco'dan Redmond'a Carl Chamerlin|
    |Steve Standish'i San Diego'dan Bellevue'ye transfer et |
    |Kansas şehrinden Tanner Thompson asansör ve Chicago vardiya|

    > [!div class="mx-imgBorder"]
    > ![MoveEmployee amacındaki yeni konuşmaları gösteren LUIS uygulaması ekran görüntüsü](./media/tutorial-entity-roles/hr-enter-utterances.png)

## <a name="add-prebuilt-entity-geographyv2"></a>Önceden oluşturulmuş varlık coğrafyası ekleV2

Önceden oluşturulmuş varlık, geographyV2, şehir adları da dahil olmak üzere konum bilgilerini ayıklar. Bu bağlamda birbirleriyle ilgili iki şehir adı olduğundan, bu bağlamı ayıklamak için rolleri kullanın.

1. Sol taraftaki gezintiden **Varlıklar'ı** seçin.

1. **Önceden oluşturulmuş varlık ekle'yi**seçin ve ardından önceden oluşturulmuş varlıkları filtrelemek için arama çubuğunu seçin. `geo`

    > [!div class="mx-imgBorder"]
    > ![Uygulamaya geographyV2 önceden oluşturulmuş varlık ekleme](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)

1. Onay kutusunu seçin ve **Bitti'yi**seçin.
1. **Varlıklar** listesinde, yeni varlığı açmak için **geographyV2'yi** seçin.
1. İki rol `Origin`ekleyin `Destination`ve .

    > [!div class="mx-imgBorder"]
    > ![Önceden oluşturulmuş varlığa rol ekleme](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)

1. Sol taraftaki gezintiden **Niyetler'i** seçin ve ardından **MoveEmployeeToCity** amacını seçin. Dikkat edin şehir adları önceden oluşturulmuş varlık **coğrafyasıV2**ile etiketlenir.
1. Bağlam araç çubuğunda **Varlık paletini**seçin.

    > [!div class="mx-imgBorder"]
    > ![İçerik araç çubuğundan Entity Palette'yi seçin](media/tutorial-entity-roles/intent-detail-context-toolbar-select-entity-palette.png)

1. Önceden oluşturulmuş varlık, **geographyV2**seçin, sonra **Varlık denetçisi**seçin.
1. Taraf **denetçisinde,** bir rol seçin, **Hedef**. Bu, fare imlecini değiştirir. Use the cursor to label the text in all the utterances that is the destination location.

    > [!div class="mx-imgBorder"]
    > ![Varlık Paletinde Rol Seçin](media/tutorial-entity-roles/entity-palette-select-entity-role.png)


1. **Entity denetçisine**dön , Rolü **Origin'e değiştirin.** Use the cursor to label the text in all the utterances that is the origin location.

## <a name="add-example-utterances-to-the-none-intent"></a>Yok amacına örnek söz ekleme

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulamada yapılan değişikliklerin test edilebilmeleri için uygulamayı eğitin

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitimli modelin bitiş noktasından sorgulanabilir olması için uygulamayı yayımlayın

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Bitiş noktasından niyet ve varlık tahmini alın

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Adres çubuğundaki URL'nin sonuna gidip `Please move Carl Chamerlin from Tampa to Portland` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. Bu söyleyiş, etiketli tüm söyleyişlerle aynı değildir, bu nedenle iyi bir `MoveEmployee` testtir ve çıkarılan varlıkla niyeti geri getirmelidir.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.9706451
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.9706451
        },
        {
          "intent": "None",
          "score": 0.0307451729
        }
      ],
      "entities": [
        {
          "entity": "tampa",
          "type": "builtin.geographyV2.city",
          "startIndex": 32,
          "endIndex": 36,
          "role": "Origin"
        },
        {
          "entity": "portland",
          "type": "builtin.geographyV2.city",
          "startIndex": 41,
          "endIndex": 48,
          "role": "Destination"
        }
      ]
    }
    ```

    Doğru niyet tahmin edilir ve varlıklar dizisi, ilgili **varlıklar** özelliğindeki hem kaynak hem de hedef rollerine sahiptir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlıklar kavramları](luis-concept-entity-types.md)
* [Roller kavramları](luis-concept-roles.md)
* [Önceden oluşturulmuş varlıklar listesi](luis-reference-prebuilt-entities.md)
* [Nasıl eğitilir?](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUIS portalında test nasıl](luis-interactive-test.md)
* [Roller](luis-concept-roles.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici yeni bir amaç oluşturdu ve kaynak ve hedef konumlarının bağlamsal olarak öğrenilen verileri için örnek ifadeler ekledi. Uygulama eğitilip yayımlandıktan sonra bir istemci uygulama bu bilgileri ilgili bilgilerle bir taşınma bileti oluşturmak için kullanabilir.

> [!div class="nextstepaction"]
> [Bileşik varlık eklemeyi öğrenin](luis-tutorial-composite-entity.md)
