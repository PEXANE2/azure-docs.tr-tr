---
title: Azureml veri kümeleri ile verilere erişmek için veri kümeleri oluşturma
titleSuffix: Azure Machine Learning service
description: Çeşitli kaynaklardan veri kümeleri oluşturmayı ve veri kümelerini çalışma alanınıza kaydetmeyi öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/21/2019
ms.openlocfilehash: 765ec8291ba873c6b200cf330d82e6e2ab53357d
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423120"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve erişme

Bu makalede, Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve yerel veya uzak denemeleri verilere erişme hakkında bilgi edineceksiniz.

Azure Machine Learning veri kümeleri ile şunları yapabilirsiniz: 

* Veri kümeleri tarafından başvurulan **depolama alanındaki verilerin tek bir kopyasını saklayın**

* Keşif verileri analizi aracılığıyla **verileri çözümleme** 

* Bağlantı dizesi veya veri yolu hakkında endişelenmeden **model eğitimi sırasında verilere kolayca erişin**

* **Veri paylaşma & diğer kullanıcılarla işbirliği yapma**

## <a name="prerequisites"></a>Önkoşullar

Veri kümeleri oluşturmak ve bunlarla çalışmak için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning hizmet çalışma alanı](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.

> [!Note]
> Bazı veri kümesi sınıflarının (Önizleme), [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) PAKETI (GA) üzerinde bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca aşağıdaki dağıtımlarda desteklenir:  Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="data-formats"></a>Veri biçimleri

Aşağıdaki biçimlerden bir Azure Machine Learning veri kümesi oluşturabilirsiniz:
+ [Ted](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset#from-delimited-files-path--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-no-rows--0---comment-none--include-path-false--archive-options-none-)
+ [nesnesinde](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)
+ [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)
+ [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)
+ [Pandas DataFrame](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-pandas-dataframe-dataframe--path-none--in-memory-false-)
+ [SQL sorgusu](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [ý](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)

## <a name="create-datasets"></a>Veri kümeleri oluşturma 

Bir veri kümesi oluşturarak, veri kaynağı konumuna, meta verilerinin bir kopyasıyla birlikte bir başvuru oluşturursunuz. Veriler mevcut konumunda kalır, bu nedenle ek depolama maliyeti tahakkuk etmemesi gerekir.

### <a name="create-from-local-files"></a>Yerel dosyalardan oluştur

[`auto_read_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) Sınıfından yöntemi`Dataset` olan dosya veya klasör yolunu belirterek yerel makinenizden dosyaları yükleyin.  Bu yöntem, dosya türü veya ayrıştırma bağımsız değişkenlerini belirtmenize gerek kalmadan aşağıdaki adımları gerçekleştirir:

* Sınırlandırıcıyı erteleme ve ayarlama.
* Dosyanın üst kısmında boş kayıtlar atlanıyor.
* Üst bilgi satırını erteleme ve ayarlama.
* Sütun veri türlerini esnek ve dönüştürme.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Alternatif olarak, dosyanızın ayrıştırmasını açıkça denetlemek için dosyaya özgü işlevleri kullanın. 


### <a name="create-from-azure-datastores"></a>Azure veri depolarından oluşturma

Bir [Azure veri deposundan](how-to-access-data.md)veri kümeleri oluşturmak için:

* Kayıtlı Azure veri `contributor` deposuna `owner` sahip olduğunuzu veya erişiminiz olduğunu doğrulayın.

* Veri deposundaki bir yola başvurarak veri kümesini oluşturma 

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
dstore = Datastore.get(workspace, datastore_name)
```

Bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)'tan sınırlandırılmış dosyaları okumak ve kayıtsız bir veri kümesi oluşturmak için yönteminikullanın.`from_delimited_files()`

```Python
# create an in-memory Dataset on your local machine
dataset = Dataset.from_delimited_files(dstore.path('data/src/crime.csv'))

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

## <a name="register-datasets"></a>Veri kümelerini Kaydet

Oluşturma işlemini gerçekleştirmek için, veri kümelerinizi çalışma alanına kaydedin:

Veri kümelerini başkalarıyla paylaşılabilmesi ve çeşitli denemeleri genelinde yeniden kullanmak için çalışma alanınıza kaydetmek üzere [yönteminikullanın.`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-)

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> ( `exist_ok = False` Varsayılan) ise ve aynı ada sahip bir veri kümesini diğeri ile kaydetmeye çalışırsanız bir hata oluşur. Varolan üzerine `True` yazmak için olarak ayarlayın.

## <a name="access-data-in-datasets"></a>Veri kümelerinde verilere erişme

Kayıtlı veri kümelerine, Azure Machine Learning işlem gibi işlem kümelerinde yerel olarak ve Uzaktan erişilebilir. Kayıtlı veri kümenize denemeleri üzerinden erişmek için, çalışma alanınızı ve kayıt kümenizi ada göre almak üzere aşağıdaki kodu kullanın.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
print(Dataset.list(workspace))

# Get dataset by name
dataset = Dataset.get(workspace, 'dataset_crime')

# Load data into pandas DataFrame
dataset.to_pandas_dataframe()
```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri kümelerini keşfet ve hazırlayın](how-to-explore-prepare-data.md).
* Veri kümelerini kullanmayla ilgili bir örnek için bkz. [örnek Not defterleri](https://aka.ms/dataset-tutorial).
