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
ms.openlocfilehash: 89bc2723a0d7c99160c651fb433db6f8892ee676
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321078"
---
# <a name="analyze-with-apache-spark"></a>Apache Spark ile Çözümle

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Spark kullanarak, blob depolamada NYC TAXI verilerini çözümleme

Bu öğreticide, Azure SYNAPSE için Apache Spark verileri yüklemek ve analiz etmek için temel adımları öğreneceksiniz.

1. **Veri** hub 'ında **Yeni Kaynak Ekle** ' ye tıklayın (Ayrıca, **bağlı** düğme)  >> **örneklere gözatamazsınız**. **NYC taxı & Limousine Komisyonu bulun-sarı TAXI seyahat kayıtları** bulun ve üzerine tıklayın. Sayfanın alt kısmındaki **devam** ' a ve ardından **veri kümesi Ekle** ' ye basın. Şimdi **veri** hub 'ında **bağlantılı** seç altında **Azure Blob depolama >> örnek veri kümeleri >> nyc_tlc_yellow** ' na sağ tıklayın ve **Yeni Not defteri** ' ni seçin.
1. Bu, aşağıdaki kod ile yeni bir not defteri oluşturur:
    ```
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    display(data_df.limit(10))
    ```
1. Not defterinde, **Ekle** menüsünde sunucusuz Spark havuzu seçin
1. Hücrede **Çalıştır** 'ı seçin

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>NYC TAXI verilerini Spark nyctaxi veritabanına yükleme

**SQLDB1** ' deki bir tabloda veri mevcuttur. **Nyctaxi** adlı bir Spark veritabanına yükleyin.

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin.
1. **+**  >  **Not defteri** seçin.
1. Not defterinin en üstünde, **Attach** değerini **Spark1** olarak ayarlayın.
1. Bir not defteri kod hücresi eklemek için **Kod Ekle** ' yi seçin ve ardından şu metni yapıştırın:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. **Veri** merkezine gidin, **veritabanları** ' na sağ tıklayın ve ardından **Yenile** ' yi seçin. Şu veritabanlarını görmeniz gerekir:

    - **SQLDB1** (adanmış SQL havuzu)
    - **nyctaxi** (sunucusuz Apache Spark havuzu)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

1. Not defterinize geri dönün.
1. Yeni bir kod hücresi oluşturun ve aşağıdaki metni girin. Ardından, **nyctaxi** Spark veritabanına YÜKLEDIĞIMIZ NYC TAXI verilerini göstermek için hücreyi çalıştırın.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Daha önce adanmış SQL havuzu **SQLDB1** ile yaptığımız analizi yapmak için aşağıdaki kodu çalıştırın. Bu kod, çözümlemenin sonuçlarını **nyctaxi. passengercountstats** adlı bir tabloya kaydeder ve sonuçları görselleştirir.

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

Grafiklerin not defterlerini kullanarak nasıl işleneceğini kontrol edebilirsiniz. Aşağıdaki kod basit bir örnek gösterir. **Matplotlib** ve **Seaborn** popüler kitaplıklarını kullanır. Kod, daha önce çalıştırdığımız SQL sorgularıyla aynı tür çizgi grafiğini işler.

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



## <a name="load-data-from-a-spark-table-into-a-dedicated-sql-pool-table"></a>Spark tablosundan adanmış bir SQL havuzu tablosuna veri yükleme

Daha önce, **SQLDB1. dbo. yolculuğa** YÖNELIK adanmış SQL havuzu tablosundan veri kopyaladık **nyctaxi. seyahat**. Daha sonra Spark kullanarak, verileri Spark tablosuna **nyctaxi. passengercountstats** olarak toplandık. Artık **nyctaxi. passengercountstats** Içindeki verileri **SQLDB1. dbo. passengercountstats** adlı adanmış bir SQL havuzu tablosuna kopyalayacağız.

Not defterinizde aşağıdaki hücreyi çalıştırın. Toplu Spark tablosunu adanmış SQL havuzu tablosuna kopyalar.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sunucusuz SQL havuzu ile verileri analiz etme](get-started-analyze-sql-on-demand.md)


