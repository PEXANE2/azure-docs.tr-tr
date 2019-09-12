---
title: Çevrimdışı değerlendirme-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Çevrimdışı değerlendirmede öğrenme döngünüzü çözümlemeyi öğrenin
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: diberry
ms.openlocfilehash: a157b36ad2b4f5a7f2e50a9609d8acd621efd5a8
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70884450"
---
# <a name="analyze-your-learning-loop-with-an-offline-evaluation"></a>Çevrimdışı değerlendirmede öğrenme döngünüzü çözümleyin

Çevrimdışı değerlendirmeyi tamamlamayı ve sonuçları anlamayı öğrenin.

Çevrimdışı değerlendirmeler, etkin kişiselleştirmenin uygulamanızın varsayılan davranışıyla nasıl karşılaştırıldığını ölçmenize olanak tanır, kişiselleştirmeye en çok katkıda bulunan özellikleri öğrenin ve yeni makine öğrenimi ayarlarını otomatik olarak bulabilir.

Daha fazla bilgi edinmek için [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md) hakkında bölümünü okuyun.


## <a name="prerequisites"></a>Önkoşullar

* Yapılandırılmış bir kişiselleştirici döngüsü
* Kişiselleştirme döngüsünün temsili bir veri miktarına sahip olması gerekir. bunun için, en az 50.000 olay üzerinde anlamlı değerlendirme sonuçları elde etmenizi öneririz. İsteğe bağlı olarak, aynı değerlendirmede karşılaştırmak ve test yapmak için daha önce de daha önce _öğrendiğiniz öğrenme ilkesi_ dosyalarını vermiş olabilirsiniz.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Yeni bir çevrimdışı değerlendirme başlatma adımları

1. [Azure Portal](https://azure.microsoft.com/free/), kişiselleştirme kaynağınızı bulun.
1. Azure portal **değerlendirmeler** bölümüne gidin ve **değerlendirme oluştur**' u seçin.
    ![Azure portal, * * Değerlendirmeler * * bölümüne gidin ve * * değerlendirme oluştur * * seçeneğini belirleyin.](./media/offline-evaluation/create-new-offline-evaluation.png)
1. Aşağıdaki ayarları seçin:

    * Değerlendirme adı
    * Başlangıç ve bitiş tarihi-bunlar geçmişte, değerlendirmede kullanılacak veri aralığını belirten tarihlerdir. [Veri saklama](how-to-settings.md) ayarında belirtilen şekilde bu verilerin günlüklerde bulunması gerekir.
    * İyileştirme bulma, **Evet** olarak ayarlandı

    ![Çevrimdışı değerlendirme ayarlarını seçin](./media/offline-evaluation/create-an-evaluation-form.png)

1. **Tamam ' ı**seçerek değerlendirmeyi başlatın. 

## <a name="results"></a>Sonuçlar

İşlenecek veri miktarına, Karşılaştırılacak öğrenme ilkesi sayısına ve bir iyileştirmenin istenip istenmediğine bağlı olarak, değerlendirmelere çalıştırılması uzun zaman alabilir.

Tamamlandıktan sonra değerlendirme listesinden değerlendirmeyi seçebilirsiniz. 

Öğrenme Ilkelerinin karşılaştırmaları şunları içerir:

* **Çevrimiçi ilke**: Kişiselleştirici içinde kullanılan geçerli öğrenme Ilkesi
* **Taban çizgisi**: Uygulamanın varsayılan (derece çağrılarında gönderilen ilk eylem tarafından belirlendiği şekilde),
* **Rastgele ilke**: Her zaman sağlanan Işlemlerden rastgele eylem seçimi döndüren sanal bir sıra davranışı.
* **Özel ilkeler**: Değerlendirme başlatılırken ek öğrenme Ilkeleri karşıya yüklendi.
* **Iyileştirilmiş ilke**: Değerlendirme, iyileştirilmiş bir ilkeyi bulma seçeneği ile başlatıldıysa, bu da karşılaştırılır ve bunu indirebilir veya çevrimiçi öğrenme ilkesi haline getirmek için geçerli olanı değiştirin.

![Çevrimdışı değerlendirme ayarlarının sonuç grafiği](./media/offline-evaluation/evaluation-results.png)

Eylemler ve bağlam [özelliklerinin](concepts-features.md) verimliliği.

## <a name="next-steps"></a>Sonraki adımlar

* [Çevrimdışı değerlendirmelerinin nasıl çalıştığını](concepts-offline-evaluation.md)öğrenin.
