---
title: 'Öğretici: Azure Databricks kullanarak veri akışında anormallik algılama'
titleSuffix: Azure Cognitive Services
description: Verilerinizdeki anormallikleri izlemek için Anomali Dedektörü API'sını ve Azure Veri Tuğlalarını nasıl kullanacağınızı öğrenin.
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: tutorial
ms.date: 03/05/2020
ms.author: aahi
ms.openlocfilehash: e0df0773daf8f9be21ac70d8390013adfd93483a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78402678"
---
# <a name="tutorial-anomaly-detection-on-streaming-data-using-azure-databricks"></a>Öğretici: Azure Databricks kullanarak veri akışında anormallik algılama

[Azure Databricks](https://azure.microsoft.com/services/databricks/) hızlı, kolay ve işbirlikçi apache Spark tabanlı bir analiz hizmetidir. Azure Bilişsel Hizmetleri'nin bir parçası olan Anomaly Detector API, zaman serisi verilerinizi izlemenin bir yolunu sağlar. Azure Databricks'i kullanarak neredeyse gerçek zamanlı olarak bir veri akışında anormallik algılaması çalıştırmak için bu öğreticiyi kullanın. Azure Etkinlik Hub'larını kullanarak twitter verilerini yuvarlar ve Spark Event Hub'ları bağlayıcısını kullanarak Azure Veri Tuğlaları'na aktaracaksınız. Daha sonra, akışlı verilerdeki anormallikleri algılamak için API'yi kullanırsınız. 

Aşağıdaki şekilde uygulama akışı gösterilmektedir:

![Etkinlik Hub'ları ve Bilişsel Hizmetlere sahip Azure Veri Tuğlaları](../media/tutorials/databricks-cognitive-services-tutorial.png "Etkinlik Hub'ları ve Bilişsel Hizmetlere sahip Azure Veri Tuğlaları")

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Databricks çalışma alanı oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Akış verilerine erişmek için bir Twitter uygulaması oluşturma
> * Azure Databricks’te not defterleri oluşturma
> * Event Hubs ve Twitter API’si için kitaplıklar ekleme
> * Bir Anomali Dedektörü kaynağı oluşturun ve erişim anahtarını alın
> * Event Hubs’a tweet’ler gönderme
> * Event Hubs’tan tweet’leri okuma
> * Tweetlerde anomali algılamayı çalıştırın

> [!Note]
> * Bu öğretici, Anomali Dedektörü API'sı için önerilen [çözüm mimarisini](https://azure.microsoft.com/solutions/architecture/anomaly-detector-process/) uygulamak için bir yaklaşım sunar.
> * Bu öğretici, Anomaly Detector API veya Azure Databricks için ücretsiz deneme sürümüyle tamamlanamaz. 

Yoksa [bir Azure aboneliği](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Azure [Etkinlik Hub'ları ad alanı](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) ve etkinlik merkezi.

- Olay Hub'larına erişmek için [bağlantı dizesi](../../../event-hubs/event-hubs-get-connection-string.md) ad alanı. Bağlantı dizesi aşağıdakilere benzer bir biçime sahip olmalıdır:

    `Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<key name>;SharedAccessKey=<key value>`. 

- Olay Hub'ları için paylaşılan erişim ilkesi adı ve ilke anahtarı.

Ad alanı ve etkinlik merkezi oluşturma hakkında bilgi için Azure Etkinlik Hub'larına [hızlı](../../../event-hubs/event-hubs-create.md) bir başlangıç bakın.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde, [Azure portalını](https://portal.azure.com/)kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure portalında, **bir kaynak** > Oluştur**Analytics** > **Azure Databricks'i**seçin.

    ![Azure portalında veri tuğlaları](../media/tutorials/azure-databricks-on-portal.png "Azure portalında veri tuğlaları")

3. **Azure Databricks Hizmeti**altında, bir Databricks çalışma alanı oluşturmak için aşağıdaki değerleri sağlar:


    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../../azure-resource-manager/management/overview.md). |
    |**Konum**     | **Doğu ABD 2'yi** veya kullanılabilir diğer bölgelerden birini seçin. Bölge kullanılabilirliği için [bölgeye göre kullanılabilen Azure hizmetlerine](https://azure.microsoft.com/regions/services/) bakın.        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. **Deneme seçmeyin.** Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

    **Oluştur'u**seçin.

4. Çalışma alanının oluşturulması birkaç dakika sürer. 

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan Yeni **Küme'yi**seçin.

    ![Azure'da Veri Tuğlaları](../media/tutorials/databricks-on-azure.png "Azure'da Veri Tuğlaları")

3. Yeni **Küme** sayfasında, küme oluşturmak için değerleri sağlayın.

    ![Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma](../media/tutorials/create-databricks-spark-cluster.png "Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

   * Küme için bir ad girin.
   * Bu makale için, **5,2** çalışma süresine sahip bir küme oluşturun. **5.3** çalışma süresini SEÇMEYIn.
   * Dakikalar süren inactivity onay kutusunun **seçilmesinden sonra \_ \_ Sonlandırma'nın seçildiğinden** emin olun. Küme kullanılmıyorsa, kümeyi sonlandırmak için bir süre (dakika olarak) sağlayın.

     **Küme oluştur**’u seçin. 
4. Küme oluşturma birkaç dakika sürer. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Tweet’lerin akışını almak için Twitter’da bir uygulama oluşturmanız gerekir. Adımları izleyerek bir Twitter uygulaması oluşturun ve bu öğreticiyi tamamlamak için ihtiyaç duyduğunuz değerleri kaydedin.

1. Bir web tarayıcısından [Twitter Uygulama Yönetimi](https://apps.twitter.com/)’ne gidip **Yeni Uygulama Oluştur**’u seçin.

    ![Twitter uygulaması oluşturma](../media/tutorials/databricks-create-twitter-app.png "Twitter uygulaması oluşturma")

2. **Uygulama oluşturun** sayfasında yeni uygulamaya ilişkin ayrıntıları sağlayın ve **Kendi Twitter uygulamanızı oluşturun**’u seçin.

    ![Twitter uygulama detayları](../media/tutorials/databricks-provide-twitter-app-details.png "Twitter uygulama detayları")

3. Uygulama sayfasında **Anahtarlar ve Erişim Belirteçleri** sekmesini seçin ve **Tüketici Anahtarı** ve **Tüketici Parolası** değerlerini kopyalayın. Ayrıca **Erişim belirtecimi oluştur**’u seçip erişim belirteçleri oluşturun. **Erişim Belirteci** ve**Erişim Belirteci Parolası** değerlerini kopyalayın.

    ![Twitter uygulama detayları](../media/tutorials/twitter-app-key-secret.png "Twitter uygulama detayları")

Twitter uygulaması için aldığınız değerleri kaydedin. Öğreticinin sonraki bölümlerinde bu değerlere ihtiyacınız olacaktır.

## <a name="attach-libraries-to-spark-cluster"></a>Spark kümesine kitaplıklar ekleme

Bu öğreticide, Event Hubs’a tweet’ler göndermek için Twitter API’lerini kullanırsınız. Azure Event Hubs’a verileri okuyup yazmak için de [Apache Spark Event Hubs bağlayıcısını](https://github.com/Azure/azure-event-hubs-spark) kullanırsınız. Kümenizin parçası olarak bu API’leri kullanmak için, Azure Databricks’e bunları kitaplıklar olarak ekler ve sonra Spark kümenizle ilişkilendirirsiniz. Aşağıdaki yönergeler, çalışma alanınızdaki **Paylaşılan** klasöre kitaplıkların nasıl ekleyeceğinigösterir.

1. Azure Databricks çalışma alanında **Çalışma Alanı**’nı seçin ve sonra **Paylaşılan**’a sağ tıklayın. Bağlam menüsünden**Kitaplık** **Oluştur'u** > seçin.

   ![Kitaplık iletişim kutusu ekleme](../media/tutorials/databricks-add-library-option.png "Kitaplık iletişim kutusu ekleme")

2. Yeni Kütüphane sayfasında, **Kaynak** için **Maven**seçin. **Koordinatlar**için, eklemek istediğiniz paketin koordinatını girin. Bu öğreticide kullanılan kitaplıklar için Maven koordinatları aşağıdaki gibidir:

   * Spark Event Hubs bağlayıcısı - `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`
   * Twitter API’si - `org.twitter4j:twitter4j-core:4.0.7`

     ![Maven koordinatlarını sağlayın](../media/tutorials/databricks-eventhub-specify-maven-coordinate.png "Maven koordinatlarını sağlayın")

3. **Oluştur'u**seçin.

4. Kitaplığı eklediğiniz klasörü seçin ve sonra kitaplık adını seçin.

    ![Eklemek için kitaplığı seçin](../media/tutorials/select-library.png "Eklemek için kitaplığı seçin")

5. Kitaplık sayfasında küme yoksa, **Kümeler'i** seçin ve oluşturduğunuz kümeyi çalıştırın. Durum 'Çalışan' gösterene kadar bekleyin ve ardından kitaplık sayfasına geri dön.
Kitaplık sayfasında, kitaplığı kullanmak istediğiniz kümeyi seçin ve sonra **Yükle'yi**seçin. Kitaplık kümeyle başarıyla ilişkilendirildikten sonra, durum hemen **Yüklenilen'e**dönüşür.

    ![Kitaplığı kümelemek için yükleme](../media/tutorials/databricks-library-attached.png "Kitaplığı kümelemek için yükleme")

6. Twitter paketi için şu adımları yineleyin, `twitter4j-core:4.0.7`.

## <a name="get-a-cognitive-services-access-key"></a>Bilişsel Hizmetler erişim anahtarını alma

Bu eğitimde, neredeyse gerçek zamanlı olarak bir tweet akışında anormallik algılaması çalıştırmak için [Azure Bilişsel Hizmetler Anomali Dedektörü API'lerini](../overview.md) kullanırsınız. API'leri kullanmadan önce Azure'da bir Anomaly Dedektörü kaynağı oluşturmanız ve Anomali Dedektörü API'lerini kullanmak için bir erişim anahtarı almanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **+ Kaynak oluştur**’u seçin.

3. Azure Marketi altında, **Select AI + Machine Learning** > **Tüm** > Bilişsel Hizmetleri Gör **- Daha Fazla** > **Anomali Dedektörü**. Veya doğrudan **Oluştur** iletişim kutusuna gitmek için [bu bağlantıyı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) kullanabilirsiniz.

    ![Anomali Dedektörü kaynağı oluşturma](../media/tutorials/databricks-cognitive-services-anomaly-detector.png "Anomali Dedektörü kaynağı oluşturma")

4. **Oluştur** iletişim kutusunda aşağıdaki değerleri sağlayın:

    |Değer |Açıklama  |
    |---------|---------|
    |Adı     | Anomali Dedektörü kaynağının adı.        |
    |Abonelik     | Kaynağın ilişkilendireceği Azure aboneliği.        |
    |Konum     | Azure konumu.        |
    |Fiyatlandırma katmanı     | Hizmet için bir fiyatlandırma katmanı. Anomaly Detector fiyatlandırması hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/anomaly-detector/)bakın.        |
    |Kaynak grubu     | Yeni bir kaynak grubu oluşturmak mı yoksa varolan bir tane seçmek mi istediğinizi belirtin.        |


     **Oluştur'u**seçin.

5. Kaynak oluşturulduktan sonra, **Genel Bakış** sekmesinden, ekran görüntüsünde gösterildiği gibi **Bitiş Noktası** URL'sini kopyalayın ve kaydedin. Ardından **erişim tuşlarını göster'i**seçin.

    ![Erişim anahtarlarını göster](../media/tutorials/cognitive-services-get-access-keys.png "Erişim anahtarlarını göster")

6. **Tuşlar**altında, kullanmak istediğiniz tuşa karşı kopya simgesini seçin. Erişim anahtarını kaydedin.

    ![Erişim anahtarlarını kopyalama](../media/tutorials/cognitive-services-copy-access-keys.png "Erişim anahtarlarını kopyalama")

## <a name="create-notebooks-in-databricks"></a>Databricks’te not defterleri oluşturma

Bu bölümde, Databricks çalışma alanında aşağıdaki adlarla iki not defteri oluşturacaksınız

- **SendTweetsToEventHub** - Bu, Twitter’dan tweet’ler almak ve bunları Event Hubs’ta akışa almak için kullandığınız bir üretici not defteridir.
- **AnalyzeTweetsFromEventHub** - Olay Hub'larından gelen tweetleri okumak ve anormallik algılamasını çalıştırmak için kullandığınız bir tüketici dizüstü bilgisayarı.

1. Azure Veri Tuğlaları çalışma alanında, sol bölmeden **Çalışma Alanı'nı** seçin. **Çalışma Alanı** açılır listesinden **Oluştur**’u ve sonra **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](../media/tutorials/databricks-create-notebook.png "Databricks'te not defteri oluşturma")

2. Not **Defteri Oluştur** iletişim kutusunda, **sendTweetsToEventHub'ı** ad olarak girin, dil olarak **Scala'yı** seçin ve daha önce oluşturduğunuz Kıvılcım kümesini seçin.

    ![Databricks'te not defteri oluşturma](../media/tutorials/databricks-notebook-details.png "Databricks'te not defteri oluşturma")

    **Oluştur'u**seçin.

3. **AnalyzeTweetsFromEventHub** not defterini oluşturma adımlarını yineleyin.

## <a name="send-tweets-to-event-hubs"></a>Event Hubs’a tweet’ler gönderme

**SendTweetsToEventHub** not defterine, aşağıdaki kodu yapıştırın ve yer tutucuyu daha önce oluşturduğunuz Etkinlik Hub'ları ad alanı ve Twitter uygulamanızın değerleriyle değiştirin. Bu not defteri, "Azure" anahtar kelimesi ile tweetlerden "Beğen" oluşturma süresini ve sayısını ayıklar ve bunları olay hub'larına gerçek zamanlı olarak aktarın.

```scala
//
// Send Data to Eventhub
//

import scala.collection.JavaConverters._
import com.microsoft.azure.eventhubs._
import java.util.concurrent._
import com.google.gson.{Gson, GsonBuilder, JsonParser}
import java.util.Date
import scala.util.control._
import twitter4j._
import twitter4j.TwitterFactory
import twitter4j.Twitter
import twitter4j.conf.ConfigurationBuilder

// Event Hub Config
val namespaceName = "[Placeholder: EventHub namespace]"
val eventHubName = "[Placeholder: EventHub name]"
val sasKeyName = "[Placeholder: EventHub access key name]"
val sasKey = "[Placeholder: EventHub access key key]"
val connStr = new ConnectionStringBuilder()
  .setNamespaceName(namespaceName)
  .setEventHubName(eventHubName)
  .setSasKeyName(sasKeyName)
  .setSasKey(sasKey)

// Connect to the Event Hub
val pool = Executors.newScheduledThreadPool(1)
val eventHubClient = EventHubClient.create(connStr.toString(), pool)

def sendEvent(message: String) = {
  val messageData = EventData.create(message.getBytes("UTF-8"))
  eventHubClient.get().send(messageData)
  System.out.println("Sent event: " + message + "\n")
}

case class MessageBody(var timestamp: Date, var favorite: Int)
val gson: Gson = new GsonBuilder().setDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'").create()

val twitterConsumerKey = "[Placeholder: Twitter consumer key]"
val twitterConsumerSecret = "[Placeholder: Twitter consumer seceret]"
val twitterOauthAccessToken = "[Placeholder: Twitter oauth access token]"
val twitterOauthTokenSecret = "[Placeholder: Twitter oauth token secret]"

val cb = new ConfigurationBuilder()
cb.setDebugEnabled(true)
  .setOAuthConsumerKey(twitterConsumerKey)
  .setOAuthConsumerSecret(twitterConsumerSecret)
  .setOAuthAccessToken(twitterOauthAccessToken)
  .setOAuthAccessTokenSecret(twitterOauthTokenSecret)

val twitterFactory = new TwitterFactory(cb.build())
val twitter = twitterFactory.getInstance()

// Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!

val query = new Query(" #Azure ")
query.setCount(100)
query.lang("en")

var finished = false
var maxStatusId = Long.MinValue
var preMaxStatusId = Long.MinValue
val innerLoop = new Breaks
while (!finished) {
  val result = twitter.search(query)
  val statuses = result.getTweets()
  var lowestStatusId = Long.MaxValue
  innerLoop.breakable {
    for (status <- statuses.asScala) {
      if (status.getId() <= preMaxStatusId) {
        preMaxStatusId = maxStatusId
        innerLoop.break
      }
      if(!status.isRetweet()) {
        sendEvent(gson.toJson(new MessageBody(status.getCreatedAt(), status.getFavoriteCount())))
      }
      lowestStatusId = Math.min(status.getId(), lowestStatusId)
      maxStatusId = Math.max(status.getId(), maxStatusId)
    }
  }
  
  if (lowestStatusId == Long.MaxValue) {
    preMaxStatusId = maxStatusId
  }
  Thread.sleep(10000)
  query.setMaxId(lowestStatusId - 1)
}

// Close connection to the Event Hub
eventHubClient.get().close()
pool.shutdown()
```

Not defterlerini çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı görürsünüz. Çıktıdaki her olay, Olay Hub'larına yutulan "Beğen" sayısı ve zaman damgasının bir birleşimidir.

    Sent event: {"timestamp":"2019-04-24T09:39:40.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:38:48.000Z","favorite":1}

    Sent event: {"timestamp":"2019-04-24T09:38:36.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:37:27.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:37:00.000Z","favorite":2}

    Sent event: {"timestamp":"2019-04-24T09:31:11.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:30:15.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:30:02.000Z","favorite":1}

    ...
    ...

## <a name="read-tweets-from-event-hubs"></a>Event Hubs’tan tweet’leri okuma

**AnalyzeTweetsFromEventHub** dizüstü bilgisayarında, aşağıdaki kodu yapıştırın ve yer tutucuyu daha önce oluşturduğunuz Anomaly Detector kaynağınızın değerleriyle değiştirin. Bu not defteri, **SendTweetsToEventHub** not defterini kullanarak önceden Event Hubs’ta akışa alınan tweet’leri okur.

İlk olarak, Anomaly dedektörü aramak için bir müşteri yazın. 
```scala

//
// Anomaly Detection Client
//

import java.io.{BufferedReader, DataOutputStream, InputStreamReader}
import java.net.URL
import java.sql.Timestamp

import com.google.gson.{Gson, GsonBuilder, JsonParser}
import javax.net.ssl.HttpsURLConnection

case class Point(var timestamp: Timestamp, var value: Double)
case class Series(var series: Array[Point], var maxAnomalyRatio: Double, var sensitivity: Int, var granularity: String)
case class AnomalySingleResponse(var isAnomaly: Boolean, var isPositiveAnomaly: Boolean, var isNegativeAnomaly: Boolean, var period: Int, var expectedValue: Double, var upperMargin: Double, var lowerMargin: Double, var suggestedWindow: Int)
case class AnomalyBatchResponse(var expectedValues: Array[Double], var upperMargins: Array[Double], var lowerMargins: Array[Double], var isAnomaly: Array[Boolean], var isPositiveAnomaly: Array[Boolean], var isNegativeAnomaly: Array[Boolean], var period: Int)

object AnomalyDetector extends Serializable {

  // Cognitive Services API connection settings
  val subscriptionKey = "[Placeholder: Your Anomaly Detector resource access key]"
  val endpoint = "[Placeholder: Your Anomaly Detector resource endpoint]"
  val latestPointDetectionPath = "/anomalydetector/v1.0/timeseries/last/detect"
  val batchDetectionPath = "/anomalydetector/v1.0/timeseries/entire/detect";
  val latestPointDetectionUrl = new URL(endpoint + latestPointDetectionPath)
  val batchDetectionUrl = new URL(endpoint + batchDetectionPath)
  val gson: Gson = new GsonBuilder().setDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'").setPrettyPrinting().create()

  def getConnection(path: URL): HttpsURLConnection = {
    val connection = path.openConnection().asInstanceOf[HttpsURLConnection]
    connection.setRequestMethod("POST")
    connection.setRequestProperty("Content-Type", "text/json")
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey)
    connection.setDoOutput(true)
    return connection
  }

  // Handles the call to Cognitive Services API.
  def processUsingApi(request: String, path: URL): String = {
    println(request)
    val encoded_text = request.getBytes("UTF-8")
    val connection = getConnection(path)
    val wr = new DataOutputStream(connection.getOutputStream())
    wr.write(encoded_text, 0, encoded_text.length)
    wr.flush()
    wr.close()

    val response = new StringBuilder()
    val in = new BufferedReader(new InputStreamReader(connection.getInputStream()))
    var line = in.readLine()
    while (line != null) {
      response.append(line)
      line = in.readLine()
    }
    in.close()
    return response.toString()
  }

  // Calls the Latest Point Detection API.
  def detectLatestPoint(series: Series): Option[AnomalySingleResponse] = {
    try {
      println("Process Timestamp: " + series.series.apply(series.series.length-1).timestamp.toString + ", size: " + series.series.length)
      val response = processUsingApi(gson.toJson(series), latestPointDetectionUrl)
      println(response)
      // Deserializing the JSON response from the API into Scala types
      val anomaly = gson.fromJson(response, classOf[AnomalySingleResponse])
      Thread.sleep(5000)
      return Some(anomaly)
    } catch {
      case e: Exception => {
        println(e)
        e.printStackTrace()
        return None
      }
    }
  }

  // Calls the Batch Detection API.
  def detectBatch(series: Series): Option[AnomalyBatchResponse] = {
    try {
      val response = processUsingApi(gson.toJson(series), batchDetectionUrl)
      println(response)
      // Deserializing the JSON response from the API into Scala types
      val anomaly = gson.fromJson(response, classOf[AnomalyBatchResponse])
      Thread.sleep(5000)
      return Some(anomaly)
    } catch {
      case e: Exception => {
        println(e)
        return None
      }
    }
  }
}
```

Not defterlerini çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı görürsünüz.

    import java.io.{BufferedReader, DataOutputStream, InputStreamReader}
    import java.net.URL
    import java.sql.Timestamp
    import com.google.gson.{Gson, GsonBuilder, JsonParser}
    import javax.net.ssl.HttpsURLConnection
    defined class Point
    defined class Series
    defined class AnomalySingleResponse
    defined class AnomalyBatchResponse
    defined object AnomalyDetector

Ardından, gelecekteki kullanım için bir toplama işlevi hazırlayın.
```scala
//
// User Defined Aggregation Function for Anomaly Detection
//

import org.apache.spark.sql.Row
import org.apache.spark.sql.expressions.{MutableAggregationBuffer, UserDefinedAggregateFunction}
import org.apache.spark.sql.types.{StructType, TimestampType, FloatType, MapType, BooleanType, DataType}
import scala.collection.immutable.ListMap

class AnomalyDetectorAggregationFunction extends UserDefinedAggregateFunction {
  override def inputSchema: StructType = new StructType().add("timestamp", TimestampType).add("value", FloatType)
  
  override def bufferSchema: StructType = new StructType().add("point", MapType(TimestampType, FloatType))
  
  override def dataType: DataType = BooleanType
  
  override def deterministic: Boolean = false
  
  override def initialize(buffer: MutableAggregationBuffer): Unit = {
    buffer(0) = Map()
  }
  
  override def update(buffer: MutableAggregationBuffer, input: Row): Unit = {
    buffer(0) = buffer.getAs[Map[java.sql.Timestamp, Float]](0) + (input.getTimestamp(0) -> input.getFloat(1))
  }
  
  override def merge(buffer1: MutableAggregationBuffer, buffer2: Row): Unit = {
    buffer1(0) = buffer1.getAs[Map[java.sql.Timestamp, Float]](0) ++ buffer2.getAs[Map[java.sql.Timestamp, Float]](0)
  }
  
  override def evaluate(buffer: Row): Any = {
    val points = buffer.getAs[Map[java.sql.Timestamp, Float]](0)
    if (points.size > 12) {
      val sorted_points = ListMap(points.toSeq.sortBy(_._1.getTime):_*)
      var detect_points: List[Point] = List()
      sorted_points.keys.foreach {
        key => detect_points = detect_points :+ new Point(key, sorted_points(key))
      }
      
      
      // 0.25 is maxAnomalyRatio. It represents 25%, max anomaly ratio in a time series.
      // 95 is the sensitivity of the algorithms.
      // Check Anomaly detector API reference (https://aka.ms/anomaly-detector-rest-api-ref)
      
      val series: Series = new Series(detect_points.toArray, 0.25, 95, "hourly")
      val response: Option[AnomalySingleResponse] = AnomalyDetector.detectLatestPoint(series)
      if (!response.isEmpty) {
        return response.get.isAnomaly
      }
    }
    
    return None
  }
}

```

Not defterlerini çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı görürsünüz.

    import org.apache.spark.sql.Row
    import org.apache.spark.sql.expressions.{MutableAggregationBuffer, UserDefinedAggregateFunction}
    import org.apache.spark.sql.types.{StructType, TimestampType, FloatType, MapType, BooleanType, DataType}
    import scala.collection.immutable.ListMap
    defined class AnomalyDetectorAggregationFunction

Daha sonra anomali tespiti için olay merkezinden veri yükleyin. Yer tutucuyu daha önce oluşturduğunuz Azure Etkinlik Hub'larınız için değerlerle değiştirin.

```scala
//
// Load Data from Eventhub
//

import org.apache.spark.eventhubs._
import org.apache.spark.sql.types._
import org.apache.spark.sql.functions._

val connectionString = ConnectionStringBuilder("[Placeholder: EventHub namespace connection string]")
  .setEventHubName("[Placeholder: EventHub name]")
  .build

val customEventhubParameters =
  EventHubsConf(connectionString)
  .setConsumerGroup("$Default")
  .setMaxEventsPerTrigger(100)

val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()

val messages =
  incomingStream
  .withColumn("enqueuedTime", $"enqueuedTime".cast(TimestampType))
  .withColumn("body", $"body".cast(StringType))
  .select("enqueuedTime", "body")

val bodySchema = new StructType().add("timestamp", TimestampType).add("favorite", IntegerType)

val msgStream = messages.select(from_json('body, bodySchema) as 'fields).select("fields.*")

msgStream.printSchema

display(msgStream)

```

Çıktı şimdi aşağıdaki görüntüye benzer. Veriler gerçek zamanlı olduğundan, tablodaki tarihinizin bu öğreticideki tarihten farklı olabileceğini unutmayın.
![Olay merkezinden Veri Yükleme](../media/tutorials/load-data-from-eventhub.png "Olay Merkezinden Veri Yükleme")

Artık Azure Etkinlik Hub'larından Azure Veri Tuğlaları'na alabildiğiniz verileri, Apache Spark'ın Etkinlik Hub'ları bağlayıcısını kullanarak neredeyse gerçek zamanlı olarak Azure Veri Tuğlaları'na aktarmış sınız. Spark için Event Hubs bağlayıcısını kullanma hakkında daha fazla bilgi için [bağlayıcı belgesi](https://github.com/Azure/azure-event-hubs-spark/tree/master/docs)ne başvurun.



## <a name="run-anomaly-detection-on-tweets"></a>Tweetlerde anomali algılamayı çalıştırın

Bu bölümde, Anomali dedektörü API kullanılarak alınan tweetlerde anomali algılaması yapıyorsun. Bu bölüm için, aynı **AnalyzeTweetsFromEventHub** not defterine kod parçacıklarını eklersiniz.

Anomali tespiti yapmak için öncelikle metrik sayınızı saate göre toplamanız gerekir.
```scala
//
// Aggregate Metric Count by Hour
//

// If you want to change granularity, change the groupBy window. 
val groupStream = msgStream.groupBy(window($"timestamp", "1 hour"))
  .agg(avg("favorite").alias("average"))
  .withColumn("groupTime", $"window.start")
  .select("groupTime", "average")

groupStream.printSchema

display(groupStream)
```
Çıktı şimdi aşağıdaki parçacıkları benzer.
```
groupTime                       average
2019-04-23T04:00:00.000+0000    24
2019-04-26T19:00:00.000+0000    47.888888888888886
2019-04-25T12:00:00.000+0000    32.25
2019-04-26T09:00:00.000+0000    63.4
...
...

```

Sonra delta için toplu çıkış sonucu olsun. Anomali algılama daha uzun bir geçmiş penceresi gerektirdiğinden, algılamak istediğiniz noktanın geçmiş verilerini tutmak için Delta'yı kullanıyoruz. Oluşturulacak nitelikli bir Delta tablo adı (örneğin, "tweetler") ile "[Yer Tutucu: tablo adı]" değiştirin. "[Yer tutucu: denetim noktaları için klasör adı]" kuralını her çalıştırdığınızda benzersiz bir dize değeriyle değiştirin (örneğin, "etl-from-eventhub-20190605").
Azure Databricks'te Delta Gölü hakkında daha fazla bilgi edinmek için lütfen [Delta Lake Guide'a](https://docs.azuredatabricks.net/delta/index.html) bakın


```scala
//
// Output Aggregation Result to Delta
//

groupStream.writeStream
  .format("delta")
  .outputMode("complete")
  .option("checkpointLocation", "/delta/[Placeholder: table name]/_checkpoints/[Placeholder: folder name for checkpoints]")
  .table("[Placeholder: table name]")

```

"[Yer tutucu: tablo adı]" ile yukarıda seçtiğiniz Delta tablo adını değiştirin.
```scala
//
// Show Aggregate Result
//

val twitterCount = spark.sql("SELECT COUNT(*) FROM [Placeholder: table name]")
twitterCount.show()

val twitterData = spark.sql("SELECT * FROM [Placeholder: table name] ORDER BY groupTime")
twitterData.show(200, false)

display(twitterData)
```
Aşağıdaki gibi çıktı: 
```
groupTime                       average
2019-04-08T01:00:00.000+0000    25.6
2019-04-08T02:00:00.000+0000    6857
2019-04-08T03:00:00.000+0000    71
2019-04-08T04:00:00.000+0000    55.111111111111114
2019-04-08T05:00:00.000+0000    2203.8
...
...

```

Şimdi toplanan zaman serisi verileri sürekli Delta içine yutulan. O zaman son noktanın anomalisini tespit etmek için saatlik bir iş zamanlayabilirsiniz. "[Yer tutucu: tablo adı]" ile yukarıda seçtiğiniz Delta tablo adını değiştirin.

```scala
//
// Anomaly Detection
//

import java.time.Instant
import java.time.format.DateTimeFormatter
import java.time.ZoneOffset
import java.time.temporal.ChronoUnit

val detectData = spark.read.format("delta").table("[Placeholder: table name]")

// You could use Databricks to schedule an hourly job and always monitor the latest data point
// Or you could specify a const value here for testing purpose
// For example, val endTime = Instant.parse("2019-04-16T00:00:00Z")
val endTime = Instant.now()

// This is when your input of anomaly detection starts. It is hourly time series in this tutorial, so 72 means 72 hours ago from endTime.
val batchSize = 72
val startTime = endTime.minus(batchSize, ChronoUnit.HOURS)

val DATE_TIME_FORMATTER = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").withZone(ZoneOffset.UTC);

val series = detectData.filter($"groupTime" <= DATE_TIME_FORMATTER.format(endTime))
  .filter($"groupTime" > DATE_TIME_FORMATTER.format(startTime))
  .sort($"groupTime")

series.createOrReplaceTempView("series")

//series.show()

// Register the function to access it
spark.udf.register("anomalydetect", new AnomalyDetectorAggregationFunction)

val adResult = spark.sql("SELECT '" + endTime.toString + "' as datetime, anomalydetect(groupTime, average) as anomaly FROM series")
adResult.show()
```
Aşağıdaki gibi sonuç: 

```
+--------------------+-------+
|           timestamp|anomaly|
+--------------------+-------+
|2019-04-16T00:00:00Z|  false|
+--------------------+-------+
```

İşte bu kadar! Azure Veri Tuğlaları'nı kullanarak verileri Azure Etkinlik Hub'larına başarıyla aktarmış, Akış Verilerini Olay Hub'ları bağlayıcısını kullanarak tüketmiş ve ardından veri akışında neredeyse gerçek zamanlı olarak anormallik algılaması çalıştırmışsınız.
Bu öğreticide, parçalı lık saatlik olsa da, gereksiniminizi karşılamak için her zaman parçalı birimi değiştirebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi çalıştırdıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için, Azure Veri Tuğlaları çalışma alanında sol bölmeden **Kümeler'i** seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunundaki elipslerin üzerine taşıyın ve **Sonla** simgesini seçin ve sonra **Onayla'yı**seçin.

![Databricks kümesini durdurma](../media/tutorials/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi oluştururken **etkinlik dakikalarından \_ \_ sonra Sonlandırma** onay kutusunu seçmeniz koşuluyla, kümeyi el ile sonlandırmazsanız otomatik olarak durur. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, verileri Azure Event Hubs’ta akışa almak ve sonra gerçek zamanlı olarak Event Hubs’tan akış verilerini okumak için Azure Databricks’i kullanmayı öğrendiniz. Anomaly Detector API'yi nasıl arayacağınızı ve Power BI masaüstünü kullanarak anomalileri nasıl görselleştireceğimize nasıl erişin. 

> [!div class="nextstepaction"]
>[Power BI masaüstü ile toplu anomali algılama](batch-anomaly-detection-powerbi.md)
