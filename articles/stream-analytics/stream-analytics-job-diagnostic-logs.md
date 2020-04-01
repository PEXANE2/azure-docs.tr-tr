---
title: Tanılama günlüklerini kullanarak Azure Akış Analizi sorun giderme
description: Bu makalede, Azure Akış Analizi'nde tanılama günlüklerinin nasıl analiz edilebildiğini açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: cdb6629441becd0a8356debe3360830ff11a7a9d
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398414"
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Tanılama günlüklerini kullanarak Azure Akış Analizi'ni sorun giderme

Bazen, Azure Stream Analytics işi çalışmayı beklenmedik bir şekilde durdurur. Bu tür bir olayın sorunlarını giderebilmek önemlidir. Hatalara beklenmedik bir sorgu sonucu, cihazların bağlantısı veya beklemedik bir hizmet kesintisi neden olmuş olabilir. Akış Analizi'ndeki tanılama günlükleri, sorunlar oluştuğunda nedenlerini belirlemenize ve kurtarma süresini azaltmanıza yardımcı olabilir.

Bu büyük ölçüde hata ayıklama ve izleme ile yardımcı olacaktır gibi tüm işler için tanı günlükleri etkinleştirmek için şiddetle tavsiye edilir.

## <a name="log-types"></a>Günlük türleri

Stream Analytics iki tür günlük sunar:

* İş üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlayan [etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (her zaman açık).

* Bir iş ile olur her şeyi daha zengin anlayışlar sağlayan [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (yapılandırılabilir). Tanılama günlükleri iş oluşturulduğunda başlar ve iş silindiğinde sona erer. İş güncellendiğinde ve çalışırken olayları kapsar.

> [!NOTE]
> Uygun olmayan verileri çözümlemek için Azure Depolama, Azure Etkinlik Hub'ları ve Azure Monitor günlükleri gibi hizmetleri kullanabilirsiniz. Bu hizmetlerin fiyatlandırma modeline göre ücretlendirilirsiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="debugging-using-activity-logs"></a>Etkinlik günlüklerini kullanarak hata ayıklama

Etkinlik günlükleri varsayılan olarak açıktır ve Stream Analytics işiniz tarafından gerçekleştirilen işlemler hakkında üst düzey öngörüler sağlar. Etkinlik günlüklerinde bulunan bilgiler, işinizi etkileyen sorunların temel nedenini bulmanıza yardımcı olabilir. Akış Analizi'nde etkinlik günlüklerini kullanmak için aşağıdaki adımları yapın:

1. Azure portalında oturum açın ve **Genel Bakış**altında **Etkinlik günlükünü** seçin.

   ![Akış Analizi etkinlik günlüğü](./media/stream-analytics-job-diagnostic-logs/stream-analytics-menu.png)

2. Gerçekleştirilen işlemlerin listesini görebilirsiniz. İşinizin başarısız olmasına neden olan herhangi bir işlemde kırmızı bilgi balonu vardır.

3. Özet görünümünü görmek için bir işlemi tıklatın. Buradaki bilgiler genellikle sınırlıdır. İşlem hakkında daha fazla bilgi edinmek için **JSON'ı**tıklatın.

   ![Akış Analizi etkinlik günlüğü işlem özeti](./media/stream-analytics-job-diagnostic-logs/operation-summary.png)

4. Başarısız işlem nedeniyle hatanın ayrıntılarını sağlayan JSON'un **Özellikler** bölümüne aşağı kaydırın. Bu örnekte, hata bağlı enlem değerlerinin dışında bir çalışma zamanı hatası nedeniyle oldu. Bir Akış Analizi işi tarafından işlenen verilerdeki tutarsızlık bir veri hatasına neden olur. Farklı [giriş ve çıktı veri hataları ve bunların neden oluştuğunu](https://docs.microsoft.com/azure/stream-analytics/data-errors)öğrenebilirsiniz.

   ![JSON hata ayrıntıları](./media/stream-analytics-job-diagnostic-logs/error-details.png)

5. JSON'daki hata iletisini temel alan düzeltici eylemler de yapabilirsiniz. Bu örnekte, enlem değerinin -90 derece ile 90 derece arasında olduğundan emin olunması için denetimlerin sorguya eklenmesi gerekir.

6. Etkinlik günlüklerinde hata iletisi kök nedenini belirlemede yararlı değilse, tanılama günlüklerini etkinleştirin ve Azure Monitor günlüklerini kullanın.

## <a name="send-diagnostics-to-azure-monitor-logs"></a>Azure Monitor günlüklerine tanılama gönderme

Tanılama günlüklerini açmak ve Azure Monitor günlüklerine göndermek şiddetle tavsiye edilir. Tanılama günlükleri varsayılan olarak **kapalıdır.** Tanılama günlüklerini açmak için aşağıdaki adımları tamamlayın:

1.  Azure portalında oturum açın ve Akış Analizi işinize gidin. **İzleme**altında, **Tanılama günlüklerini**seçin. Ardından **tanılamayı Aç'ı**seçin.

    ![Tanılama günlüklerine bıçak gezintisi](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs-monitoring.png)  

2.  **Tanılama ayarlarında** bir **Ad** oluşturun ve **Günlük Analizi'ne Gönder'in**yanındaki kutuyu işaretleyin. Sonra varolan bir bilgi ekleyin veya yeni bir **Log analitik çalışma alanı**oluşturun. **LOG**altında **Yürütme** ve **Yazma** için kutuları ve **METRIC**altında **AllMetrics'i** işaretleyin. **Kaydet**'e tıklayın. Ek maliyetleri önlemek için Akış Analizi işinizle aynı Azure bölgesinde bir Günlük Analizi çalışma alanı kullanmanız önerilir.

    ![Tanılama günlükleri için ayarlar](./media/stream-analytics-job-diagnostic-logs/diagnostic-settings.png)

3. Akış Analizi işiniz başladığında, tanılama günlükleri Log Analytics çalışma alanınıza yönlendirilir. İşinizin tanı günlüklerini görüntülemek için **İzleme** bölümünün altındaki **Günlükler'i** seçin.

   ![İzleme Altında Tanılama Günlükleri](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs.png)

4. Akış Analizi, ilgilendiğiniz günlükleri kolayca aramanıza olanak tanıyan önceden tanımlanmış sorgular sağlar. 3 **kategoride Genel**, **Giriş veri hataları** ve Çıkış veri **hataları**vardır. Örneğin, son 7 gün içinde işinizin tüm hatalarının bir özetini görmek için, uygun önceden tanımlanmış sorgunun **Çalıştır'ını** seçebilirsiniz. 

   ![İzleme Altında Tanılama Günlükleri](./media/stream-analytics-job-diagnostic-logs/logs-categories.png)

   ![Günlüklerin sonuçları](./media/stream-analytics-job-diagnostic-logs/logs-result.png)

## <a name="diagnostics-log-categories"></a>Tanılama günlüğü kategorileri

Azure Akış Analizi iki tanılama günlüğü kategorisi yakalar:

* **Yazar :** İş oluşturma, girdi ve çıktı ekleme ve silme, sorguekleme ve güncelleme ve işi başlatma veya durdurma gibi iş yazma işlemleriyle ilgili günlük olaylarını yakalar.

* **Yürütme**: İş yürütme sırasında meydana gelen olayları yakalar.
    * Bağlantı hataları
    * Veri işleme hataları, dahil:
        * Sorgu tanımına uymayan olaylar (eşleşmemiş alan türleri ve değerleri, eksik alanlar vb.)
        * İfade değerlendirme hataları
    * Diğer olaylar ve hatalar

## <a name="diagnostics-logs-schema"></a>Tanılama şema günlükleri

Tüm günlükler JSON formatında saklanır. Her girişaşağıdaki ortak dize alanları vardır:

Adı | Açıklama
------- | -------
time | Günlüğün zaman damgası (UTC'de).
resourceId | Büyük durumda, işlemin gerçekleştiği kaynağın kimliği. Abonelik kimliğini, kaynak grubunu ve iş adını içerir. Örneğin, **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB**.
category | Oturum açma kategorisi, **Yürütme** veya **Yazma**.
operationName | Günlüğe kaydedilen işlemin adı. Örneğin, **Olay Gönder: SQL Output mysqloutput için hata yazma**.
durum | Operasyonun durumu. Örneğin, **Başarısız** veya **Başarılı**.
düzey | Günlük düzeyi. Örneğin, **Hata**, **Uyarı**veya **Informational**.
properties | Girişe özgü ayrıntılar, JSON dizesi olarak serihale getir. Daha fazla bilgi için bu makaledeki aşağıdaki bölümlere bakın.

### <a name="execution-log-properties-schema"></a>Yürütme günlüğü özellikleri şema

Yürütme günlüklerinde, Akış Analizi iş yürütme sırasında meydana gelen olaylar hakkında bilgi vardır. Özelliklerin şema olay bir veri hatası veya genel bir olay olup olmadığına bağlı olarak değişir.

### <a name="data-errors"></a>Veri hataları

İş verileri işlerken oluşan herhangi bir hata bu günlükler kategorisindedir. Bu günlükler genellikle veri okuma, serileştirme ve yazma işlemleri sırasında oluşturulur. Bu günlükler bağlantı hatalarını içermez. Bağlantı hataları genel olaylar olarak kabul edilir. Çeşitli farklı giriş ve çıktı [veri hatalarının](https://docs.microsoft.com/azure/stream-analytics/data-errors)nedeni hakkında daha fazla bilgi edinebilirsiniz.

Adı | Açıklama
------- | -------
Kaynak | Hatanın oluştuğu iş girişinin veya çıktının adı.
İleti | Hatayla ilişkili ileti.
Tür | Hata türü. Örneğin, **DataConversionError**, **CsvParserError**veya **ServiceBusPropertyColumnMissingError**.
Veriler | Hatanın kaynağını doğru bulmak için yararlı olan verileri içerir. Boyutuna bağlı olarak kesilmeye tabidir.

**IşlemAdı** değerine bağlı olarak, veri hataları aşağıdaki şema var:

* Olay okuma işlemleri sırasında meydana gelen **olayları serileştirme.** Girişteki veriler aşağıdaki nedenlerden biri için sorgu şemasını karşılamazsa oluşurlar:

   * *Olay sırasında tür uyuşmazlığı (de)serialize*: Hataya neden olan alanı tanımlar.

   * *Bir olayı okuyamıyor, geçersiz serileştirme*: Hatanın oluştuğu giriş verilerindeki konumla ilgili bilgileri listeler. Blob girişi, ofset ve verilerin bir örneği için blob adı içerir.

* **Gönderme olayları** yazma işlemleri sırasında oluşur. Hataya neden olan akış olayını tanımlarlar.

### <a name="generic-events"></a>Genel olaylar

Genel olaylar diğer her şeyi kapsar.

Adı | Açıklama
-------- | --------
Hata | (isteğe bağlı) Hata bilgileri. Genellikle, bu kullanılabilir ise özel durum bilgileridir.
İleti| İletiyi günlüğe kaydedin.
Tür | İleti türü. Hataların iç kategorizesine eşler. Örneğin, **JobValidationError** veya **BlobOutputAdapterInitializationError**.
Bağıntı Kimliği | İş yürütmeyi benzersiz olarak tanımlayan [GUID.](https://en.wikipedia.org/wiki/Universally_unique_identifier) İş durduğu anda tüm yürütme günlüğü girişleri aynı **Korelasyon Kimliği** değerine sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

* [Akış Analizine Giriş](stream-analytics-introduction.md)
* [Akış Analizi ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Akış Analizi sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Akış Analizi veri hataları](https://docs.microsoft.com/azure/stream-analytics/data-errors)
