---
title: Visual Studio Code iş diyagramını kullanarak Azure Stream Analytics sorguları yerel olarak ayıklayın
description: Bu makalede, Visual Studio Code için Azure Stream Analytics uzantısında iş diyagramını kullanarak sorguların yerel olarak nasıl ayıklanacağı açıklanır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: c31f3c998df918466e707c95f041592051e8251c
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045323"
---
# <a name="debug-azure-stream-analytics-queries-locally-using-job-diagram-in-visual-studio-code"></a>Visual Studio Code iş diyagramını kullanarak Azure Stream Analytics sorguları yerel olarak ayıklayın

Sonuç olarak çıkış yapan veya beklenmedik sonuçlara neden olan akış işleri genellikle sorun gidermeye gerek duyar. Azure Stream Analytics için Visual Studio Code uzantısı, bir sorunun kaynağını hızlı bir şekilde ayırmanıza yardımcı olması için iş diyagramlarını, ölçümleri, tanılama günlüklerini ve ara sonuçları tümleştirir. Her adımın ara sonuç kümesini ve ölçümlerini incelemek için sorgunuzu yerel olarak test ederken iş diyagramını kullanabilirsiniz.

## <a name="debug-a-query-using-job-diagram"></a>İş diyagramı kullanarak bir sorgu hatalarını ayıklama

Giriş verilerini çıktı verilerine dönüştürmek için bir Azure Stream Analytics betiği kullanılır. İş diyagramı, çıkış havuzları için birden çok sorgu adımı aracılığıyla Olay Hub 'ı veya IoT Hub gibi giriş kaynaklarından verilerin nasıl akacağını gösterir. Her sorgu adımı, bir ifade kullanılarak betikte tanımlanan geçici bir sonuç kümesiyle eşlenir `WITH` . Bir sorunun kaynağını bulmak için her bir ara sonuç kümesindeki her bir sorgu adımındaki ölçümleri ve verileri görüntüleyebilirsiniz.

> [!NOTE]
> Bu iş diyagramı yalnızca tek bir düğümdeki yerel test için verileri ve ölçümleri gösterir. Performans ayarlama ve sorun giderme için kullanılmamalıdır.

### <a name="start-local-testing"></a>Yerel Testi Başlat

Visual Studio Code kullanarak Stream Analytics bir iş oluşturmayı veya [var olan bir işi yerel bir projeye aktarmayı](visual-studio-code-explore-jobs.md)öğrenmek Için bu [hızlı](quick-create-vs-code.md) başlangıcı kullanın. Giriş ve çıkışlara yönelik kimlik bilgileri, otomatik olarak dışarıya aktarılmış işler için doldurulur.

Sorguyu yerel giriş verileriyle test etmek istiyorsanız, bu [yönergeleri](visual-studio-code-local-run.md)izleyin. Canlı giriş ile test etmek istiyorsanız, [giriş alanınızı](stream-analytics-add-inputs.md) bir sonraki adıma geçin. 

* \. Aşama QL* betik dosyasını açın ve **yerel olarak çalıştır**' ı seçin. Ardından, **yerel giriş kullan** veya **canlı giriş kullan**' ı seçin. İş diyagramı pencerenin sağ tarafında görünür.

### <a name="view-the-output-and-intermediate-result-set"></a>Çıktıyı ve ara sonuç kümesini görüntüleme  

1. Tüm iş çıkışları, Visual Studio Code penceresinin sağ alt tarafındaki sonuç penceresinde görüntülenir.

   > [!div class="mx-imgBorder"]
   > ![İş çıkışı sonuçları](./media/debug-locally-using-job-diagram-vs-code/job-output-results.png)

2. Betiğe gitmek için sorgu adımını seçin. Sol taraftaki düzenleyicide ilgili betiğe otomatik olarak yönlendirilirsiniz. Ara sonuç, Visual Studio Code penceresinin sağ alt tarafındaki sonuç penceresinde görünür.

   > [!div class="mx-imgBorder"]
   > ![İş diyagramı önizleme sonucu](./media/debug-locally-using-job-diagram-vs-code/preview-result.png)

### <a name="view-metrics"></a>Ölçümleri görüntüle

Bu bölümde, diyagramın her bir bölümü için kullanılabilen ölçümleri keşfedebilirsiniz.

1. Visual Studio Code penceresinin sağ alt tarafındaki **sonuç** sekmesinin yanındaki **ölçümler** sekmesini seçin.

2. Açılan listeden **iş** ' ı seçin. Bir Graph düğümündeki boş alanı seçerek iş düzeyi ölçümlerine gidebilirsiniz. Bu görünüm, iş çalışırken her 10 saniyede bir güncellenen tüm ölçümleri içerir. Bunları grafiklerde görüntülemek için sağ taraftaki ölçümleri seçebilir veya seçimden kaldırabilirsiniz.

   > [!div class="mx-imgBorder"]
   > ![İş diyagramı ölçümleri](./media/debug-locally-using-job-diagram-vs-code/job-metrics.png)

3. Giriş ölçümlerini görmek için açılan listeden giriş veri kaynağının adını seçin. Aşağıdaki ekran görüntüsünde bulunan giriş kaynağına *tırnak işareti*adı verilir. Giriş ölçümleri hakkında daha fazla bilgi için bkz. [Stream Analytics iş Izlemeyi anlama ve sorguları izleme](stream-analytics-monitoring.md).

   > [!div class="mx-imgBorder"]
   > ![İş diyagramı ölçümleri](./media/debug-locally-using-job-diagram-vs-code/input-metrics.png)

4. İş diyagramından bir sorgu adımı seçin veya adım düzeyi ölçümleri görmek için açılan menüden adım adını seçin. Filigran gecikmesi, kullanılabilen tek adım ölçümdür.

   > [!div class="mx-imgBorder"]
   > ![Adım ölçümleri](./media/debug-locally-using-job-diagram-vs-code/step-metrics.png)

5. Çıktı ile ilgili ölçümleri görmek için diyagramda veya açılan listeden bir çıkış seçin. Çıkış ölçümleri hakkında daha fazla bilgi için bkz. [Stream Analytics iş Izlemeyi anlama ve sorguları izleme](stream-analytics-monitoring.md). Canlı çıkış havuzları desteklenmez.

   > [!div class="mx-imgBorder"]
   > ![Çıkış ölçümleri](./media/debug-locally-using-job-diagram-vs-code/output-metrics.png)

### <a name="view-diagnostic-logs"></a>Tanılama günlüklerini görüntüleme

İş düzeyi tanılama günlükleri, giriş veri kaynakları ve çıkış havuzları için tanılama bilgilerini içerir. Bir giriş düğümü veya çıkış düğümü seçtiğinizde, yalnızca ilgili Günlükler gösterilir. Bir sorgu adımı seçerseniz hiçbir günlük gösterilmez. Tüm günlükleri iş düzeyinde bulabilir ve günlükleri önem derecesine ve zamana göre filtreleyebilirsiniz.

   > [!div class="mx-imgBorder"]
   > ![Tanılama günlükleri](./media/debug-locally-using-job-diagram-vs-code/diagnostic-logs.png)

   Tüm iletiyi görmek için bir günlük girişi seçin.

   > [!div class="mx-imgBorder"]
   > ![Tanılama günlükleri iletisi](./media/debug-locally-using-job-diagram-vs-code/diagnostic-logs-message.png)


## <a name="other-job-diagram-features"></a>Diğer iş diyagramı özellikleri

Gerektiğinde araç çubuğundan **Durdur** veya **Duraklat** seçeneğini belirleyebilirsiniz. İş duraklatıldıktan sonra, bunu son çıktıdan sürdürebilirsiniz.

> [!div class="mx-imgBorder"]
> ![İşi Durdur veya Duraklat](./media/debug-locally-using-job-diagram-vs-code/stop-pause-job.png)

Yerel işiniz için özellikleri ve konfigürasyonları görmek üzere iş diyagramının sağ üst kısmındaki **Iş Özeti** ' ni seçin.

> [!div class="mx-imgBorder"]
> ![Yerel iş Özeti](./media/debug-locally-using-job-diagram-vs-code/job-summary.png)

## <a name="limitations"></a>Sınırlamalar

* Canlı çıkış havuzları yerel çalıştırmada desteklenmez.

* JavaScript işleviyle yerel olarak iş çalıştırma yalnızca Windows işletim sisteminde desteklenir.

* C# özel kodu ve Azure Machine Learning işlevleri desteklenmez. 

* Yalnızca bulut girişi seçeneklerinde [zaman ilkeleri](stream-analytics-out-of-order-and-late-events.md) desteklenir, ancak yerel giriş seçenekleri değildir.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio Code kullanarak Stream Analytics işi oluşturma](quick-create-vs-code.md)
* [Visual Studio Code ile Azure Stream Analytics keşfet](visual-studio-code-explore-jobs.md)
* [Visual Studio Code kullanarak örnek verilerle yerel olarak Stream Analytics sorguları test edin](visual-studio-code-local-run.md)
* [Visual Studio Code kullanarak canlı giriş ile Azure Stream Analytics işleri yerel olarak test etme](visual-studio-code-local-run-live-input.md)
