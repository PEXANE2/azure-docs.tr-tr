---
title: Çevrimdışı değerlendirme nasıl gerçekleşilir - Personalizer
titleSuffix: Azure Cognitive Services
description: Bu makalede, uygulamanızın etkinliğini ölçmek ve öğrenme döngünüzü analiz etmek için çevrimdışı değerlendirmeyi nasıl kullanacağınızı gösterecektir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: diberry
ms.openlocfilehash: ce85c2d264b2b4849a4a36ed757150292fdf39f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77622772"
---
# <a name="analyze-your-learning-loop-with-an-offline-evaluation"></a>Çevrimdışı bir değerlendirme yle öğrenme döngünüz

Çevrimdışı bir değerlendirmeyi nasıl tamamlayacağınızı ve sonuçları nasıl anlayacağınızı öğrenin.

Çevrimdışı Değerlendirmeler, Personalizer'ın uygulamanızın varsayılan davranışıyla karşılaştırıldığında ne kadar etkili olduğunu ölçmenize, kişiselleştirmeye en çok hangi özelliklerin katkıda bulunduğunu öğrenmenize ve yeni makine öğrenimi değerlerini otomatik olarak keşfetmenize olanak sağlar.

Daha fazla bilgi edinmek için [Çevrimdışı Değerlendirmeler](concepts-offline-evaluation.md) hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

* Yapılandırılmış Personalizer döngüsü
* Personalizer döngüsü temsili bir veri miktarına sahip olmalıdır - bir ballpark olarak anlamlı değerlendirme sonuçları için günlüklerinde en az 50.000 etkinlik öneririz. İsteğe bağlı olarak, aynı değerlendirmede karşılaştırabileceğiniz ve test edebileceğiniz daha önce _öğrenme ilkesi_ dosyalarını da dışa aktarmış olabilirsiniz.

## <a name="run-an-offline-evaluation"></a>Çevrimdışı değerlendirme çalıştırma

1. Azure [portalında](https://azure.microsoft.com/free/)Kikizer kaynağınızı bulun.
1. Azure **portalında, Değerlendirmeler** bölümüne gidin ve **Değerlendirme Oluştur'u**seçin.
    ![Azure portalında **Değerlendirmeler** bölümüne gidin ve **Değerlendirme Oluştur**'yi seçin.](./media/offline-evaluation/create-new-offline-evaluation.png)
1. Aşağıdaki değerleri yapılandırın:

    * Bir değerlendirme adı.
    * Başlangıç ve bitiş tarihi - bunlar değerlendirmede kullanılacak veri aralığını belirten tarihlerdir. Bu veriler, [Veri Saklama](how-to-settings.md) değerinde belirtildiği gibi günlüklerde bulunmalıdır.
    * Optimizasyon Bulma **evet**olarak ayarlanır.

    > [!div class="mx-imgBorder"]
    > ![Çevrimdışı değerlendirme ayarlarını seçin](./media/offline-evaluation/create-an-evaluation-form.png)

1. **Tamam'ı**seçerek Değerlendirmeyi başlatın.

## <a name="review-the-evaluation-results"></a>Değerlendirme sonuçlarını gözden geçirme

İşleme verilebilen veri miktarına, karşılaştırılacak öğrenme ilkelerinin sayısına ve bir optimizasyon istenip istenmediğine bağlı olarak değerlendirmelerin çalıştırılması uzun zaman alabilir.

Tamamlandıktan sonra, değerlendirme listesinden değerlendirmeyi seçebilir, ardından **uygulamanızın puanını diğer olası öğrenme ayarlarıyla karşılaştır'ı**seçebilirsiniz. Geçerli öğrenme politikanızın yeni bir ilkeyle karşılaştırıldığında nasıl performans gösterdiğini görmek istediğinizde bu özelliği seçin.

1. [Öğrenme politikalarının](concepts-offline-evaluation.md#discovering-the-optimized-learning-policy)performansını gözden geçirin.

    > [!div class="mx-imgBorder"]
    > [![Değerlendirme sonuçlarını gözden geçirme](./media/offline-evaluation/evaluation-results.png)](./media/offline-evaluation/evaluation-results.png#lightbox)

1. Verileriniz için modeli en iyi şekilde geliştiren ilkeyi uygulamak için **Uygula'yı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Çevrimdışı değerlendirmelerin nasıl çalıştığı](concepts-offline-evaluation.md)hakkında daha fazla bilgi edinin.
