---
title: 'Öğretici: Spark ile Çözümle çalışmaya başlama'
description: Bu öğreticide, Apache Spark ile verileri çözümlemeyi öğreneceksiniz
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: ebcec3907e40a8ba58aab841cd788c58ec7a94fe
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90017923"
---
# <a name="analyze-with-apache-spark"></a>Apache Spark ile Çözümle

## <a name="analyze-nyc-taxi-data-in-blob-storage--using-spark"></a>Spark kullanarak, blob depolamada NYC TAXI verilerini çözümleme

Bu öğreticide, Azure SYNAPSE için Apache Spark verileri yüklemek ve analiz etmek için temel adımları öğreneceksiniz.

1. **Bağlı**' ın altındaki **veri** hub 'ında Azure Blob Storage ' a sağ tıklayıp **> örnek veri kümeleri > Nyc_tlc_yellow** ve **Yeni Not defteri Seç** ' i seçin.
1. Bu, aşağıdaki kod ile yeni bir not defteri oluşturur:
    ```
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    display(data_df.limit(10))
    ```
1. Not defteri 'nde, **Ekle** menüsünde bir Spark havuzu seçin
1. Hücrede **Çalıştır** 'a tıklayın

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>NYC TAXI verilerini Spark nyctaxi veritabanına yükleme

**SQLDB1**' deki bir tabloda veri mevcuttur. **Nyctaxi**adlı bir Spark veritabanına yükleyin.

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin.
1. **+**  >  **Not defteri**seçin.
1. Not defterinin en üstünde, **Attach** değerini **Spark1**olarak ayarlayın.
1. Bir not defteri kod hücresi eklemek için **Kod Ekle** ' yi seçin ve ardından şu metni yapıştırın:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. **Veri** merkezine gidin, **veritabanları**' na sağ tıklayın ve ardından **Yenile**' yi seçin. Şu veritabanlarını görmeniz gerekir:

    - **SQLDB1** (SQL havuzu)
    - **nyctaxi** (Spark)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

1. Not defterinize geri dönün.
1. Yeni bir kod hücresi oluşturun ve aşağıdaki metni girin. Ardından, **nyctaxi** Spark veritabanına YÜKLEDIĞIMIZ NYC TAXI verilerini göstermek için hücreyi çalıştırın.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Daha önce SQL Pool **SQLDB1**ile yaptığımız analizi yapmak için aşağıdaki kodu çalıştırın. Bu kod, çözümlemenin sonuçlarını **nyctaxi. passengercountstats** adlı bir tabloya kaydeder ve sonuçları görselleştirir.

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

## <a name="customize-data-visualization-with-spark-and-notebooks"></a>Spark ve Not defterleri ile veri görselleştirmesini özelleştirme

Grafiklerin not defterlerini kullanarak nasıl işleneceğini kontrol edebilirsiniz. Aşağıdaki kod basit bir örnek gösterir. **Matplotlib** ve **Seaborn**popüler kitaplıklarını kullanır. Kod, daha önce çalıştırdığımız SQL sorgularıyla aynı tür çizgi grafiğini işler.

```py
%%pyspark
import matplotlib.pyplot
import seaborn

seaborn.set(style = "whitegrid")
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.toPandas()
seaborn.lineplot(x="PassengerCount", y="SumTripDistance" , data = df)
seaborn.lineplot(x="PassengerCount", y="AvgTripDistance" , data = df)
matplotlib.pyplot.show()
```



## <a name="load-data-from-a-spark-table-into-a-sql-pool-table"></a>Spark tablosundan SQL havuz tablosuna veri yükleme

Daha önce, **SQLDB1. dbo. SEYAHATSQL** havuz tablosundan verileri Spark tablosu **nyctaxi. seyahat**içine kopyaladık. Daha sonra Spark kullanarak, verileri Spark tablosuna **nyctaxi. passengercountstats**olarak toplandık. Artık **nyctaxi. passengercountstats** Içindeki verileri **SQLDB1. dbo. passengercountstats**adlı bir SQL havuzu tablosuna kopyalayacağız.

Not defterinizde aşağıdaki hücreyi çalıştırın. Toplanan Spark tablosunu SQL havuz tablosuna geri kopyalar.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Depolamadaki verileri analiz etme](get-started-analyze-storage.md)


