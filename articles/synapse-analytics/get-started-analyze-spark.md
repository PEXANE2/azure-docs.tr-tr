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
ms.date: 12/31/2020
ms.openlocfilehash: 2feabda5ea3f0c0748b92de9fcb7ef05abbdcf4c
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209449"
---
# <a name="analyze-with-apache-spark"></a>Apache Spark ile Çözümle

Bu öğreticide, Azure SYNAPSE için Apache Spark verileri yüklemek ve analiz etmek için temel adımları öğreneceksiniz.

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Spark kullanarak, blob depolamada NYC TAXI verilerini çözümleme

1. **Veri** merkezinde, **+** **Yeni bir kaynak eklemek** için düğmeye tıklayın ve ardından >> **Galeriye gidin**' e tıklayın. 
1. **NYC taxı & Limousine Komisyonu bulun-sarı TAXI seyahat kayıtları** bulun ve üzerine tıklayın. 
1. Sayfanın alt kısmındaki **devam**' a ve ardından **veri kümesi Ekle**' ye basın. 
1. Bir süre sonra, **bağlı**' ın altında **veri** veri **Nyc_tlc_yellow >> kümeleri >> Azure Blob Storage** ' a sağ tıklayın ve **Yeni Not defteri**' ni seçin, sonra da **veri çerçevesine yükleyin**.
1. Bu, aşağıdaki kod ile yeni bir not defteri oluşturur:
    ```

    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    # Display 10 rows
    display(data_df.limit(10))
    ```
1. Not defterinde, **Ekle** menüsünde, daha önce oluşturduğumuz **Spark1** sunucusuz Spark havuzunu seçin.
1. Hücrede **Çalıştır** 'ı seçin
1. Yalnızca veri çerçevesinin şemasını görmek istiyorsanız aşağıdaki kodla bir hücre çalıştırın:
    ```

    data_df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>NYC TAXI verilerini Spark nyctaxi veritabanına yükleme

Veriler, **SQLPOOL1** içindeki bir tabloda kullanılabilir. **Nyctaxi** adlı bir Spark veritabanına yükleyin.

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin.
1. **+**  >  **Not defteri** seçin.
1. Not defterinin en üstünde, **Attach** değerini **Spark1** olarak ayarlayın.
1. Yeni Not defterinin ilk kod hücresinde aşağıdaki kodu girin:


    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLPOOL1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```


1. Betiği çalıştırın. Bu işlem 2-3 dakika sürebilir.
1. **Veri** hub 'ında, **çalışma alanı** sekmesinde, **veritabanları**' na sağ tıklayın ve ardından **Yenile**' yi seçin. Şimdi listede veritabanını **nyctaxi (Spark)** görmeniz gerekir.


## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

1. Not defterinize geri dönün.
1. Yeni bir kod hücresi oluşturun ve aşağıdaki kodu girin. 


   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. **Nyctaxi** Spark veritabanına YÜKLEDIĞIMIZ NYC TAXI verilerini göstermek için hücreyi çalıştırın.
1. Yeni bir kod hücresi oluşturun ve aşağıdaki kodu girin. Ardından, daha önce adanmış SQL havuzu **SQLPOOL1** ile yaptığımız analizi yapmak için hücreyi çalıştırın. Bu kod, çözümlemenin sonuçlarını **nyctaxi. passengercountstats** adlı bir tabloya kaydeder ve görüntüler.


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

Daha önce, **SQLPOOL1. dbo. yolculuğa** YÖNELIK adanmış SQL havuzu tablosundan veri kopyaladık **nyctaxi. seyahat**. Daha sonra verileri Spark tablosu **nyctaxi. passengercountstats** içine yığıldıktan sonra. Artık **nyctaxi. passengercountstats** Içindeki verileri **SQLPOOL1. dbo. passengercountstats** adlı özel bir SQL havuzu tablosuna kopyalayacaksınız.

1. Yeni bir kod hücresi oluşturun ve aşağıdaki kodu girin. Hücreyi Not defterinizde çalıştırın. Toplu Spark tablosunu adanmış SQL havuzu tablosuna kopyalar.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLPOOL1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sunucusuz SQL havuzu ile verileri analiz etme](get-started-analyze-sql-on-demand.md)
