---
title: Büyük verilerde toplu tahminleri Çalıştır
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ' de ParallelRunStep kullanarak büyük miktarlarda veriyi zaman uyumsuz olarak nasıl alabileceğinizi öğrenin. ParallelRunStep, büyük veri kullanım durumları için, paralel işleme özelliklerini kutudan çıkar ve yüksek aktarım hızı, yangın ve unutma çıkarımı için optimize eder.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: vaidyas
author: vaidya-s
ms.date: 01/15/2020
ms.custom: Ignite2019
ms.openlocfilehash: 3d283d1094336b928869aa281b4a640d7a62dd94
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79477196"
---
# <a name="run-batch-inference-on-large-amounts-of-data-by-using-azure-machine-learning"></a>Azure Machine Learning kullanarak büyük miktarlarda veri üzerinde toplu çıkarımı çalıştırın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning kullanarak, büyük miktarlarda verileri zaman uyumsuz ve paralel olarak işleme hakkında bilgi edinin. Burada açıklanan ParallelRunStep özelliği genel önizlemeye sunuldu. Bu, anahtarların ve işlem verilerinin işlenmesi için yüksek performanslı ve yüksek maliyetli bir yoldur. Bu, kutudan zaman uyumsuz yetenekler sağlar.

ParallelRunStep sayesinde, çevrimdışı ınmalların terabaytlarca üretim verilerinde büyük miktarda makinenin ölçeğini artırmak, daha fazla üretkenlik ve iyileştirilmiş maliyet elde etmek kolaydır.

Bu makalede, aşağıdaki görevleri öğreneceksiniz:

> * Uzaktan işlem kaynağı oluşturun.
> * Özel bir çıkarım betiği yazın.
> * Bir [makine öğrenme işlem hattı](concept-ml-pipelines.md) [oluşturun ve daha](https://publicdataset.azurewebsites.net/dataDetail/mnist/) önce eğitimli bir görüntü sınıflandırma modelini, veri kümesine göre kaydedin. 
> * Azure Blob depolama hesabınızda bulunan örnek görüntülerde toplu çıkarımı çalıştırmak için modeli kullanın. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* Bir Azure Machine Learning çalışma alanınız veya Not defteri sanal makineniz yoksa, kılavuzlu bir hızlı başlangıç için [Kurulum öğreticisini](tutorial-1st-experiment-sdk-setup.md) doldurun. 

* Kendi ortamınızı ve bağımlılıklarınızı yönetmek için kendi ortamınızı yapılandırma hakkında [nasıl yapılır Kılavuzu](how-to-configure-environment.md) ' na bakın. Gerekli `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-contrib-pipeline-steps` bağımlılıkları indirmek için ortamınızda çalıştırın.

## <a name="set-up-machine-learning-resources"></a>Makine öğrenimi kaynaklarını ayarlama

Aşağıdaki eylemler bir toplu çıkarım ardışık düzeni çalıştırmak için ihtiyacınız olan kaynakları ayarlar:

- Çıkarımını görüntülere sahip bir blob kapsayıcısını işaret eden bir veri deposu oluşturun.
- Toplu çıkarım ardışık düzen adımı için veri başvurularını giriş ve çıkış olarak ayarlayın.
- Yığın çıkarımı adımını çalıştırmak için bir işlem kümesi ayarlayın.

### <a name="create-a-datastore-with-sample-images"></a>Örnek görüntülerle bir veri deposu oluşturma

Ortak blob kapsayıcısından `sampledata` , adlı `pipelinedata`BIR hesapta bulunan mnist değerlendirmesi kümesini alın. Bu kapsayıcıya işaret eden ada `mnist_datastore`sahip bir veri deposu oluşturun. Aşağıdaki çağrısında `register_azure_blob_container`, `overwrite` bayrağını, daha önce bu adla oluşturulmuş `True` herhangi bir veri deposundaki üzerine yazacak şekilde ayarlamak. 

, `datastore_name` `container_name`Ve `account_name`için kendi değerlerinizi sağlayarak bu adımı blob kapsayıcınızı işaret etmek üzere değiştirebilirsiniz.

```python
from azureml.core import Datastore
from azureml.core import Workspace

# Load workspace authorization details from config.json
ws = Workspace.from_config()

mnist_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="mnist_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata",
                      overwrite=True)
```

Ardından, çıkış veri deposu olarak çalışma alanı varsayılan veri deposunu belirtin. Çıkarımı çıkışı için kullanacaksınız.

Çalışma alanınızı oluşturduğunuzda, [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [BLOB depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) alanı varsayılan olarak çalışma alanına eklenir. Azure dosyaları bir çalışma alanı için varsayılan veri depo, ancak blob depolamayı da bir veri deposu olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Storage seçenekleri](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-inputs-and-outputs"></a>Veri girişlerini ve çıkışları yapılandırma

Artık aşağıdakiler dahil olmak üzere veri girişlerini ve çıkışları yapılandırmanız gerekir:

- Girdi görüntülerini içeren dizin.
- Önceden eğitilen modelin depolandığı dizin.
- Etiketleri içeren dizin.
- Çıkış dizini.

[`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py)Azure Machine Learning verileri keşfetmek, dönüştürmek ve yönetmek için bir sınıftır. Bu sınıfta iki tür vardır: [`TabularDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) ve [`FileDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py). Bu örnekte, Batch çıkarım ardışık `FileDataset` düzen adımının girdileri olarak kullanacaksınız. 

> [!NOTE] 
> `FileDataset`Batch çıkarımı içindeki destek şimdilik Azure Blob depolama ile kısıtlıdır. 

Ayrıca, özel çıkarım betiğinizdeki diğer veri kümelerine de başvurabilirsiniz. Örneğin, ve `Dataset.register` `Dataset.get_by_name`kullanarak resimleri etiketlemek için betiğinizdeki etiketlere erişmek için kullanabilirsiniz.

Azure Machine Learning veri kümeleri hakkında daha fazla bilgi için bkz. [veri kümeleri oluşturma ve erişim (Önizleme)](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets).

[`PipelineData`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py)nesneler, işlem hattı adımları arasında ara verileri aktarmak için kullanılır. Bu örnekte, çıkarım çıktıları için kullanacaksınız.

```python
from azureml.core.dataset import Dataset

mnist_ds_name = 'mnist_sample_data'

path_on_datastore = mnist_blob.path('mnist/')
input_mnist_ds = Dataset.File.from_files(path=path_on_datastore, validate=False)
registered_mnist_ds = input_mnist_ds.register(ws, mnist_ds_name, create_new_version=True)
named_mnist_ds = registered_mnist_ds.as_named_input(mnist_ds_name)

output_dir = PipelineData(name="inferences", 
                          datastore=def_data_store, 
                          output_path_on_compute="mnist/results")
```

### <a name="set-up-a-compute-target"></a>İşlem hedefi ayarlama

Azure Machine Learning, *işlem* (veya *işlem hedefi*), makine öğrenimi ardışık düzeninde hesaplama adımlarını gerçekleştiren makinelere veya kümelere başvurur. CPU tabanlı bir [Amlcompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) hedefi oluşturmak için aşağıdaki kodu çalıştırın.

```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpu-cluster")
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

Ardından, uzak işlem kaynağınız tarafından kullanılabilmesi için modeli çalışma alanınıza kaydedin.

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
>Aşağıdaki kod, [örnek Not defterinin](https://aka.ms/batch-inference-notebooks) kullandığı bir örnektir. Senaryonuz için kendi komut dosyanızı oluşturmanız gerekir.

Betik iki işlev *içermelidir* :
- `init()`: Bu işlevi, daha sonraki çıkarım için pahalı veya genel hazırlık için kullanın. Örneğin, modeli genel bir nesneye yüklemek için kullanın. Bu işlev, işlem başlangıcında yalnızca bir kez çağrılır.
-  `run(mini_batch)`: İşlev her `mini_batch` örnek için çalışacaktır.
    -  `mini_batch`: Paralel çalıştırma adımı Run metodunu çağırır ve bir liste ya da Pandas DataFrame 'i yönteme bağımsız değişken olarak geçirmeyecektir. Giriş bir TabularDataset ise, min_batch içindeki her giriş bir dosya yolu olur.
    -  `response`: Run () yöntemi bir Pandas DataFrame veya Array döndürmelidir. Append_row output_action için, döndürülen bu öğeler ortak çıkış dosyasına eklenir. Summary_only için öğelerin içeriği yok sayılır. Tüm çıkış eylemleri için, döndürülen her çıkış öğesi girdi öğesinin giriş mini Batch 'de başarılı bir şekilde çalıştırıldığını belirtir. Girişi çalışacak şekilde eşlemek için, çalıştırma sonuçlarına yeterli miktarda veri eklendiğinden emin olmanız gerekir. Çalıştırma çıkışı çıkış dosyasında yazılır ve bu sırada olması garanti edilmez, çıktıda bir anahtarı, girişle eşlemek için kullanmalısınız.

```python
# Snippets from a sample script.
# Refer to the accompanying digit_identification.py
# (https://aka.ms/batch-inference-notebooks)
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

### <a name="how-to-access-other-files-in-source-directory-in-entry_script"></a>Entry_script kaynak dizinindeki diğer dosyalara erişme

Giriş betiğiyle aynı dizinde başka bir dosya veya klasörünüz varsa, geçerli çalışma dizinini bularak buna başvurabilirsiniz.

```python
script_dir = os.path.realpath(os.path.join(__file__, '..',))
file_path = os.path.join(script_dir, "<file_name>")
```

## <a name="build-and-run-the-pipeline-containing-parallelrunstep"></a>ParallelRunStep içeren işlem hattını derleyin ve çalıştırın

Artık işlem hattını oluşturmak için ihtiyacınız olan her şeye sahipsiniz.

### <a name="prepare-the-run-environment"></a>Çalıştırma ortamını hazırlama

İlk olarak, betiğinizin bağımlılıklarını belirtin. Bu nesneyi daha sonra işlem hattı adımını oluştururken kullanırsınız.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

batch_conda_deps = CondaDependencies.create(pip_packages=["tensorflow==1.13.1", "pillow"])

batch_env = Environment(name="batch_environment")
batch_env.python.conda_dependencies = batch_conda_deps
batch_env.docker.enabled = True
batch_env.docker.base_image = DEFAULT_GPU_IMAGE
batch_env.spark.precache_packages = False
```

### <a name="specify-the-parameters-for-your-batch-inference-pipeline-step"></a>Toplu çıkarım ardışık düzen adımınızda parametreleri belirtin

`ParallelRunConfig`, Yeni tanıtılan toplu çıkarım `ParallelRunStep` örneği için Azure Machine Learning işlem hattı içinde ana yapılandırmadır. Komut dosyanızı kaydırmak ve aşağıdaki parametrelerin tümü de dahil olmak üzere gerekli parametreleri yapılandırmak için kullanın:
- `entry_script`: Birden çok düğümde paralel olarak çalıştırılacak yerel dosya yolu olarak bir Kullanıcı betiği. Varsa `source_directory` , göreli bir yol kullanın. Aksi takdirde, makinede erişilebilen herhangi bir yolu kullanın.
- `mini_batch_size`: Tek `run()` bir çağrıya geçirilen mini toplu iş boyutu. (isteğe bağlı; varsayılan değer, `10` filedataset ve `1MB` tabulardataset için dosyalardır.)
    - İçin `FileDataset`, en az değeri olan dosya sayısıdır `1`. Birden çok dosyayı tek bir mini toplu işte birleştirebilirsiniz.
    - İçin `TabularDataset`, verilerin boyutudur. Örnek değerler şunlardır `1024` `1024KB` `10MB`,, ve `1GB`. Önerilen değer `1MB`. Mini toplu iş, hiçbir `TabularDataset` zamanı çapraz dosya sınırlarına sahip olmayacaktır. Örneğin, çeşitli boyutlarda. csv dosyalarınız varsa en küçük dosya 100 KB 'tır ve en büyük değer 10 MB 'tır. Ayarlarsanız `mini_batch_size = 1MB`, boyutu 1 MB 'tan küçük olan dosyalar bir mini toplu işlem olarak kabul edilir. Boyutu 1 MB 'tan büyük olan dosyalar birden çok mini toplu iş içine bölünür.
- `error_threshold`: İşlem sırasında yok sayılacak olması gereken `TabularDataset` için kayıt hatalarının ve `FileDataset` dosya hatalarının sayısı. Tüm girdinin hata sayısı bu değerin üzerine gittiğinde, iş iptal edilir. Hata eşiği, `run()` yönteme gönderilen tek bir mini toplu iş için değil, tüm giriş içindir. Aralık `[-1, int.max]`. Bölüm `-1` , işlem sırasında tüm hataların yoksayıyor olduğunu gösterir.
- `output_action`: Aşağıdaki değerlerden biri çıktının nasıl düzenleneceğini gösterir:
    - `summary_only`: Kullanıcı betiği çıktıyı depolayacaktır. `ParallelRunStep`yalnızca hata eşiği hesaplaması için çıktıyı kullanır.
    - `append_row`: Tüm giriş dosyaları için, Line ile ayrılmış tüm çıktıları eklemek için çıkış klasöründe yalnızca bir dosya oluşturulur. Dosya adı olacaktır `parallel_run_step.txt`.
- `source_directory`: İşlem hedefinde yürütülecek tüm dosyaları içeren klasörlere yönelik yollar (isteğe bağlı).
- `compute_target`: Yalnızca `AmlCompute` desteklenir.
- `node_count`: Kullanıcı betiğini çalıştırmak için kullanılacak işlem düğümlerinin sayısı.
- `process_count_per_node`: Düğüm başına işlem sayısı.
- `environment`: Python ortam tanımı. Bunu mevcut bir Python ortamını kullanacak şekilde yapılandırabilir veya deneme için geçici bir ortam ayarlayabilirsiniz. Tanım ayrıca gerekli uygulama bağımlılıklarını ayarlamaktan de sorumludur (isteğe bağlı).
- `logging_level`: Günlük ayrıntı düzeyi. Artan ayrıntı değerleri: `WARNING`, `INFO`, ve. `DEBUG` (isteğe bağlı; varsayılan değer `INFO`)
- `run_invocation_timeout`: Saniye `run()` cinsinden Yöntem çağırma zaman aşımı. (isteğe bağlı; varsayılan değer `60`)

```python
from azureml.contrib.pipeline.steps import ParallelRunConfig

parallel_run_config = ParallelRunConfig(
    source_directory=scripts_folder,
    entry_script="digit_identification.py",
    mini_batch_size="5",
    error_threshold=10,
    output_action="append_row",
    environment=batch_env,
    compute_target=compute_target,
    node_count=4)
```

### <a name="create-the-pipeline-step"></a>İşlem hattı adımını oluşturma

Komut dosyası, ortam yapılandırması ve parametreleri kullanarak işlem hattı adımını oluşturun. Komut dosyası için yürütme hedefi olarak çalışma alanınıza zaten iliştirtiğiniz işlem hedefini belirtin. Aşağıdaki `ParallelRunStep` parametreleri alan toplu çıkarım ardışık düzen adımını oluşturmak için kullanın:
- `name`: Adım adı, şu adlandırma kısıtlamalarına sahip: benzersiz, 3-32 karakter ve Regex ^\[a-z\]([-a-Z0-9] * [a-Z0-9])? $.
- `models`: Azure Machine Learning model kayıt defterine sıfır veya daha fazla model adı zaten kaydedilmiş.
- `parallel_run_config`: Daha `ParallelRunConfig` önce tanımlanan bir nesne.
- `inputs`: Bir veya daha fazla tek oluşturulmuş Azure Machine Learning veri kümesi.
- `output`: Çıkış `PipelineData` dizinine karşılık gelen bir nesne.
- `arguments`: Kullanıcı betiğine geçirilen bağımsız değişkenlerin listesi (isteğe bağlı).
- `allow_reuse`: Adımın, aynı ayarlarla/girişlerle çalıştırıldığında önceki sonuçları yeniden kullanıp kullanmayacağını belirtir. Bu parametre ise, `False`işlem hattı yürütmesi sırasında bu adım için her zaman yeni bir çalıştırma oluşturulacaktır. (isteğe bağlı; varsayılan değer `True`.)

```python
from azureml.contrib.pipeline.steps import ParallelRunStep

parallelrun_step = ParallelRunStep(
    name="batch-mnist",
    models=[model],
    parallel_run_config=parallel_run_config,
    inputs=[named_mnist_ds],
    output=output_dir,
    arguments=[],
    allow_reuse=True
)
```

>[!Note]
> Yukarıdaki adım `azureml-contrib-pipeline-steps`, [Önkoşullar](#prerequisites)bölümünde açıklandığı gibi bağımlıdır. 

### <a name="submit-the-pipeline"></a>İşlem hattını gönderme

Şimdi işlem hattını çalıştırın. İlk olarak, çalışma [`Pipeline`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) alanı başvurusunu ve oluşturduğunuz işlem hattı adımını kullanarak bir nesne oluşturun. `steps` Parametresi bir adım dizisidir. Bu durumda, toplu Puanlama için yalnızca bir adım vardır. Birden çok adım içeren işlem hatları oluşturmak için, adımları Bu dizide sırayla yerleştirin.

Sonra, işlem hattını `Experiment.submit()` yürütmeye göndermek için işlevini kullanın.

```python
from azureml.pipeline.core import Pipeline
from azureml.core.experiment import Experiment

pipeline = Pipeline(workspace=ws, steps=[parallelrun_step])
pipeline_run = Experiment(ws, 'digit_identification').submit(pipeline)
```

## <a name="monitor-the-parallel-run-job"></a>Paralel çalıştırma işini izleme

Bir toplu çıkarım işinin tamamlanması uzun zaman alabilir. Bu örnek, bir Jupyıter pencere öğesi kullanarak ilerlemeyi izler. Ayrıca şunları kullanarak işin ilerlemesini yönetebilirsiniz:

* Azure Machine Learning Studio. 
* [`PipelineRun`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.run.pipelinerun?view=azure-ml-py) Nesneden konsol çıktısı.

```python
from azureml.widgets import RunDetails
RunDetails(pipeline_run).show()

pipeline_run.wait_for_completion(show_output=True)
```

## <a name="next-steps"></a>Sonraki adımlar

Bu işlemin sona erdirmek için, [toplu çıkarım Not defterini](https://aka.ms/batch-inference-notebooks)deneyin. 

ParallelRunStep için hata ayıklama ve sorun giderme kılavuzu için bkz. [nasıl yapılır Kılavuzu](how-to-debug-parallel-run-step.md).

İşlem hatları için hata ayıklama ve sorun giderme kılavuzu için bkz. [nasıl yapılır Kılavuzu](how-to-debug-pipelines.md).

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

