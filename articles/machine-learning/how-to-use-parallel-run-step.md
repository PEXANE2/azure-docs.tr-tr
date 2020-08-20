---
title: Büyük verilerde toplu tahminleri Çalıştır
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ' de ParallelRunStep kullanarak büyük miktarlarda veriyi zaman uyumsuz olarak nasıl alabileceğinizi öğrenin. ParallelRunStep, büyük veri kullanım durumları için, paralel işleme özelliklerini kutudan çıkar ve yüksek aktarım hızı, yangın ve unutma çıkarımı için optimize eder.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: jmartens, larryfr
ms.author: tracych
author: tracychms
ms.date: 08/14/2020
ms.custom: Build2020, devx-track-python
ms.openlocfilehash: 04d1e531f3041ef0a6231607cc795c67168ebf2e
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88651208"
---
# <a name="run-batch-inference-on-large-amounts-of-data-by-using-azure-machine-learning"></a>Azure Machine Learning kullanarak büyük miktarlarda veri üzerinde toplu çıkarımı çalıştırın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, büyük miktarlarda veriyi hızla değerlendirmek için Azure Machine Learning modelinizi paralel olarak nasıl çalıştırabileceğiniz gösterilmektedir. 

Büyük veri kümeleri üzerinde veya karmaşık modellerle sınırlama zaman alabilir. `ParallelRunStep`Sınıfı, genel sonuçları daha hızlı bir şekilde elde etmenizi sağlar. Tek bir değerlendirme çalıştırmak oldukça hızlı olsa da birçok senaryo (nesne algılama, video işleme, doğal dil işleme vb.) birçok değerlendirme çalıştırmayı içerir. 

İle `ParallelRunStep` , toplu iş ınlaların büyük makine kümelerine ölçeklendirilmesi basittir. Bu tür kümeler, geliştirilmiş üretkenlik ve iyileştirilmiş maliyetle terabaytlarca yapılandırılmış veya yapılandırılmamış verileri işleyebilir.

Bu makalede, aşağıdaki görevleri öğreneceksiniz:

> 1. Machine Learning kaynaklarını ayarlayın.
> 1. Toplu çıkarım veri girişlerini ve çıkışını yapılandırın.
> 1. Daha önce eğitilen görüntü sınıflandırma modelini [, veri kümesine](https://publicdataset.azurewebsites.net/dataDetail/mnist/) göre hazırlayın. 
> 1.  Çıkarım betiğinizi yazın.
> 1. ParallelRunStep içeren bir [makine öğrenimi işlem hattı](concept-ml-pipelines.md) oluşturun ve genel test görüntülerinde Batch çıkarımı çalıştırın. 
> 1. Yeni veri girişi ve parametreleriyle toplu çıkarımı çalıştırmayı yeniden gönderin. 
> 1. Sonuçlara bakın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* Bir Azure Machine Learning çalışma alanınız yoksa, kılavuzlu bir hızlı başlangıç için [Kurulum öğreticisini](tutorial-1st-experiment-sdk-setup.md) doldurun. 

* Kendi ortamınızı ve bağımlılıklarınızı yönetmek için kendi yerel ortamınızı yapılandırma hakkında [nasıl yapılır kılavuzuna](how-to-configure-environment.md) bakın.

## <a name="set-up-machine-learning-resources"></a>Makine öğrenimi kaynaklarını ayarlama

Aşağıdaki eylemler bir toplu çıkarım ardışık düzeni çalıştırmak için ihtiyacınız olan makine öğrenimi kaynaklarını ayarlar:

- Bir çalışma alanına bağlanın.
- Mevcut işlem kaynağını oluşturun veya ekleyin.

### <a name="configure-workspace"></a>Çalışma alanını yapılandırma

Mevcut çalışma alanından bir çalışma alanı nesnesi oluşturun. `Workspace.from_config()` dosyadaki config.jsokur ve ayrıntıları WS adlı bir nesneye yükler.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
```

> [!IMPORTANT]
> Bu kod parçacığı, çalışma alanı yapılandırmasının geçerli dizine veya onun üst öğesine kaydedilmesini bekliyor. Çalışma alanı oluşturma hakkında daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanları oluşturma ve yönetme](how-to-manage-workspace.md). Yapılandırmayı dosyaya kaydetme hakkında daha fazla bilgi için bkz. [çalışma alanı yapılandırma dosyası oluşturma](how-to-configure-environment.md#workspace).

### <a name="create-a-compute-target"></a>İşlem hedefi oluştur

Azure Machine Learning, *işlem* (veya *işlem hedefi*), makine öğrenimi ardışık düzeninde hesaplama adımlarını gerçekleştiren makinelere veya kümelere başvurur. CPU tabanlı bir [Amlcompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) hedefi oluşturmak için aşağıdaki kodu çalıştırın.

```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpucluster")
compute_min_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("AML_COMPUTE_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use get_status()
    print(compute_target.get_status().serialize())
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

## <a name="configure-inputs-and-output"></a>Girişleri ve çıktıyı yapılandırma

### <a name="create-a-datastore-with-sample-images"></a>Örnek görüntülerle bir veri deposu oluşturma

Ortak blob kapsayıcısından, adlı bir hesapta bulunan MNIST değerlendirmesi kümesini alın `sampledata` `pipelinedata` . Bu kapsayıcıya işaret eden ada sahip bir veri deposu oluşturun `mnist_datastore` . Aşağıdaki çağrısında `register_azure_blob_container` , `overwrite` bayrağını, `True` daha önce bu adla oluşturulmuş herhangi bir veri deposundaki üzerine yazacak şekilde ayarlamak. 

, Ve için kendi değerlerinizi sağlayarak bu adımı blob kapsayıcınızı işaret etmek üzere değiştirebilirsiniz `datastore_name` `container_name` `account_name` .

```python
from azureml.core import Datastore
from azureml.core import Workspace

mnist_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="mnist_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata",
                      overwrite=True)
```

Ardından, çıkış veri deposu olarak çalışma alanı varsayılan veri deposunu belirtin. Çıkarımı çıkışı için kullanacaksınız.

Çalışma alanınızı oluşturduğunuzda, [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)   ve [BLOB depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)   alanı varsayılan olarak çalışma alanına eklenir. Azure dosyaları bir çalışma alanı için varsayılan veri depo, ancak blob depolamayı da bir veri deposu olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Storage seçenekleri](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="create-the-data-inputs"></a>Veri girişlerini oluşturma

Toplu çıkarım girişleri, paralel işleme için bölümlemek istediğiniz veri. Toplu çıkarım ardışık düzeni, aracılığıyla veri girişlerini kabul eder [`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) .

`Dataset` Azure Machine Learning verileri keşfetmek, dönüştürmek ve yönetmek içindir. İki tür vardır: [`TabularDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) ve [`FileDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py) . Bu örnekte, `FileDataset` giriş olarak kullanacaksınız. `FileDataset` dosyaları, işlem için indirme veya işleme özelliğini sağlar. Bir veri kümesi oluşturarak, veri kaynağı konumuna bir başvuru oluşturursunuz. Veri kümesine kümeleme dönüştürmeleri uyguladıysanız, bunlar veri kümesinde da depolanır. Veriler mevcut konumunda kalır, bu nedenle ek depolama maliyeti tahakkuk etmemesi gerekir.

Azure Machine Learning veri kümeleri hakkında daha fazla bilgi için bkz. [veri kümeleri oluşturma ve erişim (Önizleme)](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets).

```python
from azureml.core.dataset import Dataset

path_on_datastore = mnist_blob.path('mnist/')
input_mnist_ds = Dataset.File.from_files(path=path_on_datastore, validate=False)
```

Toplu çıkarım ardışık düzenini çalıştırırken dinamik veri girişlerini kullanmak için girdileri `Dataset` bir olarak tanımlayabilirsiniz [`PipelineParameter`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) . Bir toplu çıkarım işlem hattı çalıştırmasını her yeniden başlattığınızda giriş veri kümesini belirtebilirsiniz.

```python
from azureml.data.dataset_consumption_config import DatasetConsumptionConfig
from azureml.pipeline.core import PipelineParameter

pipeline_param = PipelineParameter(name="mnist_param", default_value=input_mnist_ds)
input_mnist_ds_consumption = DatasetConsumptionConfig("minist_param_config", pipeline_param).as_mount()
```

### <a name="create-the-output"></a>Çıktıyı oluşturma

[`PipelineData`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesneler, işlem hattı adımları arasında ara verileri aktarmak için kullanılır. Bu örnekte, çıkarım çıkışı için kullanırsınız.

```python
from azureml.pipeline.core import Pipeline, PipelineData

output_dir = PipelineData(name="inferences", datastore=def_data_store)
```

## <a name="prepare-the-model"></a>Modeli hazırlama

[Önceden eğitilen görüntü sınıflandırma modelini indirin](https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz)ve ardından `models` dizine ayıklayın.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url="https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

Ardından, işlem kaynağınız için kullanılabilir olması için modeli çalışma alanınıza kaydedin.

```python
from azureml.core.model import Model

# Register the downloaded model 
model = Model.register(model_path="models/",
                       model_name="mnist",
                       tags={'pretrained': "mnist"},
                       description="Mnist trained tensorflow model",
                       workspace=ws)
```

## <a name="write-your-inference-script"></a>Çıkarım betiğinizi yazma

>[!Warning]
>Aşağıdaki kod, [örnek Not defterinin](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/parallel-run) kullandığı bir örnektir. Senaryonuz için kendi komut dosyanızı oluşturmanız gerekir.

Betik iki işlev *içermelidir* :
- `init()`: Bu işlevi, daha sonraki çıkarım için pahalı veya genel hazırlık için kullanın. Örneğin, modeli genel bir nesneye yüklemek için kullanın. Bu işlev, işlem başlangıcında yalnızca bir kez çağrılır.
-  `run(mini_batch)`: İşlev her örnek için çalışacaktır `mini_batch` .
    -  `mini_batch`: `ParallelRunStep` çalıştırma yöntemini çağırır ve `DataFrame` yönteme bağımsız değişken olarak bir liste ya da Pandas geçirirsiniz. Giriş bir ise ve giriş bir ise, mini_batch içindeki her giriş bir dosya yolu olacaktır `FileDataset` `DataFrame` `TabularDataset` .
    -  `response`: Run () yöntemi bir Pandas `DataFrame` veya dizi döndürmelidir. Append_row output_action için, döndürülen bu öğeler ortak çıkış dosyasına eklenir. Summary_only için öğelerin içeriği yok sayılır. Tüm çıkış eylemleri için, döndürülen her çıkış öğesi girdi öğesinin giriş mini Batch 'de başarılı bir şekilde çalıştırıldığını belirtir. Girişi, çıkış sonucunu çalıştırmak için eşlemek üzere, çalışma sonuçlarına yeterli miktarda veri eklendiğinden emin olun. Çalıştırma çıkışı çıkış dosyasında yazılır ve bu sırada olması garanti edilmez, çıktıda bir anahtarı, girişle eşlemek için kullanmalısınız.

```python
%%writefile digit_identification.py
# Snippets from a sample script.
# Refer to the accompanying digit_identification.py
# (https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/parallel-run)
# for the implementation script.

import os
import numpy as np
import tensorflow as tf
from PIL import Image
from azureml.core import Model


def init():
    global g_tf_sess

    # Pull down the model from the workspace
    model_path = Model.get_model_path("mnist")

    # Construct a graph to execute
    tf.reset_default_graph()
    saver = tf.train.import_meta_graph(os.path.join(model_path, 'mnist-tf.model.meta'))
    g_tf_sess = tf.Session()
    saver.restore(g_tf_sess, os.path.join(model_path, 'mnist-tf.model'))


def run(mini_batch):
    print(f'run method start: {__file__}, run({mini_batch})')
    resultList = []
    in_tensor = g_tf_sess.graph.get_tensor_by_name("network/X:0")
    output = g_tf_sess.graph.get_tensor_by_name("network/output/MatMul:0")

    for image in mini_batch:
        # Prepare each image
        data = Image.open(image)
        np_im = np.array(data).reshape((1, 784))
        # Perform inference
        inference_result = output.eval(feed_dict={in_tensor: np_im}, session=g_tf_sess)
        # Find the best probability, and add it to the result list
        best_result = np.argmax(inference_result)
        resultList.append("{}: {}".format(os.path.basename(image), best_result))

    return resultList
```

Çıkarım betiğinizle aynı dizinde başka bir dosya veya klasörünüz varsa, geçerli çalışma dizinini bularak buna başvurabilirsiniz.

```python
script_dir = os.path.realpath(os.path.join(__file__, '..',))
file_path = os.path.join(script_dir, "<file_name>")
```

## <a name="build-and-run-the-pipeline-containing-parallelrunstep"></a>ParallelRunStep içeren işlem hattını derleyin ve çalıştırın

Artık ihtiyacınız olan her şeye sahipsiniz: veri girişleri, model, çıktı ve çıkarım betiğinizin. ParallelRunStep içeren Batch çıkarımı ardışık düzeni oluşturalım.

### <a name="prepare-the-environment"></a>Ortamı hazırlama

İlk olarak, betiğinizin bağımlılıklarını belirtin. Bunun yapılması, PIP paketleri yüklemenize ve ortamı yapılandırmanıza olanak tanır.

Her zaman, PIP paketi listesine **azureml-Core** ve **azureml-DataSet-Runtime [Pandas, sigortası]** dahil edin. Özel bir Docker görüntüsü (user_managed_dependencies = true) kullanırsanız Ayrıca, Conda yüklemiş olmanız gerekir.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

batch_conda_deps = CondaDependencies.create(pip_packages=["tensorflow==1.15.2", "pillow", 
                                                          "azureml-core", "azureml-dataset-runtime[pandas, fuse]"])

batch_env = Environment(name="batch_environment")
batch_env.python.conda_dependencies = batch_conda_deps
batch_env.docker.enabled = True
batch_env.docker.base_image = DEFAULT_GPU_IMAGE
```

### <a name="specify-the-parameters-using-parallelrunconfig"></a>ParallelRunConfig kullanarak parametreleri belirtme

`ParallelRunConfig` , `ParallelRunStep` Azure Machine Learning işlem hattının içinde örnek için önemli bir yapılandırmadır. Komut dosyanızı kaydırmak ve aşağıdaki girdilerin tümü de dahil olmak üzere gerekli parametreleri yapılandırmak için kullanın:
- `entry_script`: Birden çok düğümde paralel olarak çalıştırılacak yerel dosya yolu olarak bir Kullanıcı betiği. Varsa `source_directory` , göreli bir yol kullanın. Aksi takdirde, makinede erişilebilen herhangi bir yolu kullanın.
- `mini_batch_size`: Tek bir çağrıya geçirilen mini toplu iş boyutu `run()` . (isteğe bağlı; varsayılan değer `10` `FileDataset` ve `1MB` için dosyalarıdır `TabularDataset` .)
    - İçin `FileDataset` , en az değeri olan dosya sayısıdır `1` . Birden çok dosyayı tek bir mini toplu işte birleştirebilirsiniz.
    - İçin `TabularDataset` , verilerin boyutudur. Örnek değerler şunlardır,, `1024` `1024KB` `10MB` ve `1GB` . Önerilen değer `1MB` . Mini toplu iş, `TabularDataset` hiçbir zamanı çapraz dosya sınırlarına sahip olmayacaktır. Örneğin, çeşitli boyutlarda. csv dosyalarınız varsa en küçük dosya 100 KB 'tır ve en büyük değer 10 MB 'tır. Ayarlarsanız `mini_batch_size = 1MB` , boyutu 1 MB 'tan küçük olan dosyalar bir mini toplu işlem olarak kabul edilir. Boyutu 1 MB 'tan büyük olan dosyalar birden çok mini toplu iş içine bölünür.
- `error_threshold`: `TabularDataset` `FileDataset` İşlem sırasında yok sayılacak olması gereken için kayıt hatalarının ve dosya hatalarının sayısı. Tüm girdinin hata sayısı bu değerin üzerine gittiğinde, iş iptal edilir. Hata eşiği, yönteme gönderilen tek bir mini toplu iş için değil, tüm giriş içindir `run()` . Aralık `[-1, int.max]` . `-1`Bölüm, işlem sırasında tüm hataların yoksayıyor olduğunu gösterir.
- `output_action`: Aşağıdaki değerlerden biri çıktının nasıl düzenleneceğini gösterir:
    - `summary_only`: Kullanıcı betiği çıktıyı depolayacaktır. `ParallelRunStep` yalnızca hata eşiği hesaplaması için çıktıyı kullanır.
    - `append_row`: Tüm girişler için, Line ile ayrılmış tüm çıktıları eklemek için çıkış klasöründe yalnızca bir dosya oluşturulur.
- `append_row_file_name`: Append_row output_action için çıkış dosyası adını özelleştirmek için (isteğe bağlı; varsayılan değer `parallel_run_step.txt` ).
- `source_directory`: İşlem hedefinde yürütülecek tüm dosyaları içeren klasörlere yönelik yollar (isteğe bağlı).
- `compute_target`: Yalnızca `AmlCompute` desteklenir.
- `node_count`: Kullanıcı betiğini çalıştırmak için kullanılacak işlem düğümlerinin sayısı.
- `process_count_per_node`: Düğüm başına işlem sayısı. En iyi yöntem, GPU sayısına veya CPU 'ya bir düğüm (isteğe bağlı; varsayılan değer) göre ayarlanmalıdır `1` .
- `environment`: Python ortam tanımı. Bunu, mevcut bir Python ortamını kullanacak şekilde veya geçici bir ortam ayarlamak için yapılandırabilirsiniz. Tanım ayrıca gerekli uygulama bağımlılıklarını ayarlamaktan de sorumludur (isteğe bağlı).
- `logging_level`: Günlük ayrıntı düzeyi. Artan ayrıntı değerleri: `WARNING` , `INFO` , ve `DEBUG` . (isteğe bağlı; varsayılan değer `INFO` )
- `run_invocation_timeout`: `run()` Saniye cinsinden Yöntem çağırma zaman aşımı. (isteğe bağlı; varsayılan değer `60` )
- `run_max_try`: `run()` Bir mini toplu iş için deneme sayısı üst sınırı. Bir `run()` özel durum oluşursa bir hata oluşur veya ulaşıldığında hiçbir şey döndürülmez `run_invocation_timeout` (isteğe bağlı; varsayılan değer `3` ). 

İşlem hattı çalıştırmasını yeniden gönderdiğinizde,,,,, `mini_batch_size` `node_count` ve olarak belirtebilirsiniz, `process_count_per_node` `logging_level` `run_invocation_timeout` `run_max_try` `PipelineParameter` Bu sayede parametre değerleri üzerinde ince ayar yapabilirsiniz. Bu örnekte, `PipelineParameter` ve için kullanırsınız `mini_batch_size` `Process_count_per_node` ve daha sonra bir çalıştırmayı yeniden gönderdiğinizde bu değerleri değiştirirsiniz. 

Bu örnek, `digit_identification.py` daha önce ele alınan betiği kullandığınızı varsayar. Kendi komut dosyanızı kullanırsanız, `source_directory` ve `entry_script` parametrelerini uygun şekilde değiştirin.

```python
from azureml.pipeline.core import PipelineParameter
from azureml.pipeline.steps import ParallelRunConfig

parallel_run_config = ParallelRunConfig(
    source_directory='.',
    entry_script="digit_identification.py",
    mini_batch_size=PipelineParameter(name="batch_size_param", default_value="5"),
    error_threshold=10,
    output_action="append_row",
    append_row_file_name="mnist_outputs.txt",
    environment=batch_env,
    compute_target=compute_target,
    process_count_per_node=PipelineParameter(name="process_count_param", default_value=2),
    node_count=2)
```

### <a name="create-the-parallelrunstep"></a>ParallelRunStep oluşturma

Betiği, ortam yapılandırması ve parametreleri kullanarak ParallelRunStep öğesini oluşturun. Çalışma alanınıza zaten eklediğiniz işlem hedefini, çıkarım betiğinizin yürütme hedefi olarak belirtin. `ParallelRunStep`Aşağıdaki parametreleri alan toplu çıkarım ardışık düzen adımını oluşturmak için kullanın:
- `name`: Adım adı, şu adlandırma kısıtlamalarına sahip: benzersiz, 3-32 karakter ve Regex ^ \[ a-z \] ([-a-Z0-9] * [a-Z0-9])? $.
- `parallel_run_config`: `ParallelRunConfig` Daha önce tanımlanan bir nesne.
- `inputs`: Bir veya daha fazla tek tür veri kümesi paralel işleme için bölümlenecek Azure Machine Learning.
- `side_inputs`: Bir veya daha fazla başvuru verisi veya yan giriş olarak kullanılan veri kümelerinin bölümlenmiş olması gerekmez.
- `output`: `PipelineData` Çıkış dizinine karşılık gelen bir nesne.
- `arguments`: Kullanıcı betiğine geçirilen bağımsız değişkenlerin bir listesi. Giriş betiğinizi (isteğe bağlı) almak için unknown_args kullanın.
- `allow_reuse`: Adımın, aynı ayarlarla/girişlerle çalıştırıldığında önceki sonuçları yeniden kullanıp kullanmayacağını belirtir. Bu parametre ise `False` , işlem hattı yürütmesi sırasında bu adım için her zaman yeni bir çalıştırma oluşturulacaktır. (isteğe bağlı; varsayılan değer `True` .)

```python
from azureml.pipeline.steps import ParallelRunStep

parallelrun_step = ParallelRunStep(
    name="predict-digits-mnist",
    parallel_run_config=parallel_run_config,
    inputs=[input_mnist_ds_consumption],
    output=output_dir,
    allow_reuse=True
)
```
### <a name="create-and-run-the-pipeline"></a>İşlem hattını oluşturma ve çalıştırma

Şimdi işlem hattını çalıştırın. İlk olarak, [`Pipeline`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) çalışma alanı başvurusunu ve oluşturduğunuz işlem hattı adımını kullanarak bir nesne oluşturun. `steps`Parametresi bir adım dizisidir. Bu durumda, toplu iş çıkarımı için yalnızca bir adım vardır. Birden çok adım içeren işlem hatları oluşturmak için, adımları Bu dizide sırayla yerleştirin.

Sonra, işlem hattını `Experiment.submit()` yürütmeye göndermek için işlevini kullanın.

```python
from azureml.pipeline.core import Pipeline
from azureml.core.experiment import Experiment

pipeline = Pipeline(workspace=ws, steps=[parallelrun_step])
experiment = Experiment(ws, 'digit_identification')
pipeline_run = experiment.submit(pipeline)
```

## <a name="monitor-the-batch-inference-job"></a>Toplu çıkarım işini izleme

Bir toplu çıkarım işinin tamamlanması uzun zaman alabilir. Bu örnek, bir Jupyıter pencere öğesi kullanarak ilerlemeyi izler. Ayrıca şunları kullanarak işin ilerlemesini izleyebilirsiniz:

* Azure Machine Learning Studio. 
* Nesneden konsol çıktısı [`PipelineRun`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.run.pipelinerun?view=azure-ml-py) .

```python
from azureml.widgets import RunDetails
RunDetails(pipeline_run).show()

pipeline_run.wait_for_completion(show_output=True)
```

## <a name="resubmit-a-run-with-new-data-inputs-and-parameters"></a>Yeni veri girişleri ve parametreleriyle bir çalışmayı yeniden gönderin

Girişleri ve birkaç yapılandırmayı yaparken `PipelineParameter` , farklı bir veri kümesi girişi ile toplu çıkarım çalıştırmasını yeniden gönderebilir ve tamamen yeni bir işlem hattı oluşturmak zorunda kalmadan parametreleri hassas bir şekilde ayarlayabilirsiniz. Aynı veri deposunu kullanacaksınız ancak yalnızca tek bir görüntüyü veri girişi olarak kullanacaksınız.

```python
path_on_datastore = mnist_blob.path('mnist/0.png')
single_image_ds = Dataset.File.from_files(path=path_on_datastore, validate=False)

pipeline_run_2 = experiment.submit(pipeline, 
                                   pipeline_parameters={"mnist_param": single_image_ds, 
                                                        "batch_size_param": "1",
                                                        "process_count_param": 1}
)

pipeline_run_2.wait_for_completion(show_output=True)
```
## <a name="view-the-results"></a>Sonuçları görüntüleme

Yukarıdaki çalıştırmanın sonuçları `DataStore` nesnede belirtilen şekilde, `PipelineData` Bu durumda *ını*olarak adlandırılan çıkış verileri olarak yazılır. Sonuçlar varsayılan blob kapsayıcısında depolanır, depolama hesabınıza gidebilir ve Depolama Gezgini aracılığıyla görüntüleyebilirsiniz, dosya yolu azureml-BlobStore-*GUID*/azureml/*RunId* / *output_dir*olur.

Sonuçları görüntülemek için bu verileri de indirebilirsiniz. İlk 10 satırı görüntülemek için örnek kod aşağıda verilmiştir.

```python
import pandas as pd
import tempfile

batch_run = pipeline_run.find_step_run(parallelrun_step.name)[0]
batch_output = batch_run.get_output_data(output_dir.name)

target_dir = tempfile.mkdtemp()
batch_output.download(local_path=target_dir)
result_file = os.path.join(target_dir, batch_output.path_on_datastore, parallel_run_config.append_row_file_name)

df = pd.read_csv(result_file, delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
print("Prediction has ", df.shape[0], " rows")
df.head(10) 
```

## <a name="next-steps"></a>Sonraki adımlar

Bu işlemin sona erdirmek için, [toplu çıkarım Not defterini](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/parallel-run)deneyin. 

ParallelRunStep için hata ayıklama ve sorun giderme kılavuzu için bkz. [nasıl yapılır Kılavuzu](how-to-debug-parallel-run-step.md).

İşlem hatları için hata ayıklama ve sorun giderme kılavuzu için bkz. [nasıl yapılır Kılavuzu](how-to-debug-pipelines.md).

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

