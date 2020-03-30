---
title: Denemeleri TensorBoard ile görselleştirme
titleSuffix: Azure Machine Learning
description: Deneme çalışma geçmişlerini görselleştirmek ve hiperparametre alamı ve yeniden eğitim için potansiyel alanları belirlemek için TensorBoard'u başlatın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: maxluk
ms.author: maxluk
ms.date: 02/27/2020
ms.openlocfilehash: b6b7e47acdbc5bd059e17e512731bd09c8580798
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78195388"
---
# <a name="visualize-experiment-runs-and-metrics-with-tensorboard-and-azure-machine-learning"></a>TensorBoard ve Azure Machine Learning ile deneme çalıştırmalarını ve ölçümlerini görselleştirin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, ana Azure Machine Learning SDK'daki [ `tensorboard` paketi](https://docs.microsoft.com/python/api/azureml-tensorboard/?view=azure-ml-py) kullanarak TensorBoard'daki deneme çalıştırmalarınızı ve ölçümlerinizi nasıl görüntüleyebilirsiniz. Deneme çalışmalarınızı inceledikten sonra, makine öğrenimi modellerinizi daha iyi ayarlayabilir ve yeniden eğitebilirsiniz.

[TensorBoard,](https://www.tensorflow.org/tensorboard/r1/overview) deneme yapınızı ve performansınızı incelemek ve anlamak için bir web uygulama paketidir.

Azure Machine Learning denemeleriyle TensorBoard'u nasıl başlattabileceğiniz denemenin türüne bağlıdır:
+ Denemeniz, PyTorch, Chainer ve TensorFlow denemeleri gibi TensorBoard tarafından tüketilebilen günlük dosyalarını doğal olarak çıktıysa, [TensorBoard'u](#direct) doğrudan denemenin çalışma geçmişinden başlatabilirsiniz. 

+ Scikit-learn veya Azure Machine Learning denemeleri gibi TensorBoard sarf dosyalarının yerel olarak çıktısını yapmayan denemeler için, çalışma geçmişlerini TensorBoard günlükleri olarak dışa aktarmak ve TensorBoard'u oradan başlatmak için [ `export_to_tensorboard()` yöntemi](#export) kullanın. 

> [!TIP]
> Bu belgedeki bilgiler öncelikle model eğitim sürecini izlemek isteyen veri bilimciler ve geliştiriciler içindir. Kotalar, tamamlanmış eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine öğrenimindeki kaynak kullanımını ve etkinlikleri izlemek isteyen bir yöneticiyseniz, [bkz.](monitor-azure-machine-learning.md)

## <a name="prerequisites"></a>Ön koşullar

* TensorBoard'u başlatmak ve deneme çalışma geçmişlerinizi görüntülemek için, denemelerinizin ölçümlerini ve performansını izlemek için daha önce günlüğe kaydetmeyi etkinleştirmiş olması gerekir.  

* Bu belgedeki kod aşağıdaki ortamlardan birinde çalıştırılabilir: 

    * Azure Machine Learning bilgi işlem örneği - indirme veya yükleme ye gerek yok

        * [Öğreticiyi tamamlayın:](tutorial-1st-experiment-sdk-setup.md) SDK ve örnek depoyla önceden yüklenmiş özel bir dizüstü bilgisayar sunucusu oluşturmak için kurulum ortamı ve çalışma alanı.

        * Not defteri sunucusundaki örnekler klasöründe, bu dizinlere yönlendirerek tamamlanmış ve genişletilmiş iki not defteri bulun:
            * **nasıl kullanılır-azureml > eğitim-ile-derin öğrenme > ihracat-run-history-to-tensorboard > ihracat-run-history-to-tensorboard.ipynb**

            * **nasıl kullanılır-azureml > izleme ve izleme-deneyler > tensorboard.ipynb**

    * Kendi Juptyer dizüstü bilgisayar sunucunuz
       * [Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) `tensorboard` ekstra
        * [Azure Machine Learning çalışma alanı oluşturun.](how-to-manage-workspace.md)  
        * [Çalışma alanı yapılandırma dosyası oluşturun.](how-to-configure-environment.md#workspace)
  
<a name="direct"></a>

## <a name="option-1-directly-view-run-history-in-tensorboard"></a>Seçenek 1: TensorBoard'da çalışma geçmişini doğrudan görüntüleme

Bu seçenek, PyTorch, Chainer ve TensorFlow denemeleri gibi TensorBoard tarafından tüketilen günlük dosyalarını doğal olarak çıkan denemeler için çalışır. Denemenizin durumu bu değilse, [ `export_to_tensorboard()` bunun](#export) yerine yöntemi kullanın.

Aşağıdaki örnek kod, TensorFlow'un uzak bir bilgi işlem hedefi olan Azure Machine Learning Compute'daki deposundaki [MNIST demo deneyini](https://raw.githubusercontent.com/tensorflow/tensorflow/r1.8/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py) kullanır. Daha sonra, modelimizi SDK'nın özel [TensorFlow tahmincisi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ile eğitiyoruz ve tensorflow deneyine karşı TensorBoard'u başlatıyoruz, yani TensorBoard olay dosyalarını doğal olarak çıkan bir deney.

### <a name="set-experiment-name-and-create-project-folder"></a>Deneme adını ayarlama ve proje klasörü oluşturma

Burada denemenin adını ve klasörünü oluştururuz. 
 
```python
from os import path, makedirs
experiment_name = 'tensorboard-demo'

# experiment folder
exp_dir = './sample_projects/' + experiment_name

if not path.exists(exp_dir):
    makedirs(exp_dir)

```

### <a name="download-tensorflow-demo-experiment-code"></a>TensorFlow demo deneme kodunu indirin

TensorFlow'un deposunda kapsamlı TensorBoard enstrümantasyonuna sahip bir MNIST demosu vardır. Azure Machine Learning ile çalışması için bu demonun kodunu değiştirmemiz veya değiştirmemiz gerekmez. Aşağıdaki kodda, MNIST kodunu indirip yeni oluşturulan deneme klasörümüzde kaydediyoruz.

```python
import requests
import os

tf_code = requests.get("https://raw.githubusercontent.com/tensorflow/tensorflow/r1.8/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py")
with open(os.path.join(exp_dir, "mnist_with_summaries.py"), "w") as file:
    file.write(tf_code.text)
```
MNIST kod dosyası boyunca, mnist_with_summaries.py, çağrı `tf.summary.scalar()`satırları olduğunu `tf.summary.histogram()` `tf.summary.FileWriter()` fark , vb. Bu yöntemler, çalışma geçmişiyle denemelerinizin temel ölçümlerini grupla, günlük ve etiketleme yöntemidir. TensorBoard'un `tf.summary.FileWriter()` bunlardan görselleştirmeler oluşturmasına olanak tanıyan, günlüğe kaydedilmiş deneme ölçümlerinizdeki verileri seri hale getirerek özellikle önemlidir.

 ### <a name="configure-experiment"></a>Denemeyi yapılandırma

Aşağıda, denememizi yapılandırıp günlükler ve veriler için dizinler ayarlıyoruz. Bu günlükler, TensorBoard'un daha sonra erişediği Artifakı Servisi'ne yüklenir.

>[!Note]
> Bu TensorFlow örneği için TensorFlow'u yerel makinenize yüklemeniz gerekir. Ayrıca, TensorBoard modülü (yani TensorFlow ile birlikte verilen) bu dizüstü bilgisayarın çekirdeğine erişilebilmeli, çünkü tensorboard'u çalıştıran yerel makinedir.

```Python
import azureml.core
from azureml.core import Workspace
from azureml.core import Experiment

ws = Workspace.from_config()

# create directories for experiment logs and dataset
logs_dir = os.path.join(os.curdir, "logs")
data_dir = os.path.abspath(os.path.join(os.curdir, "mnist_data"))

if not path.exists(data_dir):
    makedirs(data_dir)

os.environ["TEST_TMPDIR"] = data_dir

# Writing logs to ./logs results in their being uploaded to Artifact Service,
# and thus, made accessible to our TensorBoard instance.
arguments_list = ["--log_dir", logs_dir]

# Create an experiment
exp = Experiment(ws, experiment_name)
```

### <a name="create-a-cluster-for-your-experiment"></a>Denemeniz için bir küme oluşturma
Bu deney için bir AmlCompute kümesi oluşturuyoruz, ancak deneyleriniz herhangi bir ortamda oluşturulabilir ve deneme nin çalışma geçmişine karşı TensorBoard'u başlatabilirsiniz. 

```Python
from azureml.core.compute import ComputeTarget, AmlCompute

cluster_name = "cpucluster"

cts = ws.compute_targets
found = False
if cluster_name in cts and cts[cluster_name].type == 'AmlCompute':
   found = True
   print('Found existing compute target.')
   compute_target = cts[cluster_name]
if not found:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

compute_target.wait_for_completion(show_output=True, min_node_count=None)

# use get_status() to get a detailed status for the current cluster. 
# print(compute_target.get_status().serialize())
```

### <a name="submit-run-with-tensorflow-estimator"></a>TensorFlow tahmincisi ile çalıştırmayı gönderme

TensorFlow tahmincisi, bir bilgi işlem hedefinde TensorFlow eğitim işini başlatmanın basit bir yolunu sağlar. Herhangi bir çerçeveyi desteklemek [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) için kullanılabilecek genel sınıf aracılığıyla uygulanır. Genel tahminciyi kullanarak eğitim modelleri hakkında daha fazla bilgi [için, tahminci kullanarak Azure Machine Learning'e sahip tren modellerine](how-to-train-ml-models.md) bakın

```Python
from azureml.train.dnn import TensorFlow
script_params = {"--log_dir": "./logs"}

tf_estimator = TensorFlow(source_directory=exp_dir,
                          compute_target=compute_target,
                          entry_script='mnist_with_summaries.py',
                          script_params=script_params)

run = exp.submit(tf_estimator)
```

### <a name="launch-tensorboard"></a>Fırlatma TensorBoard

Çalışmasırasında veya tamamlandıktan sonra TensorBoard başlatabilirsiniz. Aşağıdaki, bir TensorBoard nesne örneği `tb`oluşturmak, bu deneme çalışma geçmişi `run`yüklenen alır ve daha sonra `start()` yöntem ile TensorBoard başlattı. 
  
[TensorBoard oluşturucu](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py) bir dizi çalıştırma alır, bu nedenle emin olun ve tek öğeli bir dizi olarak geçirin.

```python
from azureml.tensorboard import Tensorboard

tb = Tensorboard([run])

# If successful, start() returns a string with the URI of the instance.
tb.start()

# After your job completes, be sure to stop() the streaming otherwise it will continue to run. 
tb.stop()
```

>[!Note]
 Bu örnektensorFlow kullanılırken, TensorBoard PyTorch veya Chainer modelleri ile kolayca kullanılabilir. TensorFlow TensorBoard çalıştıran makinede mevcut olmalıdır, ancak PyTorch veya Chainer hesaplamaları yapan makinede gerekli değildir. 


<a name="export"></a>

## <a name="option-2-export-history-as-log-to-view-in-tensorboard"></a>Seçenek 2: TensorBoard'da görüntülemek için günlük olarak geçmiş iaktar

Aşağıdaki kod örnek bir deneme ayarlar, Azure Machine Learning run history API'lerini kullanarak günlük işlemini başlatır ve deneme nin çalışma geçmişini tensorBoard tarafından görselleştirme için tüketilen günlüklere dışa aktarım. 

### <a name="set-up-experiment"></a>Deneme ayarlama

Aşağıdaki kod yeni bir deneme ayarlar ve `root_run`çalışma dizinini adlandırır. 

```python
from azureml.core import Workspace, Experiment
import azuremml.core

# set experiment name and run name
ws = Workspace.from_config()
experiment_name = 'export-to-tensorboard'
exp = Experiment(ws, experiment_name)
root_run = exp.start_logging()
```

Burada diyabet veri setini yüklüyoruz-- scikit-learn ile birlikte gelen dahili küçük bir veri seti, ve bunu test ve eğitim kümelerine bölüyoruz.

```Python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
X, y = load_diabetes(return_X_y=True)
columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
x_train, x_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
data = {
    "train":{"x":x_train, "y":y_train},        
    "test":{"x":x_test, "y":y_test}
}
```

### <a name="run-experiment-and-log-metrics"></a>Deneme ve günlük ölçümlerini çalıştırma

Bu kod için, doğrusal bir regresyon modeli ve günlük `alpha`anahtar ölçümleri, `mse`alfa katsayısı ve ortalama kare hata, çalışma geçmişi.

```Python
from tqdm import tqdm
alphas = [.1, .2, .3, .4, .5, .6 , .7]
# try a bunch of alpha values in a Linear Regression (aka Ridge regression) mode
for alpha in tqdm(alphas):
  # create child runs and fit lines for the resulting models
  with root_run.child_run("alpha" + str(alpha)) as run
 
   reg = Ridge(alpha=alpha)
   reg.fit(data["train"]["x"], data["train"]["y"])    
 
   preds = reg.predict(data["test"]["x"])
   mse = mean_squared_error(preds, data["test"]["y"])
   # End train and eval

# log alpha, mean_squared_error and feature names in run history
   root_run.log("alpha", alpha)
   root_run.log("mse", mse)
```

### <a name="export-runs-to-tensorboard"></a>TensorBoard'a dışa aktarma çalışır

SDK'nın [export_to_tensorboard()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.export?view=azure-ml-py) yöntemiyle, Azure makine öğrenimi denememizin çalışma geçmişini TensorBoard günlüklerine aktarabiliriz, böylece bunları TensorBoard üzerinden görüntüleyebiliriz.  

Aşağıdaki kodda, geçerli çalışma `logdir` dizinimizde klasörü oluştururuz. Bu klasör, deneme çalışma geçmişimizi ve günlüklerimizi `root_run` dışa aktarıp bu çalıştırmayı tamamlanmış olarak işaretlediğimiz yerdir. 

```Python
from azureml.tensorboard.export import export_to_tensorboard
import os

logdir = 'exportedTBlogs'
log_path = os.path.join(os.getcwd(), logdir)
try:
    os.stat(log_path)
except os.error:
    os.mkdir(log_path)
print(logdir)

# export run history for the project
export_to_tensorboard(root_run, logdir)

root_run.complete()
```

>[!Note]
 Ayrıca, çalıştırmanın adını belirterek TensorBoard'a belirli bir çalıştırmayı dışa aktarabilirsiniz`export_to_tensorboard(run_name, logdir)`

### <a name="start-and-stop-tensorboard"></a>TensorBoard'u başlatın ve durdurun
Bu deneme için çalışma geçmişimiz dışa aktarıldıktan sonra, [start()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py#start-start-browser-false-) yöntemiyle TensorBoard'u başlatabiliriz. 

```Python
from azureml.tensorboard import Tensorboard

# The TensorBoard constructor takes an array of runs, so be sure and pass it in as a single-element array here
tb = Tensorboard([], local_root=logdir, port=6006)

# If successful, start() returns a string with the URI of the instance.
tb.start()
```

İşi bittiğinde, TensorBoard nesnesinin [stop()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py#stop--) yöntemini aradığından emin olun. Aksi takdirde, TensorBoard not defteri çekirdeğini kapatana kadar çalışmaya devam edecektir. 

```python
tb.stop()
```

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl-toniçin, iki deney oluşturdu ve potansiyel ayarı ve yeniden eğitim alanları belirlemek için kendi çalışma geçmişlerine karşı TensorBoard başlatmak için nasıl öğrendim. 

* Modelinizden memnunsanız, model makalesini [nasıl dağıtabileceğinize](how-to-deploy-and-where.md) gidin. 
* [Hiperparametre hakkında](how-to-tune-hyperparameters.md)daha fazla bilgi edinin. 
