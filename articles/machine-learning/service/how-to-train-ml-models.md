---
title: Tahmini ile ml modellerini eğitme
titleSuffix: Azure Machine Learning service
description: Tek düğümlü ve dağıtılmış eğitim, öğrenme ve derin öğrenme modellerini Azure Machine Learning Hizmetleri Estimator sınıfını kullanarak geleneksel makinenin yapmayı öğrenin
ms.author: maxluk
author: maxluk
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 04/19/2019
ms.custom: seodec18
ms.openlocfilehash: 0080c8ac5e957912c5fd59a7051619ee60bd914c
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68260061"
---
# <a name="train-models-with-azure-machine-learning-using-estimator"></a>Tahmin aracı kullanarak modelleri Azure Machine Learning eğitme

Azure Machine Learning ile, [farklı işlem hedeflerine](how-to-set-up-training-targets.md#compute-targets-for-training)eğitim betiğinizi, [RunConfiguration nesnesi](how-to-set-up-training-targets.md#whats-a-run-configuration) ve [ScriptRunConfig nesnesini](how-to-set-up-training-targets.md#submit)kullanarak kolayca gönderebilirsiniz. Bu model size çok fazla esneklik ve en yüksek denetim sağlar.

Derin öğrenme modeli eğitimini kolaylaştırmak için, Azure Machine Learning Python SDK 'sı, kullanıcıların kolayca çalıştırma yapılandırması oluşturmasına olanak tanıyan, daha yüksek düzeyde bir soyutlama ve tahmin aracı sınıfı sağlar. Herhangi bir bilgi işlem hedefinde çalıştırmak istediğiniz herhangi bir öğrenme çerçevesini (örneğin, yerel makineniz, Azure 'da tek bir VM veya Azure 'da bir GPU kümesi) kullanarak eğitim betiği göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturup kullanabilirsiniz. Pytorch, TensorFlow ve Chainer görevleri için Azure Machine Learning Ayrıca bu çerçeveleri kullanmayı basitleştirmek için, ilgili [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmini 'ı da sağlar.

## <a name="train-with-an-estimator"></a>Bir tahmin ile eğitme

Oluşturduktan sonra [çalışma](concept-workspace.md) ve ayarlayın, [geliştirme ortamı](how-to-configure-environment.md), Azure Machine Learning modeli, aşağıdaki adımları içerir:  
1. [Uzaktan işlem hedefi](how-to-set-up-training-targets.md) oluşturma (yerel bilgisayarı işlem hedefi olarak da kullanabilirsiniz)
2. [Eğitim verilerinizi](how-to-access-data.md) veri deposuna yükleme (isteğe bağlı)
3. Oluşturma, [eğitim betiği](tutorial-train-models-with-aml.md#create-a-training-script)
4. Oluşturma bir `Estimator` nesnesi
5. Tahmin aracı 'ı çalışma alanı altındaki bir deneme nesnesine gönderme

Bu makalede, adım 4-5 üzerinde odaklanır. Adım 1-3 başvurmak için [model Öğreticisi eğitme](tutorial-train-models-with-aml.md) örneği.

### <a name="single-node-training"></a>Tek düğümlü eğitim

Kullanım bir `Estimator` bir scikit için uzak işlem azure'da çalışan bir tek düğümlü eğitim-model öğrenin. Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#amlcompute) nesne `compute_target` ve [veri deposu](how-to-access-data.md) nesne `ds`.

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

Bu kod parçacığı için aşağıdaki parametreleri belirtir `Estimator` Oluşturucusu.

Parametre | Açıklama
--|--
`source_directory`| Eğitim işine yönelik gerekli kodunuzun tamamını içeren yerel dizin. Bu klasörü yerel makinenizden uzak bilgisayarda kopyalanır 
`script_params`| Komut satırı bağımsız değişkenlerini, `entry_script` `<command-line argument, value>` çiftler biçiminde eğitim betiğinizi belirten sözlük. İçinde `script_params`ayrıntılı bir bayrak belirtmek için, kullanın `<command-line argument, "">`.
`compute_target`| Eğitim betiğinizin çalışacağı uzaktan işlem hedefi, bu durumda bir Azure Machine Learning Işlem ([Amlcompute](how-to-set-up-training-targets.md#amlcompute)) kümesi olur. (Lütfen AmlCompute kümesi yaygın olarak kullanılan hedef olsa da, Azure VM 'Leri veya hatta yerel bilgisayar gibi diğer bilgi işlem hedefi türlerini de seçebilirsiniz.)
`entry_script`| FilePath (göreli `source_directory`) eğitim betiğin uzak işlem üzerinde çalıştırılacak. Bu dosya ve, bağımlı herhangi bir ek dosyaları bu klasörde bulunmalıdır
`conda_packages`| Eğitim betiğinizi gerekli conda aracılığıyla yüklenecek Python paketleri listesi.  

Oluşturucu adlı başka bir parametreye sahip `pip_packages` , gerekli herhangi bir pip paketleri kullanın

Oluşturduğunuza göre `Estimator` nesne, bir çağrı ile uzak işlem üzerinde çalıştırılacak eğitim işini gönderme `submit` işlevini, [deneme](concept-azure-machine-learning-architecture.md#experiments) nesne `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_portal_url())
```

> [!IMPORTANT]
> **Özel klasörler** iki klasör *çıkarır* ve *günlükleri*, Azure Machine Learning tarafından özel olarak değerlendirilmesi alırsınız. Klasörleri için dosyaları yazdığınızda eğitim sırasında *çıkarır* ve *günlükleri* kök dizine göre olan (`./outputs` ve `./logs`sırasıyla), dosyaları otomatik olarak ayarlanır çalıştırmanız tamamlandıktan sonra onlara yönelik erişimi olması için çalıştırma geçmişi karşıya yükleyin.
>
> Oluşturmak için (örneğin, model dosyaları, kontrol noktaları, veri dosyalarını veya çizilen görüntüsü) eğitim sırasında yapıtları için bu yazma `./outputs` klasör.
>
> Benzer şekilde, eğitim çalıştırınızdan `./logs` klasöre her türlü günlüğü de yazabilirsiniz. Azure Machine Learning'ın kullanmaya [TensorBoard tümleştirme](https://aka.ms/aml-notebook-tb) bu klasöre TensorBoard günlüklerinizi aldığınızdan emin olun. Çalıştırma sürerken TensorBoard başlatın ve bu günlüklerin akışını mümkün olacaktır.  Daha sonra ayrıca günlükleri, önceki çalıştırmaları birini geri yüklenmesi mümkün olacaktır.
>
> Örneğin, yazılan bir dosyayı indirmek için *çıkarır* klasör yerel makinenize sonra uzak eğitim çalıştırın: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Dağıtılmış eğitimi ve özel Docker görüntüleri

Sizin gerçekleştirdiğiniz ile iki ek eğitim senaryosu vardır `Estimator`:
* Özel Docker görüntüsü kullanma
* Çok düğümlü bir küme üzerinde dağıtılmış eğitimi

Aşağıdaki kod, bir keras modeli için dağıtılmış eğitimin nasıl alınacağını gösterir. Ayrıca, varsayılan Azure Machine Learning görüntülerini kullanmak yerine, eğitim için Docker Hub 'ından `continuumio/miniconda` özel bir Docker görüntüsü belirtir.

Oluşturmuş olmanız, [hedef işlem](how-to-set-up-training-targets.md#amlcompute) nesne `compute_target`. Tahmin aracı gibi oluşturun:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-keras-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      conda_packages=['tensorflow', 'keras'],
                      custom_docker_base_image='continuumio/miniconda')
```

Yukarıdaki kod için aşağıdaki yeni parametreleri sunan `Estimator` Oluşturucusu:

Parametre | Açıklama | Varsayılan
--|--|--
`custom_docker_base_image`| Kullanmak istediğiniz resmin adı. Yalnızca genel docker depolardaki (Bu durum Docker hub'daki) görüntüleri sağlar. Özel bir Docker deposundan bir görüntü kullanmak için, oluşturucunun `environment_definition` parametresini kullanın. [Örneğe bakın](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb). | `None`
`node_count`| Eğitim işine yönelik kullanmak için düğüm sayısı. | `1`
`process_count_per_node`| Her bir düğümde çalıştırılacak işlemleri (veya "çalışanları") sayısı. Bu durumda, kullandığınız `2` GPU'ları her düğümde kullanılabilir.| `1`
`distributed_backend`| Başlatmak için arka uç MPI Estimator sunan eğitim dağıtılmış.  Paralel veya dağıtılmış eğitimini yürütmek için (örneğin `node_count`> 1 veya `process_count_per_node`> 1 veya her ikisi) ayarlayın `distributed_backend='mpi'`. AML tarafından kullanılan MPI uygulamasıdır [açık MPI](https://www.open-mpi.org/).| `None`

Son olarak, eğitim işini gönder:
```Python
run = experiment.submit(estimator)
print(run.get_portal_url())
```

## <a name="github-tracking-and-integration"></a>GitHub izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Örneğin, deponun geçerli kayıt KIMLIĞI, geçmişin bir parçası olarak günlüğe kaydedilir.

## <a name="examples"></a>Örnekler
Bir tahmin aracı deseninin temellerini gösteren bir not defteri için bkz.:
* [Nasıl yapılır kullanımı-azureml/eğitim-ayrıntılı-öğrenme/nasıl yapılır-kullan-Estimator](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb)

Estimator kullanarak bir scikit-model öğrenerek bir not defteri için, bkz.:
* [öğreticiler/img-sınıflandırma-bölüm 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Derinlemesine öğrenme çerçevesine özgü estimators kullanarak eğitim modellerindeki Not defterleri için bkz.:
* [How-to-use-azureml/Training-With-DEEP-Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [PyTorch modellerini eğitin](how-to-train-pytorch.md)
* [TensorFlow modellerini eğitin](how-to-train-tensorflow.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
