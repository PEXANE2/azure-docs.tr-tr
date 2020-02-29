---
title: Azureml veri kümeleriyle eğitme
titleSuffix: Azure Machine Learning
description: Eğitim aşamasında veri kümelerini nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 09/25/2019
ms.openlocfilehash: ece8ee77f57dc3252c70c3f8b49dcee72967dc9e
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198074"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleriyle eğitme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bağlantı dizeleri veya veri yolları hakkında endişelenmeden uzak deneme eğitiminde [Azure Machine Learning veri kümelerini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) kullanmanın iki yolunu öğrenirsiniz.

- Seçenek 1: yapılandırılmış veriler varsa, bir TabularDataset oluşturun ve doğrudan eğitim betiğinizdeki kullanın.

- Seçenek 2: yapılandırılmamış verileriniz varsa, bir dosya veri kümesi oluşturun ve eğitim için uzak bir işlem ile dosyaları bağlayın veya indirin.

Azure Machine Learning veri kümeleri, [Scriptrun](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py), [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py), [hiper sürücü](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py) ve [Azure Machine Learning işlem hatları](how-to-create-your-first-pipeline.md)gibi Azure Machine Learning eğitim ürünleriyle sorunsuz bir tümleştirme sağlar.

## <a name="prerequisites"></a>Önkoşullar

Veri kümeleri oluşturup eğitmeniz için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.

> [!Note]
> Bazı veri kümesi sınıflarının [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca şu dağıtımlarda desteklenir: Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="option-1-use-datasets-directly-in-training-scripts"></a>Seçenek 1: doğrudan eğitim betiklerde veri kümelerini kullanma

Bu örnekte, bir [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) oluşturun ve bunu eğitim için `estimator` nesneniz için doğrudan bir giriş olarak kullanacaksınız. 

### <a name="create-a-tabulardataset"></a>TabularDataset oluşturma

Aşağıdaki kod, bir Web URL 'sinden kayıtlı olmayan bir TabularDataset oluşturur. Ayrıca, yerel dosyalardan veya veri depolarındaki yollardan veri kümeleri oluşturabilirsiniz. [Veri kümeleri oluşturma](https://aka.ms/azureml/howto/createdatasets)hakkında daha fazla bilgi edinin.

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

### <a name="access-the-input-dataset-in-your-training-script"></a>Eğitim betiğinizdeki giriş veri kümesine erişin

TabularDataset nesneleri, tanıdık veri hazırlama ve eğitim kitaplıklarıyla çalışabilmeniz için verileri bir Pandas veya Spark veri çerçevesine yükleme yeteneği sağlar. Bu özellikten yararlanmak için, bir TabularDataset 'i eğitim yapılandırmanızda giriş olarak geçirebilir ve sonra betiğinizde alabilirsiniz.

Bunu yapmak için, eğitim betiğinizdeki [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py) nesnesi aracılığıyla giriş veri kümesine erişin ve [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) metodunu kullanın. 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic_ds']
# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

### <a name="configure-the-estimator"></a>Tahmin aracı 'ı yapılandırma

Deneme çalıştırmasını göndermek için bir [tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) nesnesi kullanılır. Azure Machine Learning ortak makine öğrenimi çerçeveleri ve genel bir Estimator için önceden yapılandırılmış tahmini 'a sahiptir.

Bu kod, şunu belirten `est`bir genel tahmin aracı nesnesi oluşturur.

* Betikleriniz için bir komut dosyası dizini. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir.
* Eğitim betiği, *train_titanic. Kopyala*.
* Eğitim için giriş veri kümesi, `titanic`. `as_named_input()`, giriş veri kümesinin eğitim betiğinizdeki atanan ad tarafından başvurulabilmesi için gereklidir. 
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


## <a name="option-2--mount-files-to-a-remote-compute-target"></a>Seçenek 2: dosyaları uzak bir işlem hedefine bağlama

Veri dosyalarınızı eğitim için işlem hedefinde kullanılabilir hale getirmek istiyorsanız, bu dosyanın başvurduğu dosyaları bağlamak veya indirmek için dosya [veri kümesini](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) kullanın.

### <a name="mount-vs-download"></a>Bağlama ile Indir

Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı tarafından oluşturulan veri kümelerinde herhangi bir biçimdeki dosyaları bağlama veya indirme işlemi desteklenir. 

Bir veri kümesini bağladığınızda, veri kümesinin başvurduğu dosyaları bir dizine (bağlama noktası) ekler ve işlem hedefinde kullanılabilir hale getirin. Bağlama, Azure Machine Learning Işlem, sanal makineler ve HDInsight dahil olmak üzere Linux tabanlı hesaplar için desteklenir. Bir veri kümesini indirdiğinizde, veri kümesi tarafından başvurulan tüm dosyalar işlem hedefine indirilir. Tüm işlem türleri için indirme desteklenir. 

Betiğinizin veri kümesi tarafından başvurulan tüm dosyaları işliyorsa ve işlem diskiniz tam veri kümesine uyuyorsa, depolama hizmetlerinden veri akışı yükünü ortadan kaldırmak için indirme önerilir. Veri boyutunuz işlem diski boyutunu aşarsa, indirme mümkün değildir. Bu senaryo için, işleme sırasında yalnızca komut dosyası tarafından kullanılan veri dosyaları yüklendiğinden, bağlama yapmanız önerilir.

Aşağıdaki kod, `mounted_path` konumundaki geçici dizine `dataset` takar

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

### <a name="create-a-filedataset"></a>Dosya veri kümesi oluşturma

Aşağıdaki örnek, Web URL 'lerinden kaydedilmemiş bir dosya veri kümesi oluşturur. Diğer kaynaklardan [veri kümeleri oluşturma](https://aka.ms/azureml/howto/createdatasets) hakkında daha fazla bilgi edinin.

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

Veri kümesini Estimator 'daki `inputs` parametresi aracılığıyla geçirmenin yanı sıra, veri kümesini `script_params` aracılığıyla geçirebilir ve bağımsız değişkenler aracılığıyla eğitim betiğinizdeki veri yolunu (takma noktası) alabilirsiniz. Bu şekilde, eğitim betiğinizi azureml-SDK 'dan bağımsız tutabilirsiniz. Diğer bir deyişle, yerel hata ayıklama ve herhangi bir bulut platformunda uzaktan eğitim için aynı eğitim betiğini kullanacaksınız.

Bir [sköğrenme](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) tahmin aracı nesnesi, scikit-denemeleri için çalışmayı göndermek üzere kullanılır. [Sköğren tahmin aracı](how-to-train-scikit-learn.md)ile eğitim hakkında daha fazla bilgi edinin.

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

Çalıştırmayı gönderdikten sonra, `mnist` veri kümesi tarafından başvurulan veri dosyaları işlem hedefine bağlanır. Aşağıdaki kod, betiğinizdeki verilerin nasıl alınacağını gösterir.

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

## <a name="notebook-examples"></a>Not defteri örnekleri

[Veri kümesi Not defterleri](https://aka.ms/dataset-tutorial) bu makaledeki kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar

* Tabulardataset ile [makine öğrenimi modellerini otomatik eğitme](how-to-auto-train-remote.md)

* Filedataset 'ler ile [görüntü sınıflandırma modellerini eğitme](https://aka.ms/filedataset-samplenotebook)

* [İşlem hatları kullanarak veri kümeleriyle eğitme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/pipeline-with-datasets/pipeline-for-image-classification.ipynb)
