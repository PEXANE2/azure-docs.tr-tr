---
title: Azure Stream Analytics, tanılama günlüklerini kullanarak sorun giderme
description: Bu makalede, Azure Stream analytics'te tanılama günlüklerini çözümleme açıklar.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/19/2019
ms.openlocfilehash: f318b373f6a6f46ee3a85703c6099c76568580ba
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426119"
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Azure Stream Analytics, tanılama günlükleri kullanarak sorun giderme

Bazen, Azure Stream Analytics işi çalışmayı beklenmedik bir şekilde durdurur. Bu tür bir olayın sorunlarını giderebilmek önemlidir. Hatalara beklenmedik bir sorgu sonucu, cihazların bağlantısı veya beklemedik bir hizmet kesintisi neden olmuş olabilir. Stream Analytics tanılama günlükleri, oluşabilecek sorunları belirlemenize ve kurtarma süresini azaltmanıza yardımcı olabilir.

Bu, hata ayıklama ve izleme konusunda büyük ölçüde yardımcı olacağı için tüm işler için tanılama günlüklerinin etkinleştirilmesi önemle önerilir.

## <a name="log-types"></a>Günlük türü

Stream Analytics, günlükleri iki tür sunar:

* İşler üzerinde gerçekleştirilen işlemlere Öngörüler veren [etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (Always on).

* Bir işle gerçekleşen her şeye daha zengin Öngörüler sağlayan [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (yapılandırılabilir). Tanılama günlükleri iş oluşturulduğunda başlar ve iş silindiğinde biter. Bunlar, iş güncelleştirildiğinde ve çalışırken olayları kapsar.

> [!NOTE]
> Uyumsuz verileri çözümlemek için Azure depolama, Azure Event Hubs ve Azure Izleyici günlükleri gibi hizmetleri kullanabilirsiniz. Bu hizmetler için fiyatlandırma modeline göre ücretlendirilirsiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="debugging-using-activity-logs"></a>Etkinlik günlüklerini kullanarak hata ayıklama

Etkinlik günlükleri varsayılan olarak açık ve Stream Analytics işiniz tarafından gerçekleştirilen işlemlere yüksek düzeyde öngörüler sağlar. Etkinlik günlüklerinde bulunan bilgiler, işinizi etkileyen sorunların temel nedenini bulmanıza yardımcı olabilir. Stream Analytics 'de etkinlik günlüklerini kullanmak için aşağıdaki adımları uygulayın:

1. Azure portal oturum açın ve **genel bakış**altında **etkinlik günlüğü** ' nü seçin.

   ![Etkinlik günlüğünü Stream Analytics](./media/stream-analytics-job-diagnostic-logs/stream-analytics-menu.png)

2. Gerçekleştirilmiş işlemlerin bir listesini görebilirsiniz. İşinizin başarısız olmasına neden olan herhangi bir işlemin kırmızı bir bilgi balonu vardır.

3. Özet görünümünü görmek için bir işleme tıklayın. Buradaki bilgiler genellikle sınırlıdır. İşlem hakkında daha fazla bilgi edinmek için **JSON**' a tıklayın.

   ![Stream Analytics etkinlik günlüğü işlem Özeti](./media/stream-analytics-job-diagnostic-logs/operation-summary.png)

4. JSON 'ın **Özellikler** bölümüne ilerleyin ve bu, başarısız olan işleme neden olan hatanın ayrıntılarını sağlar. Bu örnekte hata, bağlı Enlem değerlerinin dışında bir çalışma zamanı hatası nedeniyle oluştu. Bir Stream Analytics işi tarafından işlenen verilerde tutarsızlık bir veri hatasına neden olur. Farklı [giriş ve çıkış verileri hataları ve neden gerçekleştikleri](https://docs.microsoft.com/azure/stream-analytics/data-errors)hakkında bilgi edinebilirsiniz.

   ![JSON hata ayrıntıları](./media/stream-analytics-job-diagnostic-logs/error-details.png)

5. JSON 'daki hata iletisine göre düzeltici eylemler gerçekleştirebilirsiniz. Bu örnekte, enlem değerinin ila 90 derece arasında olduğundan emin olup, sorguya 90 derece eklenmelidir.

6. Etkinlik günlüklerindeki hata iletisi, kök nedeni belirlemek için yararlı değilse, tanılama günlüklerini etkinleştirin ve Azure Izleyici günlüklerini kullanın.

## <a name="send-diagnostics-to-azure-monitor-logs"></a>Azure Izleyici günlüklerine tanılama gönder

Tanılama günlüklerini açıp Azure Izleyici günlüklerine göndermek, önemle önerilir. Tanılama günlükleri **kapalı** varsayılan olarak. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları tamamlayın:

1.  Azure portal oturum açın ve Stream Analytics işinize gidin. Altında **izleme**seçin **tanılama günlükleri**. Sonra **tanılamayı aç '** ı seçin.

    ![Tanılama günlükleri dikey penceresinde gezinme](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs-monitoring.png)  

2.  **Tanılama ayarları** ' nda bir **ad** oluşturun ve **Log Analytics gönder**' in yanındaki kutuyu işaretleyin. Sonra var olan veya yeni bir **Log Analytics çalışma alanı**oluşturun. **Günlük**altında **yürütme** ve **yazma** kutularını ve **ölçüm**' in altındaki **allölçümleri** ' ni işaretleyin. **Save (Kaydet)** düğmesine tıklayın. Ek maliyetleri engellemek için Stream Analytics işiniz ile aynı Azure bölgesinde bir Log Analytics çalışma alanı kullanmanız önerilir.

    ![Tanılama günlükleri için ayarlar](./media/stream-analytics-job-diagnostic-logs/diagnostic-settings.png)

3. Stream Analytics işiniz başlatıldığında, tanılama günlükleri Log Analytics çalışma alanınıza yönlendirilir. İşiniz için tanılama günlüklerini görüntülemek için **izleme** bölümünün altındaki **Günlükler** ' i seçin.

   ![Izleme altındaki tanılama günlükleri](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs.png)

4. Stream Analytics, ilgilendiğiniz günlüklere kolayca arama yapmanıza olanak tanıyan önceden tanımlanmış sorgular sağlar. 3 kategori **genel**, **giriş verileri hataları** ve **çıkış verileri hataları**. Örneğin, son 7 gün içinde işinizin tüm hatalarının özetini görmek için, uygun önceden tanımlanmış sorgunun **çalıştırılmasını** seçebilirsiniz. 

   ![Izleme altındaki tanılama günlükleri](./media/stream-analytics-job-diagnostic-logs/logs-categories.png)

   ![Günlüklerin sonuçları](./media/stream-analytics-job-diagnostic-logs/logs-result.png)

## <a name="diagnostics-log-categories"></a>Tanılama günlük kategorileri

Azure Stream Analytics iki tanılama günlüğü kategorisini yakalar:

* **Yazma**: iş oluşturma, giriş ve çıkışları ekleme ve silme, sorguyu ekleme ve güncelleştirme ve işi başlatma veya durdurma gibi iş yazma işlemleriyle ilgili günlük olaylarını yakalar.

* **Yürütme**: iş yürütme sırasında oluşan olayları yakalar.
    * Bağlantı hataları
    * Dahil olmak üzere, veri işleme hataları:
        * Sorgu tanımı (eşleşmeyen alan türleri ve değerleri, eksik alanlar vb.) için uygun olmayan olayları
        * İfade değerlendirme hataları
    * Diğer olayları ve hataları

## <a name="diagnostics-logs-schema"></a>Tanılama günlükleri şeması

Tüm günlükler, JSON biçiminde depolanır. Her girişin aşağıdaki ortak dize alanlara sahiptir:

Ad | Açıklama
------- | -------
time | Zaman damgası (UTC) günlüğü.
resourceId | İşlem büyük harf, yerinde geçen kaynak kimliği. Bu abonelik kimliği, kaynak grubunu ve iş adı içerir. Örneğin,   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMINGJOBS/STREAMANALYTICS/MYSTREAMINGJOB**.
category | Kategori ya da oturum **yürütme** veya **yazma**.
operationName | Oturum açmış işlemin adı. Örneğin, **olayları gönderme: SQL çıkış yazma hatası için mysqloutput**.
status | İşlemin durumu. Örneğin, **başarısız** veya **başarılı**.
level | Günlük düzeyi. Örneğin, **hata**, **uyarı**, veya **bilgilendirici**.
properties | Günlük girdisi özgü ayrıntılı olarak serileştirilmiş bir JSON dizesi. Daha fazla bilgi için bu makaledeki aşağıdaki bölümlere bakın.

### <a name="execution-log-properties-schema"></a>Yürütme günlüğü özellikleri şeması

Stream Analytics iş yürütme sırasında gerçekleşen olaylar hakkında bilgi yürütme günlükleri vardır. Özelliklerin şeması, olayın bir veri hatası veya genel bir olay olmasına bağlı olarak değişir.

### <a name="data-errors"></a>Veri hataları

İş verileri işlerken oluşan herhangi bir hata günlükleri Bu kategoride var. Bu günlükler genellikle okuma, oluşturma sırasında yerleşim verilerine seri hale getirme, oluşturulur ve yazma işlemleri. Bu günlükler, bağlantı hataları içermez. Bağlantı hataları genel olayları olarak kabul edilir. Çeşitli farklı [giriş ve çıkış verileri hatalarının](https://docs.microsoft.com/azure/stream-analytics/data-errors)nedeni hakkında daha fazla bilgi edinebilirsiniz.

Ad | Açıklama
------- | -------
Kaynak | İş adı, giriş veya hatanın oluştuğu çıktı.
İleti | Hatayla ilişkili ileti.
Tür | Hata türü. Örneğin, **DataConversionError**, **CsvParserError**, veya **ServiceBusPropertyColumnMissingError**.
Veriler | Hatanın kaynağını doğru bir şekilde bulmak yararlı olan veriler içerir. Kesilme tabidir, boyutuna bağlı olarak.

Yapılandırmanıza bağlı olarak **operationName** değeri aşağıdaki şema veri hatalar var:

* Olay okuma işlemleri sırasında **serileştirme olayları** oluşur. Giriş verileri Bu nedenlerden biri için sorgu şeması karşılamaz bunlar ortaya çıkar:

   * *Tür uyumsuzluğu (de) olay sırasında serileştirmek*: hataya neden olan bir alan tanımlar.

   * *Bir olay, geçersiz bir seri hale getirme okunamıyor*: hatanın oluştuğu giriş verilerinin konumu hakkında bilgi listeler. BLOB adı için blob giriş uzaklığı ve verilerin bir örnek içerir.

* Yazma işlemleri sırasında **gönderme olayları** oluşur. Bunlar, hataya neden olan akış olayı tanımlar.

### <a name="generic-events"></a>Genel olaylar

Genel olaylar, diğer her şeyi kapsar.

Ad | Açıklama
-------- | --------
Hata | (isteğe bağlı) Hata bilgileri. Genellikle, bu, varsa özel durum bilgileri olur.
İleti| Günlük iletisi.
Tür | İleti türü. İç hataların kategorilere ayrılması eşlenir. Örneğin, **JobValidationError** veya **BlobOutputAdapterInitializationFailure**.
Bağıntı Kimliği | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) tanımlamasının, iş yürütme. Aynı işi durur bulunana kadar andan itibaren tüm yürütme günlüğü girişleri işini başlatır. **bağıntı kimliği** değeri.

## <a name="next-steps"></a>Sonraki adımlar

* [Stream analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Stream Analytics veri hataları](https://docs.microsoft.com/azure/stream-analytics/data-errors)
