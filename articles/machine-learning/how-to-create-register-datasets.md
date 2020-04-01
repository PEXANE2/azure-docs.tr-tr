---
title: Verilere erişmek için Azure Machine Learning veri kümeleri oluşturma
titleSuffix: Azure Machine Learning
description: Makine öğrenimi deneme çalıştırmaları için verilerinize erişmek için Azure Machine Learning veri kümelerini nasıl oluşturabilirsiniz öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 02/10/2020
ms.openlocfilehash: 4025c620aea49dfb26ab203630c121d29d88d9d7
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474540"
---
# <a name="create-azure-machine-learning-datasets"></a>Azure Machine Learning veri kümeleri oluşturma

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, yerel veya uzak denemelerinizin verilerine erişmek için Azure Machine Learning veri kümelerini nasıl oluşturabileceğinizi öğreneceksiniz.

Azure Machine Learning veri kümeleri ile şunları yapabilirsiniz:

* Veri kümeleri tarafından başvurulan tek bir veri kopyasını depolama alanınızda saklayın.

* Bağlantı dizeleri veya veri yolları hakkında endişelenmeden model eğitimi sırasında verilere sorunsuz bir şekilde erişin.

* Verileri paylaşın ve diğer kullanıcılarla işbirliği yapın.

## <a name="prerequisites"></a>Ön koşullar
' Veri kümeleri oluşturmak ve onlarla çalışmak için şunları yapmanız gerekir:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* [Azure Makine Öğrenimi çalışma alanı.](how-to-manage-workspace.md)

* Azureml-datasets paketini içeren [Python için Azure Machine Learning SDK yüklendi.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)

> [!NOTE]
> Bazı veri kümesi sınıflarının [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için bu sınıflar yalnızca aşağıdaki dağıtımlarda desteklenir: Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="compute-size-guidance"></a>İşlem boyutu kılavuzu

Bir veri kümesi oluştururken, bilgi işlem gücünüzü ve bellekteki verilerinizin boyutunu gözden geçirin. Depolamadaki verilerinizin boyutu, veri çerçevesindeki verilerin boyutuyla aynı değildir. Örneğin, CSV dosyalarındaki veriler bir veri çerçevesinde 10 kata kadar genişletilebilir, böylece 1 GB'lık bir CSV dosyası veri çerçevesinde 10 GB'a kadar çıkabilir. 

Ana faktör, veri kümesinin bellekte ne kadar büyük olduğudur, yani bir veri çerçevesi olarak. İşlem boyutuve işlem gücünüzün RAM boyutundan 2 kat daha fazla içerdiğini tavsiye ediyoruz. Bu nedenle, veri çerçeveniz 10 GB ise, veri çerçevesinin belleğe rahatça sığmasını ve işlenmesini sağlamak için 20+ GB RAM içeren bir bilgi işlem hedefi istersiniz. Verileriniz sıkıştırılmışsa, daha da genişleyebilir; Sıkıştırılmış parke formatında depolanan 20 GB'lık nispeten seyrek veri bellekte ~800 GB'a kadar genişleyebilir. Parke dosyaları verileri sütun biçiminde depoladığından, sütunların yalnızca yarısına ihtiyacınız varsa, yalnızca ~400 GB bellek yüklemeniz gerekir.
 
Pandalar kullanıyorsanız, tüm kullanacağı bu yana 1 vCPU'dan fazla olması için bir neden yoktur. Modin ve Dask/Ray üzerinden tek bir Azure Machine Learning bilgi işlem örneğinde/düğümde birçok vCPUs'a `import pandas as pd` `import modin.pandas as pd`kolayca paralelleyebilir ve gerekirse büyük bir kümeye ölçeklendirebilirsiniz. 
 
Veriler için yeterince büyük bir sanal alamıyorsanız, iki seçeneğiniz vardır: 'bellek dışında' veri işleme gerçekleştirmek için Kıvılcım veya Dask gibi bir çerçeve kullanın, yani veri çerçevesi bölümleme tarafından RAM bölümüne yüklenir ve işlenir, nihai sonuç sonunda toplanır. Bu çok yavaşsa, Spark veya Dask hala etkileşimli olarak kullanılabilen bir kümeye ölçeklendirmenize olanak sağlar. 

## <a name="dataset-types"></a>Veri kümesi türleri

Kullanıcıların bunları eğitimde nasıl tüketire bağlı olarak iki veri kümesi türü vardır:

* [TabularDataset,](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) sağlanan dosyayı veya dosya listesini ayrıştarak verileri tabular biçiminde temsil eder. Bu, verileri Pandalar veya Spark DataFrame'e aktarabilme olanağı sağlar. .csv, .tsv, .parke, .jsonl dosyaları ve SQL sorgu sonuçlarından bir `TabularDataset` nesne oluşturabilirsiniz. Tam liste için [Bkz. TabularDatasetFactory sınıfı.](https://aka.ms/tabulardataset-api-reference)

* [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) sınıfı, veri depolarınızda veya herkese açık URL'lerinizde tek veya birden çok dosyaya başvurur. Bu yöntemle, dosyaları dosyayı dosyanıza FileDataset nesnesi olarak indirebilir veya monte edebilirsiniz. Dosyalar, derin öğrenme de dahil olmak üzere daha geniş bir makine öğrenimi senaryosu yelpazesine olanak tanıyan herhangi bir biçimde olabilir.

Yaklaşan API değişiklikleri hakkında daha fazla bilgi edinmek için [Dataset API değişiklik bildirimine](https://aka.ms/tabular-dataset)bakın.

## <a name="create-datasets"></a>Veri kümeleri oluşturma

Bir veri kümesi oluşturarak, meta verilerinin bir kopyasıyla birlikte veri kaynağı konumuna bir başvuru oluşturursunuz. Veriler varolan konumunda kaldığından, ek depolama maliyetine neden olmazsınız. Python SDK'yı kullanarak veya .'de `TabularDataset` `FileDataset` https://ml.azure.comhem de veri kümelerini oluşturabilirsiniz.

Verilerin Azure Machine Learning tarafından erişilebilir olabilmesi için, veri kümelerinin [Azure veri depolarında](how-to-access-data.md) veya herkese açık web URL'lerinden oluşturulması gerekir. 

### <a name="use-the-sdk"></a>SDK'yı kullan

Python SDK'yı kullanarak bir [Azure veri deposundan](how-to-access-data.md) veri kümeleri oluşturmak için:

1. Kayıtlı Azure `contributor` veri `owner` deposuna sahip olduğunuzu veya bu verilere eriştinolduğunuzu doğrulayın.

2. Veri deposundaki yollara başvurarak veri kümesini oluşturun.
> [!Note]
> Birden çok veri deposunda birden çok yoldan bir veri kümesi oluşturabilirsiniz. Veri kümesi oluşturabileceğiniz dosya veya veri boyutu sayısında sabit bir sınır yoktur. Ancak, her veri yolu için, bir dosyayı veya klasörü işaret edip etmediğini denetlemek için depolama hizmetine birkaç istek gönderilir. Bu genel merkez, performansın düşmesine veya başarısızlığa yol açabilir. İçinde 1000 dosya bulunan bir klasöre atıfta bulunan bir veri kümesi, bir veri yoluna başvurmak olarak kabul edilir. En iyi performans için veri depolarında 100'den az yola atıfta bulunan veri kümesi oluşturmanızı öneririz.

#### <a name="create-a-tabulardataset"></a>Bir TabularDataset oluşturma

.csv `TabularDatasetFactory` veya .tsv biçimindeki dosyaları okumak ve kayıtdışı bir TabularDataset oluşturmak için sınıftaki [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none-) yöntemi kullanın. Birden çok dosyadan okuyorsanız, sonuçlar tek bir satır gösteriminde toplanır. 

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

Varsayılan olarak, bir TabularDataset oluşturduğunuzda, sütun veri türleri otomatik olarak çıkarılır. Çıkarılan türler beklentilerinize uymuyorsa, aşağıdaki kodu kullanarak sütun türlerini belirtebilirsiniz. Parametre `infer_column_type` yalnızca sınırlı dosyalardan oluşturulan veri kümeleri için geçerlidir. [Desteklenen veri türleri hakkında daha fazla bilgi edinebilirsiniz.](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py)

> [!IMPORTANT] 
> Depolama alanınız sanal bir ağın veya güvenlik duvarının arkasındaysa, yalnızca SDK aracılığıyla bir veri kümesi oluşturulması desteklenir. Veri kümenizi oluşturmak için parametreleri `validate=False` ve `infer_column_types=False` yönteminize `from_delimited_files()` eklediğinden emin olun. Bu, ilk doğrulama denetimini atlar ve bu güvenli dosyalardan veri kümenizi oluşturabilmesini sağlar. 

```Python
from azureml.core import Dataset
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |Yolcu Kimliği|Hayatta|Pclass|Adı|Seks|Yaş|SibSp|Parşa|Bilet|Ücret|Kabin|Girişti
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Bay Owen Harris.|erkek|22.0|1|0|A/5 21171|7.2500||S
1|2|True|1|Cumings, Bayan John Bradley (Floransa Briggs Th...|kadın|38.0|1|0|ADET 17599|71.2833|C85|C
2|3|True|3|Heikkinen, bayan. Laina|kadın|26.0|0|0|STON/O2. 3101282|7.9250||S


Bir in memory pandas veri çerçevesinden bir veri kümesi oluşturmak için, verileri csv gibi yerel bir dosyaya yazın ve bu dosyadan veri kümenizi oluşturun. Aşağıdaki kod bu iş akışını gösterir.

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

Azure [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none--query-timeout-30-) SQL Veritabanı'ndan okumak için sınıftaki `TabularDatasetFactory` yöntemi kullanın:

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

TabularDatasets'te, bir zaman serisi özelliğini etkinleştirmek için verilerdeki bir sütundan veya yol deseni verilerinin depolanan yerden bir zaman damgası belirtebilirsiniz. Bu belirtim, zamana göre kolay ve verimli filtreleme sağlar.

Zaman [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-) damgası`TabularDataset` sütununuzu belirtmek ve zamana göre filtreleme yi etkinleştirmek için sınıftaki yöntemi kullanın. Daha fazla bilgi için [NOAA hava durumu verileriyle Tabular zaman serisiyle ilgili API](https://aka.ms/azureml-tsd-notebook)demosu'na bakın.

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

#### <a name="create-a-filedataset"></a>FileDataset oluşturma

Dosyaları [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) herhangi bir `FileDatasetFactory` biçimde yüklemek ve kayıtdışı bir FileDataset oluşturmak için sınıftaki yöntemi kullanın. Depolama alanınız sanal bir ağın veya güvenlik duvarının `from_files()` arkasındaysa, yönteminizdeki parametreyi `validate =False` ayarlayın. Bu, ilk doğrulama adımını atlar ve bu güvenli dosyalardan veri kümenizi oluşturabilmesini sağlar.

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
Aşağıdaki adımlar ve animasyon, Azure Machine Learning stüdyosunda https://ml.azure.comnasıl bir veri kümesi oluşturulacak şekilde gösteriş gösterir.

![UI ile bir veri kümesi oluşturma](./media/how-to-create-register-datasets/create-dataset-ui.gif)

Stüdyoda bir veri kümesi oluşturmak için:
1. Oturum aç. https://ml.azure.com
1. Sol bölmenin **Varlıklar** bölümünde **Veri Kümeleri'ni** seçin. 
1. **Veri kümenizin** kaynağını seçmek için Veri Kümesi Oluştur'u seçin. Bu kaynak yerel dosyalar, veri deposu veya ortak URL'ler olabilir.
1. Dataset türü için **Tabular** veya **Dosya'yı** seçin.
1. **Datastore ve dosya seçim** formunu açmak için **İleri'yi** seçin. Bu formda, oluşturulduktan sonra veri kümenizi nerede tutacağınızı ve veri kümeniz için hangi veri dosyalarını kullanacağınızı seçersiniz. 
1. Ayarlar ve önizleme **ve** **Şema** formlarını doldurmak için **İleri'yi** seçin; dosya türüne göre akıllıca doldurulur ve bu formlarda oluşturulmeden önce veri kümenizi daha da yapılandırabilirsiniz. 
1. **Ayrıntıları Doğrula** formunu gözden geçirmek için **İleri'yi** seçin. Seçimlerinizi kontrol edin ve veri setiniz için isteğe bağlı bir veri profili oluşturun. [Veri profilleme](how-to-use-automated-ml-for-ml-models.md#profile)hakkında daha fazla bilgi edinin. 
1. Veri kümesi oluşturmanızı tamamlamak için **Oluştur'u** seçin.

## <a name="register-datasets"></a>Veri kümelerini kaydetme

Oluşturma işlemini tamamlamak için veri kümelerinizi bir çalışma alanına kaydedin. Veri [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--create-new-version-false-) kümelerini başkalarıyla paylaşmak ve çeşitli denemeler de yeniden kullanmak için veri kümelerini çalışma alanınızla kaydetmek için yöntemi kullanın:

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> Azure Machine Learning stüdyosu aracılığıyla oluşturulan veri kümeleri otomatik olarak çalışma alanına kaydedilir.

## <a name="create-datasets-with-azure-open-datasets"></a>Azure Açık Veri Kümeleri ile veri kümeleri oluşturma

[Azure Açık Veri Kümeleri,](https://azure.microsoft.com/services/open-datasets/) daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz genel veri kümeleridir. Veri kümeleri, hava durumu, nüfus sayımı, tatiller, genel güvenlik ve makine öğrenimi modellerini eğitmenize ve tahmine dayalı çözümleri zenginleştirmenize yardımcı olan konum için genel alan verilerini içerir. Açık Veri Kümeleri Microsoft Azure'da buluttadır ve hem SDK'ya hem de çalışma alanı arabirimi'ne dahildir.

### <a name="use-the-sdk"></a>SDK'yı kullan

SDK'dan Azure Açık Veri Kümeleri ile veri kümeleri oluşturmak için `pip install azureml-opendatasets`paketi '' ile yüklediğinizden emin olun Her ayrık veri kümesi SDK'da kendi sınıfı tarafından temsil edilir ve `TabularDataset`belirli `FileDataset`sınıflar , veya her ikisi olarak kullanılabilir. Sınıfların tam listesi için [başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py) bakın.

Dosyaları doğrudan işlemeve/veya `TabularDataset` `FileDataset`indirmenize olanak tanıyan belirli sınıfları a veya , olarak alabilirsiniz. Diğer sınıflar yalnızca bir veri **kümesini** `get_tabular_dataset()` `get_file_dataset()` kullanarak alabilir. Aşağıdaki kod örneği, bu tür sınıfların birkaç örneğini gösterir.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY return TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

Açık Veri Kümeleri'nden oluşturulan bir veri kümesini kaydettiğinizde, hemen veri indirilmez, ancak verilere daha sonra merkezi bir depolama konumundan istendiğinde (örneğin eğitim sırasında) erişilir.

### <a name="use-the-ui"></a>UI'yi kullanma

Ayrıca, UI aracılığıyla Açık Veri Kümeleri sınıflarından veri kümeleri oluşturabilirsiniz. Çalışma alanınızda, **Varlıklar**altında **Veri Kümeleri** sekmesini seçin. Veri **kümesi** açılır menüsünde, **Veri Kümelerini Aç'tan'ı**seçin.

![UI ile Veri Kümesini Aç](./media/how-to-create-register-datasets/open-datasets-1.png)

Döşemesini seçerek bir veri kümesi seçin. (Arama çubuğunu kullanarak filtreleme seçeneğiniz vardır.) **Sonraki'ni**seçin.

![Veri kümesini seçin](./media/how-to-create-register-datasets/open-datasets-2.png)

Veri kümesini kaydetmek için altında bir ad seçin ve kullanılabilir filtreleri kullanarak isteğe bağlı olarak verileri filtreleyin. Bu durumda, resmi tatillerde veri kümesi için, süreyi bir yıla, ülke kodunu ise yalnızca ABD'ye filtrelersiniz. **Oluştur'u**seçin.

![Veri kümesi paramlarını ayarlama ve veri kümesi oluşturma](./media/how-to-create-register-datasets/open-datasets-3.png)

Veri kümesi artık çalışma alanınızda **Datasets**altında kullanılabilir. Oluşturduğunuz diğer veri kümeleri ile aynı şekilde kullanabilirsiniz.

## <a name="version-datasets"></a>Sürüm veri kümeleri

Yeni bir sürüm oluşturarak aynı adaltında yeni bir veri kümesi kaydedebilirsiniz. Dataset sürümü, deneme veya gelecekteki çoğaltma için veri kümesinin belirli bir sürümünü uygulayabilmeniz için verilerinizin durumunu yer imi belirlemenin bir yoludur. [Dataset sürümleri](how-to-version-track-datasets.md)hakkında daha fazla bilgi edinin.
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

## <a name="access-datasets-in-your-script"></a>Komut dosyanızda veri kümelerini erişin

Kayıtlı veri kümelerine Azure Machine Learning bilgi işlem ağı gibi bilgi işlem kümelerinde hem yerel hem de uzaktan erişilebilir. Denemeler arasında kayıtlı veri kümenize erişmek için çalışma alanınıza ve kayıtlı veri kümenize ada göre erişmek için aşağıdaki kodu kullanın. Varsayılan olarak, [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) sınıftaki `Dataset` yöntem çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür.

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

* [Veri kümeleriyle nasıl eğitim verilebildiğini](how-to-train-with-datasets.md)öğrenin.
* [TabularDatasets ile eğitmek](https://aka.ms/automl-dataset)için otomatik makine öğrenimini kullanın.
* Daha fazla veri seti eğitim örneği için [örnek not defterlerine](https://aka.ms/dataset-tutorial)bakın.
