---
title: 'Hızlı başlangıç: Spark ile çözümlemeye başlama'
description: Bu öğreticide, Apache Spark ile verileri çözümlemeyi öğreneceksiniz
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: ee4dc945f63180fd06f13287b22949d0ac1e3873
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96862029"
---
# <a name="analyze-with-apache-spark"></a>Apache Spark ile Çözümle

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Spark kullanarak, blob depolamada NYC TAXI verilerini çözümleme

Bu öğreticide, Azure SYNAPSE için Apache Spark verileri yüklemek ve analiz etmek için temel adımları öğreneceksiniz.

1. **Veri** hub 'ında **Yeni Kaynak Ekle**' ye tıklayın (Ayrıca, **bağlı** düğme)  >> **Galeriye** tıklayın. 
1. **NYC taxı & Limousine Komisyonu bulun-sarı TAXI seyahat kayıtları** bulun ve üzerine tıklayın. 
1. Sayfanın alt kısmındaki **devam** ' a ve ardından **veri kümesi Ekle**' ye basın. 
1. Artık **veri** hub 'ında **bağlı** sağ ' ye tıklayın **>> örnek veri kümeleri >> nyc_tlc_yellow** ve **Yeni Not defteri** ' ni seçin.
1. Bu, aşağıdaki kod ile yeni bir not defteri oluşturur:
    ```
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    display(data_df.limit(10))
    ```
1. Not defterinde, **Ekle** menüsünde sunucusuz Spark havuzu seçin
1. Hücrede **Çalıştır** 'ı seçin
1. Yalnızca veri çerçevesinin şemasını görmek istiyorsanız aşağıdaki kodla bir hücre çalıştırın:
    ```
    data_df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>NYC TAXI verilerini Spark nyctaxi veritabanına yükleme

**SQLPOOL1**' deki bir tabloda veri mevcuttur. **Nyctaxi** adlı bir Spark veritabanına yükleyin.

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin.
1. **+**  >  **Not defteri** seçin.
1. Not defterinin en üstünde, **Attach** değerini **Spark1** olarak ayarlayın.
1. Bir not defteri kod hücresi eklemek için **Kod Ekle** ' yi seçin ve ardından şu metni yapıştırın:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLPOOL1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. **Veri** merkezine gidin, **veritabanları**' na sağ tıklayın ve ardından **Yenile**' yi seçin. Şu veritabanlarını görmeniz gerekir:

    - **SQLPOOL1 (SQL)**
    - **nyctaxi (Spark)**

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

1. Not defterinize geri dönün.
1. Yeni bir kod hücresi oluşturun ve aşağıdaki metni girin. Ardından, **nyctaxi** Spark veritabanına YÜKLEDIĞIMIZ NYC TAXI verilerini göstermek için hücreyi çalıştırın.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Daha önce adanmış SQL havuzu **SQLPOOL1** ile yaptığımız analizi yapmak için aşağıdaki kodu çalıştırın. Bu kod, çözümlemenin sonuçlarını **nyctaxi. passengercountstats** adlı bir tabloya kaydeder ve sonuçları görselleştirir.

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

Daha önce, **SQLPOOL1. dbo. yolculuğa** YÖNELIK adanmış SQL havuzu tablosundan veri kopyaladık **nyctaxi. seyahat**. Daha sonra Spark kullanarak, verileri Spark tablosuna **nyctaxi. passengercountstats** olarak toplandık. Artık **nyctaxi. passengercountstats** Içindeki verileri **SQLPOOL1. dbo. passengercountstats** adlı adanmış bir SQL havuzu tablosuna kopyalayacağız.

Not defterinizde aşağıdaki hücreyi çalıştırın. Toplu Spark tablosunu adanmış SQL havuzu tablosuna kopyalar.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLPOOL1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sunucusuz SQL havuzu ile verileri analiz etme](get-started-analyze-sql-on-demand.md)


