---
title: 'Regresyon modeli öğreticisi: Verileri hazırlama'
titleSuffix: Azure Machine Learning service
description: Bu öğreticinin ilk bölümünde, Azure Machine Learning SDK kullanarak regresyon modellemesi için Python 'da verileri nasıl hazırlayacağınızı öğreneceksiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: MayMSFT
ms.author: sihhu
ms.reviewer: trbye
ms.date: 07/16/2019
ms.custom: seodec18
ms.openlocfilehash: 241c84212132ee90e71291758e094cb4a115f2e2
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69018090"
---
# <a name="tutorial-prepare-data-for-regression-modeling"></a>Öğretici: Gerileme modelleme için verileri hazırlama

Bu öğreticide, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)'dan [veri hazırlama paketini](https://aka.ms/data-prep-sdk) kullanarak regresyon modellemesi için verileri nasıl hazırlayacağınızı öğreneceksiniz. İki farklı NYC TAXI veri kümesini filtrelemek ve birleştirmek için çeşitli dönüştürmeler çalıştırırsınız.

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin birinci bölümüdür**. Öğretici serisini tamamladıktan sonra, veri özelliklerinde bir modeli eğitmek için bir TAXI seyahatinin maliyetini tahmin edebilirsiniz. Bu özellikler, çekme günü ve saati, pascuların sayısını ve toplama konumunu içerir.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Python ortamı ayarlama ve paketleri içeri aktarma.
> * Farklı alan adlarına sahip iki veri kümesini yükleyin.
> * Anormallikleri kaldırmak için verileri temizleme.
> * Yeni özellikler oluşturmak için akıllı dönüşümler kullanarak verileri dönüştürün.
> * Veri akışı nesneniz, regresyon modelinde kullanmak üzere kaydedin.

## <a name="prerequisites"></a>Önkoşullar

Not defteri adımlarını okumak için [geliştirme ortamınızı ayarlamayı](#start) atlayın veya Not defterini almak ve Azure Notebooks veya kendi Not defteri sunucunuzda çalıştırmak için aşağıdaki yönergeleri kullanın. Not defterini çalıştırmak için şunlar gerekir:

* Aşağıdaki yüklü bir Python 3,6 Not defteri sunucusu:
    * Azure Machine Learning SDK 'dan paket `azureml-dataprep`
* Öğretici Not defteri

* Çalışma alanınızda bir [bulut Not defteri sunucusu](#azure) kullanın 
* [Kendi Not defteri sunucunuzu](#server) kullanın

### <a name="azure"></a>Çalışma alanınızda bir bulut Not defteri sunucusu kullanın

Kendi bulut tabanlı Not defteri sunucunuza kolayca başlamak kolaydır. Python için Azure Machine Learning SDK zaten yüklenmiş ve bu bulut kaynağını oluşturduktan sonra sizin için yapılandırılmış.

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* Not defteri Web sayfasını başlattıktan sonra **öğreticiler/Regression-part1-Data-Prep. ipynb** Not defterini çalıştırın.

### <a name="server"></a>Kendi Jupyter Not defteri sunucunuzu kullanın

Bilgisayarınızda yerel bir Jupyter not defteri sunucusu oluşturmak için aşağıdaki adımları kullanın.  Adımları tamamladıktan sonra **öğreticiler/Regression-part1-Data-Prep. ipynb** Not defterini çalıştırın.

1. [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)'daki yükleme adımlarını doldurun.
1. SDK 'yı yüklediğinizde paket otomatik olarak yüklenir. `azureml-dataprep`
1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```

## <a name="start"></a>Geliştirme ortamınızı ayarlama

Geliştirme çalışmanızdaki tüm kurulum bir Python not defterinde gerçekleştirilebilir. Kurulum aşağıdaki eylemleri içerir:

* SDK yükle
* Python paketlerini içeri aktarma

### <a name="install-and-import-packages"></a>Paketleri yükleyip içeri aktarma

Gerekli paketleri henüz yoksa yüklemek için aşağıdakileri kullanın.

```shell
pip install "azureml-dataprep[pandas]>=1.1.0,<1.2.0"
```

Paketi içeri aktarın.

```python
import azureml.dataprep as dprep
```

> [!IMPORTANT]
> En son azureml. dataprep paketi sürümünü yüklediğinizden emin olun. Bu öğretici, 1.1.0 'den düşük sürüm numarasıyla çalışmaz

## <a name="load-data"></a>Veri yükleme

İki farklı NYC TAXI veri kümesini veri akışı nesnelerine indirin. Veri kümelerinin biraz farklı alanları vardır. Yöntemi `auto_read_file()` , giriş dosyası türünü otomatik olarak tanır.

```python
from IPython.display import display

green_path = "https://dprepdata.blob.core.windows.net/demo/green-small/*"
yellow_path = "https://dprepdata.blob.core.windows.net/demo/yellow-small/*"

# (optional) Download and view a subset of the data: https://dprepdata.blob.core.windows.net/demo/green-small/green_tripdata_2013-08.csv

green_df_raw = dprep.read_csv(
    path=green_path, header=dprep.PromoteHeadersMode.GROUPED)
# auto_read_file automatically identifies and parses the file type, which is useful when you don't know the file type.
yellow_df_raw = dprep.auto_read_file(path=yellow_path)

display(green_df_raw.head(5))
display(yellow_df_raw.head(5))
```

Bir `Dataflow` nesne bir veri çerçevesine benzerdir ve veriler üzerinde geç tarafından değerlendirilen, sabit bir işlem dizisi temsil eder. İşlemler, farklı dönüştürme ve filtreleme yöntemleri çağrılarak eklenebilir. Bir `Dataflow` işlemi öğesine eklemenin sonucu her zaman yeni `Dataflow` bir nesnedir.

## <a name="cleanse-data"></a>Verilerini temizlemek

Artık tüm veri akışlarına uygulamak için bazı değişkenleri kısayol dönüşümlerinde doldurursunuz. Değişkeni `drop_if_all_null` , tüm alanların null olduğu kayıtları silmek için kullanılır. Değişkeni `useful_columns` , her veri akışında tutulan bir sütun açıklamaları dizisi içerir.

```python
all_columns = dprep.ColumnSelector(term=".*", use_regex=True)
drop_if_all_null = [all_columns, dprep.ColumnRelationship(
    dprep.ColumnRelationship.ALL)]
useful_columns = [
    "cost", "distance", "dropoff_datetime", "dropoff_latitude", "dropoff_longitude",
    "passengers", "pickup_datetime", "pickup_latitude", "pickup_longitude", "store_forward", "vendor"
]
```

İlk olarak yeşil TAXI verileriyle birlikte çalışarak sarı TAXI verileriyle birleştirilebilecek geçerli bir şekle ulaşabilirsiniz. Oluşturduğunuz kısayol dönüştürme `drop_nulls()`değişkenlerini kullanarak `keep_columns()`,, ve işlevlerini çağırın. `replace_na()` Ayrıca, veri çerçevesindeki tüm sütunları `useful_columns` değişkendeki adlarla eşleşecek şekilde yeniden adlandırın.


```python
green_df = (green_df_raw
            .replace_na(columns=all_columns)
            .drop_nulls(*drop_if_all_null)
            .rename_columns(column_pairs={
                "VendorID": "vendor",
                "lpep_pickup_datetime": "pickup_datetime",
                "Lpep_dropoff_datetime": "dropoff_datetime",
                "lpep_dropoff_datetime": "dropoff_datetime",
                "Store_and_fwd_flag": "store_forward",
                "store_and_fwd_flag": "store_forward",
                "Pickup_longitude": "pickup_longitude",
                "Pickup_latitude": "pickup_latitude",
                "Dropoff_longitude": "dropoff_longitude",
                "Dropoff_latitude": "dropoff_latitude",
                "Passenger_count": "passengers",
                "Fare_amount": "cost",
                "Trip_distance": "distance"
            })
            .keep_columns(columns=useful_columns))
display(green_df.head(5))
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
      <th>Satıcı</th>
      <th>pickup_datetime</th>
      <th>dropoff_datetime</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>Yolcuların</th>
      <th>uzaklık</th>
      <th>maliyet</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 08:14:37</td>
      <td>2013-08-01 09:09:06</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>21,25</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 09:13:00</td>
      <td>2013-08-01 11:38:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>.00</td>
      <td>74,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-01 09:48:00</td>
      <td>2013-08-01 09:49:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-01 10:38:35</td>
      <td>2013-08-01 10:38:51</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-01 11:51:45</td>
      <td>2013-08-01 12:03:52</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>8.5</td>
    </tr>
  </tbody>
</table>
</div>

Sarı TAXI verilerinde aynı dönüştürme adımlarını çalıştırın. Bu işlevler, makine öğrenimi modelinin doğruluğunu artırmaya yardımcı olacak veri kümesinden null verilerinin kaldırılmasını sağlar.

```python
yellow_df = (yellow_df_raw
             .replace_na(columns=all_columns)
             .drop_nulls(*drop_if_all_null)
             .rename_columns(column_pairs={
                 "vendor_name": "vendor",
                 "VendorID": "vendor",
                 "vendor_id": "vendor",
                 "Trip_Pickup_DateTime": "pickup_datetime",
                 "tpep_pickup_datetime": "pickup_datetime",
                 "Trip_Dropoff_DateTime": "dropoff_datetime",
                 "tpep_dropoff_datetime": "dropoff_datetime",
                 "store_and_forward": "store_forward",
                 "store_and_fwd_flag": "store_forward",
                 "Start_Lon": "pickup_longitude",
                 "Start_Lat": "pickup_latitude",
                 "End_Lon": "dropoff_longitude",
                 "End_Lat": "dropoff_latitude",
                 "Passenger_Count": "passengers",
                 "passenger_count": "passengers",
                 "Fare_Amt": "cost",
                 "fare_amount": "cost",
                 "Trip_Distance": "distance",
                 "trip_distance": "distance"
             })
             .keep_columns(columns=useful_columns))
display(yellow_df.head(5))
```

Sarı TAXI verilerini eklemek için yeşil TAXI verilerinde işleviçağırın.`append_rows()` Yeni bir Birleşik veri çerçevesi oluşturulur.

```python
combined_df = green_df.append_rows([yellow_df])
```

### <a name="convert-types-and-filter"></a>Türleri ve filtre Dönüştür

Verilerin nasıl dağıtılması görmek için alma ve teslim koordinatları Özet istatistikleri inceleyin. İlk olarak, enlem `TypeConverter` ve Boylam alanlarını ondalık türe değiştirecek bir nesne tanımlayın. Sonra, çıktıyı yalnızca `keep_columns()` Enlem ve Boylam alanlarıyla sınırlamak için işlevini çağırın ve sonra `get_profile()` işlevi çağırın. Bu işlev çağrıları, eksik veya kapsam dışı koordinatları değerlendirmek daha kolay hale getiren yalnızca Enlem/Long alanlarını göstermek için veri akışının sıkıştırılmış bir görünümünü oluşturur.


```python
decimal_type = dprep.TypeConverter(data_type=dprep.FieldType.DECIMAL)
combined_df = combined_df.set_column_types(type_conversions={
    "pickup_longitude": decimal_type,
    "pickup_latitude": decimal_type,
    "dropoff_longitude": decimal_type,
    "dropoff_latitude": decimal_type
})
combined_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude",
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Sayı</th>
      <th>Eksik sayısı</th>
      <th>Sayısı eksik</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>% 0,1 Quantile</th>
      <th>%1 Quantile</th>
      <th>%5 Quantile</th>
      <th>% 25 Quantile</th>
      <th>% 50 Quantile</th>
      <th>% 75 Quantile</th>
      <th>% 95 Quantile</th>
      <th>% 99 Quantile</th>
      <th>% 99,9 Quantile</th>
      <th>Standart Sapma</th>
      <th>Ortalama</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115.179337</td>
      <td>0.000000</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-88.114046</td>
      <td>-73.961840</td>
      <td>-73.961964</td>
      <td>-73.947693</td>
      <td>-73.922097</td>
      <td>-73.846670</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>18.792672</td>
      <td>-68.833579</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0.000000</td>
      <td>40.919121</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>40.682889</td>
      <td>40.675541</td>
      <td>40.721075</td>
      <td>40.756159</td>
      <td>40.803909</td>
      <td>40.849406</td>
      <td>40.870681</td>
      <td>40.891244</td>
      <td>10.345967</td>
      <td>37.936742</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115.179337</td>
      <td>0.000000</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-87.699611</td>
      <td>-73.984734</td>
      <td>-73.985777</td>
      <td>-73.956250</td>
      <td>-73.928948</td>
      <td>-73.866208</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>18.696526</td>
      <td>-68.896978</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0.000000</td>
      <td>41.008934</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>40.662763</td>
      <td>40.654851</td>
      <td>40.717821</td>
      <td>40.756534</td>
      <td>40.784688</td>
      <td>40.852437</td>
      <td>40.879289</td>
      <td>40.937291</td>
      <td>10.290780</td>
      <td>37.963774</td>
    </tr>
  </tbody>
</table>



Özet istatistik çıktısından, New York şehrinde olmayan koordinatları ve koordinatları olduğunu görürsünüz (Bu, öznel Analize göre belirlenir). Şehir kenarlığının dışında kalan konumlar için koordinatları filtreleyin. `filter()` İşlev içindeki sütun filtresi komutlarını zincirleyip her bir alan için en düşük ve en büyük sınırları tanımlayın. Sonra dönüştürmeyi doğrulamak `get_profile()` için işlevi yeniden çağırın.


```python
latlong_filtered_df = (combined_df
                       .drop_nulls(
                           columns=["pickup_longitude", "pickup_latitude",
                                    "dropoff_longitude", "dropoff_latitude"],
                           column_relationship=dprep.ColumnRelationship(
                               dprep.ColumnRelationship.ANY)
                       )
                       .filter(dprep.f_and(
                           dprep.col("pickup_longitude") <= -73.72,
                           dprep.col("pickup_longitude") >= -74.09,
                           dprep.col("pickup_latitude") <= 40.88,
                           dprep.col("pickup_latitude") >= 40.53,
                           dprep.col("dropoff_longitude") <= -73.72,
                           dprep.col("dropoff_longitude") >= -74.09,
                           dprep.col("dropoff_latitude") <= 40.88,
                           dprep.col("dropoff_latitude") >= 40.53
                       )))
latlong_filtered_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude",
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Sayı</th>
      <th>Eksik sayısı</th>
      <th>Sayısı eksik</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>% 0,1 Quantile</th>
      <th>%1 Quantile</th>
      <th>%5 Quantile</th>
      <th>% 25 Quantile</th>
      <th>% 50 Quantile</th>
      <th>% 75 Quantile</th>
      <th>% 95 Quantile</th>
      <th>% 99 Quantile</th>
      <th>% 99,9 Quantile</th>
      <th>Standart Sapma</th>
      <th>Ortalama</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.078156</td>
      <td>-73.736481</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.076314</td>
      <td>-73.962542</td>
      <td>-73.962893</td>
      <td>-73.948975</td>
      <td>-73.927856</td>
      <td>-73.866662</td>
      <td>-73.830438</td>
      <td>-73.823160</td>
      <td>-73.769750</td>
      <td>0.048711</td>
      <td>-73.913865</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.575485</td>
      <td>40.879852</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.632884</td>
      <td>40.713105</td>
      <td>40.711600</td>
      <td>40.721403</td>
      <td>40.758142</td>
      <td>40.805145</td>
      <td>40.848855</td>
      <td>40.867567</td>
      <td>40.877690</td>
      <td>0.048348</td>
      <td>40.765226</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.085747</td>
      <td>-73.720871</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.078828</td>
      <td>-73.985650</td>
      <td>-73.985813</td>
      <td>-73.959041</td>
      <td>-73.936681</td>
      <td>-73.884846</td>
      <td>-73.815507</td>
      <td>-73.776697</td>
      <td>-73.733471</td>
      <td>0.055961</td>
      <td>-73.920718</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.583530</td>
      <td>40.879734</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.597741</td>
      <td>40.695376</td>
      <td>40.695115</td>
      <td>40.727549</td>
      <td>40.758160</td>
      <td>40.788378</td>
      <td>40.850372</td>
      <td>40.867968</td>
      <td>40.878586</td>
      <td>0.050462</td>
      <td>40.759487</td>
    </tr>
  </tbody>
</table>

### <a name="split-and-rename-columns"></a>Bölme ve sütunları yeniden adlandırma

Veri profili bakın `store_forward` sütun. Bu alan, bir ön izleme sonrasında sunucuya `Y` bir bağlantı olmadığında ve bu nedenle seyahat verilerinin bellekte depolanması ve daha sonra bağlıyken sunucuya iletmesi durumunda olan bir Boolean bayrağıdır.


```python
latlong_filtered_df.keep_columns(columns='store_forward').get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Sayı</th>
      <th>Eksik sayısı</th>
      <th>Sayısı eksik</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>% 0,1 Quantile</th>
      <th>%1 Quantile</th>
      <th>%5 Quantile</th>
      <th>% 25 Quantile</th>
      <th>% 50 Quantile</th>
      <th>% 75 Quantile</th>
      <th>% 95 Quantile</th>
      <th>% 99 Quantile</th>
      <th>% 99,9 Quantile</th>
      <th>Standart Sapma</th>
      <th>Ortalama</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>E</td>
      <td>7059.0</td>
      <td>99.0</td>
      <td>6960.0</td>
      <td>0.014025</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>



`store_forward` Sütundaki veri profili çıkışının verilerin tutarsız olduğunu ve eksik veya null değerler olduğunu gösterir. Bu değerleri " `fill_nulls()` N" dizesiyle değiştirmek için veişlevlerinikullanın:`replace()`


```python
replaced_stfor_vals_df = latlong_filtered_df.replace(
    columns="store_forward", find="0", replace_with="N").fill_nulls("store_forward", "N")
```

`distance` Alanında `replace` işlevi yürütün. İşlev, yanlış şekilde `.00`etiketlendirilmiş uzaklık değerlerini yeniden biçimlendirir ve tüm null değerleri sıfırlarla doldurur. Dönüştürme `distance` sayısal biçimi alanı. Bu yanlış veri noktaları, TAXI Cabs üzerindeki veri toplama sisteminde büyük olasılıkla bozukluklar olabilir.


```python
replaced_distance_vals_df = replaced_stfor_vals_df.replace(
    columns="distance", find=".00", replace_with=0).fill_nulls("distance", 0)
replaced_distance_vals_df = replaced_distance_vals_df.to_number(["distance"])
```

Pickup ve Drop DateTime değerlerini ilgili tarih ve Saat sütunlarına ayırın. Bölmeyi yapmak için işlevini kullanın. `split_column_by_example()` Bu durumda, `split_column_by_example()` işlevinin isteğe bağlı `example` parametresi atlanır. Bu nedenle, işlevi otomatik olarak verilere göre bölüneceği belirler.


```python
time_split_df = (replaced_distance_vals_df
                 .split_column_by_example(source_column="pickup_datetime")
                 .split_column_by_example(source_column="dropoff_datetime"))
time_split_df.head(5)
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
      <th>Satıcı</th>
      <th>pickup_datetime</th>
      <th>pickup_datetime_1</th>
      <th>pickup_datetime_2</th>
      <th>dropoff_datetime</th>
      <th>dropoff_datetime_1</th>
      <th>dropoff_datetime_2</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>Yolcuların</th>
      <th>uzaklık</th>
      <th>maliyet</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>N</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>2013-08-01</td>
      <td>17:24:00</td>
      <td>2013-08-01 17:25:00</td>
      <td>2013-08-01</td>
      <td>17:25:00</td>
      <td>N</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>2013-08-06</td>
      <td>06:51:19</td>
      <td>2013-08-06 06:51:36</td>
      <td>2013-08-06</td>
      <td>06:51:36</td>
      <td>N</td>
      <td>-73.937721</td>
      <td>40.758404</td>
      <td>-73.937721</td>
      <td>40.758369</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>2013-08-06</td>
      <td>13:26:34</td>
      <td>2013-08-06 13:26:57</td>
      <td>2013-08-06</td>
      <td>13:26:57</td>
      <td>N</td>
      <td>-73.937691</td>
      <td>40.758419</td>
      <td>-73.937790</td>
      <td>40.758358</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>2013-08-06</td>
      <td>13:27:53</td>
      <td>2013-08-06 13:28:08</td>
      <td>2013-08-06</td>
      <td>13:28:08</td>
      <td>N</td>
      <td>-73.937805</td>
      <td>40.758396</td>
      <td>-73.937775</td>
      <td>40.758450</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
  </tbody>
</table>
</div>

`split_column_by_example()` İşlev tarafından üretilen sütunları anlamlı adlar kullanacak şekilde yeniden adlandırın.

```python
renamed_col_df = (time_split_df
                  .rename_columns(column_pairs={
                      "pickup_datetime_1": "pickup_date",
                      "pickup_datetime_2": "pickup_time",
                      "dropoff_datetime_1": "dropoff_date",
                      "dropoff_datetime_2": "dropoff_time"
                  }))
renamed_col_df.head(5)
```

Tüm Temizleme adımından sonra tam Özet istatistiklerini görmek için işleviçağırın.`get_profile()`

```python
renamed_col_df.get_profile()
```

## <a name="transform-data"></a>Verileri dönüştürme

Toplama ve bırakma tarihini haftanın gününe, ayın gününe ve ay değerlerine göre ayırın. Hafta değerinin gününü almak için `derive_column_by_example()` işlevini kullanın. İşlevi, giriş verilerini tanımlayan örnek nesnelerin bir dizi parametresini ve tercih edilen çıktıyı alır. İşlevi tercih ettiğiniz dönüştürmeyi otomatik olarak belirler. Çekme ve bırakma zaman sütunları için, `split_column_by_example()` işlevi örnek parametre olmadan kullanarak saati saat, dakika ve saniye olarak ayırın.

Yeni özellikleri oluşturduktan sonra, yeni oluşturulan özellikler tercih `drop_columns()` edildiği için özgün alanları silmek üzere işlevini kullanın. Anlamlı açıklamaları kullanmak için alanların geri kalanını yeniden adlandırın.

Verileri bu şekilde dönüştürmek, zaman tabanlı yeni özellikler oluşturmak için makine öğrenimi modelinin doğruluğunu geliştirir. Örneğin, hafta içi yeni bir özellik oluşturmak, haftanın günü ve TAXI tarifeli havayolu fiyatı arasında bir ilişki kurmaya yardımcı olur. Bu, genellikle yüksek talebe bağlı olarak haftanın belirli günlerinde daha pahalıdır.


```python
transformed_features_df = (renamed_col_df
                           .derive_column_by_example(
                               source_columns="pickup_date",
                               new_column_name="pickup_weekday",
                               example_data=[
                                   ("2009-01-04", "Sunday"), ("2013-08-22", "Thursday")]
                           )
                           .derive_column_by_example(
                               source_columns="dropoff_date",
                               new_column_name="dropoff_weekday",
                               example_data=[
                                   ("2013-08-22", "Thursday"), ("2013-11-03", "Sunday")]
                           )

                           .split_column_by_example(source_column="pickup_time")
                           .split_column_by_example(source_column="dropoff_time")
                           # The following two calls to split_column_by_example reference the column names generated from the previous two calls.
                           .split_column_by_example(source_column="pickup_time_1")
                           .split_column_by_example(source_column="dropoff_time_1")
                           .drop_columns(columns=[
                               "pickup_date", "pickup_time", "dropoff_date", "dropoff_time",
                               "pickup_date_1", "dropoff_date_1", "pickup_time_1", "dropoff_time_1"
                           ])

                           .rename_columns(column_pairs={
                               "pickup_date_2": "pickup_month",
                               "pickup_date_3": "pickup_monthday",
                               "pickup_time_1_1": "pickup_hour",
                               "pickup_time_1_2": "pickup_minute",
                               "pickup_time_2": "pickup_second",
                               "dropoff_date_2": "dropoff_month",
                               "dropoff_date_3": "dropoff_monthday",
                               "dropoff_time_1_1": "dropoff_hour",
                               "dropoff_time_1_2": "dropoff_minute",
                               "dropoff_time_2": "dropoff_second"
                           }))

transformed_features_df.head(5)
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
      <th>Satıcı</th>
      <th>pickup_datetime</th>
      <th>pickup_weekday</th>
      <th>pickup_hour</th>
      <th>pickup_minute</th>
      <th>pickup_second</th>
      <th>dropoff_datetime</th>
      <th>dropoff_weekday</th>
      <th>dropoff_hour</th>
      <th>dropoff_minute</th>
      <th>dropoff_second</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>Yolcuların</th>
      <th>uzaklık</th>
      <th>maliyet</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>22</td>
      <td>0</td>
      <td>2013-08-01 17:22:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>22</td>
      <td>0</td>
      <td>N</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>24</td>
      <td>0</td>
      <td>2013-08-01 17:25:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>25</td>
      <td>0</td>
      <td>N</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>Salı</td>
      <td>06</td>
      <td>51</td>
      <td>19</td>
      <td>2013-08-06 06:51:36</td>
      <td>Salı</td>
      <td>06</td>
      <td>51</td>
      <td>36</td>
      <td>N</td>
      <td>-73.937721</td>
      <td>40.758404</td>
      <td>-73.937721</td>
      <td>40.758369</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>Salı</td>
      <td>13</td>
      <td>26</td>
      <td>34</td>
      <td>2013-08-06 13:26:57</td>
      <td>Salı</td>
      <td>13</td>
      <td>26</td>
      <td>57</td>
      <td>N</td>
      <td>-73.937691</td>
      <td>40.758419</td>
      <td>-73.937790</td>
      <td>40.758358</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>Salı</td>
      <td>13</td>
      <td>27</td>
      <td>53</td>
      <td>2013-08-06 13:28:08</td>
      <td>Salı</td>
      <td>13</td>
      <td>28</td>
      <td>08</td>
      <td>N</td>
      <td>-73.937805</td>
      <td>40.758396</td>
      <td>-73.937775</td>
      <td>40.758450</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
  </tbody>
</table>
</div>

Verilerin, türetilmiş dönüşümlerden üretilen toplama ve bırakma tarihi ve saat bileşenlerinin doğru olduğunu gösterir. `pickup_datetime` Ve`dropoff_datetime` sütunlarını, artık gerekli olmadığı için bırakın (saat, dakika ve saniye gibi ayrıntılı zaman özellikleri model eğitimi için daha yararlıdır).


```python
processed_df = transformed_features_df.drop_columns(
    columns=["pickup_datetime", "dropoff_datetime"])
```

Her bir alanın veri türü otomatik olarak denetlemek için tür çıkarımı işlevselliği kullanmak ve çıkarım sonuçları görüntüleyebilirsiniz.


```python
type_infer = processed_df.builders.set_column_types()
type_infer.learn()
type_infer
```

Sonuç çıktısı `type_infer` aşağıdaki gibidir.

    Column types conversion candidates:
    'pickup_weekday': [FieldType.STRING],
    'pickup_hour': [FieldType.DECIMAL],
    'pickup_minute': [FieldType.DECIMAL],
    'pickup_second': [FieldType.DECIMAL],
    'dropoff_hour': [FieldType.DECIMAL],
    'dropoff_minute': [FieldType.DECIMAL],
    'dropoff_second': [FieldType.DECIMAL],
    'store_forward': [FieldType.STRING],
    'pickup_longitude': [FieldType.DECIMAL],
    'dropoff_longitude': [FieldType.DECIMAL],
    'passengers': [FieldType.DECIMAL],
    'distance': [FieldType.DECIMAL],
    'vendor': [FieldType.STRING],
    'dropoff_weekday': [FieldType.STRING],
    'pickup_latitude': [FieldType.DECIMAL],
    'dropoff_latitude': [FieldType.DECIMAL],
    'cost': [FieldType.DECIMAL]

Çıkarım sonuçları verilere göre doğru şekilde görünür. Şimdi veri akışına tür dönüştürmeleri uygulayın.


```python
type_converted_df = type_infer.to_dataflow()
type_converted_df.get_profile()
```

Veri akışını paketleyerek, veri kümesinde iki son filtre çalıştırın. Hatalı yakalanmış veri noktalarını ortadan kaldırmak için, hem hem de `cost` `distance` değişken değerlerinin sıfırdan büyük olduğu kayıtlardaki veri akışını filtreleyin. Bu adım, düşük maliyetli veya mesafedeki veri noktaları tahmin doğruluğunu gerçekleştiren büyük aykırı değerleri temsil ettiğinden, makine öğrenimi modelinin doğruluğunu önemli ölçüde artırır.

```python
final_df = type_converted_df.filter(dprep.col("distance") > 0)
final_df = final_df.filter(dprep.col("cost") > 0)
```

Artık bir makine öğrenimi modelinde kullanmak üzere tamamen dönüştürülmüş ve hazırlanan bir veri akışı nesneniz vardır. SDK, aşağıdaki kodda gösterildiği gibi kullanılan nesne serileştirme işlevlerini içerir.

```python
import os

file_path = os.path.join(os.getcwd(), "dflows.dprep")
final_df.save(file_path)
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticinin ikinci bölümüne devam etmek için geçerli dizindeki **dakışlar. dprep** dosyasına ihtiyacınız vardır.

İkinci bölüm olmaya devam etmeyi planlamıyorsanız geçerli dizininizde **dakışlar. dprep** dosyasını silin. Yürütmeyi yerel olarak veya [Azure Notebooks](https://notebooks.azure.com/)' de çalıştırıp çalıştırsanız bu dosyayı silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticinin bir parçası olarak:

> [!div class="checklist"]
> * Geliştirme ortamınızı ayarlayın.
> * Yüklenen ve temizlenen veri kümeleri.
> * Bir örneğe göre mantığınızı tahmin etmek için akıllı dönüşümler kullanılır.
> * Machine Learning eğitimi için birleştirilmiş ve paketlenmiş veri kümeleri.

Öğreticinin ikinci bölümünde eğitim verilerini kullanmaya hazırsınız:

> [!div class="nextstepaction"]
> [Öğretici (ikinci bölüm): Regresyon modelini eğitme](tutorial-auto-train-models.md)
