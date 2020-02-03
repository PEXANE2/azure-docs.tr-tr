---
title: Azure Stream Analytics için sorun giderme girişler
description: Bu makalede, giriş bağlantılarınızı Azure Stream Analytics işlerinde sorun giderme teknikleri açıklar.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: dac3037f82c38980c9ac16685aa7fddac68a2e7b
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720308"
---
# <a name="troubleshoot-input-connections"></a>Giriş bağlantısı sorunlarını giderme

Bu sayfa, giriş bağlantıları ve bunları nasıl giderebileceğinizden ile ilgili yaygın sorunları açıklar.

## <a name="input-events-not-received-by-job"></a>İş tarafından alınmayan giriş olayları 
1.  Bağlantınızı test edin. Her giriş ve çıkış için **Bağlantıyı Sına** düğmesini kullanarak giriş ve çıkışlarla bağlantıyı doğrulayın.

2.  Girişinizi inceleyin.

    1. Giriş verilerinin Olay Hub 'ına akmasını doğrulamak için, Azure Olay Hub 'ına bağlanmak üzere [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) kullanın (Olay Hub 'ı girişi kullanılıyorsa).
        
    1. Her giriş için [**örnek veri**](stream-analytics-sample-data-input.md) düğmesini kullanın. Giriş örnek verilerini indirin.
        
    1. Verilerin şeklini anlamak için örnek verileri inceleyin; Yani, şema ve [veri türleri](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics).

3.  Giriş önizlemede bir zaman aralığı seçtiğinizden emin olun. **Zaman aralığını Seç**' i seçin ve ardından sorgunuzu test etmeden önce bir örnek süre girin.

## <a name="malformed-input-events-causes-deserialization-errors"></a>Yanlış biçimlendirilmiş giriş olayları seri durumundan çıkarma hatalarına neden oluyor 
Stream Analytics işinizin Giriş akışı yanlış biçimlendirilmiş iletiler içerdiğinde seri durumundan çıkarma sorunları nedeniyle oluşur. Örneğin, hatalı bir ileti parantezin eksik olması veya bir JSON nesnesinde bir küme ayracı veya hatalı zaman damgası biçimi zaman alanı kaynaklanabilir. 
 
Bir Stream Analytics işi girdi hatalı bir ileti aldığında, iletiyi bırakır ve bir uyarı ile bildirir. Stream Analytics işinizin **giriş** kutucuğunda bir uyarı simgesi gösterilir. İşi çalışır durumda olduğu sürece, bu uyarı işareti vardır:

![Azure Stream Analytics girişler kutucuğunda](media/stream-analytics-malformed-events/stream-analytics-inputs-tile.png)

Uyarı ayrıntılarını görüntülemek tanılama günlüklerini etkinleştirin. Giriş yanlış biçimlendirilmiş olaylar için yürütme günlüklerini şuna benzer bir ileti ile bir giriş içerir: 
```
Could not deserialize the input event(s) from resource <blob URI> as json.
```

### <a name="what-caused-the-deserialization-error"></a>Seri durumundan çıkarma hatası neyin
Giriş olayları seri durumdan çıkarma hataya açık anlamak için ayrıntılı analiz etmek için aşağıdaki adımları uygulayabilirsiniz. Ardından, bu sorunu yeniden ulaşmaktan önlemek için doğru biçimde olaylar oluşturmak için olay kaynağı düzeltebilirsiniz.

1. Giriş kutucuğuna gidin ve sorunları listesini görmek için uyarı simgeleri tıklatın.

2. Giriş Ayrıntıları kutucuğu, her bir sorunun ayrıntılarını içeren uyarıların bir listesini görüntüler. Aşağıdaki örnek uyarı iletisi bölümü, uzaklığı ve seri numaraları içeren JSON verileri hatalı biçimlendirilmiş olduğu. 

   ![Uzaklığa sahip Stream Analytics uyarı iletisi](media/stream-analytics-malformed-events/warning-message-with-offset.png)
   
3. Hatalı biçimli JSON verilerini bulmak için [GitHub örnekleri deposunda](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH)bulunan CheckMalformedEvents.cs kodunu çalıştırın. Bu kod okuma bölüm kimliği, uzaklığı ve bu uzaklık içinde bulunan veri yazdırır. 

4. Verileri okuduktan sonra, serileştirme biçimini analiz edebilir ve düzeltebilirsiniz.

5. Ayrıca [, Service Bus Gezgini ile IoT Hub olayları okuyabilirsiniz](https://code.msdn.microsoft.com/How-to-read-events-from-an-1641eb1b).

## <a name="job-exceeds-maximum-event-hub-receivers"></a>En fazla olay hub'ı alıcıları iş aşıyor
Event Hubs'ı kullanmaya yönelik en iyi yöntem, işi ölçeklenebilirlik sağlamak için birden fazla tüketici grupları kullanmaktır. Belirli bir giriş için Stream Analytics işinde okuyucu sayısını tek bir tüketici grubundaki okuyucu sayısını etkiler. Alıcılar kesin sayısını iç uygulama ayrıntıları genişleme topolojisi mantığı temel alır ve harici olarak gösterilmez. Bir iş başlatıldığında veya iş yükseltmeleri sırasında okuyucu sayısını değiştirebilirsiniz.

Alıcı sayısı üst sınırı aşıldığında gösterilen hata: `The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Okuyucu sayısıyla proje yükseltme sırasında değiştiğinde, geçici uyarıları denetim günlükleri yazılır. Stream Analytics işleri, bu geçici sorunları otomatik olarak kurtarma.

### <a name="add-a-consumer-group-in-event-hubs"></a>Event hubs'ı tüketici grubu Ekle
Event Hubs Örneğinizde yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın.

2. Event hubs'ı bulun.

3. **Varlıklar** başlığı altında **Event Hubs** ' yi seçin.

4. Ada göre olay hub'ı seçin.

5. **Event Hubs örneği** sayfasında, **varlıklar** başlığı altında **tüketici grupları**' nı seçin. **$Default** adlı bir tüketici grubu listelenir.

6. Yeni bir tüketici grubu eklemek için **+ Tüketici grubu** ' nu seçin. 

   ![Event hubs'ı tüketici grubu Ekle](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Olay Hub'ına işaret edecek şekilde Stream Analytics işinde giriş oluşturduğunuzda, tüketici grubu belirtildi. Belirtilmediğinde $Default kullanılır. Yeni bir tüketici grubu oluşturduktan sonra Stream Analytics işi olay hub'ı giriş düzenleyin ve yeni tüketici grubu adını belirtin.


## <a name="readers-per-partition-exceeds-event-hubs-limit"></a>Bölüm başına okuyucular, Event Hubs sınırı aşıyor

Akış sorgu sözdiziminizin birden çok kez aynı giriş olay hub'ı kaynağa başvuruyorsa, proje altyapısı sorgudan aynı tüketici grubu başına birden fazla okuyucuyu kapsayacak kullanabilirsiniz. Aynı tüketici grubu için çok fazla başvuru olduğunda, iş beş sınırını aşabilir ve bir hata oluşturulur. Bu durumlarda, aşağıdaki bölümde açıklanan çözümünü kullanarak birden fazla tüketici grupları arasında birden fazla giriş kullanarak daha da ayırabilirsiniz. 

Bölüm başına okuyucu sayısıyla beş Event Hubs sınırını aşıyor senaryolar aşağıdakileri içerir:

* Çoklu SELECT deyimleri: **aynı** Olay Hub 'ına başvuran bırden çok select deyimi kullanıyorsanız, her SELECT deyimi yeni bir alıcının oluşturulmasına neden olur.
* BIRLEŞIM: bir BIRLEŞIM kullandığınızda, **aynı** Olay Hub 'ına ve tüketici grubuna başvuran birden çok giriş olması mümkündür.
* Kendi kendine JOIN: bir kendi kendine JOIN işlemi kullandığınızda, **aynı** Olay Hub 'ına birden çok kez başvurmak mümkündür.

Aşağıdaki en iyi, bölüm başına okuyucu sayısıyla beş Event Hubs sınırını aşıyor senaryoları azaltılmasına yardımcı olur.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>WITH yan tümcesini kullanarak sorgunuzu içine birden çok adımı Böl

WITH yan tümcesiyle sorgusunda FROM yan tümcesi tarafından başvurulan bir geçici adlandırılmış sonuç kümesi belirtir. WITH yan tümcesi tek bir SELECT deyimi yürütme kapsamında tanımlarsınız.

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

Bu sorguyu kullanın:

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

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Girişler için farklı bir tüketici grubu bağlama emin olun.

Aynı Event hubs'ı tüketici grubu için üç veya daha fazla giriş bağlı sorgular için ayrı bir tüketici grubu oluşturun. Bu ek Stream Analytics'e giriş oluşturulmasını gerektirir.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için [Azure Stream Analytics Forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
