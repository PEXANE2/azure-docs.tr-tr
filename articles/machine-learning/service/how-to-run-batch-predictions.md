---
title: Büyük veriler üzerinde toplu Öngörüler çalıştırın
titleSuffix: Azure Machine Learning service
description: Batch zaman uyumsuz olarak büyük miktarlarda verileri Azure Machine Learning hizmetini kullanarak tahminlerde bulunmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens, garye
ms.author: jordane
author: jpe316
ms.date: 07/12/2019
ms.openlocfilehash: 689ee003e0923a65d3ca3f2d13c1a2d05c299dbd
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358717"
---
# <a name="run-batch-predictions-on-large-data-sets-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile büyük veri kümelerinde toplu Öngörüler çalıştırma

Bu makalede, Azure Machine Learning hizmeti kullanarak zaman uyumsuz olarak büyük miktarlarda veri tahminlerinin nasıl yapılacağını öğreneceksiniz.

Toplu işlem Tahmini (veya toplu Puanlama), zaman uyumsuz uygulamalar için paralellik verimlilik ile uygun maliyetli çıkarım sağlar. Toplu tahmin işlem hatlarını çıkarımı terabaytlarca üretim veri gerçekleştirmek için ölçeklendirilebilir. Toplu tahmin, büyük bir veri koleksiyonu için yüksek aktarım hızı, yangın ve unutma tahminleri için iyileştirilmiştir.

>[!TIP]
> Sisteminiz düşük gecikmeli işleme gerektiriyorsa (tek bir belgeyi veya küçük bir belge kümesini hızlı bir şekilde işlemek için), toplu tahmin yerine [gerçek zamanlı Puanlama](how-to-consume-web-service.md) kullanın.

Aşağıdaki adımlarda, önceden eğitilen bir bilgisayar Vision modelini ([Inception-v3](https://arxiv.org/abs/1512.00567)) kaydetmek için bir [makine öğrenimi işlem hattı](concept-ml-pipelines.md) oluşturacaksınız. Daha sonra, Azure Blob depolama hesabınızda bulunan görüntülerde toplu Puanlama yapmak için önceden eğitilen modeli kullanın. Puanlama için kullanılan görüntülerin etiketlenmemiş gelen görüntüleri [Imagenet](http://image-net.org/) veri kümesi.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning hizmetinin ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- Azure Machine Learning SDK'sını yüklemek için geliştirme ortamınızı yapılandırın. Daha fazla bilgi için [bir geliştirme ortamı yapılandırmak için Azure Machine Learning](how-to-configure-environment.md).

- Tüm işlem hattı kaynakları barındıracak bir Azure Machine Learning çalışma alanı oluşturun. Aşağıdaki kodu kullanın veya daha fazla seçenek için bkz: [çalışma alanı yapılandırma dosyası oluşturma](how-to-configure-environment.md#workspace).

  ```python
  from azureml.core import Workspace
  ws = Workspace.create(name = '<workspace-name>',
                        subscription_id = '<subscription-id>',
                        resource_group = '<resource-group>',
                        location = '<workspace_region>',
                        exist_ok = True
                        )
  ```

## <a name="set-up-machine-learning-resources"></a>Machine learning kaynaklarını ayarlama

Aşağıdaki adımlar bir işlem hattı çalıştırmak için gereken kaynakları ayarlar:

- Modeli kullanan, giriş etiketleri ve puanlamak için görüntüleri olan veri deposuna erişim (Bu zaten sizin için ayarlanır).
- Bir veri deposu, çıktılarının depolanması için ayarlayın.
- Nesneleri `DataReference`, önceki veri depolarındaki verileri gösterecek şekilde yapılandırın .
- İşlem hattı adımları çalıştıracağınız işlem makineler veya kümeleri ayarlayın.

### <a name="access-the-datastores"></a>Erişim veri depoları

İlk olarak, model, etiketler ve görüntüleri olan veri deposuna erişin.

Imagenet değerlendirme kümesinden görüntüleri tutan *pipelinedata* hesabında *sampleData*adlı bir genel blob kapsayıcısı kullanın. Bu ortak kapsayıcı için veri deposu adı *images_datastore*. Bu veri deposu ile çalışma alanınızı kaydedin:

```python
from azureml.core import Datastore

account_name = "pipelinedata"
datastore_name = "images_datastore"
container_name = "sampledata"

batchscore_blob = Datastore.register_azure_blob_container(ws,
                                                          datastore_name=datastore_name,
                                                          container_name=container_name,
                                                          account_name=account_name,
                                                          overwrite=True)
```

Ardından, çıktılar için varsayılan veri deposunu kullanacak şekilde ayarlayın.

Çalışma alanınızı oluşturduğunuzda, [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [BLOB depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) alanı varsayılan olarak çalışma alanına eklenir. Azure dosyaları bir çalışma alanı için varsayılan veri depo, ancak blob depolamayı da bir veri deposu olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Storage seçenekleri](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-references"></a>Veri başvuruları yapılandırın

Artık, işlem hattınızdaki veri işlem hattı adımları girdi olarak başvuru.

Bir veri kaynağında bir işlem hattı tarafından temsil edilen bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) nesne.  `DataReference`Nesnesi,birverideposutarafındanerişilebilenveya'deerişilebilir olan verileri işaret eder. Giriş görüntüleri `DataReference`için kullanılan dizin, önceden eğitilen modelin depolandığı dizin, Etiketler için dizin ve çıkış dizini için nesneler gerekir  .

```python
from azureml.data.data_reference import DataReference

input_images = DataReference(datastore=batchscore_blob,
                             data_reference_name="input_images",
                             path_on_datastore="batchscoring/images",
                             mode="download")

model_dir = DataReference(datastore=batchscore_blob,
                          data_reference_name="input_model",
                          path_on_datastore="batchscoring/models",
                          mode="download")

label_dir = DataReference(datastore=batchscore_blob,
                          data_reference_name="input_labels",
                          path_on_datastore="batchscoring/labels",
                          mode="download")

output_dir = PipelineData(name="scores",
                          datastore=def_data_store,
                          output_path_on_compute="batchscoring/results")
```

### <a name="set-up-compute-target"></a>İşlem Hedefi ' ayarlayın

Azure Machine Learning, *işlem* (veya *işlem hedefi*), makine öğrenimi ardışık düzeninde hesaplama adımlarını gerçekleştiren makinelere veya kümelere başvurur. Örneğin, oluşturabileceğiniz bir `Azure Machine Learning compute`.

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget

compute_name = "gpucluster"
compute_min_nodes = 0
compute_max_nodes = 4
vm_size = "STANDARD_NC6"

if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target. just use it. ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(
        vm_size=vm_size,  # NC6 is GPU-enabled
        vm_priority='lowpriority',  # optional
        min_nodes=compute_min_nodes,
        max_nodes=compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws,
                                          compute_name,
                                          provisioning_config)

    compute_target.wait_for_completion(
        show_output=True,
        min_node_count=None,
        timeout_in_minutes=20)
```

## <a name="prepare-the-model"></a>Model hazırlama

Pretrained modeli kullanabilmeniz için model indirin ve çalışma alanınızla kaydetmek gerekir.

### <a name="download-the-pretrained-model"></a>Pretrained modeli indirin.

Kullanan görüntü işleme modelinizle (InceptionV3) indirmesine <http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz>. Ardından `models` alt klasöre ayıklayın.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url = "http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

### <a name="register-the-model"></a>Modeli kaydedin

Modelin nasıl kaydedileceği aşağıda verilmiştir:

```python
import shutil
from azureml.core.model import Model

# register downloaded model
model = Model.register(
    model_path="models/inception_v3.ckpt",
    model_name="inception",  # This is the name of the registered model
    tags={'pretrained': "inception"},
    description="Imagenet trained tensorflow inception",
    workspace=ws)
```

## <a name="write-your-scoring-script"></a>Puanlama betiğinizi yazma

>[!Warning]
>Aşağıdaki kod, [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/pipeline-batch-scoring/pipeline-batch-scoring.ipynb)tarafından kullanılan [batch_score. Kopyala](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/pipeline-batch-scoring/batch_scoring.py) dosyasında yer alan bir örnektir. Senaryonuz için kendi Puanlama betiğinizi oluşturmanız gerekir.

`batch_score.py` Betik alır girdi görüntülerini *dataset_path*, kullanan modellerinde *model_dir,* ve çıkaran *sonuçları label.txt* için *output_dir*.

```python
# Snippets from a sample scoring script
# Refer to the accompanying batch-scoring Notebook
# https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb
# for the implementation script

# Get labels
def get_class_label_dict(label_file):
  label = []
  proto_as_ascii_lines = tf.gfile.GFile(label_file).readlines()
  for l in proto_as_ascii_lines:
    label.append(l.rstrip())
  return label

class DataIterator:
  # Definition of the DataIterator here

def main(_):
    # Refer to batch-scoring Notebook for implementation.
    label_file_name = os.path.join(args.label_dir, "labels.txt")
    label_dict = get_class_label_dict(label_file_name)
    classes_num = len(label_dict)
    test_feeder = DataIterator(data_dir=args.dataset_path)
    total_size = len(test_feeder.labels)

    # get model from model registry
    model_path = Model.get_model_path(args.model_name)
    with tf.Session() as sess:
        test_images = test_feeder.input_pipeline(batch_size=args.batch_size)
        with slim.arg_scope(inception_v3.inception_v3_arg_scope()):
            input_images = tf.placeholder(tf.float32, [args.batch_size, image_size, image_size, num_channel])
            logits, _ = inception_v3.inception_v3(input_images,
                                                        num_classes=classes_num,
                                                        is_training=False)
            probabilities = tf.argmax(logits, 1)

        sess.run(tf.global_variables_initializer())
        sess.run(tf.local_variables_initializer())
        coord = tf.train.Coordinator()
        threads = tf.train.start_queue_runners(sess=sess, coord=coord)
        saver = tf.train.Saver()
        saver.restore(sess, model_path)
        out_filename = os.path.join(args.output_dir, "result-labels.txt")

        # copy the file to artifacts
        shutil.copy(out_filename, "./outputs/")
```

## <a name="build-and-run-the-batch-scoring-pipeline"></a>Derleme ve toplu işlem Puanlama işlem hattı çalıştırma

### <a name="prepare-the-run-environment"></a>Çalışma ortamı hazırlama

Betiği için conda bağımlılıklarını belirtin. İşlem hattı adımını oluştururken bu nesneye daha sonra ihtiyacınız olacaktır.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

cd = CondaDependencies.create(
    pip_packages=["tensorflow-gpu==1.10.0", "azureml-defaults"])

# Runconfig
amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.gpu_support = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="specify-the-parameter-for-your-pipeline"></a>İşlem hattınızı parametresi belirtin

Bir [pipelineparameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) nesnesini varsayılan değerle kullanarak bir işlem hattı parametresi oluşturun.

```python
from azureml.pipeline.core.graph import PipelineParameter
batch_size_param = PipelineParameter(
    name="param_batch_size",
    default_value=20)
```

### <a name="create-the-pipeline-step"></a>İşlem hattı adım oluşturma

Komut dosyası, ortam yapılandırması ve parametreleri kullanarak işlem hattı adımını oluşturun. Betik yürütme hedefi olarak zaten çalışma alanınıza bağlı işlem hedefini belirtin. Kullanım [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) işlem hattı adımı oluşturmak.

```python
from azureml.pipeline.steps import PythonScriptStep
inception_model_name = "inception_v3.ckpt"

batch_score_step = PythonScriptStep(
    name="batch_scoring",
    script_name="batch_score.py",
    arguments=["--dataset_path", input_images,
               "--model_name", "inception",
               "--label_dir", label_dir,
               "--output_dir", output_dir,
               "--batch_size", batch_size_param],
    compute_target=compute_target,
    inputs=[input_images, label_dir],
    outputs=[output_dir],
    runconfig=amlcompute_run_config,
    source_directory=scripts_folder
```

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma

Şimdi işlem hattını çalıştırın ve ürettiği çıktıyı inceleyin. Çıktıda her giriş görüntüsüne karşılık gelen bir puan bulunur.

```python
import pandas as pd
from azureml.pipeline.core import Pipeline

# Run the pipeline
pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(
    pipeline, pipeline_params={"param_batch_size": 20})

# Wait for the run to finish (this might take several minutes)
pipeline_run.wait_for_completion(show_output=True)

# Download and review the output
step_run = list(pipeline_run.get_children())[0]
step_run.download_file("./outputs/result-labels.txt")

df = pd.read_csv("result-labels.txt", delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
df.head()
```

## <a name="publish-the-pipeline"></a>Yayımlama kanalı

Çalıştırmanın sonucuna memnun olduktan sonra, işlem hattını yayımlayın, böylece daha sonra farklı giriş değerleriyle çalıştırabilirsiniz. Bir işlem hattı yayımladığınızda, bir REST uç noktası alırsınız. Bu uç nokta, [Pipelineparameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py)kullanarak zaten dahil ettiğiniz parametrelerin kümesiyle işlem hattının çağrılması kabul eder.

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring",
    description="Batch scoring using Inception v3 model",
    version="1.0")
```

## <a name="rerun-the-pipeline-by-using-the-rest-endpoint"></a>REST uç noktasını kullanarak işlem hattını yeniden çalıştırma

İşlem hattını yeniden çalıştırmak için, [Azurecliauthentication sınıfında](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py)açıklandığı gibi Azure Active Directory bir kimlik doğrulama üst bilgisi belirtecine ihtiyacınız olacaktır.

```python
from azureml.pipeline.core.run import PipelineRun
from azureml.pipeline.core import PublishedPipeline

rest_endpoint = published_pipeline.endpoint
# specify batch size when running the pipeline
response = requests.post(rest_endpoint,
                         headers=aad_token,
                         json={"ExperimentName": "batch_scoring",
                               "ParameterAssignments": {"param_batch_size": 50}})

# Monitor the run
published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)

RunDetails(published_pipeline_run).show()
```

## <a name="next-steps"></a>Sonraki adımlar

Bu çalışmayı uçtan uca görmek için, [GitHub](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines)'da Batch Puanlama Not defterini deneyin.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

