---
title: 'Hızlı başlangıç: Spark ile çözümlemeye başlama'
description: Bu öğreticide, Apache Spark ile verileri çözümlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 3b5f5d64498922e9fc35942ff4570d801aa6c516
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98118889"
---
# <a name="analyze-with-apache-spark"></a>Apache Spark ile Çözümle

Bu öğreticide, Azure SYNAPSE için Apache Spark verileri yüklemek ve analiz etmek için temel adımları öğreneceksiniz.

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Spark kullanarak, blob depolamada NYC TAXI verilerini çözümleme

1. **Veri** hub 'ında, **Galeriye**> **Yeni Kaynak Ekle** (Ayrıca, **bağlı** düğme) öğesini seçin.
1. **NYC taxı & Limousine Commission-sarı TAXI seyahat kayıtları**' nı seçin.
1. Sayfanın alt kısmında,   >  **veri kümesi Ekle** devam ' ı seçin.
1. **Bağlı** altındaki **veri** hub 'ında **Azure Blob depolama**' yı sağ tıklatın ve ardından **örnek veri kümeleri**  >  **nyc_tlc_yellow** seçin.
1. Aşağıdaki kodla yeni bir not defteri oluşturmak için **Yeni Not defteri** ' ni seçin:

    ```py
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    display(data_df.limit(10))
    ```

1. Not defterindeki **Ekle** menüsünde sunucusuz Spark havuzu ' nu seçin.
1. Hücrede **Çalıştır** ' ı seçin.
1. Yalnızca veri çerçevesinin şemasını görmek istiyorsanız aşağıdaki kodla bir hücre çalıştırın:

    ```py
    data_df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>NYC TAXI verilerini Spark nyctaxi veritabanına yükleme

Veriler, **SQLPOOL1** içindeki bir tabloda kullanılabilir. **Nyctaxi** adlı bir Spark veritabanına yükleyin.

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin.
1. **+**  >  **Not defteri** seçin.
1. Not defterinin en üstünde, **Attach** değerini **Spark1** olarak ayarlayın.
1. **Kod Ekle** ' yi seçerek bir not defteri kod hücresi ekleyin ve ardından şu metni girin:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLPOOL1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Hücrede **Çalıştır** ' ı seçin.
1. **Veri** hub 'ında **veritabanları**' na sağ tıklayın ve ardından **Yenile**' yi seçin. Şu veritabanlarını görmeniz gerekir:

    - **SQLPOOL1 (SQL)**
    - **nyctaxi (Spark)**

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

1. Not defterinize geri dönün.
1. Yeni bir kod hücresi oluşturun ve aşağıdaki metni girin.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. **Nyctaxi** Spark veritabanına yüklediğiniz NYC TAXI verilerini göstermek için hücreyi çalıştırın.
1. Daha önce adanmış SQL havuzu **SQLPOOL1** ile yaptığınız aynı analizi yapmak için aşağıdaki kodu çalıştırın. Bu kod, çözümlemenin sonuçlarını **nyctaxi. passengercountstats** adlı bir tabloya kaydeder ve görüntüler.

   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```

1. Hücre sonuçlarında, görselleştirilen verileri görmek için **grafik** ' i seçin.

## <a name="load-data-from-a-spark-table-into-a-dedicated-sql-pool-table"></a>Spark tablosundan adanmış bir SQL havuzu tablosuna veri yükleme

Daha önce, **SQLPOOL1. dbo. yolculuğa** YÖNELIK adanmış SQL havuzu tablosundan verileri Spark tablosu **nyctaxi. seyahat** içine kopyaladınız. Daha sonra verileri Spark tablosu **nyctaxi. passengercountstats** içine yığıldıktan sonra. Artık **nyctaxi. passengercountstats** Içindeki verileri **SQLPOOL1. dbo. passengercountstats** adlı özel bir SQL havuzu tablosuna kopyalayacaksınız.

Not defterinizde aşağıdaki hücreyi çalıştırın. Toplu Spark tablosunu adanmış SQL havuzu tablosuna kopyalar.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLPOOL1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sunucusuz SQL havuzu ile verileri analiz etme](get-started-analyze-sql-on-demand.md)
