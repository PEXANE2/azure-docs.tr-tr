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
ms.openlocfilehash: 8559bd0a354a64872e58d014d1027ed971773b60
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104655351"
---
# <a name="analyze-with-apache-spark"></a>Apache Spark ile Çözümle

Bu öğreticide, Azure SYNAPSE için Apache Spark verileri yüklemek ve analiz etmek için temel adımları öğreneceksiniz.

## <a name="create-a-serverless-apache-spark-pool"></a>Sunucusuz Apache Spark havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede   >  **Apache Spark havuzlarını** Yönet ' i seçin.
1. **Yeni** ' yi seçin 
1. **Apache Spark havuz adı** için **Spark1** girin.
1. **Düğüm boyutu** için **küçük** girin.
1. **Düğüm sayısı** için en az 3 ve en fazla 3 olarak ayarlayın
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

## <a name="understanding-serverless-apache-spark-pools"></a>Sunucusuz Apache Spark havuzlarını anlama

Sunucusuz Spark havuzu, bir kullanıcının Spark ile nasıl çalışmak istediğini belirten bir yoldur. Bir havuzu kullanmaya başladığınızda, gerekirse Spark oturumu oluşturulur. Havuz, bu oturum tarafından kaç Spark kaynağı kullanılacağını ve oturumun otomatik olarak duraklamadan önce ne kadar süreceğine ilişkin bir denetim verir. Havuzun kendisi için değil, bu oturum sırasında kullanılan Spark kaynakları için ödeme yaparsınız. Bu şekilde, bir Spark havuzu, kümeleri yönetmek zorunda kalmadan Spark ile çalışmanıza olanak sağlar. Bu, sunucusuz SQL havuzunun çalışmasına benzer.

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Spark kullanarak, blob depolamada NYC TAXI verilerini çözümleme

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin
2. Varsayılan dili **Pyspark (Python)** olarak ayarlanmış bir Newnnot defteri oluşturun.
3. Yeni bir kod hücresi oluşturun ve aşağıdaki kodu bu hücreye yapıştırın.
    ```
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    df = data.to_spark_dataframe()
    # Display 10 rows
    display(df.limit(10))
    ```
1. Not defterinde, **Ekle** menüsünde, daha önce oluşturduğumuz **Spark1** sunucusuz Spark havuzunu seçin.
1. Hücrede **Çalıştır** ' ı seçin. SYNAPSE, gerekirse bu hücreyi çalıştırmak için yeni bir Spark oturumu başlatacak. Yeni bir Spark oturumu gerekliyse, başlangıçta oluşturulması iki saniye sürer. 
1. Yalnızca veri çerçevesinin şemasını görmek istiyorsanız aşağıdaki kodla bir hücre çalıştırın:

    ```py
    df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>NYC TAXI verilerini Spark nyctaxi veritabanına yükleme

Veriler, **veri adlı veri** çerçevesi aracılığıyla kullanılabilir. **Nyctaxi** adlı bir Spark veritabanına yükleyin.

1. Not defterine yeni bir ekleyin ve ardından aşağıdaki kodu girin:

    ```py
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```
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


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Adanmış SQL havuzu ile verileri analiz etme](get-started-analyze-sql-pool.md)
