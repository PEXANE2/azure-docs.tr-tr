---
title: Storm ile Etkinlik Hub'larından etkinlikleri işleme - Azure HDInsight
description: Visual Studio için HDInsight araçlarını kullanarak Visual Studio'da oluşturulan C# Storm topolojisi ile Azure Event Hub'larından gelen verileri nasıl işleyerek işleyerek nasıl işleniriz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: 85b7093df99127b690c51e8f2f28d18e3f5f3c95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981633"
---
# <a name="process-events-from-azure-event-hubs-with-apache-storm-on-hdinsight-c"></a>HDInsight'ta Apache Storm ile Azure Etkinlik Hub'larından etkinlikleri işleme (C#)

HDInsight'ta [Apache Storm'dan](https://storm.apache.org/) Azure Etkinlik Hub'larıyla nasıl çalışacağınızı öğrenin. Bu belge, Olay Hub'larından gelen verileri okumak ve yazmak için bir C# Storm topolojisi kullanır

> [!NOTE]  
> Bu projenin Java sürümü için, [HDInsight'ta (Java) Apache Storm olan Azure Etkinlik Hub'larından İşlem etkinliklerine](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)bakın.

## <a name="scpnet"></a>SCP.NET

Bu belgedeki adımlar, HDInsight'ta Storm ile kullanılmak üzere C# topolojileri ve bileşenleri oluşturmayı kolaylaştıran bir NuGet paketi olan SCP.NET kullanır.

HDInsight 3.4 ve daha fazla C # topologies çalıştırmak için Mono kullanın. Bu belgede kullanılan örnek HDInsight 3.6 ile çalışır. HDInsight için kendi .NET çözümlerinizi oluşturmayı planlıyorsanız, olası uyumsuzluklar için [Mono uyumluluk](https://www.mono-project.com/docs/about-mono/compatibility/) belgesini kontrol edin.

### <a name="cluster-versioning"></a>Küme sürümü

Projeniz için kullandığınız Microsoft.SCP.Net.SDK NuGet paketi, HDInsight'ta yüklü olan Storm'un ana sürümüyle eşleşmelidir. HDInsight sürümleri 3.5 ve 3.6 Storm 1.x kullanın, bu nedenle bu kümeleri ile sürüm 1.0.x.x SCP.NET kullanmanız gerekir.

C# topolojileri de .NET 4.5'i hedeflemelidir.

## <a name="how-to-work-with-event-hubs"></a>Etkinlik Hub'ları ile çalışma

Microsoft, Bir Fırtına topolojisinden Olay Hub'ları ile iletişim kurmak için kullanılabilecek bir Java bileşeni kümesi sağlar. Bu bileşenlerin HDInsight 3.6 uyumlu sürümünü içeren Java arşivi (JAR) dosyasını [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar)bulabilirsiniz.

> [!IMPORTANT]  
> Bileşenler Java'da yazılıyken, bunları C# topolojisinden kolayca kullanabilirsiniz.

Aşağıdaki bileşenler bu örnekte kullanılır:

* __EventHubSpout__: Olay Hub'larından gelen verileri okur.
* __EventHubBolt__: Olay Hub'larına veri yazar.
* __EventHubSpoutConfig__: EventHubSpout yapılandırmak için kullanılır.
* __EventHubBoltConfig : EventHubBolt__yapılandırmak için kullanılır.

### <a name="example-spout-usage"></a>Örnek spout kullanımı

SCP.NET, topolojinize bir EventHubSpout eklemek için yöntemler sağlar. Bu yöntemler, bir Java bileşeni eklemek için genel yöntemleri kullanarak daha bir emzme eklemek için daha kolay olun. Aşağıdaki örnek, SCP.NET tarafından sağlanan __SetEventHubSpout__ ve **EventHubSpoutConfig** yöntemlerini kullanarak bir emzme oluşturmanın nasıl yapılacağını gösterir:

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

Önceki örnek, __EventHubSpout__adlı yeni bir spout bileşeni oluşturur ve bir olay hub'ı ile iletişim kurmak için yapılandırır. Bileşen için paralellik ipucu olay hub'ındaki bölüm sayısına ayarlanır. Bu ayar, Storm'un her bölüm için bileşenin bir örneğini oluşturmasına olanak tanır.

### <a name="example-bolt-usage"></a>Örnek cıvata kullanımı

Cıvata bir örnek oluşturmak için **JavaComponmentConstructor** yöntemini kullanın. Aşağıdaki örnek, **EventHubBolt'un**yeni bir örneğinin nasıl oluşturulup yapılandırılabildiğini göstermektedir:

```csharp
// Java construcvtor for the Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set the bolt to subscribe to data from the spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]  
> Bu örnek, bir **EventHubBoltConfig**oluşturmak için **JavaComponentConstructor** kullanarak yerine, bir dize olarak geçirilen bir Clojure ifadesi kullanır , spout örnek yaptığı gibi. Her iki yöntem de çalışır. Size en iyi hissettiren yöntemi kullanın.

## <a name="download-the-completed-project"></a>Tamamlanan projeyi indirin

Bu makalede oluşturulan projenin tam bir sürümünü [GitHub'dan](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub)indirebilirsiniz. Ancak, yine de bu makaledeki adımları izleyerek yapılandırma ayarlarını sağlamanız gerekir.

### <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Storm kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **Fırtına'yı**seçin. **Storm**

* Azure [olay hub'ı.](../../event-hubs/event-hubs-create.md)

* [Azure .NET SDK](https://azure.microsoft.com/downloads/).

* [Visual Studio için HDInsight araçları.](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)

* Java JDK 1.8 veya daha sonra geliştirme ortamınızda. JDK indirmeoracle [mevcuttur.](https://aka.ms/azure-jdks)

  * **JAVA_HOME** ortamı değişkeni Java içeren dizini işaret etmelidir.
  * **%JAVA_HOME/bin** dizini yolda olmalıdır.

## <a name="download-the-event-hubs-components"></a>Olay Hub'ları bileşenlerini indirin

Olay Hub'ları emzve bolt [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar)bileşenini indirin.

Adlı `eventhubspout`bir dizin oluşturun ve dosyayı dizine kaydedin.

## <a name="configure-event-hubs"></a>Etkinlik Hub'larını Yapılandırma

Olay Hub'ları bu örnek için veri kaynağıdır. [Olay Hub'ları ile başlayın](../../event-hubs/event-hubs-create.md)"Bir olay merkezi oluşturma" bölümündeki bilgileri kullanın.

1. Etkinlik merkezi oluşturulduktan sonra Azure portalındaki **EventHub** ayarlarını görüntüleyin ve **Paylaşılan erişim ilkelerini**seçin. Aşağıdaki ilkeleri oluşturmak için **+ Ekle'yi** seçin:

   | Adı | İzinler |
   | --- | --- |
   | Yazar |Gönder |
   | Okuyucu |Dinle |

    ![Erişim ilkeleri paylaş penceresinin ekran görüntüsü](./media/apache-storm-develop-csharp-event-hub-topology/share-access-policies.png)

2. **Okuyucu** ve **yazar** ilkelerini seçin. Bu değerler daha sonra kullanıldığından, her iki ilkenin birincil anahtar değerini kopyalayın ve kaydedin.

## <a name="configure-the-eventhubwriter"></a>EventHubWriter'ı yapılandırın

1. Visual Studio için HDInsight araçlarının en son sürümünü henüz yüklemediyseniz, [Bkz. Visual Studio için HDInsight araçlarını kullanmaya başlayın.](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)

2. [Eventhub-storm-hybrid'den](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub)çözümü indirin.

3. **Açık EventHubExample.sln**. **EventHubWriter** projesinde **App.config** dosyasını açın. Aşağıdaki anahtarların değerini doldurmak için daha önce yapılandırdığınız olay merkezinden gelen bilgileri kullanın:

   | Anahtar | Değer |
   | --- | --- |
   | EventHubPolicyName |yazar *(İzle* izniyle ilke için farklı bir ad kullandıysanız, bunun yerine kullanın.) |
   | EventHubPolicyKey |Yazar politikasının anahtarı. |
   | EventHubNamespace |Olay hub'ınızı içeren ad alanı. |
   | EventHubName |Olay hub adınız. |
   | EventHubPartitionCount |Olay merkezinizdeki bölüm sayısı. |

4. **App.config** dosyasını kaydedin ve kapatın.

## <a name="configure-the-eventhubreader"></a>EventHubReader'ı yapılandırın

1. **EventHubReader** projesini açın.

2. **EventHubReader**için **App.config** dosyasını açın. Aşağıdaki anahtarların değerini doldurmak için daha önce yapılandırdığınız olay merkezinden gelen bilgileri kullanın:

   | Anahtar | Değer |
   | --- | --- |
   | EventHubPolicyName |okuyucu *(Dinleme* izniyle ilke için farklı bir ad kullandıysanız, bunun yerine kullanın.) |
   | EventHubPolicyKey |Okuyucu ilkesiiçin anahtar. |
   | EventHubNamespace |Olay hub'ınızı içeren ad alanı. |
   | EventHubName |Olay hub adınız. |
   | EventHubPartitionCount |Olay merkezinizdeki bölüm sayısı. |

3. **App.config** dosyasını kaydedin ve kapatın.

## <a name="deploy-the-topologies"></a>Topolojileri dağıtın

1. **Solution Explorer'dan** **EventHubReader** projesine sağ tıklayın ve **HDInsight'ta Fırtınaya Gönder'i**seçin.

    ![HDInsight'ta Storm'a Gönder vurgulanan Solution Explorer ekran görüntüsü](./media/apache-storm-develop-csharp-event-hub-topology/submit-to-apache-storm.png)

2. **Topoloji Gönder** iletişim kutusunda Fırtına **Kümesi'ni**seçin. **Ek Yapılandırmaları**Genişletin, **Java Dosya Yolları'nı**seçin, **...**'u seçin ve daha önce indirdiğiniz JAR dosyasını içeren dizini seçin. Son olarak, **Gönder'i**tıklatın.

    ![Topoloji Gönder iletişim kutusunun ekran görüntüsü](./media/apache-storm-develop-csharp-event-hub-topology/submit-storm-topology.png)

3. Topoloji sunulduğunda, Fırtına **Topolojileri Görüntüleyici** görüntülenir. Topoloji hakkındaki bilgileri görüntülemek için sol bölmedeki **EventHubReader** topolojisini seçin.

    ![Fırtına Topolojileri Görüntüleyici ekran görüntüsü](./media/apache-storm-develop-csharp-event-hub-topology/storm-topology-viewer.png)

4. **Solution Explorer'dan** **EventHubWriter** projesine sağ tıklayın ve **HDInsight'ta Fırtınaya Gönder'i**seçin.

5. **Topoloji Gönder** iletişim kutusunda Fırtına **Kümesi'ni**seçin. **Ek Yapılandırmaları**Genişletin, **Java Dosya Yolları'nı**seçin, **...** seçin ve daha önce indirdiğiniz JAR dosyasını içeren dizini seçin. Son olarak, **Gönder'i**tıklatın.

6. Topoloji gönderildiğinde, her iki topolojinin kümeüzerinde çalıştığını doğrulamak için **Storm Topologies Viewer'daki** topoloji listesini yenileyin.

7. **Storm Topologies Viewer'da** **EventHubReader** topolojisini seçin.

8. Cıvata için bileşen özetini açmak için diyagramdaki **LogBolt** bileşenini çift tıklatın.

9. **Uygulayıcılar** bölümünde, **Bağlantı Noktası** sütunundaki bağlantılardan birini seçin. Bu, bileşen tarafından günlüğe kaydedilen bilgileri görüntüler. Günlüğe kaydedilen bilgiler aşağıdaki metne benzer:

    ```output
    2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
    2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
    2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}
    ```

## <a name="stop-the-topologies"></a>Topolojileri durdurma

Topolojileri durdurmak için, **Storm Topology Viewer'daki**her topolojiyi seçin, ardından **Öldür'e**tıklayın.

![Storm Topology Viewer ekran görüntüsü, Kill düğmesi vurgulanmış](./media/apache-storm-develop-csharp-event-hub-topology/kill-storm-topology1.png)

## <a name="delete-your-cluster"></a>Kümenizi silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Olay Hub'larında verilerle çalışmak için Bir C# topolojisinden Java Event Hub'larını ve cıvatayı kullanmayı öğrendiniz. C# topolojileri oluşturma hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Visual Studio'yu kullanarak HDInsight'ta Apache Storm için C# topolojileri geliştirin](apache-storm-develop-csharp-visual-studio-topology.md)
* [SCP programlama kılavuzu](apache-storm-scp-programming-guide.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)
