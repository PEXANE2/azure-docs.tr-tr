---
title: Web hizmeti dağıtımı sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Azure Kubernetes hizmeti ve Azure Container Instances ile ilgili genel Docker dağıtım hatalarını çözmenin, çözme ve sorunlarını giderme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: gvashishtha
ms.author: gopalv
ms.reviewer: jmartens
ms.date: 11/02/2020
ms.topic: troubleshooting
ms.custom: contperfq4, devx-track-python, deploy
ms.openlocfilehash: dfbfea22738e6aeb0df31ad941b2ff10e53795a4
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93311300"
---
# <a name="troubleshoot-model-deployment"></a>Model dağıtımı sorunlarını giderme

Azure Machine Learning kullanarak Azure Container Instances (ACI) ve Azure Kubernetes Service (AKS) ile genel Docker dağıtım hatalarını nasıl giderebileceğinizi ve çözeceğinizi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure aboneliği**. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.
* [Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
* [Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md).
* Yerel olarak hata ayıklamak için yerel sisteminizde çalışan bir Docker yüklemeniz olmalıdır.

    Docker yüklemenizi doğrulamak için, `docker run hello-world` bir Terminal veya komut isteminden komutunu kullanın. Docker 'ı yükleme veya Docker hataları sorunlarını giderme hakkında bilgi için bkz. [Docker belgeleri](https://docs.docker.com/).

## <a name="steps-for-docker-deployment-of-machine-learning-models"></a>Makine öğrenimi modellerinin Docker dağıtımı için adımlar

Azure Machine Learning, yerel olmayan bir işlem için bir modeli dağıtırken, aşağıdakiler gerçekleşir:

1. Inısenceconfig 'teki ortamlar nesneniz içinde belirttiğiniz Dockerfile, kaynak dizininizin içeriğiyle birlikte buluta gönderilir
1. Daha önce oluşturulmuş bir görüntü, kapsayıcı kayıt defterinizde yoksa, bulutta yerleşik olarak yeni bir Docker görüntüsü oluşturulur ve çalışma alanınızın varsayılan kapsayıcı kayıt defterinde saklanır.
1. Kapsayıcı kayıt defterinizin Docker görüntüsü işlem hedefinizden indirilir.
1. Çalışma alanınızın varsayılan Blob deposu, işlem hedeflerinize bağlanır, böylece kayıtlı modellere erişebilirsiniz
1. Web sunucunuz, giriş betiğinizin işlevi çalıştırılarak başlatılır `init()`
1. Dağıtılan modeliniz bir istek aldığında, `run()` işleviniz bu isteği işler

Yerel bir dağıtım kullanmanın asıl farkı, kapsayıcı görüntüsünün yerel makinenizde yerleşik olarak oluşturulması ve bu nedenle yerel bir dağıtım için Docker 'ın yüklü olması gerekir.

Bu üst düzey adımların anlaşılması hataların nerede olduğunu anlamanıza yardımcı olmalıdır.

## <a name="get-deployment-logs"></a>Dağıtım günlüklerini al

Hata ayıklama hataındaki ilk adım, dağıtım günlüklerinizi almak için kullanılır. İlk olarak, çalışma alanınıza bağlanmak için [buradaki yönergeleri](how-to-deploy-and-where.md#connect-to-your-workspace) izleyin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

Dağıtılan bir Web hizmetinden günlükleri almak için şunları yapın:

```bash
az ml service get-logs --verbose --workspace-name <my workspace name> --name <service name>
```

# <a name="python"></a>[Python](#tab/python)


Çağrılan türünde bir nesneniz olduğu varsayılarak `azureml.core.Workspace` `ws` , şunları yapabilirsiniz:

```python
print(ws.webservices)

# Choose the webservice you are interested in

from azureml.core import Webservice

service = Webservice(ws, '<insert name of webservice>')
print(service.get_logs())
```

---

## <a name="debug-locally"></a>Yerel olarak hata ayıkla

Bir modeli ACI veya AKS 'e dağıtırken sorun yaşıyorsanız, yerel bir Web hizmeti olarak dağıtın. Yerel web hizmetinin kullanılması sorunları gidermeyi kolaylaştırır.

Bir çalıştırılabilir örnek bulmak için [Machinelearningnotebook](https://github.com/Azure/MachineLearningNotebooks) deposunda örnek bir [Yerel dağıtım Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-local/register-model-deploy-local.ipynb) bulabilirsiniz.

> [!WARNING]
> Yerel Web hizmeti dağıtımları, üretim senaryolarında desteklenmez.

Yerel olarak dağıtmak için kodunuzu `LocalWebservice.deploy_configuration()` bir dağıtım yapılandırması oluşturmak üzere değiştirin. Ardından `Model.deploy()` hizmeti dağıtmak için kullanın. Aşağıdaki örnek, bir modeli (model değişkeninde yer alan) yerel bir Web hizmeti olarak dağıtır:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import LocalWebservice


# Create inference configuration based on the environment definition and the entry script
myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(
    ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Kendi Conda Specification YAML 'nizi tanımlıyorsanız, 1.0.45 = bir PIP bağımlılığı olarak >=. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereklidir.

Bu noktada, hizmetle normal şekilde çalışabilirsiniz. Aşağıdaki kod, hizmete veri gönderilmesini göstermektedir:

```python
import json

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

test_sample = bytes(test_sample, encoding='utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

Python ortamınızı özelleştirme hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md). 

### <a name="update-the-service"></a>Hizmeti güncelleştirme

Yerel test sırasında, `score.py` kayıt eklemek veya bulduğunuz herhangi bir sorunu çözmeye çalışmak için dosyayı güncelleştirmeniz gerekebilir. Değişiklikleri dosyaya yeniden yüklemek için `score.py` kullanın `reload()` . Örneğin, aşağıdaki kod, hizmeti için betiği yeniden yükler ve ardından verileri bu sunucuya gönderir. Veriler, güncelleştirilmiş dosya kullanılarak puanlanır `score.py` :

> [!IMPORTANT]
> `reload`Yöntemi yalnızca yerel dağıtımlar için kullanılabilir. Bir dağıtımı başka bir işlem hedefine güncelleştirme hakkında daha fazla bilgi için bkz. [Web weblerinizi güncelleştirme](how-to-deploy-update-web-service.md).

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Betik, `InferenceConfig` hizmet tarafından kullanılan nesne tarafından belirtilen konumdan yeniden yüklenir.

Modeli, Conda bağımlılıklarını veya dağıtım yapılandırmasını değiştirmek için [Update ()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueupdate--args-)kullanın. Aşağıdaki örnek, hizmet tarafından kullanılan modeli güncelleştirir:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Hizmeti Sil

Hizmeti silmek için [Delete ()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truedelete--)kullanın.

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a> Docker günlüğünü İnceleme

Service nesnesinden ayrıntılı Docker motoru günlük iletilerini yazdırabilirsiniz. ACI, AKS ve yerel dağıtımlar için günlüğü görüntüleyebilirsiniz. Aşağıdaki örnek günlükleri nasıl yazdırabileceğinizi gösterir.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```
Bir satırı `Booting worker with pid: <pid>` günlüklerde birden çok kez görürseniz, çalışanı başlatmak için yeterli bellek yok demektir.
İçindeki değerini artırarak hatayı ele alabilirsiniz. `memory_gb``deployment_config`
 
## <a name="container-cannot-be-scheduled"></a>Kapsayıcı zamanlanamaz

Azure Kubernetes Service işlem hedefine hizmet dağıtırken, Azure Machine Learning hizmeti istenen miktarda kaynakla zamanlamayı dener. Kümede 5 dakikadan sonra uygun miktarda kaynağa sahip bir düğüm yoksa dağıtım başarısız olur. Hata iletisi `Couldn't Schedule because the kubernetes cluster didn't have available resources after trying for 00:05:00` . Bu hatayı daha fazla düğüm ekleyerek, düğümlerinizin SKU 'sunu değiştirerek ya da hizmetinizin kaynak gereksinimlerini değiştirerek ele alabilirsiniz. 

Hata iletisi genellikle ne kaynak için daha fazla gereksinim duyacağını gösterir. Örneğin, `0/3 nodes are available: 3 Insufficient nvidia.com/gpu` hizmetin GPU gerektirdiğini ve kümede kullanılabilir GPU olmayan üç düğüm olduğunu belirten bir hata iletisi görürseniz. Bu, bir GPU SKU 'SU kullanıyorsanız daha fazla düğüm eklenerek, bir GPU etkin SKU 'ya geçiş yaparak, ortamınızda GPU gerektirmez.  

## <a name="service-launch-fails"></a>Hizmet başlatılamadı

Görüntü başarıyla derlendikten sonra, sistem dağıtım yapılandırmanızı kullanarak bir kapsayıcı başlatmaya çalışır. Kapsayıcı başlatma işleminin bir parçası olarak, `init()` Puanlama betiğinizdeki işlev sistem tarafından çağırılır. İşlevde yakalanamayan özel durumlar varsa `init()` , hata Iletisinde **Crashloopgeri** alma hatası ' nı görebilirsiniz.

Günlükleri denetlemek için [Docker günlüğünü İnceleme](#dockerlog) bölümündeki bilgileri kullanın.

## <a name="function-fails-get_model_path"></a>İşlev başarısız oldu: get_model_path ()

Genellikle, `init()` Puanlama betiğinin işlevindeki [model.get_model_path ()](/python/api/azureml-core/azureml.core.model.model?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-model-path-model-name--version-none---workspace-none-) işlevi, bir model dosyasını veya kapsayıcıdaki model dosyalarının bir klasörünü bulmak için çağırılır. Model dosyası veya klasörü bulunamazsa işlev başarısız olur. Bu hatada hata ayıklamanın en kolay yolu, kapsayıcı kabuğu 'nda aşağıdaki python kodunu çalıştırmalıdır:

```python
from azureml.core.model import Model
import logging
logging.basicConfig(level=logging.DEBUG)
print(Model.get_model_path(model_name='my-best-model'))
```

Bu örnek, `/var/azureml-app` Puanlama betiğinizin model dosyasını veya klasörünü bulmasını beklediği kapsayıcıda yerel yolu (göreli olarak) yazdırır. Dosya veya klasörün gerçekten beklenen yerde olduğunu doğrulayabilirsiniz.

Günlüğe kaydetme düzeyinin hata ayıklama olarak ayarlanması ek bilgilerin günlüğe kaydedilmesine neden olabilir ve bu da hatayı belirlemek için yararlı olabilir.

## <a name="function-fails-runinput_data"></a>İşlev başarısız: çalıştırma (input_data)

Hizmet başarıyla dağıtılırsa ancak Puanlama uç noktasına veri gönderdiğinizde çöktüğünde, `run(input_data)` bunun yerine ayrıntılı hata mesajı döndürmesi için işlevinize hata yakalama ifadesini ekleyebilirsiniz. Örneğin:

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```

**Note** : çağrıdan hata iletilerinin döndürülmesi `run(input_data)` yalnızca hata ayıklama amacıyla yapılmalıdır. Güvenlik nedenleriyle, bir üretim ortamında bu şekilde hata iletileri döndürmemelisiniz.

## <a name="http-status-code-502"></a>HTTP durum kodu 502

502 durum kodu hizmetin bir özel durum yaptığını veya `run()` Score.py dosyasının yönteminde kilitlendiğini gösterir. Dosyada hata ayıklamak için bu makaledeki bilgileri kullanın.

## <a name="http-status-code-503"></a>HTTP durum kodu 503

Azure Kubernetes hizmet dağıtımları otomatik ölçeklendirmeyi destekler, bu da ek yükü desteklemek için çoğaltmaların eklenmesine izin verir. Otomatik Scaler, yükteki **aşamalı** değişiklikleri işlemek için tasarlanmıştır. Saniye başına isteklerde büyük ani artışlar alıyorsanız, istemciler bir HTTP durum kodu 503 alabilir. Otomatik olarak yeniden hareket etmekle birlikte, ek kapsayıcılar oluşturmak için önemli miktarda zaman alır.

Ölçeği artırma/azaltma kararları, geçerli kapsayıcı çoğaltmalarının kullanımına dayanır. Meşgul olan çoğaltma sayısı (bir isteği işleme) geçerli çoğaltmanın toplam sayısına bölünmüş geçerli kullanımdır. Bu sayı aşarsa `autoscale_target_utilization` , daha fazla çoğaltma oluşturulur. Daha düşükse çoğaltmalar azalır. Çoğaltmaları ekleme kararları, ekip ve hızlı (1 saniye içinde). Çoğaltmaları kaldırma kararları (yaklaşık 1 dakika). Varsayılan olarak, otomatik ölçeklendirme hedef kullanımı **%70** olarak ayarlanır, bu da hizmetin saniyede %30 ' a **varan** (RPS) istek sayısını işleyebileceği anlamına gelir.

503 durum kodlarının önlenmesine yardımcı olabilecek iki şey vardır:

> [!TIP]
> Bu iki yaklaşım tek tek veya birlikte kullanılabilir.

* Otomatik ölçeklendirmenin yeni çoğaltmalar oluşturduğu kullanım düzeyini değiştirin. `autoscale_target_utilization`Daha düşük bir değere ayarlayarak kullanım hedefini ayarlayabilirsiniz.

    > [!IMPORTANT]
    > Bu değişiklik çoğaltmaların *daha hızlı* oluşturulmasına neden olmaz. Bunun yerine, daha düşük bir kullanım eşiğine göre oluşturulur. Hizmetin %70 olması beklenene kadar beklemek yerine %30 kullanım gerçekleştiğinde, çoğaltmanın oluşturulmasına neden olur.
    
    Web hizmeti zaten geçerli en fazla çoğaltmaları kullanıyorsa ve 503 durum kodu görmeye devam ediyorsanız, `autoscale_max_replicas` en fazla çoğaltma sayısını artırmak için değeri arttırın.

* En az çoğaltma sayısını değiştirin. En düşük çoğaltmaları artırmak, gelen ani artışları işlemek için daha büyük bir havuz sağlar.

    En az çoğaltma sayısını artırmak için, `autoscale_min_replicas` daha yüksek bir değere ayarlayın. Aşağıdaki kodu kullanarak gerekli çoğaltmaları hesaplayabilirsiniz ve değerleri projenize özgü değerlerle değiştirin:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > Yeni en düşük çoğaltmalardan daha büyük istek ani artışları alırsanız, yeniden 503s alabilirsiniz. Örneğin, hizmetinizin trafiği arttıkça, en düşük çoğaltmaları artırmanız gerekebilir.

, Ve için ayarları hakkında daha fazla bilgi için, `autoscale_target_utilization` `autoscale_max_replicas` `autoscale_min_replicas` bkz. [akswebservice](/python/api/azureml-core/azureml.core.webservice.akswebservice?preserve-view=true&view=azure-ml-py) modül başvurusu.

## <a name="http-status-code-504"></a>HTTP durum kodu 504

504 durum kodu, isteğin zaman aşımına uğradığını gösterir. Varsayılan zaman aşımı 1 dakikadır.

Gereksiz çağrıları kaldırmak için score.py değiştirerek, zaman aşımını artırabilir veya hizmeti hızlandırmayı deneyebilirsiniz. Bu eylemler sorunu düzeltmez, score.py dosyasında hata ayıklamak için bu makaledeki bilgileri kullanın. Kod, yanıt vermeyen bir durumda veya sonsuz bir döngüde olabilir.

## <a name="advanced-debugging"></a>Gelişmiş hata ayıklama

Model dağıtımınızda yer alan Python kodunda etkileşimli hata ayıklaması yapmanız gerekebilir. Örneğin, giriş betiği başarısız olursa ve neden ek günlüğe kaydetme ile saptanamaz. Visual Studio Code ve hata ayıklama GPY kullanarak, Docker kapsayıcısının içinde çalışan koda iliştirebilirsiniz.

Daha fazla bilgi için [vs Code kılavuzunda etkileşimli hata ayıklamayı](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-deployments)ziyaret edin.

## <a name="model-deployment-user-forum"></a>[Model dağıtımı kullanıcı forumu](/answers/topics/azure-machine-learning-inference.html)

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım hakkında daha fazla bilgi edinin:

* [Dağıtım ve nerede](how-to-deploy-and-where.md)
* [Öğretici: eğitim & dağıtım modelleri](tutorial-train-models-with-aml.md)