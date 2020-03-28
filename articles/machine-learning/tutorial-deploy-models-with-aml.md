---
title: 'Resim sınıflandırma öğretici: Modelleri dağıtma'
titleSuffix: Azure Machine Learning
description: İki bölümlük bir serinin ikincisi olan bu öğretici, Python Jupyter dizüstü bilgisayarında scikit-learn içeren bir görüntü sınıflandırma modeli dağıtmak için Azure Machine Learning'in nasıl kullanılacağını gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sdgilley
ms.author: sgilley
ms.date: 02/10/2020
ms.custom: seodec18
ms.openlocfilehash: 81e02492f7e79b87e1513a910afe4719908adbbb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80159101"
---
# <a name="tutorial-deploy-an-image-classification-model-in-azure-container-instances"></a>Öğretici: Azure Kapsayıcı Örnekleri'nde görüntü sınıflandırma modelini dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. [Önceki öğreticide](tutorial-train-models-with-aml.md) makine öğrenmesi modellerini eğittiniz ve buluttaki çalışma alanınıza modeli kaydettiniz.  Artık modeli bir web hizmeti olarak dağıtmaya hazırsınız. Bir web hizmeti bir görüntü, bu durumda bir Docker görüntüdür. Puanlama mantığını ve modelin kendisini kapsüller. 

Öğreticinin bu bölümünde, aşağıdaki görevler için Azure Machine Learning'i kullanırsınız:

> [!div class="checklist"]
> * Test ortamınızı ayarlayın.
> * Çalışma alanınızdan modeli alın.
> * Modeli yerel olarak test edin.
> * Modeli Kapsayıcı Örnekleri'ne dağıtın.
> * Dağıtılan modeli test edin.

Kapsayıcı Örnekleri, iş akışını test etmek ve anlamak için harika bir çözümdür. Ölçeklenebilir üretim dağıtımları için Azure Kubernetes Service’i kullanmayı göz önünde bulundurabilirsiniz. Daha fazla bilgi için [nasıl dağıtılanın ve nerede dağıtılanın.](how-to-deploy-and-where.md)

>[!NOTE]
> Bu makaledeki kod Azure Machine Learning SDK sürüm 1.0.41 ile test edilmiştir.

## <a name="prerequisites"></a>Ön koşullar

Not defterini çalıştırmak için, önce Öğretici 'deki model eğitimini tamamlayın [(bölüm 1): Bir görüntü sınıflandırma modeli eğitin.](tutorial-train-models-with-aml.md)   Ardından klonlanmış *öğreticileriniz/resim sınıflandırması-mnist-veri* klasörünüzde *img-classification-part2-deploy.ipynb* not defterini açın.

Bu öğretici, kendi [yerel ortamınızda](how-to-configure-environment.md#local)kullanmak istiyorsanız [GitHub'da](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials) da mevcuttur.  Yüklü `matplotlib` `scikit-learn` ve ortamınızda olduğundan emin olun. 

> [!Important]
> Bu makalenin geri kalanı, not defterinde gördüğünüz le aynı içeriği içerir.  
>
> Kodu çalıştırırken okumak istiyorsanız hemen Jupyter not defterine geçin.
> Not defterinde tek bir kod hücresini çalıştırmak için kod hücresini tıklatın ve **Shift+Enter'a**basın. Veya tüm dizüstü bilgisayarı üst araç çubuğundan **Çalıştır'ı** seçerek tüm dizüstü bilgisayarı çalıştırın.

## <a name="set-up-the-environment"></a><a name="start"></a>Ortamı ayarlama

Başlangıç olarak test ortamını ayarlayın.

### <a name="import-packages"></a>Paketleri içeri aktarma

Bu öğretici için gereken Python paketlerini alma:

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
 
import azureml
from azureml.core import Workspace, Run

# Display the core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="retrieve-the-model"></a>Modeli alma

Önceki öğreticide çalışma alanınızda bir modeli kaydetmiştiniz. Şimdi bu çalışma alanını yükleyin ve modeli yerel dizine indirin:


```python
from azureml.core import Workspace
from azureml.core.model import Model
import os
ws = Workspace.from_config()
model = Model(ws, 'sklearn_mnist')

model.download(target_dir=os.getcwd(), exist_ok=True)

# verify the downloaded model file
file_path = os.path.join(os.getcwd(), "sklearn_mnist_model.pkl")

os.stat(file_path)
```

## <a name="test-the-model-locally"></a>Modeli yerel olarak test etme

Dağıtmadan önce, modelinizin yerel olarak çalıştığından emin olun:
* Test verilerini yükleyin.
* Test verilerini tahmin edin.
* Karışıklık matrisini inceleyin.

### <a name="load-test-data"></a>Test verilerini yükleme

Eğitim eğitimi sırasında oluşturulan **./data/dizindeki** test verilerini yükleyin:

```python
from utils import load_data
import os

data_folder = os.path.join(os.getcwd(), 'data')
# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the neural network converge faster
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_test = load_data(os.path.join(
    data_folder, 'test-labels.gz'), True).reshape(-1)
```

### <a name="predict-test-data"></a>Test verilerini tahmin etme

Öngörüler almak için test veri kümesini modele aktarın:

```python
import pickle
from sklearn.externals import joblib

clf = joblib.load(os.path.join(os.getcwd(), 'sklearn_mnist_model.pkl'))
y_hat = clf.predict(X_test)
```

###  <a name="examine-the-confusion-matrix"></a>Karışıklık matrisini inceleme

Test kümesinden kaç örneğin doğru sınıflandırıldığını görmek için karışıklık matrisi oluşturun. Yanlış öngörüler için yanlış sınıflandırılmış değere dikkat edin: 

```python
from sklearn.metrics import confusion_matrix

conf_mx = confusion_matrix(y_test, y_hat)
print(conf_mx)
print('Overall accuracy:', np.average(y_hat == y_test))
```

Çıkış, karışıklık matrisini gösterir:

    [[ 960    0    1    2    1    5    6    3    1    1]
     [   0 1112    3    1    0    1    5    1   12    0]
     [   9    8  920   20   10    4   10   11   37    3]
     [   4    0   17  921    2   21    4   12   20    9]
     [   1    2    5    3  915    0   10    2    6   38]
     [  10    2    0   41   10  770   17    7   28    7]
     [   9    3    7    2    6   20  907    1    3    0]
     [   2    7   22    5    8    1    1  950    5   27]
     [  10   15    5   21   15   27    7   11  851   12]
     [   7    8    2   13   32   13    0   24   12  898]]
    Overall accuracy: 0.9204
   

Karışıklık matrisini grafik olarak görüntülemek için `matplotlib` kullanın. Bu grafikte, x ekseni gerçek değerleri, y ekseni ise öngörülen değerleri gösterir. Her ızgaradaki renk hata oranını gösterir. Renk ne kadar açıksa hata oranı da o kadar yüksektir. Örneğin, birçok 5's 3 olarak yanlış sınıflandırılır. Yani (5,3) parlak bir ızgara bakın:

```python
# normalize the diagonal cells so that they don't overpower the rest of the cells when visualized
row_sums = conf_mx.sum(axis=1, keepdims=True)
norm_conf_mx = conf_mx / row_sums
np.fill_diagonal(norm_conf_mx, 0)

fig = plt.figure(figsize=(8, 5))
ax = fig.add_subplot(111)
cax = ax.matshow(norm_conf_mx, cmap=plt.cm.bone)
ticks = np.arange(0, 10, 1)
ax.set_xticks(ticks)
ax.set_yticks(ticks)
ax.set_xticklabels(ticks)
ax.set_yticklabels(ticks)
fig.colorbar(cax)
plt.ylabel('true labels', fontsize=14)
plt.xlabel('predicted values', fontsize=14)
plt.savefig('conf.png')
plt.show()
```

![Karışıklık matrisini gösteren grafik](./media/tutorial-deploy-models-with-aml/confusion.png)

## <a name="deploy-as-a-web-service"></a>Web hizmeti olarak dağıtma

Modeli test ettikten ve sonuçlardan memnun olduktan sonra, modeli Kapsayıcı Örnekleri'nde barındırılan bir web hizmeti olarak dağıtın. 

Kapsayıcı Örnekleri için doğru ortamı oluşturmak için aşağıdaki bileşenleri sağlayın:
* Modelin nasıl kullanılacağını gösteren bir puanlama komut dosyası.
* Hangi paketlerin yüklenmesi gerektiğini gösteren bir ortam dosyası.
* Kapsayıcı örneğini oluşturmak için bir yapılandırma dosyası.
* Daha önce eğittin model.

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Puanlama betiği oluşturma

**score.py**adlı puanlama komut dosyası oluşturun. Web hizmeti çağrısı, modelin nasıl kullanılacağını göstermek için bu komut dosyasını kullanır.

Puanlama komut dosyasına bu iki gerekli işlevi ekleyin:
* Normalde modeli genel bir nesneye yükleyen `init()` işlevi. Bu işlev, Docker kapsayıcısı başlatıldığında tek bir kez çalıştırılır. 

* `run(input_data)` işlevi, giriş verileri temelinde bir değer tahmin etmek için modeli kullanır. Çalıştırmanın girişleri ve çıkışlarında serileştirme ve seriden çıkarma için normal olarak JSON kullanılır ama başka biçimler de desteklenir.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

def init():
    global model
    # retrieve the path to the model file using the model name
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_mnist_model.pkl')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    # you can return any data type as long as it is JSON-serializable
    return y_hat.tolist()
```

<a name="make-myenv"></a>

### <a name="create-environment-file"></a>Ortam dosyası oluşturma

Sonraki **myenv.yml**adlı bir ortam dosyası oluşturmak , komut dosyasının tüm paket bağımlılıkları belirtir. Bu dosya, tüm bu bağımlılıkların Docker görüntüsüne yüklendiğinden emin olmak için kullanılır. Bu modele `scikit-learn` ve `azureml-sdk` gerekir. Tüm özel ortam dosyaları, verion >= 1,0,45 pip bağımlılığı olarak azureml varsayılanlarını listelemalı. Bu paket, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerir.

```python
from azureml.core.conda_dependencies import CondaDependencies

myenv = CondaDependencies()
myenv.add_conda_package("scikit-learn")
myenv.add_pip_package("azureml-defaults")

with open("myenv.yml", "w") as f:
    f.write(myenv.serialize_to_string())
```
Dosyanın içeriğini `myenv.yml` gözden geçirin:

```python
with open("myenv.yml", "r") as f:
    print(f.read())
```

### <a name="create-a-configuration-file"></a>Yapılandırma dosyası oluşturma

Dağıtım yapılandırma dosyası oluşturun. Konteyner Örnekleri kapsayıcınız için gereken CPU ve gigabayt ram sayısını belirtin. Modelinize bağlı olmasına rağmen, bir çekirdek ve 1 gigabayt RAM varsayılan birçok model için yeterlidir. Daha sonra daha fazlasına ihtiyacınız varsa, görüntüyü yeniden oluşturmanız ve hizmeti yeniden dağıtmanız gerekir.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "MNIST",  
                                                     "method": "sklearn"},
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-container-instances"></a>Konteyner Örneklerinde Dağıtma
Dağıtımı tamamlamak için tahmini süre **yaklaşık yedi ila sekiz dakikadır.**

Görüntüyü yapılandırın ve dağıtın. Aşağıdaki kod şu adımlardan geçer:

1. Bu dosyaları kullanarak bir görüntü oluşturun:
   * Puanlama dosyası, `score.py`.
   * Çevre dosyası, `myenv.yml`.
   * Model dosyası.
1. Görüntüyü çalışma alanının altına kaydedin. 
1. Resmi Kapsayıcı Örnekleri kapsayıcısına gönderin.
1. Görüntüyü kullanarak Kapsayıcı Örnekleri'nde bir kapsayıcı başlatın.
1. Web hizmeti HTTP uç noktasını alın.

Kendi ortam dosyanızı tanımlıyorsanız, azureml varsayılanlarını >= 1,0,45 sürümüyle pip bağımlılığı olarak listelemeniz gerektiğini lütfen unutmayın. Bu paket, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerir.

```python
%%time
from azureml.core.webservice import Webservice
from azureml.core.model import InferenceConfig
from azureml.core.environment import Environment

myenv = Environment.from_conda_specification(name="myenv", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

service = Model.deploy(workspace=ws,
                       name='sklearn-mnist-svc',
                       models=[model], 
                       inference_config=inference_config,
                       deployment_config=aciconfig)

service.wait_for_deployment(show_output=True)
```

REST istemci çağrılarını kabul eden puanlama web hizmetinin HTTP uç noktasını alın. Bu bitiş noktasını web hizmetini sınamak veya bir uygulamaya entegre etmek isteyen herkesle paylaşabilirsiniz: 

```python
print(service.scoring_uri)
```

## <a name="test-the-deployed-service"></a>Dağıtılan hizmeti test edin

Daha önce, modelin yerel sürümü ile tüm test verilerini puan. Artık dağıtılan modeli test verilerinden rastgele 30 görüntü örneğiyle test edebilirsiniz.  

Aşağıdaki kod şu adımlardan geçer:
1. Verileri JSON dizisi olarak Kapsayıcı Örnekleri'nde barındırılan web hizmetine gönderin. 

1. Hizmeti çağırmak için SDK'nın `run` API’sini kullanma. Ayrıca **kıvırma**gibi herhangi bir HTTP aracı nı kullanarak ham aramalar yapabilirsiniz.

1. Döndürülen tahminleri yazdırma ve bunları giriş görüntüleriyle birlikte çizme. Yanlış sınıflandırılan örnekleri vurgulamak için siyah üzerine beyaz kırmızı yazı tipi ve ters görüntü kullanılır. 

Model doğruluğu yüksek olduğundan, yanlış sınıflandırılan bir örneği göremeden önce aşağıdaki kodu birkaç kez çalıştırmanız gerekebilir:

```python
import json

# find 30 random samples from test set
n = 30
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding='utf8')

# predict using the deployed model
result = service.run(input_data=test_samples)

# compare actual value vs. the predicted values:
i = 0
plt.figure(figsize=(20, 1))

for s in sample_indices:
    plt.subplot(1, n, i + 1)
    plt.axhline('')
    plt.axvline('')
    
    # use different color for misclassified sample
    font_color = 'red' if y_test[s] != result[i] else 'black'
    clr_map = plt.cm.gray if y_test[s] != result[i] else plt.cm.Greys
    
    plt.text(x=10, y=-10, s=result[i], fontsize=18, color=font_color)
    plt.imshow(X_test[s].reshape(28, 28), cmap=clr_map)
    
    i = i + 1
plt.show()
```

Bu sonuç, test görüntülerinin rastgele bir örneğinden elde edilir:

![Grafik gösterim sonuçları](./media/tutorial-deploy-models-with-aml/results.png)

Ayrıca web hizmetini test etmek için ham bir HTTP isteği gönderebilirsiniz:

```python
import requests

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

headers = {'Content-Type': 'application/json'}

# for AKS deployment you'd need to the service key in the header as well
# api_key = service.get_key()
# headers = {'Content-Type':'application/json',  'Authorization':('Bearer '+ api_key)} 

resp = requests.post(service.scoring_uri, input_data, headers=headers)

print("POST to url", service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve diğer öğreticiler ve keşifler için çalışma alanını korumak için, bu API çağrısını kullanarak yalnızca Kapsayıcı Örnekleri dağıtımını silebilirsiniz:

```python
service.delete()
```

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]


## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Machine Learning için](how-to-deploy-and-where.md)tüm dağıtım seçenekleri hakkında bilgi edinin.
+ [Web hizmeti için istemci oluşturmayı](how-to-consume-web-service.md)öğrenin.
+  Büyük miktarlarda veri üzerinde eşzamanlı olarak [öngörülerde bulunun.](how-to-use-parallel-run-step.md)
+ Azure Machine Learning modellerinizi [Uygulama Öngörüleri](how-to-enable-app-insights.md)ile izleyin.
+ [Otomatik algoritma seçim](tutorial-auto-train-models.md) öğretici deneyin. 
