---
title: 'Öğretici: Otomatikleştirilmiş makine öğrenimi modelini zenginleştirme'
titleSuffix: Azure Open Datasets
description: NYC TAXI tarifeli havayolu fiyatlarını tahmin etmek üzere bir gerileme modeli oluşturmak için Azure açık veri kümelerinin kullanımını ve Azure Machine Learning hizmetinin gücünden nasıl yararlanacağınızı öğrenin.
services: open-datasets
ms.service: open-datasets
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 05/02/2019
ms.openlocfilehash: 6f72daa4a601df0e3592910645c2f9b35ab64431
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845824"
---
# <a name="tutorial-build-a-regression-model-with-automated-machine-learning-and-open-datasets"></a>Öğretici: Otomatik makine öğrenimi ve açık veri kümeleri ile regresyon modeli oluşturma

Bu öğreticide, NYC TAXI tarifeli havayolu fiyatlarını tahmin etmek üzere bir gerileme modeli oluşturmak için Azure açık veri kümelerinin yanı da Azure Machine Learning hizmetin gücünden faydalanabilirsiniz. Genel olarak kullanılabilen TAXI, tatil ve hava durumu verilerini kolayca indirin ve Azure Machine Learning hizmeti kullanarak otomatik makine öğrenimi denemesi yapılandırın. Bu işlem, eğitim verilerini ve yapılandırma ayarlarını kabul eder ve en iyi modele ulaşmak için farklı özellik normalleştirme/standartlaştırma yöntemlerinin, modellerinin ve hiper parametre ayarlarının birleşimleri aracılığıyla otomatik olarak yinelenir.

Bu öğreticide aşağıdaki görevleri öğreneceksiniz:

- Bir Azure Machine Learning hizmeti çalışma alanında yapılandırın
- Yerel bir Python ortamı ayarlama
- Azure açık veri kümelerini kullanarak veriye erişme, dönüştürme ve bu verileri katma
- Otomatik makine öğrenimi regresyon modelini eğitme
- Model doğruluğunu hesapla

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici aşağıdaki önkoşulları gerektirir.

* Azure Machine Learning hizmet çalışma alanı
* Python 3,6 ortamı

### <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Henüz bir tane yoksa Azure portal bir çalışma alanı oluşturmak için [yönergeleri](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-workspace) izleyin. Oluşturulduktan sonra çalışma alanı adınızı, kaynak grubu adını ve abonelik KIMLIĞINI unutmayın.

### <a name="create-a-python-environment"></a>Python ortamı oluşturma

Bu örnek, jupi Not defterleri ile bir Anaconda ortamı kullanır, ancak bu kodu herhangi bir 3.6. x ortamında ve herhangi bir metin düzenleyici veya IDE ile çalıştırabilirsiniz. Yeni bir geliştirme ortamı oluşturmak için aşağıdaki adımları kullanın.

1. Henüz yoksa, Anaconda [indirip](https://www.anaconda.com/distribution/) yükleyin ve **Python 3,7 sürümünü**seçin.
1. Bir Anaconda istemi açın ve yeni bir ortam oluşturun. Bileşenler ve paketler indirilirken ortamın oluşturulması birkaç dakika sürer.
    ```
    conda create -n tutorialenv python=3.6.5
    ```
1. Ortamı etkinleştirin.
    ```
    conda activate tutorialenv
    ```
1. Ortama özel IPython kernels 'Yi etkinleştirin.
    ```
    conda install notebook ipykernel
    ```
1. Çekirdeği oluşturun.
    ```
    ipython kernel install --user
    ```
1. Bu öğretici için ihtiyacınız olan paketleri yükleyebilirsiniz. Bu paketler büyük olur ve yüklenmesi 5-10 dakika sürer.
    ```
    pip install azureml-sdk[automl] azureml-opendatasets
    ```
1. Ortamınızdan bir not defteri çekirdeği başlatın.
    ```
    jupyter notebook
    ```

Bu adımları tamamladıktan sonra, [açık veri kümeleri Not defteri](https://github.com/Azure/OpenDatasetsNotebooks) deposunu kopyalayın ve bu dosyayı çalıştırmak için **öğreticiler/Taxi-automl/01-tutorial-opendatasets-automl. ipynb** Not defterini açın.

## <a name="download-and-prepare-data"></a>Verileri indirme ve hazırlama

Gerekli paketleri içeri aktarın. Açık veri kümeleri paketi, indirilmeden önce tarih parametrelerini kolayca filtrelemek`NycTlcGreen` için her bir veri kaynağını temsil eden bir sınıf içerir (örneğin).


```python
from azureml.opendatasets import NycTlcGreen
import pandas as pd
from datetime import datetime
from dateutil.relativedelta import relativedelta
```

Taxı verilerini tutmak için bir veri çerçevesi oluşturarak başlayın. Spark olmayan bir ortamda çalışırken, açık veri kümeleri, büyük veri kümelerinde kaçınmak `MemoryError` için belirli sınıflarla tek seferde bir aydan verilerin indirilmesini sağlar. Bir yıllık TAXI verisi indirmek için, bir ay içinde bir ayda bir kez ve veri çerçevesini bloktan kaçınmak için her `green_taxi_df` aydan rastgele örnek 2000 kayıtlarına eklemeden önce bir ayı tekrarlayarak. Sonra verileri önizleyin.

>[!NOTE]
> Açık veri kümelerinde veri boyutunun ve belleğin sorun olmadığı Spark ortamlarında çalışmak için yansıtma sınıfları vardır.

```python
green_taxi_df = pd.DataFrame([])
start = datetime.strptime("1/1/2016", "%m/%d/%Y")
end = datetime.strptime("1/31/2016", "%m/%d/%Y")

for sample_month in range(12):
    temp_df_green = NycTlcGreen(start + relativedelta(months=sample_month), end + relativedelta(months=sample_month)) \
        .to_pandas_dataframe()
    green_taxi_df = green_taxi_df.append(temp_df_green.sample(2000))

green_taxi_df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>Konağında VendorID</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>passengerCount</th>
      <th>Üçlü mesafe</th>
      <th>Pulocationıd</th>
      <th>Dolocationıd</th>
      <th>Pickupboylam</th>
      <th>Pickupenlem</th>
      <th>Açılan Boylam</th>
      <th>...</th>
      <th>paymentType</th>
      <th>fareAmount</th>
      <th>ına</th>
      <th>Mtavergisi</th>
      <th>improvementSurcharge</th>
      <th>tipAmount</th>
      <th>tollsAmount</th>
      <th>Ehailücret</th>
      <th>totalAmount</th>
      <th>Üçlü tür</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20 17:38:28</td>
      <td>2016-01-20 17:46:33</td>
      <td>1\.</td>
      <td>0,98</td>
      <td>Yok.</td>
      <td>None</td>
      <td>-73,921715</td>
      <td>40,766682</td>
      <td>-73,916908</td>
      <td>...</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01 21:53:28</td>
      <td>2016-01-02 00:00:00</td>
      <td>1\.</td>
      <td>3,08</td>
      <td>Yok.</td>
      <td>None</td>
      <td>-73,979973</td>
      <td>40,677071</td>
      <td>-73,934349</td>
      <td>...</td>
      <td>2.0</td>
      <td>11,5</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01 00:50:23</td>
      <td>2016-01-01 01:05:37</td>
      <td>1\.</td>
      <td>2.44</td>
      <td>Yok.</td>
      <td>None</td>
      <td>-73,863045</td>
      <td>40,882923</td>
      <td>-73,839836</td>
      <td>...</td>
      <td>2.0</td>
      <td>12,5</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04 17:50:03</td>
      <td>2016-01-04 18:03:43</td>
      <td>1\.</td>
      <td>2,87</td>
      <td>Yok.</td>
      <td>None</td>
      <td>-73,977730</td>
      <td>40,684647</td>
      <td>-73,931259</td>
      <td>...</td>
      <td>1.0</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1\.</td>
      <td>2016-01-13 08:48:20</td>
      <td>2016-01-13 08:52:16</td>
      <td>1\.</td>
      <td>0.50</td>
      <td>Yok.</td>
      <td>Yok.</td>
      <td>-73,942589</td>
      <td>40,841423</td>
      <td>-73,943672</td>
      <td>...</td>
      <td>2.0</td>
      <td>4,5</td>
      <td>0.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>5.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1049431</th>
      <td>2</td>
      <td>2016-01-29 17:16:18</td>
      <td>2016-01-29 17:27:52</td>
      <td>1\.</td>
      <td>2.25</td>
      <td>None</td>
      <td>Yok.</td>
      <td>-73,830894</td>
      <td>40,759434</td>
      <td>-73,842422</td>
      <td>...</td>
      <td>2.0</td>
      <td>10.5</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>646563</th>
      <td>2</td>
      <td>2016-01-14 00:45:30</td>
      <td>2016-01-14 00:54:16</td>
      <td>1\.</td>
      <td>1.93</td>
      <td>Yok.</td>
      <td>Yok.</td>
      <td>-73,927109</td>
      <td>40,762848</td>
      <td>-73,909302</td>
      <td>...</td>
      <td>1.0</td>
      <td>8.5</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>9,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>438204</th>
      <td>1\.</td>
      <td>2016-01-09 14:25:02</td>
      <td>2016-01-09 14:32:48</td>
      <td>2</td>
      <td>0,80</td>
      <td>None</td>
      <td>Yok.</td>
      <td>-73,881195</td>
      <td>40,741779</td>
      <td>-73,872086</td>
      <td>...</td>
      <td>2.0</td>
      <td>6.5</td>
      <td>0.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>7.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>303784</th>
      <td>2</td>
      <td>2016-01-25 18:13:47</td>
      <td>2016-01-25 18:23:50</td>
      <td>1\.</td>
      <td>1,04</td>
      <td>None</td>
      <td>None</td>
      <td>-73,954376</td>
      <td>40,805729</td>
      <td>-73,939117</td>
      <td>...</td>
      <td>1.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>11,3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>269105</th>
      <td>2</td>
      <td>2016-01-24 20:46:50</td>
      <td>2016-01-24 21:04:03</td>
      <td>6</td>
      <td>2,82</td>
      <td>Yok.</td>
      <td>Yok.</td>
      <td>-73,845200</td>
      <td>40,722134</td>
      <td>-73,810638</td>
      <td>...</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>16,3</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>10 satır × 23 sütun</p>
</div>



Artık ilk veriler yüklendikten sonra, toplama tarih/saati alanından çeşitli zamana dayalı özellikler oluşturmak için bir işlev tanımlayın. Bu işlem, ay numarası, ay günü, haftanın günü ve günün saati için yeni alanlar oluşturur ve modelin zaman tabanlı mevsimsellik açısından çarpaya izin verir. İşlev Ayrıca, ülke kodu için tatil verilerini katmak üzere statik bir özellik ekler. İşlevi, TAXI verilerinde her satıra yinelemeli olarak `apply()` uygulamak için dataframe üzerindeki `build_time_features()` işlevini kullanın.


```python
def build_time_features(vector):
    pickup_datetime = vector[0]
    month_num = pickup_datetime.month
    day_of_month = pickup_datetime.day
    day_of_week = pickup_datetime.weekday()
    hour_of_day = pickup_datetime.hour
    country_code = "US"

    return pd.Series((month_num, day_of_month, day_of_week, hour_of_day, country_code))


green_taxi_df[["month_num", "day_of_month", "day_of_week", "hour_of_day", "country_code"]
              ] = green_taxi_df[["lpepPickupDatetime"]].apply(build_time_features, axis=1)
green_taxi_df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>Konağında VendorID</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>passengerCount</th>
      <th>Üçlü mesafe</th>
      <th>Pulocationıd</th>
      <th>Dolocationıd</th>
      <th>Pickupboylam</th>
      <th>Pickupenlem</th>
      <th>Açılan Boylam</th>
      <th>...</th>
      <th>tipAmount</th>
      <th>tollsAmount</th>
      <th>Ehailücret</th>
      <th>totalAmount</th>
      <th>Üçlü tür</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20 17:38:28</td>
      <td>2016-01-20 17:46:33</td>
      <td>1\.</td>
      <td>0,98</td>
      <td>Yok.</td>
      <td>None</td>
      <td>-73,921715</td>
      <td>40,766682</td>
      <td>-73,916908</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01 21:53:28</td>
      <td>2016-01-02 00:00:00</td>
      <td>1\.</td>
      <td>3,08</td>
      <td>Yok.</td>
      <td>Yok.</td>
      <td>-73,979973</td>
      <td>40,677071</td>
      <td>-73,934349</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,8</td>
      <td>1.0</td>
      <td>1</td>
      <td>1\.</td>
      <td>4</td>
      <td>21</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01 00:50:23</td>
      <td>2016-01-01 01:05:37</td>
      <td>1\.</td>
      <td>2.44</td>
      <td>Yok.</td>
      <td>Yok.</td>
      <td>-73,863045</td>
      <td>40,882923</td>
      <td>-73,839836</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
      <td>1</td>
      <td>1\.</td>
      <td>4</td>
      <td>0</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04 17:50:03</td>
      <td>2016-01-04 18:03:43</td>
      <td>1\.</td>
      <td>2,87</td>
      <td>Yok.</td>
      <td>Yok.</td>
      <td>-73,977730</td>
      <td>40,684647</td>
      <td>-73,931259</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1\.</td>
      <td>2016-01-13 08:48:20</td>
      <td>2016-01-13 08:52:16</td>
      <td>1\.</td>
      <td>0.50</td>
      <td>None</td>
      <td>None</td>
      <td>-73,942589</td>
      <td>40,841423</td>
      <td>-73,943672</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>5.3</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>1049431</th>
      <td>2</td>
      <td>2016-01-29 17:16:18</td>
      <td>2016-01-29 17:27:52</td>
      <td>1\.</td>
      <td>2.25</td>
      <td>Yok.</td>
      <td>None</td>
      <td>-73,830894</td>
      <td>40,759434</td>
      <td>-73,842422</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,3</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>29</td>
      <td>4</td>
      <td>17</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>646563</th>
      <td>2</td>
      <td>2016-01-14 00:45:30</td>
      <td>2016-01-14 00:54:16</td>
      <td>1\.</td>
      <td>1.93</td>
      <td>None</td>
      <td>Yok.</td>
      <td>-73,927109</td>
      <td>40,762848</td>
      <td>-73,909302</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>9,8</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>14</td>
      <td>3</td>
      <td>0</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>438204</th>
      <td>1\.</td>
      <td>2016-01-09 14:25:02</td>
      <td>2016-01-09 14:32:48</td>
      <td>2</td>
      <td>0,80</td>
      <td>None</td>
      <td>Yok.</td>
      <td>-73,881195</td>
      <td>40,741779</td>
      <td>-73,872086</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>7.3</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>9</td>
      <td>5</td>
      <td>14</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>303784</th>
      <td>2</td>
      <td>2016-01-25 18:13:47</td>
      <td>2016-01-25 18:23:50</td>
      <td>1</td>
      <td>1,04</td>
      <td>None</td>
      <td>Yok.</td>
      <td>-73,954376</td>
      <td>40,805729</td>
      <td>-73,939117</td>
      <td>...</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>11,3</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>25</td>
      <td>0</td>
      <td>18</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>269105</th>
      <td>2</td>
      <td>2016-01-24 20:46:50</td>
      <td>2016-01-24 21:04:03</td>
      <td>6</td>
      <td>2,82</td>
      <td>Yok.</td>
      <td>Yok.</td>
      <td>-73,845200</td>
      <td>40,722134</td>
      <td>-73,810638</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>16,3</td>
      <td>1.0</td>
      <td>1\.</td>
      <td>24</td>
      <td>6</td>
      <td>20</td>
      <td>ABD</td>
    </tr>
  </tbody>
</table>
<p>10 satır × 28 sütun</p>
</div>

Modelleme veya ek özellik oluşturma için ihtiyaç duymayabilmeniz gereken bazı sütunları kaldırın. Toplama zamanı için saat alanını yeniden adlandırın ve ek olarak saati kullanarak `pandas.Series.dt.normalize`gece yarısına dönüştürün. Bu, tüm zaman özelliklerine göre yapılır, böylece tarih saat bileşeni daha sonra bir günlük ayrıntı düzeyi düzeyinde bir araya birleştirilirken bir anahtar olarak kullanılabilir.

```python
columns_to_remove = ["lpepDropoffDatetime", "puLocationId", "doLocationId", "extra", "mtaTax",
                     "improvementSurcharge", "tollsAmount", "ehailFee", "tripType", "rateCodeID",
                     "storeAndFwdFlag", "paymentType", "fareAmount", "tipAmount"
                     ]
for col in columns_to_remove:
    green_taxi_df.pop(col)

green_taxi_df = green_taxi_df.rename(
    columns={"lpepPickupDatetime": "datetime"})
green_taxi_df["datetime"] = green_taxi_df["datetime"].dt.normalize()
green_taxi_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>Konağında VendorID</th>
      <th>datetime</th>
      <th>passengerCount</th>
      <th>Üçlü mesafe</th>
      <th>Pickupboylam</th>
      <th>Pickupenlem</th>
      <th>Açılan Boylam</th>
      <th>Açılan Enlem</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20</td>
      <td>1</td>
      <td>0,98</td>
      <td>-73,921715</td>
      <td>40,766682</td>
      <td>-73,916908</td>
      <td>40,761257</td>
      <td>8.8</td>
      <td>1\.</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1\.</td>
      <td>3,08</td>
      <td>-73,979973</td>
      <td>40,677071</td>
      <td>-73,934349</td>
      <td>40,671654</td>
      <td>12,8</td>
      <td>1\.</td>
      <td>1\.</td>
      <td>4</td>
      <td>21</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1\.</td>
      <td>2.44</td>
      <td>-73,863045</td>
      <td>40,882923</td>
      <td>-73,839836</td>
      <td>40,868336</td>
      <td>13,8</td>
      <td>1\.</td>
      <td>1\.</td>
      <td>4</td>
      <td>0</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04</td>
      <td>1\.</td>
      <td>2,87</td>
      <td>-73,977730</td>
      <td>40,684647</td>
      <td>-73,931259</td>
      <td>40,694248</td>
      <td>13,8</td>
      <td>1\.</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>ABD</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1</td>
      <td>2016-01-13</td>
      <td>1\.</td>
      <td>0.50</td>
      <td>-73,942589</td>
      <td>40,841423</td>
      <td>-73,943672</td>
      <td>40,834396</td>
      <td>5.3</td>
      <td>1\.</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>ABD</td>
    </tr>
  </tbody>
</table>
</div>

### <a name="enrich-with-holiday-data"></a>Tatil verileriyle zenginleştirme

Artık ve kabaca hazırlanan vergilenmm verisi olduğuna göre, tatil verilerini ek özellikler olarak ekleyin. Büyük tatiller, TAXI talebinin önemli ölçüde arttığı ve tedarikin sınırlı olacağı durumlar olduğu sürece, tatil açısından özel özellikler model doğruluğuna yardımcı olur. Tatil veri kümesi görece küçük olduğundan, filtre uygulamak üzere parametre olmadan `PublicHolidays` sınıf oluşturucusunu kullanarak tam kümeyi getirin. Biçimlendirmeyi denetlemek için verileri önizleyin.

```python
from azureml.opendatasets import PublicHolidays
# call default constructor to download full dataset
holidays_df = PublicHolidays().to_pandas_dataframe()
holidays_df.head(5)
```

    ActivityStarted, to_pandas_dataframe
    Looking for parquet files...
    Reading them into Pandas dataframe...
    Reading Processed/part-00000-tid-1353805596865908763-9ee4e95b-0d55-4292-addd-a0e19d7c32cb-3559-c000.snappy.parquet under container holidaydatacontainer
    Done.
    ActivityCompleted: Activity=to_pandas_dataframe, HowEnded=Success, Duration=1799.89 [ms]

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>countryOrRegion</th>
      <th>holidayName</th>
      <th>ıpaidtimeoff</th>
      <th>countryRegionCode</th>
      <th>normalizeHolidayName</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40688</th>
      <td>Arnavutluk</td>
      <td>Yılbaşı günü</td>
      <td>Yok.</td>
      <td>AL</td>
      <td>Yılbaşı günü</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40689</th>
      <td>Cezayir</td>
      <td>Yılbaşı günü</td>
      <td>Yok.</td>
      <td>DZ</td>
      <td>Yılbaşı günü</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40690</th>
      <td>Andorra</td>
      <td>Yılbaşı günü</td>
      <td>None</td>
      <td>AD</td>
      <td>Yılbaşı günü</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40691</th>
      <td>Angola</td>
      <td>Yılbaşı günü</td>
      <td>Yok.</td>
      <td>AO</td>
      <td>Yılbaşı günü</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40692</th>
      <td>Arjantin</td>
      <td>Yılbaşı günü</td>
      <td>Yok.</td>
      <td>AR</td>
      <td>Yılbaşı günü</td>
      <td>2008-01-01</td>
    </tr>
  </tbody>
</table>
</div>



`countryRegionCode` Ve`date` sütunlarını, TAXI verilerinden ilgili alan adlarıyla eşleşecek şekilde yeniden adlandırın ve aynı zamanda anahtar olarak kullanılabilmesi için süreyi normalleştirin. Ardından, Pandas `merge()` işlevini kullanarak bir sol katın gerçekleştirerek, vergileni verileriyle tatil verilerini birleştirin. Bu `green_taxi_df`, tüm kayıtları korur, ancak karşılık gelen `datetime` ve `country_code`bu durumda her zaman `"US"`olduğu gibi tatil verilerini ekler. Doğru birleştirildiklerini doğrulamak için verileri önizleyin.

```python
holidays_df = holidays_df.rename(
    columns={"countryRegionCode": "country_code", "date": "datetime"})
holidays_df["datetime"] = holidays_df["datetime"].dt.normalize()
holidays_df.pop("countryOrRegion")
holidays_df.pop("holidayName")

taxi_holidays_df = pd.merge(green_taxi_df, holidays_df, how="left", on=[
                            "datetime", "country_code"])
taxi_holidays_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>Konağında VendorID</th>
      <th>datetime</th>
      <th>passengerCount</th>
      <th>Üçlü mesafe</th>
      <th>Pickupboylam</th>
      <th>Pickupenlem</th>
      <th>Açılan Boylam</th>
      <th>Açılan Enlem</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
      <th>ıpaidtimeoff</th>
      <th>normalizeHolidayName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2016-01-20</td>
      <td>1\.</td>
      <td>0,98</td>
      <td>-73,921715</td>
      <td>40,766682</td>
      <td>-73,916908</td>
      <td>40,761257</td>
      <td>8.8</td>
      <td>1\.</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>ABD</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1\.</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1\.</td>
      <td>3,08</td>
      <td>-73,979973</td>
      <td>40,677071</td>
      <td>-73,934349</td>
      <td>40,671654</td>
      <td>12,8</td>
      <td>1\.</td>
      <td>1\.</td>
      <td>4</td>
      <td>21</td>
      <td>ABD</td>
      <td>Doğru</td>
      <td>Yılbaşı günü</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1\.</td>
      <td>2.44</td>
      <td>-73,863045</td>
      <td>40,882923</td>
      <td>-73,839836</td>
      <td>40,868336</td>
      <td>13,8</td>
      <td>1\.</td>
      <td>1\.</td>
      <td>4</td>
      <td>0</td>
      <td>ABD</td>
      <td>Doğru</td>
      <td>Yılbaşı günü</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2016-01-04</td>
      <td>1\.</td>
      <td>2,87</td>
      <td>-73,977730</td>
      <td>40,684647</td>
      <td>-73,931259</td>
      <td>40,694248</td>
      <td>13,8</td>
      <td>1\.</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>ABD</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1\.</td>
      <td>2016-01-13</td>
      <td>1\.</td>
      <td>0.50</td>
      <td>-73,942589</td>
      <td>40,841423</td>
      <td>-73,943672</td>
      <td>40,834396</td>
      <td>5.3</td>
      <td>1\.</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>ABD</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

### <a name="enrich-with-weather-data"></a>Hava durumu verileriyle zenginleştirme

Şimdi, TAXI ve tatil verilerine NOAA Surface Hava durumu verilerini ekleyin. Her seferinde bir ay indirerek Hava durumu verilerini getirmek için benzer bir yaklaşım kullanın. Ayrıca, indirmek istediğiniz `cols` sütunları filtrelemek için bir dizeler dizisi ile parametresini belirtin. Bu, dünyanın her yerinden hava durumu yüzey verilerini içeren çok büyük bir veri kümesidir. bu nedenle, her ay eklemeden önce, enlem/Long alanlarını dataframe üzerindeki `query()` işlevini kullanarak NYC yakınında olacak şekilde filtreleyin. Bu, `weather_df` çok büyük olmadığından emin olur.

```python
from azureml.opendatasets import NoaaIsdWeather

weather_df = pd.DataFrame([])
start = datetime.strptime("1/1/2016", "%m/%d/%Y")
end = datetime.strptime("1/31/2016", "%m/%d/%Y")

for sample_month in range(12):
    tmp_df = NoaaIsdWeather(cols=["temperature", "precipTime", "precipDepth", "snowDepth"], start_date=start + relativedelta(months=sample_month), end_date=end + relativedelta(months=sample_month))\
        .to_pandas_dataframe()
    print("--weather downloaded--")

    # filter out coordinates not in NYC to conserve memory
    tmp_df = tmp_df.query("latitude>=40.53 and latitude<=40.88")
    tmp_df = tmp_df.query("longitude>=-74.09 and longitude<=-73.72")
    print("--filtered coordinates--")
    weather_df = weather_df.append(tmp_df)

weather_df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>wban dili</th>
      <th>precipTime</th>
      <th>Kar derinliği</th>
      <th>sıcaklık</th>
      <th>Enlem</th>
      <th>precipDepth</th>
      <th>Boylam</th>
      <th>datetime</th>
      <th>USAF</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1754979</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>7.2</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 00:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754980</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.7</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 01:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754981</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.7</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 02:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754982</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.1</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 03:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754983</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.6</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 04:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754984</th>
      <td>94741</td>
      <td>24,0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40,85</td>
      <td>5.0</td>
      <td>-74,061</td>
      <td>2016-01-01 04:59:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754985</th>
      <td>94741</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40,85</td>
      <td>NaN</td>
      <td>-74,061</td>
      <td>2016-01-01 04:59:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754986</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.6</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 05:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754987</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 06:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754988</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>-74,061</td>
      <td>2016-01-01 07:51:00</td>
      <td>725025</td>
    </tr>
  </tbody>
</table>
</div>

Hava durumu `pandas.Series.dt.normalize` verilerinde, `datetime` içindeki `taxi_holidays_df`zaman anahtarıyla eşleşecek şekilde bu alana çağrı yapın. Gereksiz sütunları silin ve sıcaklığın olduğu `NaN`kayıtları filtreleyin.

Daha sonra günlük toplam hava durumu değerlerinizin olması için hava durumu verilerini gruplandırın. Her bir alanın günlük `aggregations` düzeyde nasıl toplanacağını tanımlamak için bir dict tanımlayın. İçin, ve için `precipTime` ortalama vegünlükenyüksekdeğerialın.`precipDepth` `snowDepth` `temperature` Verileri gruplandırmak için toplamaların birlikte işlevinikullanın.`groupby()` Günde bir kayıt olduğundan emin olmak için verileri önizleyin.

```python
weather_df["datetime"] = weather_df["datetime"].dt.normalize()
weather_df.pop("usaf")
weather_df.pop("wban")
weather_df.pop("longitude")
weather_df.pop("latitude")

# filter out NaN
weather_df = weather_df.query("temperature==temperature")

# group by datetime
aggregations = {"snowDepth": "mean", "precipTime": "max",
                "temperature": "mean", "precipDepth": "max"}
weather_df_grouped = weather_df.groupby("datetime").agg(aggregations)
weather_df_grouped.head(10)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>Kar derinliği</th>
      <th>precipTime</th>
      <th>sıcaklık</th>
      <th>precipDepth</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-01</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>5,197345</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-02</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>2,567857</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-03</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>3,846429</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-04</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>0,123894</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>-7,206250</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>-0,896396</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>3,180645</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>4,384091</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-09</th>
      <td>NaN</td>
      <td>6.0</td>
      <td>6,710274</td>
      <td>3,0</td>
    </tr>
    <tr>
      <th>2016-01-10</th>
      <td>NaN</td>
      <td>24,0</td>
      <td>10,943655</td>
      <td>254,0</td>
    </tr>
  </tbody>
</table>
</div>

> [!NOTE]
> Bu öğreticideki örnekler, Pandas işlevlerini ve özel toplamaları kullanarak verileri birleştirir, ancak açık veri kümeleri SDK 'Sı, veri kümelerini kolayca birleştirmek ve zenginleştirme amacıyla tasarlanan sınıflara sahiptir. Bu Tasarım desenlerinin kod örnekleri için [Not defterine](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) bakın.

### <a name="cleanse-data"></a>Verilerini temizlemek

Yeni hava durumu verileriyle hazırladığınız TAXI ve tatil verilerini birleştirin. Bu kez yalnızca `datetime` anahtara ihtiyacınız vardır ve verilerin bir LEFT-JOIN işlemini gerçekleştirirsiniz. Her bir alanın Özet istatistiklerini görmek için bu işleviyenidataframeüzerindeçalıştırın.`describe()`

```python
taxi_holidays_weather_df = pd.merge(
    taxi_holidays_df, weather_df_grouped, how="left", on=["datetime"])
taxi_holidays_weather_df.describe()
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>Konağında VendorID</th>
      <th>passengerCount</th>
      <th>Üçlü mesafe</th>
      <th>Pickupboylam</th>
      <th>Pickupenlem</th>
      <th>Açılan Boylam</th>
      <th>Açılan Enlem</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>Kar derinliği</th>
      <th>precipTime</th>
      <th>sıcaklık</th>
      <th>precipDepth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>1671,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
      <td>24000,000000</td>
    </tr>
    <tr>
      <th>Ortalama</th>
      <td>1,786583</td>
      <td>6,576208</td>
      <td>1,582588</td>
      <td>20,505491</td>
      <td>84,936413</td>
      <td>-36,232825</td>
      <td>21,723144</td>
      <td>7,863018</td>
      <td>6,500000</td>
      <td>15,113708</td>
      <td>3,240250</td>
      <td>13,664125</td>
      <td>11,764141</td>
      <td>13,258875</td>
      <td>13,903524</td>
      <td>1056,644458</td>
    </tr>
    <tr>
      <th>STD</th>
      <td>0,409728</td>
      <td>9,086857</td>
      <td>2,418177</td>
      <td>108,847821</td>
      <td>70,678506</td>
      <td>37,650276</td>
      <td>19,104384</td>
      <td>10,648766</td>
      <td>3,452124</td>
      <td>8,485155</td>
      <td>1,956895</td>
      <td>6,650676</td>
      <td>15,651884</td>
      <td>10,339720</td>
      <td>9,474396</td>
      <td>2815,592754</td>
    </tr>
    <tr>
      <th>dk</th>
      <td>1,000000</td>
      <td>-60,000000</td>
      <td>-1,000000</td>
      <td>-74,179482</td>
      <td>0.000000</td>
      <td>-74,190704</td>
      <td>0.000000</td>
      <td>-52,800000</td>
      <td>1,000000</td>
      <td>1,000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>3,000000</td>
      <td>1,000000</td>
      <td>-13,379464</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>%25</th>
      <td>2,000000</td>
      <td>1,000000</td>
      <td>0,330000</td>
      <td>-73,946680</td>
      <td>40,717712</td>
      <td>-73,945429</td>
      <td>1,770000</td>
      <td>1,000000</td>
      <td>3,750000</td>
      <td>8,000000</td>
      <td>2,000000</td>
      <td>9,000000</td>
      <td>3,000000</td>
      <td>1,000000</td>
      <td>6,620773</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2,000000</td>
      <td>4,000000</td>
      <td>0,830000</td>
      <td>1.500000</td>
      <td>40,814129</td>
      <td>0,500000</td>
      <td>21,495000</td>
      <td>2,000000</td>
      <td>6,500000</td>
      <td>15,000000</td>
      <td>3,000000</td>
      <td>15,000000</td>
      <td>4,428571</td>
      <td>6,000000</td>
      <td>13,090753</td>
      <td>10,000000</td>
    </tr>
    <tr>
      <th>% 75</th>
      <td>2,000000</td>
      <td>9,000000</td>
      <td>1,870000</td>
      <td>89,000000</td>
      <td>129,000000</td>
      <td>1,000000</td>
      <td>40,746146</td>
      <td>11,300000</td>
      <td>9,250000</td>
      <td>22,000000</td>
      <td>5,000000</td>
      <td>19,000000</td>
      <td>12,722222</td>
      <td>24,000000</td>
      <td>22,944737</td>
      <td>132,000000</td>
    </tr>
    <tr>
      <th>en çok</th>
      <td>2,000000</td>
      <td>460,000000</td>
      <td>51,950000</td>
      <td>265,000000</td>
      <td>265,000000</td>
      <td>6,000000</td>
      <td>58,600000</td>
      <td>498,000000</td>
      <td>12,000000</td>
      <td>30,000000</td>
      <td>6,000000</td>
      <td>23,000000</td>
      <td>67,090909</td>
      <td>24,000000</td>
      <td>31,303665</td>
      <td>9999,000000</td>
    </tr>
  </tbody>
</table>
</div>

Özet istatistiklerinde, mantıksal veya daha fazla alan içeren birkaç alanın, model doğruluğunu azaltacak şekilde olduğunu görürsünüz. İlk olarak, Lat/Long alanlarını, hava durumu verilerini filtrelemek için kullandığınız sınırlar dahilinde olacak şekilde filtreleyin. En küçük değer negatif olduğundan, alandabazıhatalıverilervardır.`tripDistance` `passengerCount` Alan, en büyük değer olan 210 pasılcılar olacak şekilde hatalı veriler de içerir. Son olarak, `totalAmount` alanın, modelimizin bağlamında anlamamakta olmayan negatif değerleri vardır.

Sorgu işlevlerini kullanarak bu anomali filtreleme yapın ve ardından eğitim için gereksiz son sütunu kaldırın.

```python
final_df = taxi_holidays_weather_df.query(
    "pickupLatitude>=40.53 and pickupLatitude<=40.88")
final_df = final_df.query(
    "pickupLongitude>=-74.09 and pickupLongitude<=-73.72")
final_df = final_df.query("tripDistance>0 and tripDistance<75")
final_df = final_df.query("passengerCount>0 and passengerCount<100")
final_df = final_df.query("totalAmount>0")

columns_to_remove_for_training = ["datetime", "pickupLongitude",
                                  "pickupLatitude", "dropoffLongitude", "dropoffLatitude", "country_code"]
for col in columns_to_remove_for_training:
    final_df.pop(col)
```

Temizleme `describe()` 'nin beklendiği gibi çalıştığından emin olmak için verileri yeniden çağırın. Artık makine öğrenimi model eğitimi için kullanılacak, hazırlanmış ve yeniden hazırlanan bir TAXI, tatil ve hava durumu verisi ayarlamış olursunuz.

```python
final_df.describe()
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

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
      <th>Konağında VendorID</th>
      <th>passengerCount</th>
      <th>Üçlü mesafe</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>Kar derinliği</th>
      <th>precipTime</th>
      <th>sıcaklık</th>
      <th>precipDepth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>1490,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
      <td>11765,000000</td>
    </tr>
    <tr>
      <th>Ortalama</th>
      <td>1,786910</td>
      <td>1,343476</td>
      <td>2,848488</td>
      <td>14,689039</td>
      <td>3,499788</td>
      <td>14,948916</td>
      <td>3,234254</td>
      <td>13,647344</td>
      <td>12,508581</td>
      <td>11,855929</td>
      <td>10,301433</td>
      <td>208,432384</td>
    </tr>
    <tr>
      <th>STD</th>
      <td>0,409508</td>
      <td>1,001232</td>
      <td>2,895960</td>
      <td>10,289832</td>
      <td>1,707865</td>
      <td>8,442438</td>
      <td>1,958477</td>
      <td>6,640280</td>
      <td>16,203195</td>
      <td>10,125701</td>
      <td>8,553512</td>
      <td>1284,892832</td>
    </tr>
    <tr>
      <th>dk</th>
      <td>1,000000</td>
      <td>1,000000</td>
      <td>0,010000</td>
      <td>3,300000</td>
      <td>1,000000</td>
      <td>1,000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>3,000000</td>
      <td>1,000000</td>
      <td>-13,379464</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>%25</th>
      <td>2,000000</td>
      <td>1,000000</td>
      <td>1,070000</td>
      <td>8,160000</td>
      <td>2,000000</td>
      <td>8,000000</td>
      <td>2,000000</td>
      <td>9,000000</td>
      <td>3,000000</td>
      <td>1,000000</td>
      <td>3,504580</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2,000000</td>
      <td>1,000000</td>
      <td>1,900000</td>
      <td>11,300000</td>
      <td>3,000000</td>
      <td>15,000000</td>
      <td>3,000000</td>
      <td>15,000000</td>
      <td>4,250000</td>
      <td>6,000000</td>
      <td>10,168182</td>
      <td>3,000000</td>
    </tr>
    <tr>
      <th>% 75</th>
      <td>2,000000</td>
      <td>1,000000</td>
      <td>3,550000</td>
      <td>17,800000</td>
      <td>5,000000</td>
      <td>22,000000</td>
      <td>5,000000</td>
      <td>19,000000</td>
      <td>15,647059</td>
      <td>24,000000</td>
      <td>16,966923</td>
      <td>41,000000</td>
    </tr>
    <tr>
      <th>en çok</th>
      <td>2,000000</td>
      <td>6,000000</td>
      <td>51,950000</td>
      <td>150,300000</td>
      <td>6,000000</td>
      <td>30,000000</td>
      <td>6,000000</td>
      <td>23,000000</td>
      <td>67,090909</td>
      <td>24,000000</td>
      <td>26,524107</td>
      <td>9999,000000</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="train-a-model"></a>Modeli eğitme

Artık hazırlanan verileri otomatik makine öğrenimi modelini eğitmek için kullanırsınız. Bu model için `final_df` TAXI tarifeli havayolu maliyeti olan Özellikler (X değerleri) ve Etiketler (y değeri) ile ayırarak başlayın.

```python
y_df = final_df.pop("totalAmount")
x_df = final_df
```

Artık, `train_test_split()` `scikit-learn` kitaplıktaki işlevini kullanarak verileri eğitim ve test kümelerine bölecektir. `test_size` Parametresi test ayrılacak veri yüzdesini belirler. `random_state` Parametresi, tren-test bölünmeleri belirleyici olacak şekilde rastgele sayı oluşturucusuna bir çekirdek ayarlar.


```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    x_df, y_df, test_size=0.2, random_state=222)
```

### <a name="load-workspace-and-configure-experiment"></a>Çalışma alanını yükleme ve deneme yapılandırma

Aboneliğiniz ve çalışma alanı bilgileriniz ile `get()` işlevini kullanarak Azure Machine Learning hizmeti çalışma alanınızı yükleyin. Modelinizin depolamak ve izlemek için çalışma alanınızda bir deneme oluşturun.


```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment

workspace = Workspace.get(subscription_id="<your-subscription-id>",
                          name="<your-workspace-name>", resource_group="<your-resource-group>")
experiment = Experiment(workspace, "opendatasets-ml")
```

`AutoMLConfig` Sınıfını kullanarak deneme için bir yapılandırma nesnesi oluşturun. Eğitim verilerinizi iliştirmeye ve ayrıca eğitim sürecini denetleyen ayarları ve parametreleri de belirtebilirsiniz. Parametreler aşağıdaki amaçlara sahiptir:

* `task`: çalıştırılacak denemenin türü.
* `X`: eğitim özellikleri.
* `y`: eğitim etiketleri.
* `iterations`: çalıştırılacak yineleme sayısı. Her yineleme, farklı özellik normalleştirme/standartlaştırma yöntemlerinin birleşimlerini ve birden çok hiper parametre ayarı kullanan farklı modelleri dener.
* `primary_metric`: model eğitimi sırasında optimize edilecek birincil ölçüm. Bu ölçüm temelinde en uygun model seçilecek.
* `preprocess`: denemenin giriş verilerini önceden işleyebilir (eksik verileri işleme, metni sayısal olarak dönüştürme vb.)
* `n_cross_validations`: Doğrulama verileri belirtilmediğinde gerçekleştirilecek çapraz doğrulama bölme sayısı.


```python
from azureml.train.automl import AutoMLConfig

automl_config = AutoMLConfig(task="regression",
                             X=X_train.values,
                             y=y_train.values.flatten(),
                             iterations=20,
                             primary_metric="spearman_correlation",
                             preprocess=True,
                             n_cross_validations=5
                             )
```

### <a name="submit-experiment"></a>Denemeyi gönderme

Eğitim için denemeyi iletin. Deneme gönderdikten sonra işlem, farklı makine öğrenimi algoritmalarını ve hiper parametre ayarlarını kullanarak, tanımlı kısıtlamalarınız için uygun şekilde yinelenir. Tanımlanan doğruluk ölçüsünü en iyi duruma getirerek en uygun modeli seçer. `automl_config` Nesneyi denemenize geçirin. Deneme sırasında ilerlemeyi görüntülemek `True` için çıktıyı ayarlayın.

Denemeyi gönderdikten sonra eğitim süreci için canlı çıktıyı görürsünüz. Her yineleme için model türü ve özellik normalleştirme/standartlaştırma yöntemi, çalışma süresi ve eğitim doğruluğunu görürsünüz. Alan `BEST` , ölçüm türünüz temelinde en iyi çalışan eğitim Puanını izler.

```python
training_run = experiment.submit(automl_config, show_output=True)
```

    Running on local machine
    Parent Run ID: AutoML_5c35f2a7-e479-4e7f-a131-ed4cb51e29d1
    Current status: DatasetEvaluation. Gathering dataset statistics.
    Current status: FeaturesGeneration. Generating features for the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetCrossValidationSplit. Generating individually featurized CV splits.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: ModelSelection. Beginning model selection.

    ****************************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ****************************************************************************************************

     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler RandomForest                      0:00:07       0.9081    0.9081
             1   StandardScalerWrapper DecisionTree             0:00:05       0.9121    0.9121
             2   StandardScalerWrapper LightGBM                 0:00:04       0.9318    0.9318
             3   StandardScalerWrapper LightGBM                 0:00:07       0.9286    0.9318
             4   MaxAbsScaler LightGBM                          0:00:05       0.9246    0.9318
             5   MaxAbsScaler LightGBM                          0:00:05       0.9199    0.9318
             6   MaxAbsScaler RandomForest                      0:00:07       0.9327    0.9327
             7   StandardScalerWrapper ElasticNet               0:00:04       0.9371    0.9371
             8   MaxAbsScaler LightGBM                          0:00:05       0.9327    0.9371
             9   MaxAbsScaler SGD                               0:00:04       0.9077    0.9371
            10   MaxAbsScaler LightGBM                          0:00:04       0.9340    0.9371
            11   StandardScalerWrapper LightGBM                 0:00:04       0.8301    0.9371
            12   MaxAbsScaler DecisionTree                      0:00:05       0.9214    0.9371
            13   StandardScalerWrapper DecisionTree             0:00:04       0.9201    0.9371
            14   MaxAbsScaler DecisionTree                      0:00:05       0.9179    0.9371
            15   MaxAbsScaler ExtremeRandomTrees                0:00:05       0.9052    0.9371
            16   StandardScalerWrapper DecisionTree             0:00:04       0.9282    0.9371
            17   StandardScalerWrapper ElasticNet               0:00:04       0.9319    0.9371
            18   VotingEnsemble                                 0:00:16       0.9380    0.9380
            19   StackEnsemble                                  0:00:17       0.9376    0.9380

### <a name="retrieve-the-fitted-model"></a>Sığdırılmış modeli alma

Tüm eğitim yinelemelerinin sonunda, otomatik makine öğrenimi işlemi, tüm bireysel çalıştırmaların, Bagging veya yığınlama ile bir ensebir algoritma oluşturur. Değişkene `fitted_model`uydurulur ve en iyi bireyi değişkende `best_run`alın.

```python
best_run, fitted_model = training_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="test-model-accuracy"></a>Test modeli doğruluğu

Taksi Fares 'yi tahmin etmek için test veri kümesindeki tahminleri çalıştırmak üzere, bağlı olan bir modeli kullanın. İşlevi `predict()` , monte edilen modeli kullanır ve `X_test` veri kümesi için y, TAXI tarifeli havayolu Cost değerlerini tahmin eder.


```python
y_predict = fitted_model.predict(X_test.values)
```

Sonuçların kök ortalama kare hatası sayısını hesaplayın. Veri çerçevesini kullanın ve tahmin edilen değerlerle karşılaştırmak için bir listeye `y_actual` dönüştürün. `y_test` İşlevi `mean_squared_error` iki dizi değer alır ve aralarındaki ortalama kare içinde hata sayısını hesaplar. Sonucun kare kökünü almak, y değişkeni ile aynı birimlerde bir hata verir. EPI tarifeli havayolu öngörülerinin gerçek farlıklardan ne kadar ilerlediğini, büyük bir süre içinde çok büyük bir hata olduğunu gösterir.


```python
from sklearn.metrics import mean_squared_error
from math import sqrt

y_actual = y_test.values.flatten().tolist()
rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```




    4.178568987067901



Tam `y_actual` ve`y_predict` veri kümelerini kullanarak ortalama mutlak yüzde hatasını (mape) hesaplamak için aşağıdaki kodu çalıştırın. Bu ölçüm, tahmin edilen ve gerçek değerler arasındaki mutlak bir farkı hesaplar ve tüm farkları toplar. Ardından, bu toplamı gerçek değerlerin toplamının yüzdesi olarak ifade eder.


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
    0.14923619644924357

    Model Accuracy:
    0.8507638035507564

Tam veri kümesine (n = 11748) göre çok küçük bir veri örneği kullandıysanız, model doğruluğu% 85 ' de oldukça yüksektir ve bu, TAXI tarifeli havayolu fiyatında tahmine dayalı olarak +-$4,00 hatası ile birlikte. Doğruluğu artırmak için olası bir sonraki adım olarak, bu not defterinin ikinci hücresine geri dönün ve ayda 2.000 kayıttan örnek boyutunu artırın ve daha fazla veri ile modeli yeniden eğmek için tüm denemeyi yeniden çalıştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, herhangi bir ücret ödemezsiniz.

1. Azure portalının en sol tarafındaki **Kaynak gruplarını** seçin.
1. Listeden oluşturduğunuz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin.
1. Kaynak grubu adını girin. Ardından **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla kod örneği için bkz. Azure açık veri kümeleri [Not defterleri](https://github.com/Azure/OpenDatasetsNotebooks) .
* Azure Machine Learning hizmetinde otomatik makine öğrenimi hakkında daha fazla bilgi için [bkz. nasıl yapılır?](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train) .
