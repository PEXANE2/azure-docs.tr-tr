---
title: Tahmincileri olan ML modellerini eğitin
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Estimator sınıfını kullanarak geleneksel makine öğrenimi ve derin öğrenme modellerinin tek düğümlü ve dağıtılmış eğitimini nasıl gerçekleştireceklerini öğrenin
ms.author: maxluk
author: maxluk
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: 678af1855baf52efa727444236de8a1724a7d0b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79078486"
---
# <a name="train-models-with-azure-machine-learning-using-estimator"></a>Tahminci kullanarak Azure Machine Learning ile modelleri eğitin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ile, bir [RunConfiguration nesnesi](how-to-set-up-training-targets.md#whats-a-run-configuration) ve [ScriptRunConfig nesnesi](how-to-set-up-training-targets.md#submit)kullanarak eğitim komut dosyanızı [çeşitli bilgi işlem hedeflerine](how-to-set-up-training-targets.md#compute-targets-for-training)kolayca gönderebilirsiniz. Bu desen size çok fazla esneklik ve maksimum kontrol sağlar.

Azure Machine Learning Python SDK, derin öğrenme modeli eğitimini kolaylaştırmak için, kullanıcıların çalıştırılan yapılandırmaları kolayca oluşturmalarına olanak tanıyan, tahmin sınıfı olan alternatif bir üst düzey soyutlama sağlar. Yerel makineniz, Azure'daki tek bir VM veya Azure'da bir GPU kümesi olsun, seçtiğiniz herhangi bir bilgi işlem hedefinde seçtiğiniz herhangi bir öğrenme çerçevesini (örneğin, scikit-learn) kullanarak eğitim komut dosyası göndermek için genel bir [Tahminci](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz. PyTorch, TensorFlow ve Chainer görevleri için Azure Machine Learning, bu çerçeveleri kullanmayı kolaylaştırmak için ilgili [PyTorch,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmincileri de sağlar.

## <a name="train-with-an-estimator"></a>Bir tahminci ile tren

[Çalışma alanınızı](concept-workspace.md) oluşturduktan ve geliştirme [ortamınızı](how-to-configure-environment.md)oluşturduktan sonra, Azure Machine Learning'de bir modeli eğitmek aşağıdaki adımları içerir:  
1. Uzaktan [işlem hedefi](how-to-set-up-training-targets.md) oluşturma (yerel bilgisayarı bilgi işlem hedefi olarak da kullanabilirsiniz)
2. Eğitim [verilerinizi](how-to-access-data.md) datastore'a yükleyin (İsteğe bağlı)
3. Eğitim [komut dosyanızı](tutorial-train-models-with-aml.md#create-a-training-script) oluşturun
4. `Estimator` nesnesi oluşturun
5. Tahminciyi çalışma alanı nın altındaki bir deneme nesnesine gönderme

Bu makalede, adımlar 4-5 üzerinde duruluyor. Adım 1-3 için, [bir](tutorial-train-models-with-aml.md) örnek için bir model öğretici tren bakın.

### <a name="single-node-training"></a>Tek düğümlü eğitim

Scikit-learn modeli için Azure'da uzaktan bilgi işlemde tek düğümlü bir eğitim çalışması için bir `Estimator` çalışma kullanın. [İşlem hedef](how-to-set-up-training-targets.md#amlcompute) nesnenizi ve `compute_target` [FileDataset](how-to-create-register-datasets.md) nesnenizi `ds`zaten oluşturmuş olmalısınız.

```Python
from azureml.train.estimator import Estimator

script_params = {
    # to mount files referenced by mnist dataset
    '--data-folder': ds.as_named_input('mnist').as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Bu kod snippet oluşturucuya aşağıdaki `Estimator` parametreleri belirtir.

Parametre | Açıklama
--|--
`source_directory`| Eğitim işi için gereken tüm kodunuzu içeren yerel dizini. Bu klasör, yerel makinenizden uzak bilgisayara kopyalanır.
`script_params`| Sözlük, eğitim komut dosyasına `entry_script`çift şeklinde `<command-line argument, value>` geçecek komut satırı bağımsız değişkenlerini belirtir. Ayrıntılı bayrak belirtmek için `script_params`, `<command-line argument, "">`.
`compute_target`| Eğitim komut dosyanızın çalışacağı uzak bilgi işlem hedefi, bu durumda bir Azure Machine Learning Compute[(AmlCompute)](how-to-set-up-training-targets.md#amlcompute)kümesi. (AmlCompute kümesi yaygın olarak kullanılan hedef olsa da, Azure VM'leri ve hatta yerel bilgisayar gibi diğer bilgi işlem hedef türlerini seçmek de mümkündür.)
`entry_script`| Uzak bilgi işlemde `source_directory`çalıştırılacak eğitim komut dosyasının dosya yolu (göreli) . Bu dosya ve bağlı olduğu ek dosyalar bu klasörde bulunmalıdır.
`conda_packages`| Eğitim komut dosyasının gerektirdiği conda üzerinden yüklenecek Python paketlerinin listesi.  

Oluşturucu, gerekli olan `pip_packages` pip paketleri için kullandığınız başka bir parametreye sahiptir.

Nesnenizi oluşturduğunuza `Estimator` göre, [Deneme](concept-azure-machine-learning-architecture.md#experiments) nesnenizdeki `experiment` `submit` işleve çağrı yla uzak bilgi işlemde çalıştırılmak üzere eğitim işini gönderin. 

```Python
run = experiment.submit(sk_est)
print(run.get_portal_url())
```

> [!IMPORTANT]
> **Özel Klasörler** İki klasör, *çıktı* ve *günlük,* Azure Machine Learning tarafından özel muamele alır. Eğitim sırasında, kök dizinine göre`./outputs` (ve `./logs`sırasıyla) adlandırılmış çıktılara ve *günlüklere* dosya yazdığınızda, dosyalarınız çalışmanız tamamlandıktan sonra bunlara erişebilmeniz için otomatik olarak çalışma geçmişinize yüklenir. *logs*
>
> Eğitim sırasında yapı oluşturmak için (model dosyaları, denetim noktaları, veri dosyaları veya `./outputs` çizilmiş resimler gibi) bunları klasöre yazın.
>
> Benzer şekilde, eğitim çalıştırdığınızdan herhangi bir `./logs` günlükleri klasöre yazabilirsiniz. Azure Machine Learning'in [TensorBoard tümleştirmesini](https://aka.ms/aml-notebook-tb) kullanmak için TensorBoard günlüklerinizi bu klasöre yazdığınızdan emin olun. Çalışmanız devam ederken, TensorBoard'u başlatabilir ve bu günlükleri aktarabilirsiniz.  Daha sonra, önceki çalıştırmalarınızın herhangi birinden günlükleri geri yükleyebilirsiniz.
>
> Örneğin, *çıktılar* klasörüne yazılmış bir dosyayı uzaktan eğitim çalıştırmanızdan sonra yerel makinenize indirmek için:`run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Dağıtılmış eğitim ve özel Docker görüntüleri

İki ek eğitim senaryosu `Estimator`vardır:
* Özel Docker görüntüsü kullanma
* Çok düğümlü kümede dağıtılmış eğitim

Aşağıdaki kod, bir Keras modeli için dağıtılmış eğitimin nasıl yürütilengerektiğini gösterir. Ayrıca, varsayılan Azure Machine Learning görüntülerini kullanmak yerine, eğitim için Docker `continuumio/miniconda` Hub'dan özel bir docker görüntüsü belirtir.

[İşlem hedef](how-to-set-up-training-targets.md#amlcompute) nesnenizi `compute_target`zaten oluşturmuş olmalısınız. Tahminciyi aşağıdaki gibi oluşturursunuz:

```Python
from azureml.train.estimator import Estimator
from azureml.core.runconfig import MpiConfiguration

estimator = Estimator(source_directory='./my-keras-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),        
                      conda_packages=['tensorflow', 'keras'],
                      custom_docker_image='continuumio/miniconda')
```

Yukarıdaki kod, aşağıdaki yeni parametreleri `Estimator` oluşturucuya ait:

Parametre | Açıklama | Varsayılan
--|--|--
`custom_docker_image`| Kullanmak istediğiniz görüntünün adı. Yalnızca ortak docker depolarında (bu durumda Docker Hub) kullanılabilen görüntüleri sağlayın. Özel bir docker deposundan bir görüntü kullanmak için, bunun `environment_definition` yerine oluşturucuparametresini kullanın. [Bkz. örnek](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb). | `None`
`node_count`| Eğitim işinizde kullanılacak düğüm sayısı. | `1`
`process_count_per_node`| Her düğümde çalışacak işlem sayısı (veya "işçi"). Bu durumda, her `2` düğümde bulunan GPU'ları kullanırsınız.| `1`
`distributed_training`| MPI arka uç kullanarak dağıtılmış eğitim başlatmak için [MPIConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) nesnesi.  | `None`


Son olarak, eğitim işi gönderin:
```Python
run = experiment.submit(estimator)
print(run.get_portal_url())
```

## <a name="registering-a-model"></a>Bir modeli kaydetme

Modeli eğittikten sonra, modelini kaydedebilir ve çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimini ve dağıtımı](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümünüzü yapmanızı sağlar.

Aşağıdaki kodu çalıştırmak modeli çalışma alanınıza kaydeder ve uzak bilgi işlem bağlamlarında veya dağıtım komut dosyalarında ada göre başvuruda bulunabilmek için kullanılabilir hale getirecektir. Daha [`register_model`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-) fazla bilgi ve ek parametreler için başvuru dokümanlarına bakın.

```python
model = run.register_model(model_name='sklearn-sample', model_path=None)
```

## <a name="github-tracking-and-integration"></a>GitHub izleme ve entegrasyon

Kaynak dizininin yerel bir Git deposu olduğu bir eğitim çalışması başlattığınızda, depo hakkındaki bilgiler çalışma geçmişinde depolanır. Daha fazla bilgi [için Azure Machine Learning için Git tümleştirmesi'ne](concept-train-model-git-integration.md)bakın.

## <a name="examples"></a>Örnekler
Tahminci deseninin temellerini gösteren bir not defteri için bkz:
* [nasıl kullanılır-azureml/eğitim-ile-derin öğrenme/nasıl kullanılır-tahmin](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb)

Tahminci kullanarak scikit-learn modelini eğiten bir dizüstü bilgisayar için bkz:
* [öğreticiler/img-sınıflandırma-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Derin öğrenme çerçevesine özgü tahmincileri kullanarak eğitim modellerindeki dizüstü bilgisayarlar için bkz:

* [nasıl kullanılır-azureml/ml-çerçeveler](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim sırasında koşu ölçümlerini izleme](how-to-track-experiments.md)
* [PyTorch modellerini eğitme](how-to-train-pytorch.md)
* [TensorFlow modellerini eğitme](how-to-train-tensorflow.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitimli bir model dağıtma](how-to-deploy-and-where.md)
* [Eğitim ve dağıtım için ortamlar oluşturma ve yönetme](how-to-use-environments.md)