---
title: 'Öğretici: Sorunları bulmak için toplu test - LUIS'
description: Bu öğretici, Dil Anlama (LUIS) uygulamanızın kalitesini doğrulamak için toplu testin nasıl kullanılacağını gösterir.
ms.topic: tutorial
ms.date: 03/02/2020
ms.openlocfilehash: c276f0b52f83937fbe3b6fd9e0b7c1a66f665095
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78250529"
---
# <a name="tutorial-batch-test-data-sets"></a>Öğretici: Toplu test veri setleri

Bu öğretici, Dil Anlama (LUIS) uygulamanızın kalitesini doğrulamak için toplu testin nasıl kullanılacağını gösterir.

Toplu işlem, etkin, eğitilmiş modelin durumunu bilinen bir etiketlenmiş sözcük ve varlıklar kümesiyle doğrulamanıza olanak tanır. JSON biçimli toplu iş dosyasında, sözcük leri ekleyin ve söyleyiş içinde tahmin edilmesi gereken varlık etiketlerini ayarlayın.

Toplu test için gereksinimler:

* Test başına en fazla 1000 kelime.
* Kopya yok.
* İzin verilen varlık türleri: yalnızca işlenmiş öğrenilen varlıklar.

Bu öğretici dışında bir uygulama *not* kullanırken, uygulamanız için zaten eklenen örnek ifadelerini kullanmayın.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içe aktarma
> * Toplu iş testi dosyası oluşturma
> * Toplu iş testi çalıştırma
> * Test sonuçlarını gözden geçirme

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Örnek uygulamayı içe aktarma

Pizza siparişi alan bir uygulamayı `1 pepperoni pizza on thin crust`içe aktarın.

1.  [Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json?raw=true) indirip kaydedin.

1. Önizleme [LUIS portalını](https://preview.luis.ai/)kullanın, JSON'u yeni bir `Pizza app`uygulamaya aktarın, uygulamayı adlandırın.

1. Uygulamayı eğitmek için navigasyonun sağ üst köşesindeki **Tren'i** seçin.

## <a name="what-should-the-batch-file-utterances-include"></a>Toplu dosya tasnifleri neleri içermelidir?

Toplu iş dosyası, başlangıç ve bitiş konumu da dahil olmak üzere etiketlenmiş üst düzey makine öğrenilen varlıklara sahip tümlemeleri içermelidir. Bu söyleyişler, uygulamada zaten bulunan örneklerin bir parçası olmamalıdır. Bunlar, niyet ve varlıklar için olumlu tahmin etmek istediğiniz söyleyişler olmalıdır.

Testleri niyet ve/veya tüzel kişiye göre ayırabilir veya tüm testleri (1000'e kadar söyleyerek) aynı dosyada bulabilirsiniz.

## <a name="batch-file"></a>Toplu iş dosyası

JSON örneği, test dosyasının nasıl göründüğünü göstermek için etiketli bir varlıkla bir sözcük içerir. Kendi testlerinizde, doğru niyete sahip birçok söyleyişiniz ve makineden öğrenilen varlık etiketlenmiş olmalıdır.

1. Metin `pizza-with-machine-learned-entity-test.json` düzenleyicisinde oluşturun veya [indirin.](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/batch-tests/pizza-with-machine-learned-entity-test.json?raw=true)

2. JSON biçimlendirilmiş toplu iş dosyasında, testte tahmin edilen **Niyet** ile bir söyleyiş ekleyin.

   [!code-json[Add the intents to the batch test file](~/samples-cognitive-services-data-files/luis/batch-tests/pizza-with-machine-learned-entity-test.json "Add the intent to the batch test file")]

## <a name="run-the-batch"></a>Toplu işlemi çalıştırma

1. Üst gezinti çubuğunda **Test'i** seçin.

2. Sağ taraftaki **panelde Toplu İşlem test panelini** seçin.

3. **Veri kümesini Aktar'ı**seçin.

    > [!div class="mx-imgBorder"]
    > ![Alma veri kümesi vurgulanan LUIS uygulamasının ekran görüntüsü](./media/luis-tutorial-batch-testing/import-dataset-button.png)

4. `pizza-with-machine-learned-entity-test.json` Dosyanın dosya konumunu seçin.

5. Veri kümesini `pizza test` adlandırın ve **Bitti'yi**seçin.

    > [!div class="mx-imgBorder"]
    > ![Dosyayı seçin](./media/luis-tutorial-batch-testing/import-dataset-modal.png)

6. **Çalıştır** düğmesini seçin.

7. **Sonuçları Gör'i**seçin.

8. Grafik ve gösterge deki sonuçları gözden geçirin.

## <a name="review-batch-results-for-intents"></a>Niyetler için toplu iş sonuçlarını gözden geçirme

Test sonuçları, test intibakının etkin sürüme karşı nasıl tahmin edildiğini grafikolarak gösterir.

Toplu iş grafiği dört dizi sonuç görüntüler. Grafiğin sağında bir filtre var. Filtre niyet ve varlıklar içerir. [Grafiğin](luis-concept-batch-test.md#batch-test-results) bir bölümünü veya grafik içindeki bir noktayı seçtiğinizde, grafiğin altındaki ilişkili sözcük(ler) görüntülenir.

Grafiğin üzerinde gezinirken, fare tekerleği grafikteki ekranı büyütebilir veya azaltabilir. Bu, grafikte sıkıca kümelenmiş birçok nokta olduğunda yararlıdır.

Grafik dört çeyreğinde, iki bölüm kırmızı görüntülenir.

1. Filtre listesinde **ki ModifyOrder** amacını seçin.

    > [!div class="mx-imgBorder"]
    > ![Filtre listesinden DeğiştirAmaçlı amacını seçin](./media/luis-tutorial-batch-testing/select-intent-from-filter-list.png)

    Söyleyiş, **gerçek pozitif** olarak tahmin edilir, bu da sözün toplu iş dosyasında listelenen olumlu tahminini başarıyla eşleştirmesi anlamına gelir.

    > [!div class="mx-imgBorder"]
    > ![Utterance başarıyla olumlu tahmin eşleşti](./media/luis-tutorial-batch-testing/intent-predicted-true-positive.png)

    Filtreler listesindeki yeşil onay işaretleri, her amaç için testin başarısını da gösterir. Tüm diğer niyetler 1/1 pozitif puanla listelenir, çünkü söz, toplu iş testinde listelenmemiş herhangi bir niyet için negatif bir test olarak, her bir niyete karşı test edilmiştir.

1. **Onay** niyetini seçin. Bu amaç toplu iş testinde listelenmez, bu nedenle bu toplu iş testinde listelenen söyleyiş negatif bir testtir.

    > [!div class="mx-imgBorder"]
    > ![Toplu iş dosyasında listelenmemiş amaç için negatif olarak başarıyla tahmin edilen](./media/luis-tutorial-batch-testing/true-negative-intent.png)

    Negatif test, filtredeki yeşil metin ve ızgarada belirtildiği gibi başarılı oldu.

## <a name="review-batch-test-results-for-entities"></a>Varlıklar için toplu test sonuçlarını gözden geçirme

ModifyOrder varlığı, subentities olan bir makine varlığı olarak, üst düzey varlık eşleşen ve subentities nasıl tahmin görüntüler görüntüler.

1. Filtre listesindeki **ModifyOrder** varlığını seçin ve ardından ızgaradaki daireyi seçin.

1. Varlık tahmini grafiğin altında görüntülenir. Ekran, beklentiyle eşleşen öngörüler için düz çizgiler ve beklentiyle eşleşmeyen öngörüler için noktalı çizgiler içerir.

    > [!div class="mx-imgBorder"]
    > ![Toplu iş dosyasında başarıyla tahmin edilen varlık üst öğesi](./media/luis-tutorial-batch-testing/labeled-entity-prediction.png)

## <a name="finding-errors-with-a-batch-test"></a>Toplu iş testiile hataları bulma

Bu öğretici nasıl bir test çalıştırmak ve sonuçları yorumlamak gösterdi. Test felsefesini ya da başarısız testlere nasıl yanıt verilebildiğini kapsamıyor.

* Farklı ama ilgili bir amaç için tahmin edilebilecek söyleyişler de dahil olmak üzere, testinizde hem pozitif hem de olumsuz sözlerinizi kapsadığınızdan emin olun.
* Başarısız ifadeler için aşağıdaki görevleri gerçekleştirin ve testleri yeniden çalıştırın:
    * Niyetler ve varlıklar için geçerli örnekleri gözden geçirin, etkin sürümün örnek söyleyişlerini doğrulayın hem niyet hem de varlık etiketleme için doğrudur.
    * Uygulamanızın amaçları ve varlıkları tahmin etmelerine yardımcı olacak özellikler ekleme
    * Daha olumlu örnek söyleyinekler ekleyin
    * Örneğin dilermedengesini,

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](./includes/cleanup-resources-preview-portal.md)]

## <a name="next-step"></a>Sonraki adım

Öğretici, geçerli modeli doğrulamak için bir toplu iş testi kullandı.

> [!div class="nextstepaction"]
> [Desenler hakkında bilgi edinin](luis-tutorial-pattern.md)

