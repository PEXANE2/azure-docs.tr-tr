---
title: 'Regresyon eğitimi: Otomatik ML'
titleSuffix: Azure Machine Learning
description: Bu eğitimde, otomatik makine öğrenimi kullanarak bir makine öğrenme modeli oluşturmak için nasıl öğrenirler. Azure Machine Learning, veri önişleme, algoritma seçimi ve hiperparametre seçimini sizin için otomatik bir şekilde gerçekleştirebilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 02/10/2020
ms.openlocfilehash: 75e61ea3f4fa6c2b346f912a9effd66ad94e7e93
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77116454"
---
# <a name="tutorial-use-automated-machine-learning-to-predict-taxi-fares"></a>Öğretici: Taksi ücretlerini tahmin etmek için otomatik makine öğrenimini kullanın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu eğitimde, Azure Machine Learning'de otomatik makine öğrenimini kullanarak NYC taksi ücret fiyatlarını tahmin etmek için bir regresyon modeli oluşturursunuz. Bu işlem, eğitim verilerini ve yapılandırma ayarlarını kabul eder ve en iyi modele ulaşmak için farklı özellik normalleştirme/standartlaştırma yöntemleri, modeller ve hiperparametre ayarlarının birleşimleri aracılığıyla otomatik olarak yinelenir.

![Akış diyagramı](./media/tutorial-auto-train-models/flow2.png)

Bu öğreticide aşağıdaki görevleri öğrenirsiniz:

> [!div class="checklist"]
> * Azure Açık Veri Kümelerini kullanarak veri indirme, dönüştürme ve temizleme
> * Otomatik makine öğrenme regresyon modelini eğitin
> * Model doğruluğunu hesaplama

Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir hesap oluşturun. Azure Machine Learning'in [ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanınız veya dizüstü bilgisayarınız yoksa [kurulum eğitimini](tutorial-1st-experiment-sdk-setup.md) tamamlayın.
* Kurulum eğitimini tamamladıktan sonra, aynı dizüstü bilgisayar sunucusunu kullanarak *öğreticiler/regresyon-automl-nyc-taxi-data/regression-automated-ml.ipynb* dizüstü bilgisayar defterini açın.

Bu öğretici, kendi [yerel ortamınızda](how-to-configure-environment.md#local)çalıştırmak istiyorsanız [GitHub'da](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials) da mevcuttur. Gerekli `pip install azureml-sdk[automl] azureml-opendatasets azureml-widgets` paketleri almak için çalıştırın.

## <a name="download-and-prepare-data"></a>Verileri indirin ve hazırlayın

Gerekli paketleri içeri aktarın. Açık Veri Kümeleri paketi, indirmeden önce`NycTlcGreen` tarih parametrelerini kolayca filtrelemek için her veri kaynağını (örneğin) temsil eden bir sınıf içerir.

```python
from azureml.opendatasets import NycTlcGreen
import pandas as pd
from datetime import datetime
from dateutil.relativedelta import relativedelta
```

Taksi verilerini tutmak için bir veri çerçevesi oluşturarak başlayın. Spark olmayan bir ortamda çalışırken, Açık Veri Kümeleri büyük veri kümeleriyle kaçınmak `MemoryError` için belirli sınıflarla aynı anda yalnızca bir aylık veri indirmeye izin verir.

Taksi verilerini indirmek için, yinelemeli olarak bir seferde bir ay `green_taxi_df` getirin ve veri çerçevesini şişirmemek için her aydan rastgele 2.000 kayıt örneği almak için eklemeden önce. Ardından verileri önizleyin.


```python
green_taxi_df = pd.DataFrame([])
start = datetime.strptime("1/1/2015","%m/%d/%Y")
end = datetime.strptime("1/31/2015","%m/%d/%Y")

for sample_month in range(12):
    temp_df_green = NycTlcGreen(start + relativedelta(months=sample_month), end + relativedelta(months=sample_month)) \
        .to_pandas_dataframe()
    green_taxi_df = green_taxi_df.append(temp_df_green.sample(2000))

green_taxi_df.head(10)
```

<div>
<style scoped>.dataframe tbody tr th:only-of-type { dikey hizalama: orta; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Vendorıd</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>yolcu Sayısı</th>
      <th>tripDistance</th>
      <th>puLocationId</th>
      <th>doLocationId</th>
      <th>pickupBoyitude</th>
      <th>pickupLatitude</th>
      <th>dropoffBoyitude</th>
      <th>...</th>
      <th>ödemeTürü</th>
      <th>ücretTutar</th>
      <th>Ekstra</th>
      <th>mtaTax</th>
      <th>iyileştirmeSurcharge</th>
      <th>ipucuTutar</th>
      <th>geçiş ücretleriTutar</th>
      <th>ehailÜcreti</th>
      <th>toplamTutar</th>
      <th>tripType</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>131969</th>
      <td>2</td>
      <td>2015-01-11 05:34:44</td>
      <td>2015-01-11 05:45:03</td>
      <td>3</td>
      <td>4.84</td>
      <td>None</td>
      <td>None</td>
      <td>-73.88</td>
      <td>40.84</td>
      <td>-73.94</td>
      <td>...</td>
      <td>2</td>
      <td>15.00</td>
      <td>0,50</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>16.30</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>1129817</th>
      <td>2</td>
      <td>2015-01-20 16:26:29</td>
      <td>2015-01-20 16:30:26</td>
      <td>1</td>
      <td>0.69</td>
      <td>None</td>
      <td>None</td>
      <td>-73.96</td>
      <td>40.81</td>
      <td>-73.96</td>
      <td>...</td>
      <td>2</td>
      <td>4.50</td>
      <td>1,00</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>6.30</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>1278620</th>
      <td>2</td>
      <td>2015-01-01 05:58:10</td>
      <td>2015-01-01 06:00:55</td>
      <td>1</td>
      <td>0.45</td>
      <td>None</td>
      <td>None</td>
      <td>-73.92</td>
      <td>40.76</td>
      <td>-73.91</td>
      <td>...</td>
      <td>2</td>
      <td>4,00</td>
      <td>0,00</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>4,80</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>348430</th>
      <td>2</td>
      <td>2015-01-17 02:20:50</td>
      <td>2015-01-17 02:41:38</td>
      <td>1</td>
      <td>0,00</td>
      <td>None</td>
      <td>None</td>
      <td>-73.81</td>
      <td>40.70</td>
      <td>-73.82</td>
      <td>...</td>
      <td>2</td>
      <td>12.50</td>
      <td>0,50</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>13.80</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>1269627</th>
      <td>1</td>
      <td>2015-01-01 05:04:10</td>
      <td>2015-01-01 05:06:23</td>
      <td>1</td>
      <td>0,50</td>
      <td>None</td>
      <td>None</td>
      <td>-73.92</td>
      <td>40.76</td>
      <td>-73.92</td>
      <td>...</td>
      <td>2</td>
      <td>4,00</td>
      <td>0,50</td>
      <td>0,50</td>
      <td>0</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>5,00</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>811755</th>
      <td>1</td>
      <td>2015-01-04 19:57:51</td>
      <td>2015-01-04 20:05:45</td>
      <td>2</td>
      <td>1.10</td>
      <td>None</td>
      <td>None</td>
      <td>-73.96</td>
      <td>40.72</td>
      <td>-73.95</td>
      <td>...</td>
      <td>2</td>
      <td>6.50</td>
      <td>0,50</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>7.80</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>737281</th>
      <td>1</td>
      <td>2015-01-03 12:27:31</td>
      <td>2015-01-03 12:33:52</td>
      <td>1</td>
      <td>0,90</td>
      <td>None</td>
      <td>None</td>
      <td>-73.88</td>
      <td>40.76</td>
      <td>-73.87</td>
      <td>...</td>
      <td>2</td>
      <td>6.00</td>
      <td>0,00</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>6.80</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>113951</th>
      <td>1</td>
      <td>2015-01-09 23:25:51</td>
      <td>2015-01-09 23:39:52</td>
      <td>1</td>
      <td>3.30</td>
      <td>None</td>
      <td>None</td>
      <td>-73.96</td>
      <td>40.72</td>
      <td>-73.91</td>
      <td>...</td>
      <td>2</td>
      <td>12.50</td>
      <td>0,50</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>13.80</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>150436</th>
      <td>2</td>
      <td>2015-01-11 17:15:14</td>
      <td>2015-01-11 17:22:57</td>
      <td>1</td>
      <td>1.19</td>
      <td>None</td>
      <td>None</td>
      <td>-73.94</td>
      <td>40.71</td>
      <td>-73.95</td>
      <td>...</td>
      <td>1</td>
      <td>7.00</td>
      <td>0,00</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>1,75</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>9.55</td>
      <td>1,00</td>
    </tr>
    <tr>
      <th>432136</th>
      <td>2</td>
      <td>2015-01-22 23:16:33</td>
      <td>2015-01-22 23:20:13</td>
      <td>1</td>
      <td>0.65</td>
      <td>None</td>
      <td>None</td>
      <td>-73.94</td>
      <td>40.71</td>
      <td>-73.94</td>
      <td>...</td>
      <td>2</td>
      <td>5,00</td>
      <td>0,50</td>
      <td>0,50</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>6.30</td>
      <td>1,00</td>
    </tr>
  </tbody>
</table>
<p>10 satır × 23 sütun</p>
</div>


İlk veriler yüklendiğine göre, teslim alma tarih saatinden çeşitli zaman tabanlı özellikler oluşturmak için bir işlev tanımlayın. Bu, ay sayısı, ayın günü, haftanın günü ve günün saati için yeni alanlar oluşturur ve modelin zamana dayalı mevsimselliği faktöre etmesini sağlar. Taksi `apply()` verilerindeki her satıra `build_time_features()` işlevi yinelemek için veri çerçevesindeki işlevi kullanın.

```python
def build_time_features(vector):
    pickup_datetime = vector[0]
    month_num = pickup_datetime.month
    day_of_month = pickup_datetime.day
    day_of_week = pickup_datetime.weekday()
    hour_of_day = pickup_datetime.hour

    return pd.Series((month_num, day_of_month, day_of_week, hour_of_day))

green_taxi_df[["month_num", "day_of_month","day_of_week", "hour_of_day"]] = green_taxi_df[["lpepPickupDatetime"]].apply(build_time_features, axis=1)
green_taxi_df.head(10)
```

<div>
<style scoped>.dataframe tbody tr th:only-of-type { dikey hizalama: orta; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Vendorıd</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>yolcu Sayısı</th>
      <th>tripDistance</th>
      <th>puLocationId</th>
      <th>doLocationId</th>
      <th>pickupBoyitude</th>
      <th>pickupLatitude</th>
      <th>dropoffBoyitude</th>
      <th>...</th>
      <th>iyileştirmeSurcharge</th>
      <th>ipucuTutar</th>
      <th>geçiş ücretleriTutar</th>
      <th>ehailÜcreti</th>
      <th>toplamTutar</th>
      <th>tripType</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>131969</th>
      <td>2</td>
      <td>2015-01-11 05:34:44</td>
      <td>2015-01-11 05:45:03</td>
      <td>3</td>
      <td>4.84</td>
      <td>None</td>
      <td>None</td>
      <td>-73.88</td>
      <td>40.84</td>
      <td>-73.94</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>16.30</td>
      <td>1,00</td>
      <td>1</td>
      <td>11</td>
      <td>6</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1129817</th>
      <td>2</td>
      <td>2015-01-20 16:26:29</td>
      <td>2015-01-20 16:30:26</td>
      <td>1</td>
      <td>0.69</td>
      <td>None</td>
      <td>None</td>
      <td>-73.96</td>
      <td>40.81</td>
      <td>-73.96</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>6.30</td>
      <td>1,00</td>
      <td>1</td>
      <td>20</td>
      <td>1</td>
      <td>16</td>
    </tr>
    <tr>
      <th>1278620</th>
      <td>2</td>
      <td>2015-01-01 05:58:10</td>
      <td>2015-01-01 06:00:55</td>
      <td>1</td>
      <td>0.45</td>
      <td>None</td>
      <td>None</td>
      <td>-73.92</td>
      <td>40.76</td>
      <td>-73.91</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>4,80</td>
      <td>1,00</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>348430</th>
      <td>2</td>
      <td>2015-01-17 02:20:50</td>
      <td>2015-01-17 02:41:38</td>
      <td>1</td>
      <td>0,00</td>
      <td>None</td>
      <td>None</td>
      <td>-73.81</td>
      <td>40.70</td>
      <td>-73.82</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>13.80</td>
      <td>1,00</td>
      <td>1</td>
      <td>17</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1269627</th>
      <td>1</td>
      <td>2015-01-01 05:04:10</td>
      <td>2015-01-01 05:06:23</td>
      <td>1</td>
      <td>0,50</td>
      <td>None</td>
      <td>None</td>
      <td>-73.92</td>
      <td>40.76</td>
      <td>-73.92</td>
      <td>...</td>
      <td>0</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>5,00</td>
      <td>1,00</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>811755</th>
      <td>1</td>
      <td>2015-01-04 19:57:51</td>
      <td>2015-01-04 20:05:45</td>
      <td>2</td>
      <td>1.10</td>
      <td>None</td>
      <td>None</td>
      <td>-73.96</td>
      <td>40.72</td>
      <td>-73.95</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>7.80</td>
      <td>1,00</td>
      <td>1</td>
      <td>4</td>
      <td>6</td>
      <td>19</td>
    </tr>
    <tr>
      <th>737281</th>
      <td>1</td>
      <td>2015-01-03 12:27:31</td>
      <td>2015-01-03 12:33:52</td>
      <td>1</td>
      <td>0,90</td>
      <td>None</td>
      <td>None</td>
      <td>-73.88</td>
      <td>40.76</td>
      <td>-73.87</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>6.80</td>
      <td>1,00</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>12</td>
    </tr>
    <tr>
      <th>113951</th>
      <td>1</td>
      <td>2015-01-09 23:25:51</td>
      <td>2015-01-09 23:39:52</td>
      <td>1</td>
      <td>3.30</td>
      <td>None</td>
      <td>None</td>
      <td>-73.96</td>
      <td>40.72</td>
      <td>-73.91</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>13.80</td>
      <td>1,00</td>
      <td>1</td>
      <td>9</td>
      <td>4</td>
      <td>23</td>
    </tr>
    <tr>
      <th>150436</th>
      <td>2</td>
      <td>2015-01-11 17:15:14</td>
      <td>2015-01-11 17:22:57</td>
      <td>1</td>
      <td>1.19</td>
      <td>None</td>
      <td>None</td>
      <td>-73.94</td>
      <td>40.71</td>
      <td>-73.95</td>
      <td>...</td>
      <td>0.3</td>
      <td>1,75</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>9.55</td>
      <td>1,00</td>
      <td>1</td>
      <td>11</td>
      <td>6</td>
      <td>17</td>
    </tr>
    <tr>
      <th>432136</th>
      <td>2</td>
      <td>2015-01-22 23:16:33</td>
      <td>2015-01-22 23:20:13</td>
      <td>1</td>
      <td>0.65</td>
      <td>None</td>
      <td>None</td>
      <td>-73.94</td>
      <td>40.71</td>
      <td>-73.94</td>
      <td>...</td>
      <td>0.3</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>Nan</td>
      <td>6.30</td>
      <td>1,00</td>
      <td>1</td>
      <td>22</td>
      <td>3</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
<p>10 satır × 27 sütun</p>
</div>

Eğitim veya ek özellik oluşturma için gerek duymayacağınız sütunlardan bazılarını kaldırın.

```python
columns_to_remove = ["lpepPickupDatetime", "lpepDropoffDatetime", "puLocationId", "doLocationId", "extra", "mtaTax",
                     "improvementSurcharge", "tollsAmount", "ehailFee", "tripType", "rateCodeID",
                     "storeAndFwdFlag", "paymentType", "fareAmount", "tipAmount"
                    ]
for col in columns_to_remove:
    green_taxi_df.pop(col)

green_taxi_df.head(5)
```

### <a name="cleanse-data"></a>Verileri temizleme

Her `describe()` alanın özet istatistiklerini görmek için yeni veri çerçevesi üzerindeki işlevi çalıştırın.

```python
green_taxi_df.describe()
```

<div>
<style scoped>.dataframe tbody tr th:only-of-type { dikey hizalama: orta; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Vendorıd</th>
      <th>yolcu Sayısı</th>
      <th>tripDistance</th>
      <th>pickupBoyitude</th>
      <th>pickupLatitude</th>
      <th>dropoffBoyitude</th>
      <th>dropoffLatitude</th>
      <th>toplamTutar</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
      <td>48000.00</td>
    </tr>
    <tr>
      <th>ortalama</th>
      <td>1.78</td>
      <td>1.37</td>
      <td>2.87</td>
      <td>-73.83</td>
      <td>40.69</td>
      <td>-73.84</td>
      <td>40.70</td>
      <td>14.75</td>
      <td>6.50</td>
      <td>15.13</td>
      <td>3.27</td>
      <td>13.52</td>
    </tr>
    <tr>
      <th>Standart</th>
      <td>0,41</td>
      <td>1.04</td>
      <td>2,93</td>
      <td>2.76</td>
      <td>1,52</td>
      <td>2.61</td>
      <td>1.44</td>
      <td>12.08</td>
      <td>3.45</td>
      <td>8.45</td>
      <td>1.95</td>
      <td>6.83</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1,00</td>
      <td>0,00</td>
      <td>0,00</td>
      <td>-74.66</td>
      <td>0,00</td>
      <td>-74.66</td>
      <td>0,00</td>
      <td>-300.00</td>
      <td>1,00</td>
      <td>1,00</td>
      <td>0,00</td>
      <td>0,00</td>
    </tr>
    <tr>
      <th>%25</th>
      <td>2,00</td>
      <td>1,00</td>
      <td>1.06</td>
      <td>-73.96</td>
      <td>40.70</td>
      <td>-73.97</td>
      <td>40.70</td>
      <td>7.80</td>
      <td>3,75</td>
      <td>8.00</td>
      <td>2,00</td>
      <td>9.00</td>
    </tr>
    <tr>
      <th>%50</th>
      <td>2,00</td>
      <td>1,00</td>
      <td>1.90</td>
      <td>-73.94</td>
      <td>40.75</td>
      <td>-73.94</td>
      <td>40.75</td>
      <td>11.30</td>
      <td>6.50</td>
      <td>15.00</td>
      <td>3,00</td>
      <td>15.00</td>
    </tr>
    <tr>
      <th>%75</th>
      <td>2,00</td>
      <td>1,00</td>
      <td>3,60</td>
      <td>-73.92</td>
      <td>40.80</td>
      <td>-73.91</td>
      <td>40.79</td>
      <td>17.80</td>
      <td>9.25</td>
      <td>22.00</td>
      <td>5,00</td>
      <td>19.00</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2,00</td>
      <td>9.00</td>
      <td>97.57</td>
      <td>0,00</td>
      <td>41.93</td>
      <td>0,00</td>
      <td>41.94</td>
      <td>450.00</td>
      <td>12.00</td>
      <td>30.00</td>
      <td>6.00</td>
      <td>23.00</td>
    </tr>
  </tbody>
</table>
</div>


Özet istatistiklerinden, model doğruluğunu azaltacak aykırı lıkları veya değerleri olan birkaç alan olduğunu görürsünüz. İlk olarak Lat/Long alanlarını Manhattan bölgesisınırları içinde olmak üzere filtreleyin. Bu uzun taksi gezileri veya diğer özellikleri ile ilişkileri açısından aykırı olan geziler filtre olacaktır.

Ayrıca `tripDistance` alanı sıfırdan büyük, ancak 31 milden daha az (iki lat/uzun çift arasındaki haversine mesafesi) filtreleyin. Bu tutarsız seyahat maliyeti var uzun aykırı gezileri ortadan kaldırır.

Son olarak, `totalAmount` alan bizim model bağlamında mantıklı olmayan taksi ücretleri için olumsuz değerlere sahiptir `passengerCount` ve alan minimum değerleri sıfır olan kötü veri vardır.

Sorgu işlevlerini kullanarak bu anormallikleri filtreleyin ve eğitim için gereksiz olan son birkaç sütunu kaldırın.


```python
final_df = green_taxi_df.query("pickupLatitude>=40.53 and pickupLatitude<=40.88")
final_df = final_df.query("pickupLongitude>=-74.09 and pickupLongitude<=-73.72")
final_df = final_df.query("tripDistance>=0.25 and tripDistance<31")
final_df = final_df.query("passengerCount>0 and totalAmount>0")

columns_to_remove_for_training = ["pickupLongitude", "pickupLatitude", "dropoffLongitude", "dropoffLatitude"]
for col in columns_to_remove_for_training:
    final_df.pop(col)
```

Temizlemenin beklendiği gibi çalıştığından emin olmak için verileri yeniden arayın. `describe()` Şimdi makine öğrenimi modeli eğitimi için kullanmak için taksi, tatil ve hava durumu verileri hazır ve temizlenmiş bir dizi var.

```python
final_df.describe()
```

## <a name="configure-workspace"></a>Çalışma alanını yapılandırma

Mevcut çalışma alanından bir çalışma alanı nesnesi oluşturun. [Çalışma Alanı,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) Azure aboneliğinizi ve kaynak bilgilerinizi kabul eden bir sınıftır. Ayrıca, model çalışmalarınızı izlemek ve izlemek için bir bulut kaynağı oluşturur. `Workspace.from_config()`**config.json** dosyasını okur ve kimlik doğrulama ayrıntılarını `ws`adlı bir nesneye yükler. Bu öğreticideki kodun kalanında `ws` kullanılır.

```python
from azureml.core.workspace import Workspace
ws = Workspace.from_config()
```

## <a name="split-the-data-into-train-and-test-sets"></a>Verileri tren ve test kümelerine bölme

`scikit-learn` Kitaplıktaki `train_test_split` işlevi kullanarak verileri eğitim ve test kümelerine bölün. Bu işlev, verileri model eğitimi için x (**özellikler)** veri kümesine ve test için veri kümesi ni **(tahmin edilmesi gereken değerler)** ayırır.

Parametre, `test_size` sınamaya ayrılacak verilerin yüzdesini belirler. Parametre `random_state` rastgele jeneratör için bir tohum ayarlar, böylece tren test bölmeleri deterministic vardır.

```python
from sklearn.model_selection import train_test_split

y_df = final_df.pop("totalAmount")
x_df = final_df

x_train, x_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=223)
```

Bu adımın amacı, doğru doğruluğu ölçmek için modeli eğitmek için kullanılmayan bitmiş modeli test etmek için veri noktalarına sahip olmaktır.

Başka bir deyişle, iyi eğitilmiş bir model, daha önce görmediği verilerden doğru tahminlerde bulunabilmeli. Artık bir makine öğrenme modelini otomatik olarak eğitmek için hazırlanmış verilere sahipsiniz.

## <a name="automatically-train-a-model"></a>Bir modeli otomatik olarak eğitin

Bir modeli otomatik olarak eğitmek için aşağıdaki adımları izleyin:
1. Deneme çalışması için ayarları tanımlayın. Eğitim verilerinizi yapılandırmaya iliştirin ve eğitim işlemini kontrol eden ayarları değiştirin.
1. Denemeyi model için gönderin. Denemeyi gönderdikten sonra, işlem farklı makine öğrenme algoritmaları ve hiperparametre ayarları aracılığıyla, tanımlanmış kısıtlamalarınıza bağlı kalarak yinelenir. Doğruluk ölçümlerini optimize ederek en uygun modeli seçer.

### <a name="define-training-settings"></a>Eğitim ayarlarını tanımlama

Eğitim için deneme parametresini ve model ayarlarını tanımlayın. [Ayarların](how-to-configure-auto-train.md)tam listesini görüntüleyin. Denemeyi bu varsayılan ayarlarla göndermek yaklaşık 5-20 dakika sürer, ancak daha kısa `experiment_timeout_minutes` bir çalışma süresi istiyorsanız, parametreyi azaltın.

|Özellik| Bu öğreticideki değer |Açıklama|
|----|----|---|
|**iteration_timeout_minutes**|2|Her yineleme için dakika içinde zaman sınırı. Toplam çalışma süresini azaltmak için bu değeri azaltın.|
|**experiment_timeout_minutes**|20|Tüm yinelemelerin biraraya getirebileceği dakika dakika içinde, deneme sona erdirilmeden önce maksimum süre.|
|**enable_early_stopping**|True|Puan kısa vadede iyileşmiyorsa erken sonlandırmayı etkinleştirmek için bayrak.|
|**primary_metric**| spearman_correlation | İyileştirmek istediğiniz ölçüm. En uygun model bu ölçüye göre seçilir.|
|**featurization**| auto | **Otomatik**kullanarak, deneme giriş verilerini önceden işleyebilir (eksik verileri işleme, metni sayısala dönüştürme, vb.)|
|**Ayrıntı**| logging.INFO | Günlüğe kaydetme düzeyini denetler.|
|**n_cross_validations**|5|Doğrulama verileri belirtilmediğinde gerçekleştirecek çapraz doğrulama bölmelerinin sayısı.|

```python
import logging

automl_settings = {
    "iteration_timeout_minutes": 2,
    "experiment_timeout_minutes": 20,
    "enable_early_stopping": True,
    "primary_metric": 'spearman_correlation',
    "featurization": 'auto',
    "verbosity": logging.INFO,
    "n_cross_validations": 5
}
```

Tanımlı eğitim ayarlarınızı `**kwargs` bir `AutoMLConfig` nesneye parametre olarak kullanın. Ayrıca, eğitim verilerinizi ve bu `regression` durumda olan model türünü belirtin.

```python
from azureml.train.automl import AutoMLConfig

automl_config = AutoMLConfig(task='regression',
                             debug_log='automated_ml_errors.log',
                             X=x_train.values,
                             y=y_train.values.flatten(),
                             **automl_settings)
```

> [!NOTE]
> Otomatik makine öğrenme ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısala dönüştürme vb.) temel modelin bir parçası haline gelir. Öngörüler için modeli kullanırken, eğitim sırasında uygulanan aynı ön işleme adımları giriş verilerinize otomatik olarak uygulanır.

### <a name="train-the-automatic-regression-model"></a>Otomatik regresyon modelini eğitin

Çalışma alanınızda bir deneme nesnesi oluşturun. Bir deneme, bireysel çalıştırmalarınız için bir kapsayıcı görevi görür. Tanımlanan `automl_config` nesneyi denemeye geçirin ve `True` çıktıyı çalışma sırasında ilerlemeyi görüntülemek için ayarlayın.

Denemebaşladıktan sonra, gösterilen çıktılar güncelleştirmeleri deneme çalışırken canlı olarak gösterir. Her yineleme için model türünü, çalışma süresini ve eğitim doğruluğunu görürsünüz. Alan, `BEST` metrik türünüze göre en iyi çalışan eğitim puanını izler.

```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, "taxi-experiment")
local_run = experiment.submit(automl_config, show_output=True)
```

    Running on local machine
    Parent Run ID: AutoML_1766cdf7-56cf-4b28-a340-c4aeee15b12b
    Current status: DatasetFeaturization. Beginning to featurize the dataset.
    Current status: DatasetEvaluation. Gathering dataset statistics.
    Current status: FeaturesGeneration. Generating features for the dataset.
    Current status: DatasetFeaturizationCompleted. Completed featurizing the dataset.
    Current status: DatasetCrossValidationSplit. Generating individually featurized CV splits.
    Current status: ModelSelection. Beginning model selection.

    ****************************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ****************************************************************************************************

     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   StandardScalerWrapper RandomForest             0:00:16       0.8746    0.8746
             1   MinMaxScaler RandomForest                      0:00:15       0.9468    0.9468
             2   StandardScalerWrapper ExtremeRandomTrees       0:00:09       0.9303    0.9468
             3   StandardScalerWrapper LightGBM                 0:00:10       0.9424    0.9468
             4   RobustScaler DecisionTree                      0:00:09       0.9449    0.9468
             5   StandardScalerWrapper LassoLars                0:00:09       0.9440    0.9468
             6   StandardScalerWrapper LightGBM                 0:00:10       0.9282    0.9468
             7   StandardScalerWrapper RandomForest             0:00:12       0.8946    0.9468
             8   StandardScalerWrapper LassoLars                0:00:16       0.9439    0.9468
             9   MinMaxScaler ExtremeRandomTrees                0:00:35       0.9199    0.9468
            10   RobustScaler ExtremeRandomTrees                0:00:19       0.9411    0.9468
            11   StandardScalerWrapper ExtremeRandomTrees       0:00:13       0.9077    0.9468
            12   StandardScalerWrapper LassoLars                0:00:15       0.9433    0.9468
            13   MinMaxScaler ExtremeRandomTrees                0:00:14       0.9186    0.9468
            14   RobustScaler RandomForest                      0:00:10       0.8810    0.9468
            15   StandardScalerWrapper LassoLars                0:00:55       0.9433    0.9468
            16   StandardScalerWrapper ExtremeRandomTrees       0:00:13       0.9026    0.9468
            17   StandardScalerWrapper RandomForest             0:00:13       0.9140    0.9468
            18   VotingEnsemble                                 0:00:23       0.9471    0.9471
            19   StackEnsemble                                  0:00:27       0.9463    0.9471

## <a name="explore-the-results"></a>Sonuçları inceleme

[Jupyter widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)ile otomatik eğitim sonuçlarını keşfedin. Widget, eğitim doğruluğu ölçümleri ve meta verilerle birlikte tüm tek tek çalışan yinelemelerin bir grafiğini ve tablosunu görmenizi sağlar. Ayrıca, açılır bırakma seçiciile birincil metrikten farklı doğruluk ölçümlerine filtre uygulayabilirsiniz.

```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Jupyter widget](./media/tutorial-auto-train-models/automl-dash-output.png)
![çalıştırmak ayrıntıları Jupyter widget arsa](./media/tutorial-auto-train-models/automl-chart-output.png)

### <a name="retrieve-the-best-model"></a>En iyi modeli alma

Yinelemelerinizden en iyi modeli seçin. Fonksiyon, `get_output` son uyum çağrısı için en iyi çalıştırmayı ve uygun modeli döndürür. Aşırı `get_output`yüklemeleri kullanarak, herhangi bir günlüğe kaydedilmiş metrik veya belirli bir yineleme için en iyi çalıştırma ve uygun modeli alabilirsiniz.

```python
best_run, fitted_model = local_run.get_output()
print(best_run)
print(fitted_model)
```

### <a name="test-the-best-model-accuracy"></a>En iyi model doğruluğunu test edin

Taksi ücretlerini tahmin etmek için test veri kümesindeki tahminleri çalıştırmak için en iyi modeli kullanın. İşlev `predict` en iyi modeli kullanır ve `x_test` veri kümesinden y, **yolculuk maliyeti**değerlerini tahmin eder. İlk 10 öngörülen maliyet değerlerini `y_predict`yazdırın.

```python
y_predict = fitted_model.predict(x_test.values)
print(y_predict[:10])
```

Sonuçların `root mean squared error` hesaplayın. Öngörülen `y_test` değerlerle karşılaştırmak için veri çerçevesini listeye dönüştürün. İşlev `mean_squared_error` iki değer dizisini alır ve aralarındaki ortalama kare hatasını hesaplar. Sonucun kare kökünü almak y değişkeni, **maliyet**ile aynı birimlerde hata verir. Bu kabaca ne kadar taksi ücreti tahminleri gerçek ücretler den gösterir.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

y_actual = y_test.values.flatten().tolist()
rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Tam `y_actual` ve `y_predict` veri kümelerini kullanarak ortalama mutlak yüzde hatasını (MAPE) hesaplamak için aşağıdaki kodu çalıştırın. Bu metrik, her tahmin edilen ve gerçek değer arasındaki mutlak farkı hesaplar ve tüm farklılıkları hesaplar. Daha sonra bu toplamı gerçek değerlerin toplamının yüzdesi olarak ifade eder.

```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

    Model MAPE:
    0.14353867606052823

    Model Accuracy:
    0.8564613239394718


İki tahmin doğruluğu ölçümünden, modelin genellikle +- $4,00 ve yaklaşık %15 hata içinde olan veri kümesinin özelliklerinden taksi ücretlerini tahmin etmede oldukça iyi olduğunu görürsünüz.

Geleneksel makine öğrenimi modeli geliştirme süreci son derece kaynak yoğundur ve düzinelerce modelin sonuçlarını çalıştırmak ve karşılaştırmak için önemli etki alanı bilgisi ve zaman yatırımı gerektirir. Otomatik makine öğrenimi kullanmak, senaryonuz için birçok farklı modeli hızla test etmenin harika bir yoludur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure Machine Learning eğitimlerini çalıştırmayı planlıyorsanız bu bölümü tamamlamayın.

### <a name="stop-the-compute-instance"></a>İşlem örneğini durdurma

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Her şeyi silme

Oluşturduğunuz kaynakları kullanmayı planlamamanız durumunda, bunları silin, böylece herhangi bir ücret ödemezsiniz.

1. Azure portalının en sol tarafındaki **Kaynak gruplarını** seçin.
1. Listeden oluşturduğunuz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin.
1. Kaynak grup adını girin. Ardından **Sil**’i seçin.

Kaynak grubunu da tutabilir, ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde aşağıdaki görevleri yaptınız:

> [!div class="checklist"]
> * Çalışma alanını yapılandırmış ve bir deneme için veri hazırlanmıştır.
> * Özel parametrelere sahip yerel otomatik bir regresyon modeli kullanılarak eğitildi.
> * Eğitim sonuçları araştırıldı ve gözden geçirildi.

Azure Machine Learning ile [modelinizi dağıtın.](tutorial-deploy-models-with-aml.md)
