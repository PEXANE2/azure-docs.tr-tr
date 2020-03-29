---
title: Smart Labeler ile görüntüleri daha hızlı etiketleme
titleSuffix: Azure Cognitive Services
description: Bu kılavuzda, görüntüler için önerilen etiketleri oluşturmak için Akıllı Etiketleyici'yi nasıl kullanacağınızı öğreneceksiniz. Bu, özel bir vizyon modelini eğitirken çok sayıda görüntüyü daha hızlı etiketlemenize olanak tanır.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: pafarley
ms.openlocfilehash: 94ca47e6114e4f8c3485f6072facd07c25e4b96a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73647762"
---
# <a name="label-images-faster-with-smart-labeler"></a>Smart Labeler ile görüntüleri daha hızlı etiketleme

Bu kılavuzda, görüntüler için önerilen etiketleri oluşturmak için Akıllı Etiketleyici'yi nasıl kullanacağınızı öğreneceksiniz. Bu, özel bir vizyon modelini eğitirken çok sayıda görüntüyü daha hızlı etiketlemenize olanak tanır.

Özel Görme modeli için görüntüleri etiketlediğinizde, hizmet etiketlenmemiş görüntülerin etiketlerini tahmin etmek için modelin en son eğitilmiş yinelemesini kullanır. Daha sonra seçilen güven eşiği ve tahmin belirsizliğine bağlı olarak bu öngörüleri önerilen etiketler olarak gösterir. Daha sonra, eğitim için görüntüleri el ile etiketleme işlemini hızlandırarak önerileri onaylayabilir veya değiştirebilirsiniz.

## <a name="when-to-use-smart-labeler"></a>Akıllı Etiketleyici ne zaman kullanılır?

Aşağıdaki sınırlamaları aklınızda bulundurun:

* Yalnızca içeriği zaten bir kez eğitilmiş görüntüler için önerilen etiketleri istemeniz gerekir. Yeni bir etiket için yeni bir etiket için yeni bir eğitim ekime başladığınız önerilerini almayın.

> [!IMPORTANT]
> Smart Labeler özelliği, normal tahminlerle aynı [fiyatlandırma modelini](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) kullanır. Bir görüntü kümesi için önerilen etiketleri ilk kez tetiklediğinizde, tahmin çağrılarıyla aynı şekilde ücretlendirilirsiniz. Bundan sonra, hizmet seçilen görüntülerin sonuçlarını 30 gün boyunca bir veritabanında saklar ve bu süre içinde bunlara istediğiniz zaman ücretsiz olarak erişebilirsiniz. 30 gün sonra, önerilen etiketlerini tekrar isterseniz ücretlendirilirsiniz.

## <a name="smart-labeler-workflow"></a>Akıllı Etiketleyici iş akışı

Aşağıdaki adımlar, Smart Labeler'ı nasıl kullanacağınızı gösterir:

1. Tüm eğitim resimlerinizi Custom Vision projenize yükleyin.
1. Her etiket için eşit sayıda resim seçerek veri setinizin bir kısmını etiketle.
    > [!TIP]
    > Daha sonra öneri almak istediğiniz tüm etiketleri kullandığınızdan emin olun.
1. Eğitim sürecini başlatın.
1. Eğitim tamamlandığında, **Etiketlenmemiş** görünüme gidin ve sol bölmedeki **önerilen etiketleri al** düğmesini seçin.
    > [!div class="mx-imgBorder"]
    > ![Önerilen etiketler düğmesi etiketlenmemiş resimler sekmesinin altında gösterilir.](./media/suggested-tags/suggested-tags-button.png)
1. Görünen açılır pencerede, öneri istediğiniz görüntü sayısını ayarlayın. Etiketlenmemiş görüntülerin yalnızca bir kısmı için ilk etiket önerileri almalısınız. Bu işlem boyunca yinelerken daha iyi etiket önerileri alırsınız.
1. Önerilen etiketleri doğrulayan etiketleri doğrulayın.
    > [!TIP]
    > Önerilen etiketlere sahip görüntüler tahmin belirsizliğlerine göre sıralanır (daha düşük değerler daha yüksek güveni gösterir). Sıralama sırasını **belirsizliğe göre sırala** seçeneğiyle değiştirebilirsiniz. Siparişi **yüksekten düşüklere**ayarlarsanız, önce yüksek belirsizlik tahminlerini düzeltebilir ve daha sonra düşük belirsizlik tahminlerini hızlı bir şekilde doğrulayabilirsiniz.
    * Resim sınıflandırma projelerinde, etiketleri toplu olarak seçebilir ve onaylayabilirsiniz. Görünümü önerilen etikete göre filtreleyin, yanlış etiketlenen görüntüleri seçin ve geri kalanını toplu olarak onaylayın.
        > [!div class="mx-imgBorder"]
        > ![Önerilen etiketler, filtrelerle IC için toplu iş modunda görüntülenir.](./media/suggested-tags/ic-batch-mode.png)

        Galeriden bir resim seçerek önerilen etiketleri tek tek görüntü modunda da kullanabilirsiniz.

        ![Önerilen etiketler IC için ayrı görüntü modunda görüntülenir.](./media/suggested-tags/ic-individual-image-mode.png)
    * Nesne algılama projelerinde toplu iş onayları desteklenmez, ancak daha düzenli bir etiketleme deneyimi için önerilen etiketlere göre filtreleyebilir ve sıralayabilirsiniz. Etiketlenmemiş resimlerinizin küçük resimleri, önerilen etiketlerin konumlarını gösteren sınırlayıcı kutuların bir kaplamasını gösterir. Önerilen etiket filtresini seçmezseniz, etiketlenmemiş tüm resimleriniz sınırlayıcı kutuları aşmadan görünür.
        > [!div class="mx-imgBorder"]
        > ![Önerilen etiketler, filtrelerle OD için toplu iş modunda görüntülenir.](./media/suggested-tags/od-batch-mode.png)

        Nesne algılama etiketlerini onaylamak için bunları galerideki her bir resme uygulamanız gerekir.

        ![Önerilen etiketler OD için ayrı görüntü modunda görüntülenir.](./media/suggested-tags/od-individual-image-mode.png)
1. Eğitim sürecini yeniden başlatın.
1. Öneri kalitesinden memnun olana kadar önceki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

Özel Vizyon projesi oluşturmaya ve eğitime başlamak için hızlı bir başlangıç izleyin.

* [Sınıflandırıcı oluşturma](getting-started-build-a-classifier.md)
* [Nesne algılayıcısı oluşturma](get-started-build-detector.md)