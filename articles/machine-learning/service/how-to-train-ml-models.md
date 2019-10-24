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
ms.date: 04/19/2019
ms.custom: seodec18
ms.openlocfilehash: 087e1cd84aa182a0aae1bef6ba3dd38f369d5189
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755950"
---
# <a name="train-models-with-azure-machine-learning-using-estimator"></a>Tahmin aracı kullanarak modelleri Azure Machine Learning eğitme

Azure Machine Learning ile, [farklı işlem hedeflerine](how-to-set-up-training-targets.md#compute-targets-for-training)eğitim betiğinizi, [RunConfiguration nesnesi](how-to-set-up-training-targets.md#whats-a-run-configuration) ve [ScriptRunConfig nesnesini](how-to-set-up-training-targets.md#submit)kullanarak kolayca gönderebilirsiniz. Bu model size çok fazla esneklik ve en yüksek denetim sağlar.

Derin öğrenme modeli eğitimini kolaylaştırmak için, Azure Machine Learning Python SDK 'sı, kullanıcıların kolayca çalıştırma yapılandırması oluşturmasına olanak tanıyan, daha yüksek düzeyde bir soyutlama ve tahmin aracı sınıfı sağlar. Seçtiğiniz herhangi bir öğrenme çerçevesini (scikit-öğren gibi) kullanarak eğitim betiğini, sizin yerel makineniz, Azure 'da tek bir VM veya Azure 'da bir GPU kümesi olsun, seçtiğiniz herhangi bir bilgi işlem hedefinde göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz. Pytorch, TensorFlow ve Chainer görevleri için Azure Machine Learning Ayrıca bu çerçeveleri kullanmayı basitleştirmek için, ilgili [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmini 'ı da sağlar.

## <a name="train-with-an-estimator"></a>Bir tahmin aracı ile eğitme

[Çalışma](concept-workspace.md) alanınızı oluşturup [geliştirme ortamınızı](how-to-configure-environment.md)ayarladıktan sonra, Azure Machine Learning bir modeli eğitmek aşağıdaki adımları içerir:  
1. [Uzaktan işlem hedefi](how-to-set-up-training-targets.md) oluşturma (yerel bilgisayarı işlem hedefi olarak da kullanabilirsiniz)
2. [Eğitim verilerinizi](how-to-access-data.md) veri deposuna yükleme (isteğe bağlı)
3. [Eğitim betiğinizi](tutorial-train-models-with-aml.md#create-a-training-script) oluşturma
4. @No__t_0 nesnesi oluşturma
5. Tahmin aracı 'ı çalışma alanı altındaki bir deneme nesnesine gönderme

Bu makale 4-5. adıma odaklanır. 1-3 adımları için bir örnek için [modeli eğitme öğreticisine](tutorial-train-models-with-aml.md) bakın.

### <a name="single-node-training"></a>Tek düğümlü eğitim

Azure 'da bir scikit-öğrenme modeli için uzak işlem üzerinde çalışan tek düğümlü bir eğitim için `Estimator` kullanın. [İşlem hedefi](how-to-set-up-training-targets.md#amlcompute) nesne `compute_target` ve [veri deposu](how-to-access-data.md) nesneniz `ds` zaten oluşturmuş olmanız gerekir.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Bu kod parçacığı `Estimator` oluşturucusuna aşağıdaki parametreleri belirtir.

Parametre | Açıklama
--|--
`source_directory`| Eğitim işi için gereken tüm kodunuzu içeren yerel dizin. Bu klasör yerel makinenizden uzak işlem 'a kopyalanmış olur.
`script_params`| Eğitim betiğe geçirilecek komut satırı bağımsız değişkenlerini belirten sözlük, `<command-line argument, value>` çiftleri biçiminde `entry_script`. @No__t_0 bir ayrıntılı bayrak belirtmek için `<command-line argument, "">` kullanın.
`compute_target`| Eğitim betiğinizin çalışacağı uzaktan işlem hedefi, bu durumda bir Azure Machine Learning Işlem ([Amlcompute](how-to-set-up-training-targets.md#amlcompute)) kümesi olur. (AmlCompute kümesi yaygın olarak kullanılan hedef olsa da, Azure VM 'Leri veya hatta yerel bilgisayar gibi diğer bilgi işlem hedefi türlerini de seçebilirsiniz.)
`entry_script`| Uzaktan işlem üzerinde çalıştırılacak eğitim betiğinin FilePath (`source_directory` göreli). Bu dosya ve bağımlı olduğu tüm ek dosyalar bu klasörde bulunmalıdır.
`conda_packages`| Eğitim betiğinizin gerektirdiği Conda ile yüklenecek Python paketlerinin listesi.  

Oluşturucunun, gerekli tüm PIP paketleri için kullandığınız `pip_packages` adlı başka bir parametresi vardır.

@No__t_0 nesneniz oluşturduğumuzdan, [deneme](concept-azure-machine-learning-architecture.md#experiments) nesne `experiment` `submit` işlevine yapılan bir çağrı ile uzak işlem üzerinde çalıştırılacak eğitim işini iletin. 

```Python
run = experiment.submit(sk_est)
print(run.get_portal_url())
```

> [!IMPORTANT]
> **Özel klasörler** İki klasör, *Çıkış* ve *günlük*, Azure Machine Learning özel bir işleme alır. Eğitim sırasında, kök dizine (sırasıyla `./outputs` ve `./logs` göre) bağlı *çıktılar* ve *Günlükler* adlı klasörlere dosya yazdığınızda, dosyalar bir kez erişmeniz için otomatik olarak çalıştırma geçmişinize yüklenir çalıştırma bitti.
>
> Eğitim sırasında (model dosyaları, kontrol noktaları, veri dosyaları veya çizilmiş görüntüler gibi) yapıt oluşturmak için bunları `./outputs` klasöre yazın.
>
> Benzer şekilde, eğitim çalıştırınızdan `./logs` klasöre herhangi bir günlük yazabilirsiniz. Azure Machine Learning [tensorboard tümleştirmesini](https://aka.ms/aml-notebook-tb) kullanmak Için, tensorboard günlüklerinizi bu klasöre yazdığınızdan emin olun. Çalışma işlemi devam ederken, TensorBoard 'ı başlatabilir ve bu günlükleri akıtırabileceksiniz.  Daha sonra, günlükleri önceki çalıştırabileceksiniz herhangi birinden geri yükleyebilirsiniz.
>
> Örneğin, uzak eğitim çalıştırıldıktan sonra, *çıktılar* klasörüne yazılmış bir dosyayı yerel makinenize indirmek için: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Dağıtılmış eğitim ve özel Docker görüntüleri

@No__t_0 ile gerçekleştirebilmeniz için kullanabileceğiniz iki ek eğitim senaryosu vardır:
* Özel bir Docker görüntüsü kullanma
* Çok düğümlü bir kümede dağıtılmış eğitim

Aşağıdaki kod, bir keras modeli için dağıtılmış eğitimin nasıl alınacağını gösterir. Ayrıca, varsayılan Azure Machine Learning görüntülerini kullanmak yerine, Docker Hub 'dan eğitim için `continuumio/miniconda` özel bir Docker görüntüsü belirtir.

@No__t_1 [işlem hedefi](how-to-set-up-training-targets.md#amlcompute) nesneniz zaten oluşturmuş olmanız gerekir. Tahmin aracı 'ı aşağıdaki gibi oluşturursunuz:

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

Yukarıdaki kod `Estimator` oluşturucusuna aşağıdaki yeni parametreleri kullanıma sunar:

Parametre | Açıklama | Varsayılan
--|--|--
`custom_docker_image`| Kullanmak istediğiniz görüntünün adı. Yalnızca ortak Docker depolarında (Bu durumda Docker Hub 'da) bulunan görüntüleri sağlayın. Özel bir Docker deposundan bir görüntü kullanmak için, onun yerine oluşturucunun `environment_definition` parametresini kullanın. [Örneğe bakın](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb). | `None`
`node_count`| Eğitim işiniz için kullanılacak düğüm sayısı. | `1`
`process_count_per_node`| Her düğümde çalıştırılacak işlem sayısı (veya "çalışanlar"). Bu durumda, her bir düğümde bulunan `2` GPU 'ları kullanırsınız.| `1`
`distributed_training`| MPı arka ucunu kullanarak dağıtılmış eğitime başlatmaya yönelik [Mpiconation](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) nesnesi.  | `None`


Son olarak, eğitim işini gönder:
```Python
run = experiment.submit(estimator)
print(run.get_portal_url())
```

## <a name="github-tracking-and-integration"></a>GitHub izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Daha fazla bilgi için bkz. [Azure Machine Learning Için git tümleştirmesi](concept-train-model-git-integration.md).

## <a name="examples"></a>Örnekler
Bir tahmin aracı deseninin temellerini gösteren bir not defteri için bkz.:
* [Nasıl yapılır kullanımı-azureml/eğitim-ayrıntılı-öğrenme/nasıl yapılır-kullan-Estimator](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb)

Estimator kullanarak bir scikit-model öğrenerek bir not defteri için, bkz.:
* [Öğreticiler/img-Classification-part1-Training. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Derinlemesine öğrenme çerçevesine özgü estimators kullanarak eğitim modellerindeki Not defterleri için bkz.:
* [Nasıl yapılır kullanımı-azureml/eğitim-ayrıntılı-öğrenme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim sırasında çalıştırma ölçümlerini izleyin](how-to-track-experiments.md)
* [PyTorch modellerini eğitme](how-to-train-pytorch.md)
* [TensorFlow modellerini eğitme](how-to-train-tensorflow.md)
* [Hiper parametreleri ayarla](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
