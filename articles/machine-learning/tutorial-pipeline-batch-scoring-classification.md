---
title: 'Öğretici: toplu Puanlama için ML işlem hatları'
titleSuffix: Azure Machine Learning
description: Bu öğreticide, bir görüntü sınıflandırma modelinde Batch Puanlama gerçekleştirmek için bir makine öğrenimi işlem hattı oluşturacaksınız. Azure Machine Learning altyapı ve otomasyon yerine makine öğrenimine odaklanabilmenizi sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: lobrien
ms.author: laobri
ms.reviewer: laobri
ms.date: 03/11/2020
ms.custom: contperfq4, devx-track-python
ms.openlocfilehash: c981bed2b30f47223a1fd562d4a5d0fff96e3adf
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89646978"
---
# <a name="tutorial-build-an-azure-machine-learning-pipeline-for-batch-scoring"></a>Öğretici: toplu Puanlama için Azure Machine Learning işlem hattı oluşturma

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu gelişmiş öğreticide, Batch Puanlama işi çalıştırmak için Azure Machine Learning bir işlem hattı oluşturmayı öğreneceksiniz. Makine öğrenimi ardışık düzenleri, iş akışınızı hız, taşınabilirlik ve yeniden kullanım açısından iyileştirerek altyapı ve otomasyon yerine makine öğrenimine odaklanmanıza olanak sağlayabilir. Bir işlem hattı derleyip yayımladıktan sonra, işlem hattını herhangi bir platformda herhangi bir HTTP kitaplığından tetiklemek için kullanabileceğiniz bir REST uç noktası yapılandırırsınız. 

Örnek, etiketli görüntüleri sınıflandırmak için TensorFlow 'da uygulanan önceden eğitilen [Inception-v3](https://arxiv.org/abs/1512.00567) sinir ağ modelini kullanır. [Makine öğrenimi işlem hatları hakkında daha fazla bilgi edinin](concept-ml-pipelines.md).

Bu öğreticide, aşağıdaki görevleri tamamlayacaksınız:

> [!div class="checklist"]
> * Çalışma alanını yapılandırma 
> * Örnek verileri indirme ve depolama
> * Verileri getirmek ve çıkarmak için veri kümesi nesneleri oluşturma
> * Çalışma alanınıza modeli indirme, hazırlama ve kaydetme
> * İşlem hedeflerini sağlama ve bir Puanlama betiği oluşturma
> * `ParallelRunStep`Zaman uyumsuz toplu Puanlama için sınıfı kullanın
> * İşlem hattı oluşturma, çalıştırma ve yayımlama
> * İşlem hattı için bir REST uç noktasını etkinleştirme

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="prerequisites"></a>Önkoşullar

* Zaten bir Azure Machine Learning çalışma alanınız veya Not defteri sanal makineniz yoksa, [Kurulum öğreticisinin 1. kısmını](tutorial-1st-experiment-sdk-setup.md)doldurun.
* Kurulum öğreticisini tamamladığınızda, *öğreticiler/Machine-Learning-Pipelines-Advanced/tutorial-Pipeline-Batch-Scoring-Classification. ipynb* Not defterini açmak için aynı not defteri sunucusunu kullanın.

Kurulum öğreticisini kendi [Yerel ortamınızda](how-to-configure-environment.md#local)çalıştırmak istiyorsanız [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials)'daki öğreticiye erişebilirsiniz. `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-pipeline-steps pandas requests`Gerekli paketleri almak için öğesini çalıştırın.

## <a name="configure-workspace-and-create-a-datastore"></a>Çalışma alanını yapılandırma ve veri deposu oluşturma

Mevcut Azure Machine Learning çalışma alanından bir çalışma alanı nesnesi oluşturun.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

> [!IMPORTANT]
> Bu kod parçacığı, çalışma alanı yapılandırmasının geçerli dizine veya onun üst öğesine kaydedilmesini bekliyor. Çalışma alanı oluşturma hakkında daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanları oluşturma ve yönetme](how-to-manage-workspace.md). Yapılandırmayı dosyaya kaydetme hakkında daha fazla bilgi için bkz. [çalışma alanı yapılandırma dosyası oluşturma](how-to-configure-environment.md#workspace).

## <a name="create-a-datastore-for-sample-images"></a>Örnek görüntüler için bir veri deposu oluşturma

`pipelinedata`Hesapta, `sampledata` genel blob kapsayıcısından ımagenet değerlendirmesi ortak veri örneğini alın. `register_azure_blob_container()`Verileri, ad altında çalışma alanı için kullanılabilir hale getirmek için çağırın `images_datastore` . Daha sonra, çalışma alanı varsayılan veri deposunu çıkış veri deposu olarak ayarlayın. İşlem hattındaki çıktıyı öğrenmek için çıkış veri deposunu kullanın.

Verilere erişme hakkında daha fazla bilgi için bkz. [verilere erişme](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#python-sdk).

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

İşlem hatları oluştururken, `Dataset` nesneler çalışma alanı veri depolarından verileri okumak için kullanılır ve `PipelineData` nesneler işlem hattı adımları arasında ara verileri aktarmak için kullanılır.

> [!Important]
> Bu öğreticideki toplu işlem Puanlama örneği yalnızca bir ardışık düzen adımını kullanır. Birden çok adımı olan kullanım durumlarında, tipik akış şu adımları içerir:
>
> 1. `Dataset`Ham verileri getirmek, bazı dönüşümleriniz gerçekleştirmek ve ardından bir nesneyi *çıkarmak* için nesneleri *giriş* olarak kullanın `PipelineData` .
>
> 2. `PipelineData`Önceki adımda bir *giriş nesnesi*olarak *Çıkış nesnesini* kullanın. Sonraki adımlar için tekrarlayın.

Bu senaryoda, `Dataset` hem giriş görüntüleri hem de sınıflandırma etiketleri (y-test değerleri) için veri deposu dizinlerine karşılık gelen nesneler oluşturursunuz. `PipelineData`Toplu Puanlama çıkış verileri için de bir nesne oluşturun.

```python
from azureml.core.dataset import Dataset
from azureml.pipeline.core import PipelineData

input_images = Dataset.File.from_files((batchscore_blob, "batchscoring/images/"))
label_ds = Dataset.File.from_files((batchscore_blob, "batchscoring/labels/"))
output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

Sonra, veri kümelerini çalışma alanına kaydedin.

```python

input_images = input_images.register(workspace = ws, name = "input_images")
label_ds = label_ds.register(workspace = ws, name = "label_ds")
```

## <a name="download-and-register-the-model"></a>Modeli indir ve Kaydet

Bir işlem hattındaki toplu Puanlama için kullanmak üzere önceden eğitilen TensorFlow modelini indirin. İlk olarak, modeli depoladığınız yerel bir dizin oluşturun. Ardından, modeli indirip ayıklayın.

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

Ardından, modeli işlem hattı işleminde kolayca alabilmeniz için çalışma alanınıza kaydedin. `register()`Statik işlevde `model_name` parametresi SDK genelinde modelinizi bulmak için kullandığınız anahtardır.

```python
from azureml.core.model import Model
 
model = Model.register(model_path="models/inception_v3.ckpt",
                       model_name="inception",
                       tags={"pretrained": "inception"},
                       description="Imagenet trained tensorflow inception",
                       workspace=ws)
```

## <a name="create-and-attach-the-remote-compute-target"></a>Uzaktan işlem hedefini oluşturma ve iliştirme

Makine öğrenimi ardışık düzenleri yerel olarak çalıştırılamaz, bu nedenle bunları bulut kaynaklarında veya *uzak işlem hedeflerinde*çalıştırmalısınız. Uzaktan işlem hedefi, denemeleri ve makine öğrenimi iş akışlarını çalıştırdığınız yeniden kullanılabilir bir sanal işlem ortamıdır. 

GPU etkin bir [`AmlCompute`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py&preserve-view=true) hedef oluşturmak ve ardından çalışma alanınıza eklemek için aşağıdaki kodu çalıştırın. İşlem hedefleri hakkında daha fazla bilgi için [kavramsal makaleye](https://docs.microsoft.com/azure/machine-learning/concept-compute-target)bakın.


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

## <a name="write-a-scoring-script"></a>Puanlama betiği yazma

Puanlama yapmak için, adlı bir Batch Puanlama betiği oluşturun `batch_scoring.py` ve sonra geçerli dizine yazın. Betik, giriş görüntülerini alır, sınıflandırma modelini uygular ve daha sonra tahminleri bir sonuç dosyasına çıkarır.

`batch_scoring.py`Betik, daha sonra oluşturduğunuz sunucudan geçirilen aşağıdaki parametreleri alır `ParallelRunStep` :

- `--model_name`: Kullanılan modelin adı.
- `--labels_dir`: `labels.txt` Dosyanın konumu.

İşlem hattı altyapısı, `ArgumentParser` parametreleri işlem hattı adımlarına geçirmek için sınıfını kullanır. Örneğin, aşağıdaki kodda, ilk bağımsız değişkene `--model_name` özellik tanımlayıcısı verilir `model_name` . `init()`İşlevinde, `Model.get_model_path(args.model_name)` Bu özelliğe erişmek için kullanılır.


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


def get_class_label_dict(labels_dir):
    label = []
    labels_path = os.path.join(labels_dir, 'labels.txt')
    proto_as_ascii_lines = tf.gfile.GFile(labels_path).readlines()
    for l in proto_as_ascii_lines:
        label.append(l.rstrip())
    return label


def init():
    global g_tf_sess, probabilities, label_dict, input_images

    parser = argparse.ArgumentParser(description="Start a tensorflow model serving")
    parser.add_argument('--model_name', dest="model_name", required=True)
    parser.add_argument('--labels_dir', dest="labels_dir", required=True)
    args, _ = parser.parse_known_args()

    label_dict = get_class_label_dict(args.labels_dir)
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
> Bu öğreticideki işlem hattının yalnızca bir adımı vardır ve çıktıyı bir dosyaya yazar. Çok adımlı işlem hatları için, `ArgumentParser` sonraki adımlara giriş için çıkış verilerini yazmak üzere bir dizin tanımlamak üzere öğesini de kullanabilirsiniz. Tasarım düzenini kullanarak birden çok işlem hattı adımı arasında veri geçirmenin bir örneği için `ArgumentParser` bkz. [Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb).

## <a name="build-the-pipeline"></a>İşlem hattını oluşturma

İşlem hattını çalıştırmadan önce, Python ortamını tanımlayan bir nesne oluşturun ve `batch_scoring.py` betiğinizin gerektirdiği bağımlılıkları oluşturur. Gereken ana bağımlılık TensorFlow ' dır, ancak aynı zamanda `azureml-core` `azureml-dataprep[fuse]` ParallelRunStep için gereklidir. Ayrıca, Docker ve Docker-GPU desteği de belirtin.

```python
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.15.2",
                                            "azureml-core", "azureml-dataprep[fuse]"])
env = Environment(name="parallelenv")
env.python.conda_dependencies = cd
env.docker.base_image = DEFAULT_GPU_IMAGE
```

### <a name="create-the-configuration-to-wrap-the-script"></a>Betiği kaydırmak için yapılandırma oluştur

Komut dosyası, ortam yapılandırması ve parametreleri kullanarak işlem hattı adımını oluşturun. Çalışma alanınıza zaten eklediğiniz işlem hedefini belirtin.

```python
from azureml.pipeline.steps import ParallelRunConfig

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

### <a name="create-the-pipeline-step"></a>İşlem hattı adımını oluşturma

İşlem hattı adımı, aşağıdakiler dahil olmak üzere bir işlem hattı çalıştırmak için ihtiyacınız olan her şeyi kapsülleyen bir nesnedir:

* Ortam ve bağımlılık ayarları
* İşlem hattının çalıştırılacağı işlem kaynağı
* Giriş ve çıkış verileri ve tüm özel parametreler
* Adım sırasında çalışacak bir betik veya SDK mantığına başvuru

Birden çok sınıf üst sınıftan devralınır [`PipelineStep`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py&preserve-view=true) . Bir adım oluşturmak için belirli çerçeveleri veya yığınları kullanmak üzere sınıfları seçebilirsiniz. Bu örnekte, `ParallelRunStep` özel bir Python betiği kullanarak adım mantığınızı tanımlamak için sınıfını kullanırsınız. Betiğinizin bir bağımsız değişkeni, adımın bir girişi ya da adımın bir çıkışı ise, bağımsız değişkenin sırasıyla *hem* dizide hem de ya da parametresinde tanımlanması `arguments` gerekir *and* `input` `output` . 

Birden fazla adımın olduğu senaryolarda dizideki bir nesne başvurusu, `outputs` sonraki bir işlem hattı adımı için *giriş* olarak kullanılabilir hale gelir.

```python
from azureml.pipeline.steps import ParallelRunStep
from datetime import datetime

parallel_step_name = "batchscoring-" + datetime.now().strftime("%Y%m%d%H%M")

label_config = label_ds.as_named_input("labels_input")

batch_score_step = ParallelRunStep(
    name=parallel_step_name,
    inputs=[input_images.as_named_input("input_images")],
    output=output_dir,
    arguments=["--model_name", "inception",
               "--labels_dir", label_config],
    side_inputs=[label_config],
    parallel_run_config=parallel_run_config,
    allow_reuse=False
)
```

Farklı adım türleri için kullanabileceğiniz tüm sınıfların bir listesi için, bkz. [adımlar paketi](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py&preserve-view=true).

## <a name="submit-the-pipeline"></a>İşlem hattını gönderme

Şimdi işlem hattını çalıştırın. İlk olarak, `Pipeline` çalışma alanı başvurusunu ve oluşturduğunuz işlem hattı adımını kullanarak bir nesne oluşturun. `steps`Parametresi bir adım dizisidir. Bu durumda, toplu Puanlama için yalnızca bir adım vardır. Birden çok adım içeren işlem hatları oluşturmak için, adımları Bu dizide sırayla yerleştirin.

Sonra, işlem hattını `Experiment.submit()` yürütmeye göndermek için işlevini kullanın. `wait_for_completion`İşlevi, işlem hattı derleme işlemi sırasında günlükleri çıktı olarak verir. Geçerli ilerlemeyi görmek için günlükleri kullanabilirsiniz.

> [!IMPORTANT]
> İlk işlem hattı çalıştırması yaklaşık *15 dakika*sürer. Tüm bağımlılıklar indirilmelidir, bir Docker görüntüsü oluşturulur ve Python ortamı sağlanmakta ve oluşturulur. Bu kaynaklar oluşturulması yerine yeniden kullanıldığından işlem hattının yeniden çalıştırılması çok daha az zaman alır. Ancak, işlem hattının toplam çalışma süresi, betiklerinizin ve her bir ardışık düzen adımında çalışan işlemlerin iş yüküne bağlıdır.

```python
from azureml.core import Experiment
from azureml.pipeline.core import Pipeline

pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline)
pipeline_run.wait_for_completion(show_output=True)
```

### <a name="download-and-review-output"></a>Çıktıyı indirin ve gözden geçirin

Betikten oluşturulan çıkış dosyasını indirmek için aşağıdaki kodu çalıştırın `batch_scoring.py` . Ardından, Puanlama sonuçlarını araştırın.

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

## <a name="publish-and-run-from-a-rest-endpoint"></a>REST uç noktasından Yayımla ve Çalıştır

İşlem hattını çalışma alanınıza yayımlamak için aşağıdaki kodu çalıştırın. Azure Machine Learning Studio 'daki çalışma alanınızda çalışma geçmişi ve süreler dahil olmak üzere işlem hattı için meta verileri görebilirsiniz. Ardışık düzeni Studio 'dan el ile de çalıştırabilirsiniz.

İşlem hattının yayımlanması, herhangi bir platformda herhangi bir HTTP kitaplığından işlem hattını çalıştırmak için kullanabileceğiniz bir REST uç noktası sağlar.

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", description="Batch scoring using Inception v3 model", version="1.0")

published_pipeline
```

İşlem hattını REST uç noktasından çalıştırmak için, OAuth2 taşıyıcı türünde bir kimlik doğrulama üst bilgisi gerekir. Aşağıdaki örnek etkileşimli kimlik doğrulaması kullanır (çizim amaçları için), ancak otomatik veya gözetimsiz kimlik doğrulaması gerektiren çoğu üretim senaryosunda, [Bu makalede açıklandığı](how-to-setup-authentication.md)gibi hizmet sorumlusu kimlik doğrulamasını kullanın.

Hizmet sorumlusu kimlik doğrulaması, *Azure Active Directory*bir *uygulama kaydı* oluşturulmasını içerir. İlk olarak, bir istemci parolası oluşturun ve ardından hizmet sorumlusu rolünüze Machine Learning çalışma alanınıza *erişim* verirsiniz. [`ServicePrincipalAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?view=azure-ml-py&preserve-view=true)Kimlik doğrulama akışınızı yönetmek için sınıfını kullanın. 

Hem hem de [`InteractiveLoginAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.interactiveloginauthentication?view=azure-ml-py&preserve-view=true) ' `ServicePrincipalAuthentication` den devralınır `AbstractAuthentication` . Her iki durumda da, [`get_authentication_header()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.abstractauthentication?view=azure-ml-py#&preserve-view=trueget-authentication-header--) üst bilgiyi getirmek için işlevini aynı şekilde kullanın:

```python
from azureml.core.authentication import InteractiveLoginAuthentication

interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()
```

`endpoint`Yayımlanan ardışık düzen nesnesinin ÖZELLIĞINDEN Rest URL 'sini alın. REST URL 'sini Azure Machine Learning Studio 'daki çalışma alanınızda de bulabilirsiniz. 

Uç noktaya bir HTTP POST isteği oluşturun. İstekte kimlik doğrulama üst bilgisini belirtin. Deneme adına sahip bir JSON yük nesnesi ekleyin.

Çalıştırmayı tetiklemeye yönelik isteği yapın. Yanıt sözlüğünden anahtara erişmek için kodu ekleyin ve `Id` çalıştırma kimliği değerini alın.

```python
import requests

rest_endpoint = published_pipeline.endpoint
response = requests.post(rest_endpoint, 
                         headers=auth_header, 
                         json={"ExperimentName": "batch_scoring",
                               "ParameterAssignments": {"process_count_per_node": 6}})
run_id = response.json()["Id"]
```

Yeni çalıştırmanın durumunu izlemek için çalıştırma KIMLIĞINI kullanın. Yeni çalıştırmanın tamamlanmasının bir başka 10-15 dakika sürer. 

Yeni çalıştırma öğreticide daha önce çalıştırdığınız işlem hattına benzer şekilde görünür. Tam çıktıyı görüntüleme seçeneğini belirleyebilirsiniz.

```python
from azureml.pipeline.core.run import PipelineRun
from azureml.widgets import RunDetails

published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)
RunDetails(published_pipeline_run).show()
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure Machine Learning öğreticileri çalıştırmayı planlıyorsanız, bu bölümü tamammayın.

### <a name="stop-the-compute-instance"></a>İşlem örneğini durdur

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Her şeyi sil

Oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, hiçbir ücret ödemezsiniz:

1. Azure portal, sol taraftaki menüden **kaynak grupları**' nı seçin.
1. Kaynak grupları listesinde, oluşturduğunuz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin.
1. Kaynak grubu adını girin. Ardından **Sil**' i seçin.

Ayrıca, kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve sonra **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makine öğrenimi ardışık düzenleri öğreticisinde aşağıdaki görevleri yaptınız:

> [!div class="checklist"]
> * Uzak bir GPU işlem kaynağında çalışacak ortam bağımlılıklarıyla bir işlem hattı oluşturuldu.
> * Önceden eğitilen bir TensorFlow modeli kullanarak toplu tahminleri çalıştırmak için bir Puanlama betiği oluşturuldu.
> * Bir işlem hattı yayımlandı ve bir REST uç noktasından çalıştırılmak üzere etkinleştirildi.

Machine Learning SDK 'Yı kullanarak işlem hattı oluşturma hakkında daha fazla örnek için [Not defteri deposuna](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines)bakın.
