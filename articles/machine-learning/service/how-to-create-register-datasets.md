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
ms.date: 11/04/2019
ms.openlocfilehash: aabbac60acc53cfccc29fc3dbd06575e09840d83
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497153"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve erişme

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

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

* [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) , belirtilen dosya veya dosya listesini ayrıştırarak verileri tablolu biçimde temsil eder. Bu, verileri bir Pandas veya Spark DataFrame 'e nasıl bir şekilde sunmanızı sağlar. CSV, TSV, Parquet dosyaları, SQL sorgu sonuçları vb. bir `TabularDataset` nesnesi oluşturulabilir. Listenin tamamı için lütfen [belgelerimizi](https://aka.ms/tabulardataset-api-reference)ziyaret edin.

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

#### <a name="create-a-tabulardataset"></a>TabularDataset oluşturma

Tabulardataset SDK aracılığıyla veya Azure Machine Learning Studio kullanılarak oluşturulabilir. Verilerdeki bir sütundan bir zaman damgası belirtilebilir veya yol deseninin verileri, zamana göre kolay ve etkili filtreleme sağlayan bir zaman serisi nitelik sağlamak için içinde depolanır.

CSV veya TSV biçimindeki dosyaları okumak ve kayıtlı olmayan bir TabularDataset oluşturmak için `TabularDatasetFactory` sınıfındaki [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---partition-format-none-) yöntemi kullanın. Birden çok dosyadan okuyorsanız, sonuçlar tek tablolu bir gösterimde toplanacaktır.

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

Varsayılan olarak, TabularDataset oluştururken sütun veri türleri otomatik olarak algılanır. Çıkarılan türler beklenmiyorsa, aşağıdaki kodu kullanarak sütun türlerini belirtebilirsiniz. Desteklenen [veri türleri hakkında](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py)daha fazla bilgi edinin.

```Python
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |Passengerıd|Kalan|PClass|Ad|Komutu|Yaş|SibSp|Parch|Ticket|Tarifeli havayolu|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen HARRIS|erkek|22,0|1|0|A/5 21171|7,2500||S
1|2|True|1|Hanler, Mrs. John Bradley (çiçek)...|kadın|38,0|1|0|BILGISAYAR 17599|71,2833|C85|C
2|3|True|3|Heıkkinen, Isabetsizlik. Laina|kadın|26,0|0|0|STON/O2. 3101282|7,9250||S

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

#### <a name="create-a-filedataset"></a>Dosya veri kümesi oluşturma

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

#### <a name="on-the-web"></a>Uygulama oluşturun 
Aşağıdaki adımlar ve animasyon, https://ml.azure.comAzure Machine Learning Studio 'da bir veri kümesi oluşturmayı gösterir.

![UI ile veri kümesi oluşturma](media/how-to-create-register-datasets/create-dataset-ui.gif)

Studio 'da bir veri kümesi oluşturmak için:
1. https://ml.azure.com'de oturum açın.
1. Sol bölmedeki **varlıklar** bölümünde **veri kümeleri** ' ni seçin. 
1. Veri kümenizin kaynağını seçmek için **+ veri kümesi oluştur** ' u seçin; Bu, yerel dosyalardan, veri deposundan veya genel Web URL 'lerinden olabilir.
1. Veri kümesi türü için **tablo** veya **Dosya** seçin.
1. Ayarları gözden geçirmek **ve önizleme**, **şema** ve **ayrıntıları onaylamak** için **İleri** ' yi seçin; dosya türüne göre akıllıca doldurulur. Seçimlerinizi denetlemek ve veri kümenizi oluşturmadan önce daha fazla yapılandırmak için bu formları kullanın.  
1. Veri kümesi oluşturmayı gerçekleştirmek için **Oluştur** ' u seçin.

## <a name="register-datasets"></a>Veri kümelerini Kaydet

Oluşturma işlemini gerçekleştirmek için, veri kümelerinizi bir çalışma alanına kaydedin.

Veri kümelerini başkalarıyla paylaşılabilmesi ve çeşitli denemeleri genelinde yeniden kullanmak için çalışma alanınıza kaydetmek üzere [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) yöntemini kullanın.

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> Azure Machine Learning Studio ile oluşturulan veri kümeleri, otomatik olarak çalışma alanına kaydedilir.

## <a name="create-datasets-with-azure-open-datasets"></a>Azure açık veri kümeleri ile veri kümeleri oluşturma

[Azure açık veri](https://azure.microsoft.com/services/open-datasets/) kümeleri, daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz, seçkin ortak veri kümeleridir. Veri kümeleri, makine öğrenimi modellerini ve zenginleştirme çözümlerini eğitmenize yardımcı olan hava durumu, Census, tatiller, genel güvenlik ve konum için genel etki alanı verilerini içerir. Açık veri kümeleri bulutta Microsoft Azure ve hem SDK hem de çalışma alanı kullanıcı arabirimine dahildir.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

SDK 'da Azure açık veri kümeleri ile veri kümeleri oluşturmak için paketi `pip install azureml-opendatasets`yüklediğinizden emin olun. Her ayrık veri kümesi SDK 'da kendi sınıfı tarafından temsil edilir ve belirli sınıflar `TabularDataset`, `FileDataset`veya her ikisi olarak sunulur. Sınıfların tam listesi için [başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py) bakın.

Çoğu sınıf öğesinden devralınır ve bir `TabularDataset`örneğinden döndürülür. Bu sınıfların örneklerine `PublicHolidays`, `BostonSafety`ve `UsPopulationZip`dahildir. Bu sınıf türlerinden bir `TabularDataset` oluşturmak için oluşturucuyu bağımsız değişken olmadan kullanın. Açık veri kümelerinden oluşturulan bir veri kümesini kaydettiğinizde, hiçbir veri hemen indirilir, ancak istendiğinde (eğitim sırasında, örneğin, merkezi bir depolama konumundan), verilere daha sonra erişilir. 

```python
from azureml.opendatasets import UsPopulationZip

tabular_dataset = UsPopulationZip()
tabular_dataset = tabular_dataset.register(workspace=workspace, name="pop data", description="US population data by zip code")
```

Belirli sınıflar, dosyaları doğrudan yönetmenize ve/veya indirmenizi sağlayan bir `TabularDataset` ya da `FileDataset`olarak alınabilir. Diğer sınıflar yalnızca `get_tabular_dataset()` **ya** da `get_file_dataset()` işlevlerini kullanarak bir veri kümesi alabilir. Aşağıdaki kod örneği, bu sınıf türleri için birkaç örnek gösterir.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY return TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

### <a name="using-the-ui"></a>Kullanıcı arabirimini kullanma

Ayrıca, Kullanıcı arabirimini kullanarak açık veri kümeleri sınıflarından veri kümeleri oluşturabilirsiniz. Çalışma alanınızda *varlıklar*' ın altındaki **veri kümeleri** sekmesine gidin. **Veri kümesi oluştur** açılır listesine ve ardından **açık veri kümeleri**' ne tıklayın.

![UI ile veri kümesini açma](media/how-to-create-register-datasets/open-datasets-1.png)

Ardından, kutucuğu seçerek ve isteğe bağlı olarak arama çubuğunu kullanarak filtreleyerek bir veri kümesi seçin. Ardından **İleri**'ye tıklayın.

![Veri kümesi seçin](media/how-to-create-register-datasets/open-datasets-2.png)

Ardından, veri kümesinin kaydedileceği bir ad seçin ve isteğe bağlı olarak, kullanılabilir filtreleri kullanarak verileri filtreleyin. Bu durumda, genel tatiller veri kümesi için zaman aralığını bir yıla, ülke kodunu ise yalnızca BIZIMLE filtreleyerek filtreleyebilirsiniz. Sonra **Oluştur**’a tıklayın.

![Veri kümesi params 'i ayarlama ve veri kümesi oluşturma](media/how-to-create-register-datasets/open-datasets-3.png)

Veri kümesi artık oluşturulur ve **veri kümeleri**altında çalışma alanınızda kullanılabilir ve oluşturduğunuz diğer veri kümeleriyle aynı şekilde kullanılabilir.

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
