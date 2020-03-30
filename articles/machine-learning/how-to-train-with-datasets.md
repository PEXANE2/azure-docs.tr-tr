---
title: Azureml-datasets ile eğitim
titleSuffix: Azure Machine Learning
description: Eğitimde veri kümelerini nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 03/09/2020
ms.openlocfilehash: 401383f2d483836bf725051810d78167869f7b22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283504"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Azure Machine Learning'de veri kümeleriyle eğitim
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bağlantı dizeleri veya veri yolları hakkında endişelenmeden uzak bir deneme eğitiminde [Azure Machine Learning veri kümelerini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) kullanmanın iki yolunu öğrenirsiniz.

- Seçenek 1: Yapılandırılmış verileriniz varsa, bir TabularDataset oluşturun ve doğrudan eğitim komut dosyanızda kullanın.

- Seçenek 2: Yapılandırılmamış verileriniz varsa, bir FileDataset oluşturun ve eğitim için uzak bir bilgisayara dosya monte edin veya indirin.

Azure Machine Learning veri kümeleri, [ScriptRun,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py) [Estimator,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) [HyperDrive](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py) ve [Azure Machine Learning boru hatları](how-to-create-your-first-pipeline.md)gibi Azure Machine Learning eğitim ürünleriyle sorunsuz bir entegrasyon sağlar.

## <a name="prerequisites"></a>Ön koşullar

Veri kümeleri oluşturmak ve eğitmek için şunları yapmanız gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure Makine Öğrenimi çalışma alanı.](how-to-manage-workspace.md)

* Azureml-datasets paketini içeren [Python için Azure Machine Learning SDK yüklendi.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)

> [!Note]
> Bazı Dataset sınıflarının [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paketine bağımlılıkları vardır. Linux kullanıcıları için bu sınıflar yalnızca aşağıdaki dağıtımlarda desteklenir: Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="option-1-use-datasets-directly-in-training-scripts"></a>Seçenek 1: Veri kümelerini doğrudan eğitim komut dosyalarında kullanma

Bu örnekte, bir [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) oluşturur ve bunu eğitim `estimator` için nesnenize doğrudan giriş olarak kullanırsınız. 

### <a name="create-a-tabulardataset"></a>Bir TabularDataset oluşturma

Aşağıdaki kod, bir web url'sinden kayıtsız bir TabularDataset oluşturur. Ayrıca, veri depolarında yerel dosyalardan veya yollardan veri kümeleri oluşturabilirsiniz. [Veri kümelerini oluşturma](https://aka.ms/azureml/howto/createdatasets)hakkında daha fazla bilgi edinin.

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

### <a name="access-the-input-dataset-in-your-training-script"></a>Eğitim komut dosyanızdaki giriş veri kümesine erişin

TabularDataset nesneleri, tanıdık veri hazırlama ve eğitim kitaplıklarıyla çalışabilmeniz için verileri pandalara yükleme veya DataFrame'i kıvılcımlama olanağı sağlar. Bu özellikten yararlanmak için, eğitim yapılandırmanızdaki giriş olarak bir TabularDataset'i geçirebilir ve komut dosyanızda geri alabilirsiniz.

Bunu yapmak için, giriş veri kümesine eğitim komut [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py) dosyanızdaki [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) nesne üzerinden erişin ve yöntemi kullanın. 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic']
# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

### <a name="configure-the-estimator"></a>Tahminciyi yapılandırma

Deneme çalışmasını göndermek için bir [tahminnesnesi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) kullanılır. Azure Machine Learning, genel makine öğrenimi çerçeveleri için önceden yapılandırılmış tahmincilerin yanı sıra genel bir tahminciye de sahiptir.

Bu kod, genel bir tahmin `est`nesnesi oluşturur, bu belirtir

* Senaryolarınız için bir senaryo dizini. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir.
* Eğitim script, *train_titanic.py*.
* Eğitim için giriş veri `titanic`kümesi, . `as_named_input()`giriş veri kümesinin eğitim komut dosyasınızda atanan adla başvurulabilmesi için gereklidir. 
* Deneyin işlem hedefi.
* Denemenin çevre tanımı.

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


## <a name="option-2--mount-files-to-a-remote-compute-target"></a>Seçenek 2: Dosyaları uzak bir işlem hedefine monte etme

Veri dosyalarınızı eğitim için bilgi işlem hedefinde kullanılabilir hale getirmek istiyorsanız, yönlendirilen dosyaları monte etmek veya indirmek için [FileDataset'i](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) kullanın.

### <a name="mount-vs-download"></a>Mount vs. İndir

Azure Blob depolama, Azure Dosyaları, Azure Veri Gölü Depolama Gen1, Azure Veri Gölü Depolama Gen2, Azure SQL Veritabanı ve PostgreSQL için Azure Veritabanı'ndan oluşturulan veri kümeleri için herhangi bir biçimde dosya montajı veya indirilmesi desteklenir. 

Bir veri kümesi bağladığınızda, veri kümesi tarafından başvurulan dosyaları bir dizin (montaj noktası) olarak ekler ve bilgi işlem hedefinde kullanılabilir hale getirin. Montaj, Azure Machine Learning Compute, sanal makineler ve HDInsight dahil olmak üzere Linux tabanlı hesaplamalar için desteklenir. Bir veri kümesi indirdiğinizde, veri kümesi tarafından başvurulan tüm dosyalar bilgi işlem hedefine indirilir. İndirme tüm bilgi işlem türleri için desteklenir. 

Komut dosyanız veri kümesi tarafından başvurulan tüm dosyaları işlerse ve bilgi işlem diskiniz tam veri kümenize sığabilirse, depolama hizmetlerinden veri akışının yüklerini önlemek için karşıdan yükleme önerilir. Veri boyutunuz bilgi işlem disk boyutunu aşıyorsa, karşıdan yükleme mümkün değildir. Bu senaryo için, yalnızca komut dosyanız tarafından kullanılan veri dosyaları işleme sırasında yüklendiğinden montaj yapmanızı öneririz.

Aşağıdaki kod `dataset` geçici dizine`mounted_path`

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

### <a name="create-a-filedataset"></a>FileDataset oluşturma

Aşağıdaki örnek, web url'lerinden kayıtsız bir FileDataset oluşturur. Diğer kaynaklardan [veri kümeleri oluşturma](https://aka.ms/azureml/howto/createdatasets) hakkında daha fazla bilgi edinin.

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

### <a name="configure-the-estimator"></a>Tahminciyi yapılandırma

Veri kümesini tahmincideki `inputs` parametreden geçirmenin yanı sıra, veri `script_params` kümesini geçirebilir ve bağımsız değişkenler aracılığıyla eğitim komut dosyasınızdaki veri yolunu (montaj noktası) da alabilirsiniz. Bu şekilde, eğitim komut dosyanızı azureml-sdk'dan bağımsız tutabilirsiniz. Başka bir deyişle, herhangi bir bulut platformunda yerel hata ayıklama ve uzaktan eğitim için aynı eğitim komut dosyasını kullanabilirsiniz.

Bir [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) tahmincisi nesne, scikit-learn denemeleri için çalıştırmayı göndermek için kullanılır. [SKlearn tahmincisi](how-to-train-scikit-learn.md)ile eğitim hakkında daha fazla bilgi edinin.

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

### <a name="retrieve-the-data-in-your-training-script"></a>Eğitim komut dosyanızdaki verileri alma

Çalıştırmayı gönderdikten sonra, veri `mnist` kümesi tarafından yönlendirilen veri dosyaları bilgi işlem hedefine monte edilir. Aşağıdaki kod, komut dosyanızdaki verileri nasıl alısüreceğini gösterir.

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

[Veri kümesi not defterleri](https://aka.ms/dataset-tutorial) bu makaledeki kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar

* TabularDatasets ile [otomatik tren makine öğrenme modelleri](how-to-auto-train-remote.md)

* FileDatasets ile [görüntü sınıflandırma modellerini eğitin](https://aka.ms/filedataset-samplenotebook)

* [Veri ayarlarını kullanarak tren](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/pipeline-with-datasets/pipeline-for-image-classification.ipynb)
