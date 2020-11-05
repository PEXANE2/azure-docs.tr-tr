---
title: Azureml veri kümeleriyle eğitme
titleSuffix: Azure Machine Learning
description: Verilerinizi Azure Machine Learning veri kümeleri ile ML modeli eğitimi için yerel veya uzak işlem için nasıl kullanılabilir yapacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 07/31/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, data4ml
ms.openlocfilehash: bf1dcb34b762c3dcae39316d804b617edbf2302d
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93360743"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleriyle eğitme


Bu makalede, eğitim denemeleri [Azure Machine Learning veri kümeleriyle](/python/api/azureml-core/azureml.core.dataset%28class%29?preserve-view=true&view=azure-ml-py) nasıl çalışacağınızı öğreneceksiniz.  Bağlantı dizeleri veya veri yolları hakkında endişelenmeden, yerel veya uzaktan işlem Hedefinizdeki veri kümelerini kullanabilirsiniz.

Azure Machine Learning veri kümeleri, [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py), [hyperdrive](/python/api/azureml-train-core/azureml.train.hyperdrive?preserve-view=true&view=azure-ml-py) ve [Azure Machine Learning işlem hatları](how-to-create-your-first-pipeline.md)gibi Azure Machine Learning eğitim işlevleriyle sorunsuz bir tümleştirme sağlar.

## <a name="prerequisites"></a>Önkoşullar

Veri kümeleri oluşturup eğitmeniz için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

* Azureml [için Azure Machine Learning SDK 'sı yüklendi](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py) (>= 1.13.0), bu da azureml-veri kümesi paketini içerir.

> [!Note]
> Bazı veri kümesi sınıflarının [azureml-dataprep](/python/api/azureml-dataprep/?preserve-view=true&view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca şu dağıtımlarda desteklenir: Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="use-datasets-directly-in-training-scripts"></a>Doğrudan eğitim betiklerde veri kümelerini kullanma

Henüz bir veri kümesi olarak kayıtlı olmayan yapılandırılmış veriler varsa, bir TabularDataset oluşturun ve bunu yerel veya uzaktan denemenize yönelik eğitim betiğinizdeki doğrudan kullanın.

Bu örnekte, kayıtlı olmayan bir [Tabulardataset](/python/api/azureml-core/azureml.data.tabulardataset?preserve-view=true&view=azure-ml-py) oluşturun ve bunu eğitim için ScriptRunConfig nesnesinde bir betik bağımsız değişkeni olarak belirtirsiniz. Bu TabularDataset 'i çalışma alanınızdaki diğer denemeleri birlikte yeniden kullanmak istiyorsanız, bkz. [veri kümelerini çalışma alanınıza kaydetme](how-to-create-register-datasets.md#register-datasets).

### <a name="create-a-tabulardataset"></a>TabularDataset oluşturma

Aşağıdaki kod, bir Web URL 'sinden kayıtlı olmayan bir TabularDataset oluşturur.  

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

TabularDataset nesneleri, Not defterinize çıkmadan tanıdık veri hazırlama ve eğitim kitaplıklarıyla çalışabilmeniz için TabularDataset 'teki verileri bir Pandas veya Spark veri çerçevesine yükleme olanağı sağlar.

### <a name="access-dataset-in-training-script"></a>Eğitim betiğinde veri kümesine erişme

Aşağıdaki kod, eğitim çalıştırmanızı yapılandırırken belirlediğiniz bir betik bağımsız değişkenini yapılandırır `--input-data` (sonraki bölüme bakın). Tablosal veri kümesi bağımsız değişken değeri olarak geçirildiğinde, Azure ML, veri kümesinin KIMLIĞINI çözer ve daha sonra eğitim betiğinizdeki veri kümesine erişmek için kullanabilirsiniz (betiğinizdeki veri kümesinin adını veya KIMLIĞINI kodlamadan bağımsız olarak kodlayın). Daha sonra, [`to_pandas_dataframe()`](/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) eğitim öncesinde daha fazla veri araştırması ve hazırlığı için bu veri kümesini bir Pandas dataframe 'e yüklemek için yöntemini kullanır.

> [!Note]
> Özgün veri kaynağınız NaN, boş dizeler veya boş değerler içeriyorsa, kullandığınızda `to_pandas_dataframe()` Bu değerler *null* değer olarak değişir.

Hazırlanan verileri, bellek içi Pandas dataframe 'ten yeni bir veri kümesine yüklemeniz gerekiyorsa, verileri bir Parquet gibi yerel bir dosyaya yazın ve bu dosyadan yeni bir veri kümesi oluşturun. Ayrıca, yerel dosyalardan veya veri depolarındaki yollardan veri kümeleri oluşturabilirsiniz. [Veri kümeleri oluşturma](how-to-create-register-datasets.md)hakkında daha fazla bilgi edinin.

```Python
%%writefile $script_folder/train_titanic.py

import argparse
from azureml.core import Dataset, Run

parser = argparse.ArgumentParser()
parser.add_argument("--input-data", type=str)
args = parser.parse_args()

run = Run.get_context()
ws = run.experiment.workspace

# get the input dataset by ID
dataset = Dataset.get_by_id(ws, id=args.input_data)

# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

### <a name="configure-the-training-run"></a>Eğitim çalıştırmasını yapılandırma
Eğitim çalıştırmasını yapılandırmak ve göndermek için bir [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrun?preserve-view=true&view=azure-ml-py) nesnesi kullanılır.

Bu kod, şunu belirten bir ScriptRunConfig nesnesi oluşturur `src`

* Betikleriniz için bir komut dosyası dizini. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir.
* Eğitim betiği, *train_titanic. Kopyala*.
* Eğitim için giriş veri kümesi, `titanic_ds` komut dosyası bağımsız değişkeni olarak. Azure ML, betiğe geçirildiğinde bu veri kümesinin karşılık gelen KIMLIĞINE çözüm olarak çözer.
* Çalıştırma için işlem hedefi.
* Çalıştırmanın ortamı.

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=script_folder,
                      script='train_titanic.py',
                      # pass dataset as an input with friendly name 'titanic'
                      arguments=['--input-dataset', titanic_ds],
                      compute_target=compute_target,
                      environment=myenv)
                             
# Submit the run configuration for your training run
run = experiment.submit(src)
run.wait_for_completion(show_output=True)                             
```

## <a name="mount-files-to-remote-compute-targets"></a>Dosyaları uzaktan işlem hedeflerine bağlama

Yapılandırılmamış verileriniz varsa, bir [dosya veri kümesi](/python/api/azureml-core/azureml.data.filedataset?preserve-view=true&view=azure-ml-py) oluşturun ve veri dosyalarınızı, eğitim için uzaktan işlem hedefi için kullanılabilir hale getirmek üzere bağlayın veya indirin. Uzaktan Eğitim denemeleri için [bağlama ve indirme](#mount-vs-download) bilgilerini ne zaman kullanacağınızı öğrenin. 

Aşağıdaki örnek bir dosya veri kümesi oluşturur ve veri kümesini, eğitim betiğine bir bağımsız değişken olarak geçirerek işlem hedefine bağlar. 

> [!Note]
> Özel bir Docker temel görüntüsü kullanıyorsanız, `apt-get install -y fuse` veri kümesi bağlama 'nın çalışması için bir bağımlılık olarak kullanarak sigortası yüklemeniz gerekir. [Özel bir yapı görüntüsü](how-to-deploy-custom-docker-image.md#build-a-custom-base-image)oluşturmayı öğrenin.

### <a name="create-a-filedataset"></a>Dosya veri kümesi oluşturma

Aşağıdaki örnek, Web URL 'lerinden kaydedilmemiş bir dosya veri kümesi oluşturur. Diğer kaynaklardan [veri kümeleri oluşturma](how-to-create-register-datasets.md) hakkında daha fazla bilgi edinin.

```Python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
mnist_ds = Dataset.File.from_files(path = web_paths)
```

### <a name="configure-the-training-run"></a>Eğitim çalıştırmasını yapılandırma
Oluşturucunun parametresi aracılığıyla bağlanırken veri kümesini bir bağımsız değişken olarak geçirmeyi öneririz `arguments` `ScriptRunConfig` . Bunu yaptığınızda, eğitim betiğinizdeki veri yolunu (takma noktası) bağımsız değişkenler aracılığıyla alacaksınız. Bu şekilde, yerel hata ayıklama ve herhangi bir bulut platformunda uzaktan eğitim için aynı eğitim betiğini kullanacaksınız.

Aşağıdaki örnek ile FileDataset içinde geçen bir ScriptRunConfig oluşturur `arguments` . Çalışmayı gönderdikten sonra, veri kümesi tarafından başvurulan veri dosyaları `mnist_ds` işlem hedefine bağlanır.

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=script_folder,
                      script='train_mnist.py',
                      # the dataset will be mounted on the remote compute and the mounted path passed as an argument to the script
                      arguments=['--data-folder', mnist_ds.as_mount(), '--regularization', 0.5],
                      compute_target=compute_target,
                      environment=myenv)

# Submit the run configuration for your training run
run = experiment.submit(src)
run.wait_for_completion(show_output=True)
```

### <a name="retrieve-the-data-in-your-training-script"></a>Eğitim betiğinizdeki verileri alın

Aşağıdaki kod, betiğinizdeki verilerin nasıl alınacağını gösterir.

```Python
%%writefile $script_folder/train_mnist.py

import argparse
import os
import numpy as np
import glob

from utils import load_data

# retrieve the 2 arguments configured through `arguments` in the ScriptRunConfig
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = args.data_folder
print('Data folder:', data_folder)

# get the file paths on the compute
X_train_path = glob.glob(os.path.join(data_folder, '**/train-images-idx3-ubyte.gz'), recursive=True)[0]
X_test_path = glob.glob(os.path.join(data_folder, '**/t10k-images-idx3-ubyte.gz'), recursive=True)[0]
y_train_path = glob.glob(os.path.join(data_folder, '**/train-labels-idx1-ubyte.gz'), recursive=True)[0]
y_test = glob.glob(os.path.join(data_folder, '**/t10k-labels-idx1-ubyte.gz'), recursive=True)[0]

# load train and test set into numpy arrays
X_train = load_data(X_train_path, False) / 255.0
X_test = load_data(X_test_path, False) / 255.0
y_train = load_data(y_train_path, True).reshape(-1)
y_test = load_data(y_test, True).reshape(-1)
```

## <a name="mount-vs-download"></a>Bağlama vs indirmesi

Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı tarafından oluşturulan veri kümelerinde herhangi bir biçimdeki dosyaları bağlama veya indirme işlemi desteklenir. 

**Bir veri kümesini bağladığınızda,** veri kümesinin başvurduğu dosyaları bir dizine (bağlama noktası) ekler ve işlem hedefinde kullanılabilir hale getirin. Bağlama, Azure Machine Learning Işlem, sanal makineler ve HDInsight dahil olmak üzere Linux tabanlı hesaplar için desteklenir. 

Bir veri kümesini **indirdiğinizde** , veri kümesi tarafından başvurulan tüm dosyalar işlem hedefine indirilir. Tüm işlem türleri için indirme desteklenir. 

Betiğinizin veri kümesi tarafından başvurulan tüm dosyaları işliyorsa ve işlem diskiniz tam veri kümesine uyuyorsa, depolama hizmetlerinden veri akışı yükünü ortadan kaldırmak için indirme önerilir. Veri boyutunuz işlem diski boyutunu aşarsa, indirme mümkün değildir. Bu senaryo için, işleme sırasında yalnızca komut dosyası tarafından kullanılan veri dosyaları yüklendiğinden, bağlama yapmanız önerilir.

Aşağıdaki kod, şu `dataset` adreste geçici dizine bağlar: `mounted_path`

```python
import tempfile
mounted_path = tempfile.mkdtemp()

# mount dataset onto the mounted_path of a Linux-based compute
mount_context = dataset.mount(mounted_path)

mount_context.start()

import os
print(os.listdir(mounted_path))
print (mounted_path)
```

## <a name="directly-access-datasets-in-your-script"></a>Betikinizdeki veri kümelerine doğrudan erişin

Kayıtlı veri kümelerine, Azure Machine Learning işlem gibi işlem kümelerinde hem yerel olarak hem de uzaktan erişilebilir. Kayıtlı veri kümenize denemeleri üzerinden erişmek için aşağıdaki kodu kullanarak çalışma alanınıza ve kayıtlı veri kümesine ad ile erişin. Varsayılan olarak, [`get_by_name()`](/python/api/azureml-core/azureml.core.dataset.dataset?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-by-name-workspace--name--version--latest--) sınıfındaki yöntemi, `Dataset` çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür.

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

## <a name="accessing-source-code-during-training"></a>Eğitim sırasında kaynak koduna erişme

Azure Blob depolama, bir Azure dosya paylaşımından daha yüksek işleme hızına sahiptir ve paralel olarak başlatılan çok sayıda işi ölçeklendirecektir. Bu nedenle, çalıştırmaların kaynak kodu dosyalarını aktarmak için blob depolamayı kullanacak şekilde yapılandırılmasını öneririz.

Aşağıdaki kod örneği, kaynak kodu aktarımları için kullanılacak blob veri deposuna ait çalıştırma yapılandırması ' nda belirtir.

```python 
# workspaceblobstore is the default blob storage
src.run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="notebook-examples"></a>Not defteri örnekleri

[Veri kümesi Not defterleri](https://aka.ms/dataset-tutorial) bu makaledeki kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar

* Tabulardataset ile [makine öğrenimi modellerini otomatik olarak eğitme](how-to-auto-train-remote.md) .

* Dosya veri kümeleri ile [görüntü sınıflandırma modellerini eğitme](https://aka.ms/filedataset-samplenotebook) .

* İşlem [hatları kullanarak veri kümeleriyle eğitme](how-to-create-your-first-pipeline.md).