---
title: Toplu iş testi nasıl gerçekleştirilecek - LUIS
titleSuffix: Azure Cognitive Services
description: Yanlış niyet ve varlıklara sahip sözlerle ilgili söz bulma ları bulmak için Dil Bilgisi (LUIS) toplu test kümelerini kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: bfef7eae7158a05b09a3534e8fb44335333d8cf1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73904348"
---
# <a name="batch-testing-with-a-set-of-example-utterances"></a>Örnek sözlerle toplu test

 Toplu test, LUIS'teki performansını ölçmek için mevcut eğitimli modeliniz üzerinde kapsamlı bir testtir. Toplu iş testi için kullanılan veri kümeleri, tahmin çalışma zamanı bitiş noktasından alınan niyetveya deyişlen örnek söyleyişlerini içermemelidir. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

<a name="batch-testing"></a>

## <a name="import-a-dataset-file-for-batch-testing"></a>Toplu iş testi için bir veri kümesi dosyası alma

1. Üst çubukta **Test'i** ve ardından **Toplu İşlem test panelini**seçin.

    ![Toplu İşlem Eki](./media/luis-how-to-batch-test/batch-testing-link.png)

2. **Veri kümesini Aktar'ı**seçin. Yeni veri kümesi iletişim kutusunu **içe aktarın** görüntülenir. **Dosyayı Seçin'i** seçin ve test etmek için *en fazla 1.000* sözcük içeren doğru [JSON biçimine](luis-concept-batch-test.md#batch-file-format) sahip bir JSON dosyasını bulun.

    Alma hataları tarayıcının üst kısmındaki kırmızı bildirim çubuğunda bildirilir. Bir aktarımhataları olduğunda, veri kümesi oluşturulmaz. Daha fazla bilgi için [genel hatalara](luis-concept-batch-test.md#common-errors-importing-a-batch)bakın.

3. **Dataset Adı** alanına, veri kümesi dosyanızın adını girin. Dataset *dosyası, etiketli amaç* ve *varlıklar*da dahil olmak üzere bir **dizi sözcük** içerir. Sözdizimi için [örnek toplu iş dosyasını](luis-concept-batch-test.md#batch-file-format) gözden geçirin. 

4. **Done** (Bitti) öğesini seçin. Dataset dosyası eklenir.

## <a name="run-rename-export-or-delete-dataset"></a>Veri kümesini çalıştırma, yeniden adlandırma, dışa aktarma veya silme

Veri kümesini çalıştırmak, yeniden adlandırmak, dışa aktarmak veya silmek için veri kümesi satırının sonundaki elips (***...***) düğmesini kullanın.

![Dataset Eylemleri](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Eğitimli uygulamanızda toplu iş testi çalıştırın

Testi çalıştırmak için veri kümesi adını seçin. Test tamamlandığında, bu satır veri kümesinin test sonucunu görüntüler.

![Toplu Test Sonucu](./media/luis-how-to-batch-test/run-test.png)

İndirilebilir veri kümesi, toplu iş testi için yüklenen dosyayla aynıdır.

|Durum|Anlamı|
|--|--|
|![Başarılı test yeşil daire simgesi](./media/luis-how-to-batch-test/batch-test-result-green.png)|Tüm söyleyişler başarılıdır.|
|![Başarısız test kırmızı x simgesi](./media/luis-how-to-batch-test/batch-test-result-red.png)|En az bir söyleyiş niyeti tahmin eşleşmiyor.|
|![Test simgesine hazır](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Test çalışmaya hazır.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>

## <a name="view-batch-test-results"></a>Toplu iş testi sonuçlarını görüntüleme 

Toplu iş testi sonuçlarını gözden geçirmek için **Sonuçları Gör'üni**seçin.

![Toplu test sonuçları](./media/luis-how-to-batch-test/run-test-results.png)

<a name="filter-chart-results-by-intent-or-entity"></a>  

## <a name="filter-chart-results"></a>Filtre grafiği sonuçları

Grafiği belirli bir amaç veya tevkiye göre filtrelemek için, sağ taraftaki filtreleme panelindeki amaç veya varlığı seçin. Seçiminize göre grafikteki veri noktaları ve bunların dağılımı güncellenir. 
 
![Görselleştirilmiş Toplu İşlem Test Sonucu](./media/luis-how-to-batch-test/filter-by-entity.png) 

## <a name="view-single-point-utterance-data"></a>Tek noktalı söyleyiş verilerini görüntüleme

Grafikte, tahmin kesinlik puanını görmek için bir veri noktasının üzerine gezin. Sayfanın altındaki sözcük listesinde karşılık gelen söyleyişini almak için bir veri noktası seçin. 

![Seçilen söyleyiş](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="view-section-data"></a>Bölüm verilerini görüntüleme

Dört bölümlü grafikte, grafiğin sağ üst kısmındaki **False Positive** gibi bölüm adını seçin. Grafiğin altında, bu bölümdeki tüm sözcükler grafiğin altında bir listede görüntülenir. 

![Bölüme göre seçilen söyleyinmeler](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

Bu önceki resimde, söyleyiş `switch on` TurnAllOn niyeti ile etiketlenir, ancak Yok niyet tahmini aldı. Bu, TurnAllOn niyetinin beklenen tahmini yapabilmek için daha fazla örnek söze ihtiyacı olduğunun bir göstergesidir. 

Grafiğin kırmızı ile iki bölümü, beklenen tahminle eşleşmeyan söyleyişleri gösterir. Bunlar, LUIS'in daha fazla eğitime ihtiyacı olduğunu gösteriyor. 

Grafiğin yeşil deki iki bölümü beklenen tahminle eşleşti.

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Sonraki adımlar

Test, LUIS uygulamanızın doğru amaçları ve varlıkları tanımadığını gösteriyorsa, daha fazla sözcük etiketleyerek veya özellikler ekleyerek LUIS uygulamanızın performansını artırmak için çalışabilirsiniz. 

* [Etiket LUIS ile önerilen söyleyünmeler](luis-how-to-review-endpoint-utterances.md) 
* [LUIS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md) 
* [Bu öğretici ile toplu test anlama](luis-tutorial-batch-testing.md)
* [Toplu iş testi kavramlarını öğrenin.](luis-concept-batch-test.md)
