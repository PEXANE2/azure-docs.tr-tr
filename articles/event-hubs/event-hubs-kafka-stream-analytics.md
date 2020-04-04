---
title: Azure Etkinlik Hub'ları - Apache Kafka etkinliklerini işleme
description: "Öğretici: Bu makalede, Azure Akış Analitiği kullanılarak etkinlik hub'ları aracılığıyla yutulan Kafka olaylarının nasıl işlendiği gösterilmektedir"
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
ms.date: 04/02/2020
ms.author: spelluru
ms.openlocfilehash: 9c678a91b88b87acb438311b4968be4cae46733b
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632799"
---
# <a name="tutorial-process-apache-kafka-for-event-hubs-events-using-stream-analytics"></a>Öğretici: Akış analizini kullanarak Etkinlik Hub'ları etkinlikleri için Apache Kafka'yı işleme 
Bu makalede, veri lerin Olay Hub'larına nasıl aktarılacağa ve Azure Akış Analitiği ile nasıl işlenirken işlenmeyi gösterilmektedir. Size aşağıdaki adımları ilerehberlik edin: 

1. Bir Event Hubs ad alanı oluşturun.
2. Olay merkezine ileti gönderen bir Kafka istemcisi oluşturun.
3. Etkinlik merkezindeki verileri Azure blob depolama alanına kopyalayan bir Akış Analizi işi oluşturun. 

Bir olay hub'ı tarafından açığa çıkarılan Kafka uç noktasını kullandığınızda protokol istemcilerinizi değiştirmeniz veya kendi kümelerinizi çalıştırmanız gerekmez. Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler. ve yukarıda. 


## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Geliştirme Kiti (JDK) 1.7+](https://aka.ms/azure-jdks).
* Bir Maven ikili arşivini [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)
* Bir **Azure Depolama hesabı**. Eğer yoksa, daha fazla devam etmeden önce [bir tane oluşturun.](../storage/common/storage-account-create.md) Bu gözden geçirmedeki Akış Analizi işi, çıktı verilerini bir Azure blob depolama alanında depolar. 


## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma
**Standart** bir katman Olay Hub'ları ad alanı oluşturduğunuzda, ad alanı için Kafka bitiş noktası otomatik olarak etkinleştirilir. Kafka protokolünü kullanan uygulamalarınızdan olayları standart katman Etkinlik Hub'larına aktarabilirsiniz. **Standart** bir katman Olay Hub'ları ad alanı oluşturmak için [Azure portalını kullanarak etkinlik hub'ı oluştur'da](event-hubs-create.md) adım adım yönergeleri izleyin. 

> [!NOTE]
> Kafka için Etkinlik Hub'ları yalnızca **standart** ve **özel** katmanlarda kullanılabilir. **Temel** katman Etkinlik Hub'larında Kafka'yı desteklemez.

## <a name="send-messages-with-kafka-in-event-hubs"></a>Etkinlik Hub'larında Kafka ile mesaj gönderin

1. [Kafka deposu için Azure Etkinlik Hub'larını](https://github.com/Azure/azure-event-hubs-for-kafka) makinenize kopyala.
2. Klasöre gidin: `azure-event-hubs-for-kafka/quickstart/java/producer`. 
4. Üreticinin yapılandırma ayrıntılarını `src/main/resources/producer.config`güncelleştirin. **Olay göbeği ad alanının** **adını** ve **bağlantı dizesini** belirtin. 

    ```xml
    bootstrap.servers={EVENT HUB NAMESPACE}.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING for EVENT HUB NAMESPACE}";
    ```

5. `azure-event-hubs-for-kafka/quickstart/java/producer/src/main/java/`Seçtiğiniz bir düzenleyicide **TestDataReporter.java** dosyasına gidin ve açın. 
6. Aşağıdaki kod satırına yorum yapın:

    ```java
                //final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "Test Data " + i);
    ```
3. Yorumlanan kodun yerine aşağıdaki kod satırını ekleyin: 

    ```java
                final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "{ \"eventData\": \"Test Data " + i + "\" }");            
    ```

    Bu kod olay verilerini **JSON** biçiminde gönderir. Bir Akış Analizi işi için giriş yapılandırırken, giriş verilerinin biçimi olarak JSON'u belirtirsiniz. 
7. **Yapımcıyı çalıştırın** ve Olay Hub'larına aktarın. Bir Windows makinesinde, **Bir Düğüm.js komut istemi** `azure-event-hubs-for-kafka/quickstart/java/producer` kullanırken, bu komutları çalıştırmadan önce klasöre geçin. 
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```

## <a name="verify-that-event-hub-receives-the-data"></a>Olay merkezinin verileri aldığını doğrulayın

1. **VARLıKLAR**altında **Olay Hub'larını** seçin. **Test**adlı bir olay hub'ı gördüğünüze onaylayın. 

    ![Olay merkezi - test](./media/event-hubs-kafka-stream-analytics/test-event-hub.png)
2. Etkinlik merkezine gelen iletileri gördüğünüzden onaylayın. 

    ![Olay merkezi - iletiler](./media/event-hubs-kafka-stream-analytics/confirm-event-hub-messages.png)

## <a name="process-event-data-using-a-stream-analytics-job"></a>Akış Analizi işini kullanarak olay verilerini işleme
Bu bölümde, bir Azure Akışı Analizi işi oluşturursunuz. Kafka istemcisi olayları etkinlik merkezine gönderir. Olay verilerini giriş olarak alan ve azure blob depolamasına çıkaran bir Akış Analizi işi oluşturursunuz. **Azure Depolama hesabınız**yoksa, bir [hesap oluşturun.](../storage/common/storage-account-create.md)

Akış Analizi işindeki sorgu, herhangi bir analiz yapmadan verilerden geçer. Çıktı verilerini farklı bir biçimde veya elde edilmiş öngörülerle oluşturmak için giriş verilerini dönüştüren bir sorgu oluşturabilirsiniz.  

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma 

1. Select + [Azure portalında](https://portal.azure.com) **kaynak oluşturun.**
2. **Azure Marketi** menüsünde **Analytics'i** seçin ve **Akış Analizi işini**seçin. 
3. Yeni **Akış Analizi** sayfasında aşağıdaki işlemleri yapın: 
    1. İş için bir **ad** girin. 
    2. **Aboneliğinizi**seçin.
    3. Kaynak grubu için **yeni oluştur'u** seçin ve adı girin. **resource group** Varolan bir kaynak grubunu da **kullanabilirsiniz.** 
    4. İş için bir **konum** seçin.
    5. İşi oluşturmak için **Oluştur'u** seçin. 

        ![Yeni Stream Analytics işi](./media/event-hubs-kafka-stream-analytics/new-stream-analytics-job.png)

### <a name="configure-job-input"></a>İş girişi yapılandırma

1. Bildirim iletisinde, **Akış Analizi iş** sayfasını görmek için **kaynağa Git'i** seçin. 
2. Sol menüdeki **JOB TOPOLOGY** bölümündeki **Girişleri** seçin.
3. **Akış girişi ekle'yi**seçin ve ardından **Olay Hub'ını**seçin. 

    ![Giriş olarak olay hub'ı ekleme](./media/event-hubs-kafka-stream-analytics/select-event-hub-input.png)
4. Olay **Hub'ı giriş** yapılandırma sayfasında aşağıdaki işlemleri yapın: 

    1. Giriş için bir **takma ad** belirtin. 
    2. Azure **aboneliğinizi**seçin.
    3. Daha önce oluşturduğunuz **etkinlik merkezi ad alanını** seçin. 
    4. **Olay merkezi**için **test** seçin. 
    5. **Kaydet'i**seçin. 

        ![Olay merkezi giriş yapılandırması](./media/event-hubs-kafka-stream-analytics/event-hub-input-configuration.png)

### <a name="configure-job-output"></a>İş çıkışını yapılandırma 

1. Menüdeki JOB **TOPOLOGY** bölümünde **çıktıları** seçin. 
2. Araç çubuğuna **+ Ekle'yi** seçin ve **Blob depolama alanını** seçin
3. Blob depolama çıktı ayarları sayfasında aşağıdaki işlemleri yapın: 
    1. Çıktı için bir **takma ad** belirtin. 
    2. Azure **aboneliğinizi**seçin. 
    3. Azure **Depolama hesabınızı**seçin. 
    4. Akış Analizi sorgusundaki çıktı verilerini depolayan **kapsayıcıiçin** bir ad girin.
    5. **Kaydet'i**seçin.

        ![Blob Depolama çıktı yapılandırması](./media/event-hubs-kafka-stream-analytics/output-blob-settings.png)
 

### <a name="define-a-query"></a>Sorgu tanımlama
Gelen bir veri akışını okumak için bir Stream Analytics işi ayarladıktan sonraki adım, verileri gerçek zamanlı olarak analiz eden bir dönüştürme oluşturmaktır. Dönüştürme sorgusunu [Stream Analytics sorgu dilini](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) kullanarak tanımlarsınız. Bu gözden geçirmede, herhangi bir dönüşüm gerçekleştirmeden verilerden geçen bir sorgu tanımlarsınız.

1. **Sorgu'u**seçin.
2. Sorgu penceresinde, daha `[YourOutputAlias]` önce oluşturduğunuz çıktı diğer adını değiştirin.
3. Daha `[YourInputAlias]` önce oluşturduğunuz giriş takma adlarıyla değiştirin. 
4. Araç çubuğunda **Kaydet**’i seçin. 

    ![Sorgu](./media/event-hubs-kafka-stream-analytics/query.png)


### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

1. Sol menüde **Genel Bakış'ı** seçin. 
2. **Başlat**'ı seçin. 

    ![Başlat menüsü](./media/event-hubs-kafka-stream-analytics/start-menu.png)
1. İş **başlat** sayfasında **Başlat'ı**seçin. 

    ![İş sayfasını başlat](./media/event-hubs-kafka-stream-analytics/start-job-page.png)
1. **Çalıştırmaya başlamadan** işin durumu **değişene**kadar bekleyin. 

    ![İş durumu - çalışan](./media/event-hubs-kafka-stream-analytics/running.png)

## <a name="test-the-scenario"></a>Senaryoyu test edin
1. Etkinlik merkezine etkinlik göndermek için **Kafka yapımcısını** tekrar çalıştırın. 

    ```shell
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
1. **Azure blob depolama**alanında **çıktı verilerinin** oluşturulduğunu gördüğünüzü doğrulayın. Aşağıdaki örnek satırlara benzeyen 100 satıriçeren kapsayıcıda bir JSON dosyası görürsünüz: 

    ```
    {"eventData":"Test Data 0","EventProcessedUtcTime":"2018-08-30T03:27:23.1592910Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 1","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 2","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    ```

    Azure Akış Analizi işi, olay merkezinden giriş verilerini aldı ve bu senaryoda Azure blob depolama alanında depolandı. 



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Olay Hub'larına nasıl akış yapılacağını öğrendiniz. Apache Kafka için Etkinlik Hub'ları hakkında daha fazla bilgi edinmek [için Azure Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzuna](apache-kafka-developer-guide.md)bakın. 