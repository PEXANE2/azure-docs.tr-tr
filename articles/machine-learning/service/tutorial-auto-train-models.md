---
title: 'Regresyon modeli öğreticisi: Otomatikleştirilmiş ML'
titleSuffix: Azure Machine Learning service
description: Otomatik makine öğrenimi kullanarak makine öğrenimi modeli oluşturmayı öğrenin. Azure Machine Learning, veri ön işleme, algoritma seçimi ve hiper parametre seçimini sizin için otomatik bir şekilde gerçekleştirebilir. Ardından, son model Azure Machine Learning hizmeti ile dağıtılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: nacharya1
ms.author: nilesha
ms.reviewer: trbye
ms.date: 04/11/2019
ms.custom: seodec18
ms.openlocfilehash: 70a95cdba2a8b41c7b2fc3ee4b2664f049a84e95
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68846024"
---
# <a name="tutorial-use-automated-machine-learning-to-build-your-regression-model"></a>Öğretici: Gerileme modelinizi derlemek için otomatik makine öğrenimi kullanma

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. Önceki öğreticide, [NYC taksi verileri regresyon modelleme için hazırlanmış](tutorial-data-prep.md).

Artık Azure Machine Learning hizmeti ile modelinizi oluşturmaya başlamaya hazırsınız. Öğreticinin bu bölümünde, hazırlanan verileri kullanır ve TAXI tarifeli havayolu fiyatlarını tahmin etmek için otomatik olarak bir gerileme modeli oluşturursunuz. Hizmetin otomatik makine öğrenimi özelliklerini kullanarak Machine Learning hedeflerinizi ve kısıtlamalarını tanımlarsınız. Otomatik makine öğrenimi sürecini başlatın. Ardından, algoritma seçimine ve hiper parametre ayarlamaya sizin için izin verin. Otomatik makine öğrenimi tekniği, kriterinize göre en iyi modeli bulana kadar birçok algoritma ve hiper parametre kombinasyonu üzerinde yinelenir.

![Akış diyagramı](./media/tutorial-auto-train-models/flow2.png)

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Python ortamı ayarlama ve SDK paketlerini içeri aktarma.
> * Azure Machine Learning bir hizmet çalışma alanı yapılandırın.
> * Regresyon modelini oto eğitme.
> * Modeli özel parametrelerle yerel olarak çalıştırın.
> * Sonuçları keşfet.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

>[!NOTE]
> Bu makaledeki kod, Azure Machine Learning SDK sürümü 1.0.39 ile test edilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Not defteri adımlarını okumak için [geliştirme ortamınızı ayarlamayı](#start) atlayın veya Not defterini almak ve Azure Notebooks veya kendi Not defteri sunucunuzda çalıştırmak için aşağıdaki yönergeleri kullanın. Not defterini çalıştırmak için şunlar gerekir:

* [Veri hazırlama öğreticisini çalıştırma](tutorial-data-prep.md).
* Aşağıdaki yüklü bir Python 3,6 Not defteri sunucusu:
    * `automl` Ve`notebooks` ek özellikler ile Python için Azure Machine Learning SDK
    * `matplotlib`
* Öğretici Not defteri
* Machine Learning çalışma alanı
* Çalışma alanının yapılandırma dosyası, not defteriyle aynı dizinde

Bu önkoşulların tümünü aşağıdaki bölümlerden birine alın.

* Çalışma alanınızda bir [bulut Not defteri sunucusu](#azure) kullanın
* [Kendi Not defteri sunucunuzu](#server) kullanın

### <a name="azure"></a>Çalışma alanınızda bir bulut Not defteri sunucusu kullanın

Kendi bulut tabanlı Not defteri sunucunuza kolayca başlamak kolaydır. [Python için Azure MACHINE LEARNING SDK](https://aka.ms/aml-sdk) zaten yüklenmiş ve bu bulut kaynağını oluşturduktan sonra sizin için yapılandırılmış.

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* Not defteri Web sayfasını başlattıktan sonra **öğreticiler/Regression-part2-Automated-ml. ipynb** Not defterini çalıştırın.

### <a name="server"></a>Kendi Jupyter Not defteri sunucunuzu kullanın

Bilgisayarınızda yerel bir Jupyter not defteri sunucusu oluşturmak için aşağıdaki adımları kullanın.  Ortamınızdaki ve diğer `matplotlib` `automl` eközellikleriortamınızayüklediğinizdeneminolun`notebooks` .

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Adımları tamamladıktan sonra **öğreticiler/Regression-part2-Automated-ml. ipynb** Not defterini çalıştırın.

## <a name="start"></a>Geliştirme ortamınızı ayarlama

Geliştirme çalışmanızdaki tüm kurulum bir Python not defterinde gerçekleştirilebilir. Kurulum aşağıdaki eylemleri içerir:

* SDK yükle
* Python paketlerini içeri aktarma
* Çalışma alanınızı yapılandırma

### <a name="install-and-import-packages"></a>Paketleri yükleyip içeri aktarma

Kendi Python ortamınızdaki öğreticiyi takip ediyorsanız, gerekli paketleri yüklemek için aşağıdakileri kullanın.

```shell
pip install azureml-sdk[automl,notebooks] matplotlib
```

Bu öğreticide ihtiyacınız olan Python paketlerini içeri aktarın:

```python
import azureml.core
import pandas as pd
from azureml.core.workspace import Workspace
import logging
import os
```

### <a name="configure-workspace"></a>Çalışma alanını yapılandırma

Mevcut çalışma alanından bir çalışma alanı nesnesi oluşturun. [Çalışma alanı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) , Azure aboneliğinizi ve kaynak bilgilerinizi kabul eden bir sınıftır. Ayrıca, modelinizi izlemek ve izlemek için bir bulut kaynağı oluşturur.

`Workspace.from_config()`, **config.json** dosyasını okur ve ayrıntıları `ws` adlı nesneye yükler.  Bu öğreticideki kodun kalanında `ws` kullanılır.

Çalışma alanı nesneniz olduktan sonra, deneme için bir ad belirtin. Çalışma alanıyla yerel bir dizin oluşturun ve kaydedin. Tüm çalıştırmaların geçmişi belirtilen deneyime ve [Azure Portal](https://portal.azure.com)göre kaydedilir.


```python
ws = Workspace.from_config()
# choose a name for the run history container in the workspace
experiment_name = 'automated-ml-regression'
# project folder
project_folder = './automated-ml-regression'

output = {}
output['SDK version'] = azureml.core.VERSION
output['Subscription ID'] = ws.subscription_id
output['Workspace'] = ws.name
output['Resource Group'] = ws.resource_group
output['Location'] = ws.location
output['Project Directory'] = project_folder
pd.set_option('display.max_colwidth', -1)
pd.DataFrame(data=output, index=['']).T
```

## <a name="explore-data"></a>Verileri inceleme

Önceki öğreticide oluşturulan veri akışı nesnesini kullanın. Özetlemek gerekirse, Bu öğreticinin 1. bölümünde, bir makine öğrenimi modelinde kullanılabilmesi için NYC TAXI verilerini temizledi. Şimdi, veri kümesindeki çeşitli özellikleri kullanıyorsunuz ve bir otomatik modelin, bir vergilenm yolculuğuna ilişkin özellikler ve fiyat arasında ilişkiler oluşturmasına izin veriyor. Veri akışını açıp çalıştırın ve sonuçları gözden geçirin:


```python
import azureml.dataprep as dprep

file_path = os.path.join(os.getcwd(), "dflows.dprep")

dflow_prepared = dprep.Dataflow.open(file_path)
dflow_prepared.get_profile()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Count</th>
      <th>Eksik sayım</th>
      <th>Eksik sayı yok</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>% 0,1 quantile</th>
      <th>% 1 quantile</th>
      <th>% 5 quantile</th>
      <th>% 25 quantile</th>
      <th>% 50 quantile</th>
      <th>% 75 quantile</th>
      <th>% 95 quantile</th>
      <th>% 99 quantile</th>
      <th>% 99,9 quantile</th>
      <th>Ortalama</th>
      <th>Standart sapma</th>
      <th>Varyans</th>
      <th>Komutunu</th>
      <th>Basıklık</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Satıcı</th>
      <td>FieldType.STRING</td>
      <td>1</td>
      <td>VTS</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
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
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_weekday</th>
      <td>FieldType.STRING</td>
      <td>Cuma</td>
      <td>Çarşamba</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
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
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>2.90047</td>
      <td>2.69355</td>
      <td>9.72889</td>
      <td>16</td>
      <td>19.3713</td>
      <td>22.6974</td>
      <td>23</td>
      <td>23</td>
      <td>14.2731</td>
      <td>6.59242</td>
      <td>43.46</td>
      <td>-0.693723</td>
      <td>-0.570403</td>
    </tr>
    <tr>
      <th>pickup_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>4.99701</td>
      <td>4.95833</td>
      <td>14.1528</td>
      <td>29.3832</td>
      <td>44.6825</td>
      <td>56.4444</td>
      <td>58.9909</td>
      <td>59</td>
      <td>29.427</td>
      <td>17.4333</td>
      <td>303.921</td>
      <td>0.0120999</td>
      <td>-1.20981</td>
    </tr>
    <tr>
      <th>pickup_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.28131</td>
      <td>5</td>
      <td>14.7832</td>
      <td>29.9293</td>
      <td>44.725</td>
      <td>56.7573</td>
      <td>59</td>
      <td>59</td>
      <td>29.7443</td>
      <td>17.3595</td>
      <td>301.351</td>
      <td>-0.0252399</td>
      <td>-1.19616</td>
    </tr>
    <tr>
      <th>dropoff_weekday</th>
      <td>FieldType.STRING</td>
      <td>Cuma</td>
      <td>Çarşamba</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
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
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>dropoff_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>2.57153</td>
      <td>2</td>
      <td>9.58795</td>
      <td>15.9994</td>
      <td>19.6184</td>
      <td>22.8317</td>
      <td>23</td>
      <td>23</td>
      <td>14.2105</td>
      <td>6.71093</td>
      <td>45.0365</td>
      <td>-0.687292</td>
      <td>-0.61951</td>
    </tr>
    <tr>
      <th>dropoff_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.44383</td>
      <td>4.84694</td>
      <td>14.1036</td>
      <td>28.8365</td>
      <td>44.3102</td>
      <td>56.6892</td>
      <td>59</td>
      <td>59</td>
      <td>29.2907</td>
      <td>17.4108</td>
      <td>303.136</td>
      <td>0.0222514</td>
      <td>-1.2181</td>
    </tr>
    <tr>
      <th>dropoff_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.07801</td>
      <td>5</td>
      <td>14.5751</td>
      <td>29.5972</td>
      <td>45.4649</td>
      <td>56.2729</td>
      <td>59</td>
      <td>59</td>
      <td>29.772</td>
      <td>17.5337</td>
      <td>307.429</td>
      <td>-0.0212575</td>
      <td>-1.226</td>
    </tr>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>E</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
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
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.0781</td>
      <td>-73.7459</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.0578</td>
      <td>-73.9639</td>
      <td>-73.9656</td>
      <td>-73.9508</td>
      <td>-73.9255</td>
      <td>-73.8529</td>
      <td>-73.8302</td>
      <td>-73.8238</td>
      <td>-73.7697</td>
      <td>-73.9123</td>
      <td>0.0503757</td>
      <td>0.00253771</td>
      <td>0.352172</td>
      <td>-0.923743</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5755</td>
      <td>40.8799</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.632</td>
      <td>40.7117</td>
      <td>40.7115</td>
      <td>40.7213</td>
      <td>40.7565</td>
      <td>40.8058</td>
      <td>40.8478</td>
      <td>40.8676</td>
      <td>40.8778</td>
      <td>40.7649</td>
      <td>0.0494674</td>
      <td>0.00244702</td>
      <td>0.205972</td>
      <td>-0.777945</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.0857</td>
      <td>-73.7209</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.0775</td>
      <td>-73.9875</td>
      <td>-73.9882</td>
      <td>-73.9638</td>
      <td>-73.935</td>
      <td>-73.8755</td>
      <td>-73.8125</td>
      <td>-73.7759</td>
      <td>-73.7327</td>
      <td>-73.9202</td>
      <td>0.0584627</td>
      <td>0.00341789</td>
      <td>0.623622</td>
      <td>-0.262603</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5835</td>
      <td>40.8797</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.5973</td>
      <td>40.6928</td>
      <td>40.6911</td>
      <td>40.7226</td>
      <td>40.7567</td>
      <td>40.7918</td>
      <td>40.8495</td>
      <td>40.868</td>
      <td>40.8787</td>
      <td>40.7583</td>
      <td>0.0517399</td>
      <td>0.00267701</td>
      <td>0.0390404</td>
      <td>-0.203525</td>
    </tr>
    <tr>
      <th>Yolcuların</th>
      <td>FieldType.DECIMAL</td>
      <td>1</td>
      <td>6</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>1\.</td>
      <td>1\.</td>
      <td>1\.</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>6</td>
      <td>2.39249</td>
      <td>1.83197</td>
      <td>3.3561</td>
      <td>0.763144</td>
      <td>-1.23467</td>
    </tr>
    <tr>
      <th>uzaklık</th>
      <td>FieldType.DECIMAL</td>
      <td>0.01</td>
      <td>32.34</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0108744</td>
      <td>0.743898</td>
      <td>0.738194</td>
      <td>1.243</td>
      <td>2.40168</td>
      <td>4.74478</td>
      <td>10.5136</td>
      <td>14.9011</td>
      <td>21.8035</td>
      <td>3.5447</td>
      <td>3.2943</td>
      <td>10.8524</td>
      <td>1.91556</td>
      <td>4.99898</td>
    </tr>
    <tr>
      <th>maliyet</th>
      <td>FieldType.DECIMAL</td>
      <td>0.1</td>
      <td>88</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.33837</td>
      <td>5.00491</td>
      <td>5</td>
      <td>6.93129</td>
      <td>10.524</td>
      <td>17.4811</td>
      <td>33.2343</td>
      <td>50.0093</td>
      <td>63.1753</td>
      <td>13.6843</td>
      <td>9.66571</td>
      <td>93.426</td>
      <td>1.78518</td>
      <td>4.13972</td>
    </tr>
  </tbody>
</table>

Sütunlar ekleyerek deneme için verileri hazırlama `dflow_x` bizim model oluşturmaya yönelik özellikler için. Tahmin değeri `dflow_y` , maliyetimiz olacak şekilde tanımlayın:

```python
dflow_X = dflow_prepared.keep_columns(
    ['pickup_weekday', 'pickup_hour', 'distance', 'passengers', 'vendor'])
dflow_y = dflow_prepared.keep_columns('cost')
```

### <a name="split-the-data-into-train-and-test-sets"></a>Verileri eğitme ve test kümelerine Böl

Artık, `train_test_split` `sklearn` kitaplıktaki işlevini kullanarak verileri eğitim ve test kümelerine bölecektir. Bu işlev, verileri x, **Özellikler**, model eğitimi için veri kümesine ayırır ve y, **tahmin edilecek değerler**, test için veri kümesi. `test_size` Parametresi test ayrılacak veri yüzdesini belirler. `random_state` Parametresi rastgele oluşturucuya bir çekirdek ayarlar, böylece tren-test bölünmeleri her zaman belirleyici olacak şekilde ayarlanır:

```python
from sklearn.model_selection import train_test_split

x_df = dflow_X.to_pandas_dataframe()
y_df = dflow_y.to_pandas_dataframe()

x_train, x_test, y_train, y_test = train_test_split(
    x_df, y_df, test_size=0.2, random_state=223)
# flatten y_train to 1d array
y_train.values.flatten()
```

Bu adımın amacı, doğru doğruluğu ölçmek için modeli eğmekte kullanılmamış olan tamamlanmış modeli test etmek için veri noktalarına sahip değildir. Diğer bir deyişle, iyi eğitilen bir modelin zaten görmemiş verilerden tahmine dayalı hale getirme yapabilmesi gerekir. Artık gerekli paketleri ve verileri modelinize yeniden eğitmek için hazır hale getirin.

## <a name="automatically-train-a-model"></a>Otomatik olarak bir modeli eğitme

Bir modeli otomatik olarak eğitmek için aşağıdaki adımları uygulayın:
1. Deneme çalıştırmasının ayarlarını tanımlayın. Eğitim verilerinizi yapılandırmaya ekleyin ve eğitim sürecini denetleyen ayarları değiştirin.
1. Model ayarlama için denemeyi gönder. Deneme gönderdikten sonra işlem, farklı makine öğrenimi algoritmalarını ve hiper parametre ayarlarını kullanarak, tanımlı kısıtlamalarınız için uygun şekilde yinelenir. Doğruluk ölçüsünü en iyi duruma getirerek en uygun modeli seçer.

### <a name="define-settings-for-autogeneration-and-tuning"></a>Serilerinin otomatik oluşturulması ve ayarlamak için ayarlarını tanımlayın

Oto oluşturma ve ayarlama için denemeler parametresini ve model ayarlarını tanımlayın. Tam listesini görüntüleyin [ayarları](how-to-configure-auto-train.md). Denemeyi bu varsayılan ayarlarla göndermek yaklaşık 10-15 dakika sürer, ancak daha kısa bir çalışma süresi isterseniz, ya `iterations` `iteration_timeout_minutes`da değerini azaltın.


|Özellik| Bu öğreticideki değer |Açıklama|
|----|----|---|
|**iteration_timeout_minutes**|10|Her yineleme için dakika cinsinden zaman sınırı. Toplam çalışma zamanını azaltmak için bu değeri küçültün.|
|**iterations**|30|Yineleme sayısı. Her yinelemede yeni bir Machine Learning modeli, verilerinize göre eğitilir. Bu, toplam çalışma süresini etkileyen birincil değerdir.|
|**primary_metric**| spearman_correlation | İyileştirmek istediğiniz ölçüm. En uygun model bu ölçüme göre seçilecek.|
|**preprocess**| Doğru | **Doğru**kullanarak, deneme giriş verilerini önceden işleyebilir (eksik verileri işleme, metni sayısal olarak dönüştürme vb.)|
|**Ayrıntı düzeyi**| logging.INFO | Günlüğe kaydetme düzeyini denetler.|
|**n_cross_validations**|5|Doğrulama verileri belirtilmediğinde gerçekleştirilecek çapraz doğrulama bölme sayısı.|



```python
automl_settings = {
    "iteration_timeout_minutes": 10,
    "iterations": 30,
    "primary_metric": 'spearman_correlation',
    "preprocess": True,
    "verbosity": logging.INFO,
    "n_cross_validations": 5
}
```

Tanımlı eğitim ayarlarınızı bir `AutoMLConfig` nesne için parametre olarak kullanın. Ayrıca, bu durumda eğitim verilerinizi ve model `regression` türünü de belirtin.

```python
from azureml.train.automl import AutoMLConfig

# local compute
automated_ml_config = AutoMLConfig(task='regression',
                                   debug_log='automated_ml_errors.log',
                                   path=project_folder,
                                   X=x_train.values,
                                   y=y_train.values.flatten(),
                                   **automl_settings)
```

> [!NOTE]
> Otomatik makine öğrenimi ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı ön işleme adımları, giriş verilerinize otomatik olarak uygulanır.

### <a name="train-the-automatic-regression-model"></a>Otomatik bir regresyon modeli eğitme

Denemeyi yerel olarak çalıştırmak için başlatın. Tanımlanan `automated_ml_config` nesneyi denemenize geçirin. Deneme sırasında ilerlemeyi görüntülemek `True` için çıktıyı ayarlayın:


```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, experiment_name)
local_run = experiment.submit(automated_ml_config, show_output=True)
```

Deneyin, deney olarak canlı güncelleştirmelerin çalıştırıldığı çıktı. Her yineleme için model türünü, çalışma süresini ve eğitim doğruluğunu görürsünüz. Alan `BEST` , ölçüm türünüz temelinde en iyi çalışan eğitim Puanını izler.

    Parent Run ID: AutoML_02778de3-3696-46e9-a71b-521c8fca0651
    *******************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    *******************************************************************************************

     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler ExtremeRandomTrees                0:00:08       0.9447    0.9447
             1   StandardScalerWrapper GradientBoosting         0:00:09       0.9536    0.9536
             2   StandardScalerWrapper ExtremeRandomTrees       0:00:09       0.8580    0.9536
             3   StandardScalerWrapper RandomForest             0:00:08       0.9147    0.9536
             4   StandardScalerWrapper ExtremeRandomTrees       0:00:45       0.9398    0.9536
             5   MaxAbsScaler LightGBM                          0:00:08       0.9562    0.9562
             6   StandardScalerWrapper ExtremeRandomTrees       0:00:27       0.8282    0.9562
             7   StandardScalerWrapper LightGBM                 0:00:07       0.9421    0.9562
             8   MaxAbsScaler DecisionTree                      0:00:08       0.9526    0.9562
             9   MaxAbsScaler RandomForest                      0:00:09       0.9355    0.9562
            10   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            11   MaxAbsScaler LightGBM                          0:00:09       0.9553    0.9602
            12   MaxAbsScaler DecisionTree                      0:00:07       0.9484    0.9602
            13   MaxAbsScaler LightGBM                          0:00:08       0.9540    0.9602
            14   MaxAbsScaler RandomForest                      0:00:10       0.9365    0.9602
            15   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            16   StandardScalerWrapper ExtremeRandomTrees       0:00:49       0.9171    0.9602
            17   SparseNormalizer LightGBM                      0:00:08       0.9191    0.9602
            18   MaxAbsScaler DecisionTree                      0:00:08       0.9402    0.9602
            19   StandardScalerWrapper ElasticNet               0:00:08       0.9603    0.9603
            20   MaxAbsScaler DecisionTree                      0:00:08       0.9513    0.9603
            21   MaxAbsScaler SGD                               0:00:08       0.9603    0.9603
            22   MaxAbsScaler SGD                               0:00:10       0.9602    0.9603
            23   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            24   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            25   MaxAbsScaler SGD                               0:00:09       0.9603    0.9603
            26   TruncatedSVDWrapper ElasticNet                 0:00:09       0.9602    0.9603
            27   MaxAbsScaler SGD                               0:00:12       0.9413    0.9603
            28   StandardScalerWrapper ElasticNet               0:00:07       0.9603    0.9603
            29    Ensemble                                      0:00:38       0.9622    0.9622

## <a name="explore-the-results"></a>Sonuçları inceleme

Otomatik eğitim Jupyter pencere öğesi veya deneme geçmişini inceleyerek sonuçları keşfedin.

### <a name="option-1-add-a-jupyter-widget-to-see-results"></a>Seçenek 1: Sonuçları görmek için bir Jupyıter pencere öğesi ekleme

Jupyter Not Defteri kullanıyorsanız, bir grafiği ve tüm sonuçların tablosunu görmek için bu [Jupyter pencere öğesini](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) kullanın:


```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Jupi pencere öğesi çalıştırma ayrıntıları](./media/tutorial-auto-train-models/automl-dash-output.png)
![jupi pencere öğesi çizimi](./media/tutorial-auto-train-models/automl-chart-output.png)

Aynı sonuçlar çalışma alanınızda depolanır.  Çalıştırmanın sonuçlarının bağlantısını elde edebilirsiniz:

```
local_run.get_portal_url()
```


### <a name="option-2-get-and-examine-all-run-iterations-in-python"></a>Seçenek 2: Python 'da tüm çalıştırma yinelemelerini alın ve inceleyin

Her bir deneyinin geçmişini de alabilir ve her yineleme çalıştırmasına yönelik ölçümleri tek tek inceleyebilirsiniz. Her bir model çalıştırması için RMDE (root_mean_squared_error) ' i inceleyerek, en fazla yinelemenin, makul bir kenar ücreti ($ 3-4) içinde vergilenme maliyeti tahmindiğini görürsünüz.

```python
children = list(local_run.get_children())
metricslist = {}
for run in children:
    properties = run.get_properties()
    metrics = {k: v for k, v in run.get_metrics().items()
               if isinstance(v, float)}
    metricslist[int(properties['iteration'])] = metrics

rundata = pd.DataFrame(metricslist).sort_index(1)
rundata
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
      <th>0</th>
      <th>1\.</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>explained_variance</th>
      <td>0.811037</td>
      <td>0.880553</td>
      <td>0.398582</td>
      <td>0.776040</td>
      <td>0.663869</td>
      <td>0.875911</td>
      <td>0.115632</td>
      <td>0.586905</td>
      <td>0.851911</td>
      <td>0.793964</td>
      <td>...</td>
      <td>0.850023</td>
      <td>0.883603</td>
      <td>0.883704</td>
      <td>0.880797</td>
      <td>0.881564</td>
      <td>0.883708</td>
      <td>0.881826</td>
      <td>0.585377</td>
      <td>0.883123</td>
      <td>0.886817</td>
    </tr>
    <tr>
      <th>mean_absolute_error</th>
      <td>2.189444</td>
      <td>1.500412</td>
      <td>5.480531</td>
      <td>2.626316</td>
      <td>2.973026</td>
      <td>1.550199</td>
      <td>6.383868</td>
      <td>4.414241</td>
      <td>1.743328</td>
      <td>2.294601</td>
      <td>...</td>
      <td>1.797402</td>
      <td>1.415815</td>
      <td>1.418167</td>
      <td>1.578617</td>
      <td>1.559427</td>
      <td>1.413042</td>
      <td>1.551698</td>
      <td>4.069196</td>
      <td>1.505795</td>
      <td>1.430957</td>
    </tr>
    <tr>
      <th>median_absolute_error</th>
      <td>1.438417</td>
      <td>0.850899</td>
      <td>4.579662</td>
      <td>1.765210</td>
      <td>1.594600</td>
      <td>0.869883</td>
      <td>4.266450</td>
      <td>3.627355</td>
      <td>0.954992</td>
      <td>1.361014</td>
      <td>...</td>
      <td>0.973634</td>
      <td>0.774814</td>
      <td>0.797269</td>
      <td>1.147234</td>
      <td>1.116424</td>
      <td>0.783958</td>
      <td>1.098464</td>
      <td>2.709027</td>
      <td>1.003728</td>
      <td>0.851724</td>
    </tr>
    <tr>
      <th>normalized_mean_absolute_error</th>
      <td>0.024908</td>
      <td>0.017070</td>
      <td>0.062350</td>
      <td>0.029878</td>
      <td>0.033823</td>
      <td>0.017636</td>
      <td>0.072626</td>
      <td>0.050219</td>
      <td>0.019833</td>
      <td>0.026105</td>
      <td>...</td>
      <td>0.020448</td>
      <td>0.016107</td>
      <td>0.016134</td>
      <td>0.017959</td>
      <td>0.017741</td>
      <td>0.016076</td>
      <td>0.017653</td>
      <td>0.046293</td>
      <td>0.017131</td>
      <td>0.016279</td>
    </tr>
    <tr>
      <th>normalized_median_absolute_error</th>
      <td>0.016364</td>
      <td>0.009680</td>
      <td>0.052101</td>
      <td>0.020082</td>
      <td>0.018141</td>
      <td>0.009896</td>
      <td>0.048538</td>
      <td>0.041267</td>
      <td>0.010865</td>
      <td>0.015484</td>
      <td>...</td>
      <td>0.011077</td>
      <td>0.008815</td>
      <td>0.009070</td>
      <td>0.013052</td>
      <td>0.012701</td>
      <td>0.008919</td>
      <td>0.012497</td>
      <td>0.030819</td>
      <td>0.011419</td>
      <td>0.009690</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_error</th>
      <td>0.047968</td>
      <td>0.037882</td>
      <td>0.085572</td>
      <td>0.052282</td>
      <td>0.065809</td>
      <td>0.038664</td>
      <td>0.109401</td>
      <td>0.071104</td>
      <td>0.042294</td>
      <td>0.049967</td>
      <td>...</td>
      <td>0.042565</td>
      <td>0.037685</td>
      <td>0.037557</td>
      <td>0.037643</td>
      <td>0.037513</td>
      <td>0.037560</td>
      <td>0.037465</td>
      <td>0.072077</td>
      <td>0.037249</td>
      <td>0.036716</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_log_error</th>
      <td>0.055353</td>
      <td>0.045000</td>
      <td>0.110219</td>
      <td>0.065633</td>
      <td>0.063589</td>
      <td>0.044412</td>
      <td>0.123433</td>
      <td>0.092312</td>
      <td>0.046130</td>
      <td>0.055243</td>
      <td>...</td>
      <td>0.046540</td>
      <td>0.041804</td>
      <td>0.041771</td>
      <td>0.045175</td>
      <td>0.044628</td>
      <td>0.041617</td>
      <td>0.044405</td>
      <td>0.079651</td>
      <td>0.042799</td>
      <td>0.041530</td>
    </tr>
    <tr>
      <th>r2_score</th>
      <td>0.810900</td>
      <td>0.880328</td>
      <td>0.398076</td>
      <td>0.775957</td>
      <td>0.642812</td>
      <td>0.875719</td>
      <td>0.021603</td>
      <td>0.586514</td>
      <td>0.851767</td>
      <td>0.793671</td>
      <td>...</td>
      <td>0.849809</td>
      <td>0.880142</td>
      <td>0.880952</td>
      <td>0.880586</td>
      <td>0.881347</td>
      <td>0.880887</td>
      <td>0.881613</td>
      <td>0.548121</td>
      <td>0.882883</td>
      <td>0.886321</td>
    </tr>
    <tr>
      <th>root_mean_squared_error</th>
      <td>4.216362</td>
      <td>3.329810</td>
      <td>7.521765</td>
      <td>4.595604</td>
      <td>5.784601</td>
      <td>3.398540</td>
      <td>9.616354</td>
      <td>6.250011</td>
      <td>3.717661</td>
      <td>4.392072</td>
      <td>...</td>
      <td>3.741447</td>
      <td>3.312533</td>
      <td>3.301242</td>
      <td>3.308795</td>
      <td>3.297389</td>
      <td>3.301485</td>
      <td>3.293182</td>
      <td>6.335581</td>
      <td>3.274209</td>
      <td>3.227365</td>
    </tr>
    <tr>
      <th>root_mean_squared_log_error</th>
      <td>0.243184</td>
      <td>0.197702</td>
      <td>0.484227</td>
      <td>0.288349</td>
      <td>0.279367</td>
      <td>0.195116</td>
      <td>0.542281</td>
      <td>0.405559</td>
      <td>0.202666</td>
      <td>0.242702</td>
      <td>...</td>
      <td>0.204464</td>
      <td>0.183658</td>
      <td>0.183514</td>
      <td>0.198468</td>
      <td>0.196067</td>
      <td>0.182836</td>
      <td>0.195087</td>
      <td>0.349935</td>
      <td>0.188031</td>
      <td>0.182455</td>
    </tr>
    <tr>
      <th>spearman_correlation</th>
      <td>0.944743</td>
      <td>0.953618</td>
      <td>0.857965</td>
      <td>0.914703</td>
      <td>0.939846</td>
      <td>0.956159</td>
      <td>0.828187</td>
      <td>0.942069</td>
      <td>0.952581</td>
      <td>0.935477</td>
      <td>...</td>
      <td>0.951287</td>
      <td>0.960335</td>
      <td>0.960195</td>
      <td>0.960279</td>
      <td>0.960288</td>
      <td>0.960323</td>
      <td>0.960161</td>
      <td>0.941254</td>
      <td>0.960293</td>
      <td>0.962158</td>
    </tr>
    <tr>
      <th>spearman_correlation_max</th>
      <td>0.944743</td>
      <td>0.953618</td>
      <td>0.953618</td>
      <td>0.953618</td>
      <td>0.953618</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>...</td>
      <td>0.960303</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.962158</td>
    </tr>
  </tbody>
</table>
<p>12 × 30 sütunları satırlar</p>
</div>

## <a name="retrieve-the-best-model"></a>En iyi modeli alma

Bizim yinelemeler en iyi işlem hattını seçin. `get_output` Metodunda `automl_classifier` son çağırma sığdırmak için en iyi çalıştırmanın ve ekrana sığdırılmış modeli döndürür. Üzerinde aşırı yüklemeleri kullanarak, `get_output`herhangi bir günlüğe kaydedilmiş ölçüm veya belirli bir yineleme için en iyi çalıştırma ve bağlı modeli elde edebilirsiniz:

```python
best_run, fitted_model = local_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="test-the-best-model-accuracy"></a>En iyi modeli doğruluk testi

Taksi Fares 'yi tahmin etmek için test veri kümesindeki tahminleri çalıştırmak için en iyi modeli kullanın. İşlevi `predict` en iyi modeli kullanır ve `x_test` veri kümesinden y, **seyahat maliyeti**değerlerini tahmin eder. İlk 10 tahmini maliyet değerini `y_predict`Yazdır:

```python
y_predict = fitted_model.predict(x_test.values)
print(y_predict[:10])
```

Gerçek maliyet değerleriyle karşılaştırılan tahmin edilen maliyet değerlerini görselleştirmek için bir dağılım çizimi oluşturun. Aşağıdaki kod, `distance` özelliği x eksenindeki ve y ekseni olarak seyahat `cost` eden bir biçimde kullanır. Her seyahat mesafesi değerindeki tahmin edilen maliyet farkını karşılaştırmak için, tahmin edilen ilk 100 ve gerçek maliyet değerleri ayrı seriler olarak oluşturulur. Çizimin incelenmesinin, uzaklık/maliyet ilişkisinin neredeyse doğrusal olduğunu ve tahmin edilen maliyet değerlerinin çoğu durumda aynı seyahat mesafesi için gerçek maliyet değerlerine çok yakın olduğunu gösterir.

```python
%matplotlib inline

import matplotlib.pyplot as plt

fig = plt.figure(figsize=(14, 10))
ax1 = fig.add_subplot(111)

distance_vals = [x[4] for x in x_test.values]
y_actual = y_test.values.flatten().tolist()

ax1.scatter(distance_vals[:100], y_predict[:100],
            s=18, c='b', marker="s", label='Predicted')
ax1.scatter(distance_vals[:100], y_actual[:100],
            s=18, c='r', marker="o", label='Actual')

ax1.set_xlabel('distance (mi)')
ax1.set_title('Predicted and Actual Cost/Distance')
ax1.set_ylabel('Cost ($)')

plt.legend(loc='upper left', prop={'size': 12})
plt.rcParams.update({'font.size': 14})
plt.show()
```

![Tahmin dağılım çizimi](./media/tutorial-auto-train-models/automl-scatter-plot.png)

`root mean squared error` Sonuçların sayısını hesaplayın. `y_test` Veri çerçevesini kullanın. Tahmin edilen değerlerle karşılaştırmak için bunu bir listeye dönüştürün. İşlevi `mean_squared_error` iki dizi değer alır ve aralarındaki ortalama kare içinde hata sayısını hesaplar. Sonucun kare kökünü almak, y değişkeni ile aynı birimlerde bir hata verir. EPI tarifeli havayolu tahminlerinin gerçek farlarından ne kadar olduğunu kabaca gösterir:

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

    3.2204936862688798

Tam `y_actual` ve`y_predict` veri kümelerini kullanarak ortalama mutlak yüzde hatasını (mape) hesaplamak için aşağıdaki kodu çalıştırın. Bu ölçüm, tahmin edilen ve gerçek değerler arasındaki mutlak bir farkı hesaplar ve tüm farkları toplar. Ardından, bu toplamı gerçek değerlerin toplamının yüzdesi olarak ifade eder:

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
    0.10545153869569586

    Model Accuracy:
    0.8945484613043041

Son tahmin doğruluk ölçümlerinde, modelin, genellikle +-$3,00 ' de bulunan veri kümesinin özelliklerinden alınan taksi Fareli tahminlerinde oldukça iyi olduğunu görürsünüz. Geleneksel makine öğrenme modeli geliştirme sürecinde yüksek kaynak kullanımı yoğun ve çalıştırmak ve modelleri onlarca sonuçları karşılaştırmak için önemli bir etki alanı bilgilerini ve saat yatırım gerektirir. Otomatik makine öğrenimi kullanmak, senaryonuza yönelik birçok farklı modeli hızlıca test etmenin harika bir yoludur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde aşağıdaki görevleri yaptınız:

> [!div class="checklist"]
> * Bir deneme için çalışma alanı ve hazırlanan veriler yapılandırıldı.
> * Özel parametrelerle yerel olarak otomatik regresyon modeli kullanılarak eğitilen.
> * Araştırılan ve gözden geçirilmiş eğitim sonuçları.

[Modelinizi](tutorial-deploy-models-with-aml.md) Azure Machine Learning ile.
