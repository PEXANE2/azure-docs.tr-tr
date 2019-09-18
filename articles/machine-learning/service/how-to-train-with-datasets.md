---
title: Azureml veri kümeleriyle eğitme
titleSuffix: Azure Machine Learning service
description: Eğitim aşamasında veri kümelerini nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 09/16/2019
ms.openlocfilehash: ceccc515b73bd41c7933889c61617c360c678eb7
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059300"
---
# <a name="train-with-datasets-preview-in-azure-machine-learning"></a>Azure Machine Learning veri kümeleri (Önizleme) ile eğitme

Bu makalede, bağlantı dizeleri veya veri yolları hakkında endişelenmeden uzak deneme eğitiminde [Azure Machine Learning veri kümelerini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) kullanmanın iki yolunu öğrenirsiniz.

- 1\. Seçenek: Veri kümelerini doğrudan eğitim betiğinizdeki geçirin.

- 2\. Seçenek: Eğitim için uzak bir işlem için dosya bağlamak veya indirmek üzere veri kümeleri kullanın.

## <a name="prerequisites"></a>Önkoşullar

Veri kümeleri oluşturup eğitmeniz için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Bir [Azure Machine Learning hizmet çalışma alanı](how-to-manage-workspace.md).

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümesi paketini içerir.

> [!Note]
> Bazı veri kümesi sınıflarının (Önizleme), [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca aşağıdaki dağıtımlarda desteklenir:  Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="option-1-pass-datasets-as-inputs-to-training-scripts"></a>1\. Seçenek: Veri kümelerini eğitim betiklerine giriş olarak geçirme

Azure Machine Learning veri kümeleri, [Scriptrun](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py), [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) ve [Hyperdrive](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py)gibi Azure Machine Learning eğitim ürünleriyle sorunsuz bir tümleştirme sağlar. Bu örnekte, bir [tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) oluşturun ve bunu eğitim için `estimator` nesneniz olarak kullanacaksınız. 

### <a name="create-a-tabulardataset"></a>TabularDataset oluşturma

Aşağıdaki kod, bir Web URL 'sinden kayıtlı olmayan bir TabularDataset oluşturur. Ayrıca, yerel dosyalardan veya veri depolarındaki yollardan veri kümeleri oluşturabilirsiniz. [Veri kümeleri oluşturma](https://aka.ms/azureml/howto/createdatasets)hakkında daha fazla bilgi edinin.

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

### <a name="configure-the-estimator"></a>Tahmin aracı 'ı yapılandırma

Deneme çalıştırmasını göndermek için bir [tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) nesnesi kullanılır. Azure Machine Learning ortak makine öğrenimi çerçeveleri ve genel bir Estimator için önceden yapılandırılmış tahmini 'a sahiptir.

Bu kod, şunu belirten bir genel tahmin aracı `est`nesnesi oluşturur,

* Betikleriniz için bir komut dosyası dizini. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir.
* Eğitim betiği, *train_titanic. Kopyala*.
* Eğitim için giriş veri kümesi, `titanic`.
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

### <a name="access-the-input-dataset-in-your-training-script"></a>Eğitim betiğinizdeki giriş veri kümesine erişin

TabularDataset nesneleri, tanıdık veri hazırlama ve eğitim kitaplıklarıyla çalışabilmeniz için verileri bir Pandas veya Spark veri çerçevesine yükleme yeteneği sağlar. Bu özellikten yararlanmak için, bir TabularDataset 'i eğitim yapılandırmanızda giriş olarak geçirebilir ve sonra betiğinizde alabilirsiniz.

Bunu yapmak için, eğitim betiğinizdeki [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py) nesne aracılığıyla giriş veri kümesine erişin ve [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#to-pandas-dataframe--) yöntemini kullanın. 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic']
# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

## <a name="option-2--mount-files-to-a-remote-compute-target"></a>2\. Seçenek:  Dosyaları uzak bir işlem hedefine bağlama

Veri dosyalarınızı eğitim için işlem hedefinde kullanılabilir hale getirmek istiyorsanız, bu dosyanın başvurduğu dosyaları bağlamak veya indirmek için dosya [veri kümesini](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) kullanın.

Bir dosya sistemi bağladığınızda, bu dosya sistemini bir dizine ekler (bağlama noktası) ve işlem hedefinde kullanılabilir hale getirin. Dosyalar yalnızca işlem sırasında yüklendiği için takma anında gönderilir. Bağlama, Azure Machine Learning Işlem, sanal makineler ve HDInsight dahil olmak üzere Linux tabanlı hesaplar için desteklenir ve önerilir. Linux tabanlı olmayan işlem için yalnızca indirme desteklenir.  

>[!WARNING]
> Veri boyutunuz, işlem hedefinin depolama sınırını aşarsa, indirme başarısız olur.

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

Veri kümesini Estimator 'daki `inputs` parametre aracılığıyla geçirmek yerine, veri kümesini aracılığıyla `script_params` geçirebilir ve bağımsız değişkenler aracılığıyla eğitim betiğinizdeki veri yolunu (takma noktası) alabilirsiniz. Bu şekilde, eğitim betiğinizden Azure Machine Learning SDK 'nın bağımlılığını önleyebilirsiniz.

Bir [sköğrenme](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) tahmin aracı nesnesi, scikit-denemeleri için çalışmayı göndermek üzere kullanılır.

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

Çalışmayı gönderdikten sonra, veri `mnist` kümesi tarafından başvurulan veri dosyaları işlem hedefine bağlanır. Aşağıdaki kod, betiğinizdeki verilerin nasıl alınacağını gösterir.

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

[Örnek Not defterleri](https://aka.ms/dataset-tutorial) , Scriptrun ve [hyperddrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) nesneleriyle veri kümeleri kullanarak gibi bu makaledeki kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar

* Tabulardataset ile [makine öğrenimi modellerini otomatik olarak eğitme](how-to-auto-train-remote.md) .

* Dosya veri kümeleri ile [görüntü sınıflandırma modellerini eğitme](https://aka.ms/filedataset-samplenotebook) .
