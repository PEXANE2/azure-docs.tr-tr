---
title: Büyük veri Scala örnekleri için bilişsel hizmetler
description: Büyük verilere ilişkin MMLSpark işlem hattınızı çalıştırmak için bilişsel hizmetler 'i Azure Databricks kullanın.
services: cognitive-services
author: mhamilton723
manager: nitinme
ms.service: cognitive-services
ms.topic: sample
ms.date: 07/06/2020
ms.author: marhamil
ms.openlocfilehash: 4546ef03c82f19d188a71a86f6964ca87c0f834e
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90524972"
---
# <a name="quick-examples"></a>Hızlı örnekler

Aşağıdaki kod parçacıkları çalıştırılmaya hazırlanıyor ve Spark üzerinde bilişsel hizmetler 'i kullanmaya başlamanıza yardımcı olur. Aşağıdaki örnekler Scala içinde verilmiştir.

Örnekler şu bilişsel Hizmetleri kullanır:

- Metin Analizi-bir tümce kümesinin yaklaşımını (veya ruh) alın.
- Görüntü İşleme-bir görüntü kümesiyle ilişkili etiketleri (tek sözcüklük açıklamaları) alın.
- Bing Resim Arama-Web 'de doğal dil sorgusuyla ilgili görüntüleri arayın.
- Metin tabanlı döküm dosyalarını ayıklamak için konuşmayı metne dönüştürme ses dosyaları.
- Anomali algılayıcısı-bir zaman serisi verileri içindeki aykırlıkları algılayın.

## <a name="prerequisites"></a>Önkoşullar

1. Azure Databricks ve [bilişsel](getting-started.md) hizmetler ortamınızı ayarlamaya Başlarken bölümündeki adımları izleyin. Bu öğretici, MMLSpark 'ın nasıl yükleneceğini ve Databricks 'te Spark kümenizin nasıl oluşturulacağını içerir.
1. Azure Databricks yeni bir not defteri oluşturduktan sonra, aşağıdaki **paylaşılan kodu** kopyalayın ve Not defterinize yeni bir hücreye yapıştırın.
1. Aşağıda bir hizmet örneği seçin ve kopyalayın ve Not defterinize ikinci bir yeni hücreye yapıştırın.
1. Hizmet aboneliği anahtar yertutucuları kendi anahtarınızla değiştirin.
1. Hücrenin sağ üst köşesindeki Çalıştır düğmesini (üçgen simgesi) seçin ve ardından **hücreyi Çalıştır**' ı seçin.
1. Sonuçları hücrenin altındaki bir tabloda görüntüleyin.

## <a name="shared-code"></a>Paylaşılan kod
Başlamak için bu kodu projenize ekleyin:

```scala
import com.microsoft.ml.spark.cognitive._
import spark.implicits._ 

val serviceKey = "ADD-YOUR-SUBSCRIPTION-KEY"
val location = "eastus"
```

## <a name="text-analytics"></a>Metin Analizi

[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) hizmeti, metinden akıllı öngörüleri ayıklamak için çeşitli algoritmalar sağlar. Örneğin, verilen giriş metninin yaklaşımını bulabiliriz. Hizmet arasında bir puan döndürür `0.0` ve `1.0` düşük puanlar negatif yaklaşım ve yüksek puan pozitif yaklaşımı gösterir.  Aşağıdaki örnek üç basit cümle kullanır ve her biri için yaklaşım Puanını döndürür.

```scala
import org.apache.spark.sql.functions.col

val df = Seq(
  ("I am so happy today, its sunny!", "en-US"),
  ("I am frustrated by this rush hour traffic", "en-US"),
  ("The cognitive services on spark aint bad", "en-US")
).toDF("text", "language")

val sentiment = new TextSentiment()
    .setTextCol("text")
    .setLocation(location)
    .setSubscriptionKey(serviceKey)
    .setOutputCol("sentiment")
    .setErrorCol("error")
    .setLanguageCol("language")

display(sentiment.transform(df).select(col("text"), col("sentiment")(0).getItem("score").alias("sentiment")))
```

### <a name="expected-result"></a>Beklenen sonuç

| metin                                      | yaklaşım                                             |
|:------------------------------------------|:------------------------------------------------------|
| Bugün bugün kutlu olsun!           | 0.9789592027664185                                    |
| Bu aceleniz saat trafiği tarafından hayal dışındayım | 0.023795604705810547                                  |
| Spark aint hatalı üzerinde bilişsel hizmetler  | 0.8888956308364868                                    |

## <a name="computer-vision"></a>Görüntü İşleme

[Görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) , yüzeyleri, nesneleri ve doğal dil açıklamaları gibi yapıyı belirlemek için görüntüleri analiz eder.
Bu örnekte, bir resim listesini etiketliyoruz. Etiketler, görüntüde tanınabilir nesneler, kişiler, manzara ve eylemler gibi öğelerin bir sözcüklerdir.

```scala
// Create a dataframe with the image URLs
val df = Seq(
    ("https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/objects.jpg"),
    ("https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/dog.jpg"),
    ("https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/house.jpg")
).toDF("image")

// Run the Computer Vision service. Analyze Image extracts infortmation from/about the images.
val analysis = new AnalyzeImage()
    .setLocation(location)
    .setSubscriptionKey(serviceKey)
    .setVisualFeatures(Seq("Categories","Color","Description","Faces","Objects","Tags"))
    .setOutputCol("results")
    .setImageUrlCol("image")
    .setErrorCol("error"))

// Show the results of what you wanted to pull out of the images.
display(analysis.transform(df).select(col("image"), col("results").getItem("tags").getItem("name")).alias("results")))

// Uncomment for full results with all visual feature requests
//display(analysis.transform(df).select(col("image"), col("results")))
``` 

### <a name="expected-result"></a>Beklenen sonuç

| image | etiketler |
|:------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------|
| https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/objects.jpg | [' ' ' kişi ' ' adam ' ' Başak ' ' adlı taraf ' ' adlı taraf ' ', ' ', ' ' havayolu ' '-' ' uçak ' ' Havaalanı ' '-' ' Kara ' '
| https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/dog.jpg | [' köpek ' ' ' ', ' ', ' ' tezgahtır ' ', ' ' öne çıkış ' ' adlı küçük ' ' kahverengi ' ' bina ' ', ' ' öne ' '                
| https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/house.jpg | [' ınstaya ' ' evsi ' ' House ' ' derleme ' ' eski ' ' Ana ' ' ön ' ' küçük ' '-' ' ' i ' ' ' blok ' ' tezgahtır ' ' Street ' ' ' ' ' ' ' '-' ' ' ' açık ' ' ' ' ' ' ' ' ' ' alanı ' ' işareti ' ' 

## <a name="bing-image-search"></a>Bing Resim Arama

[Bing resim arama](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview) , kullanıcının doğal dil sorgusuyla ilgili görüntüleri almak için Web 'de arama yapar. Bu örnekte, tırnak içine alınmış resimleri gösteren bir metin sorgusu kullanıyoruz. Sorgumuzla ilgili fotoğraflar içeren resim URL 'Lerinin bir listesini döndürür.


```scala
import org.apache.spark.ml.Pipeline

// Number of images Bing will return per query
val imgsPerBatch = 10

// A list of offsets, used to page into the search results
val df = (0 until 100).map(o => Tuple1(o*imgsPerBatch)).toSeq.toDF("offset")

// Run the Bing Image Search service with our text query
val bingSearch = new BingImageSearch()
    .setSubscriptionKey(bingSearchKey)
    .setOffsetCol("offset")
    .setQuery("Martin Luther King Jr. quotes")
    .setCount(imgsPerBatch)
    .setOutputCol("images")

// Transformer that extracts and flattens the richly structured output of Bing Image Search into a simple URL column
val getUrls = BingImageSearch.getUrlTransformer("images", "url")

// This displays the full results returned, uncomment to use
// display(bingSearch.transform(bingParameters))

// Since we have two services, they are put into a pipeline
val pipeline = new Pipeline().setStages(Array(bingSearch, getUrls))

// Show the results of your search: image URLs
display(pipeline.fit(df).transform(df))
```

### <a name="expected-result"></a>Beklenen sonuç

| url |
|:-------------------------------------------------------------------------------------------------------------------|
| https://iheartintelligence.com/wp-content/uploads/2019/01/powerful-quotes-martin-luther-king-jr.jpg      |
| http://everydaypowerblog.com/wp-content/uploads/2014/01/Martin-Luther-King-Jr.-Quotes-16.jpg             |
| http://www.sofreshandsogreen.com/wp-content/uploads/2012/01/martin-luther-king-jr-quote-sofreshandsogreendotcom.jpg |
| https://everydaypowerblog.com/wp-content/uploads/2014/01/Martin-Luther-King-Jr.-Quotes-18.jpg            |
| https://tsal-eszuskq0bptlfh8awbb.stackpathdns.com/wp-content/uploads/2018/01/MartinLutherKingQuotes.jpg  |

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

[Konuşmadan metne](https://docs.microsoft.com/azure/cognitive-services/speech-service/index-speech-to-text) hizmeti, okunan ses akışlarını veya dosyalarını metne dönüştürür. Bu örnekte, iki ses dosyası ekleyeceğiz. İlk dosyanın anlaşılması kolaydır ve ikincisi daha zorlayıcı bir işlemdir.

```scala
import org.apache.spark.sql.functions.col

// Create a dataframe with audio URLs, tied to the column called "url"
val df = Seq(("https://mmlspark.blob.core.windows.net/datasets/Speech/audio2.wav"),
             ("https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3")).toDF("url")

// Run the Speech-to-text service to translate the audio into text
val speechToText = new SpeechToTextSDK()
    .setSubscriptionKey(serviceKey)
    .setLocation("eastus")
    .setOutputCol("text")
    .setAudioDataCol("url")
    .setLanguage("en-US")
    .setProfanity("Masked")

// Show the results of the translation
display(speechToText.transform(df).select(col("url"), col("text").getItem("DisplayText")))
```

### <a name="expected-result"></a>Beklenen sonuç

| url | Görünüm |
|:------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio2.wav | Özel konuşma, bir modelin tanınma kalitesini görsel olarak incelemenize olanak sağlayan araçlar sağlar. Bu, ses verilerini özel konuşma portalından karşılık gelen tanıma sonucuyla karşılaştırır. Karşıya yüklenen sesi kayıttan çalarken, belirtilen tanıma sonucunun doğru olup olmadığını belirleyebilirsiniz. Bu araç, herhangi bir ses verisi almak zorunda kalmadan Microsoft 'un temel konuşma konuşmasının kalitesini metin modeline veya eğitilen özel bir modele hızlıca incelemenizi sağlar. |
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3 | Bir genttaman sır ekleyin görsel denetimi.    |
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3 | Beni duydum. |
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3 | Daha fazla dinediğim radyo için sorunsuz bir küme istiyorum. |

## <a name="anomaly-detector"></a>Anomali Algılayıcısı

[Anomali algılayıcısı](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/) , zaman serisi verilerinizde ırdüzenleyeaykırıluluğunu saptamak için idealdir. Bu örnekte, tüm zaman serisinde bozukluklar bulmak için hizmeti kullanırız.

```scala
import org.apache.spark.sql.functions.{col, lit}

val anomalyKey = "84a2c303cc7e49f6a44d692c27fb9967"

val df = Seq(
    ("1972-01-01T00:00:00Z", 826.0),
    ("1972-02-01T00:00:00Z", 799.0),
    ("1972-03-01T00:00:00Z", 890.0),
    ("1972-04-01T00:00:00Z", 900.0),
    ("1972-05-01T00:00:00Z", 766.0),
    ("1972-06-01T00:00:00Z", 805.0),
    ("1972-07-01T00:00:00Z", 821.0),
    ("1972-08-01T00:00:00Z", 20000.0),
    ("1972-09-01T00:00:00Z", 883.0),
    ("1972-10-01T00:00:00Z", 898.0),
    ("1972-11-01T00:00:00Z", 957.0),
    ("1972-12-01T00:00:00Z", 924.0),
    ("1973-01-01T00:00:00Z", 881.0),
    ("1973-02-01T00:00:00Z", 837.0),
    ("1973-03-01T00:00:00Z", 9000.0)
  ).toDF("timestamp", "value").withColumn("group", lit("series1"))

// Run the Anomaly Detector service to look for irregular data
val anamolyDetector = new SimpleDetectAnomalies()
    .setSubscriptionKey(anomalyKey)
    .setLocation("eastus")
    .setTimestampCol("timestamp")
    .setValueCol("value")
    .setOutputCol("anomalies")
    .setGroupbyCol("group")
    .setGranularity("monthly")

// Show the full results of the analysis with the anomalies marked as "True"
display(anamolyDetector.transform(df).select("timestamp", "value", "anomalies.isAnomaly"))
```

### <a name="expected-result"></a>Beklenen sonuç

| timestamp            |   değer | isAnomaly   |
|:---------------------|--------:|:------------|
| 1972-01-01T00:00:00Z |     826 | Yanlış       |
| 1972-02-01T00:00:00Z |     799 | Yanlış       |
| 1972-03-01T00:00:00Z |     890 | Yanlış       |
| 1972-04-01T00:00:00Z |     900 | Yanlış       |
| 1972-05-01T00:00:00Z |     766 | Yanlış       |
| 1972-06-01T00:00:00Z |     805 | Yanlış       |
| 1972-07-01T00:00:00Z |     821 | Yanlış       |
| 1972-08-01T00:00:00Z |   20000 | Doğru        |
| 1972-09-01T00:00:00Z |     883 | Yanlış       |
| 1972-10-01T00:00:00Z |     898 | Yanlış       |
| 1972-11-01T00:00:00Z |     957 | Yanlış       |
| 1972-12-01T00:00:00Z |     924 | Yanlış       |
| 1973-01-01T00:00:00Z |     881 | Yanlış       |
| 1973-02-01T00:00:00Z |     837 | Yanlış       |
| 1973-03-01T00:00:00Z |    9000 | Doğru        |
