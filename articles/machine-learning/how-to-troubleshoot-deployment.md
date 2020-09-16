---
title: Docker dağıtımı sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Azure Kubernetes hizmeti ile genel Docker dağıtım hatalarını çözmenin, çözme ve sorunlarını giderme hakkında bilgi edinin ve Azure Machine Learning kullanarak Azure Container Instances.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: clauren42
ms.author: clauren
ms.reviewer: jmartens
ms.date: 08/06/2020
ms.topic: conceptual
ms.custom: troubleshooting, contperfq4, devx-track-python
ms.openlocfilehash: 22f9c709ced1069caa39ba2145981efa353caadf
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90602642"
---
# <a name="troubleshoot-docker-deployment-of-models-with-azure-kubernetes-service-and-azure-container-instances"></a>Azure Kubernetes hizmeti ve Azure Container Instances modelinin Docker dağıtımı sorunlarını giderin 

Azure Machine Learning kullanarak Azure Container Instances (ACI) ve Azure Kubernetes Service (AKS) ile genel Docker dağıtım hatalarını nasıl giderebileceğinizi ve çözeceğinizi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure aboneliği**. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.
* [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true).
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md).
* Yerel olarak hata ayıklamak için yerel sisteminizde çalışan bir Docker yüklemeniz olmalıdır.

    Docker yüklemenizi doğrulamak için, `docker run hello-world` bir Terminal veya komut isteminden komutunu kullanın. Docker 'ı yükleme veya Docker hataları sorunlarını giderme hakkında bilgi için bkz. [Docker belgeleri](https://docs.docker.com/).

## <a name="steps-for-docker-deployment-of-machine-learning-models"></a>Makine öğrenimi modellerinin Docker dağıtımı için adımlar

Bir modeli Azure Machine Learning dağıtırken, [model. deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) API 'sini ve bir [ortam](how-to-use-environments.md) nesnesini kullanırsınız. Hizmet, dağıtım aşaması sırasında bir temel Docker görüntüsü oluşturur ve tüm gerekli modelleri tek bir çağrıda takar. Temel dağıtım görevleri şunlardır:

1. Modeli çalışma alanı modeli kayıt defterine kaydedin.

2. Çıkarım yapılandırmasını tanımla:
    1. [Ortam](how-to-use-environments.md) nesnesi oluşturun. Bu nesne, bir ortam YAML dosyasındaki bağımlılıkları kullanarak, seçkin ortamlarımızın biri olabilir.
    2. Ortama ve Puanlama betiğine göre bir çıkarım yapılandırması (ınenceconfig nesnesi) oluşturun.

3. Modeli Azure Container Instance (ACI) hizmetine veya Azure Kubernetes Service 'e (AKS) dağıtın.

[Model yönetimi](concept-model-management-and-deployment.md) giriş bölümünde bu işlem hakkında daha fazla bilgi edinin.

## <a name="before-you-begin"></a>Başlamadan önce

Herhangi bir sorunla karşılaşırsanız, ilk yapılacak şey, sorunu yalıtmak için dağıtım görevinin (önceki adı daha önce açıklanan) bireysel adımlara bölünmemaktır.

Bir [ortam](how-to-use-environments.md) nesnesi olan [model. deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) öğesini giriş parametresi olarak kullanırken, kodunuz üç ana adıma ayrılabilir:

1. Modeli kaydedin. Örnek kod aşağıda verilmiştir:

    ```python
    from azureml.core.model import Model


    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Dağıtım için çıkarım yapılandırmasını tanımla:

    ```python
    from azureml.core.model import InferenceConfig
    from azureml.core.environment import Environment


    # create inference configuration based on the requirements defined in the YAML
    myenv = Environment.from_conda_specification(name="myenv", file_path="myenv.yml")
    inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
    ```

3. Önceki adımda oluşturulan çıkarım yapılandırmasını kullanarak modeli dağıtın:

    ```python
    from azureml.core.webservice import AciWebservice


    # deploy the model
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)
    aci_service = Model.deploy(workspace=ws,
                           name='my-service',
                           models=[model],
                           inference_config=inference_config,
                           deployment_config=aci_config)
    aci_service.wait_for_deployment(show_output=True)
    ```

E-dağıtım işleminin tek tek görevlere bölünmesi, daha yaygın hataların bazılarını belirlemeyi kolaylaştırır.

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

Modeli, Conda bağımlılıklarını veya dağıtım yapılandırmasını değiştirmek için [Update ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#&preserve-view=trueupdate--args-)kullanın. Aşağıdaki örnek, hizmet tarafından kullanılan modeli güncelleştirir:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Hizmeti Sil

Hizmeti silmek için [Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#&preserve-view=truedelete--)kullanın.

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

Bir Azure Kubernetes hizmet işlem hedefine bir hizmet dağıttığınızda, Azure Machine Learning hizmeti istenen miktarda kaynakla zamanlamayı dener. Kümede 5 dakikadan sonra uygun miktarda kaynağa sahip bir düğüm yoksa dağıtım başarısız olur. Hata iletisi `Couldn't Schedule because the kubernetes cluster didn't have available resources after trying for 00:05:00` . Bu hatayı daha fazla düğüm ekleyerek, düğümlerinizin SKU 'sunu değiştirerek ya da hizmetinizin kaynak gereksinimlerini değiştirerek ele alabilirsiniz. 

Hata iletisi genellikle ne kaynak için daha fazla gereksinim duyacağını gösterir. Örneğin, `0/3 nodes are available: 3 Insufficient nvidia.com/gpu` hizmetin GPU gerektirdiğini ve kümede kullanılabilir GPU olmayan üç düğüm olduğunu belirten bir hata iletisi görürseniz. Bu, bir GPU SKU 'SU kullanıyorsanız daha fazla düğüm eklenerek, bir GPU etkin SKU 'ya geçiş yaparak, ortamınızda GPU gerektirmez.  

## <a name="service-launch-fails"></a>Hizmet başlatılamadı

Görüntü başarıyla derlendikten sonra, sistem dağıtım yapılandırmanızı kullanarak bir kapsayıcı başlatmaya çalışır. Kapsayıcı başlatma işleminin bir parçası olarak, `init()` Puanlama betiğinizdeki işlev sistem tarafından çağırılır. İşlevde yakalanamayan özel durumlar varsa `init()` , hata Iletisinde **Crashloopgeri** alma hatası ' nı görebilirsiniz.

Günlükleri denetlemek için [Docker günlüğünü İnceleme](#dockerlog) bölümündeki bilgileri kullanın.

## <a name="function-fails-get_model_path"></a>İşlev başarısız oldu: get_model_path ()

Genellikle, `init()` Puanlama betiğindeki işlevinde model [. get_model_path ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#&preserve-view=trueget-model-path-model-name--version-none---workspace-none-) işlevi, bir model dosyasını veya kapsayıcıdaki model dosyalarının bir klasörünü bulmak için çağırılır. Model dosyası veya klasörü bulunamazsa, işlev başarısız olur. Bu hatada hata ayıklamanın en kolay yolu, kapsayıcı kabuğu 'nda aşağıdaki python kodunu çalıştırmalıdır:

```python
from azureml.core.model import Model
import logging
logging.basicConfig(level=logging.DEBUG)
print(Model.get_model_path(model_name='my-best-model'))
```

Bu örnek, `/var/azureml-app` Puanlama betiğinizin model dosyasını veya klasörünü bulmasını beklediği kapsayıcıda yerel yolu (göreli olarak) yazdırır. Dosya veya klasörün gerçekten beklenen yerde olduğunu doğrulayabilirsiniz.

Günlüğe kaydetme düzeyinin hata ayıklama olarak ayarlanması ek bilgilerin günlüğe kaydedilmesine neden olabilir ve bu da hatayı belirlemek için yararlı olabilir.

## <a name="function-fails-runinput_data"></a>İşlev başarısız: çalıştırma (input_data)

Hizmet başarıyla dağıtılırsa ancak Puanlama uç noktasına veri gönderdiğinizde çöktüğünde, `run(input_data)` bunun yerine ayrıntılı hata mesajı döndürmesi için işlevinize hata yakalama ifadesini ekleyebilirsiniz. Örnek:

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

**Note**: çağrıdan hata iletilerinin döndürülmesi `run(input_data)` yalnızca hata ayıklama amacıyla yapılmalıdır. Güvenlik nedenleriyle, bir üretim ortamında bu şekilde hata iletileri döndürmemelisiniz.

## <a name="http-status-code-502"></a>HTTP durum kodu 502

502 durum kodu hizmetin bir özel durum yaptığını veya `run()` Score.py dosyasının yönteminde kilitlendiğini gösterir. Dosyada hata ayıklamak için bu makaledeki bilgileri kullanın.

## <a name="http-status-code-503"></a>HTTP durum kodu 503

Azure Kubernetes hizmet dağıtımları otomatik ölçeklendirmeyi destekler, bu da ek yükü desteklemek için çoğaltmaların eklenmesine izin verir. Otomatik Scaler, yükteki **aşamalı** değişiklikleri işlemek için tasarlanmıştır. Saniye başına isteklerde büyük ani artışlar alıyorsanız, istemciler bir HTTP durum kodu 503 alabilir. Otomatik olarak yeniden hareket etmekle birlikte, ek kapsayıcılar oluşturmak için önemli miktarda zaman alır.

Ölçeği artırma/azaltma kararları, geçerli kapsayıcı çoğaltmalarının kullanımına dayanır. Meşgul olan çoğaltma sayısı (bir isteği işleme) geçerli çoğaltmanın toplam sayısına bölünmüş geçerli kullanımdır. Bu sayı aşarsa `autoscale_target_utilization` , daha fazla çoğaltma oluşturulur. Daha düşükse çoğaltmalar azalır. Çoğaltmaları ekleme kararları, ekip ve hızlı (1 saniye içinde). Çoğaltmaları kaldırma kararları (yaklaşık 1 dakika). Varsayılan olarak, otomatik ölçeklendirme hedef kullanımı **%70**olarak ayarlanır, bu da hizmetin saniyede %30 ' a **varan**(RPS) istek sayısını işleyebileceği anlamına gelir.

503 durum kodlarının önlenmesine yardımcı olabilecek iki şey vardır:

> [!TIP]
> Bu iki yaklaşım tek tek veya birlikte kullanılabilir.

* Otomatik ölçeklendirmenin yeni çoğaltmalar oluşturduğu kullanım düzeyini değiştirin. `autoscale_target_utilization`Daha düşük bir değere ayarlayarak kullanım hedefini ayarlayabilirsiniz.

    > [!IMPORTANT]
    > Bu değişiklik çoğaltmaların *daha hızlı*oluşturulmasına neden olmaz. Bunun yerine, daha düşük bir kullanım eşiğine göre oluşturulur. Hizmetin %70 olması beklenene kadar beklemek yerine %30 kullanım gerçekleştiğinde, çoğaltmanın oluşturulmasına neden olur.
    
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

, Ve için ayarları hakkında daha fazla bilgi için, `autoscale_target_utilization` `autoscale_max_replicas` `autoscale_min_replicas` bkz. [akswebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py&preserve-view=true) modül başvurusu.

## <a name="http-status-code-504"></a>HTTP durum kodu 504

504 durum kodu, isteğin zaman aşımına uğradığını gösterir. Varsayılan zaman aşımı 1 dakikadır.

Gereksiz çağrıları kaldırmak için score.py değiştirerek, zaman aşımını artırabilir veya hizmeti hızlandırmayı deneyebilirsiniz. Bu eylemler sorunu düzeltmez, score.py dosyasında hata ayıklamak için bu makaledeki bilgileri kullanın. Kod, yanıt vermeyen bir durumda veya sonsuz bir döngüde olabilir.

## <a name="advanced-debugging"></a>Gelişmiş hata ayıklama

Model dağıtımınızda bulunan Python kodunda etkileşimli olarak hata ayıklaması yapmanız gerekebilir. Örneğin, giriş betiği başarısız olursa ve neden ek günlüğe kaydetme ile saptanamaz. Visual Studio Code ve hata ayıklama GPY kullanarak, Docker kapsayıcısının içinde çalışan koda iliştirebilirsiniz.

Daha fazla bilgi için [vs Code kılavuzunda etkileşimli hata ayıklamayı](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-deployments)ziyaret edin.

## <a name="model-deployment-user-forum"></a>[Model dağıtımı kullanıcı forumu](https://docs.microsoft.com/answers/topics/azure-machine-learning-inference.html)

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım hakkında daha fazla bilgi edinin:

* [Dağıtım ve nerede](how-to-deploy-and-where.md)
* [Öğretici: eğitim & dağıtım modelleri](tutorial-train-models-with-aml.md)
