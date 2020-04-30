---
title: Verilere erişmek için Azure Machine Learning veri kümeleri oluşturma
titleSuffix: Azure Machine Learning
description: Machine Learning deneme çalıştırmaları için verilerinize erişmek üzere Azure Machine Learning veri kümeleri oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 02/10/2020
ms.openlocfilehash: cc7a8df80e719173c7818055ab8771ddd7f73691
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81682782"
---
# <a name="create-azure-machine-learning-datasets"></a>Azure Machine Learning veri kümeleri oluşturma

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, yerel veya uzak denemeleri verilerinize erişmek için Azure Machine Learning veri kümeleri oluşturmayı öğreneceksiniz.

Azure Machine Learning veri kümeleri ile şunları yapabilirsiniz:

* Veri kümeleri tarafından başvurulan, depolama alanındaki verilerin tek bir kopyasını saklayın.

* Bağlantı dizeleri veya veri yolları hakkında endişelenmeden model eğitimi sırasında verilere sorunsuzca erişin.

* Veri paylaşma ve diğer kullanıcılarla işbirliği yapma.

## <a name="prerequisites"></a>Ön koşullar
' Veri kümelerini oluşturmak ve bunlarla çalışmak için şunlar gerekir:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.

> [!NOTE]
> Bazı veri kümesi sınıflarının [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca şu dağıtımlarda desteklenir: Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="compute-size-guidance"></a>İşlem boyutu kılavuzu

Bir veri kümesi oluştururken, işlem işleme gücünü ve verilerinizin bellekteki boyutunu gözden geçirin. Depolama alanındaki verilerinizin boyutu bir veri çerçevesindeki verilerin boyutuyla aynı değildir. Örneğin, CSV dosyalarındaki veriler bir veri çerçevesinde en fazla 10 x genişleyebilir, bu nedenle 1 GB CSV dosyası bir veri çerçevesinde 10 GB olabilir. 

Ana faktör, veri kümesinin bellek içi, yani bir veri çerçevesi olarak ne kadar büyük olduğunu gösteren bir etkendir. İşlem boyutunuzu ve işlem gücünü kullanmanızı öneririz, RAM 'in boyutu 2x olmalıdır. Bu nedenle, veri çerçeverizin 10 GB ise, veri çerçevesinin belleğe uygun ve işlenebilir olmasını sağlamak için 20 + GB RAM 'e sahip bir işlem hedefi istersiniz. Verileriniz sıkıştırılmışsa, daha fazla genişleyebilir; sıkıştırılmış Parquet biçiminde depolanan 20 GB görece seyrek veri, bellekte ~ 800 GB 'a genişleyebilir. Parquet dosyaları bir sütunlu biçimde veri depolarsanız, yalnızca sütunların yarısını içeriyorsa yalnızca ~ 400 GB 'yi belleğe yüklemeniz gerekir.
 
Pandas kullanıyorsanız, hepsi kullanacağı için 1 ' den fazla vCPU olması gerekmez. Tek bir Azure Machine Learning işlem örneğinde/düğümünde Modın ve DASK/Ray aracılığıyla kolayca paralel hale getirmek ve gerektiğinde `import pandas as pd` büyük bir kümeye ölçeklendirebilirsiniz. `import modin.pandas as pd` 
 
Veriler için yeterince büyük bir sanal disk alamazsanız iki seçeneğiniz vardır: ' bellek yetersiz ' verileri üzerinde işlem gerçekleştirmek için Spark veya Davsk gibi bir çerçeve kullanın, yani veri çerçevesi, bölüm ve işleme göre RAM bölümüne yüklenir ve nihai sonuç sonunda toplanmaktadır. Bu çok yavaşsa, Spark veya Davsk etkileşimli olarak hala kullanılabilecek bir kümeye ölçeklendirmenize imkan tanır. 

## <a name="dataset-types"></a>Veri kümesi türleri

Kullanıcıların eğitiminde kullanıcıları nasıl tükettiği temel alınarak iki veri kümesi türü vardır:

* [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) , belirtilen dosya veya dosya listesini ayrıştırarak verileri tablolu biçimde temsil eder. Bu, verileri bir Pandas veya Spark DataFrame 'e nasıl bir şekilde sunmanızı sağlar. . Csv,. `TabularDataset` tsv,. Parquet,. jsonl DOSYALARıNDAN ve SQL sorgu sonuçlarından bir nesne oluşturabilirsiniz. Tüm liste için bkz. [Tabulardatasetfactory sınıfı](https://aka.ms/tabulardataset-api-reference).

* [Filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) sınıfı, veri mağazalarınızın veya genel URL 'nizin tek veya birden çok dosyasına başvurur. Bu yöntemde, dosyaları dosya veri kümesi nesnesi olarak işlem dosyalarınıza indirebilir veya bağlayabilirsiniz. Dosyalar, derin öğrenme dahil olmak üzere daha geniş bir makine öğrenimi senaryolarına izin veren herhangi bir biçimde olabilir.

Yaklaşan API değişiklikleri hakkında daha fazla bilgi edinmek için bkz. [DataSet API Change bildirimi](https://aka.ms/tabular-dataset).

## <a name="create-datasets"></a>Veri kümeleri oluşturma

Bir veri kümesi oluşturarak, veri kaynağı konumuna, meta verilerinin bir kopyasıyla birlikte bir başvuru oluşturursunuz. Veriler mevcut konumunda kaldığı için ek depolama ücreti ödemeniz gerekmez. Hem hem de `TabularDataset` `FileDataset` veri kümelerini Python SDK 'sını kullanarak oluşturabilirsiniz https://ml.azure.com.

Azure Machine Learning tarafından erişilebilmesi için, veri kümelerinin [Azure veri depoları](how-to-access-data.md) veya genel Web URL 'lerinde yollardan oluşturulması gerekir. 

### <a name="use-the-sdk"></a>SDK 'Yı kullanma

Python SDK kullanarak bir [Azure veri deposundan](how-to-access-data.md) veri kümeleri oluşturmak için:

1. Kayıtlı Azure veri deposuna `contributor` sahip `owner` olduğunuzu veya erişiminiz olduğunu doğrulayın.

2. Veri deposundaki yollara başvurarak veri kümesini oluşturun.
> [!Note]
> Birden çok veri mağazasında birden çok yoldan bir veri kümesi oluşturabilirsiniz. İçinden veri kümesi oluşturabileceğiniz dosya sayısı veya veri boyutu için sabit sınır yoktur. Bununla birlikte, her bir veri yolu için depolama hizmetine bir dosya veya klasöre işaret edilip edilmeyeceğini denetlemek için birkaç istek gönderilir. Bu ek yük, performansın düşmesine veya başarısız olmasına neden olabilir. İçindeki 1000 dosya içeren bir klasöre başvuran bir veri kümesi, bir veri yoluna başvurulur. En iyi performansı elde etmek için veri depolarında 100 ' den az yola başvuran veri kümesi oluşturmayı öneririz.

#### <a name="create-a-tabulardataset"></a>TabularDataset oluşturma

. Csv [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none--support-multi-line-false-) veya. tsv `TabularDatasetFactory` biçimindeki dosyaları okumak ve kayıtlı olmayan bir tabulardataset oluşturmak için sınıfındaki yöntemini kullanın. Birden çok dosyadan okuyorsanız, sonuçlar tek tablolu bir gösterimde toplanacaktır. 

```Python
from azureml.core import Workspace, Datastore, Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
    
# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)

# create a TabularDataset from 3 paths in datastore
datastore_paths = [(datastore, 'weather/2018/11.csv'),
                   (datastore, 'weather/2018/12.csv'),
                   (datastore, 'weather/2019/*.csv')]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

Varsayılan olarak, bir TabularDataset oluşturduğunuzda, sütun veri türleri otomatik olarak algılanır. Çıkarılan türler beklentilerinizle eşleşmiyorsa, aşağıdaki kodu kullanarak sütun türlerini belirtebilirsiniz. Parametresi `infer_column_type` yalnızca sınırlandırılmış dosyalardan oluşturulmuş veri kümeleri için geçerlidir. [Desteklenen veri türleri hakkında daha fazla bilgi](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py)edinebilirsiniz.

> [!IMPORTANT] 
> Depolama alanınızı bir sanal ağın veya güvenlik duvarının arkasındaysa, yalnızca SDK aracılığıyla bir veri kümesinin oluşturulması desteklenir. Veri kümenizi oluşturmak için, parametreleri `validate=False` ve `infer_column_types=False` `from_delimited_files()` yöntemesinizi eklediğinizden emin olun. Bu, ilk doğrulama denetimini atlar ve veri kümenizi bu güvenli dosyalardan oluşturmanıza da emin olmanızı sağlar. 

```Python
from azureml.core import Dataset
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |Passengerıd|Kalan|PClass|Adı|Komutu|Yaş|SibSp|Parch|Bilet|Tarifeli havayolu|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen HARRIS|erkek|22,0|1|0|A/5 21171|7,2500||S
1|2|True|1|Hanler, Mrs. John Bradley (çiçek)...|kadın|38,0|1|0|BILGISAYAR 17599|71,2833|C85|C
2|3|True|3|Heıkkinen, Isabetsizlik. Laina|kadın|26,0|0|0|STON/O2. 3101282|7,9250||S


Bellek Pandas dataframe 'ten bir veri kümesi oluşturmak için, verileri CSV gibi yerel bir dosyaya yazın ve veri kümenizi bu dosyadan oluşturun. Aşağıdaki kod bu iş akışını gösterir.

```python
local_path = 'data/prepared.csv'
dataframe.to_csv(local_path)
upload the local file to a datastore on the cloud
# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required
from azureml.core import Workspace, Dataset

subscription_id = 'xxxxxxxxxxxxxxxxxxxxx'
resource_group = 'xxxxxx'
workspace_name = 'xxxxxxxxxxxxxxxx'

workspace = Workspace(subscription_id, resource_group, workspace_name)

# get the datastore to upload prepared data
datastore = workspace.get_default_datastore()

# upload the local file from src_dir to the target_path in datastore
datastore.upload(src_dir='data', target_path='data')
# create a dataset referencing the cloud location
dataset = Dataset.Tabular.from_delimited_files(datastore.path('data/prepared.csv'))
```

Azure SQL [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none--query-timeout-30-) veritabanından okumak için `TabularDatasetFactory` sınıfındaki yöntemini kullanın:

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

Tabulardataset ' te, bir zaman serisi nitelik sağlamak için veri içindeki bir sütundan veya yol deseninin verilerinin depolandığı her yerde bir zaman damgası belirtebilirsiniz. Bu belirtim zamana göre kolay ve etkili filtrelemeye olanak tanır.

Zaman damgası [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) sütununu belirtmek ve`TabularDataset` zamana göre filtrelemeyi etkinleştirmek için sınıfındaki yöntemini kullanın. Daha fazla bilgi için bkz. [NOAA Hava durumu verileri Içeren tablolu zaman serisiyle ılgılı API tanıtımı](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb).

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

Dosyaları herhangi [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) bir biçimde yüklemek `FileDatasetFactory` ve kayıtsız bir dosya veri kümesi oluşturmak için sınıfındaki yöntemini kullanın. Depolama alanınızı bir sanal ağ veya güvenlik duvarının arkasındaysa, yöntebinizdeki `validate =False` `from_files()` parametreyi ayarlayın. Bu, ilk doğrulama adımını atlar ve veri kümenizi bu güvenli dosyalardan oluşturmanıza da emin olmanızı sağlar.

```Python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
datastore_paths = [(datastore, 'animals')]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = ['https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
             'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz']
mnist_ds = Dataset.File.from_files(path=web_paths)
```

#### <a name="on-the-web"></a>Web’de 
Aşağıdaki adımlar ve animasyon, https://ml.azure.comAzure Machine Learning Studio 'da veri kümesi oluşturmayı gösterir.

![UI ile veri kümesi oluşturma](./media/how-to-create-register-datasets/create-dataset-ui.gif)

Studio 'da bir veri kümesi oluşturmak için:
1. ' Da oturum https://ml.azure.comaçın.
1. Sol bölmedeki **varlıklar** bölümünde **veri kümeleri** ' ni seçin. 
1. Veri kümenizin kaynağını seçmek için **veri kümesi oluştur** ' u seçin. Bu kaynak yerel dosyalar, bir veri deposu veya genel URL olabilir.
1. Veri kümesi türü için **tablo** veya **Dosya** seçin.
1. **Veri deposu ve dosya seçim** formunu açmak için **İleri ' yi** seçin. Bu formda, oluşturulduktan sonra veri kümenizin saklanacağı yeri ve veri kümeniz için kullanılacak veri dosyalarını seçin. 
1. **Ayarları ve önizleme** ve **şema** formlarını doldurmak için **İleri ' yi** seçin; Bunlar dosya türüne göre akıllıca doldurulmuştur ve veri kümenizi bu formlarda oluşturmadan önce daha sonra yapılandırabilirsiniz. 
1. **Ayrıntıları Onayla** formunu gözden geçirmek için **İleri ' yi** seçin. Seçimlerinizi denetleyin ve veri kümeniz için isteğe bağlı bir veri profili oluşturun. [Veri profili oluşturma](how-to-use-automated-ml-for-ml-models.md#profile)hakkında daha fazla bilgi edinin. 
1. Veri kümesi oluşturmayı gerçekleştirmek için **Oluştur** ' u seçin.

## <a name="register-datasets"></a>Veri kümelerini Kaydet

Oluşturma işlemini gerçekleştirmek için, veri kümelerinizi bir çalışma alanına kaydedin. Veri kümelerini [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--create-new-version-false-) başkalarıyla paylaşmak ve çeşitli denemeleri genelinde yeniden kullanmak için çalışma alanınıza kaydetmek üzere yöntemini kullanın:

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> Azure Machine Learning Studio aracılığıyla oluşturulan veri kümeleri otomatik olarak çalışma alanına kaydedilir.

## <a name="create-datasets-with-azure-open-datasets"></a>Azure açık veri kümeleri ile veri kümeleri oluşturma

[Azure açık veri](https://azure.microsoft.com/services/open-datasets/) kümeleri, daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz, seçkin ortak veri kümeleridir. Veri kümeleri, makine öğrenimi modellerini ve zenginleştirme çözümlerini eğitmenize yardımcı olan hava durumu, Census, tatiller, genel güvenlik ve konum için genel etki alanı verilerini içerir. Açık veri kümeleri bulutta Microsoft Azure ve hem SDK hem de çalışma alanı kullanıcı arabirimine dahildir.

### <a name="use-the-sdk"></a>SDK 'Yı kullanma

SDK 'da Azure açık veri kümeleri ile veri kümeleri oluşturmak için paketini ile `pip install azureml-opendatasets`yüklediğinizden emin olun. Her ayrık veri kümesi SDK 'daki kendi sınıfıyla temsil edilir ve belirli sınıflar `TabularDataset`, `FileDataset`, veya her ikisi olarak kullanılabilir. Sınıfların tam listesi için [başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py) bakın.

Dosyaları doğrudan yönetmenize ve/veya indirmenizi `TabularDataset` sağlayan `FileDataset`bir veya olarak belirli sınıfları alabilirsiniz. Diğer sınıflar **yalnızca** `get_tabular_dataset()` veya `get_file_dataset()` işlevlerinden birini kullanarak bir veri kümesi alabilir. Aşağıdaki kod örneği, bu sınıf türleri için birkaç örnek gösterir.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

Açık veri kümelerinden oluşturulan bir veri kümesini kaydettiğinizde, hiçbir veri hemen indirilir, ancak istendiğinde (eğitim sırasında, örneğin, merkezi bir depolama konumundan), verilere daha sonra erişilir.

### <a name="use-the-ui"></a>Kullanıcı arabirimini kullanma

Ayrıca, Kullanıcı arabirimi aracılığıyla açık veri kümeleri sınıflarından veri kümeleri oluşturabilirsiniz. Çalışma alanınızda **varlıklar**' ın altındaki **veri kümeleri** sekmesini seçin. **Veri kümesi oluştur** aşağı açılan menüsünde **açık veri kümeleri ' nden**seçim yapın.

![UI ile veri kümesini açma](./media/how-to-create-register-datasets/open-datasets-1.png)

Kutucuğunu seçerek bir veri kümesi seçin. (Arama çubuğunu kullanarak filtreleme seçeneğiniz vardır.) **İleri ' yi**seçin.

![Veri kümesi seçin](./media/how-to-create-register-datasets/open-datasets-2.png)

Veri kümesinin kaydedileceği bir ad seçin ve isteğe bağlı olarak, kullanılabilir filtreleri kullanarak verileri filtreleyin. Bu durumda, genel tatiller veri kümesi için zaman aralığını bir yıla, ülke kodunu ise yalnızca BIZIMLE filtreleyerek filtreleyebilirsiniz. **Oluştur**’u seçin.

![Veri kümesi params 'i ayarlama ve veri kümesi oluşturma](./media/how-to-create-register-datasets/open-datasets-3.png)

Veri kümesi artık veri **kümeleri**altında çalışma alanınızda kullanılabilir. Bunu, oluşturduğunuz diğer veri kümeleriyle aynı şekilde kullanabilirsiniz.

## <a name="version-datasets"></a>Sürüm veri kümeleri

Yeni bir sürüm oluşturarak aynı ada sahip yeni bir veri kümesini kaydedebilirsiniz. Veri kümesi sürümü, deneme veya gelecekteki bir çoğaltma için veri kümesinin belirli bir sürümünü uygulayabilmeniz için verilerinizin durumuna yer işaretinin bir yoludur. [Veri kümesi sürümleri](how-to-version-track-datasets.md)hakkında daha fazla bilgi edinin.
```Python
# create a TabularDataset from Titanic training data
web_paths = ['https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
             'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv']
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```

## <a name="access-datasets-in-your-script"></a>Betiğinizdeki veri kümelerine erişin

Kayıtlı veri kümelerine, Azure Machine Learning işlem gibi işlem kümelerinde hem yerel olarak hem de uzaktan erişilebilir. Kayıtlı veri kümenize denemeleri üzerinden erişmek için aşağıdaki kodu kullanarak çalışma alanınıza ve kayıtlı veri kümesine ad ile erişin. Varsayılan olarak, `Dataset` sınıfındaki [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) yöntemi, çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür.

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
* Daha fazla veri kümesi eğitimi örnekleri için bkz. [örnek Not defterleri](https://aka.ms/dataset-tutorial).
