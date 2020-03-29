---
title: Visual Studio için Azure Stream Analytics ile canlı verileri test edin
description: Azure Akış Analizi işinizi canlı akış verilerini kullanarak yerel olarak nasıl test edebilirsiniz öğrenin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: f2876ea32bdcd900a454ae6b7ac58c11b8ec67c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840494"
---
# <a name="test-live-data-locally-using-azure-stream-analytics-tools-for-visual-studio-preview"></a>Visual Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak test edin (Önizleme)

Visual Studio için Azure Akış Analizi araçları, Azure Event Hub, IoT Hub ve Blob Depolama'dan canlı etkinlik akışlarını kullanarak IDE'deki işleri yerel olarak test etmenizi sağlar. Canlı veri yerel testi bulutta gerçekleştirebileceğiniz [performans ve ölçeklenebilirlik testinin](stream-analytics-streaming-unit-consumption.md) yerini alamaz, ancak Akış Analizi işinizi her test etmek istediğinizde buluta göndermek zorunda kalmadan işlevsel test sırasında zamandan tasarruf edebilirsiniz. Bu özellik önizlemededir ve üretim iş yükleri için kullanılmamalıdır.

## <a name="testing-options"></a>Test seçenekleri

Aşağıdaki yerel test seçenekleri desteklenir:

|**Giriş**  |**Çıktı**  |**İş Türü**  |
|---------|---------|---------|
|Yerel statik veriler   |  Yerel statik veriler   |   Bulut/Kenar |
|Canlı giriş verileri   |  Yerel statik veriler   |   Bulut |
|Canlı giriş verileri   |  Canlı çıktı verileri   |   Bulut |

## <a name="local-testing-with-live-data"></a>Canlı verilerle yerel test

1. [Visual Studio'da](stream-analytics-quick-create-vs.md)bir Azure Akışı Analytics bulut projesi oluşturduktan sonra komut **dosyalarını aç.asaql**. Yerel sınama varsayılan olarak yerel giriş ve yerel çıktı kullanır.

   ![Azure Stream Analytics Visual Studio yerel girişi ve yerel çıktı](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-local-input-output.png)

2. Canlı verileri sınamak için açılan kutudan **Bulut Girişi'ni kullan'ı** seçin.

   ![Azure Stream Analytics Visual Studio canlı bulut girişi](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input.png)

3. **İşin** giriş verilerini işlemeye ne zaman başlayacağını tanımlamak için Başlangıç Saatini ayarlayın. İşin doğru sonuçları sağlamak için giriş verilerini önceden okuması gerekebilir. Varsayılan süre, geçerli saatin 30 dakika ilerisinde ayarlanır.

   ![Azure Stream Analytics Visual Studio canlı veri başlangıç saati](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-start-time.png)

4. **Yerel Olarak Çalıştır'ı**tıklatın. Çalışan ilerleme ve iş ölçümleri ile bir konsol penceresi görüntülenir. İşlemi durdurmak istiyorsanız, bunu el ile yapabilirsiniz. 

   ![Azure Stream Analytics Visual Studio canlı veri işlem penceresi](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-process-window.png)

   Çıktı sonuçları, yerel çalıştırma sonucu penceresindeki ilk 500 çıkış satırıyla her üç saniyede bir yenilenir ve çıktı dosyaları proje yolunuz **ASALocalRun** klasörüne yerleştirilir. Ayrıca, yerel çalıştırma sonucu penceresinde **Sonuçlar Klasörünü Aç** düğmesini tıklatarak çıktı dosyalarını da açabilirsiniz.

   ![Azure Stream Analytics Visual Studio canlı veri açık sonuçlar klasörü](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-open-results-folder.png)

5. Sonuçları bulut çıktılavabolarınıza çıkarmak istiyorsanız, ikinci açılır kutudan **Bulut'a Çıktı'yı** seçin. Power BI ve Azure Veri Gölü Depolaması desteklenmez çıktı lavaboları.

   ![Azure Stream Analytics Visual Studio buluta canlı veri çıkışı](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-output.png)
 
## <a name="limitations"></a>Sınırlamalar

* Power BI ve Azure Veri Gölü Depolama, kimlik doğrulama modeli sınırlamaları nedeniyle çıktı lavaboları desteklenmez.

* Yalnızca bulut giriş seçenekleri [zaman ilkeleri](stream-analytics-out-of-order-and-late-events.md) desteğine sahipken, yerel giriş seçenekleri desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio için Azure Akış Analizi araçlarını kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-vs.md)
* [Visual Studio ile Test Stream Analytics sorgularını yerel olarak sorgulayın](stream-analytics-vs-tools-local-run.md)
* [Azure Akış Analizi işlerini görüntülemek için Visual Studio'yı kullanın](stream-analytics-vs-tools.md)
