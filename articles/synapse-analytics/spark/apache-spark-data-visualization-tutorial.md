---
title: Apache Spark ile verileri görselleştirme
description: Apache Spark ve Azure SYNAPSE Analytics not defterlerini kullanarak zengin veri görselleştirmeleri oluşturma
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 10/20/2020
ms.author: midesa
ms.openlocfilehash: 8735514b639cb0322a83ffb19d661027327c0f73
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96458767"
---
# <a name="analyze-data-with-apache-spark"></a>Apache Spark ile verileri analiz etme

Bu öğreticide, Azure açık veri kümelerini kullanarak keşif verileri analizini gerçekleştirmeyi ve Apache Spark ve ardından sonuçları bir Azure SYNAPSE Studio not defterinde görselleştirmeyi öğreneceksiniz.

Özellikle, [New York City (NYC) TAXI](https://azure.microsoft.com/en-us/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) veri kümesini analiz edeceğiz. Veriler Azure açık veri kümeleri aracılığıyla kullanılabilir. Veri kümesinin bu alt kümesi, her seyahat, başlangıç ve bitiş saati ve konumları, maliyet ve diğer ilgi çekici öznitelikler de dahil olmak üzere sarı TAXI seyahatleriyle ilgili bilgiler içerir.
  
## <a name="before-you-begin"></a>Başlamadan önce
- [Apache Spark Havuzu Oluşturma öğreticisini](../articles/../quickstart-create-apache-spark-pool-studio.md) Izleyerek Apache Spark havuzu oluşturma 

## <a name="download-and-prepare-the-data"></a>Verileri indirin ve hazırlayın
1. PySpark çekirdeğini kullanarak bir not defteri oluşturun. Yönergeler için bkz. [Not defteri oluşturma](https://docs.microsoft.com/azure/synapse-analytics/quickstart-apache-spark-notebook#create-a-notebook). 
   
> [!Note]
> 
> PySpark çekirdeği nedeniyle, açıkça herhangi bir bağlam oluşturmanız gerekmez. İlk kod hücresini çalıştırdığınızda Spark bağlamı sizin için otomatik olarak oluşturulur.
>

2. Bu öğreticide, veri kümesini görselleştirmemize yardımcı olmak için birkaç farklı kitaplık kullanacağız. Bu analizi yapmak için aşağıdaki kitaplıkları içeri aktarmanız gerekir: 

```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
```

3. Ham veriler bir Parquet biçiminde olduğundan, dosyayı doğrudan veri çerçevesi olarak belleğe çekmek için Spark bağlamını kullanabilirsiniz. Açık veri kümeleri API 'SI aracılığıyla verileri alarak Spark dataframe oluşturun. Burada, veri türlerini ve şemayı çıkarması için *okuma* özelliklerinde Spark dataframe şemasını kullanacağız.

```python
from azureml.opendatasets import NycTlcYellow
from datetime import datetime
from dateutil import parser

end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcYellow(start_date=start_date, end_date=end_date)
df = nyc_tlc.to_spark_dataframe()
```

4. Veriler okunduktan sonra, veri kümesini temizlemek için bazı ilk filtreleme yapmak istiyoruz. Gereksiz sütunları kaldırabiliriz ve önemli bilgileri ayıklayarak ek sütunlar ekleyebiliriz. Ayrıca, veri kümesi içindeki anormallikleri de filtreleyecağız.

```python
# Filter the dataset 
from pyspark.sql.functions import *

filtered_df = df.select('vendorID', 'passengerCount', 'tripDistance','paymentType', 'fareAmount', 'tipAmount'\
                                , date_format('tpepPickupDateTime', 'hh').alias('hour_of_day')\
                                , dayofweek('tpepPickupDateTime').alias('day_of_week')\
                                , dayofmonth(col('tpepPickupDateTime')).alias('day_of_month'))\
                            .filter((df.passengerCount > 0)\
                                & (df.tipAmount >= 0)\
                                & (df.fareAmount >= 1) & (df.fareAmount <= 250)\
                                & (df.tripDistance > 0) & (df.tripDistance <= 200))

filtered_df.createOrReplaceTempView("taxi_dataset")
```

## <a name="analyze-data"></a>Verileri çözümleme
 Veri analisti olarak, verilerden Öngörüler ayıklamaya yardımcı olacak çok çeşitli araçlara sahip olursunuz. Öğreticinin bu bölümünde, Azure SYNAPSE Analytics Not defterleri içinde sunulan birkaç faydalı araç hakkında yol göstereceğiz. Bu çözümlemede, seçili dönemimiz için daha yüksek bir TAXI ipucu sunan faktörleri anlamak istiyoruz.

###  <a name="apache-spark-sql-magic"></a>Apache Spark SQL Magic 
İlk olarak, SYNAPSE not defteriyle SQL ve MAGIC komutlarıyla Apache Spark araştırmacı veri analizi gerçekleştireceğiz. Sorgumuza ulaştıktan sonra, yerleşik özelliği kullanarak sonuçları görselleştirecağız ```chart options``` . 

1. Not defteriniz içinde yeni bir hücre oluşturun ve aşağıdaki kodu kopyalayın. Bu sorguyu kullanarak, ortalama ipucu tutarlarının seçildiğimiz süre içinde nasıl değiştiğini anlamak istiyoruz. Bu sorgu, günlük minimum/maksimum tıp miktarı ve ortalama tarifeli havayolu miktarı dahil diğer yararlı içgörüleri tanımlamamıza da yardımcı olur.
   
```sql
%%sql
SELECT 
    day_of_month
    , MIN(tipAmount) AS minTipAmount
    , MAX(tipAmount) AS maxTipAmount
    , AVG(tipAmount) AS avgTipAmount
    , AVG(fareAmount) as fareAmount
FROM taxi_dataset 
GROUP BY day_of_month
ORDER BY day_of_month ASC
```

2. Sorgu çalışmayı bitirdikten sonra **grafik görünümüne** geçerek sonuçları görselleştirebiliriz. Bu örnekte, **line chart** ```day_of_month``` alanı **anahtar** olarak ve ```avgTipAmount``` **değer** olarak belirterek bir çizgi grafik oluşturacağız. Seçimleri yaptıktan sonra, grafiğinizi yenilemek için **Uygula** ' ya tıklayın. 
   
## <a name="visualize-data"></a>Verileri görselleştirme
Yerleşik Not defteri grafik seçeneklerine ek olarak, kendi görselleştirmelerinizi oluşturmak için popüler açık kaynaklı kitaplıkları da kullanabilirsiniz. Aşağıdaki örneklerde, veri görselleştirme için yaygın olarak kullanılan Python kitaplıkları olan Seaborn ve Matplotlib kullanacağız. 

> [!Note]
> 
> Varsayılan olarak, her Azure SYNAPSE Analytics Apache Spark havuzu, yaygın olarak kullanılan ve varsayılan kitaplıkların bir kümesini içerir. Kitaplıkların tam listesini [Azure SYNAPSE çalışma zamanı](../spark/apache-spark-version-support.md)' nda görüntüleyebilirsiniz. belgelerle. Ayrıca, üçüncü tarafa veya yerel olarak oluşturulmuş bir kodu uygulamalarınız için kullanılabilir hale getirmek için, Spark Havuzlarınızdan birine [bir kitaplık yükleyebilirsiniz](../spark/apache-spark-azure-portal-add-libraries.md) .
>

1. Geliştirmeyi daha kolay ve daha ucuz hale getirmek için veri kümesini örnekliyoruz. Yerleşik Apache Spark örnekleme özelliğini kullanacağız. Ayrıca, hem Seaborn hem de Matplotlib bir Pandas dataframe veya bir sayısal tuş takımı dizisi gerektirir. Bir Pandas dataframe almak için, ```toPandas()``` komutunu kullanarak veri çerçevemizi dönüştürmeniz gerekir.

```python
# To make development easier, faster and less expensive down sample for now
sampled_taxi_df = filtered_df.sample(True, 0.001, seed=1234)

# The charting package needs a Pandas dataframe or numpy array do the conversion
sampled_taxi_pd_df = sampled_taxi_df.toPandas()
```

1. İlk olarak, veri kümizdeki ipuçlarının dağılımını anlamak istiyoruz. Tıp tutarının ve sayısının dağılımını gösteren bir histogram oluşturmak için Matplotlib kullanacağız. Dağıtıma bağlı olarak, ipuçlarının $10 ' den küçük veya buna eşit miktarlarda çarpılır olduğunu görebiliriz.

```python
# Look at tips by count histogram using Matplotlib

ax1 = sampled_taxi_pd_df['tipAmount'].plot(kind='hist', bins=25, facecolor='lightblue')
ax1.set_title('Tip amount distribution')
ax1.set_xlabel('Tip Amount ($)')
ax1.set_ylabel('Counts')
plt.suptitle('')
plt.show()
```

![Ipuçları histogramı](./media/apache-spark-machine-learning-mllib-notebook/histogram.png)

1. Ardından, belirli bir yolculuğa yönelik ipuçları ve haftanın günü arasındaki ilişkiyi anlamak istiyoruz. Haftanın her günü için eğilimleri özetleyen bir kutu çizimi oluşturmak için Seaborn kullanacağız. 

```python
# View the distribution of tips by day of week using Seaborn
ax = sns.boxplot(x="day_of_week", y="tipAmount",data=sampled_taxi_pd_df, showfliers = False)
ax.set_title('Tip amount distribution per day')
ax.set_xlabel('Day of Week')
ax.set_ylabel('Tip Amount ($)')
plt.show()

```
![Gün başına ipuçları dağılımı](./media/apache-spark-data-viz/data-analyst-tutorial-per-day.png)

4. Şimdi, yaptığımız bir varsayım, pastıcılar sayısı ile toplam TAXI tıp miktarı arasında bir pozitif ilişki olması olabilir. Bu ilişkiyi doğrulamak için, her bir pasla sayısı için ipuçlarının dağıtımını gösteren bir kutu çizimi oluşturmak için aşağıdaki kodu çalıştıracağız. 
   
```python
# How many passengers tipped by various amounts 
ax2 = sampled_taxi_pd_df.boxplot(column=['tipAmount'], by=['passengerCount'])
ax2.set_title('Tip amount by Passenger count')
ax2.set_xlabel('Passenger count')
ax2.set_ylabel('Tip Amount ($)')
ax2.set_ylim(0,30)
plt.suptitle('')
plt.show()
```
![Kutu yatay çizimi](./media/apache-spark-machine-learning-mllib-notebook/box-whisker-plot.png)

1. Son olarak, tarifeli havayolu amount tip miktarı arasındaki ilişkiyi anlamak istiyoruz. Sonuçlara bağlı olarak, insanların ipucu olmayan birçok gözlemde olduğunu görebiliriz. Bununla birlikte, genel tarifeli havayolu ve tıp tutarları arasında da pozitif bir ilişki görürsünüz.
   
```python
# Look at the relationship between fare and tip amounts

ax = sampled_taxi_pd_df.plot(kind='scatter', x= 'fareAmount', y = 'tipAmount', c='blue', alpha = 0.10, s=2.5*(sampled_taxi_pd_df['passengerCount']))
ax.set_title('Tip amount by Fare amount')
ax.set_xlabel('Fare Amount ($)')
ax.set_ylabel('Tip Amount ($)')
plt.axis([-2, 80, -2, 20])
plt.suptitle('')
plt.show()
```
![Dağılım çizimi](./media/apache-spark-machine-learning-mllib-notebook/scatter.png)

## <a name="shut-down-the-spark-instance"></a>Spark örneğini kapatma

Uygulamayı çalıştırmayı bitirdikten sonra, sekmeyi kapatarak veya Not defterinin alt kısmındaki durum panelinden **oturumu bitir** ' i seçerek kaynakları serbest bırakmak için Not defterini kapatın.

## <a name="see-also"></a>Ayrıca bkz.

- [Genel Bakış: Azure SYNAPSE Analytics 'te Apache Spark](apache-spark-overview.md)
- [Apache mini ml ile makine öğrenimi modeli oluşturma](../spark/apache-spark-machine-learning-mllib-notebook.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Resmi belgeleri Apache Spark](https://spark.apache.org/docs/latest/)