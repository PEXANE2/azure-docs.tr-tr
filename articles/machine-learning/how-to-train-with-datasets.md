---
title: Azureml veri kümeleriyle eğitme
titleSuffix: Azure Machine Learning
description: Eğitim aşamasında veri kümelerini nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 07/31/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 1db62b77f3b9b1bcfc524a68b52c4aef5c16d851
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89648187"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleriyle eğitme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, eğitim denemeleri [Azure Machine Learning veri kümeleriyle](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py&preserve-view=true) nasıl çalışacağınızı öğreneceksiniz.  Bağlantı dizeleri veya veri yolları hakkında endişelenmeden, yerel veya uzaktan işlem Hedefinizdeki veri kümelerini kullanabilirsiniz.

Azure Machine Learning veri kümeleri, [Scriptrun](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py&preserve-view=true), [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py&preserve-view=true), [hiper sürücü](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py&preserve-view=true) ve [Azure Machine Learning işlem hatları](how-to-create-your-first-pipeline.md)gibi Azure Machine Learning eğitim ürünleriyle sorunsuz bir tümleştirme sağlar.

## <a name="prerequisites"></a>Ön koşullar

Veri kümeleri oluşturup eğitmeniz için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true), azureml veri kümesi paketini içerir.

> [!Note]
> Bazı veri kümesi sınıflarının [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py&preserve-view=true) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca şu dağıtımlarda desteklenir: Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="access-and-explore-input-datasets"></a>Giriş veri kümelerine erişin ve bunları gezin

Var olan bir TabularDataset 'e, çalışma alanınızdaki bir deneyinin eğitim betiğine erişebilir ve bu veri kümesini, yerel ortamınızda daha fazla araştırma için bir Pandas veri çerçevesine yükleyebilirsiniz.

Aşağıdaki kod, [`get_context()`]() [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py&preserve-view=true) eğitim betiğindeki var olan TabularDataset girişine erişmek için sınıfındaki yöntemini kullanır `titanic` . Daha sonra [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) eğitim öncesinde daha fazla veri araştırması ve hazırlığı için bu veri kümesini bir Pandas dataframe 'e yüklemek için yöntemini kullanır.

> [!Note]
> Özgün veri kaynağınız NaN, boş dizeler veya boş değerler içeriyorsa, to_pandas_dataframe () kullandığınızda, bu değerler *null* değer olarak değiştirilmiştir. 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic']

# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

Hazırlanan verileri bellek Pandas dataframe 'ten yeni bir veri kümesine yüklemeniz gerekiyorsa, verileri bir Parquet gibi yerel bir dosyaya yazın ve bu dosyadan yeni bir veri kümesi oluşturun. Ayrıca, yerel dosyalardan veya veri depolarındaki yollardan veri kümeleri oluşturabilirsiniz. [Veri kümeleri oluşturma](how-to-create-register-datasets.md)hakkında daha fazla bilgi edinin.

## <a name="use-datasets-directly-in-training-scripts"></a>Doğrudan eğitim betiklerde veri kümelerini kullanma

Henüz bir veri kümesi olarak kayıtlı olmayan yapılandırılmış veriler varsa, bir TabularDataset oluşturun ve bunu yerel veya uzaktan denemenize yönelik eğitim betiğinizdeki doğrudan kullanın.

Bu örnekte, kayıtlı olmayan bir [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py&preserve-view=true) oluşturun ve bunu `estimator` eğitim için nesnenizin doğrudan bir giriş olarak kullanacaksınız. Bu TabularDataset 'i çalışma alanınızdaki diğer denemeleri birlikte yeniden kullanmak istiyorsanız, bkz. [veri kümelerini çalışma alanınıza kaydetme](how-to-create-register-datasets.md#register-datasets).

### <a name="create-a-tabulardataset"></a>TabularDataset oluşturma

Aşağıdaki kod, bir Web URL 'sinden kayıtlı olmayan bir TabularDataset oluşturur.  

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

TabularDataset nesneleri, Not defterinize çıkmadan tanıdık veri hazırlama ve eğitim kitaplıklarıyla çalışabilmeniz için TabularDataset 'teki verileri bir Pandas veya Spark veri çerçevesine yükleme olanağı sağlar. Bu özellikten yararlanmak için bkz. [erişim ve giriş veri kümelerini araştırma](#access-and-explore-input-datasets).

### <a name="configure-the-estimator"></a>Tahmin aracı 'ı yapılandırma

Deneme çalıştırmasını göndermek için bir [tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py&preserve-view=true) nesnesi kullanılır. Azure Machine Learning ortak makine öğrenimi çerçeveleri ve genel bir Estimator için önceden yapılandırılmış tahmini 'a sahiptir.

Bu kod, şunu belirten bir genel tahmin aracı nesnesi oluşturur, `est`

* Betikleriniz için bir komut dosyası dizini. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir.
* Eğitim betiği, *train_titanic. Kopyala*.
* Eğitim için giriş veri kümesi, `titanic_ds` . `as_named_input()` , giriş veri kümesinin eğitim betiğinizdeki atanan ad tarafından başvurulabilmesi için gereklidir `titanic` . 
* Deneme için işlem hedefi.
* Deneme için ortam tanımı.

```Python
est = Estimator(source_directory=script_folder,
                entry_script='train_titanic.py',
                # pass dataset object as an input with name 'titanic'
                inputs=[titanic_ds.as_named_input('titanic')],
                compute_target=compute_target,
                environment_definition= conda_env)

# Submit the estimator as part of your experiment run
experiment_run = experiment.submit(est)
experiment_run.wait_for_completion(show_output=True)
```

## <a name="mount-files-to-remote-compute-targets"></a>Dosyaları uzaktan işlem hedeflerine bağlama

Yapılandırılmamış verileriniz varsa, bir [dosya veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py&preserve-view=true) oluşturun ve veri dosyalarınızı, eğitim için uzaktan işlem hedefi için kullanılabilir hale getirmek üzere bağlayın veya indirin. Uzaktan Eğitim denemeleri için [bağlama ve indirme](#mount-vs-download) bilgilerini ne zaman kullanacağınızı öğrenin. 

Aşağıdaki örnek bir dosya veri kümesi oluşturur ve veri kümesini, eğitim için tahmin aracı 'da bir bağımsız değişken olarak geçirerek işlem hedefine bağlar. 

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

### <a name="configure-the-estimator"></a>Tahmin aracı 'ı yapılandırma

Bağlama sırasında veri kümesini bir bağımsız değişken olarak geçirmeyi öneririz. Veri kümesini `inputs` Estimator 'daki parametre aracılığıyla iletmenin yanı sıra, veri kümesini ile de geçirebilir `script_params` ve bağımsız değişkenler aracılığıyla eğitim betiğinizdeki veri yolunu (takma noktası) alabilirsiniz. Bu şekilde, yerel hata ayıklama ve herhangi bir bulut platformunda uzaktan eğitim için aynı eğitim betiğini kullanacaksınız.

Bir [sköğrenme](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py&preserve-view=true) tahmin aracı nesnesi, scikit-denemeleri için çalışmayı göndermek üzere kullanılır. Çalışmayı gönderdikten sonra, veri kümesi tarafından başvurulan veri dosyaları `mnist` işlem hedefine bağlanır. [Sköğren tahmin aracı](how-to-train-scikit-learn.md)ile eğitim hakkında daha fazla bilgi edinin.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    # mount the dataset on the remote compute and pass the mounted path as an argument to the training script
    '--data-folder': mnist_ds.as_named_input('mnist').as_mount(),
    '--regularization': 0.5
}

est = SKLearn(source_directory=script_folder,
              script_params=script_params,
              compute_target=compute_target,
              environment_definition=env,
              entry_script='train_mnist.py')

# Run the experiment
run = experiment.submit(est)
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

# retrieve the 2 arguments configured through script_params in estimator
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

## <a name="access-datasets-in-your-script"></a>Betiğinizdeki veri kümelerine erişin

Kayıtlı veri kümelerine, Azure Machine Learning işlem gibi işlem kümelerinde hem yerel olarak hem de uzaktan erişilebilir. Kayıtlı veri kümenize denemeleri üzerinden erişmek için aşağıdaki kodu kullanarak çalışma alanınıza ve kayıtlı veri kümesine ad ile erişin. Varsayılan olarak, [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#&preserve-view=trueget-by-name-workspace--name--version--latest--) sınıfındaki yöntemi, `Dataset` çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür.

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
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="notebook-examples"></a>Not defteri örnekleri

[Veri kümesi Not defterleri](https://aka.ms/dataset-tutorial) bu makaledeki kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar

* Tabulardataset ile [makine öğrenimi modellerini otomatik olarak eğitme](how-to-auto-train-remote.md) .

* Dosya veri kümeleri ile [görüntü sınıflandırma modellerini eğitme](https://aka.ms/filedataset-samplenotebook) .

* İşlem [hatları kullanarak veri kümeleriyle eğitme](how-to-create-your-first-pipeline.md).
