---
title: 'Öğretici: rollerle bağlama verisi-LUSıS'
description: Bağlama göre ilgili verileri bulun. Örneğin, bir bina ya da ofisten başka bir bina ya da ofise fiziksel olarak taşınmada çıkış ve varış konumları.
ms.topic: tutorial
ms.date: 03/30/2020
ms.openlocfilehash: fdb463896e531619ea7ebe7c384729763dc84138
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80475832"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Öğretici: bağlamsal olarak ilgili verileri bir söylenişi 'ten ayıklayın

Bu öğreticide bağlama göre ilgili veri parçalarını bulacaksınız. Örneğin, bir şehirden diğerine aktarım için kaynak ve hedef konumlar. Her iki veri parçası de gerekli olabilir ve birbirleriyle ilişkilidir.

Bir rol, önceden oluşturulmuş veya özel varlık türü ile kullanılabilir ve her iki örnek de ve desenlerinde kullanılabilir.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Yeni uygulama oluşturma
> * Amaç ekleme
> * Rolleri kullanarak kaynak ve hedef bilgilerini al
> * Eğitim
> * Yayımlama
> * Uç noktadan amaçları ve varlık rollerini al

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>İlgili veriler

Bu uygulama, bir çalışanın kaynak şehirden hedef şehre nereye taşınacağını belirler. Şehir adlarını belirlemek için GeographyV2 önceden oluşturulmuş bir varlık kullanır ve konum türlerini (Origin ve Destination) söylik içinde belirlemek için rolleri kullanır.

Ayıklanacak varlık verileri için bir rol kullanılmalıdır:

* , Utterance bağlamında birbirleriyle ilişkilidir.
* Her bir rolü göstermek için belirli bir sözcük seçimi kullanır. Bu sözcüklere örnekler şunlardır: from/to, leaving/headed to, away from/toward (çıkış/varış, ayrılıyor/gidiyor, kaynaktan/hedefe doğru)
* Her iki rol de genellikle aynı şekilde, LUSıS 'in bu sık kullanılan bağlamsal kullanımdan öğrenmesine izin verir.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

1. [Lusıs **Önizleme** portalında](https://preview.luis.ai)oturum açın.

1. **Konuşma için + yeni uygulama**' yı seçin, adı `HumanResources` girin ve varsayılan kültür olan **İngilizce**'yi koruyun. Açıklama ve tahmin kaynağını boş bırakın. **Done** (Bitti) öğesini seçin.

## <a name="create-an-intent-to-move-employees-between-cities"></a>Çalışanları şehir arasında taşımak için bir amaç oluşturun

Amaç, kullanıcının doğal dil metinden belirlenen amaç temelinde sınıflandırılır.

Bir utterance sınıflandırmak için, amaç bu amaca göre sınıflandırılması gereken Kullanıcı arasları örneklerine ihtiyaç duyuyor.

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. **+ Oluştur**seçeneğini belirleyin.

1. Açılan iletişim kutusuna `MoveEmployeeToCity` girip **Done** (Bitti) öğesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Create new intent (Yeni amaç oluştur) iletişim kutusunun ekran görüntüsü](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Bir kullanıcının sormasını beklediğinizi bu amaca birkaç örnek ekleyin.

    |Örnek konuşmalar|
    |--|
    |John W. Smith 'i Seattle uçlu, Orlando 'ya taşı|
    |Jill Jones Seattle 'dan Cairo 'ya aktar|
    |John Jackson yerine, Atlanta 'e yaklaşan |
    |KAG Chicago 'ten Tulsa 'ya taşı|
    |MV Jill Jones, Cairo uçlu|
    |Ali Anderson 'dan Oakland 'e kaydır|
    |San Francisco 'dan Redmond 'a Carl Chamerlin|
    |San Diego 'dan Bellevue 'e doğru Steve Stançe aktarma |
    |SSAS şehrinde Tanner Thompson 'yı kaldırın ve Chicago 'ye SHIFT 'e gidin|

    > [!div class="mx-imgBorder"]
    > ![MoveEmployee amacındaki yeni konuşmaları gösteren LUIS uygulaması ekran görüntüsü](./media/tutorial-entity-roles/hr-enter-utterances.png)

## <a name="add-prebuilt-entity-geographyv2"></a>Önceden oluşturulmuş varlık geographyV2 Ekle

Önceden oluşturulmuş varlık olan **geographyV2**, şehir adları dahil konum bilgilerini ayıklar. Söyleyenlerde iki şehir adı bulunduğundan, bağlamdaki birbirleriyle ilgili olarak bu bağlamı ayıklamak için roller kullanın.

1. Sol taraftaki gezinmede **varlıklar** ' ı seçin.

1. **+ Önceden oluşturulmuş varlık Ekle**' yi seçin ve `geo` ardından önceden oluşturulmuş varlıkları filtrelemek için arama çubuğuna girin.

    > [!div class="mx-imgBorder"]
    > ![Uygulamaya geographyV2 önceden oluşturulmuş varlık ekleme](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)

1. Onay kutusunu seçin ve **bitti**' yi seçin.

## <a name="add-roles-to-prebuilt-entity"></a>Önceden oluşturulmuş varlığa roller ekleme

1. **Varlıklar** listesinde, yeni varlığı açmak için **geographyV2** seçin.
1. Rol eklemek için aşağıdaki iki rolü **+** seçin ve ekleyin: `Origin`, ve. `Destination`

    > [!div class="mx-imgBorder"]
    > ![Önceden oluşturulmuş varlığa roller ekleme](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)

## <a name="label-entity-roles-in-example-utterances"></a>Örnek Meklerde varlık rollerini etiketleme

1. Sol taraftaki gezinmede **amaçları** seçin, sonra **Moveemployeetocity** hedefini seçin. Şehir adlarının **geographyV2**önceden oluşturulmuş varlıkla etiketlendirildiğine dikkat edin.
1. Bağlam araç çubuğunda _kalem simgesini_içeren **varlık paletini** seçin.

    > [!div class="mx-imgBorder"]
    > ![İçerik araç çubuğundan varlık paleti seçin](media/tutorial-entity-roles/intent-detail-context-toolbar-select-entity-palette.png)

1. Önceden oluşturulmuş varlığı seçin, **geographyV2**ve ardından **varlık denetçisi**' ni seçin.
1. **Varlık denetçisinde**bir rol, **hedef**' i seçin. Bu, fare imlecini değiştirir. Hedef konum olan tüm söyleylerdeki metni etiketlemek için imleci kullanın.

    > [!div class="mx-imgBorder"]
    > ![Varlık paletinde rol seçin](media/tutorial-entity-roles/entity-palette-select-entity-role.png)


1. **Varlık denetçisi**' ne dönün, role **kaynak**olarak geçin. Kaynak konumu olan tüm söyleylerdeki metni etiketlemek için imleci kullanın.

## <a name="add-example-utterances-to-the-none-intent"></a>Hiçbiri amacına örnek ekleme

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulamaya yapılan değişikliklerin test edilebilir olması için uygulamayı eğitme

Uygulamayı eğiteiçin **eğitme**' yi seçin. Eğitim, etkin modele yeni varlıklar ve etiketli utterler gibi değişiklikleri uygular.

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Uygulamayı HTTP uç noktasından erişmek üzere yayımlayın

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]


## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktadan amacı ve varlık tahminini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]


1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ ile `Please move Carl Chamerlin from Tampa to Portland`değiştirin.

Bu söylenişi, etiketlendirmelerdeki herhangi biriyle aynı değildir ve bu sayede iyi bir test olur ve ayıklanan varlıkla ilgili `MoveEmployee` amacı döndürmelidir.

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

    The correct intent is predicted and the entities array has both the origin and destination roles in the corresponding **entities** property.

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık kavramları](luis-concept-entity-types.md)
* [Rol kavramları](luis-concept-roles.md)
* [Önceden oluşturulmuş varlıklar listesi](luis-reference-prebuilt-entities.md)
* [Eğitme](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)
* [Roller](luis-concept-roles.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, kaynak ve hedef konumların bağlamsal olarak öğrenilen verileri için yeni bir amaç ve ek bir örnek açıklaması oluşturdu. Uygulama eğitilip yayımlandıktan sonra bir istemci uygulama bu bilgileri ilgili bilgilerle bir taşınma bileti oluşturmak için kullanabilir.

> [!div class="nextstepaction"]
> [Bileşik varlık eklemeyi öğrenin](luis-tutorial-composite-entity.md)
