---
title: Azure Event Hubs-Işlem Apache Kafka olayları
description: "Öğretici: Bu makalede Azure Stream Analytics kullanılarak Olay Hub 'ları aracılığıyla alınan Kafka olaylarının nasıl işlenmesi gösterilmektedir."
services: event-hubs
documentationcenter: ''
author: spelluru
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: d7b060a2b35ca41bf87b69be706284174d7b1012
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587167"
---
# <a name="tutorial-process-apache-kafka-for-event-hubs-events-using-stream-analytics"></a>Öğretici: Stream Analytics kullanarak Event Hubs olaylar için Işlem Apache Kafka 
Bu makalede, veri akışı Kafka özellikli Event Hubs'a ve Azure Stream Analytics ile işlemek gösterilmektedir. Aşağıdaki adımları gösterilmektedir: 

1. Oluşturma bir Kafka Event Hubs ad alanı etkin.
2. Olay hub'ına ileti gönderen bir Kafka istemci oluşturun.
3. Olay hub'ından bir Azure blob depolama alanına veri kopyalayan bir Stream Analytics işi oluşturun. 

Protokol istemcilerinize değiştirmek veya bir olay hub'ı tarafından kullanıma sunulan Kafka uç noktası kullandığınızda, kendi kümeleri çalıştırın gerekmez. Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler. ve üstü. 


## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks).
* Bir Maven ikili arşivini [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)
* Bir **Azure depolama hesabı**. Yoksa, devam etmeden önce [bir tane oluşturun](../storage/common/storage-account-create.md) . Bu izlenecek yolda Stream Analytics işi çıktı verilerini bir Azure blob depolama alanında depolar. 


## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Kafka etkin Event Hubs ad alanı oluşturma
Bir **Standart** katman Event Hubs ad alanı oluşturduğunuzda, ad alanı için Kafka uç noktası otomatik olarak etkinleştirilir. Kafka protokolünü kullanan uygulamalarınızdan olayları Standart katman Event Hubs akışı yapabilirsiniz. **Standart** katman Event Hubs ad alanı oluşturmak için [Azure Portal kullanarak Olay Hub 'ı oluşturma](event-hubs-create.md) bölümünde adım adım yönergeleri izleyin. 

> [!NOTE]
> Kafka için Event Hubs yalnızca **Standart** ve **adanmış** katmanlarda kullanılabilir. **Temel** katman Event Hubs Kafka desteklemez.

## <a name="send-messages-with-kafka-in-event-hubs"></a>Olay hub'ları, Kafka ile iletileri gönder

1. [Azure Event Hubs for Kafka Repository](https://github.com/Azure/azure-event-hubs-for-kafka) 'yi makinenize kopyalayın.
2. Şu klasöre gidin: `azure-event-hubs-for-kafka/quickstart/java/producer`. 
4. `src/main/resources/producer.config`' de üreticinin yapılandırma ayrıntılarını güncelleştirin. **Olay Hub 'ı ad alanı**için **adı** ve **bağlantı dizesini** belirtin. 

    ```xml
    bootstrap.servers={EVENT HUB NAMESPACE}.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING for EVENT HUB NAMESPACE}";
    ```

5. `azure-event-hubs-for-kafka/quickstart/java/producer/src/main/java/com/example/app`gidin ve **Testdatareporter. Java** dosyasını seçtiğiniz bir düzenleyicide açın. 
6. Aşağıdaki kod satırını açıklama satırı yapın:

    ```java
                //final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "Test Data " + i);
    ```
3. Aşağıdaki açıklama eklenen kod yerine kod satırını ekleyin: 

    ```java
                final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "{ \"eventData\": \"Test Data " + i + "\" }");            
    ```

    Bu kod, olay verilerini **JSON** biçiminde gönderir. Stream Analytics işine ilişkin giriş yapılandırdığınızda, giriş veri biçimi olarak JSON belirtin. 
7. **Producer** ve Stream 'i Kafka özellikli Event Hubs olarak çalıştırın. Bir Windows makinesinde, **Node. js komut istemi**kullanırken, bu komutları çalıştırmadan önce `azure-event-hubs-for-kafka/quickstart/java/producer` klasörüne geçin. 
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```

## <a name="verify-that-event-hub-receives-the-data"></a>Bu olay hub'ın verileri alan doğrulayın

1. **Varlıklar**altında **Event Hubs** seçin. **Test**adlı bir olay hub 'ı görtığınızdan emin olun. 

    ![Olay hub'ı - test etme](./media/event-hubs-kafka-stream-analytics/test-event-hub.png)
2. Olay hub'ına gelen iletileri gördüğünüzü onaylayın. 

    ![Olay hub'ı - iletileri](./media/event-hubs-kafka-stream-analytics/confirm-event-hub-messages.png)

## <a name="process-event-data-using-a-stream-analytics-job"></a>Bir Stream Analytics işi kullanarak olay verilerini işleme
Bu bölümde, Azure Stream Analytics işi oluşturun. Kafka istemci, olayları olay hub'ına gönderir. Olay verileri girdi olarak alır ve bir Azure blob depolama alanına çıkaran bir Stream Analytics işi oluşturun. **Azure depolama hesabınız**yoksa [bir tane oluşturun](../storage/common/storage-account-create.md).

Stream Analytics işinde sorgu, herhangi bir analiz yapmadan aracılığıyla verileri geçirir. Farklı bir biçimde veya elde edildi öngörülerle çıkış verileri üretmek üzere giriş verilerini dönüştüren bir sorgu oluşturabilirsiniz.  

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma 

1. Azure portal **+ kaynak oluştur** ' u seçin [](https://portal.azure.com).
2. **Azure Marketi** menüsünde **analiz** ' i seçin ve **Stream Analytics iş**' ı seçin. 
3. **Yeni Stream Analytics** sayfasında aşağıdaki işlemleri yapın: 
    1. İş için bir **ad** girin. 
    2. **Aboneliğinizi**seçin.
    3. **Kaynak grubu** Için **Yeni oluştur** ' u seçin ve adı girin. **Var olan bir** kaynak grubunu da kullanabilirsiniz. 
    4. İş için bir **konum** seçin.
    5. İşi oluşturmak için **Oluştur** ' u seçin. 

        ![Yeni Stream Analytics işi](./media/event-hubs-kafka-stream-analytics/new-stream-analytics-job.png)

### <a name="configure-job-input"></a>İş girişi yapılandırma

1. Bildirim iletisinde, **Stream Analytics işi** sayfasını görmek Için **Kaynağa Git** ' i seçin. 
2. Sol menüdeki **Iş topolojisi** bölümünde **girişler** ' i seçin.
3. **Akış girişi Ekle**' yi ve ardından **Olay Hub**'ı ' nı seçin. 

    ![Girdi olarak Event hub'ı Ekle](./media/event-hubs-kafka-stream-analytics/select-event-hub-input.png)
4. **Olay Hub 'ı giriş** yapılandırması sayfasında, aşağıdaki işlemleri yapın: 

    1. Giriş için bir **diğer ad** belirtin. 
    2. Azure **aboneliğinizi** seçin.
    3. Daha önce oluşturduğunuz **Olay Hub 'ı ad alanını** seçin. 
    4. **Olay Hub 'ı**için **Test** ' i seçin. 
    5. **Kaydet**’i seçin. 

        ![Olay hub'ı giriş yapılandırma](./media/event-hubs-kafka-stream-analytics/event-hub-input-configuration.png)

### <a name="configure-job-output"></a>İş çıkışını yapılandırma 

1. Menüdeki **Iş topolojisi** bölümünde yer alan **çıktılar** ' i seçin. 
2. Araç çubuğunda **+ Ekle** ' yi seçin ve **BLOB depolama** ' yı seçin.
3. Blob Depolama çıkış Ayarları sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. Çıkış için bir **diğer ad** belirtin. 
    2. Azure **aboneliğinizi** seçin. 
    3. **Azure depolama hesabınızı**seçin. 
    4. Stream Analytics sorgusundan çıkış verilerini depolayan **kapsayıcı için bir ad** girin.
    5. **Kaydet**’i seçin.

        ![BLOB Depolama çıktı yapılandırma](./media/event-hubs-kafka-stream-analytics/output-blob-settings.png)
 

### <a name="define-a-query"></a>Bir sorgu tanımlama
Gelen bir veri akışını okumak için bir Stream Analytics işi ayarladıktan sonraki adım, verileri gerçek zamanlı olarak analiz eden bir dönüştürme oluşturmaktır. Dönüştürme sorgusunu [Stream Analytics sorgu dilini](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) kullanarak tanımlarsınız. Bu izlenecek yolda, herhangi bir dönüştürme gerçekleştirmeden veri aktaran bir sorgu tanımlayabilirsiniz.

1. **Sorgu**seçin.
2. Sorgu penceresinde, `[YourOutputAlias]` daha önce oluşturduğunuz çıktı diğer adıyla değiştirin.
3. `[YourInputAlias]`, daha önce oluşturduğunuz giriş diğer adıyla değiştirin. 
4. Araç çubuğunda **Kaydet**’i seçin. 

    ![Sorgu](./media/event-hubs-kafka-stream-analytics/query.png)


### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

1. Sol menüdeki **Genel Bakış ' ı** seçin. 
2. **Başlat**' ı seçin. 

    ![Başlat menüsü](./media/event-hubs-kafka-stream-analytics/start-menu.png)
1. **Işi Başlat** sayfasında **Başlat**' ı seçin. 

    ![Başlangıç işi sayfası](./media/event-hubs-kafka-stream-analytics/start-job-page.png)
1. İşin durumunun **çalışmaya** **başlamasını** bekleyin. 

    ![İş durumu - çalışan](./media/event-hubs-kafka-stream-analytics/running.png)

## <a name="test-the-scenario"></a>Test senaryosu
1. Olayları Olay Hub 'ına göndermek için **Kafka üreticisi** 'ni yeniden çalıştırın. 

    ```shell
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
1. **Azure Blob depolamada** **çıktı verilerinin** oluşturulduğunu görtığınızdan emin olun. Aşağıdaki örnek satırlar gibi ara 100 satır kapsayıcıdaki bir JSON dosyası görürsünüz: 

    ```
    {"eventData":"Test Data 0","EventProcessedUtcTime":"2018-08-30T03:27:23.1592910Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 1","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 2","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    ```

    Azure Stream Analytics işi, giriş verileri olay hub'ından alınan ve bu senaryoda Azure blob depolamada depolanan. 



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturacağınızı öğrendiniz. Event Hubs ve Kafka için Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki konuya bakın:  

- [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
- [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md)
- [Kafka özellikli Event Hubs oluşturma](event-hubs-create-kafka-enabled.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Kafka özellikli bir olay hub'ında Kafka aracısını yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i Kafka özellikli olay hub'ıyla tümleştirme](event-hubs-kafka-connect-tutorial.md)
- [Akka Streams’i Kafka özellikli olay hub'ına bağlama](event-hubs-kafka-akka-streams-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka) 
