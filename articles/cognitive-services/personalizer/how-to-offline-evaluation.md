---
title: Çevrimdışı değerlendirme gerçekleştirme-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Bu makalede, uygulamanızın verimliliğini ölçmek ve öğrenme döngünüzü çözümlemek için çevrimdışı değerlendirmeyi nasıl kullanacağınız gösterilmektedir.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: how-to
ms.date: 02/20/2020
ms.openlocfilehash: a473085f9c94ca42a75d01b342d60cc33836b096
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88244849"
---
# <a name="analyze-your-learning-loop-with-an-offline-evaluation"></a>Çevrimdışı değerlendirmede öğrenme döngünüzü çözümleyin

Çevrimdışı değerlendirmeyi tamamlamayı ve sonuçları anlamayı öğrenin.

Çevrimdışı değerlendirmeler, etkin kişiselleştirmenin uygulamanızın varsayılan davranışıyla nasıl karşılaştırıldığını ölçmenize olanak tanır, kişiselleştirmeye en çok katkıda bulunan özellikleri öğrenin ve yeni makine öğrenimi değerlerini otomatik olarak bulabilir.

Daha fazla bilgi edinmek için [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md) hakkında bölümünü okuyun.

## <a name="prerequisites"></a>Önkoşullar

* Yapılandırılmış bir kişiselleştirici döngüsü
* Kişiselleştirme döngüsünün temsili bir veri miktarına sahip olması gerekir. bunun için, en az 50.000 olay üzerinde anlamlı değerlendirme sonuçları elde etmenizi öneririz. İsteğe bağlı olarak, aynı değerlendirmede karşılaştırmak ve test yapmak için daha önce de daha önce _öğrendiğiniz öğrenme ilkesi_ dosyalarını vermiş olabilirsiniz.

## <a name="run-an-offline-evaluation"></a>Çevrimdışı değerlendirme çalıştırma

1. [Azure Portal](https://azure.microsoft.com/free/cognitive-services), kişiselleştirici kaynağını bulun.
1. Azure portal **değerlendirmeler** bölümüne gidin ve **değerlendirme oluştur**' u seçin.
    ![Azure portal, * * Değerlendirmeler * * bölümüne gidin ve * * değerlendirme oluştur * * seçeneğini belirleyin.](./media/offline-evaluation/create-new-offline-evaluation.png)
1. Aşağıdaki değerleri yapılandırın:

    * Değerlendirme adı.
    * Başlangıç ve bitiş tarihi-Bunlar, değerlendirmede kullanılacak veri aralığını belirten tarihlerdir. [Veri saklama](how-to-settings.md) değerinde belirtildiği gibi bu verilerin günlüklerde bulunması gerekir.
    * İyileştirme bulma, **Evet**olarak ayarlandı.

    > [!div class="mx-imgBorder"]
    > ![Çevrimdışı değerlendirme ayarlarını seçin](./media/offline-evaluation/create-an-evaluation-form.png)

1. **Tamam ' ı**seçerek değerlendirmeyi başlatın.

## <a name="review-the-evaluation-results"></a>Değerlendirme sonuçlarını gözden geçirme

İşlenecek veri miktarına, Karşılaştırılacak öğrenme ilkesi sayısına ve bir iyileştirmenin istenip istenmediğine bağlı olarak, değerlendirmelere çalıştırılması uzun zaman alabilir.

Tamamlandıktan sonra, değerlendirme listesinden değerlendirmeyi seçebilir ve daha sonra **uygulamanızın Puanını diğer olası öğrenme ayarlarıyla Karşılaştır**' ı seçebilirsiniz. Geçerli öğrenme ilkenizin yeni bir ilkeyle karşılaştırıldığında nasıl çalıştığını görmek istediğinizde bu özelliği seçin.

1. [Öğrenme ilkelerinin](concepts-offline-evaluation.md#discovering-the-optimized-learning-policy)performansını gözden geçirin.

    > [!div class="mx-imgBorder"]
    > [![Değerlendirme sonuçlarını gözden geçirme](./media/offline-evaluation/evaluation-results.png)](./media/offline-evaluation/evaluation-results.png#lightbox)

1. Verilerinizin en iyi modelini artıran ilkeyi uygulamak için **Uygula** ' yı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Çevrimdışı değerlendirmelerinin nasıl çalıştığı](concepts-offline-evaluation.md)hakkında daha fazla bilgi edinin.
