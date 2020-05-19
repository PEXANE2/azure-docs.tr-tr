---
title: Tahmini ile ml modellerini eğitme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Estimator sınıfını kullanarak geleneksel makine öğrenimi ve derin öğrenme modellerine yönelik tek düğümlü ve dağıtılmış eğitime nasıl gerçekleştirileceğini öğrenin
ms.author: maxluk
author: maxluk
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: c47bdc17041b2c388b01681dc1e65ddea29b0efb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83584408"
---
# <a name="train-models-with-azure-machine-learning-using-estimator"></a>Tahmin aracı kullanarak modelleri Azure Machine Learning eğitme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning, bir [RunConfiguration nesnesi](how-to-set-up-training-targets.md#whats-a-run-configuration) ve bir [ScriptRunConfig nesnesi](how-to-set-up-training-targets.md#submit)kullanarak eğitim betiğinizi [çeşitli işlem hedeflerine](how-to-set-up-training-targets.md#compute-targets-for-training)kolayca gönderebilirsiniz. Bu model size çok fazla esneklik ve en yüksek denetim sağlar.


Tahmin aracı sınıfı, modellerinizi derin öğrenme ve pekiştirmeye dayalı öğrenme ile eğmenizi kolaylaştırır. Çalışma yapılandırmasını kolayca oluşturmanıza olanak tanıyan üst düzey bir soyutlama sağlar. Seçtiğiniz herhangi bir öğrenme çerçevesini (scikit-öğren gibi) kullanarak eğitim betiğini, sizin yerel makineniz, Azure 'da tek bir VM veya Azure 'da bir GPU kümesi olsun, seçtiğiniz herhangi bir bilgi işlem hedefinde göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz. Pytorch, TensorFlow, Chainer ve pekiştirmeye dayalı Learning görevleri için Azure Machine Learning Ayrıca bu çerçeveleri kullanmayı basitleştirmek için, ilgili [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py), [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py)ve [pekiştirmeye dayalı Learning](how-to-use-reinforcement-learning.md) tahmini 'ı da sağlar.

## <a name="train-with-an-estimator"></a>Bir tahmin aracı ile eğitme

[Çalışma](concept-workspace.md) alanınızı oluşturup [geliştirme ortamınızı](how-to-configure-environment.md)ayarladıktan sonra, Azure Machine Learning bir modeli eğitmek aşağıdaki adımları içerir:  
1. [Uzaktan işlem hedefi](how-to-set-up-training-targets.md) oluşturma (yerel bilgisayarı işlem hedefi olarak da kullanabilirsiniz)
2. [Eğitim verilerinizi](how-to-access-data.md) veri deposuna yükleme (isteğe bağlı)
3. [Eğitim betiğinizi](tutorial-train-models-with-aml.md#create-a-training-script) oluşturma
4. `Estimator` nesnesi oluşturun
5. Tahmin aracı 'ı çalışma alanı altındaki bir deneme nesnesine gönderme

Bu makale 4-5. adıma odaklanır. 1-3 adımları için bir örnek için [modeli eğitme öğreticisine](tutorial-train-models-with-aml.md) bakın.

### <a name="single-node-training"></a>Tek düğümlü eğitim

Bir `Estimator` scikit-öğrenme modeli Için Azure 'da uzaktan işlem üzerinde çalışan tek düğümlü bir eğitim için kullanın. [İşlem hedefi](how-to-set-up-training-targets.md#amlcompute) nesneniz `compute_target` ve [filedataset](how-to-create-register-datasets.md) nesneniz zaten oluşturmuş olmanız gerekir `ds` .

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

Bu kod parçacığı oluşturucuya aşağıdaki parametreleri belirtir `Estimator` .

Parametre | Açıklama
--|--
`source_directory`| Eğitim işi için gereken tüm kodunuzu içeren yerel dizin. Bu klasör yerel makinenizden uzak işlem 'a kopyalanmış olur.
`script_params`| Bir komut satırı bağımsız değişkenlerini belirten, çiftler biçiminde eğitim betiğinizi geçirilecek sözlük `entry_script` `<command-line argument, value>` . İçinde ayrıntılı bir bayrak belirtmek için `script_params` , kullanın `<command-line argument, "">` .
`compute_target`| Eğitim betiğinizin çalışacağı uzaktan işlem hedefi, bu durumda bir Azure Machine Learning Işlem ([Amlcompute](how-to-set-up-training-targets.md#amlcompute)) kümesi olur. (AmlCompute kümesi yaygın olarak kullanılan hedef olsa da, Azure VM 'Leri veya hatta yerel bilgisayar gibi diğer bilgi işlem hedefi türlerini de seçebilirsiniz.)
`entry_script`| `source_directory`Uzaktan işlem üzerinde çalıştırılacak eğitim betiğinin FilePath (öğesine göre). Bu dosya ve bağımlı olduğu tüm ek dosyalar bu klasörde bulunmalıdır.
`conda_packages`| Eğitim betiğinizin gerektirdiği Conda ile yüklenecek Python paketlerinin listesi.  

Oluşturucunun, `pip_packages` gerekli tüm PIP paketleri için kullandığınız başka bir parametresi vardır.

Artık nesneniz oluşturduğumuzdan `Estimator` , `submit` [deneme](concept-azure-machine-learning-architecture.md#experiments) nesneniz üzerindeki işleve bir çağrı ile uzak işlem üzerinde çalıştırılacak eğitim işini iletin `experiment` . 

```Python
run = experiment.submit(sk_est)
print(run.get_portal_url())
```

> [!IMPORTANT]
> **Özel klasörler** İki klasör, *Çıkış* ve *günlük*, Azure Machine Learning özel bir işleme alır. Eğitim sırasında, kök dizine (ve sırasıyla) göre olan *çıktılar* ve *Günlükler* adlı klasörlere dosya yazdığınızda `./outputs` `./logs` , çalışma işlemi tamamlandıktan sonra dosyalara erişmeniz için dosyalar otomatik olarak çalıştırma geçmişinize yüklenir.
>
> Eğitim sırasında (model dosyaları, kontrol noktaları, veri dosyaları veya çizilmiş görüntüler gibi) yapıt oluşturmak için bunları `./outputs` klasörüne yazın.
>
> Benzer şekilde, eğitim çalıştırınızdan klasöre her türlü günlüğü de yazabilirsiniz `./logs` . Azure Machine Learning [tensorboard tümleştirmesini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/export-run-history-to-tensorboard/export-run-history-to-tensorboard.ipynb) kullanmak Için, tensorboard günlüklerinizi bu klasöre yazdığınızdan emin olun. Çalışma işlemi devam ederken, TensorBoard 'ı başlatabilir ve bu günlükleri akıtırabileceksiniz.  Daha sonra, günlükleri önceki çalıştırabileceksiniz herhangi birinden geri yükleyebilirsiniz.
>
> Örneğin, uzak eğitime çalıştıktan sonra *çıktılar* klasörüne yazılmış bir dosyayı yerel makinenize indirmek için:`run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Dağıtılmış eğitim ve özel Docker görüntüleri

İle gerçekleştirebilmeniz için kullanabileceğiniz iki ek eğitim senaryosu vardır `Estimator` :
* Özel bir Docker görüntüsü kullanma
* Çok düğümlü bir kümede dağıtılmış eğitim

Aşağıdaki kod, bir keras modeli için dağıtılmış eğitimin nasıl alınacağını gösterir. Ayrıca, varsayılan Azure Machine Learning görüntülerini kullanmak yerine, eğitim için Docker Hub 'ından özel bir Docker görüntüsü belirtir `continuumio/miniconda` .

[İşlem hedefi](how-to-set-up-training-targets.md#amlcompute) nesnenizin zaten oluşturulmuş olması gerekir `compute_target` . Tahmin aracı 'ı aşağıdaki gibi oluşturursunuz:

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

Yukarıdaki kod, oluşturucuya aşağıdaki yeni parametreleri sunar `Estimator` :

Parametre | Açıklama | Varsayılan
--|--|--
`custom_docker_image`| Kullanmak istediğiniz görüntünün adı. Yalnızca ortak Docker depolarında (Bu durumda Docker Hub 'da) bulunan görüntüleri sağlayın. Özel bir Docker deposundan bir görüntü kullanmak için, oluşturucunun `environment_definition` parametresini kullanın. [Örneğe bakın](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb). | `None`
`node_count`| Eğitim işiniz için kullanılacak düğüm sayısı. | `1`
`process_count_per_node`| Her düğümde çalıştırılacak işlem sayısı (veya "çalışanlar"). Bu durumda, `2` her bir düğümde bulunan GPU 'ları kullanırsınız.| `1`
`distributed_training`| MPı arka ucunu kullanarak dağıtılmış eğitime başlatmaya yönelik [Mpiconation](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) nesnesi.  | `None`


Son olarak, eğitim işini gönder:
```Python
run = experiment.submit(estimator)
print(run.get_portal_url())
```

## <a name="registering-a-model"></a>Model kaydetme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilir ve kaydedebilirsiniz. Model kaydı, [model yönetimi ve dağıtımını](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümlerini oluşturmanıza imkan tanır.

Aşağıdaki kodu çalıştırmak, modeli çalışma alanınıza kaydeder ve uzak işlem bağlamlarındaki veya dağıtım betiklerdeki ada göre başvuruda bulunmak için kullanılabilir hale gelir. [`register_model`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-)Daha fazla bilgi ve ek parametreler için başvuru belgelerine bakın.

```python
model = run.register_model(model_name='sklearn-sample', model_path=None)
```

## <a name="github-tracking-and-integration"></a>GitHub izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Daha fazla bilgi için bkz. [Azure Machine Learning Için git tümleştirmesi](concept-train-model-git-integration.md).

## <a name="examples"></a>Örnekler
Bir tahmin aracı deseninin temellerini gösteren bir not defteri için bkz.:
* [Nasıl yapılır kullanımı-azureml/eğitim-ayrıntılı-öğrenme/nasıl yapılır-kullan-Estimator](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb)

Estimator kullanarak bir scikit-model öğrenerek bir not defteri için, bkz.:
* [Öğreticiler/img-Classification-part1-Training. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

Derinlemesine öğrenme çerçevesine özgü estimators kullanarak eğitim modellerindeki Not defterleri için bkz.:

* [nasıl kullanılır-azureml/ml-çerçeveleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim sırasında çalıştırma ölçümlerini izleyin](how-to-track-experiments.md)
* [PyTorch modellerini eğitme](how-to-train-pytorch.md)
* [TensorFlow modellerini eğitme](how-to-train-tensorflow.md)
* [Pekiştirmeye dayalı Learning derin sinir ağını eğitme](how-to-use-reinforcement-learning.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
* [Eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md)