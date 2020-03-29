---
title: Kafka'dan Azure Veri Gezgini'ne veri alma
description: Bu makalede, Kafka'dan Azure Veri Gezgini'ne nasıl veri öttürecek (yüklenir) öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 03b46ff50683149a22c71ccb155480a0f08455bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66497278"
---
# <a name="ingest-data-from-kafka-into-azure-data-explorer"></a>Kafka'dan Azure Veri Gezgini'ne veri alma
 
Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Data Explorer Kafka'dan alım (veri yükleme) sunar. Kafka, verileri sistemler veya uygulamalar arasında güvenilir bir şekilde hareket ettiren gerçek zamanlı akış veri boru hatlarının oluşturulmasına olanak tanıyan dağıtılmış bir akış platformudur.
 
## <a name="prerequisites"></a>Ön koşullar
 
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun. 
 
* [Bir test kümesi ve veritabanı.](create-cluster-database-portal.md)
 
* Veri üreten ve Kafka'ya gönderen örnek bir [uygulama.](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/kafka)

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) örnek uygulamayı çalıştırmak için.
 
## <a name="kafka-connector-setup"></a>Kafka konektör kurulumu

Kafka Connect, Apache Kafka ve diğer sistemler arasında ölçeklenebilir ve güvenilir veri akışı için bir araçtır. Kafka'ya ve Kafka'nın dışına büyük veri koleksiyonlarını hareket ettiren konektörleri hızlı bir şekilde tanımlamayı kolaylaştırır. ADX Kafka Lavabo Kafka konektörü olarak hizmet vermektedir.
 
### <a name="bundle"></a>Paket

Kafka özel `.jar` bir konektör olarak hareket edecek bir eklenti olarak bir yük olabilir. Böyle bir `.jar`üretmek için, biz yerel kodu klonlamak ve Maven kullanarak inşa edecektir. 

#### <a name="clone"></a>Kopyalama

```bash
git clone git://github.com:Azure/kafka-sink-azure-kusto.git
cd ./kafka-sink-azure-kusto/kafka/
```

#### <a name="build"></a>Oluşturma

Bağımlılıklarla birlikte eksiksiz bir `.jar` ürün üretmek için Maven ile yerel olarak oluşturun.

* JDK >= 1.8 [indir](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Maven [indir](https://maven.apache.org/install.html)
 

Kök dizini *kafka-sink-azure-kusto*içinde , çalıştırın:

```bash
mvn clean compile assembly:single
```

### <a name="deploy"></a>Dağıtma 

Kafka'ya yük eklentisi. Docker kullanarak bir dağıtım örneği [kafka-sink-azure-kusto](https://github.com/Azure/kafka-sink-azure-kusto#deploy) bulunabilir
 

Kafka konektörleri ve nasıl dağıtılabilenlerle ilgili detaylı dokümantasyona [Kafka Connect'te](https://kafka.apache.org/documentation/#connect) bulunabilir. 

### <a name="example-configuration"></a>Örnek yapılandırma 
 
```config
name=KustoSinkConnector 
connector.class=com.microsoft.azure.kusto.kafka.connect.sink.KustoSinkConnector 
kusto.sink.flush_interval_ms=300000 
key.converter=org.apache.kafka.connect.storage.StringConverter 
value.converter=org.apache.kafka.connect.storage.StringConverter 
tasks.max=1 
topics=testing1 
kusto.tables.topics_mapping=[{'topic': 'testing1','db': 'daniel', 'table': 'TestTable','format': 'json', 'mapping':'TestMapping'}] 
kusto.auth.authority=XXX 
kusto.url=https://ingest-{mycluster}.kusto.windows.net/ 
kusto.auth.appid=XXX 
kusto.auth.appkey=XXX 
kusto.sink.tempdir=/var/tmp/ 
kusto.sink.flush_size=1000
```
 
## <a name="create-a-target-table-in-adx"></a>ADX'te hedef tablo oluşturma
 
ADX'te Kafka'nın veri gönderebileceği bir tablo oluşturun. **Önkoşullar'da**verilen küme ve veritabanında tablooluşturun.
 
1. Azure portalında kümenize gidin ve **Sorgula'yı**seçin.
 
    ![Sorgu uygulama bağlantısı](media/ingest-data-event-hub/query-explorer-link.png)
 
1. Aşağıdaki komutu pencereye kopyalayıp **Çalıştır**'ı seçin.
 
    ```Kusto
    .create table TestTable (TimeStamp: datetime, Name: string, Metric: int, Source:string)
    ```
 
    ![Oluşturma sorgusunu çalıştırma](media/ingest-data-event-hub/run-create-query.png)
 
1. Aşağıdaki komutu pencereye kopyalayıp **Çalıştır**'ı seçin.
 
    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.timeStamp","datatype":"datetime"},{"column":"Name","path":"$.name","datatype":"string"},{"column":"Metric","path":"$.metric","datatype":"int"},{"column":"Source","path":"$.source","datatype":"string"}]'
    ```

    Bu komut gelen JSON verilerini tablonun (TestTable) sütun adları ve veri türleriyle eşler.


## <a name="generate-sample-data"></a>Örnek veri oluşturma

Kafka kümesi ADX'e bağlı olduğuna göre, veri oluşturmak için indirdiğiniz [örnek uygulamayı](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) kullanın.

### <a name="clone"></a>Kopyalama

Örnek uygulamayı yerel olarak klonla:

```cmd
git clone git://github.com:Azure/azure-kusto-samples-dotnet.git
cd ./azure-kusto-samples-dotnet/kafka/
```

### <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Örnek uygulama çözümünü Visual Studio'da açın.

1. Dosyada, `Program.cs` sabiti `connectionString` Kafka bağlantı dizenize güncelleştirin.

    ```csharp    
    const string connectionString = @"<YourConnectionString>";
    ```

1. Uygulamayı derleyin ve çalıştırın. Uygulama Kafka kümesine mesaj gönderir ve durumunu her 10 saniyede bir yazdırır.

1. Uygulama birkaç mesaj gönderdikten sonra bir sonraki adıma geçin.
 
## <a name="query-and-review-the-data"></a>Verileri sorgula ve gözden geçirin

1. Yutma sırasında hata oluşmadığından emin olmak için:

    ```Kusto
    .show ingestion failures
    ```

1. Yeni alınan verileri görmek için:

    ```Kusto
    TestTable 
    | count
    ```

1. İletilerin içeriğini görmek için:
 
    ```Kusto
    TestTable
    ```
 
    Sonuç kümesi aşağıdaki gibi görünmelidir:
 
    ![İleti sonuç kümesi](media/ingest-data-event-hub/message-result-set.png)
 
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure Veri Gezgini'nde verileri sorgula](web-query-data.md)
