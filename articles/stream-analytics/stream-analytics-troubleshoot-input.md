---
title: Azure Akış Analizi için Sorun Giderme Girişleri
description: Bu makalede, Azure Akış Analizi işlerinde giriş bağlantılarınızı giderme teknikleri açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: 3d88123b3dd79e5707c5c19cbbae13c30cbdeb84
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409408"
---
# <a name="troubleshoot-input-connections"></a>Giriş bağlantısı sorunlarını giderme

Bu makalede, Azure Akış Analizi giriş bağlantılarıyla ilgili sık karşılaşılan sorunlar, giriş sorunlarının nasıl giderilen ve sorunların nasıl düzeltilene açıklanmaktadır. Birçok sorun giderme adımı, Akış Analizi işiniz için tanılama günlüklerinin etkinleştirilmesini gerektirir. Tanılama günlükleriniz etkinleştirildiyse, [tanılama günlüklerini kullanarak Azure Akış Analizi Sorun Giderme](stream-analytics-job-diagnostic-logs.md)bölümüne bakın.

## <a name="input-events-not-received-by-job"></a>İş tarafından alınmayan giriş olayları 

1.  Giriş ve çıktı bağlantınızı test edin. Her giriş ve çıkış için Test **Bağlantısı** düğmesini kullanarak giriş ve çıktılara bağlantıyı doğrulayın.

2.  Giriş verilerinizi inceleyin.

    1. Her giriş için [**Örnek Veri**](stream-analytics-sample-data-input.md) düğmesini kullanın. Giriş örnek verilerini indirin.
        
    1. Şemayı ve [veri türlerini](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)anlamak için örnek verileri inceleyin.
    
    1. Olayların gönderildiğinden emin olmak için [Olay Hub ölçümlerini](../event-hubs/event-hubs-metrics-azure-monitor.md) denetleyin. Olay Hub'ları ileti alıyorsa ileti ölçümleri sıfırdan büyük olmalıdır.

3.  Giriş önizlemesinde bir zaman aralığı seçtiğinizden emin olun. **Zaman aralığını seçin**ve sorgunuzu test etmeden önce örnek bir süre girin.

## <a name="malformed-input-events-causes-deserialization-errors"></a>Yanlış biçimlendirilmiş giriş olayları seri durumundan çıkarma hatalarına neden oluyor 

Deserialization sorunları, Akış Analizi iş giriş akışı hatalı biçimlendirilmiş iletiler içerdiğinde neden olur. Örneğin, hatalı biçimlendirilmiş bir ileti, bir JSON nesnesindeki eksik bir parantez veya ayraç veya zaman alanında yanlış bir zaman damgası biçiminden kaynaklanabilir. 
 
Bir Akış Analizi işi bir girişten yanlış biçimlendirilmiş bir ileti aldığında, iletiyi düşürür ve sizi bir uyarıyla bilgilendirir. Akış Analizi işgirişleri **döşemesinde** bir uyarı simgesi gösterilir. İş çalışma durumunda olduğu sürece aşağıdaki uyarı simgesi vardır:

![Azure Akışı Analizi girişleri döşeme](media/stream-analytics-malformed-events/stream-analytics-inputs-tile.png)

Hataya ve hataya neden olan iletinin (yük) ayrıntılarını görüntülemek için tanılama günlüklerini etkinleştirin. Deserialization hataları oluşabilir neden birden çok nedeni vardır. Belirli deserialization hataları ile ilgili daha fazla bilgi için [Giriş veri hataları](data-errors.md#input-data-errors)bakın. Tanılama günlükleri etkinleştirilmezse, Azure portalında kısa bir bildirim kullanılabilir.

![Giriş ayrıntıları uyarı bildirimi](media/stream-analytics-malformed-events/warning-message-with-offset.png)

İleti yükünün 32 KB'den büyük olduğu veya ikili biçimde olduğu durumlarda, [GitHub örnekleri deposunda](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH)bulunan CheckMalformedEvents.cs kodunu çalıştırın. Bu kod, bölüm kimliğini, mahsup'u okur ve bu ofsette bulunan verileri yazdırır. 

## <a name="job-exceeds-maximum-event-hub-receivers"></a>İş maksimum Olay Hub alıcılarını aşıyor

Olay Hub'larını kullanmak için en iyi yöntem, iş ölçeklenebilirliği için birden çok tüketici grubu kullanmaktır. Belirli bir giriş için Stream Analytics işinde okuyucu sayısı, tek bir tüketici grubundaki okuyucu sayısını etkiler. Alıcıların kesin sayısı, ölçeklendirme topolojisi mantığı için dahili uygulama ayrıntılarına dayanır ve dışarıdan açıklanmaz. Bir iş başlatıldığında veya iş yükseltmeleri sırasında okuyucu sayısı değişebilir.

Alıcı sayısı üst sınırı aşıldığında gösterilen hata: 

`The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Bir iş yükseltmesi sırasında okuyucu sayısı değiştiğinde, denetim günlüklerine geçici uyarılar yazılır. Akış Analizi işleri bu geçici sorunlardan otomatik olarak kurtulur.

### <a name="add-a-consumer-group-in-event-hubs"></a>Etkinlik Hub'larına tüketici grubu ekleme

Olay Hub'ları örneğinize yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın.

2. Etkinlik Hub'ınızı bulun.

3. **Varlıklar** başlığı altında **Olay Hub'larını** seçin.

4. Etkinlik Hub'ını ada göre seçin.

5. Etkinlik **Hub'ları Örneği** sayfasında, **Varlıklar** başlığı altında **Tüketici gruplarını**seçin. Ad $Default olan **bir** tüketici grubu listelenir.

6. Yeni bir tüketici grubu eklemek için **+ Tüketici Grubu'ni** seçin. 

   ![Etkinlik Hub'larına tüketici grubu ekleme](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Olay Merkezi'ni işaret etmek için Akış Analizi işinde girdi oluşturduğunuzda, oradaki tüketici grubunu belirlediğinizi. **$Default** belirtilmediğinde kullanılır. Yeni bir tüketici grubu oluşturduktan sonra, Stream Analytics işinde Olay Hub girdisini düzenle ve yeni tüketici grubunun adını belirtin.

## <a name="readers-per-partition-exceeds-event-hubs-limit"></a>Bölüm başına okuyucular Olay Hub'ları sınırını aşıyor

Akış sorgusu sözdiziminiz aynı giriş Olay Hub kaynağına birden çok kez başvuruyorsa, iş altyapısı aynı tüketici grubundan sorgu başına birden çok okuyucu kullanabilir. Aynı tüketici grubuna çok fazla başvuru olduğunda, iş beş sınırını aşabilir ve bir hata atabilir. Bu durumlarda, aşağıdaki bölümde açıklanan çözümü kullanarak birden çok tüketici grubu arasında birden çok giriş kullanarak daha fazla bölebilirsiniz. 

Bölüm başına okuyucu sayısının Beş Olay Hub'ı sınırını aştığı senaryolar şunlardır:

* Birden çok SELECT bildirimi: **Aynı** olay merkezi girişine başvuran birden çok SELECT deyimi kullanırsanız, her SELECT deyimi yeni bir alıcı oluşturulmasına neden olur.

* BİrLİk: BİrLİk kullandığınızda, **aynı** olay merkezi ve tüketici grubuna atıfta bulunan birden çok girişi olması mümkündür.

* SELF JOIN: SELF JOIN işlemi kullandığınızda, **aynı** olay hub'ına birden çok kez başvurmak mümkündür.

Aşağıdaki en iyi uygulamalar, bölüm başına okuyucu sayısının Beş Olay Hub'ı sınırını aştığı senaryoları azaltmaya yardımcı olabilir.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>WITH yan tümcesi kullanarak sorgunuzu birden çok adıma bölme

WITH yan tümcesi, sorgudaki BIR FROM yan tümcesi ile başvurulabilecek geçici bir adlandırılmış sonuç kümesi belirtir. TEK bir SELECT deyiminin yürütme kapsamında WITH yan tümcesini tanımlarsınız.

Örneğin, bu sorgu yerine:

```SQL
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Bu sorguyı kullanın:

```SQL
WITH data AS (
   SELECT * FROM inputEventHub
)

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Girdilerin farklı tüketici gruplarına bağlanmasını sağlamak

Üç veya daha fazla girdinin aynı Olay Hub'ları tüketici grubuna bağlı olduğu sorgular için ayrı tüketici grupları oluşturun. Bu, ek Akış Analizi girdileri oluşturulmasını gerektirir.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
