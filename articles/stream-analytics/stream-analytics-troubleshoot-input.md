---
title: Azure Akış Analizi için Sorun Giderme Girişleri
description: Bu makalede, Azure Akış Analizi işlerinde giriş bağlantılarınızı giderme teknikleri açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: dac3037f82c38980c9ac16685aa7fddac68a2e7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720308"
---
# <a name="troubleshoot-input-connections"></a>Giriş bağlantısı sorunlarını giderme

Bu sayfada, giriş bağlantılarıyla ilgili sık karşılaşılan sorunlar ve bunları nasıl giderecekleri açıklanmaktadır.

## <a name="input-events-not-received-by-job"></a>İş tarafından alınmayan giriş olayları 
1.  Bağlantınızı test edin. Her giriş ve çıkış için Test **Bağlantısı** düğmesini kullanarak giriş ve çıktılara bağlantıyı doğrulayın.

2.  Giriş verilerinizi inceleyin.

    1. Giriş verilerinin Olay Hub'ına aktığını doğrulamak için Azure Olay Hub'ına bağlanmak için [Service Bus Explorer'ı](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) kullanın (Olay Hub girişi kullanılıyorsa).
        
    1. Her giriş için [**Örnek Veri**](stream-analytics-sample-data-input.md) düğmesini kullanın. Giriş örnek verilerini indirin.
        
    1. Verilerin şeklini anlamak için örnek verileri inceleyin, yani şema ve [veri türleri.](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)

3.  Giriş önizlemesinde bir zaman aralığı seçtiğinizden emin olun. **Zaman aralığını seçin**ve sorgunuzu test etmeden önce örnek bir süre girin.

## <a name="malformed-input-events-causes-deserialization-errors"></a>Yanlış biçimlendirilmiş giriş olayları seri durumundan çıkarma hatalarına neden oluyor 
Deserialization sorunları, Akış Analizi iş giriş akışı hatalı biçimlendirilmiş iletiler içerdiğinde neden olur. Örneğin, hatalı biçimlendirilmiş bir ileti, bir JSON nesnesindeki eksik bir parantez veya ayraç veya zaman alanında yanlış zaman damgası biçiminden kaynaklanabilir. 
 
Bir Akış Analizi işi bir girişten yanlış biçimlendirilmiş bir ileti aldığında, iletiyi düşürür ve sizi bir uyarıyla bilgilendirir. Akış Analizi işgirişleri **döşemesinde** bir uyarı simgesi gösterilir. Bu uyarı işareti, iş çalışma durumunda olduğu sürece vardır:

![Azure Akışı Analizi girişleri döşeme](media/stream-analytics-malformed-events/stream-analytics-inputs-tile.png)

Uyarının ayrıntılarını görüntülemek için tanılama günlüklerini etkinleştirin. Hatalı bilgilendirilmiş giriş olayları için yürütme günlükleri aşağıdaki gibi görünen iletiiçeren bir giriş içerir: 
```
Could not deserialize the input event(s) from resource <blob URI> as json.
```

### <a name="what-caused-the-deserialization-error"></a>Deserialization hatasına ne neden oldu?
Deserialization hatasına neyin neden olduğunu net bir şekilde anlamak için giriş olaylarını ayrıntılı olarak çözümlemek için aşağıdaki adımları atabilirsiniz. Daha sonra, bu soruna tekrar çarpmanızı önlemek için doğru biçimde olaylar oluşturmak için olay kaynağını düzeltebilirsiniz.

1. Giriş döşemesine gidin ve sorunlar listesini görmek için uyarı sembollerini tıklatın.

2. Giriş ayrıntıları döşemesi, her sorunla ilgili ayrıntıları içeren bir uyarı listesi görüntüler. Aşağıdaki örnek uyarı iletisi, hatalı biçimlendirilmiş JSON verilerinin bulunduğu bölüm, ofset ve sıra numaralarını içerir. 

   ![Ofsetli Akış Analizi uyarı iletisi](media/stream-analytics-malformed-events/warning-message-with-offset.png)
   
3. Yanlış biçimde JSON verilerini bulmak [için, GitHub örnekleri deposunda](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH)bulunan CheckMalformedEvents.cs kodunu çalıştırın. Bu kod, bölüm kimliğini, mahsup'u okur ve bu ofsette bulunan verileri yazdırır. 

4. Verileri okuduktan sonra, serileştirme biçimini analiz edebilir ve düzeltebilirsiniz.

5. [Ayrıca, Servis Veri Merkezi Gezgini ile bir IoT Hub'ından etkinlikleri de okuyabilirsiniz.](https://code.msdn.microsoft.com/How-to-read-events-from-an-1641eb1b)

## <a name="job-exceeds-maximum-event-hub-receivers"></a>İş maksimum Olay Hub Alıcılarını aşıyor
Olay Hub'larını kullanmak için en iyi yöntem, iş ölçeklenebilirliğini sağlamak için birden çok tüketici grubu kullanmaktır. Belirli bir giriş için Stream Analytics işinde okuyucu sayısı, tek bir tüketici grubundaki okuyucu sayısını etkiler. Alıcıların kesin sayısı, ölçeklendirme topolojisi mantığı için dahili uygulama ayrıntılarına dayanır ve dışarıdan açıklanmaz. Bir iş başlatıldığında veya iş yükseltmeleri sırasında okuyucu sayısı değişebilir.

Alıcı sayısı üst sınırı aşıldığında gösterilen hata: `The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Bir iş yükseltmesi sırasında okuyucu sayısı değiştiğinde, denetim günlüklerine geçici uyarılar yazılır. Akış Analizi işleri bu geçici sorunlardan otomatik olarak kurtulur.

### <a name="add-a-consumer-group-in-event-hubs"></a>Etkinlik Hub'larına tüketici grubu ekleme
Olay Hub'ları örneğinize yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın.

2. Etkinlik Hub'larınızı bulun.

3. **Varlıklar** başlığı altında **Olay Hub'larını** seçin.

4. Etkinlik Hub'ını ada göre seçin.

5. Etkinlik **Hub'ları Örneği** sayfasında, **Varlıklar** başlığı altında **Tüketici gruplarını**seçin. Ad $Default olan **bir** tüketici grubu listelenir.

6. Yeni bir tüketici grubu eklemek için **+ Tüketici Grubu'ni** seçin. 

   ![Etkinlik Hub'larına tüketici grubu ekleme](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Olay Merkezi'ni işaret etmek için Akış Analizi işinde girdi oluşturduğunuzda, oradaki tüketici grubunu belirlediğinizi. $Default hiçbiri belirtilmediğinde kullanılır. Yeni bir tüketici grubu oluşturduktan sonra, Stream Analytics işinde Olay Hub girdisini düzenle ve yeni tüketici grubunun adını belirtin.


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
