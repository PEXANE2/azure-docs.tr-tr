---
title: "Öğretici: HDInsight 'ta Apache Spark Azure Event Hubs verileri işleme"
description: Öğretici-Azure HDInsight 'ta Apache Spark Azure Event Hubs 'a bağlama ve akış verilerini işleme.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/24/2019
ms.openlocfilehash: be21b809272a132ee6e63582036c36ad5dcdf4ad
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266204"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-apache-spark-in-hdinsight"></a>Öğretici: HDInsight 'ta Azure Event Hubs ve Apache Spark kullanarak işçleri işleme

Bu öğreticide, bir Azure Olay Hub 'ına fazla kayıt göndermek için bir [Apache Spark](https://spark.apache.org/) akış uygulaması oluşturmayı ve Olay Hub 'ından farklı bir uygulama oluşturmayı öğreneceksiniz. Spark akışının ayrıntılı bir açıklaması için bkz. [Apache Spark akışa genel bakış](https://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight, Azure 'daki bir Spark kümesine aynı akış özelliklerini getirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Olay Hub 'ına ileti gönderme
> * Azure Olay Hub 'ından iletileri okuma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Bkz. [Apache Spark kümesi oluşturma](./apache-spark-jupyter-spark-sql-use-portal.md).

* HDInsight üzerinde Spark ile Jupyter Notebook kullanma bilgisi. Daha fazla bilgi için bkz. [HDInsight üzerinde verileri yükleme ve sorguları çalıştırma Apache Spark](./apache-spark-load-data-run-query.md).

* [Twitter hesabı](https://twitter.com/i/flow/signup).

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Tweet’lerin akışını almak için Twitter’da bir uygulama oluşturursunuz. Twitter uygulaması oluşturma yönergelerini izleyin ve bu öğreticiyi tamamlayabilmeniz için ihtiyacınız olan değerleri yazın.

1. [Twitter uygulama yönetimine](https://apps.twitter.com/)gidin.

1. **Yeni uygulama oluştur**' u seçin.

1. Aşağıdaki değerleri sağlayın:

    |Özellik |Value |
    |---|---|
    |Name|Uygulama adını belirtin. Bu öğretici için kullanılan değer **HDISparkStreamApp0423**. Bu ad benzersiz bir ad olmalıdır.|
    |Açıklama|Uygulamanın kısa bir açıklamasını sağlayın. Bu öğretici için kullanılan değer **basit bir HDInsight Spark akış uygulamasıdır**.|
    |Web sitesi|Uygulamanın Web sitesini belirtin. Geçerli bir Web sitesi olması gerekmez.  Bu öğretici `http://www.contoso.com`için kullanılan değer.|
    |Geri çağırma URL'si|Boş bırakabilirsiniz.|

1. **Evet, Twitter geliştirici sözleşmesini okudum ve kabul**ediyorum ' u seçin ve ardından **Twitter uygulamanızı oluştur**' u seçin.

1. **Anahtarlar ve erişim belirteçleri** sekmesini seçin.

1. Sayfanın sonunda **erişim Belirtecimi oluştur** ' u seçin.

1. Sayfadan aşağıdaki değerleri yazın.  Öğreticide daha sonra bu değerlere ihtiyacınız vardır:

    - **Tüketici anahtarı (API anahtarı)**    
    - **Tüketici gizli dizisi (API parolası)**  
    - **Erişim belirteci**
    - **Erişim belirteci gizli dizisi**   

## <a name="create-an-azure-event-hubs-namespace"></a>Azure Event Hubs ad alanı oluşturma

Bu olay hub 'ını kullanarak, doldurulabilir bir mağaza yapabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Sol menüden **tüm hizmetler**' i seçin.  

3. Nesnelerin **interneti**altında **Event Hubs**' yi seçin. 

    ![Spark akış örneği için Olay Hub 'ı oluşturma](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Spark akış örneği için Olay Hub 'ı oluşturma")

4. **+ Ekle** öğesini seçin.

5. Yeni Event Hubs ad alanı için aşağıdaki değerleri girin:

    |Özellik |Value |
    |---|---|
    |Name|Olay Hub 'ı için bir ad girin.  Bu öğretici için kullanılan değer **myeventhubns20180403**.|
    |Fiyatlandırma katmanı|**Standart**' ı seçin.|
    |Subscription|Uygun aboneliğinizi seçin.|
    |Resource group|Açılan listeden var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin.|
    |Location|Gecikme ve maliyetleri azaltmak için HDInsight 'ta Apache Spark kümeniz ile aynı **konumu** seçin.|
    |Otomatik Şişir etkinleştir (Isteğe bağlı) |Otomatik şişir, trafiğiniz kendisine atanan üretilen Iş birimi kapasitesini aştığında Event Hubs ad alanına atanan üretilen Iş birimi sayısını otomatik olarak ölçeklendirir.  |
    |Maksimum üretilen Iş birimlerini otomatik Şişir (Isteğe bağlı)|Bu kaydırıcı yalnızca **Otomatik Şişir etkinleştir**' i denetlemeli görünür.  |

    ![Spark akış örneği için bir olay hub 'ı adı sağlayın](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Spark akış örneği için bir olay hub 'ı adı sağlayın")

6. Ad alanını oluşturmak için **Oluştur** ' u seçin.  Dağıtım birkaç dakika içinde tamamlanır.

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub 'ı oluşturma
Event Hubs ad alanı dağıtıldıktan sonra bir olay hub 'ı oluşturun.  Portaldan:

1. Sol menüden **tüm hizmetler**' i seçin.  

1. Nesnelerin **interneti**altında **Event Hubs**' yi seçin.  

1. Listeden Event Hubs ad alanınızı seçin.  

1. **Event Hubs ad alanı** sayfasında **+ Event hub**' ı seçin.  
1. **Olay Hub 'ı oluştur** sayfasında aşağıdaki değerleri girin:

    - **Ad**: Olay Hub 'ınız için bir ad verin. 
 
    - **Bölüm sayısı**: 10.  

    - **İleti bekletme**: 1.   
   
      ![Spark akış örneği için Olay Hub 'ı ayrıntılarını sağlama](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Spark akış örneği için Olay Hub 'ı ayrıntılarını sağlama")

1. **Oluştur**’u seçin.  Dağıtım birkaç saniye içinde tamamlanmalı ve Event Hubs ad alanı sayfasına döndürülecektir.

1. **Ayarlar**altında, **paylaşılan erişim ilkeleri**' ni seçin.

1. Seçin **RootManageSharedAccessKey**.
    
     ![Spark akış örneği Için Olay Hub 'ı Ilkelerini ayarlama](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Spark akış örneği Için Olay Hub 'ı Ilkelerini ayarlama")

1. **Birincil anahtar** ve **bağlantı dizesi-birincil anahtar** değerlerini, öğreticide daha sonra kullanmak üzere kaydedin.

     ![Spark akış örneği Için Olay Hub 'ı ilke anahtarlarını görüntüleme](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "Spark akış örneği Için Olay Hub 'ı ilke anahtarlarını görüntüleme")


## <a name="send-tweets-to-the-event-hub"></a>Olay Hub 'ına fazla doldurulabilir gönder

Bir Jupyter Not defteri oluşturun ve bunu **Sendtweetstoeventhub**olarak adlandırın. 

1. Dış Apache Maven kitaplıklarını eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.13,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Aşağıdaki `<Event hub name>`kodu `<Event hub namespace connection string>`, ,,`<CONSUMER SECRET>`,, ve`<TOKEN SECRET>` değerlerini uygun değerlerle değiştirerek düzenleyin. `<CONSUMER KEY>` `<ACCESS TOKEN>` Olay Hub 'ınıza ara kod göndermek için düzenlenmiş kodu çalıştırın:

    ```scala
    import java.util._
    import scala.collection.JavaConverters._
    import java.util.concurrent._
    
    import org.apache.spark._
    import org.apache.spark.streaming._
    import org.apache.spark.eventhubs.ConnectionStringBuilder

    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    import com.microsoft.azure.eventhubs._
    val pool = Executors.newFixedThreadPool(1)
    val eventHubClient = EventHubClient.create(connStr.toString(), pool)
    
    def sendEvent(message: String) = {
          val messageData = EventData.create(message.getBytes("UTF-8"))
          eventHubClient.get().send(messageData)
          println("Sent event: " + message + "\n")
    }
    
    import twitter4j._
    import twitter4j.TwitterFactory
    import twitter4j.Twitter
    import twitter4j.conf.ConfigurationBuilder

    // Twitter application configurations
    // Replace values below with yours   
    val twitterConsumerKey = "<CONSUMER KEY>"
    val twitterConsumerSecret = "<CONSUMER SECRET>"
    val twitterOauthAccessToken = "<ACCESS TOKEN>"
    val twitterOauthTokenSecret = "<TOKEN SECRET>"
    
    val cb = new ConfigurationBuilder()
    cb.setDebugEnabled(true).setOAuthConsumerKey(twitterConsumerKey).setOAuthConsumerSecret(twitterConsumerSecret).setOAuthAccessToken(twitterOauthAccessToken).setOAuthAccessTokenSecret(twitterOauthTokenSecret)
    
    val twitterFactory = new TwitterFactory(cb.build())
    val twitter = twitterFactory.getInstance()

    // Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!
    
    val query = new Query(" #Azure ")
    query.setCount(100)
    query.lang("en")
    var finished = false
    while (!finished) {
      val result = twitter.search(query)
      val statuses = result.getTweets()
      var lowestStatusId = Long.MaxValue
      for (status <- statuses.asScala) {
        if(!status.isRetweet()){
          sendEvent(status.getText())
        }
        lowestStatusId = Math.min(status.getId(), lowestStatusId)
        Thread.sleep(2000)
      }
      query.setMaxId(lowestStatusId - 1)
    }
    
    // Closing connection to the Event Hub
    eventHubClient.get().close()
    ```

3. Olay Hub 'ını Azure portal açın.  **Genel bakışta**, Olay Hub 'ına gönderilen iletileri gösteren bazı grafikler görürsünüz.

## <a name="read-tweets-from-the-event-hub"></a>Olay Hub 'ından daha fazla doldurulabilir

Başka bir Jupyter Not defteri oluşturun ve bunu **ReadTweetsFromEventHub**olarak adlandırın. 

1. Bir dış Apache Maven kitaplığı eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.13"}}
    ```

2. Aşağıdaki `<Event hub name>`kodu, ve `<Event hub namespace connection string>` değerlerini uygun değerlerle değiştirin. Olay Hub 'ınızdan çilleri okumak için düzenlenmiş kodu çalıştırın:

    ```scala
    import org.apache.spark.eventhubs._
    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build
    
    val customEventhubParameters = EventHubsConf(connStr).setMaxEventsPerTrigger(5)
    val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()
    //incomingStream.printSchema    
    
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    
    // Event Hub message format is JSON and contains "body" field
    // Body is binary, so you cast it to string to see the actual content of the message
    val messages = incomingStream.withColumn("Offset", $"offset".cast(LongType)).withColumn("Time (readable)", $"enqueuedTime".cast(TimestampType)).withColumn("Timestamp", $"enqueuedTime".cast(LongType)).withColumn("Body", $"body".cast(StringType)).select("Offset", "Time (readable)", "Timestamp", "Body")
    
    messages.printSchema
    
    messages.writeStream.outputMode("append").format("console").option("truncate", false).start().awaitTermination()
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

HDInsight ile Verileriniz Azure Storage 'da depolanır veya Azure Data Lake Storage, bu sayede bir kümeyi kullanımda olmadığında güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Sonraki öğreticide hemen çalışmayı planlıyorsanız kümeyi tutmak, aksi takdirde devam etmek ve kümeyi silmek isteyebilirsiniz.

Azure portalında kümeyi açıp **Sil**’i seçin.

![HDInsight Azure portalı kümeyi silme](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini Sil")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Olay Hub 'ına kayıt eklemek için bir Apache Spark akış uygulaması oluşturma ve Olay Hub 'ından farklı bir uygulama oluşturma hakkında bilgi edindiniz.  Bir makine öğrenimi uygulaması oluşturabileceğiniz makaleyi görmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Machine Learning uygulaması oluşturma](./apache-spark-ipython-notebook-machine-learning.md)
