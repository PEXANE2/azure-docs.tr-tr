---
title: Varolan modelleri kullanma ve dağıtma
titleSuffix: Azure Machine Learning
description: Hizmet dışında eğitilmiş modellerle Azure Machine Learning'i nasıl kullanabileceğinizi öğrenin. Oluşturulan modelleri Azure Machine Learning dışında kaydedebilir ve bunları bir web hizmeti veya Azure IoT Edge modülü olarak dağıtabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 03/17/2020
ms.openlocfilehash: 924bd2fdba2359e6f1108c39802ad3ce95ebdf07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472384"
---
# <a name="use-an-existing-model-with-azure-machine-learning"></a>Azure Machine Learning ile varolan bir modeli kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ile varolan bir makine öğrenimi modelini nasıl kullanacağınızı öğrenin.

Azure Machine Learning dışında eğitilmiş bir makine öğrenme modeliniz varsa, modeli web hizmeti olarak dağıtmak veya bir IoT Edge aygıtına dağıtmak için hizmeti kullanmaya devam edebilirsiniz. 

> [!TIP]
> Bu makalede, varolan bir modeli kaydetme ve dağıtma hakkında temel bilgiler sağlanmış olur. Azure Machine Learning dağıtıldıktan sonra modeliniz için izleme sağlar. Ayrıca, veri kayması çözümlemesi veya modelin yeni sürümlerini eğitmek için kullanılabilen dağıtıma gönderilen giriş verilerini depolamanızı sağlar.
>
> Burada kullanılan kavramlar ve terimler hakkında daha fazla bilgi için [bkz.](concept-model-management-and-deployment.md)
>
> Dağıtım işlemi hakkında genel bilgi için Azure [Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için [bkz.](how-to-manage-workspace.md)

    > [!TIP]
    > Bu makaledeki Python örnekleri, `ws` değişkenin Azure Machine Learning çalışma alanınız olarak ayarladığını varsayar.
    >
    > CLI örnekleri nde yer `myworkspace` tutucu `myresourcegroup`ve . Bunları çalışma alanınızın adı ve onu içeren kaynak grubuyla değiştirin.

* [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).  

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve [Machine Learning CLI uzantısı.](reference-azure-machine-learning-cli.md)

* Eğitimli bir model. Model, geliştirme ortamınızla ilgili bir veya daha fazla dosyaya kalıcı olmalıdır.

    > [!NOTE]
    > Azure Machine Learning dışında eğitilmiş bir modeli kaydetmek için, bu makaledeki örnek kod parçacıkları Paolo Ripamonti'nin [https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis](https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis)Twitter duyarlılık analizi projesi tarafından oluşturulan modelleri kullanır: .

## <a name="register-the-models"></a>Model(ler) kaydedin

Bir modeli kaydetmek, çalışma alanınızda modeller hakkındaki meta verileri depolamanızı, sürümünüzü ve izlemenizi sağlar. Aşağıdaki Python ve CLI örneklerinde, `models` dizin `model.w2v` `encoder.pkl` `model.h5`, `tokenizer.pkl` , ve dosyaları içerir. Bu örnek, `models` dizinde yer alan dosyaları yeni bir `sentiment`model kaydı olarak yükler:

```python
from azureml.core.model import Model
# Tip: When model_path is set to a directory, you can use the child_paths parameter to include
#      only some of the files from the directory
model = Model.register(model_path = "./models",
                       model_name = "sentiment",
                       description = "Sentiment analysis model trained outside Azure Machine Learning",
                       workspace = ws)
```

Daha fazla bilgi için [Model.register()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model(class)?view=azure-ml-py#register-workspace--model-path--model-name--tags-none--properties-none--description-none--datasets-none--model-framework-none--model-framework-version-none--child-paths-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none-) referansına bakın.

```azurecli
az ml model register -p ./models -n sentiment -w myworkspace -g myresourcegroup
```

> [!TIP]
> Ayrıca, kayıtlı `tags` modele ekleme ve `properties` sözlük nesneleri ayarlayabilirsiniz. Bu değerler daha sonra belirli bir modeli belirlemeye yardımcı olmak için kullanılabilir. Örneğin, kullanılan çerçeve, eğitim parametreleri, vb.

Daha fazla bilgi için [az ml model kayıt](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register) başvurusuna bakın.


Genel olarak model kaydı hakkında daha fazla bilgi için makine [öğrenim modellerini yönet, dağıt ve izleyin.](concept-model-management-and-deployment.md)

## <a name="define-inference-configuration"></a>Çıkarım yapılandırması tanımlama

Çıkarım yapılandırması, dağıtılan modeli çalıştırmak için kullanılan ortamı tanımlar. Çıkarım yapılandırması, dağıtıldığında modeli çalıştırmak için kullanılan aşağıdaki varlıklara başvurur:

* Giriş betiği. Bu dosya `score.py`(adlandırılmış) dağıtılan hizmet başladığında modeli yükler. Ayrıca, verileri almaktan, modele aktarmakndan ve ardından bir yanıtı döndürmekten de sorumludur.
* Azure Machine Learning [ortamı.](how-to-use-environments.md) Ortam, model ve giriş komut dosyasını çalıştırmak için gereken yazılım bağımlılıklarını tanımlar.

Aşağıdaki örnek, bir ortam oluşturmak için SDK'nın nasıl kullanılacağını ve sonra çıkarım yapılandırmasıyla nasıl kullanılacağını gösterir:

```python
from azureml.core.model import InferenceConfig
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

# Create the environment
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Define the packages needed by the model and scripts
conda_dep.add_conda_package("tensorflow")
conda_dep.add_conda_package("numpy")
conda_dep.add_conda_package("scikit-learn")
# You must list azureml-defaults as a pip dependency
conda_dep.add_pip_package("azureml-defaults")
conda_dep.add_pip_package("keras")
conda_dep.add_pip_package("gensim")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep

inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
```

Daha fazla bilgi için aşağıdaki makalelere bakın:

+ [Ortamlar nasıl kullanılır.](how-to-use-environments.md)
+ [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) referans.


CLI çıkarım yapılandırmasını bir YAML dosyasından yükler:

```yaml
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "myenv.yml"
}
```

CLI ile conda ortamı çıkarım `myenv.yml` yapılandırması tarafından başvurulan dosyada tanımlanır. Aşağıdaki YAML bu dosyanın içeriğidir:

```yaml
name: inference_environment
dependencies:
- python=3.6.2
- tensorflow
- numpy
- scikit-learn
- pip:
    - azureml-defaults
    - keras
    - gensim
```

Çıkarım yapılandırması hakkında daha fazla bilgi için [Azure Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

### <a name="entry-script"></a>Giriş komut dosyası

Giriş komut dosyasında yalnızca `init()` iki `run(data)`gerekli işlev vardır ve . Bu işlevler, hizmeti başlangıçta başlatmave istemci tarafından geçirilen istek verilerini kullanarak modeli çalıştırmak için kullanılır. Komut dosyasının geri kalanı yükleme ve model (ler) çalıştırMa işler.

> [!IMPORTANT]
> Tüm modeller için çalışan genel bir giriş komut dosyası yoktur. Her zaman kullanılan modele özgüdür. Modelin nasıl yüklenir, modelin beklediği veri biçimini ve modeli kullanarak veri nasıl puanalsüreceğini anlamalıdır.

Aşağıdaki Python kodu örnek bir`score.py`giriş komut dosyasıdır ( ):

```python
import os
import pickle
import json
import time
from keras.models import load_model
from keras.preprocessing.sequence import pad_sequences
from gensim.models.word2vec import Word2Vec

# SENTIMENT
POSITIVE = "POSITIVE"
NEGATIVE = "NEGATIVE"
NEUTRAL = "NEUTRAL"
SENTIMENT_THRESHOLDS = (0.4, 0.7)
SEQUENCE_LENGTH = 300

# Called when the deployed service starts
def init():
    global model
    global tokenizer
    global encoder
    global w2v_model

    # Get the path where the deployed model can be found.
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), './models')
    # load models
    model = load_model(model_path + '/model.h5')
    w2v_model = Word2Vec.load(model_path + '/model.w2v')

    with open(model_path + '/tokenizer.pkl','rb') as handle:
        tokenizer = pickle.load(handle)

    with open(model_path + '/encoder.pkl','rb') as handle:
        encoder = pickle.load(handle)

# Handle requests to the service
def run(data):
    try:
        # Pick out the text property of the JSON request.
        # This expects a request in the form of {"text": "some text to score for sentiment"}
        data = json.loads(data)
        prediction = predict(data['text'])
        #Return prediction
        return prediction
    except Exception as e:
        error = str(e)
        return error

# Determine sentiment from score
def decode_sentiment(score, include_neutral=True):
    if include_neutral:
        label = NEUTRAL
        if score <= SENTIMENT_THRESHOLDS[0]:
            label = NEGATIVE
        elif score >= SENTIMENT_THRESHOLDS[1]:
            label = POSITIVE
        return label
    else:
        return NEGATIVE if score < 0.5 else POSITIVE

# Predict sentiment using the model
def predict(text, include_neutral=True):
    start_at = time.time()
    # Tokenize text
    x_test = pad_sequences(tokenizer.texts_to_sequences([text]), maxlen=SEQUENCE_LENGTH)
    # Predict
    score = model.predict([x_test])[0]
    # Decode sentiment
    label = decode_sentiment(score, include_neutral=include_neutral)

    return {"label": label, "score": float(score),
       "elapsed_time": time.time()-start_at}  
```

Giriş komut dosyaları hakkında daha fazla bilgi için Azure [Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md)bakın.

## <a name="define-deployment"></a>Dağıtımı tanımlama

[Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice?view=azure-ml-py) paketi dağıtım için kullanılan sınıfları içerir. Kullandığınız sınıf modelin nerede dağıtılanını belirler. Örneğin, Azure Kubernetes Hizmeti'nde web hizmeti olarak dağıtmak için, dağıtım yapılandırmasını oluşturmak için [AksWebService.deploy_configuration()](/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none--gpu-cores-none--period-seconds-none--initial-delay-seconds-none--timeout-seconds-none--success-threshold-none--failure-threshold-none--namespace-none--token-auth-enabled-none--compute-target-name-none-) kullanın.

Aşağıdaki Python kodu, yerel dağıtım için dağıtım yapılandırmasını tanımlar. Bu yapılandırma, modeli web hizmeti olarak yerel bilgisayarınıza dağır.

> [!IMPORTANT]
> Yerel dağıtım, [docker'ın](https://www.docker.com/) yerel bilgisayarınıza çalışmasını gerektirir:

```python
from azureml.core.webservice import LocalWebservice

deployment_config = LocalWebservice.deploy_configuration()
```

Daha fazla bilgi için [LocalWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.localwebservice?view=azure-ml-py#deploy-configuration-port-none-) referansına bakın.

CLI dağıtım yapılandırmasını bir YAML dosyasından yükler:

```YAML
{
    "computeType": "LOCAL"
}
```

Azure bulutundaki Azure Kubernetes Hizmeti gibi farklı bir işlem hedefine dağıtmak, dağıtım yapılandırmasını değiştirmek kadar kolaydır. Daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağıkonusunda](how-to-deploy-and-where.md)bakın.

## <a name="deploy-the-model"></a>Modeli dağıtma

Aşağıdaki örnek, kayıtlı modeldeki `sentiment`bilgileri yükler ve sonra onu `sentiment`bir hizmet olarak dağıtır. Dağıtım sırasında çıkarım yapılandırması ve dağıtım yapılandırması, hizmet ortamını oluşturmak ve yapılandırmak için kullanılır:

```python
from azureml.core.model import Model

model = Model(ws, name='sentiment')
service = Model.deploy(ws, 'myservice', [model], inference_config, deployment_config)

service.wait_for_deployment(True)
print(service.state)
print("scoring URI: " + service.scoring_uri)
```

Daha fazla bilgi için [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) başvurusuna bakın.

Modeli CLI'den dağıtmak için aşağıdaki komutu kullanın. Bu komut, kayıtlı modelin sürüm`sentiment:1`1'ini ( ) `inferenceConfig.json` `deploymentConfig.json` ve dosyalarda depolanan çıkarım ve dağıtım yapılandırmasını kullanarak dağıtır:

```azurecli
az ml model deploy -n myservice -m sentiment:1 --ic inferenceConfig.json --dc deploymentConfig.json
```

Daha fazla bilgi için [az ml modeli dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) referansına bakın.

Dağıtım hakkında daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağıhakkında](how-to-deploy-and-where.md)bakın.

## <a name="request-response-consumption"></a>İstek-yanıt tüketimi

Dağıtımdan sonra, puanlama URI görüntülenir. Bu URI, istemciler tarafından hizmete istek göndermek için kullanılabilir. Aşağıdaki örnek, hizmete veri gönderen ve yanıtı görüntüleyen temel bir Python istemcisidir:

```python
import requests
import json

scoring_uri = 'scoring uri for your service'
headers = {'Content-Type':'application/json'}

test_data = json.dumps({'text': 'Today is a great day!'})

response = requests.post(scoring_uri, data=test_data, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Dağıtılan hizmetin nasıl tüketilen hakkında daha fazla bilgi için [bkz.](how-to-consume-web-service.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning modellerinizi Uygulama Öngörüleri ile izleyin](how-to-enable-app-insights.md)
* [Üretimdeki modeller için veri toplama](how-to-enable-data-collection.md)
* [Modelleri nasıl ve nerede dağıtılır](how-to-deploy-and-where.md)
* [Dağıtılan bir model için istemci oluşturma](how-to-consume-web-service.md)
