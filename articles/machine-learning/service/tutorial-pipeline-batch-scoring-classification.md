---
title: 'Öğretici: Toplu Puanlama için Azure ML işlem hatları'
titleSuffix: Azure Machine Learning service
description: Bir görüntü sınıflandırma modelinde toplu Puanlama çalıştırmak için bir ML işlem hattı oluşturun. Makine öğrenimi ardışık düzenleri, iş akışınızı hız, taşınabilirlik ve yeniden kullanım açısından iyileştirerek altyapı ve otomasyon yerine uzmanınıza, makine öğrenimine odaklanmanıza olanak sağlayabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 09/05/2019
ms.openlocfilehash: 039dbc435cbffed9fe2a9e9be0053d5eb663e9bb
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997358"
---
# <a name="use-azure-machine-learning-pipelines-for-batch-scoring"></a>Batch Puanlama için Azure Machine Learning işlem hatları kullanma

Bu öğreticide, toplu Puanlama veya çıkarım işi çalıştırmak için Azure Machine Learning hizmet işlem hatlarını kullanırsınız. Bu örnek iş, etiketli görüntüleri sınıflandırmak için önceden eğitilen [Inception-v3](https://arxiv.org/abs/1512.00567) evsel sinir Network TensorFlow modelini kullanır. İşlem hattını oluşturup yayımladıktan sonra, işlem hattını herhangi bir platformda herhangi bir HTTP kitaplığından tetikleyebilmeniz için bir REST uç noktası yapılandırırsınız.

Makine öğrenimi ardışık düzenleri, iş akışınızı hız, taşınabilirlik ve yeniden kullanım açısından iyileştirerek altyapı ve otomasyon yerine uzmanınıza, makine öğrenimine odaklanmanıza olanak sağlayabilir. [Ml işlem hatları hakkında daha fazla bilgi edinin](concept-ml-pipelines.md).

Bu öğreticide aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Çalışma alanını yapılandırma ve örnek verileri indirme
> * Verileri getirmek ve çıkarmak için veri nesneleri oluşturma
> * Çalışma alanınıza modeli indirin, hazırlayın ve kaydedin
> * İşlem hedeflerini sağlama ve bir Puanlama betiği oluşturma
> * İşlem hattı oluşturma, çalıştırma ve yayımlama
> * İşlem hattı için bir REST uç noktasını etkinleştirme

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="prerequisites"></a>Önkoşullar

* Zaten bir Azure Machine Learning hizmet çalışma alanınız veya Not defteri sanal makineniz yoksa [Kurulum öğreticisinin 1. kısmını](tutorial-1st-experiment-sdk-setup.md) doldurun.
* Kurulum öğreticisini tamamladıktan sonra, aynı not defteri sunucusunu kullanarak **Öğreticiler/Tutorial-Pipeline-Batch-Scoring-Classification. ipynb** Not defterini açın.

Bu öğretici, kendi [Yerel ortamınızda](how-to-configure-environment.md#local)çalıştırmak istiyorsanız [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials) 'da da kullanılabilir. Gerekli `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-pipeline-steps pandas requests` paketleri almak için öğesini çalıştırın.

## <a name="configure-workspace-and-create-datastore"></a>Çalışma alanını yapılandırma ve veri deposu oluşturma

Mevcut Azure Machine Learning çalışma alanından bir çalışma alanı nesnesi oluşturun. 
+ [Çalışma alanı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) , Azure aboneliğinizi ve kaynak bilgilerinizi kabul eden bir sınıftır. Ayrıca, modelinizi izlemek ve izlemek için bir bulut kaynağı oluşturur. 

+ `Workspace.from_config()`**config. JSON** dosyasını okur ve kimlik doğrulama ayrıntılarını adlı `ws`bir nesneye yükler. Bu öğreticideki kodun kalanında `ws` kullanılır.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

### <a name="create-a-datastore-for-sample-images"></a>Örnek görüntüler için bir veri deposu oluşturma

`sampledata` Hesapta`pipelinedata`genel blob kapsayıcısından ımagenet değerlendirmesi genel veri örneğini alın. Çağırma `register_azure_blob_container()` , verileri ad `images_datastore`altında çalışma alanı için kullanılabilir hale getirir. Ardından, işlem hattındaki Puanlama çıkışı için kullandığınız çıkış veri deposu olarak çalışma alanı varsayılan veri deposunu belirtin.

```python
from azureml.core.datastore import Datastore

batchscore_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="images_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata", 
                      overwrite=True)

def_data_store = ws.get_default_datastore()
```

## <a name="create-data-objects"></a>Veri nesneleri oluşturma

İşlem hatları oluştururken, `DataReference` nesneler çalışma alanı veri depolarından verileri okumak için kullanılır ve `PipelineData` nesneler işlem hattı adımları arasında ara verileri aktarmak için kullanılır.

> [!Important]
> Bu toplu işlem Puanlama örneği yalnızca bir ardışık düzen adımını kullanır, ancak birden çok adımla birlikte kullanım durumlarında, tipik akış şunları içerir:
>
> 1. Ham `DataReference` verileri getirmek, bazı dönüştürmeleri gerçekleştirmek ve sonra bir `PipelineData` nesnenin **çıktısını** almak için nesneleri **giriş** olarak kullanma.
>
> 2. Sonraki adımlar için yinelenen bir `PipelineData` *giriş nesnesi*olarak önceki adımın **Çıkış nesnesini** kullanın.

Bu senaryo için hem giriş `DataReference` görüntüleri hem de sınıflandırma etiketleri (y-test değerleri) için veri deposu dizinlerine karşılık gelen nesneler oluşturursunuz. Toplu Puanlama çıkış verileri `PipelineData` için de bir nesne oluşturun.

```python
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

input_images = DataReference(datastore=batchscore_blob, 
                             data_reference_name="input_images",
                             path_on_datastore="batchscoring/images",
                             mode="download"
                            )

label_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_labels",
                          path_on_datastore="batchscoring/labels",
                          mode="download"                          
                         )

output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

## <a name="download-and-register-the-model"></a>Modeli indir ve Kaydet

İşlem hattındaki toplu Puanlama için kullanmak üzere önceden eğitilen TensorFlow modelini indirin. Önce modeli depoladığınız bir yerel dizin oluşturun, ardından indirin ve ayıklayın.

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

Artık modeli, işlem hattı işleminde kolayca almanızı sağlayan çalışma alanınıza kaydedersiniz. `register()` Statik işlevde`model_name` parametresi SDK genelinde modelinizi bulmak için kullandığınız anahtardır.

```python
from azureml.core.model import Model
 
model = Model.register(model_path="models/inception_v3.ckpt",
                       model_name="inception",
                       tags={"pretrained": "inception"},
                       description="Imagenet trained tensorflow inception",
                       workspace=ws)
```

## <a name="create-and-attach-remote-compute-target"></a>Uzaktan işlem hedefi oluştur ve Ekle

ML işlem hatları yerel olarak çalıştırılamaz, bunları bulut kaynakları üzerinde çalıştırmanız gerekir. Bunlar, denemeleri ve ML iş akışlarını çalıştırdığınız yeniden kullanılabilir sanal işlem ortamları olan uzaktan işlem hedefleri olarak adlandırdık. GPU etkin [`AmlCompute`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) bir hedef oluşturmak ve çalışma alanınıza eklemek için aşağıdaki kodu çalıştırın. İşlem hedefleri hakkında daha fazla bilgi için [kavramsal makaleye](https://docs.microsoft.com/azure/machine-learning/service/concept-compute-target) bakın.


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

Puanlama yapmak için bir Batch Puanlama betiği `batch_scoring.py`oluşturun ve geçerli dizine yazın. Betik, giriş görüntülerini alır, sınıflandırma modelini uygular ve tahmin listesini bir sonuç dosyasına verir.

Betik `batch_scoring.py` , bu öğreticide daha sonra oluşturduğunuz ardışık düzen adımından geçirilen aşağıdaki parametreleri alır:

- `--model_name`: kullanılan modelin adı
- `--label_dir`: `labels.txt` dosyayı tutan Dizin 
- `--dataset_path`: giriş görüntülerini içeren dizin
- `--output_dir`: komut dosyası, bu dizine ait verileri ve çıktıyı `results-label.txt` bu dizine çalıştırır
- `--batch_size`: modeli çalıştırırken kullanılan toplu iş boyutu

İşlem hatları altyapısı, parametreleri `ArgumentParser` işlem hattı adımlarına geçirmek için sınıfını kullanır. Örneğin, ilk bağımsız değişkenin `--model_name` altındaki kodda özellik tanımlayıcısı `model_name`verilir. İşlevinde, bu özelliğe kullanılarak `Model.get_model_path(args.model_name)`erişilir. `main()`


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
from azureml.core.model import Model

slim = tf.contrib.slim

parser = argparse.ArgumentParser(description="Start a tensorflow model serving")
parser.add_argument('--model_name', dest="model_name", required=True)
parser.add_argument('--label_dir', dest="label_dir", required=True)
parser.add_argument('--dataset_path', dest="dataset_path", required=True)
parser.add_argument('--output_dir', dest="output_dir", required=True)
parser.add_argument('--batch_size', dest="batch_size", type=int, required=True)

args = parser.parse_args()

image_size = 299
num_channel = 3

# create output directory if it does not exist
os.makedirs(args.output_dir, exist_ok=True)


def get_class_label_dict(label_file):
    label = []
    proto_as_ascii_lines = tf.gfile.GFile(label_file).readlines()
    for l in proto_as_ascii_lines:
        label.append(l.rstrip())
    return label


class DataIterator:
    def __init__(self, data_dir):
        self.file_paths = []
        image_list = os.listdir(data_dir)
        self.file_paths = [data_dir + '/' + file_name.rstrip() for file_name in image_list]
        self.labels = [1 for file_name in self.file_paths]

    @property
    def size(self):
        return len(self.labels)

    def input_pipeline(self, batch_size):
        images_tensor = tf.convert_to_tensor(self.file_paths, dtype=tf.string)
        labels_tensor = tf.convert_to_tensor(self.labels, dtype=tf.int64)
        input_queue = tf.train.slice_input_producer([images_tensor, labels_tensor], shuffle=False)
        labels = input_queue[1]
        images_content = tf.read_file(input_queue[0])

        image_reader = tf.image.decode_jpeg(images_content, channels=num_channel, name="jpeg_reader")
        float_caster = tf.cast(image_reader, tf.float32)
        new_size = tf.constant([image_size, image_size], dtype=tf.int32)
        images = tf.image.resize_images(float_caster, new_size)
        images = tf.divide(tf.subtract(images, [0]), [255])

        image_batch, label_batch = tf.train.batch([images, labels], batch_size=batch_size, capacity=5 * batch_size)
        return image_batch


def main(_):
    label_file_name = os.path.join(args.label_dir, "labels.txt")
    label_dict = get_class_label_dict(label_file_name)
    classes_num = len(label_dict)
    test_feeder = DataIterator(data_dir=args.dataset_path)
    total_size = len(test_feeder.labels)
    count = 0

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
        with open(out_filename, "w") as result_file:
            i = 0
            while count < total_size and not coord.should_stop():
                test_images_batch = sess.run(test_images)
                file_names_batch = test_feeder.file_paths[i * args.batch_size:
                                                          min(test_feeder.size, (i + 1) * args.batch_size)]
                results = sess.run(probabilities, feed_dict={input_images: test_images_batch})
                new_add = min(args.batch_size, total_size - count)
                count += new_add
                i += 1
                for j in range(new_add):
                    result_file.write(os.path.basename(file_names_batch[j]) + ": " + label_dict[results[j]] + "\n")
                result_file.flush()
            coord.request_stop()
            coord.join(threads)

        shutil.copy(out_filename, "./outputs/")

if __name__ == "__main__":
    tf.app.run()

```

> [!TIP]
> Bu öğreticideki işlem hattı yalnızca bir adıma sahiptir ve çıktıyı bir dosyaya yazar, ancak birden çok adımlı işlem hatları için, sonraki adımlara giriş için `ArgumentParser` çıkış verilerini yazmak üzere bir dizin tanımlamak üzere öğesini de kullanabilirsiniz. `ArgumentParser` Tasarım düzenini kullanarak birden çok işlem hattı adımı arasında veri geçirme örneği için [Not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb) bakın.

## <a name="build-and-run-the-pipeline"></a>İşlem hattını oluşturma ve çalıştırma

İşlem hattını çalıştırmadan önce, komut dosyası `batch_scoring.py`için gereken Python ortamını ve bağımlılıklarını tanımlayan bir nesne oluşturun. Gerekli ana bağımlılık TensorFlow ' dır, ancak SDK 'dan arka `azureml-defaults` plan süreçlerini de yüklersiniz. Bağımlılıkları kullanarak `RunConfiguration` bir nesne oluşturun ve Docker ve Docker-GPU desteğini de belirtin.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE
from azureml.core.runconfig import CondaDependencies, RunConfiguration

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.13.1", "azureml-defaults"])

amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="parameterize-the-pipeline"></a>İşlem hattını Parametreleştirme

İşlem hattının toplu iş boyutunu denetlemek için özel bir parametre tanımlayın. İşlem hattı yayımlandıktan ve bir REST uç noktası aracılığıyla sunulduktan sonra, yapılandırılan tüm parametreler de sunulur ve işlem hattı bir HTTP isteğiyle yeniden çalıştırıldığında JSON yükünde belirtilebilir.

Bu davranışı `PipelineParameter` etkinleştirmek için bir nesne oluşturun ve bir ad ve varsayılan değer tanımlayın.

```python
from azureml.pipeline.core.graph import PipelineParameter
batch_size_param = PipelineParameter(name="param_batch_size", default_value=20)
```

### <a name="create-the-pipeline-step"></a>İşlem hattı adım oluşturma

İşlem hattı adımı, aşağıdakiler dahil olmak üzere bir işlem hattı çalıştırmak için ihtiyacınız olan her şeyi kapsülleyen bir nesnedir:

* ortam ve bağımlılık ayarları
* işlem hattının çalıştırılacağı işlem kaynağı
* giriş ve çıkış verileri ve tüm özel parametreler
* adım sırasında çalışacak bir betik veya SDK mantığı başvurusu

Belirli çerçeveleri ve yığınları kullanarak bir adım oluşturmaya yardımcı olmak [`PipelineStep`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py) için üst sınıftan kalıtımla alan birden çok sınıf vardır. Bu örnekte, özel bir Python betiği [`PythonScriptStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) kullanarak adım mantığınızı tanımlamak için sınıfını kullanırsınız. Betiğinizin bir bağımsız değişkeninin adımın adım veya çıkışının bir girişi olduğunu, **hem** `arguments` dizide `input` hem **de ya** `output` da parametresinde tanımlanması gerektiğini unutmayın. 

`outputs` Dizideki bir nesne başvurusu sonraki bir işlem hattı adımı için **giriş** olarak, birden fazla adımın olduğu senaryolar için kullanılabilir hale gelir.

```python
from azureml.pipeline.steps import PythonScriptStep

batch_score_step = PythonScriptStep(
    name="batch_scoring",
    script_name="batch_scoring.py",
    arguments=["--dataset_path", input_images, 
               "--model_name", "inception",
               "--label_dir", label_dir, 
               "--output_dir", output_dir, 
               "--batch_size", batch_size_param],
    compute_target=compute_target,
    inputs=[input_images, label_dir],
    outputs=[output_dir],
    runconfig=amlcompute_run_config
)
```

Farklı adım türleri için tüm sınıfların bir listesi için, bkz. [adımlar paketi](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py).

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma

Artık işlem hattını çalıştırırsınız. Önce çalışma alanı `Pipeline` başvurunuz ve oluşturduğunuz işlem hattı adımınızla bir nesne oluşturun. `steps` Parametresi bir adımlar dizisidir ve bu durumda Batch Puanlama için yalnızca bir adım vardır. Birden çok adımla işlem hatları oluşturmak için, bu dizideki adımları sırayla yerleştirebilirsiniz.

Sonra, `Experiment.submit()` işlem hattını yürütmeye göndermek için işlevini kullanın. Özel parametresini `param_batch_size`de belirtirsiniz. `wait_for_completion` İşlevi, işlem hattı derleme işlemi sırasında günlükleri çıktı olarak verir ve bu da geçerli ilerlemeyi görmenizi sağlar.

> [!IMPORTANT]
> İlk işlem hattı çalıştırması yaklaşık **15 dakika**sürer, tüm bağımlılıklar indirilmelidir, bir Docker görüntüsü oluşturulur ve Python ortamı sağlanır/oluşturulur. Yeniden çalıştırmak, bu kaynaklar yeniden kullanıldığından önemli ölçüde daha az zaman alır. Ancak, toplam çalışma süresi her bir işlem hattı adımında çalışan betiklerinizin ve süreçlerin iş yüküne bağlıdır.

```python
from azureml.core import Experiment
from azureml.pipeline.core import Pipeline

pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline, pipeline_parameters={"param_batch_size": 20})
pipeline_run.wait_for_completion(show_output=True)
```

### <a name="download-and-review-output"></a>Çıktıyı indirin ve gözden geçirin

`batch_scoring.py` Betikten oluşturulan çıkış dosyasını indirmek için aşağıdaki kodu çalıştırın, sonra Puanlama sonuçlarını araştırın.

```python
import pandas as pd

step_run = list(pipeline_run.get_children())[0]
step_run.download_file("./outputs/result-labels.txt")

df = pd.read_csv("result-labels.txt", delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Dosya adı</th>
      <th>Tahmin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>ILSVRC2012_val_00000102. JPEG</td>
      <td>Rhodesian bir rhogeback</td>
    </tr>
    <tr>
      <td>1\.</td>
      <td>ILSVRC2012_val_00000103. JPEG</td>
      <td>Tripod</td>
    </tr>
    <tr>
      <td>2</td>
      <td>ILSVRC2012_val_00000104. JPEG</td>
      <td>daktilo klavyesi</td>
    </tr>
    <tr>
      <td>3</td>
      <td>ILSVRC2012_val_00000105. JPEG</td>
      <td>silici korun</td>
    </tr>
    <tr>
      <td>4</td>
      <td>ILSVRC2012_val_00000106. JPEG</td>
      <td>Wınwınör kravat</td>
    </tr>
    <tr>
      <td>5</td>
      <td>ILSVRC2012_val_00000107. JPEG</td>
      <td>Harvestman</td>
    </tr>
    <tr>
      <td>6</td>
      <td>ILSVRC2012_val_00000108. JPEG</td>
      <td>Keman</td>
    </tr>
    <tr>
      <td>7</td>
      <td>ILSVRC2012_val_00000109. JPEG</td>
      <td>Hoparlörü</td>
    </tr>
    <tr>
      <td>8</td>
      <td>ILSVRC2012_val_00000110. JPEG</td>
      <td>Apron</td>
    </tr>
    <tr>
      <td>9</td>
      <td>ILSVRC2012_val_00000111. JPEG</td>
      <td>American Lobster</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="publish-and-run-from-rest-endpoint"></a>REST uç noktasından Yayımla ve Çalıştır

İşlem hattını çalışma alanınıza yayımlamak için aşağıdaki kodu çalıştırın. Portaldaki çalışma alanınızda çalışma geçmişi ve süreler dahil olmak üzere işlem hattı için meta verileri görebilirsiniz. Ardışık düzeni portaldan el ile de çalıştırabilirsiniz.

Ayrıca, işlem hattını yayımlamak, bir REST uç noktasının herhangi bir platformda herhangi bir HTTP kitaplığından işlem hattını yeniden çalıştırabilmesini sağlar.

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", description="Batch scoring using Inception v3 model", version="1.0")

published_pipeline
```

İşlem hattını REST uç noktasından çalıştırmak için önce bir OAuth2 taşıyıcı türü kimlik doğrulama üst bilgisi gerekir. Bu örnek, çizim amacıyla etkileşimli kimlik doğrulaması kullanır, ancak otomatik veya gözetimsiz kimlik doğrulaması gerektiren çoğu üretim senaryosunda, [Bu not defterinde açıklandığı](https://aka.ms/pl-restep-auth)gibi hizmet ilkesi kimlik doğrulamasını kullanın.

Hizmet sorumlusu kimlik doğrulaması, **Azure Active Directory**'de bir **uygulama kaydı** oluşturulmasını, bir istemci gizli anahtarı oluşturmayı ve ardından hizmet sorumlusu rolü için makine **öğrenimi** çalışma alanınıza izin vermeyi içerir. Ardından, [`ServicePrincipalAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?view=azure-ml-py) kimlik doğrulama akışınızı yönetmek için sınıfını kullanırsınız. 

Her `InteractiveLoginAuthentication` ikisi `ServicePrincipalAuthentication` `AbstractAuthentication` ve`get_authentication_header()` ' den devralma ve her iki durumda da, bu işlevi üst bilgiyi getirmek için aynı şekilde kullanırsınız.

```python
from azureml.core.authentication import InteractiveLoginAuthentication

interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()
```

Yayımlanan ardışık düzen nesnesinin `endpoint` özelliğinden Rest URL 'sini alın. Ayrıca, portalda çalışma alanınızda REST URL 'sini de bulabilirsiniz. Bitiş noktasına bir HTTP POST isteği oluşturun ve kimlik doğrulama üst bilgisini belirtin. Ayrıca, deneme adı ve toplu iş boyutu parametresiyle bir JSON yük nesnesi ekleyin. Bir anımsatıcı `param_batch_size` olarak, bunu adım yapılandırmasında bir `PipelineParameter` nesne olarak `batch_scoring.py` tanımladığınız için betiğe geçirilir.

Çalıştırmayı tetiklemeye yönelik isteği yapın. Çalıştırma kimliğinin değerini almak için yanıt sözlüğünden anahtaraerişin.`Id`

```python
import requests

rest_endpoint = published_pipeline.endpoint
response = requests.post(rest_endpoint, 
                         headers=auth_header, 
                         json={"ExperimentName": "batch_scoring",
                               "ParameterAssignments": {"param_batch_size": 50}})
run_id = response.json()["Id"]
```

Yeni çalıştırmanın durumunu izlemek için çalıştırma KIMLIĞINI kullanın. Bu, çalıştırmak için başka bir 10-15 dk sürer ve önceki işlem hattı çalıştırmasına benzer şekilde görünür. bu nedenle, başka bir işlem hattı çalıştırmasını görmeniz gerekmiyorsa, tam çıktıyı izlemeyi atlayabilirsiniz.

```python
from azureml.pipeline.core.run import PipelineRun
from azureml.widgets import RunDetails

published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)
RunDetails(published_pipeline_run).show()
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure Machine Learning hizmeti öğreticilerini çalıştırmayı planlıyorsanız, bu bölümü tamamlamayın.

### <a name="stop-the-notebook-vm"></a>Not defteri VM 'sini durdur

Bir bulut Not defteri sunucusu kullandıysanız, maliyeti azaltmak için kullanmadığınız sanal makineyi durdurun.

1. Çalışma alanınızda, **Not defteri VM 'leri**' ni seçin.
1. Listeden VM’yi seçin.
1. **Durdur**' u seçin.
1. Sunucuyu yeniden kullanmaya hazırsanız **Başlat**' ı seçin.

### <a name="delete-everything"></a>Her şeyi sil

Oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, herhangi bir ücret ödemezsiniz.

1. Azure portalının en sol tarafındaki **Kaynak gruplarını** seçin.
1. Listeden oluşturduğunuz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin.
1. Kaynak grubu adını girin. Ardından **Sil**’i seçin.

Ayrıca, kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makine öğrenimi ardışık düzenleri öğreticisinde aşağıdaki görevleri yaptınız:

> [!div class="checklist"]
> * Uzak bir GPU işlem kaynağında çalıştırılacak ortam bağımlılıklarıyla bir işlem hattı derleme
> * Önceden eğitilen bir TensorFlow modeliyle toplu tahminleri çalıştırmak için bir Puanlama betiği oluşturuldu
> * Bir işlem hattı yayımlandı ve bir REST uç noktasından çalıştırılmak üzere etkinleştirildi

Machine Learning SDK ile işlem hatları oluşturmaya yönelik ek örnekler için [Not defteri deposuna](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines) bakın.
