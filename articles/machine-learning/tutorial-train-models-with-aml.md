---
title: 'Resim sınıflandırma öğretici: Tren modelleri'
titleSuffix: Azure Machine Learning
description: Python Jupyter dizüstü bilgisayarında scikit-learn içeren bir görüntü sınıflandırma modeli eğitmek için Azure Machine Learning'i kullanın. Bu öğretici iki bölümbirdir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sdgilley
ms.author: sgilley
ms.date: 02/10/2020
ms.custom: seodec18
ms.openlocfilehash: 8cf46db06a4a2f8fa86f97dab5a8477cf427c999
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80159098"
---
# <a name="tutorial-train-image-classification-models-with-mnist-data-and-scikit-learn"></a>Öğretici: MNIST verileri ile tren görüntü sınıflandırma modelleri ve scikit-öğrenmek 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu eğitimde, uzaktan bilgi işlem kaynakları üzerinde bir makine öğrenme modeli eğitin. Azure Machine Learning için eğitim ve dağıtım iş akışını Python Jupyter dizüstü bilgisayarında kullanırsınız.  Ardından not defterini şablon olarak kullanıp kendi verilerinizle kendi makine öğrenmesi modelinizi eğitebilirsiniz. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin birinci bölümüdür**.  

Bu öğretici, [MNIST](http://yann.lecun.com/exdb/mnist/) veri kümesini kullanarak basit bir lojistik gerileme yi eğitiyor ve Azure Machine Learning ile [scikit-learn'i öğreniyor.](https://scikit-learn.org) MNIST, 70.000 gri tonlamalı resimden oluşan popüler bir veri kümesidir. Her görüntü, sıfırdan dokuza kadar olan bir sayıyı temsil eden 28 x 28 piksellik el yazısı bir rakamdır. Amaç, belirli bir resim temsil ettiği rakamı tanımlamak için çok sınıflı bir sınıflandırıcı oluşturmaktır.

Aşağıdaki eylemleri nasıl gerçekleştirdiğini öğrenin:

> [!div class="checklist"]
> * Geliştirme ortamınızı ayarlayın.
> * Verilere erişin ve inceleyin.
> * Uzak bir küme üzerinde basit bir lojistik regresyon modeli eğitin.
> * Eğitim sonuçlarını gözden geçirin ve en iyi modeli kaydedin.

Nasıl bir model seçmek ve [bu öğretici bölüm iki](tutorial-deploy-models-with-aml.md)dağıtmak öğrenin.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

>[!NOTE]
> Bu makaledeki kod [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) sürüm 1.0.65 ile test edilmiştir.

## <a name="prerequisites"></a>Ön koşullar

* [Öğreticiyi tamamlayın: Aşağıdakiler için ilk Azure ML denemenizi oluşturmaya başlayın:](tutorial-1st-experiment-sdk-setup.md)
    * Çalışma alanı oluşturma
    * Çalışma alanında öğreticiler not defterini klasörünüze kopyala.
    * Bulut tabanlı bir işlem örneği oluşturun.

* Klonlanmış *öğreticiler/resim-sınıflandırma-mnist-veri* *klasöründe, img-classification-part1-training.ipynb* not defterini açın. 


Öğretici ve eşlik eden **utils.py** dosyası, kendi [yerel ortamınızda](how-to-configure-environment.md#local)kullanmak isterseniz [GitHub'da](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials) da mevcuttur. Bu `pip install azureml-sdk[notebooks] azureml-opendatasets matplotlib` öğretici için bağımlılıkları yüklemek için çalıştırın.

> [!Important]
> Bu makalenin geri kalanı, not defterinde gördüğünüz le aynı içeriği içerir.  
>
> Kodu çalıştırırken okumak istiyorsanız hemen Jupyter not defterine geçin. 
> Not defterinde tek bir kod hücresini çalıştırmak için kod hücresini tıklatın ve **Shift+Enter'a**basın. Veya tüm dizüstü bilgisayarı üst araç çubuğundan **Çalıştır'ı** seçerek tüm dizüstü bilgisayarı çalıştırın.

## <a name="set-up-your-development-environment"></a><a name="start"></a>Geliştirme ortamınızı kurma

Geliştirme çalışmanızdaki tüm kurulum bir Python not defterinde gerçekleştirilebilir. Kurulum aşağıdaki eylemleri içerir:

* Python paketlerini içe aktarın.
* Yerel bilgisayarınızın uzak kaynaklarla iletişim kurabilmesi için bir çalışma alanına bağlanın.
* Tüm çalışanlarınızı izlemek için bir deneme oluşturun.
* Eğitim için kullanmak üzere uzak bir işlem hedefi oluşturun.

### <a name="import-packages"></a>Paketleri içeri aktarma

Bu oturumda ihtiyacınız olan Python paketlerini içeri aktarın. Ayrıca Azure Machine Learning SDK sürümünü görüntüleyin:

```python
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt

import azureml.core
from azureml.core import Workspace

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-a-workspace"></a>Çalışma alanına bağlanma

Mevcut çalışma alanından bir çalışma alanı nesnesi oluşturun. `Workspace.from_config()`**config.json** dosyasını okur ve ayrıntıları adlı `ws`bir nesneye yükler:

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, sep='\t')
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Çalışma alanınızdaki çalıştırmaları izleyecek bir deneme oluşturun. Çalışma alanında birden çok deneme olabilir:

```python
from azureml.core import Experiment
experiment_name = 'sklearn-mnist'

exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-or-attach-an-existing-compute-target"></a>Varolan bir işlem hedefi oluşturma veya ekleme

Veri bilimciler, yönetilen bir hizmet olan Azure Machine Learning Compute'u kullanarak, makine öğrenimi modellerini Azure sanal makine kümeleri üzerinde eğitebilir. GPU destekli VM'ler buna örnek olarak verilebilir. Bu eğitimde, eğitim ortamınız olarak Azure Machine Learning Compute'u oluşturursunuz. Python kodunu daha sonra bu VM'de çalıştırmak üzere öğreticide çalışacak şekilde göndereceksiniz. 

Aşağıdaki kod, çalışma alanınızda zaten yoksa sizin için işlem kümelerini oluşturur.

 **İşlem hedefinin oluşturulması yaklaşık beş dakika sürer.** Bilgi işlem kaynağı zaten çalışma alanındaysa, kod bunu kullanır ve oluşturma işlemini atlar.

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
import os

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
    provisioning_config = AmlCompute.provisioning_configuration(vm_size=vm_size,
                                                                min_nodes=compute_min_nodes,
                                                                max_nodes=compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(
        ws, compute_name, provisioning_config)

    # can poll for a minimum number of nodes and for a specific timeout.
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(
        show_output=True, min_node_count=None, timeout_in_minutes=20)

    # For a more detailed view of current AmlCompute status, use get_status()
    print(compute_target.get_status().serialize())
```

Artık bulutta bir modeli eğitmek için gerekli paketleriniz ve işlem kaynaklarınız vardır. 

## <a name="explore-data"></a>Verileri inceleme

Bir modeli eğitmeden önce, onu eğitmek için kullandığınız verileri anlamanız gerekir. Bu bölümde şunları nasıl yapabileceğinizi öğrenirsiniz:

* MNIST veri kümesini indirin.
* Bazı örnek görüntüleri görüntüleyin.

### <a name="download-the-mnist-dataset"></a>MNIST veri kümesini indirme

Ham MNIST veri dosyalarını almak için Azure Açık Veri kümelerini kullanın. [Azure Açık Veri Kümeleri,](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz genel veri kümeleridir. Her veri kümesinin, `MNIST` verileri farklı şekillerde almak için karşılık gelen bir sınıfı vardır.

Bu kod, verileri bir `FileDataset` nesne olarak alır, `Dataset`bu da . Veri `FileDataset` depolarınızda veya genel url'lerinizde herhangi bir biçimde tek veya birden çok dosyaya başvurur. Sınıf, veri kaynağı konumuna bir başvuru oluşturarak dosyaları bilgi işlemnize indirme veya yükleme olanağı sağlar. Ayrıca, eğitim sırasında kolay almak için Veri kümesini çalışma alanınıza kaydedebilirsiniz.

Datasets ve bunların Kullanımı hakkında SDK'da daha fazla bilgi edinmek için [nasıl yapılacağını](how-to-create-register-datasets.md) izleyin.

```python
from azureml.core import Dataset
from azureml.opendatasets import MNIST

data_folder = os.path.join(os.getcwd(), 'data')
os.makedirs(data_folder, exist_ok=True)

mnist_file_dataset = MNIST.get_file_dataset()
mnist_file_dataset.download(data_folder, overwrite=True)

mnist_file_dataset = mnist_file_dataset.register(workspace=ws,
                                                 name='mnist_opendataset',
                                                 description='training and test dataset',
                                                 create_new_version=True)
```

### <a name="display-some-sample-images"></a>Bazı örnek görüntüleri gösterme

Sıkıştırılmış dosyaları `numpy` dizilerine yükleyin. Ardından `matplotlib` kullanarak, üst kısımlarında etiketleriyle veri kümesinden 30 rastgele görüntü çizin. Bu adım, `load_data` bir `util.py` dosyaya dahil edilmiş bir işlev gerektirir. Bu dosya örnek klasöründe bulunur. Bu not defteriyle aynı klasöre yerleştirildiğinden emin olun. İşlev, `load_data` sıkıştırılmış dosyaları sayısal dizilere ayrıştirır.

```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data(os.path.join(data_folder, "train-images-idx3-ubyte.gz"), False) / 255.0
X_test = load_data(os.path.join(data_folder, "t10k-images-idx3-ubyte.gz"), False) / 255.0
y_train = load_data(os.path.join(data_folder, "train-labels-idx1-ubyte.gz"), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, "t10k-labels-idx1-ubyte.gz"), True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize=(16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Rastgele görüntü örnekleri gösterilir:

![Görüntülerin rastgele örneği](./media/tutorial-train-models-with-aml/digits.png)

Artık bu görüntülerin nasıl göründüğü ve beklenen tahmin sonucu hakkında bir fikriniz oldu.

## <a name="train-on-a-remote-cluster"></a>Uzak kümede eğitme

Bu görev için, işi daha önce ayarladığınız uzak eğitim kümesinde çalışacak şekilde gönderirsiniz.  İş göndermek için şunları yaparsınız:
* Dizin oluşturma
* Eğitim betiği oluşturma
* Tahminci nesnesi oluşturma
* İşi gönderme

### <a name="create-a-directory"></a>Dizin oluşturma

Gerekli kodu bilgisayarınızdan uzak kaynağa teslim etmek için bir dizin oluşturun.

```python
script_folder = os.path.join(os.getcwd(), "sklearn-mnist")
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Eğitim betiği oluşturma

İşi kümeye göndermek için, önce bir eğitim betiği oluşturun. Aşağıdaki kodu çalıştırarak, az önce oluşturduğunuz dizinde `train.py` adlı eğitim betiğini oluşturun.

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np
import glob

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the dataset to mount or download, and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = args.data_folder
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(glob.glob(os.path.join(data_folder, '**/train-images-idx3-ubyte.gz'), recursive=True)[0], False) / 255.0
X_test = load_data(glob.glob(os.path.join(data_folder, '**/t10k-images-idx3-ubyte.gz'), recursive=True)[0], False) / 255.0
y_train = load_data(glob.glob(os.path.join(data_folder, '**/train-labels-idx1-ubyte.gz'), recursive=True)[0], True).reshape(-1)
y_test = load_data(glob.glob(os.path.join(data_folder, '**/t10k-labels-idx1-ubyte.gz'), recursive=True)[0], True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_context()

print('Train a logistic regression model with regularization rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, solver="liblinear", multi_class="auto", random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

Betiğin verileri nasıl aldığına ve modelleri nasıl kaydettiğine dikkat edin:

+ Eğitim komut dosyası, verileri içeren dizini bulmak için bir bağımsız değişken okur. Daha sonra işi gönderdiğinizde, bu bağımsız değişken için veri deposuna işaret edersiniz: ```parser.add_argument('--data-folder', type=str, dest='data_folder', help='data directory mounting point')```

+ Eğitim komut dosyası, modelinizi çıkış **adı**verilen bir dizine kaydeder. Bu dizine yazılan her şey otomatik olarak çalışma alanınıza yüklenir. Modelinize daha sonra öğreticide bu dizinden erişebilirsiniz. `joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')`

+ Eğitim komut dosyası, `utils.py` dosyanın veri kümesini doğru şekilde yüklemesini gerektirir. Aşağıdaki kod, `utils.py` dosyaya `script_folder` uzak kaynaktaki eğitim komut dosyasıyla birlikte erişilebilmek için kopyalanır.

  ```python
  import shutil
  shutil.copy('utils.py', script_folder)
  ```

### <a name="create-an-estimator"></a>Tahmin aracı oluşturma

Bir [SKLearn tahminnesnesi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) çalıştır'ı göndermek için kullanılır. Şu öğeleri için aşağıdaki kodu çalıştırın ve tahmin aracınızı oluşturun:

* Tahmin aracı nesnesinin adı: `est`.
* Betiklerinizi içeren dizin. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir.
* Bilgi işlem hedefi. Bu örnekte oluşturduğunuz Azure Machine Learning işlem kümesini kullanacaksınız.
* Eğitim komut dosyası adı, **train.py**.
* Eğitin betiğinden gerekli parametreler.

Bu öğreticide, bu hedef AmlCompute olduğunu. Komut dosyası klasöründeki tüm dosyalar çalıştırmak için küme düğümlerine yüklenir. **data_folder** veri kümesini kullanacak şekilde ayarlanmıştır. İlk olarak, eğitim için gereken bağımlılıkları belirten bir ortam nesnesi oluşturun. 

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

env = Environment('my_env')
cd = CondaDependencies.create(pip_packages=['azureml-sdk','scikit-learn','azureml-dataprep[pandas,fuse]>=1.1.14'])
env.python.conda_dependencies = cd
```

Ardından aşağıdaki kodu içeren tahminci oluşturun.

```python
from azureml.train.sklearn import SKLearn

script_params = {
    '--data-folder': mnist_file_dataset.as_named_input('mnist_opendataset').as_mount(),
    '--regularization': 0.5
}

est = SKLearn(source_directory=script_folder,
              script_params=script_params,
              compute_target=compute_target,
              environment_definition=env, 
              entry_script='train.py')
```

### <a name="submit-the-job-to-the-cluster"></a>İşi kümeye gönderme

Tahminci nesnesini göndererek denemeyi çalıştırın:

```python
run = exp.submit(config=est)
run
```

Arama eşzamanlı olduğundan, iş başlar **başlamaz bir Hazırlama** veya **Çalıştırma** durumunu döndürür.

## <a name="monitor-a-remote-run"></a>Uzaktan çalıştırmayı izleme

Toplamda, ilk çalışma **yaklaşık 10 dakika**sürer. Ancak sonraki çalıştırmalar için, komut dosyası bağımlılıkları değişmediği sürece, aynı görüntü yeniden kullanılır. Yani konteyner başlatma süresi çok daha hızlıdır.

Beklerken ne olur:

- **Görüntü oluşturma**: Tahminci tarafından belirtilen Python ortamıyla eşleşen bir Docker görüntüsü oluşturulur. Görüntü, çalışma alanına yüklenir. Görüntü oluşturma ve yükleme **yaklaşık beş dakika**sürer.

  Kapsayıcı sonraki çalıştırmalar için önbelleğe alındı, çünkü bu aşama her Python ortamı için bir kez olur. Görüntü oluşturma sırasında, günlükler çalıştırma geçmişine aktarılır. Bu günlükleri kullanarak görüntü oluşturma ilerlemesini izleyebilirsiniz.

- **Ölçekleme**: Uzak küme, çalışma için şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, otomatik olarak ek düğümler eklenir. Ölçekleme genellikle **yaklaşık beş dakika sürer.**

- **Çalışma**: Bu aşamada, gerekli komut dosyaları ve dosyalar bilgi işlem hedefine gönderilir. Daha sonra veri depoları monte edilir veya kopyalanır. Ve sonra **entry_script** çalıştırılır. İş çalışırken, **stdout** ve **./günlükdizi** çalışma geçmişine akışı. Bu günlükleri kullanarak çalıştırmanın ilerlemesini izleyebilirsiniz.

- **Post-processing**: Çalışma alanınızdaki **./çıktılar** dizini çalışma alanınızdaki çalışma geçmişine kopyalanır, böylece bu sonuçlara erişebilirsiniz.

Çalışan bir işin ilerlemesini çeşitli şekillerde denetleyebilirsiniz. Bu öğretici bir Jupyter widget ve bir `wait_for_completion` yöntem kullanır.

### <a name="jupyter-widget"></a>Jupyter pencere öğesi

Bir [Jupyter widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)ile koşmak ilerleme izleyin. Çalıştırma gönderimi gibi, widget da eşzamanlıdır ve iş bitene kadar her 10 ila 15 saniyede bir canlı güncellemeler sağlar:

```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

Widget eğitim sonunda aşağıdaki gibi görünecektir:

![Dizüstü widget](./media/tutorial-train-models-with-aml/widget.png)

Bir çalıştırmayı iptal etmeniz gerekiyorsa, [bu yönergeleri](https://aka.ms/aml-docs-cancel-run)izleyebilirsiniz.

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Model eğitimi ve izlemesi arka planda yapılır. Daha fazla kod çalıştırmadan önce modelin eğitimini tamamlayana kadar bekleyin. Model `wait_for_completion` eğitiminin ne zaman tamamlanagerektiğini göstermek için kullanın:

```python
run.wait_for_completion(show_output=False)  # specify True for a verbose log
```

### <a name="display-run-results"></a>Çalıştırma sonuçlarını görüntüleme

Artık uzak düğümde eğitilmiş bir modeliniz vardır. Modelin doğruluğunu alın:

```python
print(run.get_metrics())
```

Çıktı, uzak modelin 0,9204 doğruluk oranına sahip olduğunu gösterir:

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

Bir sonraki öğreticide, bu modeli daha ayrıntılı olarak keşfe

## <a name="register-model"></a>Modeli kaydetme

Eğitim komut dosyasındaki son adım, dosyayı `outputs/sklearn_mnist_model.pkl` `outputs` işin çalıştırıldığı kümenin VM'sinde adlı bir dizinde yazdı. `outputs`bu dizindeki tüm içeriğin otomatik olarak çalışma alanınıza yüklendiği özel bir dizinidir. Bu içerik çalışma alanınızın altında yer alan denemedeki çalıştırma kaydında gösterilir. Yani model dosyası artık çalışma alanınızda da kullanılabilir.

Bu çalıştırmayla ilişkili dosyaları görebilirsiniz:

```python
print(run.get_file_names())
```

Siz veya diğer ortak çalışanlar daha sonra bu modeli sorgulayabilmeniz, inceleyebilmeniz ve dağıtabilmeniz için modeli çalışma alanına kaydedin:

```python
# register model
model = run.register_model(model_name='sklearn_mnist',
                           model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep='\t')
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Ayrıca azure machine learning compute kümesini de silebilirsiniz. Ancak, otomatik ölçek açık ve küme minimum sıfırdır. Bu nedenle, bu özel kaynak kullanılmadığında ek bilgi işlem ücretlerine tabi olmaz:

```python
# Optionally, delete the Azure Machine Learning Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure Machine Learning öğreticisinde, Python'u aşağıdaki görevler için kullandınız:

> [!div class="checklist"]
> * Geliştirme ortamınızı ayarlayın.
> * Verilere erişin ve inceleyin.
> * Popüler scikit-learn machine learning library'yi kullanarak uzak bir kümede birden fazla model eğitin
> * Eğitim ayrıntılarını gözden geçirin ve en iyi modeli kaydedin.

Öğretici serinin bir sonraki bölümündeki yönergeleri kullanarak bu kayıtlı modeli dağıtmaya hazırsınız:

> [!div class="nextstepaction"]
> [Öğretici 2 - Modelleri dağıtma](tutorial-deploy-models-with-aml.md)
