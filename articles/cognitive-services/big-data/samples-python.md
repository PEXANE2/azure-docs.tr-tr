---
title: Büyük veri için bilişsel hizmetler-Python örnekleri
description: Büyük veriler için MMLSpark işlem hattınızı çalıştırmak üzere Azure Databricks için Python 'da bilişsel hizmetler örneklerini deneyin.
services: cognitive-services
author: mhamilton723
manager: nitinme
ms.service: cognitive-services
ms.topic: sample
ms.date: 07/06/2020
ms.author: marhamil
ms.openlocfilehash: caf492c2cd3940fd7f37e2a4462c8376a127f393
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86189828"
---
# <a name="python-samples-for-cognitive-services-for-big-data"></a>Büyük veriler için bilişsel hizmetler için Python örnekleri

Aşağıdaki kod parçacıkları çalıştırılmaya hazırlardır ve Python ile Spark üzerinde bilişsel hizmetler 'i kullanmaya başlamanıza yardımcı olur.

Bu makaledeki örneklerde bu bilişsel hizmetler kullanılır:

- Metin Analizi-bir tümce kümesinin yaklaşımını (veya ruh) alın.
- Görüntü İşleme-bir görüntü kümesiyle ilişkili etiketleri (tek sözcüklük açıklamaları) alın.
- Bing Resim Arama-Web 'de doğal dil sorgusuyla ilgili görüntüleri arayın.
- Metin tabanlı döküm dosyalarını ayıklamak için konuşmayı metne dönüştürme ses dosyaları.
- Anomali algılayıcısı-bir zaman serisi verileri içindeki aykırlıkları algılayın.

## <a name="prerequisites"></a>Ön koşullar

1. Azure Databricks ve [bilişsel](getting-started.md) hizmetler ortamınızı ayarlamaya Başlarken bölümündeki adımları izleyin. Bu öğreticide, MMLSpark 'ın nasıl yükleneceği ve Databricks 'te Spark kümenizi nasıl oluşturacağınız gösterilmektedir.
1. Azure Databricks yeni bir not defteri oluşturduktan sonra, aşağıdaki **paylaşılan kodu** kopyalayın ve Not defterinize yeni bir hücreye yapıştırın.
1. Aşağıda bir hizmet örneği seçin ve kopyalayın ve Not defterinize ikinci bir yeni hücreye yapıştırın.
1. Hizmet aboneliği anahtar yertutucuları kendi anahtarınızla değiştirin.
1. Hücrenin sağ üst köşesindeki Çalıştır düğmesini (üçgen simgesi) seçin ve ardından **hücreyi Çalıştır**' ı seçin.
1. Sonuçları hücrenin altındaki bir tabloda görüntüleyin.

## <a name="shared-code"></a>Paylaşılan kod

Başlamak için projeye bu kodu eklemesi gerekir:

```python
from mmlspark.cognitive import *

# A general Cognitive Services key for Text Analytics and Computer Vision (or use separate keys that belong to each service)
service_key = "ADD_YOUR_SUBSCRIPION_KEY"
# A Bing Search v7 subscription key
bing_search_key = "ADD_YOUR_SUBSCRIPION_KEY"
# An Anomaly Dectector subscription key
anomaly_key = "ADD_YOUR_SUBSCRIPION_KEY"

# Validate the key
assert service_key != "ADD_YOUR_SUBSCRIPION_KEY"
```    

## <a name="text-analytics-sample"></a>Metin Analizi örneği

[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) hizmeti, metinden akıllı öngörüleri ayıklamak için çeşitli algoritmalar sağlar. Örneğin, verilen giriş metninin yaklaşımını bulabiliriz. Hizmet, 0,0 ile 1,0 arasında bir puan döndürür. bu noktada, düşük puanlar negatif yaklaşım ve yüksek puan pozitif yaklaşım olduğunu gösterir.  Bu örnek üç basit cümle kullanır ve her biri için yaklaşımı döndürür.

```python
from pyspark.sql.functions import col

# Create a dataframe that's tied to it's column names
df = spark.createDataFrame([
  ("I am so happy today, its sunny!", "en-US"),
  ("I am frustrated by this rush hour traffic", "en-US"),
  ("The cognitive services on spark aint bad", "en-US"),
], ["text", "language"])

# Run the Text Analytics service with options
sentiment = (TextSentiment()
    .setTextCol("text")
    .setLocation("eastus")
    .setSubscriptionKey(service_key)
    .setOutputCol("sentiment")
    .setErrorCol("error")
    .setLanguageCol("language"))

# Show the results of your text query in a table format
display(sentiment.transform(df).select("text", col("sentiment")[0].getItem("score").alias("sentiment")))
```

### <a name="expected-result"></a>Beklenen sonuç

| metin                                      | yaklaşım                                             |
|:------------------------------------------|:------------------------------------------------------|
| Bugün bugün kutlu olsun!           | 0.9789592027664185                                    |
| Bu aceleniz saat trafiği tarafından hayal dışındayım | 0.023795604705810547                                  |
| Spark aint hatalı üzerinde bilişsel hizmetler  | 0.8888956308364868                                    |

## <a name="computer-vision-sample"></a>Görüntü İşleme örneği

[Görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) , yüzeyleri, nesneleri ve doğal dil açıklamaları gibi yapıyı belirlemek için görüntüleri analiz eder. Bu örnekte, bir resim listesini etiketliyoruz. Etiketler, görüntüde tanınabilir nesneler, kişiler, manzara ve eylemler gibi öğelerin bir sözcüklerdir.

```python

# Create a dataframe with the image URLs
df = spark.createDataFrame([
        ("https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/objects.jpg", ),
        ("https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/dog.jpg", ),
        ("https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/house.jpg", )
    ], ["image", ])

# Run the Computer Vision service. Analyze Image extracts infortmation from/about the images.
analysis = (AnalyzeImage()
    .setLocation("eastus")
    .setSubscriptionKey(service_key)
    .setVisualFeatures(["Categories","Color","Description","Faces","Objects","Tags"])
    .setOutputCol("analysis_results")
    .setImageUrlCol("image")
    .setErrorCol("error"))

# Show the results of what you wanted to pull out of the images.
display(analysis.transform(df).select("image", "analysis_results.description.tags"))
```

### <a name="expected-result"></a>Beklenen sonuç

| image | etiketler |
|:------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------|
| https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/objects.jpg | [' ' ' kişi ' ' adam ' ' Başak ' ' adlı taraf ' ' adlı taraf ' ', ' ', ' ' havayolu ' '-' ' uçak ' ' Havaalanı ' '-' ' Kara ' '
| https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/dog.jpg | [' köpek ' ' ' ', ' ', ' ' tezgahtır ' ', ' ' öne çıkış ' ' adlı küçük ' ' kahverengi ' ' bina ' ', ' ' öne ' '                
| https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/house.jpg | [' ınstaya ' ' evsi ' ' House ' ' derleme ' ' eski ' ' Ana ' ' ön ' ' küçük ' '-' ' ' i ' ' ' blok ' ' tezgahtır ' ' Street ' ' ' ' ' ' ' '-' ' ' ' açık ' ' ' ' ' ' ' ' ' ' alanı ' ' işareti ' '


## <a name="bing-image-search-sample"></a>Bing Resim Arama örneği

[Bing resim arama](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview) , kullanıcının doğal dil sorgusuyla ilgili görüntüleri almak için Web 'de arama yapar. Bu örnekte, tırnak içine alınmış resimleri gösteren bir metin sorgusu kullanıyoruz. Sorgumuzla ilgili fotoğraflar içeren resim URL 'Lerinin bir listesini döndürür.

```python
from pyspark.ml import PipelineModel

# Number of images Bing will return per query
imgsPerBatch = 10
# A list of offsets, used to page into the search results
offsets = [(i*imgsPerBatch,) for i in range(100)]
# Since web content is our data, we create a dataframe with options on that data: offsets
bingParameters = spark.createDataFrame(offsets, ["offset"])

# Run the Bing Image Search service with our text query
bingSearch = (BingImageSearch()
    .setSubscriptionKey(bing_search_key)
    .setOffsetCol("offset")
    .setQuery("Martin Luther King Jr. quotes")
    .setCount(imgsPerBatch)
    .setOutputCol("images"))

# Transformer that extracts and flattens the richly structured output of Bing Image Search into a simple URL column
getUrls = BingImageSearch.getUrlTransformer("images", "url")

# This displays the full results returned, uncomment to use
# display(bingSearch.transform(bingParameters))

# Since we have two services, they are put into a pipeline
pipeline = PipelineModel(stages=[bingSearch, getUrls])

# Show the results of your search: image URLs
display(pipeline.transform(bingParameters))
```

### <a name="expected-result"></a>Beklenen sonuç

| url |
|:-------------------------------------------------------------------------------------------------------------------|
| https://iheartintelligence.com/wp-content/uploads/2019/01/powerful-quotes-martin-luther-king-jr.jpg      |
| http://everydaypowerblog.com/wp-content/uploads/2014/01/Martin-Luther-King-Jr.-Quotes-16.jpg             |
| http://www.sofreshandsogreen.com/wp-content/uploads/2012/01/martin-luther-king-jr-quote-sofreshandsogreendotcom.jpg |
| https://everydaypowerblog.com/wp-content/uploads/2014/01/Martin-Luther-King-Jr.-Quotes-18.jpg            |
| https://tsal-eszuskq0bptlfh8awbb.stackpathdns.com/wp-content/uploads/2018/01/MartinLutherKingQuotes.jpg  |


## <a name="speech-to-text-sample"></a>Konuşmadan metne örnek
[Konuşmadan metne](https://docs.microsoft.com/azure/cognitive-services/speech-service/index-speech-to-text) hizmeti, okunan ses akışlarını veya dosyalarını metne dönüştürür. Bu örnekte, iki ses dosyası ekleyeceğiz. İlk dosyanın anlaşılması kolaydır ve ikincisi daha zorlayıcı bir işlemdir.

```python

# Create a dataframe with our audio URLs, tied to the column called "url"
df = spark.createDataFrame([("https://mmlspark.blob.core.windows.net/datasets/Speech/audio2.wav",),
                           ("https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3",)
                           ], ["url"])

# Run the Speech-to-text service to translate the audio into text
speech_to_text = (SpeechToTextSDK()
    .setSubscriptionKey(service_key)
    .setLocation("eastus")
    .setOutputCol("text")
    .setAudioDataCol("url")
    .setLanguage("en-US")
    .setProfanity("Masked"))

# Show the results of the translation
display(speech_to_text.transform(df).select("url", "text.DisplayText"))
```

### <a name="expected-result"></a>Beklenen sonuç

| url | Görünüm |
|:------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio2.wav | Özel konuşma, bir modelin tanınma kalitesini görsel olarak incelemenize olanak sağlayan araçlar sağlar. Bu, ses verilerini özel konuşma portalından karşılık gelen tanıma sonucuyla karşılaştırır. Karşıya yüklenen sesi kayıttan çalarken, belirtilen tanıma sonucunun doğru olup olmadığını belirleyebilirsiniz. Bu araç, herhangi bir ses verisi almak zorunda kalmadan Microsoft 'un temel konuşma konuşmasının kalitesini metin modeline veya eğitilen özel bir modele hızlıca incelemenizi sağlar. |
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3 | Bir genttaman sır ekleyin görsel denetimi.    |
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3 | Beni duydum. |
| https://mmlspark.blob.core.windows.net/datasets/Speech/audio3.mp3 | Daha fazla dinediğim radyo için sorunsuz bir küme istiyorum. |


## <a name="anomaly-detector-sample"></a>Anomali algılayıcı örneği

[Anomali algılayıcısı](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/) , zaman serisi verilerinizde ırdüzenleyeaykırıluluğunu saptamak için idealdir. Bu örnekte, tüm zaman serisinde bozukluklar bulmak için hizmeti kullanırız.

```python
from pyspark.sql.functions import lit

# Create a dataframe with the point data that Anomaly Detector requires
df = spark.createDataFrame([
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
], ["timestamp", "value"]).withColumn("group", lit("series1"))

# Run the Anomaly Detector service to look for irregular data
anamoly_detector = (SimpleDetectAnomalies()
  .setSubscriptionKey(anomaly_key)
  .setLocation("eastus")
  .setTimestampCol("timestamp")
  .setValueCol("value")
  .setOutputCol("anomalies")
  .setGroupbyCol("group")
  .setGranularity("monthly"))

# Show the full results of the analysis with the anomalies marked as "True"
display(anamoly_detector.transform(df).select("timestamp", "value", "anomalies.isAnomaly"))
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

## <a name="arbitrary-web-apis"></a>Rastgele Web API 'Leri

Spark üzerinde HTTP ile, büyük veri işlem hattınızda herhangi bir Web hizmeti kullanılabilir. Bu örnekte, dünyanın dört bir yanındaki çeşitli ülkeler hakkında bilgi almak için [Dünya BANKASı API](http://api.worldbank.org/v2/country/) 'sini kullanırız.

```python
from requests import Request
from mmlspark.io.http import HTTPTransformer, http_udf
from pyspark.sql.functions import udf, col

# Use any requests from the python requests library
def world_bank_request(country):
  return Request("GET", "http://api.worldbank.org/v2/country/{}?format=json".format(country))

# Create a dataframe with spcificies which countries we want data on
df = (spark.createDataFrame([("br",),("usa",)], ["country"])
  .withColumn("request", http_udf(world_bank_request)(col("country"))))

# Much faster for big data because of the concurrency :)
client = (HTTPTransformer()
      .setConcurrency(3)
      .setInputCol("request")
      .setOutputCol("response"))

# Get the body of the response
def get_response_body(resp):
  return resp.entity.content.decode()

# Show the details of the country data returned
display(client.transform(df).select("country", udf(get_response_body)(col("response")).alias("response")))
```

### <a name="expected-result"></a>Beklenen sonuç

| ülke   | yanıt |
|:----------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| br | [{"sayfa": 1, "sayfa": 1, "per_page": "50", "Toplam": 1}, [{"id": "KÖŞELI", "iso2Code": "BR", "ad": "Brezilya", "bölge": {"ID": "LCN", "iso2Code": "ZJ", "Value": "Latin Amerika & Karayipler"}, "adminregion": {"ID": "LAC", "iso2Code": "XJ", "Value": "Latin Amerika & Karayipler (yüksek gelir hariç)"}, "incomeLevel": {"ID": "UMC", "iso2Code": "XT", "Value": "üst orta gelir"}, "lendingType": {"ID": "IBD", "iso2Code": "XF", "Value": "ıBRD"}, "capitalCity": "Brasilia", "Boylam": "-47.9292", "Enlem": "-15,7801"}]] |
| ABD  | [{"sayfa": 1, "sayfa": 1, "per_page": "50", "Toplam": 1}, [{"id": "USA", "iso2Code": "US", "ad": "Birleşik Devletler", "bölge": {"id": "NAC", "iso2Code": "XU", "değer": "Kuzey Amerika"}, "adminregion": {"ID": "", "iso2Code": "", "Value": ""}, "incomeLevel": {"ID": "HIC", "iso2Code": "XD", "Value": "yüksek gelir"}, "Merdingtype": {"ID": "LNX", "iso2Code": "XX", "Value": "sınıflandırılmamış"}, "capitalCity": "Washington D.C.", "Boylam": "-77.032", "Enlem": "38.8895"}]] |

## <a name="see-also"></a>Ayrıca bkz.

* [Tarif: anomali algılama](./recipes/anomaly-detection.md)
* [Tarif: sanat Gezgini](./recipes/art-explorer.md)
