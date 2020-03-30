---
title: Derin öğrenme Chainer modellerini eğitin
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Chainer tahmincisini kullanarak PyTorch eğitim komut dosyalarınızı kurumsal ölçekte nasıl çalıştırılacınız öğrenin.  Örnek komut dosyası, numpy'nin üzerinde çalışan Chainer Python kitaplığını kullanarak derin bir öğrenme sinir ağı oluşturmak için el yazısıyla yazılmış basamak lı görüntüleri sınıflandırır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: sdgilley
ms.date: 08/02/2019
ms.openlocfilehash: e2840a6295140e0dc22a032fa844c0488403c5a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75536622"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning"></a>Azure Machine Learning ile Chainer modellerini ölçekte eğitin ve kaydettirin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning [Chainer tahmincisini](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) kullanarak [Chainer](https://chainer.org/) eğitim komut dosyalarınızı kurumsal ölçekte nasıl çalıştırılacanız gerektiğini öğrenin. Bu makaledeki örnek eğitim komut dosyası, [numpy](https://www.numpy.org/)üzerinde çalışan Chainer Python kitaplığı kullanılarak oluşturulmuş derin bir sinir ağı (DNN) kullanarak el yazısı rakamları sınıflandırmak için popüler [MNIST veri kümesini](http://yann.lecun.com/exdb/mnist/) kullanır.

İster yerden derin öğrenme chainer modelini eğitin, ister mevcut bir modeli buluta getiriyor olun, elastik bulut bilgi işlem kaynaklarını kullanarak açık kaynak eğitim işlerini ölçeklendirmek için Azure Machine Learning'i kullanabilirsiniz. Azure Machine Learning ile üretim sınıfı modeller oluşturabilir, dağıtabilir, sürümleyebilir ve izleyebilirsiniz. 

[Derin öğrenme vs makine öğrenimi](concept-deep-learning-vs-machine-learning.md)hakkında daha fazla bilgi edinin.

Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu aşağıdaki ortamlardan herhangi birinde çalıştırın:

- Azure Machine Learning bilgi işlem örneği - indirme veya yükleme ye gerek yok

    - [Öğreticiyi tamamlayın:](tutorial-1st-experiment-sdk-setup.md) SDK ve örnek depoyla önceden yüklenmiş özel bir dizüstü bilgisayar sunucusu oluşturmak için kurulum ortamı ve çalışma alanı.
    - Not defteri sunucusundaki derin öğrenme klasöründe, nasıl kullanılır-azureml > ml çerçeveleri > > > dağıtım > tamamlanmış bir not defteri ve dosyaları **> train-hyperparameter-tune-deploy-with-chainer** klasöründe bulun.  Not defteri, akıllı hiperparametre atonlarını, model dağıtımını ve dizüstü bilgisayar widget'larını kapsayan genişletilmiş bölümler içerir.

- Kendi Jupyter Notebook sunucunuz

    - [Azure Machine Learning SDK'yı yükleyin.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Çalışma alanı yapılandırma dosyası oluşturun.](how-to-configure-environment.md#workspace)
    - Örnek komut dosyası dosyasını [chainer_mnist.py'yi](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer)indirin.
     - Ayrıca github örnekleri sayfasında bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) bulabilirsiniz. Not defteri, akıllı hiperparametre atonlarını, model dağıtımını ve dizüstü bilgisayar widget'larını kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölümde, gerekli python paketlerini yükleyerek, bir çalışma alanını başlatma, deneme oluşturma ve eğitim verilerini ve eğitim komut dosyalarını yükleyerek eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak azureml.core Python kitaplığını içe aktarın ve sürüm numarasını görüntüleyin.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning çalışma alanı,](concept-workspace.md) hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm eserlerle çalışmak için merkezi bir yer sağlar. Python SDK'da, bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapılarına erişebilirsiniz.

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyayı okuyarak bir çalışma alanı nesnesi oluşturun:

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Proje dizini oluşturma
Uzak kaynakta erişmeniz gereken yerel makinenizden gerekli tüm kodu içeren bir dizin oluşturun. Buna eğitim komut dosyası ve eğitim komut dosyanızın bağlı olduğu ek dosyalar dahildir.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Eğitim komut dosyası hazırlama

Bu öğreticide, eğitim komut dosyası **chainer_mnist.py** zaten sizin için sağlanmıştır. Uygulamada, herhangi bir özel eğitim komut dosyasını olduğu gibi alabilir ve kodunuzu değiştirmek zorunda kalmadan Azure ML ile çalıştırabilirsiniz.

Azure ML izleme ve ölçüm özelliklerini kullanmak için, eğitim komut dosyanıza az miktarda Azure ML kodu ekleyin.  **chainer_mnist.py** eğitim komut dosyası, komut dosyasıiçindeki `Run` nesneyi kullanarak Azure ML çalıştırınıza bazı ölçümlerin nasıl günlüğe kaydedilebildiğini gösterir.

Sağlanan eğitim komut dosyası, zincirleyici `datasets.mnist.get_mnist` işlevinden örnek verileri kullanır.  Kendi verileriniz için, eğitim sırasında verileri kullanılabilir hale getirmek için [veri kümesini ve komut dosyalarını yükleme](how-to-train-keras.md#data-upload) gibi adımları kullanmanız gerekebilir.

Eğitim komut dosyasını **chainer_mnist.py'yi** proje dizininize kopyalayın.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-a-deep-learning-experiment"></a>Derin öğrenme deneyi oluşturma

Deneme oluşturma. Bu örnekte, "chainer-mnist" adlı bir deneme oluşturun.

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>Bir işlem hedefi oluşturma veya alma

Modelinizi eğitmek için bir [hesaplama hedefine](concept-compute-target.md) ihtiyacınız var. Bu örnekte, uzaktan eğitim bilgi işlem kaynağınız için Azure ML yönetilen bilgi işlem (AmlCompute) kullanılır.

**AmlCompute oluşturulması yaklaşık 5 dakika sürer.** Bu ada sahip AmlCompute zaten çalışma alanınızdaysa, bu kod oluşturma işlemini atlar.  

```Python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current cluster. 
print(compute_target.get_status().serialize())
```

Bilgi işlem hedefleri hakkında daha fazla bilgi için, [bilgi işlem hedef](concept-compute-target.md) makalesinin ne olduğunu görün.

## <a name="create-a-chainer-estimator"></a>Zincirleyici tahmincisi oluşturma

[Chainer tahmincisi,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) işlem hedefiniz üzerinde Chainer eğitim işlerini başlatmanın basit bir yolunu sağlar.

Chainer tahmincisi, herhangi bir [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) çerçeveyi desteklemek için kullanılabilecek genel sınıf aracılığıyla uygulanır. Genel tahminciyi kullanarak eğitim modelleri hakkında daha fazla bilgi [için, tahminci kullanarak Azure Machine Learning'e sahip tren modellerine](how-to-train-ml-models.md) bakın

```Python
from azureml.train.dnn import Chainer

script_params = {
    '--epochs': 10,
    '--batchsize': 128,
    '--output_dir': './outputs'
}

estimator = Chainer(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    pip_packages=['numpy', 'pytest'],
                    entry_script='chainer_mnist.py',
                    use_gpu=True)
```

## <a name="submit-a-run"></a>Çalıştırma gönder

[Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) nesnesi, iş çalışırken ve tamamlandıktan sonra çalışma geçmişine arabirimi sağlar.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Run yürütülderken, aşağıdaki aşamalardan geçer:

- **Hazırlama**: Chainer tahmincisine göre docker görüntüsü oluşturulur. Görüntü çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonraki çalıştırmalar için önbelleğe alındı. Günlükler de çalışma geçmişine akışı ve ilerleme izlemek için görüntülenebilir.

- **Ölçeklendirme**: Toplu AI kümesi, çalıştırmayı yürütmek için şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeye çalışır.

- **Çalışma**: Komut dosyası klasöründeki tüm komut dosyaları bilgi işlem hedefine yüklenir, veri depolarına monte edilir veya kopyalanır ve entry_script yürütülür. Stdout ve ./logs klasöründen çıkan çıktılar çalışma geçmişine aktarılır ve çalıştırmayı izlemek için kullanılabilir.

- **Post-Processing**: Çalıştırın ./çıktılar klasörü çalışma geçmişine kopyalanır.

## <a name="save-and-register-the-model"></a>Modeli kaydetme ve kaydetme

Modeli eğittikten sonra, modelini kaydedebilir ve çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimini ve dağıtımı](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümünüzü yapmanızı sağlar.


Model eğitimi tamamlandıktan sonra, aşağıdaki kodla modeli çalışma alanınıza kaydedin.  

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```

> [!TIP]
> Az önce kaydettiğiniz model, eğitim için hangi tahminciden kullandığınıza bakılmaksızın Azure Machine Learning'deki diğer kayıtlı modelle aynı şekilde dağıtılır. Dağıtım nasıl yapılacağını niçin kaydeden modellerle ilgili bir bölüm içerir, ancak zaten kayıtlı bir modeliniz olduğundan dağıtım için [doğrudan bir işlem hedefi oluşturmayı](how-to-deploy-and-where.md#choose-a-compute-target) atlayabilirsiniz.

Ayrıca modelin yerel bir kopyasını indirebilirsiniz. Bu, ek model doğrulama çalışması yapmak için yararlı olabilir. Eğitim komut dosyasında, `chainer_mnist.py`bir koruyucu nesne modeli yerel bir klasöre (işlem hedefine yerel) devam eder. Veri deposundan bir kopyasını indirmek için Çalıştır nesnesini kullanabilirsiniz.

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

Bu makalede, Azure Machine Learning'de Chainer'ı kullanarak derin bir öğrenme, sinir ağı eğitimi almış ve kaydedin. Bir modeli nasıl dağıtılacayacağını öğrenmek [için, model deployment](how-to-deploy-and-where.md) makalemize devam edin.

* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)

* [Eğitim sırasında koşu ölçümlerini izleme](how-to-track-experiments.md)

* [Azure'da dağıtılmış derin öğrenim eğitimi için referans mimarimizi görüntüleyin](/azure/architecture/reference-architectures/ai/training-deep-learning)
