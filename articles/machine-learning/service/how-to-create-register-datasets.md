---
title: Azureml veri kümeleri ile verilere erişmek için veri kümeleri oluşturma
titleSuffix: Azure Machine Learning
description: Çeşitli kaynaklardan veri kümeleri oluşturmayı ve veri kümelerini çalışma alanınıza kaydetmeyi öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 10/10/2019
ms.openlocfilehash: a558658d7c853560f0939c99dc5dce739d985944
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900709"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve erişme

Bu makalede, Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve yerel veya uzak denemeleri verilere erişme hakkında bilgi edineceksiniz.

Azure Machine Learning veri kümeleri ile şunları yapabilirsiniz:

* Veri kümeleri tarafından başvurulan **depolama alanındaki verilerin tek bir kopyasını saklayın** .

* Bağlantı dizeleri veya veri yolları hakkında endişelenmeden **model eğitimi sırasında verilere kolayca erişin** .

* **Veri paylaşma & diğer kullanıcılarla işbirliği yapın** .

## <a name="prerequisites"></a>Önkoşullar

Veri kümeleri oluşturmak ve bunlarla çalışmak için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md)

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.

> [!Note]
> Bazı veri kümesi sınıflarının (Önizleme), [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca şu dağıtımlarda desteklenir: Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="dataset-types"></a>Veri kümesi türleri

Veri kümeleri, kullanıcıların eğitim aşamasında nasıl tükettiği üzerine iki tür olarak kategorize edilir.

* [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) , belirtilen dosya veya dosya listesini ayrıştırarak verileri tablolu biçimde temsil eder. Bu, verileri bir Pandas veya Spark DataFrame 'e nasıl bir şekilde sunmanızı sağlar. CSV, TSV, Parquet dosyaları, SQL sorgu sonuçları vb. `TabularDataset` nesnesi oluşturulabilir. Listenin tamamı için lütfen [belgelerimizi](https://aka.ms/tabulardataset-api-reference)ziyaret edin.

* [Filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) , veri mağazalarınızın veya genel URL 'lerdeki tek veya birden çok dosyaya başvurur. Bu, dosyaları kendi işlem dosyalarınıza indirme veya bağlama olanağı sağlar. Dosyalar, derin öğrenme dahil olmak üzere daha geniş bir makine öğrenimi senaryolarına izin veren herhangi bir biçimde olabilir.

Yaklaşan API değişiklikleri hakkında daha fazla bilgi edinmek için [buraya](https://aka.ms/tabular-dataset)bakın.

## <a name="create-datasets"></a>Veri kümeleri oluşturun

Bir veri kümesi oluşturarak, veri kaynağı konumuna, meta verilerinin bir kopyasıyla birlikte bir başvuru oluşturursunuz. Veriler mevcut konumunda kalır, bu nedenle ek depolama maliyeti tahakkuk etmemesi gerekir. Hem Tabulardataset hem de Filedataset 'ler Python SDK veya çalışma alanı giriş sayfası (Önizleme) kullanılarak oluşturulabilir. 

Azure Machine Learning tarafından erişilebilmesi için, veri kümelerinin [Azure veri depoları](how-to-access-data.md) veya genel Web URL 'lerinde yollardan oluşturulması gerekir.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

Python SDK kullanarak bir [Azure veri deposundan](how-to-access-data.md) veri kümeleri oluşturmak için:

* Kayıtlı Azure veri deposuna `contributor` veya `owner` erişiminizin olduğunu doğrulayın.

* Veri deposundaki bir yola başvurarak veri kümesini oluşturun.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)
```
#### <a name="create-tabulardatasets"></a>Tabulardataset oluşturma

CSV veya TSV biçimindeki dosyaları okumak ve kayıtlı olmayan bir TabularDataset oluşturmak için `TabularDatasetFactory` sınıfındaki [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---partition-format-none-) yöntemi kullanın. Birden çok dosyadan okuyorsanız, sonuçlar tek tablolu bir gösterimde toplanacaktır.

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)

# create a TabularDataset from a delimited file behind a public web url
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |Passengerıd|Kalan|PClass|Adı|Komutu|Yaş|SibSp|Parch|Ticket|Tarifeli havayolu|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|0|3|Braund, Mr. Owen HARRIS|erkek|22,0|1|0|A/5 21171|7,2500||S
1|2|1|1|Hanler, Mrs. John Bradley (çiçek)...|kadın|38,0|1|0|BILGISAYAR 17599|71,2833|C85|C
2|3|1|3|Heıkkinen, Isabetsizlik. Laina|kadın|26,0|0|0|STON/O2. 3101282|7,9250||S

Azure SQL veritabanı 'ndan okumak için `TabularDatasetFactory` sınıfındaki [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none-) yöntemi kullanın.

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

Tabulardataset içinde, verilerdeki bir sütundan bir zaman damgası belirtilebilir veya yol deseninin verileri, zamana göre kolay ve etkili filtreleme sağlayan bir zaman serisi nitelik etkinleştirmek için içinde depolanır.

Zaman damgası sütununu belirtmek ve zamana göre filtrelemeyi etkinleştirmek için `TabularDataset` sınıfındaki [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-) yöntemi kullanın. [Burada](https://aka.ms/azureml-tsd-notebook)daha fazla örnek ve ayrıntı bulabilirsiniz.

```Python
# create a TabularDataset with time series trait
datastore_paths = [(datastore, 'weather/*/*/*/data.parquet')]

# get a coarse timestamp column from the path pattern
dataset = Dataset.Tabular.from_parquet_files(path=datastore_path, partition_format='weather/{coarse_time:yyy/MM/dd}/data.parquet')

# set coarse timestamp to the virtual column created, and fine grain timestamp from a column in the data
dataset = dataset.with_timestamp_columns(fine_grain_timestamp='datetime', coarse_grain_timestamp='coarse_time')

# filter with time-series-trait-specific methods
data_slice = dataset.time_before(datetime(2019, 1, 1))
data_slice = dataset.time_after(datetime(2019, 1, 1))
data_slice = dataset.time_between(datetime(2019, 1, 1), datetime(2019, 2, 1))
data_slice = dataset.time_recent(timedelta(weeks=1, days=1))
```

#### <a name="create-filedatasets"></a>Dosya veri kümeleri oluştur

`FileDatasetFactory` sınıfında [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) yöntemi kullanarak dosyaları herhangi bir biçimde yükleyin ve kayıtsız bir filedataset oluşturun.

```Python
# create a FileDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'animals/dog/1.jpg'),
                  (datastore, 'animals/dog/2.jpg'),
                  (datastore, 'animals/dog/*.jpg')
                 ]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = [
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz'
           ]
mnist_ds = Dataset.File.from_files(path=web_paths)
```

### <a name="using-the-workspace-landing-page"></a>Çalışma alanı giriş sayfasını kullanma

Web deneyimi aracılığıyla bir veri kümesi oluşturmak için [çalışma alanı giriş sayfasında](https://ml.azure.com) oturum açın. Çalışma alanı giriş sayfası hem Tabulardataset hem de Filedataset öğelerinin oluşturulmasını destekler.

Aşağıdaki animasyon çalışma alanı giriş sayfasında bir veri kümesi oluşturmayı gösterir.

İlk olarak, sol bölmedeki **varlıklar** bölümünde **veri kümeleri** ' ni seçin. Ardından, veri kümenizin kaynağını seçmek için **+ veri kümesi oluştur** ' u seçin. Bu, yerel dosyalardan, veri deposundan veya genel Web URL 'lerinden olabilir. **Veri kümesi türünü**seçin: * tablosal veya dosya. **Ayarlar ve önizleme** ve **şema** formları dosya türüne göre akıllıca doldurulur. Bunları gözden geçirmek veya oluşturmadan önce veri kümenizi daha fazla yapılandırmak için **İleri ' yi** seçin. Veri kümesi oluşturma **işlemi tamamlandığında bitti** ' yi seçin.

![UI ile veri kümesi oluşturma](media/how-to-create-register-datasets/create-dataset-ui.gif)

## <a name="register-datasets"></a>Veri kümelerini Kaydet

Oluşturma işlemini gerçekleştirmek için, veri kümelerinizi bir çalışma alanına kaydedin.

Veri kümelerini başkalarıyla paylaşılabilmesi ve çeşitli denemeleri genelinde yeniden kullanmak için çalışma alanınıza kaydetmek üzere [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) yöntemini kullanın.

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data')
```

>[!Note]
> Çalışma alanı giriş sayfası aracılığıyla oluşturulan veri kümeleri otomatik olarak çalışma alanına kaydedilir.

## <a name="version-datasets"></a>Sürüm veri kümeleri

Yeni bir sürüm oluşturarak aynı ada sahip yeni bir veri kümesini kaydedebilirsiniz. Veri kümesi sürümü verilerinizin durumuna yer işaretinin bir yoludur. bu sayede, deneme veya gelecekteki bir çoğaltma için veri kümesinin belirli bir sürümünü uygulayabilirsiniz. [Veri kümesi sürümleri](how-to-version-track-datasets.md)hakkında daha fazla bilgi edinin.
```Python
# create a TabularDataset from Titanic training data
web_paths = [
            'https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
            'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv'
           ]
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```


## <a name="access-datasets-in-your-script"></a>Betiğinizdeki veri kümelerine erişin

Kayıtlı veri kümelerine, Azure Machine Learning işlem gibi işlem kümelerinde yerel olarak ve Uzaktan erişilebilir. Kayıtlı veri kümenize denemeleri üzerinden erişmek için, çalışma alanınızı ve kayıt kümenizi ada göre almak üzere aşağıdaki kodu kullanın. `Dataset` sınıfındaki [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) yöntemi, varsayılan olarak çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür.

```Python
%%writefile $script_folder/train.py

from azureml.core import Dataset, Run

run = Run.get_context()
workspace = run.experiment.workspace

dataset_name = 'titanic_ds'

# Get a dataset by name
titanic_ds = Dataset.get_by_name(workspace=workspace, name=dataset_name)

# Load a TabularDataset into pandas DataFrame
df = titanic_ds.to_pandas_dataframe()
```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri kümeleriyle eğitme hakkında](how-to-train-with-datasets.md)bilgi edinin.
* [Tabulardataset ile eğilmesi](https://aka.ms/automl-dataset)için otomatik makine öğrenimini kullanın.
* Veri kümeleri ile eğitim hakkında daha fazla örnek için bkz. [örnek Not defterleri](https://aka.ms/dataset-tutorial).
