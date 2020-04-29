---
title: Dağıtım sorunlarını giderme kılavuzu
titleSuffix: Azure Machine Learning
description: Azure Kubernetes hizmeti ile genel Docker dağıtım hatalarını çözmenin, çözme ve sorunlarını giderme hakkında bilgi edinin ve Azure Machine Learning kullanarak Azure Container Instances.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: clauren42
ms.author: clauren
ms.reviewer: jmartens
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: fab46f7d7ae74ad643ce3f122b27b0dc767f5a78
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78399675"
---
# <a name="troubleshooting-azure-machine-learning-azure-kubernetes-service-and-azure-container-instances-deployment"></a>Azure Kubernetes hizmeti ve Azure Container Instances dağıtımı Azure Machine Learning sorunlarını giderme

Azure Machine Learning kullanarak Azure Container Instances (ACI) ve Azure Kubernetes hizmeti (AKS) ile genel Docker dağıtım hatalarını çözmenin veya çözme hakkında bilgi edinin.

Azure Machine Learning bir modeli dağıttığınızda, sistem bir dizi görevi gerçekleştirir.

Model dağıtımı için önerilen ve en güncel yaklaşım, bir [ortam](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments) nesnesini giriş parametresi olarak kullanan [model. deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) API 'sidir. Bu durumda hizmetimiz dağıtım aşaması sırasında sizin için bir temel Docker görüntüsü oluşturacak ve gerekli modelleri tek bir çağrıda bağlamamız gerekir. Temel dağıtım görevleri şunlardır:

1. Modeli çalışma alanı modeli kayıt defterine kaydedin.

2. Çıkarım yapılandırmasını tanımla:
    1. Ortam YAML dosyasında belirttiğiniz bağımlılıklara göre bir [ortam](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments) nesnesi oluşturun veya temin ortamlarımızın birini kullanın.
    2. Ortama ve Puanlama betiğine göre bir çıkarım yapılandırması (ınenceconfig nesnesi) oluşturun.

3. Modeli Azure Container Instance (ACI) hizmetine veya Azure Kubernetes Service 'e (AKS) dağıtın.

[Model yönetimi](concept-model-management-and-deployment.md) giriş bölümünde bu işlem hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

* Bir **Azure aboneliği**. Bir tane yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.
* [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md).
* Yerel olarak hata ayıklamak için yerel sisteminizde çalışan bir Docker yüklemeniz olmalıdır.

    Docker yüklemenizi doğrulamak için, bir Terminal veya komut `docker run hello-world` isteminden komutunu kullanın. Docker 'ı yükleme veya Docker hataları sorunlarını giderme hakkında bilgi için bkz. [Docker belgeleri](https://docs.docker.com/).

## <a name="before-you-begin"></a>Başlamadan önce

Herhangi bir sorunla karşılaşırsanız, ilk yapılacak şey, sorunu yalıtmak için dağıtım görevinin (önceki adı daha önce açıklanan) bireysel adımlara bölünmemaktır.

[Model. deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) API 'si aracılığıyla bir [ortam](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments) nesnesi ile bir giriş parametresi olarak yeni/önerilen dağıtım yöntemi kullandığınızı varsayarak, kodunuz üç ana adıma ayrılabilir:

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

Dağıtım sürecini tek tek görevlere doldurduktan sonra, en yaygın hatalardan bazılarına bakabiliriz.

## <a name="debug-locally"></a>Yerel olarak hata ayıkla

Bir modeli ACG veya AKS 'e dağıtmaya yönelik sorunlarla karşılaşırsanız, yerel bir Web hizmeti olarak dağıtımı deneyin. Yerel bir Web hizmeti kullanmak, sorunları gidermenize daha kolay hale getirir. Modeli içeren Docker görüntüsü indirilip yerel sisteminizde başlatılır.

> [!WARNING]
> Yerel Web hizmeti dağıtımları, üretim senaryolarında desteklenmez.

Yerel olarak dağıtmak için kodunuzu bir dağıtım yapılandırması oluşturmak `LocalWebservice.deploy_configuration()` üzere değiştirin. Ardından hizmeti `Model.deploy()` dağıtmak için kullanın. Aşağıdaki örnek, bir modeli (model değişkeninde yer alan) yerel bir Web hizmeti olarak dağıtır:

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

Lütfen kendi Conda Specification YAML 'nizi tanımlıyorsanız, >= 1.0.45 sürümü ile azureml ön ayarlarını bir PIP bağımlılığı olarak listeleceğini unutmayın. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerir.

Bu noktada, hizmetle normal şekilde çalışabilirsiniz. Örneğin, aşağıdaki kod, hizmete veri gönderilmesini göstermektedir:

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

Yerel test sırasında, kayıt eklemek veya bulduğunuz herhangi bir `score.py` sorunu çözmeye çalışmak için dosyayı güncelleştirmeniz gerekebilir. Değişiklikleri `score.py` dosyaya yeniden yüklemek için kullanın `reload()`. Örneğin, aşağıdaki kod, hizmeti için betiği yeniden yükler ve ardından verileri bu sunucuya gönderir. Veriler, güncelleştirilmiş `score.py` dosya kullanılarak puanlanır:

> [!IMPORTANT]
> `reload` Yöntemi yalnızca yerel dağıtımlar için kullanılabilir. Bir dağıtımı başka bir işlem hedefine güncelleştirme hakkında daha fazla bilgi için bkz. dağıtım [modellerini](how-to-deploy-and-where.md#update)güncelleştirme bölümü.

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Betik, hizmet tarafından kullanılan `InferenceConfig` nesne tarafından belirtilen konumdan yeniden yüklenir.

Modeli, Conda bağımlılıklarını veya dağıtım yapılandırmasını değiştirmek için [Update ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-)kullanın. Aşağıdaki örnek, hizmet tarafından kullanılan modeli güncelleştirir:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Hizmeti Sil

Hizmeti silmek için [Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--)kullanın.

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a>Docker günlüğünü İnceleme

Service nesnesinden ayrıntılı Docker motoru günlük iletilerini yazdırabilirsiniz. ACI, AKS ve yerel dağıtımlar için günlüğü görüntüleyebilirsiniz. Aşağıdaki örnek günlükleri nasıl yazdırabileceğinizi gösterir.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Hizmet başlatılamadı

Görüntü başarıyla derlendikten sonra, sistem dağıtım yapılandırmanızı kullanarak bir kapsayıcı başlatmaya çalışır. Kapsayıcı başlatma işleminin bir parçası olarak, Puanlama betiğinizdeki `init()` işlev sistem tarafından çağırılır. `init()` İşlevde yakalanamayan özel durumlar varsa, hata Iletisinde **crashloopgeri** alma hatası ' nı görebilirsiniz.

Günlükleri denetlemek için [Docker günlüğünü İnceleme](#dockerlog) bölümündeki bilgileri kullanın.

## <a name="function-fails-get_model_path"></a>İşlev başarısız oldu: get_model_path ()

Genellikle, Puanlama betiğindeki `init()` işlevinde model [. get_model_path ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) işlevi, bir model dosyasını veya kapsayıcıdaki model dosyalarının bir klasörünü bulmak için çağırılır. Model dosyası veya klasörü bulunamazsa, işlev başarısız olur. Bu hatada hata ayıklamanın en kolay yolu, kapsayıcı kabuğu 'nda aşağıdaki python kodunu çalıştırmalıdır:

```python
from azureml.core.model import Model
import logging
logging.basicConfig(level=logging.DEBUG)
print(Model.get_model_path(model_name='my-best-model'))
```

Bu örnek, Puanlama betiğinizin model dosyasını veya `/var/azureml-app`klasörünü bulmasını beklediği kapsayıcıda yerel yolu (göreli olarak) yazdırır. Dosya veya klasörün gerçekten beklenen yerde olduğunu doğrulayabilirsiniz.

Günlüğe kaydetme düzeyinin hata ayıklama olarak ayarlanması ek bilgilerin günlüğe kaydedilmesine neden olabilir ve bu da hatayı belirlemek için yararlı olabilir.

## <a name="function-fails-runinput_data"></a>İşlev başarısız: çalıştırma (input_data)

Hizmet başarıyla dağıtılırsa ancak Puanlama uç noktasına veri gönderdiğinizde çöktüğünde, bunun yerine ayrıntılı hata mesajı döndürmesi için işlevinize `run(input_data)` hata yakalama ifadesini ekleyebilirsiniz. Örneğin:

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

**Note**: `run(input_data)` çağrıdan hata iletilerinin döndürülmesi yalnızca hata ayıklama amacıyla yapılmalıdır. Güvenlik nedenleriyle, bir üretim ortamında bu şekilde hata iletileri döndürmemelisiniz.

## <a name="http-status-code-502"></a>HTTP durum kodu 502

502 durum kodu hizmetin bir özel durum yaptığını veya score.py dosyasının `run()` yönteminde kilitlendiğini gösterir. Dosyada hata ayıklamak için bu makaledeki bilgileri kullanın.

## <a name="http-status-code-503"></a>HTTP durum kodu 503

Azure Kubernetes hizmet dağıtımları otomatik ölçeklendirmeyi destekler, bu da ek yükü desteklemek için çoğaltmaların eklenmesine izin verir. Ancak, otomatik Scaler, yükteki **aşamalı** değişiklikleri işlemek için tasarlanmıştır. Saniye başına isteklerde büyük ani artışlar alıyorsanız, istemciler bir HTTP durum kodu 503 alabilir.

503 durum kodlarının önlenmesine yardımcı olabilecek iki şey vardır:

* Otomatik ölçeklendirmenin yeni çoğaltmalar oluşturduğu kullanım düzeyini değiştirin.
    
    Varsayılan olarak, otomatik ölçeklendirme hedef kullanımı %70 olarak ayarlanır, bu da hizmetin saniyede %30 ' a varan (RPS) istek sayısını işleyebileceği anlamına gelir. Daha düşük bir değere ayarlayarak `autoscale_target_utilization` kullanım hedefini ayarlayabilirsiniz.

    > [!IMPORTANT]
    > Bu değişiklik çoğaltmaların *daha hızlı*oluşturulmasına neden olmaz. Bunun yerine, daha düşük bir kullanım eşiğine göre oluşturulur. Hizmetin %70 olması beklenene kadar beklemek yerine %30 kullanım gerçekleştiğinde, çoğaltmanın oluşturulmasına neden olur.
    
    Web hizmeti zaten geçerli en fazla çoğaltmaları kullanıyorsa ve 503 durum kodu görmeye devam ediyorsanız, en fazla çoğaltma sayısını artırmak için `autoscale_max_replicas` değeri arttırın.

* En az çoğaltma sayısını değiştirin. En düşük çoğaltmaları artırmak, gelen ani artışları işlemek için daha büyük bir havuz sağlar.

    En az çoğaltma sayısını artırmak için, daha yüksek `autoscale_min_replicas` bir değere ayarlayın. Aşağıdaki kodu kullanarak gerekli çoğaltmaları hesaplayabilirsiniz ve değerleri projenize özgü değerlerle değiştirin:

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

`autoscale_target_utilization`, Ve `autoscale_max_replicas` `autoscale_min_replicas` için ayarları hakkında daha fazla bilgi için, bkz. [akswebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) modül başvurusu.

## <a name="http-status-code-504"></a>HTTP durum kodu 504

504 durum kodu, isteğin zaman aşımına uğradığını gösterir. Varsayılan zaman aşımı 1 dakikadır.

Gereksiz çağrıları kaldırmak için score.py değiştirerek, zaman aşımını artırabilir veya hizmeti hızlandırmayı deneyebilirsiniz. Bu eylemler sorunu düzeltmez, score.py dosyasında hata ayıklamak için bu makaledeki bilgileri kullanın. Kod askıda bir durumda veya sonsuz bir döngüde olabilir.

## <a name="advanced-debugging"></a>Gelişmiş hata ayıklama

Bazı durumlarda, model dağıtımınızda bulunan Python kodunda etkileşimli olarak hata ayıklaması yapmanız gerekebilir. Örneğin, giriş betiği başarısız olursa ve neden ek günlüğe kaydetme ile saptanamaz. Visual Studio Code ve Visual Studio için Python Araçları (PTVSD) kullanarak Docker kapsayıcısının içinde çalışan koda iliştirebilirsiniz.

> [!IMPORTANT]
> Bu hata ayıklama yöntemi, bir modeli yerel olarak `Model.deploy()` dağıtırken `LocalWebservice.deploy_configuration` ve kullanılırken çalışmaz. Bunun yerine, [model. Package ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#package-workspace--models--inference-config-none--generate-dockerfile-false-) yöntemini kullanarak bir görüntü oluşturmanız gerekir.

Yerel Web hizmeti dağıtımları, yerel sisteminizde çalışan bir Docker yüklemesi gerektirir. Docker kullanma hakkında daha fazla bilgi için [Docker belgelerine](https://docs.docker.com/)bakın.

### <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

1. Yerel VS Code geliştirme ortamınıza Visual Studio için Python Araçları (PTVSD) yüklemek için şu komutu kullanın:

    ```
    python -m pip install --upgrade ptvsd
    ```

    VS Code ile PTVSD kullanma hakkında daha fazla bilgi için bkz. [Uzaktan hata ayıklama](https://code.visualstudio.com/docs/python/debugging#_remote-debugging).

1. Docker görüntüsüyle iletişim kurmak üzere VS Code yapılandırmak için yeni bir hata ayıklama yapılandırması oluşturun:

    1. VS Code, __Hata Ayıkla__ menüsünü ve ardından __yapılandırma aç__' ı seçin. __Launch. JSON__ adlı bir dosya açılır.

    1. __Launch. JSON__ dosyasında, içeren `"configurations": [`satırı bulun ve sonra aşağıdaki metni ekleyin:

        ```json
        {
            "name": "Azure Machine Learning: Docker Debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "localhost",
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}",
                    "remoteRoot": "/var/azureml-app"
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Konfigürasyonlar bölümünde zaten başka girdiler varsa, eklediğiniz koddan sonra bir virgül (,) ekleyin.

        Bu bölüm, 5678 numaralı bağlantı noktasını kullanarak Docker kapsayıcısına ekler.

    1. __Launch. JSON__ dosyasını kaydedin.

### <a name="create-an-image-that-includes-ptvsd"></a>PTVSD içeren bir görüntü oluşturma

1. Dağıtım için Conda ortamını, PTVSD öğesini içerecek şekilde değiştirin. Aşağıdaki örnek, `pip_packages` parametresini kullanarak eklemeyi gösterir:

    ```python
    from azureml.core.conda_dependencies import CondaDependencies 


    # Usually a good idea to choose specific version numbers
    # so training is made on same packages as scoring
    myenv = CondaDependencies.create(conda_packages=['numpy==1.15.4',            
                                'scikit-learn==0.19.1', 'pandas==0.23.4'],
                                 pip_packages = ['azureml-defaults==1.0.45', 'ptvsd'])

    with open("myenv.yml","w") as f:
        f.write(myenv.serialize_to_string())
    ```

1. PTVSD başlatmak ve hizmet başlatıldığında bir bağlantı beklemek için, `score.py` dosyanızın en üstüne aşağıdakileri ekleyin:

    ```python
    import ptvsd
    # Allows other computers to attach to ptvsd on this IP address and port.
    ptvsd.enable_attach(address=('0.0.0.0', 5678), redirect_output = True)
    # Wait 30 seconds for a debugger to attach. If none attaches, the script continues as normal.
    ptvsd.wait_for_attach(timeout = 30)
    print("Debugger attached...")
    ```

1. Ortam tanımına dayalı bir görüntü oluşturun ve görüntüyü yerel kayıt defterine çekin. Hata ayıklama sırasında görüntünün dosyalarında yeniden oluşturmanız gerekmeden değişiklikler yapmak isteyebilirsiniz. Docker görüntüsüne bir metin Düzenleyicisi (VIM) yüklemek için `Environment.docker.base_image` ve `Environment.docker.base_dockerfile` özelliklerini kullanın:

    > [!NOTE]
    > Bu örnek, Azure Machine Learning `ws` çalışma alanınıza işaret ettiğini ve `model` bu modelin dağıtılmakta olduğunu varsayar. Dosya `myenv.yml` , 1. adımda oluşturulan Conda bağımlılıklarını içerir.

    ```python
    from azureml.core.conda_dependencies import CondaDependencies
    from azureml.core.model import InferenceConfig
    from azureml.core.environment import Environment


    myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
    myenv.docker.base_image = None
    myenv.docker.base_dockerfile = "FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04\nRUN apt-get update && apt-get install vim -y"
    inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
    package = Model.package(ws, [model], inference_config)
    package.wait_for_creation(show_output=True)  # Or show_output=False to hide the Docker build logs.
    package.pull()
    ```

    Görüntü oluşturulup indirildikten sonra, görüntü yolu (Bu örnekte de kendi Özeti olan depo, ad ve etiket dahil) aşağıdakine benzer bir iletide görüntülenir:

    ```text
    Status: Downloaded newer image for myregistry.azurecr.io/package@sha256:<image-digest>
    ```

1. Görüntüyle çalışmayı kolaylaştırmak için, bir etiket eklemek üzere aşağıdaki komutu kullanın. Önceki `myimagepath` adımdaki konum değeriyle değiştirin.

    ```bash
    docker tag myimagepath debug:1
    ```

    Adımlar geri kalanında, tam görüntü yolu değeri `debug:1` yerine yerel görüntüye başvurabilirsiniz.

### <a name="debug-the-service"></a>Hizmette hata ayıkla

> [!TIP]
> `score.py` Dosyadaki PTVSD bağlantısı için bir zaman aşımı ayarlarsanız, zaman aşımı süresi dolmadan önce vs Code hata ayıklama oturumuna bağlamanız gerekir. VS Code başlatın, yerel kopyasını açın `score.py`, bir kesme noktası ayarlayın ve bu bölümdeki adımları kullanmadan önce başlamaya hazırlanın.
>
> Hata ayıklama ve kesme noktaları ayarlama hakkında daha fazla bilgi için bkz. [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging).

1. Görüntüyü kullanarak bir Docker kapsayıcısı başlatmak için aşağıdaki komutu kullanın:

    ```bash
    docker run --rm --name debug -p 8000:5001 -p 5678:5678 debug:1
    ```

1. Kapsayıcının içindeki PTVSD 'e VS Code iliştirmek için, VS Code açın ve F5 tuşunu kullanın veya __Hata Ayıkla__' yı seçin. İstendiğinde __Azure Machine Learning: Docker hata ayıklama__ yapılandırması ' nı seçin. Ayrıca, yan çubukta hata ayıklama simgesini, __Azure Machine Learning: Docker__ hata ayıklama girdisini hata ayıkla açılan menüsünden seçebilir ve ardından hata ayıklayıcıyı eklemek için yeşil oku kullanabilirsiniz.

    ![Hata ayıklama simgesi, hata ayıklamayı Başlat düğmesi ve yapılandırma Seçicisi](./media/how-to-troubleshoot-deployment/start-debugging.png)

Bu noktada, VS Code Docker kapsayıcısının içindeki PTVSD 'a bağlanır ve daha önce ayarladığınız kesme noktasında durmaktadır. Artık kodu çalışırken, değişkenleri görüntülerken vb. adımları izleyebilirsiniz.

Python 'da hata ayıklamak için VS Code kullanma hakkında daha fazla bilgi için bkz. [Python kodunuzda hata ayıklama](https://docs.microsoft.com/visualstudio/python/debugging-python-in-visual-studio?view=vs-2019).

<a id="editfiles"></a>
### <a name="modify-the-container-files"></a>Kapsayıcı dosyalarını değiştirme

Görüntüdeki dosyalarda değişiklik yapmak için çalışan kapsayıcıya iliştirebilir ve bash kabuğu yürütebilirsiniz. Buradan, dosyaları düzenlemek için VIM kullanabilirsiniz:

1. Çalışan kapsayıcıya bağlanmak ve kapsayıcıda bir bash kabuğu başlatmak için aşağıdaki komutu kullanın:

    ```bash
    docker exec -it debug /bin/bash
    ```

1. Hizmet tarafından kullanılan dosyaları bulmak için, varsayılan dizin şundan `/var/azureml-app`farklıysa, kapsayıcıda Bash kabuğundan aşağıdaki komutu kullanın:

    ```bash
    cd /var/azureml-app
    ```

    Buradan, `score.py` dosyayı düzenlemek için VIM kullanabilirsiniz. VIM kullanma hakkında daha fazla bilgi için bkz. [VIM düzenleyicisini kullanma](https://www.tldp.org/LDP/intro-linux/html/sect_06_02.html).

1. Kapsayıcıda yapılan değişiklikler normalde kalıcı olmaz. Yaptığınız değişiklikleri kaydetmek için aşağıdaki komutu kullanın, kabuktan çıkmadan önce, yukarıdaki adımda (başka bir kabukta) başlamadan önce aşağıdaki komutu kullanın:

    ```bash
    docker commit debug debug:2
    ```

    Bu komut, düzenlemelerinizi içeren adlı `debug:2` yeni bir görüntü oluşturur.

    > [!TIP]
    > Değişikliklerin etkili olabilmesi için geçerli kapsayıcıyı durdurmanız ve yeni sürümü kullanmaya başlamanız gerekir.

1. Kapsayıcıda bulunan dosyalarda yaptığınız değişiklikleri, VS Code yerel dosyalarla eşitlenmiş şekilde kaydettiğinizden emin olun. Aksi takdirde, hata ayıklayıcı deneyimi beklendiği gibi çalışmaz.

### <a name="stop-the-container"></a>Kapsayıcıyı durdurma

Kapsayıcıyı durdurmak için aşağıdaki komutu kullanın:

```bash
docker stop debug
```

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım hakkında daha fazla bilgi edinin:

* [Dağıtım ve nerede](how-to-deploy-and-where.md)
* [Öğretici: eğitim & dağıtım modelleri](tutorial-train-models-with-aml.md)
