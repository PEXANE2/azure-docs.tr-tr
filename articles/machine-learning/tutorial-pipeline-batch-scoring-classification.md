---
title: 'Öğretici: toplu puanlama için ML boru hatları'
titleSuffix: Azure Machine Learning
description: Bu öğreticide, görüntü sınıflandırma modelinde toplu puanlama gerçekleştirmek için bir makine öğrenme ardışık hattı oluşturursunuz. Azure Machine Learning, altyapı ve otomasyon yerine makine öğrenimine odaklanmanızı sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: laobri
ms.date: 03/11/2020
ms.openlocfilehash: 1ccd7a7f33c6ee5cab8b7173d8eb93365b6cb587
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472229"
---
# <a name="tutorial-build-an-azure-machine-learning-pipeline-for-batch-scoring"></a>Öğretici: Toplu puanlama için bir Azure Machine Learning ardışık hattı oluşturun

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Toplu puanlama işini çalıştırmak için Azure Machine Learning'de nasıl bir boru hattı oluşturabildiğini öğrenin. Makine öğrenimi boru hatları iş akışınızı hız, taşınabilirlik ve yeniden kullanımla optimize eder, böylece altyapı ve otomasyon yerine makine öğrenimine odaklanabilirsiniz. Bir ardışık yapı oluşturup yayımladıktan sonra, herhangi bir platformdaki herhangi bir HTTP kitaplığından ardışık lığını tetiklemek için kullanabileceğiniz bir DSAMA bitiş noktası nı yapılandırırsınız. 

Örnek, etiketlenmemiş görüntüleri sınıflandırmak için Tensorflow'da uygulanan önceden eğitilmiş bir [Inception-V3](https://arxiv.org/abs/1512.00567) kıvrımlı sinir ağı modelini kullanır. [Makine öğrenimi boru hatları hakkında daha fazla bilgi edinin.](concept-ml-pipelines.md)

Bu öğreticide, aşağıdaki görevleri tamamlayacaksınız:

> [!div class="checklist"]
> * Çalışma alanını yapılandırma 
> * Örnek verileri karşıdan yükleme ve depolama
> * Veri alma ve çıktı verileri için veri kümesi nesneleri oluşturma
> * Modeli çalışma alanınızda indirin, hazırlayın ve kaydettirin
> * Hesaplama hedeflerini sağlama ve puanlama komut dosyası oluşturma
> * Async `ParallelRunStep` toplu puanlama için sınıfı kullanın
> * Bir ardışık hat lar oluşturma, çalıştırma ve yayımlama
> * Ardışık hatlar için BIR REST bitiş noktası etkinleştirme

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanınız veya dizüstü sanal makineniz yoksa, [kurulum öğreticisinin Bölüm 1'ini](tutorial-1st-experiment-sdk-setup.md)tamamlayın.
* Kurulum öğreticisini bitirdiğinizde, *öğreticileri/makine öğrenimi-boru hatlarını-gelişmiş/öğretici-boru hattı-toplu-puanlama-sınıflandırma.ipynb* not defterini açmak için aynı dizüstü bilgisayar sunucusunu kullanın.

Kendi [yerel ortamda](how-to-configure-environment.md#local)kurulum öğretici çalıştırmak istiyorsanız, [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials)üzerinde öğretici erişebilirsiniz. Gerekli `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-contrib-pipeline-steps pandas requests` paketleri almak için çalıştırın.

## <a name="configure-workspace-and-create-a-datastore"></a>Çalışma alanını yapılandırma ve bir veri deposu oluşturma

Varolan Azure Machine Learning çalışma alanından bir çalışma alanı nesnesi oluşturun.

- [Çalışma alanı,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) Azure aboneliğinizi ve kaynak bilgilerinizi kabul eden bir sınıftır. Çalışma alanı, model çalışmalarınızı izlemek ve izlemek için kullanabileceğiniz bir bulut kaynağı da oluşturur. 
- `Workspace.from_config()`dosyayı `config.json` okur ve kimlik doğrulama ayrıntılarını adlı `ws`bir nesneye yükler. Nesne `ws` bu öğretici boyunca kod kullanılır.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

## <a name="create-a-datastore-for-sample-images"></a>Örnek görüntüler için bir veri deposu oluşturma

Hesapta, `pipelinedata` imagenet değerlendirme genel veri örneğini ortak blob kapsayıcısından `sampledata` alın. Verileri `register_azure_blob_container()` çalışma alanı için kullanılabilir `images_datastore`hale getirmek için çağrı. Ardından, çalışma alanı varsayılan veri deposunu çıktı veri deposu olarak ayarlayın. Ardışık yöndeçıktı puanlamak için çıktı veri deposunu kullanın.

```python
from azureml.core.datastore import Datastore

batchscore_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="images_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata", 
                      overwrite=True)

def_data_store = ws.get_default_datastore()
```

## <a name="create-dataset-objects"></a>Veri kümesi nesneleri oluşturma

Ardışık hatlar `Dataset` inşa edilirken, nesneler çalışma alanı veri `PipelineData` depolarından verileri okumak için kullanılır ve nesneler ara verileri ardışık satır adımları arasında aktarmak için kullanılır.

> [!Important]
> Bu öğreticideki toplu puanlama örneği yalnızca bir ardışık işlem adımı kullanır. Birden çok adımı olan kullanım durumlarında, tipik akış şu adımları içerir:
>
> 1. Ham `Dataset` veri almak, bazı dönüşüm gerçekleştirmek ve sonra bir `PipelineData` nesne *çıktısı* için *girişleri* olarak nesneleri kullanın.
>
> 2. Önceki adımdaki *çıktı nesnesini* *giriş nesnesi*olarak kullanın. `PipelineData` Sonraki adımlar için tekrarlayın.

Bu senaryoda, `Dataset` hem giriş görüntüleri hem de sınıflandırma etiketleri (y-test değerleri) için veri deposu dizinlerine karşılık gelen nesneler oluşturursunuz. Ayrıca, toplu `PipelineData` iş puanlama çıktıverileri için bir nesne oluşturursunuz.

```python
from azureml.core.dataset import Dataset
from azureml.pipeline.core import PipelineData

input_images = Dataset.File.from_files((batchscore_blob, "batchscoring/images/"))
label_ds = Dataset.File.from_files((batchscore_blob, "batchscoring/labels/*.txt"))
output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

Ardından, veri kümelerini çalışma alanına kaydedin.

```python

input_images = input_images.register(workspace = ws, name = "input_images")
label_ds = label_ds.register(workspace = ws, name = "label_ds")
```

## <a name="download-and-register-the-model"></a>Modeli indirin ve kaydettirin

Önceden eğitilmiş Tensorflow modelini bir ardışık hat ta yığın puanlamaiçin kullanmak için indirin. İlk olarak, modeli depoladığınız yerel bir dizin oluşturun. Daha sonra, modeli indirin ve ayıklayın.

```python
import os
import tarfile
import urllib.request

if not os.path.isdir("models"):
    os.mkdir("models")
    
response = urllib.request.urlretrieve("http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz", "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall("models")
```

Ardından, modeli çalışma alanınıza kaydedin, böylece ardışık işlemdeki modeli kolayca alabilirsiniz. `register()` Statik işlevde `model_name` parametre, modelinizi SDK boyunca bulmak için kullandığınız anahtardır.

```python
from azureml.core.model import Model
 
model = Model.register(model_path="models/inception_v3.ckpt",
                       model_name="inception",
                       tags={"pretrained": "inception"},
                       description="Imagenet trained tensorflow inception",
                       workspace=ws)
```

## <a name="create-and-attach-the-remote-compute-target"></a>Uzak işlem hedefini oluşturma ve iliştirme

Makine öğrenimi ardışık hatları yerel olarak çalıştırılamaz, bu nedenle bunları bulut kaynakları veya *uzaktan bilgi işlem hedefleri*üzerinde çalıştırın. Uzaktan işlem hedefi, denemeler ve makine öğrenimi iş akışları çalıştırdığınız yeniden kullanılabilir bir sanal bilgi işlem ortamıdır. 

GPU özellikli [`AmlCompute`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) bir hedef oluşturmak için aşağıdaki kodu çalıştırın ve ardından çalışma alanınıza takın. Bilgi işlem hedefleri hakkında daha fazla bilgi için [kavramsal makaleye](https://docs.microsoft.com/azure/machine-learning/concept-compute-target)bakın.


```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.exceptions import ComputeTargetException
compute_name = "gpu-cluster"

# checks to see if compute target already exists in workspace, else create it
try:
    compute_target = ComputeTarget(workspace=ws, name=compute_name)
except ComputeTargetException:
    config = AmlCompute.provisioning_configuration(vm_size="STANDARD_NC6",
                                                   vm_priority="lowpriority", 
                                                   min_nodes=0, 
                                                   max_nodes=1)

    compute_target = ComputeTarget.create(workspace=ws, name=compute_name, provisioning_configuration=config)
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

## <a name="write-a-scoring-script"></a>Puanlama komut dosyası yazma

Puanlama yapmak için, bir toplu `batch_scoring.py`iş puanlama komut dosyası adlı oluşturun ve ardından geçerli dizine yazın. Komut dosyası giriş görüntülerini alır, sınıflandırma modelini uygular ve sonra tahminleri bir sonuç dosyasına çıkarır.

`batch_scoring.py` Komut dosyası, daha sonra `ParallelRunStep` oluşturduğunuz dosyadan geçirilen aşağıdaki parametreleri alır:

- `--model_name`: Kullanılan modelin adı.
- `--labels_name`: Dosyayı `labels.txt` `Dataset` tutanın adı.

Boru hattı altyapısı, parametreleri `ArgumentParser` boru hattı adımlarına geçirmek için sınıfı kullanır. Örneğin, aşağıdaki kodda, ilk `--model_name` bağımsız değişken özellik tanımlayıcısı `model_name`verilir. `init()` İşlev, `Model.get_model_path(args.model_name)` bu özelliğe erişmek için kullanılır.


```python
%%writefile batch_scoring.py

import os
import argparse
import datetime
import time
import tensorflow as tf
from math import ceil
import numpy as np
import shutil
from tensorflow.contrib.slim.python.slim.nets import inception_v3

from azureml.core import Run
from azureml.core.model import Model
from azureml.core.dataset import Dataset

slim = tf.contrib.slim

image_size = 299
num_channel = 3


def get_class_label_dict():
    label = []
    proto_as_ascii_lines = tf.gfile.GFile("labels.txt").readlines()
    for l in proto_as_ascii_lines:
        label.append(l.rstrip())
    return label


def init():
    global g_tf_sess, probabilities, label_dict, input_images

    parser = argparse.ArgumentParser(description="Start a tensorflow model serving")
    parser.add_argument('--model_name', dest="model_name", required=True)
    parser.add_argument('--labels_name', dest="labels_name", required=True)
    args, _ = parser.parse_known_args()

    workspace = Run.get_context(allow_offline=False).experiment.workspace
    label_ds = Dataset.get_by_name(workspace=workspace, name=args.labels_name)
    label_ds.download(target_path='.', overwrite=True)

    label_dict = get_class_label_dict()
    classes_num = len(label_dict)

    with slim.arg_scope(inception_v3.inception_v3_arg_scope()):
        input_images = tf.placeholder(tf.float32, [1, image_size, image_size, num_channel])
        logits, _ = inception_v3.inception_v3(input_images,
                                              num_classes=classes_num,
                                              is_training=False)
        probabilities = tf.argmax(logits, 1)

    config = tf.ConfigProto()
    config.gpu_options.allow_growth = True
    g_tf_sess = tf.Session(config=config)
    g_tf_sess.run(tf.global_variables_initializer())
    g_tf_sess.run(tf.local_variables_initializer())

    model_path = Model.get_model_path(args.model_name)
    saver = tf.train.Saver()
    saver.restore(g_tf_sess, model_path)


def file_to_tensor(file_path):
    image_string = tf.read_file(file_path)
    image = tf.image.decode_image(image_string, channels=3)

    image.set_shape([None, None, None])
    image = tf.image.resize_images(image, [image_size, image_size])
    image = tf.divide(tf.subtract(image, [0]), [255])
    image.set_shape([image_size, image_size, num_channel])
    return image


def run(mini_batch):
    result_list = []
    for file_path in mini_batch:
        test_image = file_to_tensor(file_path)
        out = g_tf_sess.run(test_image)
        result = g_tf_sess.run(probabilities, feed_dict={input_images: [out]})
        result_list.append(os.path.basename(file_path) + ": " + label_dict[result[0]])
    return result_list
```

> [!TIP]
> Bu öğreticideki ardışık işlem yalnızca bir adımı vardır ve çıktıyı bir dosyaya yazar. Çok adımlı ardışık hatlar `ArgumentParser` için, sonraki adımlara giriş için çıktı verileri yazmak için bir dizin tanımlamak için de kullanın. `ArgumentParser` Tasarım deseni kullanarak birden çok satır adımı arasında veri geçirme örneği için [not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb)bakın.

## <a name="build-the-pipeline"></a>Boru hattını oluşturun

Ardışık komut dosyasını çalıştırmadan önce, Python ortamını tanımlayan ve `batch_scoring.py` komut dosyanızın gerektirdiği bağımlılıkları oluşturan bir nesne oluşturun. Gerekli temel bağımlılık Tensorflow, ancak arka `azureml-defaults` plan işlemleri için de yükleyin. Bağımlılıkları `RunConfiguration` kullanarak bir nesne oluşturun. Ayrıca, Docker ve Docker-GPU desteğini belirtin.

```python
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.13.1", "azureml-defaults"])
env = Environment(name="parallelenv")
env.python.conda_dependencies = cd
env.docker.base_image = DEFAULT_GPU_IMAGE
```

### <a name="create-the-configuration-to-wrap-the-script"></a>Komut dosyasını sarmak için yapılandırmayı oluşturma

Komut dosyasını, ortam yapılandırmasını ve parametreleri kullanarak ardışık yapı adımını oluşturun. Çalışma alanınıza zaten iliştirdiğiniz bilgi işlem hedefini belirtin.

```python
from azureml.contrib.pipeline.steps import ParallelRunConfig

parallel_run_config = ParallelRunConfig(
    environment=env,
    entry_script="batch_scoring.py",
    source_directory=".",
    output_action="append_row",
    mini_batch_size="20",
    error_threshold=1,
    compute_target=compute_target,
    process_count_per_node=2,
    node_count=1
)
```

### <a name="create-the-pipeline-step"></a>Boru hattı adımını oluşturma

Bir ardışık ad, bir ardışık hattı çalıştırmak için gereken her şeyi kapsayan bir nesnedir:

* Çevre ve bağımlılık ayarları
* Boru hattını çalıştırmak için bilgi işlem kaynağı
* Giriş ve çıktı verileri ve herhangi bir özel parametre
* Adım sırasında çalıştırmak için bir komut dosyası na veya SDK mantığına başvuru

Üst sınıftan [`PipelineStep`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py)birden çok sınıf devralır. Bir adım oluşturmak için belirli çerçeveleri veya yığınları kullanmak için sınıfları seçebilirsiniz. Bu örnekte, özel `ParallelRunStep` bir Python komut dosyası kullanarak adım mantığınızı tanımlamak için sınıfı kullanırsınız. Komut dosyanıza bir bağımsız değişken adım veya adım çıktısı için bir giriş ise, `arguments` bağımsız değişken hem `input` *dizi* de *hem de* ya da `output` parametre, sırasıyla tanımlanmalıdır. 

Birden fazla adımın olduğu senaryolarda, `outputs` dizideki bir nesne başvurusu sonraki bir ardışık adım için *giriş* olarak kullanılabilir hale gelir.

```python
from azureml.contrib.pipeline.steps import ParallelRunStep

batch_score_step = ParallelRunStep(
    name="parallel-step-test",
    inputs=[input_images.as_named_input("input_images")],
    output=output_dir,
    models=[model],
    arguments=["--model_name", "inception",
               "--labels_name", "label_ds"],
    parallel_run_config=parallel_run_config,
    allow_reuse=False
)
```

Farklı adım türleri için kullanabileceğiniz tüm sınıfların listesi için [adımlar paketine](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py)bakın.

## <a name="submit-the-pipeline"></a>Boru hattını gönderme

Şimdi, boru hattını çalıştırın. İlk olarak, `Pipeline` çalışma alanı başvurunuzu ve oluşturduğunuz ardışık hatlar adımını kullanarak bir nesne oluşturun. `steps` Parametre bir dizi adımdır. Bu durumda, toplu puanlama için yalnızca bir adım vardır. Birden çok adıma sahip ardışık hatlar oluşturmak için adımları bu dizide sırayla yerleştirin.

Ardından, yürütme `Experiment.submit()` için ardışık hatlar göndermek için işlevi kullanın. Ayrıca özel parametreyi `param_batch_size`de belirtirsiniz. İşlev, `wait_for_completion` boru hattı oluşturma işlemi sırasında günlükleri çıktırıyor. Geçerli ilerlemeyi görmek için günlükleri kullanabilirsiniz.

> [!IMPORTANT]
> İlk boru hattı çalışması yaklaşık *15 dakika*sürer. Tüm bağımlılıklar indirilmeli, Docker görüntüsü oluşturulmalı ve Python ortamı sağlanıp oluşturulmalıdır. Bu kaynaklar oluşturulduğu yerine yeniden kullanıldığından, ardışık alanı yeniden çalıştırmak çok daha az zaman alır. Ancak, ardışık işlem için toplam çalışma süresi komut dosyalarınızın iş yüküne ve her ardışık işlem adımında çalışan işlemlere bağlıdır.

```python
from azureml.core import Experiment
from azureml.pipeline.core import Pipeline

pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline)
pipeline_run.wait_for_completion(show_output=True)
```

### <a name="download-and-review-output"></a>Çıktıyı karşıdan yükleme ve gözden geçirme

Komut dosyasından oluşturulan çıktı dosyasını indirmek için `batch_scoring.py` aşağıdaki kodu çalıştırın. Ardından, puanlama sonuçlarını keşfedin.

```python
import pandas as pd

batch_run = next(pipeline_run.get_children())
batch_output = batch_run.get_output_data("scores")
batch_output.download(local_path="inception_results")

for root, dirs, files in os.walk("inception_results"):
    for file in files:
        if file.endswith("parallel_run_step.txt"):
            result_file = os.path.join(root, file)

df = pd.read_csv(result_file, delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
print("Prediction has ", df.shape[0], " rows")
df.head(10)
```

## <a name="publish-and-run-from-a-rest-endpoint"></a>REST bitiş noktasından yayımlayın ve çalıştırın

Çalışma alanınıza ardışık ardışık hatlar hattını yayımlamak için aşağıdaki kodu çalıştırın. Azure Machine Learning stüdyosundaki çalışma alanınızda, çalışma geçmişi ve süreleri de dahil olmak üzere ardışık hatlar için meta verileri görebilirsiniz. Ayrıca stüdyodan el ile boru hattı çalıştırabilirsiniz.

Ardışık kaynak yayımlama, ardışık ardışık lığı herhangi bir platformdaki herhangi bir HTTP kitaplığından çalıştırmak için kullanabileceğiniz bir REST bitiş noktası sağlar.

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", description="Batch scoring using Inception v3 model", version="1.0")

published_pipeline
```

Ardışık ardışık hattı REST bitiş noktasından çalıştırmak için bir OAuth2 Taşıyıcı türü kimlik doğrulama üstbilgisine ihtiyacınız vardır. Aşağıdaki örnekte etkileşimli kimlik doğrulama (çizim amacıyla) kullanılır, ancak otomatik veya başsız kimlik doğrulaması gerektiren çoğu üretim senaryosu [için, bu makalede açıklandığı](how-to-setup-authentication.md)gibi hizmet temel kimlik doğrulamasını kullanın.

Hizmet temel kimlik doğrulaması, *Azure Etkin Dizini'nde* *bir Uygulama Kaydı* oluşturmayı içerir. İlk olarak, bir istemci sırrı oluşturursunuz ve ardından makine öğrenimi çalışma alanınıza hizmet temel *rol erişimi* verirsiniz. Kimlik [`ServicePrincipalAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?view=azure-ml-py) doğrulama akışınızı yönetmek için sınıfı kullanın. 

Her [`InteractiveLoginAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.interactiveloginauthentication?view=azure-ml-py) `ServicePrincipalAuthentication` ikisi `AbstractAuthentication`de ve miras . Her iki durumda [`get_authentication_header()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.abstractauthentication?view=azure-ml-py#get-authentication-header--) da, üstbilgi almak için işlevi aynı şekilde kullanın:

```python
from azureml.core.authentication import InteractiveLoginAuthentication

interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()
```

Yayınlanan ardışık ardışık nesnenin `endpoint` özelliğinden REST URL'sini alın. REST URL'sini azure machine learning stüdyosunda da çalışma alanınızda bulabilirsiniz. 

Bitiş noktasına bir HTTP POST isteği oluşturun. İstekte kimlik doğrulama üstbilginizi belirtin. Deneme adı ve toplu iş boyutu parametresi olan bir JSON yük nesnesi ekleyin. Öğreticide daha önce `param_batch_size` belirtildiği gibi, `batch_scoring.py` komut dosyanızı adım `PipelineParameter` yapılandırmasında bir nesne olarak tanımladığınız için komut dosyanıza aktarılır.

Çalıştırmayı tetiklemek için istekte bulunun. Çalışan kimliğin `Id` değerini almak için yanıt sözlüğünden anahtara erişmek için kod ekleyin.

```python
import requests

rest_endpoint = published_pipeline.endpoint
response = requests.post(rest_endpoint, 
                         headers=auth_header, 
                         json={"ExperimentName": "batch_scoring",
                               "ParameterAssignments": {"param_batch_size": 50}})
run_id = response.json()["Id"]
```

Yeni çalıştırmanın durumunu izlemek için çalıştırılatan kimliği kullanın. Yeni çalıştırmak bitirmek için başka bir 10-15 dakika sürer. 

Yeni çalıştırma, öğreticide daha önce çalıştırdığınız ardışık hatlara benzer. Tam çıktıyı görüntülememeyi seçebilirsiniz.

```python
from azureml.pipeline.core.run import PipelineRun
from azureml.widgets import RunDetails

published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)
RunDetails(published_pipeline_run).show()
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure Machine Learning eğitimlerini çalıştırmayı planlıyorsanız bu bölümü tamamlamayın.

### <a name="stop-the-compute-instance"></a>İşlem örneğini durdurma

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Her şeyi silme

Oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, bunları silin, böylece herhangi bir ücret ödemezsiniz:

1. Azure portalında, sol menüde **Kaynak gruplarını**seçin.
1. Kaynak grupları listesinde oluşturduğunuz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin.
1. Kaynak grup adını girin. Ardından **Sil'i**seçin.

Kaynak grubunu da tutabilir, ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makine öğrenme boru hatları öğretici, aşağıdaki görevleri yaptı:

> [!div class="checklist"]
> * Uzak bir GPU bilgi işlem kaynağında çalışacak ortam bağımlılıkları içeren bir ardışık hat lar oluşturun.
> * Önceden eğitilmiş tensorflow modelini kullanarak toplu oluşturma tahminlerini çalıştırmak için bir puanlama komut dosyası oluşturdu.
> * Bir ardışık hatlar yayımladı ve bir REST bitiş noktasından çalıştırılmasına olanak sağladı.

Makine öğrenme SDK kullanarak boru hatları oluşturmak için nasıl daha fazla örnek için, [not defteri deposubakın.](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines)
