---
title: Keras ile derin öğrenme sinir ağını eğitme
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetini kullanarak TensorFlow 'da çalışan keras derin sinir ağ sınıflandırma modelini eğitme ve kaydetme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 08/01/2019
ms.custom: seodec18
ms.openlocfilehash: bfe7f975539c76c1d369d111729820f4d0ada470
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775075"
---
# <a name="train-and-register-a-keras-classification-model-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile keras sınıflandırma modelini eğitme ve kaydetme

Bu makalede, Azure Machine Learning hizmeti kullanılarak TensorFlow 'da oluşturulan keras sınıflandırma modelinin nasıl eğildiği ve kaydedileceği gösterilmektedir. Popüler [veri kümesini](http://yann.lecun.com/exdb/mnist/) kullanarak, [TensorFlow](https://www.tensorflow.org/overview)üzerinde çalışan [keras Python kitaplığı](https://keras.io) kullanılarak oluşturulan derin bir sinir ağı (DNN) kullanarak el yazısı rakamlarını sınıflandırın.

Keras, geliştirmeyi basitleştirmek için diğer popüler DNN çerçevelerinin üstünü çalıştırabilen üst düzey bir sinir ağ API 'sidir. Azure Machine Learning hizmeti sayesinde, esnek bulut işlem kaynaklarını kullanarak eğitim işlerini hızla ölçeklendirebilirsiniz. Ayrıca eğitim çalıştırmalarını, sürüm modellerinizi, modellerinizi dağıtmayı ve daha fazlasını izleyebilirsiniz.

Sıfırdan bir keras modeli geliştirdiğinizi veya var olan bir modeli buluta verdiğinize göre Azure Machine Learning hizmet, üretime hazır modeller oluşturmanıza yardımcı olabilir.

Machine Learning ve derin öğrenme arasındaki farklar hakkında bilgi edinmek için [kavramsal makaleye](concept-deep-learning-vs-machine-learning.md) bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu şu ortamlardan birinde çalıştırın:

 - Azure Machine Learning Not defteri VM-indirme veya yükleme gerekli değil

     - [Öğreticiyi doldurun: SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu oluşturmak için ortamı ve çalışma alanını](tutorial-1st-experiment-sdk-setup.md) ayarlayın.
    - Not defteri sunucusundaki örnekler klasöründe, bu dizine giderek tamamlanmış ve genişletilmiş bir not defteri bulun: **kullanımı nasıl kullanılır-azureml > eğitimi--derin öğrenme > tren-hyperparameter-ayarla-dağıt-i-keras** klasörü.

 - Kendi Jupyter Notebook sunucunuz

     - [Python için Azure Machine Learning SDK 'sını yükler](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - [Örnek betik dosyalarını indirin](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras) `mnist-keras.py` ve`utils.py`

    Ayrıca, bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) GitHub örnekleri sayfasından bulabilirsiniz. Not defteri, akıllı hiper parametre ayarlamayı, model dağıtımını ve Not defteri pencere öğelerini kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölüm, gerekli Python paketlerini yükleyerek, bir çalışma alanı başlatarak, bir deneme oluşturarak ve eğitim verilerini ve eğitim betikleri karşıya yükleyerek Eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıklarını içeri aktarın.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning hizmet çalışma alanı](concept-workspace.md) , hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Python SDK 'sında bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapıtlarına erişebilirsiniz.

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Eğitim betiklerinizi tutmak için bir deneme ve bir klasör oluşturun. Bu örnekte, "keras-mnist" adlı bir deneme oluşturun.

```Python
script_folder = './keras-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='keras-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Veri kümesini ve betikleri karşıya yükle

Veri [deposu](how-to-access-data.md) , verileri bir işlem hedefine bağlayarak veya kopyalayarak verilerin saklanabileceği ve erişebileceği bir yerdir. Her çalışma alanı varsayılan bir veri deposu sağlar. Eğitim sırasında kolayca erişilebilmesi için veri ve eğitim betikleri veri deposuna yükleyin.

1. MNIST veri kümesini yerel olarak indirin.

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. MNIST veri kümesini varsayılan veri deposuna yükleyin.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Keras eğitim betiği `keras_mnist.py`, ve yardımcı `utils.py`dosyasını yükleyin.

    ```Python
    shutil.copy('./keras_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>İşlem hedefi oluşturmak

Açık iş akışı işinizin üzerinde çalışacağı bir işlem hedefi oluşturun. Bu örnekte, GPU özellikli Azure Machine Learning işlem kümesi oluşturun.

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

## <a name="create-a-tensorflow-estimator-and-import-keras"></a>TensorFlow tahmin aracı oluşturma ve keras içeri aktarma

[TensorFlow tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) , işlem hedefinde TensorFlow eğitim işlerinin başlatılması için basit bir yol sağlar. Keras, TensorFlow üzerinde çalıştığından, TensorFlow tahmin Aracı ' ı kullanabilir ve `pip_packages` bağımsız değişkenini kullanarak keras kitaplığını içeri aktarabilirsiniz.

TensorFlow tahmin aracı, herhangi bir çerçeveyi desteklemek için [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) kullanılabilen genel sınıf aracılığıyla uygulanır. Ek olarak, DNN `script_params` hyperparameter ayarlarını içeren bir sözlük oluşturun. Genel tahmin aracı kullanan eğitim modelleri hakkında daha fazla bilgi için bkz. [tahmin aracı kullanarak Azure Machine Learning modelleri eğitme](how-to-train-ml-models.md)

```Python
script_params = {
    '--data-folder': ds.path('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.001
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='keras_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 pip_packages=['keras', 'matplotlib'],
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Bir çalıştırma gönder

[Run nesnesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) , iş çalışırken ve tamamlandıktan sonra çalışma geçmişi için arabirim sağlar.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütüldüğü için aşağıdaki aşamalardan geçer:

- **Hazırlanıyor**: Bir Docker görüntüsü, TensorFlow Estimator öğesine göre oluşturulur. Görüntü, çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonra çalışacak şekilde önbelleğe alınır. Günlükler, çalıştırma geçmişine de kaydedilir ve ilerlemeyi izlemek için görüntülenebilir.

- **Ölçeklendirme**: Batch AI kümesi, çalışmayı yürütmek için daha fazla düğüm, şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeyi dener.

- **Çalıştırma**: Betik klasöründeki tüm betikler işlem hedefine yüklenir, veri depoları bağlanır veya kopyalanır ve entry_script yürütülür. Stdout ve./logs klasörü çıkışları, çalıştırma geçmişine akışla kaydedilir ve çalıştırmayı izlemek için kullanılabilir.

- **Işlem sonrası**: Çalıştırmanın./çıktılar klasörü, çalışma geçmişine kopyalanır.

## <a name="register-the-model"></a>Modeli kaydedin

DNN modelini eğittikten sonra, çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimi ve dağıtımını](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümlerini oluşturmanıza imkan tanır.

```Python
model = run.register_model(model_name='keras-dnn-mnist', model_path='outputs/model')
```

Ayrıca modelin yerel bir kopyasını da indirebilirsiniz. Bu, ek model doğrulama işini yerel olarak gerçekleştirmek için yararlı olabilir. Eğitim betiğinde `mnist-keras.py`, bir TensorFlow koruyucu nesnesi modeli yerel bir klasöre (yerel olarak işlem hedefine) devam ettirir. Veri deposundan bir kopya indirmek için Run nesnesini kullanabilirsiniz.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Machine Learning hizmetinde bir keras modeli eğitildiniz ve kaydettiniz. Modeli dağıtmayı öğrenmek için model dağıtım makalemize devam edin.

> [!div class="nextstepaction"]
> [Modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)
