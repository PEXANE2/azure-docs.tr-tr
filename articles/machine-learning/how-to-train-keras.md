---
title: Tren derin öğrenme Keras modelleri
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'i kullanarak TensorFlow'da çalışan Keras derin sinir ağı sınıflandırma modelini nasıl eğitip kaydedebilirsiniz öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 08/01/2019
ms.custom: seodec18
ms.openlocfilehash: ba7976d602412037578d0a324916718b2d515aac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269971"
---
# <a name="train-and-register-a-keras-classification-model-with-azure-machine-learning"></a>Azure Machine Learning ile bir Keras sınıflandırma modeli eğitin ve kaydedin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning kullanarak TensorFlow'da oluşturulmuş bir Keras sınıflandırma modelini nasıl eğitip kaydedebilirsiniz. [TensorFlow'un](https://www.tensorflow.org/overview)üstünde çalışan [Keras Python kitaplığı](https://keras.io) kullanılarak oluşturulmuş derin bir sinir ağı (DNN) kullanarak el yazısı yla yazılmış rakamları sınıflandırmak için popüler [MNIST veri kümesini](http://yann.lecun.com/exdb/mnist/) kullanır.

Keras, geliştirmeyi kolaylaştırmak için diğer popüler DNN çerçevelerinin en üstünde çalışan üst düzey bir sinir ağı API'sidir. Azure Machine Learning ile, elastik bulut bilgi işlem kaynaklarını kullanarak eğitim işlerini hızla ölçeklendirebilirsiniz. Ayrıca eğitim çalıştırmalarınızı, sürüm modellerinizi, dağıtma modellerinizi ve çok daha fazlasını izleyebilirsiniz.

İster sıfırdan bir Keras modeli geliştiriyor olun ister buluta mevcut bir model getiriyor olun, Azure Machine Learning üretime hazır modeller oluşturmanıza yardımcı olabilir.

Makine öğrenimi ve derin öğrenme arasındaki farklar hakkında bilgi için [kavramsal makaleye](concept-deep-learning-vs-machine-learning.md) bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu aşağıdaki ortamlardan herhangi birinde çalıştırın:

- Azure Machine Learning bilgi işlem örneği - indirme veya yükleme ye gerek yok

     - [Öğreticiyi tamamlayın:](tutorial-1st-experiment-sdk-setup.md) SDK ve örnek depoyla önceden yüklenmiş özel bir dizüstü bilgisayar sunucusu oluşturmak için kurulum ortamı ve çalışma alanı.
    - Not defteri sunucusundaki örnekler klasöründe, bu dizine giderek tamamlanmış ve genişletilmiş bir not defteri bulun: **nasıl kullanılır-azureml > eğitimi-ile derin öğrenme >-hiperparametre-tune-deploy-with-keras** klasörü.

 - Kendi Jupyter Notebook sunucunuz

    - [Azure Machine Learning SDK'yı yükleyin.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Çalışma alanı yapılandırma dosyası oluşturun.](how-to-configure-environment.md#workspace)
    - [Örnek komut dosyası dosyalarını](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras) `mnist-keras.py` indirin ve`utils.py`

    Ayrıca GitHub örnekleri sayfasında bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) de bulabilirsiniz. Not defteri, akıllı hiperparametre atonlarını, model dağıtımını ve dizüstü bilgisayar widget'larını kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölümde, gerekli python paketlerini yükleyerek, bir çalışma alanını başlatma, deneme oluşturma ve eğitim verilerini ve eğitim komut dosyalarını yükleyerek eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıklarını içe aktarın.

```Python
import os
import azureml
from azureml.core import Experiment
from azureml.core import Workspace, Run
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning çalışma alanı,](concept-workspace.md) hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm eserlerle çalışmak için merkezi bir yer sağlar. Python SDK'da, bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapılarına erişebilirsiniz.

[Önkoşullar bölümünde](#prerequisites)oluşturulan `config.json` dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Çalışma alanınızda "keras-mnist" adlı bir deneme oluşturun.

```Python
exp = Experiment(workspace=ws, name='keras-mnist')
```

<a name="data-upload"></a>
### <a name="create-a-file-dataset"></a>Dosya veri kümesi oluşturma

Nesne, `FileDataset` çalışma alanı veri deponuzda veya genel url'lerinizde bir veya birden çok dosyaya başvurur. Dosyalar herhangi bir biçimde olabilir ve sınıf, dosyaları bilgisayarınıza indirme veya yükleme olanağı sağlar. Bir `FileDataset`, veri kaynağı konumuna bir başvuru oluşturursunuz. Veri kümesine herhangi bir dönüşüm uyguladıysanız, bunlar veri kümesinde de depolanır. Veriler varolan konumunda kalır, bu nedenle ek depolama maliyeti tahakkuk etmez. Daha [how-to](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets) fazla bilgi için `Dataset` paketteki nasıl yapılacağını kılavuzuna bakın.

```python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
dataset = Dataset.File.from_files(path=web_paths)
```

Veri `register()` kümesini çalışma alanınıza kaydetmek için yöntemi kullanın, böylece bunlar başkalarıyla paylaşılabilir, çeşitli denemeler arasında yeniden kullanılabilir ve eğitim komut dosyanızda adıyla anılabilen ler.

```python
dataset = dataset.register(workspace=ws,
                           name='mnist dataset',
                           description='training and test dataset',
                           create_new_version=True)
```

## <a name="create-a-compute-target"></a>İşlem hedefi oluşturma

TensorFlow işinizin çalışması için bir bilgi işlem hedefi oluşturun. Bu örnekte, GPU etkin leştirilmiş bir Azure Machine Learning bilgi işlem kümesi oluşturun.

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

Bilgi işlem hedefleri hakkında daha fazla bilgi için, [bilgi işlem hedef](concept-compute-target.md) makalesinin ne olduğunu görün.

## <a name="create-a-tensorflow-estimator-and-import-keras"></a>Bir TensorFlow tahmincisi oluşturun ve Keras'ı içe aktarın

[TensorFlow tahmincisi,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) bilgi işlem hedefinde TensorFlow eğitim işlerini başlatmanın basit bir yolunu sağlar. Keras TensorFlow'un üstünde çalıştığından, TensorFlow tahminini kullanabilir ve bağımsız `pip_packages` değişkeni kullanarak Keras kitaplığını içe aktarabilirsiniz.

Önce `Dataset` sınıfı kullanarak çalışma alanı veri deposundan veri alın.

```python
dataset = Dataset.get_by_name(ws, 'mnist dataset')

# list the files referenced by mnist dataset
dataset.to_path()
```

TensorFlow tahmincisi, herhangi bir [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) çerçeveyi desteklemek için kullanılabilecek genel sınıf aracılığıyla uygulanır. Ayrıca, DNN `script_params` hiperparametre ayarlarını içeren bir sözlük oluşturun. Genel tahminciyi kullanarak eğitim modelleri hakkında daha fazla bilgi [için, tahminci kullanarak Azure Machine Learning'e sahip tren modellerine](how-to-train-ml-models.md) bakın

```python
from azureml.train.dnn import TensorFlow

script_params = {
    '--data-folder': dataset.as_named_input('mnist').as_mount(),
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

## <a name="submit-a-run"></a>Çalıştırma gönder

[Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) nesnesi, iş çalışırken ve tamamlandıktan sonra çalışma geçmişine arabirimi sağlar.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Run yürütülderken, aşağıdaki aşamalardan geçer:

- **Hazırlama**: TensorFlow tahmincisine göre docker görüntüsü oluşturulur. Görüntü çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonraki çalıştırmalar için önbelleğe alındı. Günlükler de çalışma geçmişine akışı ve ilerleme izlemek için görüntülenebilir.

- **Ölçeklendirme**: Toplu AI kümesi, çalıştırmayı yürütmek için şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeye çalışır.

- **Çalışma**: Komut dosyası klasöründeki tüm komut dosyaları bilgi işlem hedefine yüklenir, veri depolarına monte edilir veya kopyalanır ve entry_script yürütülür. Stdout ve ./logs klasöründen çıkan çıktılar çalışma geçmişine aktarılır ve çalıştırmayı izlemek için kullanılabilir.

- **Post-Processing**: Çalıştırın ./çıktılar klasörü çalışma geçmişine kopyalanır.

## <a name="register-the-model"></a>Modeli kaydedin

DNN modelini eğittikten sonra çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimini ve dağıtımı](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümünüzü yapmanızı sağlar.

```Python
model = run.register_model(model_name='keras-dnn-mnist', model_path='outputs/model')
```

> [!TIP]
> Az önce kaydettiğiniz model, eğitim için hangi tahminciden kullandığınıza bakılmaksızın Azure Machine Learning'deki diğer kayıtlı modelle aynı şekilde dağıtılır. Dağıtım nasıl yapılacağını niçin kaydeden modellerle ilgili bir bölüm içerir, ancak zaten kayıtlı bir modeliniz olduğundan dağıtım için [doğrudan bir işlem hedefi oluşturmayı](how-to-deploy-and-where.md#choose-a-compute-target) atlayabilirsiniz.

Ayrıca modelin yerel bir kopyasını indirebilirsiniz. Bu, ek model doğrulama çalışması yapmak için yararlı olabilir. Eğitim komut dosyasında, `mnist-keras.py`TensorFlow koruyucu nesnesi modeli yerel bir klasöre (işlem hedefine yerel) devam eder. Veri deposundan bir kopyasını indirmek için Çalıştır nesnesini kullanabilirsiniz.

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

Bu makalede, Azure Machine Learning'de bir Keras modelini eğittiniz ve kaydettiniz. Bir modeli nasıl dağıtılacayacağını öğrenmek için, model deployment makalemize devam edin.

> [!div class="nextstepaction"]
> [Modelleri nasıl ve nerede dağıtılır](how-to-deploy-and-where.md)
* [Eğitim sırasında koşu ölçümlerini izleme](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitimli bir model dağıtma](how-to-deploy-and-where.md)
* [Azure'da dağıtılmış derin öğrenim eğitimi için referans mimarisi](/azure/architecture/reference-architectures/ai/training-deep-learning)
