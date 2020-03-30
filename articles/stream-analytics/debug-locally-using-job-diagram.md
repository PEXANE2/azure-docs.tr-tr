---
title: Visual Studio'da iş diyagramını kullanarak Azure Akış Analizi sorgularını yerel olarak ayıklama
description: Bu makalede, Visual Studio için Azure Akış Analizi Araçları'nda iş diyagramını kullanarak sorguları yerel olarak nasıl ayıklanırın açıklanmaktadır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: 106b1f0b765700803d2cd55b5e049fae5be3dfad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76847205"
---
# <a name="debug-azure-stream-analytics-queries-locally-using-job-diagram-in-visual-studio"></a>Visual Studio'da iş diyagramını kullanarak Azure Akış Analizi sorgularını yerel olarak ayıklama

Sonuç elde edilemeyen işler veya beklenmeyen sonuçlar, akış sorguları için sık karşılaşılan sorun giderme senaryolarıdır. Her adım için ara sonuç kümesini ve ölçümlerini incelemek için sorgunuzu Visual Studio'da yerel olarak sınarken iş diyagramını kullanabilirsiniz. İş diyagramları, sorunları giderdiğinizde sorunun kaynağını hızlı bir şekilde yalıtmanıza yardımcı olabilir.

## <a name="debug-a-query-using-job-diagram"></a>İş diyagramı kullanarak sorguhataayık

Bir Azure Akışı Analizi komut dosyası, giriş verilerini çıktı verilerine dönüştürmek için kullanılır. İş diyagramı, birden çok sorgu adımı ve son olarak da çıktı lavaboları aracılığıyla veri giriş kaynaklarından (Event Hub, IoT Hub, vb.) nasıl aktığını gösterir. Her sorgu adımı, bir deyim kullanılarak komut dosyasında `WITH` tanımlanan geçici bir sonuç kümesine eşlenir. Bir sorunun kaynağını bulmak için ayarlanan her ara sonuçta her sorgu adımının verilerinin yanı sıra ölçümleri de görüntüleyebilirsiniz.

> [!NOTE]
> Bu iş diyagramı yalnızca tek bir düğümdeki yerel sınama için verileri ve ölçümleri gösterir. Performans atonlama ve sorun giderme için kullanılmamalıdır.

### <a name="start-local-testing"></a>Yerel sınama başlatın

Visual Studio'u kullanarak Bir Akış Analizi işi oluşturmayı veya [varolan bir işi yerel bir projeye nasıl dışa aktarın'](stream-analytics-vs-tools.md#export-jobs-to-a-project)ı öğrenmek için bu [Hızlı Başlat'ı](stream-analytics-quick-create-vs.md) kullanın. Sorguyu yerel giriş verileriyle sınamak istiyorsanız, aşağıdaki [yönergeleri](stream-analytics-live-data-local-testing.md)izleyin. Canlı girişle test etmek istiyorsanız, bir sonraki adıma geçin.

> [!NOTE]
> Bir işi yerel projeye dışa aktarıyorsanız ve canlı giriş akışına karşı sınamak istiyorsanız, tüm girişlerin kimlik bilgilerini yeniden belirtmeniz gerekir.  

Komut dosyası düzenleyicisinden giriş ve çıktı kaynağını seçin ve **yerel olarak Çalıştır'ı**seçin. İş diyagramı sağ tarafta görünür.

### <a name="view-the-intermediate-result-set"></a>Ara sonuç kümesini görüntüleme  

1. Komut dosyasına gitmek için sorgu adımını seçin. Otomatik olarak soldaki düzenleyicideki ilgili komut dosyasına yönlendirilirsiniz.

   ![İş diyagramı komut dosyasıgezinmek](./media/debug-locally-using-job-diagram/navigate-script.png)

2. Sorgu adımını seçin ve açılan iletişim kutusunda **Önizleme'yi** seçin. Sonuç kümesi, alt sonuç penceresindeki bir sekmede gösterilir.

   ![İş diyagramı önizleme sonucu](./media/debug-locally-using-job-diagram/preview-result.png)

### <a name="view-step-metrics"></a>Adım ölçümlerini görüntüleme

Bu bölümde, diyagramın her bölümü için kullanılabilir ölçümleri incelersiniz.

#### <a name="input-sources-live-stream"></a>Giriş kaynakları (Canlı akış)

![İş diyagramı canlı giriş kaynakları](./media/debug-locally-using-job-diagram/live-input.png)

|Ölçüm|Açıklama|
|-|-|
|**TaksiRide**| Girişin adı.|
|**Etkinlik Merkezi** | Giriş kaynak türü.|
|**Olaylar**|Okunan olay sayısı.|
|**Backlogged Olay Kaynakları**|Olay Hub'ları ve IoT Hub girişleri için daha kaç iletinin okunması gerekir.|
|**Bayt'taki Olaylar**|Okunan bayt sayısı.|
| **Bozulmuş Olaylar**|Deserialization dışında bir sorun vardı olayların sayısı.|
|**Erken Olaylar**| Yüksek filigrandan önce uygulama zaman damgası olan olayların sayısı.|
|**Geç Olaylar**| Yüksek filigrandan sonra uygulama zaman damgası olan olayların sayısı.|
|**Etkinlik Kaynakları**| Veri birimlerinin sayısı okundu. Örneğin, lekelerin sayısı.|

#### <a name="input-sources-local-input"></a>Giriş kaynakları (Yerel giriş)

![İş diyagramı yerel giriş kaynakları](./media/debug-locally-using-job-diagram/local-input.png)

|Ölçüm|Açıklama|
|-|-|
|**TaksiRide**| Girişin adı.|
|**Satır Sayısı**| Adımdan oluşturulan satır sayısı.|
|**Veri Boyutu**| Bu adımdan oluşturulan verilerin boyutu.|
|**Yerel giriş**| Giriş olarak yerel verileri kullanın.|

#### <a name="query-steps"></a>Sorgu adımları

![İş diyagramı sorgu adımı](./media/debug-locally-using-job-diagram/query-step.png)

|Ölçüm|Açıklama|
|-|-|
|**TripData**|Geçici sonuç kümesinin adı.|
|**Satır Sayısı**| Adımdan oluşturulan satır sayısı.|
|**Veri Boyutu**| Bu adımdan oluşturulan verilerin boyutu.|
  
#### <a name="output-sinks-live-output"></a>Çıkış lavaboları (Canlı çıktı)

![İş diyagramı yerel çıktı lavabolar](./media/debug-locally-using-job-diagram/live-output.png)

|Ölçüm|Açıklama|
|-|-|
|**regionaggEH**|Çıktının adı.|
|**Olaylar**|Lavabolara çıkan olay çıktılarının sayısı.|

#### <a name="output-sinks-local-output"></a>Çıktı lavaboları (Yerel çıktı)

![İş diyagramı yerel çıktı lavabolar](./media/debug-locally-using-job-diagram/local-output.png)

|Ölçüm|Açıklama|
|-|-|
|**regionaggEH**|Çıktının adı.|
|**Yerel Çıktı**| Yerel bir dosyaya sonuç çıktısı.|
|**Satır Sayısı**| Yerel dosyaya satır çıktısı sayısı.|
|**Veri Boyutu**| Yerel dosyaya veri çıktısının boyutu.|

### <a name="close-job-diagram"></a>İş diyagramı kapatma

Artık iş diyagramına ihtiyacınız yoksa, sağ üst köşede **Kapat'ı** seçin. Diyagram penceresini kapattıktan sonra, görmek için yerel sınama yı yeniden başlatmanız gerekir.

### <a name="view-job-level-metrics-and-stop-running"></a>İş düzeyi ölçümlerini görüntüleyin ve çalıştırmayı durdurun

Diğer iş düzeyi ölçümleri açılır konsolda gösterir. İşi durdurmak istiyorsanız konsoldaki **Ctrl+C** tuşuna basın.

![İş diyagramı işi durdur](./media/debug-locally-using-job-diagram/stop-job.png)

## <a name="limitations"></a>Sınırlamalar

* Power BI ve Azure Veri Gölü Depolama Gen1 çıktıları kimlik doğrulama modeli sınırlamaları nedeniyle desteklenmez.

* Yalnızca bulut giriş seçenekleri [zaman ilkeleri](stream-analytics-out-of-order-and-late-events.md) desteğine sahipken, yerel giriş seçenekleri desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Quickstart: Visual Studio'u kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-vs.md)
* [Azure Akış Analizi işlerini görüntülemek için Visual Studio'yı kullanın](stream-analytics-vs-tools.md)
* [Visual Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak test edin (Önizleme)](stream-analytics-live-data-local-testing.md)
