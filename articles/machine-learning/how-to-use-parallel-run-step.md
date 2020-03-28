---
title: Büyük verilerde toplu tahmin yürütme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de ParallelRunStep'i kullanarak büyük miktarda veriyle ilgili çıkarımları nasıl eşit olarak elde edebilirsiniz öğrenin. ParallelRunStep kutunun dışında paralel işleme yetenekleri sağlar ve büyük veri kullanım örnekleri için yüksek iş letim, yangın ve unut çıkarımı için optimize eder.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79477196"
---
# <a name="run-batch-inference-on-large-amounts-of-data-by-using-azure-machine-learning"></a>Azure Machine Learning'i kullanarak büyük miktarda veri üzerinde toplu çıkarım çalıştırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning'i kullanarak büyük miktarda veriyi eşit ve paralel olarak nasıl işleyip işleyip işleyerek öğrenin. Burada açıklanan ParallelRunStep özelliği genel önizlemededir. Çıkarımlar ve işleme verileri oluşturmak için yüksek performanslı ve yüksek iş yaratma yolu. Kutunun dışında eşzamanlı yetenekler sağlar.

ParallelRunStep ile, terabaytlardır üretim verilerindeki büyük makine kümelerine çevrimdışı çıkarımları ölçeklendirmek kolaydır ve bu da üretkenliğin ve optimize edilmiş maliyetin artmasıyla sonuçlanır.

Bu makalede, aşağıdaki görevleri öğrenirsiniz:

> * Uzak bir bilgi işlem kaynağı oluşturun.
> * Özel bir çıkarım komut dosyası yazın.
> * [MNIST](https://publicdataset.azurewebsites.net/dataDetail/mnist/) veri kümesini temel alan önceden eğitilmiş bir görüntü sınıflandırma modelini kaydetmek için bir [makine öğrenimi ardışık hattı](concept-ml-pipelines.md) oluşturun. 
> * Azure Blob depolama hesabınızda bulunan örnek resimlerde toplu çıkarım çalıştırmak için modeli kullanın. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* Kılavuzlu hızlı bir başlangıç için, azure machine learning çalışma alanınız veya dizüstü bilgisayarınız yoksa [kurulum eğitimini](tutorial-1st-experiment-sdk-setup.md) tamamlayın. 

* Kendi ortamınızı ve bağımlılıklarınızı yönetmek için, kendi ortamınızı yapılandırma konusunda [nasıl yapılacağınız kılavuzuna](how-to-configure-environment.md) bakın. Gerekli `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-contrib-pipeline-steps` bağımlılıkları indirmek için ortamınızda çalıştırın.

## <a name="set-up-machine-learning-resources"></a>Makine öğrenimi kaynaklarını ayarlama

Aşağıdaki eylemler, toplu iş çıkarımı ardışık hattını çalıştırmak için gereken kaynakları ayarlar:

- Çıkarım için görüntüleri olan bir blob kapsayıcıişaret eden bir veri deposu oluşturun.
- Toplu çıkarım ardışık işlem adımı için veri başvurularını girdi ve çıktı olarak ayarlayın.
- Toplu çıkarım adımını çalıştırmak için bir işlem kümesi ayarlayın.

### <a name="create-a-datastore-with-sample-images"></a>Örnek görüntülerle bir veri deposu oluşturma

MNIST değerlendirme kümesini ortak blob `sampledata` kapsayıcısından `pipelinedata`alın. Bu kapsayıcıyı işaret `mnist_datastore`eden adında bir veri deposu oluşturun. Aşağıdaki `register_azure_blob_container`çağrıda, `overwrite` bayrağı daha önce `True` bu adla oluşturulmuş herhangi bir veri deposunun üzerine yazmak üzere ayarlayın. 

Kendi değerlerinizi sağlayarak blob kabınızı işaret etmek için `datastore_name` `container_name`bu adımı `account_name`değiştirebilirsiniz.

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

Ardından, çıktı veri deposu olarak çalışma alanı varsayılan veri deposunu belirtin. Çıkarım çıktısı için kullanacaksın.

Çalışma alanınızı oluşturduğunuzda, [Azure Dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [Blob depolama alanı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) varsayılan olarak çalışma alanına eklenir. Azure Dosyaları bir çalışma alanı için varsayılan veri deposudur, ancak Blob depolamayı veri deposu olarak da kullanabilirsiniz. Daha fazla bilgi için [Azure depolama seçeneklerine](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)bakın.

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-inputs-and-outputs"></a>Veri giriş ve çıktılarını yapılandırma

Şimdi veri giriş ve çıktılarını yapılandırmanız gerekir, bunlar arasında:

- Giriş görüntülerini içeren dizin.
- Önceden eğitilmiş modelin depolandığı dizin.
- Etiketleri içeren dizin.
- Çıktı dizini.

[`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py)Azure Machine Learning'de verileri keşfetmek, dönüştürmek ve yönetmek için bir sınıftır. Bu sınıfın iki [`TabularDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) türü [`FileDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py)vardır: ve . Bu örnekte, toplu `FileDataset` çıkarım ardışık aşamaya giriş olarak kullanırsınız. 

> [!NOTE] 
> `FileDataset`toplu çıkarımdesteği şimdilik Azure Blob depolama alanıyla sınırlıdır. 

Özel çıkarım komut dosyanızda diğer veri kümeleri başvuru da yapabilirsiniz. Örneğin, kullanarak görüntüleri etiketlemek için komut dosyanızdaki etiketlere `Dataset.register` `Dataset.get_by_name`erişmek için kullanabilirsiniz ve.

Azure Machine Learning veri kümeleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets)

[`PipelineData`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py)nesneler, ara verileri satır başı adımları arasında aktarmak için kullanılır. Bu örnekte, çıkarım çıktıları için kullanabilirsiniz.

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

Azure Machine Learning'de, *işlem* (veya *bilgi işlem hedefi),* makine öğrenimi ardışık ardınızdaki hesaplama adımlarını gerçekleştiren makineleri veya kümeleri ifade eder. CPU tabanlı [AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) hedefi oluşturmak için aşağıdaki kodu çalıştırın.

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

[Önceden eğitilmiş görüntü sınıflandırma modelini indirin](https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz) `models` ve ardından dizine ayıklayın.

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

Ardından, uzaktan bilgi işlem kaynağınız tarafından kullanılabilecek şekilde modeli çalışma alanınızla kaydedin.

```python
from azureml.core.model import Model

# Register the downloaded model 
model = Model.register(model_path="models/",
                       model_name="mnist",
                       tags={'pretrained': "mnist"},
                       description="Mnist trained tensorflow model",
                       workspace=ws)
```

## <a name="write-your-inference-script"></a>Çıkarım komut dosyanızı yazın

>[!Warning]
>Aşağıdaki kod yalnızca [örnek not defterinin](https://aka.ms/batch-inference-notebooks) kullandığı bir örnektir. Senaryonuz için kendi komut dosyanızı oluşturmanız gerekir.

Komut dosyası iki işlev *içermelidir:*
- `init()`: Daha sonra çıkarım için herhangi bir pahalı veya ortak hazırlık için bu işlevi kullanın. Örneğin, modeli genel bir nesneye yüklemek için kullanın. Bu işlev, işlemin başlangıcında yalnızca bir kez çağrılacaktır.
-  `run(mini_batch)`: İşlev her `mini_batch` örnek için çalışacaktır.
    -  `mini_batch`: Paralel çalıştırma adımı çalıştır yöntemini çağırır ve yönteme bir bağımsız değişken olarak bir liste veya Pandas DataFrame geçirin. min_batch her giriş olacak - giriş bir FileDataset ise bir dosya yolu, giriş tabularDataset ise bir Pandas DataFrame.
    -  `response`: run() yöntemi bir Pandas DataFrame veya bir dizi döndürmelidir. append_row output_action için, bu döndürülen öğeler ortak çıktı dosyasına eklenir. summary_only için, öğelerin içeriği yoksayılır. Tüm çıktı eylemleri için, döndürülen her çıktı öğesi, giriş mini toplu iş giriş öğesinin başarılı bir çalışmasını gösterir. Sonucu çalıştırmak için giriş yapmak için çalışma sonucuna yeterli verinin dahil olduğundan emin olmalısınız. Run çıkışı çıktı dosyasına yazılır ve sırayla olması garanti edilmez, giriş için eşlemek için çıktıbazı anahtar kullanmanız gerekir.

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

### <a name="how-to-access-other-files-in-source-directory-in-entry_script"></a>entry_script'da kaynak dizindeki diğer dosyalara nasıl erişilir?

Giriş komut dosyanızla aynı dizinde başka bir dosya veya klasörünüz varsa, geçerli çalışma dizinini bularak dosyaya başvuruda bulunabilirsiniz.

```python
script_dir = os.path.realpath(os.path.join(__file__, '..',))
file_path = os.path.join(script_dir, "<file_name>")
```

## <a name="build-and-run-the-pipeline-containing-parallelrunstep"></a>ParallelRunStep içeren ardışık hattı oluşturun ve çalıştırın

Şimdi boru hattını inşa etmek için gereken her şeye sahipsin.

### <a name="prepare-the-run-environment"></a>Çalışma ortamını hazırlama

İlk olarak, komut dosyanızın bağımlılıklarını belirtin. Bu nesneyi daha sonra ardışık işlem adımı oluştururken kullanırsınız.

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

### <a name="specify-the-parameters-for-your-batch-inference-pipeline-step"></a>Toplu çıkarım boru hattı adımınızın parametrelerini belirtin

`ParallelRunConfig`Azure Machine Learning ardışık alt hatlar `ParallelRunStep` içinde yeni tanıtılan toplu çıkarım örneği için önemli bir yapılandırmadır. Komut dosyanızı sarmak ve aşağıdaki parametrelerin tümü de dahil olmak üzere gerekli parametreleri yapılandırmak için kullanırsınız:
- `entry_script`: Birden çok düğümüzerinde paralel olarak çalışacak yerel bir dosya yolu olarak kullanıcı komut dosyası. `source_directory` Varsa, göreli bir yol kullanın. Aksi takdirde, makinede erişilebilen herhangi bir yolu kullanın.
- `mini_batch_size`: Mini partinin boyutu tek `run()` bir çağrıya geçti. (isteğe bağlı olarak; varsayılan değer `1MB` FileDataset ve TabularDataset için dosyalardır.) `10`
    - Bunun `FileDataset`için, en az değeri olan dosya `1`sayısıdır. Birden çok dosyayı tek bir mini toplu iş halinde birleştirebilirsiniz.
    - Çünkü, `TabularDataset`bu veri boyutu. Örnek değerler `1024` `1024KB`, `10MB`, `1GB`, ve . Önerilen değer `1MB`. Gelen `TabularDataset` mini toplu dosya sınırlarını asla geçemez. Örneğin, çeşitli boyutlarda .csv dosyalarınız varsa, en küçük dosya 100 KB ve en büyüğü 10 MB'dır. Ayarlarsanız, `mini_batch_size = 1MB`boyutu 1 MB'dan küçük olan dosyalar bir mini toplu işlem olarak kabul edilir. Boyutu 1 MB'dan büyük olan dosyalar birden çok mini toplu iş yerine bölünür.
- `error_threshold`: Bunun için `TabularDataset` kayıt hataları ve dosya `FileDataset` hataları sayısı işleme sırasında göz ardı edilmelidir. Tüm giriş için hata sayısı bu değerin üzerine çıkarsa, iş iptal edilecektir. Hata eşiği, `run()` yönteme gönderilen tek tek mini toplu iş için değil, tüm giriş içindir. Aralık. `[-1, int.max]` Parça, `-1` işleme sırasında tüm hataları nyoksayan gösterir.
- `output_action`: Aşağıdaki değerlerden biri çıktının nasıl düzenleneceğini gösterir:
    - `summary_only`: Kullanıcı komut dosyası çıktıyı depolar. `ParallelRunStep`çıktıyı yalnızca hata eşiği hesaplaması için kullanır.
    - `append_row`: Tüm giriş dosyaları için, satırla ayrılan tüm çıktıları bir araya getirmek için çıktı klasöründe yalnızca bir dosya oluşturulur. Dosya `parallel_run_step.txt`adı.
- `source_directory`: İşlem hedefinde yürütülecek tüm dosyaları içeren klasörlere giden yollar (isteğe bağlı).
- `compute_target`: `AmlCompute` Sadece desteklenir.
- `node_count`: Kullanıcı komut dosyasının çalıştırılmasında kullanılacak işlem düğümlerinin sayısı.
- `process_count_per_node`: Düğüm başına işlem sayısı.
- `environment`: Python ortam tanımı. Varolan bir Python ortamını kullanacak veya deneme için geçici bir ortam ayarlayabilirsin. Tanım, gerekli uygulama bağımlılıklarını (isteğe bağlı) ayarlamaktan da sorumludur.
- `logging_level`: Günlük ayrıntılı. Artan ayrıntılılık değerleri şunlardır: `WARNING`, `INFO`, ve `DEBUG`. (isteğe bağlı; `INFO`varsayılan değer)
- `run_invocation_timeout`: `run()` Saniyeler içinde zaman alameti. (isteğe bağlı; `60`varsayılan değer)

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

### <a name="create-the-pipeline-step"></a>Boru hattı adımını oluşturma

Komut dosyasını, ortam yapılandırmasını ve parametreleri kullanarak ardışık yapı adımını oluşturun. Komut dosyasının yürütme hedefi olarak çalışma alanınıza zaten iliştirdiğiniz işlem hedefini belirtin. Aşağıdaki `ParallelRunStep` parametrelerin tümlerini alan toplu çıkarım ardışık adımını oluşturmak için kullanın:
- `name`: Adımın adı, aşağıdaki adlandırma kısıtlamaları ile: benzersiz, 3-32 karakter\[ve\]regex ^ a-z ([-a-z0-9]*[a-z0-9])?$.
- `models`: Azure Machine Learning model kayıt defterine kayıtlı sıfır veya daha fazla model adı.
- `parallel_run_config`: `ParallelRunConfig` Daha önce tanımlandığı gibi bir nesne.
- `inputs`: Bir veya daha fazla tek yazılı Azure Machine Learning veri kümeleri.
- `output`: `PipelineData` Çıktı dizine karşılık gelen bir nesne.
- `arguments`: Kullanıcı komut dosyasına (isteğe bağlı) geçirilen bağımsız değişkenlerin listesi.
- `allow_reuse`: Aynı ayarlar/girişlerle çalıştırıldığında adımın önceki sonuçları yeniden kullanıp kullanmaması. Bu parametre `False`ise, boru hattı yürütme sırasında bu adım için her zaman yeni bir çalışma oluşturulur. (isteğe bağlı; `True`varsayılan değer .)

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
> Yukarıdaki adım bağlıdır `azureml-contrib-pipeline-steps`, [Önkoşullar](#prerequisites)açıklandığı gibi . 

### <a name="submit-the-pipeline"></a>Boru hattını gönderme

Şimdi, boru hattını çalıştırın. İlk olarak, [`Pipeline`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) çalışma alanı başvurunuzu ve oluşturduğunuz ardışık hatlar adımını kullanarak bir nesne oluşturun. `steps` Parametre bir dizi adımdır. Bu durumda, toplu puanlama için yalnızca bir adım vardır. Birden çok adıma sahip ardışık hatlar oluşturmak için adımları bu dizide sırayla yerleştirin.

Ardından, yürütme `Experiment.submit()` için ardışık hatlar göndermek için işlevi kullanın.

```python
from azureml.pipeline.core import Pipeline
from azureml.core.experiment import Experiment

pipeline = Pipeline(workspace=ws, steps=[parallelrun_step])
pipeline_run = Experiment(ws, 'digit_identification').submit(pipeline)
```

## <a name="monitor-the-parallel-run-job"></a>Paralel çalıştırma işini izleme

Bir toplu çıkarım işi bitirmek için uzun bir zaman alabilir. Bu örnek, bir Jupyter widget kullanarak ilerleme izler. Ayrıca aşağıdakileri kullanarak işin ilerlemesini de yönetebilirsiniz:

* Azure Machine Learning Studio. 
* Nesneden [`PipelineRun`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.run.pipelinerun?view=azure-ml-py) konsol çıkışı.

```python
from azureml.widgets import RunDetails
RunDetails(pipeline_run).show()

pipeline_run.wait_for_completion(show_output=True)
```

## <a name="next-steps"></a>Sonraki adımlar

Bu işlemin uçuca çalıştığını görmek için [toplu çıkarım not defterini](https://aka.ms/batch-inference-notebooks)deneyin. 

ParallelRunStep için hata ayıklama ve sorun giderme kılavuzu için [nasıl yapılacağını kılavuzuna](how-to-debug-parallel-run-step.md)bakın.

Boru hatları için hata ayıklama ve sorun giderme kılavuzu için [nasıl yapılacağını zedilen kılavuzuna](how-to-debug-pipelines.md)bakın.

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

