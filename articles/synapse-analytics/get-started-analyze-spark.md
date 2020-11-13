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
ms.openlocfilehash: a33b702209bf2724098b7a0db907b1b7237ceccf
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94592626"
---
# <a name="analyze-with-apache-spark"></a>Apache Spark ile Çözümle

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Spark kullanarak, blob depolamada NYC TAXI verilerini çözümleme

Bu öğreticide, Azure SYNAPSE için Apache Spark verileri yüklemek ve analiz etmek için temel adımları öğreneceksiniz.

1. **Veri** hub 'ında **Yeni Kaynak Ekle** ' ye tıklayın (Ayrıca, **bağlı** düğme)  >> **örneklere gözatamazsınız**. 
1. **NYC taxı & Limousine Komisyonu bulun-sarı TAXI seyahat kayıtları** bulun ve üzerine tıklayın. 
1. Sayfanın alt kısmındaki **devam** ' a ve ardından **veri kümesi Ekle** ' ye basın. 
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

**SQLPOOL1** ' deki bir tabloda veri mevcuttur. **Nyctaxi** adlı bir Spark veritabanına yükleyin.

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

1. **Veri** merkezine gidin, **veritabanları** ' na sağ tıklayın ve ardından **Yenile** ' yi seçin. Şu veritabanlarını görmeniz gerekir:

    - **SQLPOOL1** (adanmış SQL havuzu)
    - **nyctaxi** (sunucusuz Apache Spark havuzu)

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


