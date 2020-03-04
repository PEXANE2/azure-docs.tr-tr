---
title: 'Öğretici: sorunları bulmak için Batch testi-LUSıS'
description: Bu öğreticide, Language Understanding (LUSıS) uygulamanızın kalitesini doğrulamak için Batch test kullanımı gösterilmektedir.
ms.topic: tutorial
ms.date: 03/02/2020
ms.openlocfilehash: c276f0b52f83937fbe3b6fd9e0b7c1a66f665095
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78250529"
---
# <a name="tutorial-batch-test-data-sets"></a>Öğretici: Batch test veri kümeleri

Bu öğreticide, Language Understanding (LUSıS) uygulamanızın kalitesini doğrulamak için Batch test kullanımı gösterilmektedir.

Toplu test etkin doğrulamanıza olanak tanır, modelin bilinen durumuyla etiketli konuşma ve varlıkların eğitim. JSON biçimli bir toplu iş dosyası Konuşma ekleme ve gereksinim duyduğunuz varlık etiketleri içinde utterance tahmin ayarlayın.

Toplu test etmek için gereksinimler:

* En fazla test başına 1000 konuşma.
* Yinelenen değer yok.
* İzin verilen varlık türleri: yalnızca, öğrenilen varlıkları öğrendi.

Bu öğreticiden başka bir uygulama kullanırken, uygulamanıza zaten eklenmiş olan örnek utbotları *kullanmayın.*

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içeri aktar
> * Toplu test dosyası oluşturma
> * Toplu test çalıştırma
> * Test sonuçlarını gözden geçirme

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Örnek uygulamayı içeri aktar

`1 pepperoni pizza on thin crust`gibi bir pizza sırası alan bir uygulamayı içeri aktarın.

1.  [Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json?raw=true) indirip kaydedin.

1. [ÖNIZLEME Luo portalını](https://preview.luis.ai/)kullanın, JSON 'u yeni bir uygulamaya aktarın, uygulamayı `Pizza app`olarak adlandırın.

1. Uygulamayı eğitebilmeniz için, gezinmede sağ üst köşedeki **eğitme** ' yi seçin.

## <a name="what-should-the-batch-file-utterances-include"></a>Toplu iş dosyası dıksları neleri içerir?

Toplu iş dosyası, başlangıç ve bitiş konumu dahil olmak üzere, en üst düzey makine tarafından öğrenilen varlıklar ile utterslar içermelidir. Söyleyler uygulamada zaten bulunan örneklerin bir parçası olmamalıdır. Bu, amaç ve varlıklar için olumlu tahmin etmek istediğiniz söyler olmalıdır.

Testleri amaç ve/veya varlığa göre ayırabilir veya aynı dosyadaki tüm testlere (1000 ' e kadar) sahip olabilirsiniz.

## <a name="batch-file"></a>Toplu iş dosyası

JSON örneği, bir test dosyasının nasıl göründüğünü göstermek için etiketlenmiş bir varlığa sahip bir söylenişi içerir. Kendi testlerinizde, doğru amaç ve makine tarafından öğrenilen varlık etiketli birçok kullanım sahibi olmanız gerekir.

1. Bir metin düzenleyicisinde `pizza-with-machine-learned-entity-test.json` oluşturun veya [indirin](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/batch-tests/pizza-with-machine-learned-entity-test.json?raw=true) .

2. JSON biçimli toplu iş dosyasında, testte tahmin etmek istediğiniz **amacı** içeren bir söylenişi ekleyin.

   [!code-json[Add the intents to the batch test file](~/samples-cognitive-services-data-files/luis/batch-tests/pizza-with-machine-learned-entity-test.json "Add the intent to the batch test file")]

## <a name="run-the-batch"></a>Toplu işlem çalıştırın

1. Üst gezinti çubuğunda **Test** ' i seçin.

2. Sağ taraftaki panelde **Batch test paneli** ' ni seçin.

3. **Veri kümesini Içeri aktar**seçeneğini belirleyin.

    > [!div class="mx-imgBorder"]
    > Içeri aktarma veri kümesi vurgulanmış LUO uygulamasının ekran görüntüsünü ![](./media/luis-tutorial-batch-testing/import-dataset-button.png)

4. `pizza-with-machine-learned-entity-test.json` dosyanın dosya konumunu seçin.

5. Veri kümesini `pizza test` adlandırın ve **bitti**' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![dosya](./media/luis-tutorial-batch-testing/import-dataset-modal.png) seçin

6. **Çalıştır** düğmesini seçin.

7. **Sonuçları göster**' i seçin.

8. Gösterge ve grafik sonuçlarını gözden geçirin.

## <a name="review-batch-results-for-intents"></a>Amaçlar için Batch sonuçlarını gözden geçirme

Test sonuçları, test dıksları 'nın etkin sürüme göre nasıl tahmin edildiği hakkında grafiksel olarak gösterilir.

Batch grafik dört quadrants sonuçlarını görüntüler. Grafiğin sağına bir filtredir. Filtre, amaçları ve varlıkları içerir. Grafiğin bir [bölümünü](luis-concept-batch-test.md#batch-test-results) veya grafik içindeki bir noktayı seçtiğinizde, ilişkili söyleyler grafiğin altında görüntülenir.

Grafik üzerine gelindiğinde, fare tekerleğini büyütebilir veya grafikte görüntülenecek azaltın. Bu, sıkı bir şekilde birlikte kümelenmiş grafik üzerinde çok sayıda noktası olduğunda yararlıdır.

Grafik dört Çeyrek dairelerle iki kırmızı renkte gösterilir bölümlerin birlikte kullanılıyor.

1. Filtre listesinden **Modifyorder** hedefini seçin.

    > [!div class="mx-imgBorder"]
    > Filtre listesinden ModifyOrder hedefini ![seçin](./media/luis-tutorial-batch-testing/select-intent-from-filter-list.png)

    Söylenişi, toplu iş dosyasında listelenen pozitif tahmine başarıyla eşleşeceğinden, **gerçek bir pozitif** anlamı olarak tahmin edilir.

    > [!div class="mx-imgBorder"]
    > ![utterance, pozitif tahminiyle başarıyla eşleşti](./media/luis-tutorial-batch-testing/intent-predicted-true-positive.png)

    Filtreler listesindeki yeşil onay işaretleri de her bir amaç için testin başarısını gösterir. Diğer tüm amaçlar, toplu iş testinde listelenmeyen herhangi bir amaç için negatif bir test olarak her bir amaca karşı test edildiğinden, tüm diğer amaçlar 1/1 pozitif bir puana göre listelenir.

1. **Onay** hedefini seçin. Bu amaç Batch testinde listelenmez, bu nedenle Batch testinde listelenen söylenişi 'in negatif bir testi olur.

    > [!div class="mx-imgBorder"]
    > ![utterance, toplu iş dosyasındaki listelenmemiş amaç için negatif olarak tahmin edildi](./media/luis-tutorial-batch-testing/true-negative-intent.png)

    Negatif test, filtredeki yeşil metin ve kılavuz ile belirtilen şekilde başarılı oldu.

## <a name="review-batch-test-results-for-entities"></a>Varlıklar için Batch test sonuçlarını gözden geçirme

Alt varlıkların bulunduğu bir makine varlığı olarak ModifyOrder varlığı, üst düzey varlığın eşleştiğini ve alt varlıkların nasıl tahmin edilir olduğunu görüntüler.

1. Filtre listesinden **Modifyorder** varlığını seçin ve ardından kılavuzdaki daireyi seçin.

1. Varlık tahmini grafiğin altında görüntülenir. Ekranda, beklenmeler ile eşleşmeyen tahminler için beklenmeler ve noktalı çizgiler ile eşleşen tahminler için düz çizgiler bulunur.

    > [!div class="mx-imgBorder"]
    > ![varlık üst öğesi toplu iş dosyasında başarıyla tahmin edildi](./media/luis-tutorial-batch-testing/labeled-entity-prediction.png)

## <a name="finding-errors-with-a-batch-test"></a>Batch testinde hata bulma

Bu öğretici, bir testin nasıl çalıştırılacağını ve sonuçların nasıl yorumlanacağını gösterdi. Test felsesayısını veya başarısız testlerin nasıl yanıt vereceğini kapsamadı.

* Testinizde, farklı ancak ilgili bir amaç için tahmin edilebilir olan her ikisinin de olumlu ve negatif uttlerini kapsadığınızdan emin olun.
* Başarısız olan bildirimler için aşağıdaki görevleri gerçekleştirin ve ardından testleri yeniden çalıştırın:
    * Amaçlar ve varlıklar için geçerli örnekleri gözden geçirin, etkin sürümün örnek utalklarından her ikisi de amaç ve varlık etiketleme için doğru olduğunu doğrulayın.
    * Uygulamanızın amaçları ve varlıklarını tahmin etmeye yardımcı olacak özellikler ekleyin
    * Daha fazla olumlu örnek ekleme
    * Amaç genelinde örnek dengelerini gözden geçirme

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](./includes/cleanup-resources-preview-portal.md)]

## <a name="next-step"></a>Sonraki adım

Öğretici, geçerli modeli doğrulamak için bir Batch testi kullandı.

> [!div class="nextstepaction"]
> [Desenler hakkında bilgi edinin](luis-tutorial-pattern.md)

