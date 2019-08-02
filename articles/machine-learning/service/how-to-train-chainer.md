---
title: Chainer modellerini eğitme ve kaydetme
titleSuffix: Azure Machine Learning service
description: Bu makalede, Azure Machine Learning hizmetini kullanarak bir Chainer modelinin nasıl eğiteyapılacağı ve kaydedileceği gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: sdgilley
ms.date: 06/15/2019
ms.openlocfilehash: 7cf5650708cd951e872e3df6ea533a62bde0389d
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68618333"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile Chainer modellerini eğitme ve kaydetme

Bu makalede, Azure Machine Learning hizmetini kullanarak bir Chainer modelinin nasıl eğiteyapılacağı ve kaydedileceği gösterilmektedir. Büyük/büyük/ [](http://yann.lecun.com/exdb/mnist/) [büyük/büyük](https://www.numpy.org/)/veya büyük/veya büyük/veya büyük/veya büyük [](https://Chainer.org)

Chainer, geliştirmeyi basitleştirmek için diğer popüler DNN çerçevelerinin üzerinde çalışan üst düzey bir sinir ağ API 'sidir. Azure Machine Learning hizmeti sayesinde, esnek bulut işlem kaynaklarını kullanarak eğitim işlerini hızla ölçeklendirebilirsiniz. Ayrıca eğitim çalıştırmalarını, sürüm modellerinizi, modellerinizi dağıtmayı ve daha fazlasını izleyebilirsiniz.

Baştan sona bir Chainer modeli geliştirirken veya var olan bir modeli buluta verdiğinize göre Azure Machine Learning hizmet, üretime hazırlamış modeller oluşturmanıza yardımcı olabilir.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu şu ortamlardan birinde çalıştırın:

- Azure Machine Learning Not defteri VM-indirme veya yükleme gerekli değil

    - SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu oluşturmak için [bulut tabanlı Not defteri hızlı](quickstart-run-cloud-notebook.md) başlangıcını doldurun.
    - Not defteri sunucusundaki örnekler klasöründe, **nasıl yapılır-kullan-azureml** -------------------------ile  Not defteri, akıllı hiper parametre ayarlamayı, model dağıtımını ve Not defteri pencere öğelerini kapsayan genişletilmiş bölümler içerir.

- Kendi Jupyter Notebook sunucunuz

    - [Python için Azure Machine Learning SDK 'sını yükler](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - Örnek betik dosyası [chainer_mnist. Kopyala](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/chainer_mnist.py) ' yı İndirin
     - GitHub örnekleri sayfasında bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) de bulabilirsiniz. Not defteri, akıllı hiper parametre ayarlamayı, model dağıtımını ve Not defteri pencere öğelerini kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölüm, gerekli Python paketlerini yükleyerek, bir çalışma alanı başlatarak, bir deneme oluşturarak ve eğitim verilerini ve eğitim betikleri karşıya yükleyerek Eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, azureml. Core Python kitaplığını içeri aktarın ve sürüm numarasını görüntüleyin.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning hizmet çalışma alanı](concept-workspace.md) , hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Python SDK 'sında bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapıtlarına erişebilirsiniz.

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyayı okuyarak bir çalışma alanı nesnesi oluşturun:

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Proje dizini oluşturma
Yerel makinenizden uzak kaynak üzerinde erişmeniz gereken tüm gerekli kodu içerecek bir dizin oluşturun. Bu eğitim betiğini ve eğitim betiğinizin bağımlı olduğu diğer dosyaları içerir.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Eğitim betiğini hazırla

Bu öğreticide, **chainer_mnist. Kopyala** eğitim betiği sizin için zaten sağlanmış. Uygulamada, herhangi bir özel eğitim betiğini olduğu gibi götürebilmeniz ve kodunuzu değiştirmek zorunda kalmadan Azure ML ile çalıştırmanız gerekir.

Azure ML 'nin izleme ve ölçüm yeteneklerini kullanmak için eğitim betiğinizin içine küçük miktarda Azure ML kodu ekleyin.  Eğitim betiği **chainer_mnist. Kopyala** , bazı ölçümlerin Azure ML çalıştırmak için betik içindeki `Run` nesnesini kullanarak nasıl günlüğe alınacağını gösterir.

Belirtilen eğitim betiği, Chainer `datasets.mnist.get_mnist` işlevinden örnek verileri kullanır.  Kendi verileriniz için verileri eğitim sırasında kullanılabilir hale getirmek üzere veri [kümesini ve betikleri karşıya yükleme](how-to-train-keras.md#upload-dataset-and-scripts) gibi adımları kullanmanız gerekebilir.

Eğitim betiği **chainer_mnist.** Kopyala ' yı proje dizininize kopyalayın.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Deneme oluşturma. Bu örnekte, "Chainer-mnist" adlı bir deneme oluşturun.

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>İşlem hedefi oluştur veya al

Modelinize eğitim için bir [işlem hedefine](concept-compute-target.md) ihtiyacınız vardır. Bu örnekte, uzaktan eğitim işlem kaynağınız için Azure ML yönetilen işlem (AmlCompute) kullanacaksınız.

**AmlCompute oluşturma işlemi yaklaşık 5 dakika sürer**. Bu ada sahip AmlCompute zaten çalışma alanınızda varsa, bu kod oluşturma işlemini atlar.  

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

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

## <a name="create-a-chainer-estimator"></a>Chainer tahmin aracı oluşturma

[Chainer tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) , işlem Hedefinizdeki Chainer eğitim işlerini başlatmanın basit bir yolunu sağlar.

Chainer tahmin aracı, herhangi bir çerçeveyi desteklemek için [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) kullanılabilen genel sınıf aracılığıyla uygulanır. Genel tahmin aracı kullanan eğitim modelleri hakkında daha fazla bilgi için bkz. [tahmin aracı kullanarak Azure Machine Learning modelleri eğitme](how-to-train-ml-models.md)

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

## <a name="submit-a-run"></a>Bir çalıştırma gönder

[Run nesnesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) , iş çalışırken ve tamamlandıktan sonra çalışma geçmişi için arabirim sağlar.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütüldüğü için aşağıdaki aşamalardan geçer:

- **Hazırlanıyor**: Bir Docker görüntüsü, Chainer Estimator öğesine göre oluşturulur. Görüntü, çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonra çalışacak şekilde önbelleğe alınır. Günlükler, çalıştırma geçmişine de kaydedilir ve ilerlemeyi izlemek için görüntülenebilir.

- **Ölçeklendirme**: Batch AI kümesi, çalışmayı yürütmek için daha fazla düğüm, şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeyi dener.

- **Çalıştırma**: Betik klasöründeki tüm betikler işlem hedefine yüklenir, veri depoları bağlanır veya kopyalanır ve entry_script yürütülür. Stdout ve./logs klasörü çıkışları, çalıştırma geçmişine akışla kaydedilir ve çalıştırmayı izlemek için kullanılabilir.

- **Işlem sonrası**: Çalıştırmanın./çıktılar klasörü, çalışma geçmişine kopyalanır.

## <a name="save-and-register-the-model"></a>Modeli kaydetme ve kaydetme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilir ve kaydedebilirsiniz. Model kaydı, [model yönetimi ve dağıtımını](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümlerini oluşturmanıza imkan tanır.


Model eğitimi tamamlandıktan sonra, aşağıdaki kodla modeli çalışma alanınıza kaydedin.  

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```

> [!TIP]
> Modelin bulunamadığını belirten bir hata alırsanız bir dakika verin ve yeniden deneyin.  Bazen, eğitimin bitişi ile çıkış dizinindeki modelin kullanılabilirliği arasında küçük bir gecikme olur.

Ayrıca modelin yerel bir kopyasını da indirebilirsiniz. Bu, ek model doğrulama işini yerel olarak gerçekleştirmek için yararlı olabilir. Eğitim betiğinde `chainer_mnist.py`, bir koruyucu nesnesi modeli yerel bir klasöre (yerel olarak işlem hedefine) devam ettirir. Veri deposundan bir kopya indirmek için Run nesnesini kullanabilirsiniz.

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

Bu makalede, Azure Machine Learning hizmetinde bir Chainer modeli eğitildi. 

* Modeli dağıtmayı öğrenmek için [model dağıtım](how-to-deploy-and-where.md) makalemize devam edin.

* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
