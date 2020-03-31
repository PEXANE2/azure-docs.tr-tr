---
title: Dağıtım sorunlarını giderme kılavuzu
titleSuffix: Azure Machine Learning
description: Azure Makine Öğrenimi'ni kullanarak Azure Kubernetes Hizmeti ve Azure Kapsayıcı Örnekleri ile ortak Docker dağıtım hatalarını nasıl çözeceğinizi, çözeceğinizi ve sorun gidereceğinizi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399675"
---
# <a name="troubleshooting-azure-machine-learning-azure-kubernetes-service-and-azure-container-instances-deployment"></a>Sorun Giderme Azure Machine Learning Azure Kubernetes Hizmeti ve Azure Kapsayıcı Örnekleri dağıtımı

Azure Makine Öğrenimi'ni kullanarak Azure Kapsayıcı Örnekleri (ACI) ve Azure Kubernetes Hizmeti (AKS) ile ortak Docker dağıtım hatalarını nasıl çözeceğinizi veya çözeceğinizi öğrenin.

Azure Machine Learning'de bir model dağıtırken, sistem bir dizi görev gerçekleştirir.

Model dağıtımı için önerilen ve en güncel yaklaşım, giriş parametresi olarak bir [Ortam](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments) nesnesi kullanan [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) API'si üzerinden dir. Bu durumda hizmetimiz dağıtım aşamasında sizin için bir temel docker görüntüsü oluşturacak ve gerekli modelleri tek bir çağrıda monte edecektir. Temel dağıtım görevleri şunlardır:

1. Modeli çalışma alanı modeli kayıt defterine kaydedin.

2. Çıkarım Yapılandırması'nı tanımla:
    1. Çevre yaml dosyasında belirttiğiniz bağımlılıkları temel alan bir [Çevre](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments) nesnesi oluşturun veya satın alınan ortamlarımızdan birini kullanın.
    2. Ortama ve puanlama komut dosyasına dayalı bir çıkarım yapılandırması (InferenceConfig nesnesi) oluşturun.

3. Modeli Azure Kapsayıcı Örneği (ACI) hizmetine veya Azure Kubernetes Hizmetine (AKS) dağıtın.

[Model Yönetimi](concept-model-management-and-deployment.md) girişinde bu süreç hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği.** Yoksa, [Azure Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.
* [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Azure Machine Learning için CLI uzantısı.](reference-azure-machine-learning-cli.md)
* Yerel hata ayıklama için, yerel sisteminizde çalışan bir Docker yüklemeniz olması gerekir.

    Docker yüklemenizi doğrulamak için `docker run hello-world` bir terminal veya komut istemikomutunu kullanın. Docker'ı yükleme veya Docker hatalarını giderme hakkında bilgi için [Docker Belgeleri'ne](https://docs.docker.com/)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Herhangi bir sorunla karşılaşıyorsanız, yapmanız gereken ilk şey, dağıtımı görevini (önceki açıklanan) sorunu yalıtmak için tek tek adımlara bölmektir.

Giriş parametresi olarak bir [Ortam](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments) nesnesi ile [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) API üzerinden yeni/önerilen dağıtım yöntemini kullandığınızı varsayarsak, kodunuz üç ana adıma ayrılabilir:

1. Modeli kaydedin. Burada bazı örnek kod:

    ```python
    from azureml.core.model import Model


    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Dağıtım için çıkarım yapılandırması tanımlayın:

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

Dağıtım işlemini tek tek görevlere ayırdıktan sonra, en yaygın hatalardan bazılarına bakabiliriz.

## <a name="debug-locally"></a>Yerel hata ayıklama

Bir modeli ACI veya AKS'ye dağıtmada sorun yaşıyorsanız, modeli yerel bir web hizmeti olarak dağıtmayı deneyin. Yerel bir web hizmeti kullanmak sorunları gidermeyi kolaylaştırır. Modeli içeren Docker görüntüsü indirilir ve yerel sisteminizde başlatılır.

> [!WARNING]
> Yerel web hizmeti dağıtımları üretim senaryoları için desteklenmez.

Yerel olarak dağıtmak için, dağıtım `LocalWebservice.deploy_configuration()` yapılandırması oluşturmak için kodunuzu kullanılacak şekilde değiştirin. Ardından `Model.deploy()` hizmeti dağıtmak için kullanın. Aşağıdaki örnek, bir modeli (model değişkeninde bulunan) yerel bir web hizmeti olarak dağıtır:

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

Kendi conda belirtiminizi YAML olarak tanımlıyorsanız, azureml varsayılanlarını >= 1,0,45 sürümüyle pip bağımlılığı olarak listelemeniz gerektiğini lütfen unutmayın. Bu paket, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerir.

Bu noktada, hizmetle normal şekilde çalışabilirsiniz. Örneğin, aşağıdaki kod hizmete veri göndermeyi gösterir:

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

Python ortamınızı özelleştirme hakkında daha fazla bilgi için [eğitim ve dağıtım ortamları oluşturma ve yönetme'ye](how-to-use-environments.md)bakın. 

### <a name="update-the-service"></a>Hizmeti güncelleştirme

Yerel sınama sırasında, günlük `score.py` eklemek veya keşfettiğiniz sorunları çözmeye çalışmak için dosyayı güncelleştirmeniz gerekebilir. Dosyadaki `score.py` değişiklikleri yeniden yüklemek `reload()`için . Örneğin, aşağıdaki kod hizmetin komut dosyasını yeniden yükler ve ardından ona veri gönderir. Veriler güncelleştirilmiş `score.py` dosya kullanılarak puanlandırılır:

> [!IMPORTANT]
> Yöntem `reload` yalnızca yerel dağıtımlar için kullanılabilir. Dağıtımın başka bir bilgi işlem hedefine güncelleştirilmesi hakkında bilgi [için, Dağıtım modellerinin](how-to-deploy-and-where.md#update)güncelleştirme bölümüne bakın.

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Komut dosyası, hizmet tarafından kullanılan `InferenceConfig` nesne tarafından belirtilen konumdan yeniden yüklenir.

Modeli, Conda bağımlılıklarını veya dağıtım yapılandırmasını değiştirmek için [güncelleştirmeyi kullanın.)](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-) Aşağıdaki örnek, hizmet tarafından kullanılan modeli güncelleştirir:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Hizmeti silme

Hizmeti silmek için [delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--)kullanın.

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a>Docker günlüğünü inceleyin

Servis nesnesinden ayrıntılı Docker motor günlüğü iletilerinin çıktısını alabilirsiniz. ACI, AKS ve Yerel dağıtımların günlüğünü görüntüleyebilirsiniz. Aşağıdaki örnek, günlüklerin nasıl yazdırılabildiğini gösterir.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Hizmet başlatma başarısız oldu

Görüntü başarıyla oluşturuldıktan sonra, sistem dağıtım yapılandırmanızı kullanarak bir kapsayıcı başlatmayı dener. Kapsayıcı başlatma işleminin bir parçası `init()` olarak, puanlama komut dosyasınızdaki işlev sistem tarafından çağrılır. `init()` İşlevde yakalanmayan özel durumlar varsa, hata iletisinde **CrashLoopBackOff** hatasını görebilirsiniz.

Günlükleri kontrol etmek için Docker günlük lerini [inceleyin](#dockerlog) bölümündeki bilgileri kullanın.

## <a name="function-fails-get_model_path"></a>Fonksiyon başarısız olur: get_model_path()

Genellikle, puanlama `init()` komut dosyasındaki işlevde, [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) işlevi, kapsayıcıdaki bir model dosyasını veya model dosyaları klasörünü bulmak için çağrılır. Model dosyası veya klasörü bulunamıyorsa, işlev başarısız olur. Bu hatayı ayıklamanın en kolay yolu, aşağıdaki Python kodunu Kapsayıcı kabuğunda çalıştırmaktır:

```python
from azureml.core.model import Model
import logging
logging.basicConfig(level=logging.DEBUG)
print(Model.get_model_path(model_name='my-best-model'))
```

Bu örnek, puanlama komut dosyanızın `/var/azureml-app`model dosyasını veya klasörünü bulmayı beklediği kapsayıcıda yerel yolu (göreli) yazdırır. Daha sonra dosyanın veya klasörün gerçekten olması beklenen yerde olup olmadığını doğrulayabilirsiniz.

Günlüğe kaydetme düzeyini HATA Ayıklama olarak ayarlamak, ek bilgilerin günlüğe kaydedilmesine neden olabilir ve bu da hatayı tanımlamada yararlı olabilir.

## <a name="function-fails-runinput_data"></a>İşlev başarısız olur: run(input_data)

Hizmet başarıyla dağıtılırsa, ancak skorer uç noktasına veri gönderdiğinizde çöküyorsa, işlevinize `run(input_data)` hata yakalama deyimi ekleyerek bunun yerine ayrıntılı hata iletisi döndürebilirsiniz. Örnek:

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

**Not**: `run(input_data)` Aramadan gelen hata iletilerinin döndürülmesi yalnızca hata ayıklama amacıyla yapılmalıdır. Güvenlik nedenleriyle, üretim ortamında hata iletilerini bu şekilde döndürmemelisiniz.

## <a name="http-status-code-502"></a>HTTP durum kodu 502

502 durum kodu, hizmetin bir özel durum attığını `run()` veya score.py dosyasının yönteminde kilitlendiğini gösterir. Dosyayı hata ayıklamak için bu makaledeki bilgileri kullanın.

## <a name="http-status-code-503"></a>HTTP durum kodu 503

Azure Kubernetes Hizmet dağıtımları otomatik ölçeklemeyi destekler ve bu da yinelemelerin ek yükü desteklemek için eklenmesini sağlar. Ancak, otomatik ölçeklendirici **yükteki kademeli** değişiklikleri işlemek üzere tasarlanmıştır. Saniyede isteklerde büyük artışlar alırsanız, istemciler bir HTTP durum kodu 503 alabilir.

503 durum kodunu önlemeye yardımcı olabilecek iki şey vardır:

* Otomatik ölçeklendirmenin yeni yinelemeler oluşturduğu kullanım düzeyini değiştirin.
    
    Varsayılan olarak, hedef kullanımı otomatiklendirme %70 olarak ayarlanır, bu da hizmetin saniyede %30'a varan isteklerde (RPS) ani artışlar işleyebilir anlamına gelir. Daha düşük bir `autoscale_target_utilization` değere ayarlayarak kullanım hedefini ayarlayabilirsiniz.

    > [!IMPORTANT]
    > Bu değişiklik yinelemelerin *daha hızlı*oluşturulmasına neden olmaz. Bunun yerine, daha düşük bir kullanım eşiği oluşturulur. Hizmetin %70'ini bekleyene kadar beklemek yerine, değeri %30 olarak değiştirmek, %30 kullanım gerçekleştiğinde yinelemelerin oluşturulmasına neden olur.
    
    Web hizmeti zaten geçerli max yinelemeleri kullanıyorsa ve hala 503 `autoscale_max_replicas` durum kodları görüyorsanız, en fazla çoğaltma sayısını artırmak için değeri artırın.

* En az çoğaltma sayısını değiştirin. Minimum yinelemeleri artırmak, gelen ani artışları işlemek için daha büyük bir havuz sağlar.

    En az çoğaltma sayısını artırmak `autoscale_min_replicas` için daha yüksek bir değere ayarlayın. Aşağıdaki kodu kullanarak, değerleri projenize özgü değerlerle değiştirerek gerekli yinelemeleri hesaplayabilirsiniz:

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
    > Yeni minimum yinelemelerin işleyebileceklerinden daha büyük istek artışları alırsanız, yeniden 503'ler alabilirsiniz. Örneğin, hizmetinizin trafiği arttıkça, minimum yinelemeleri artırmanız gerekebilir.

`autoscale_target_utilization`Ayar hakkında daha `autoscale_max_replicas`fazla `autoscale_min_replicas` bilgi için , ve için, [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) modülü referans bakın.

## <a name="http-status-code-504"></a>HTTP durum kodu 504

504 durum kodu, isteğin zaman önce sona erdiğini gösterir. Varsayılan zaman dilimi 1 dakikadır.

Gereksiz çağrıları kaldırmak için score.py değiştirerek zaman ağını artırabilir veya hizmeti hızlandırmayı deneyebilirsiniz. Bu eylemler sorunu düzeltmezse, score.py dosyasını hata ayıklamak için bu makaledeki bilgileri kullanın. Kod asılı bir durumda veya sonsuz bir döngü olabilir.

## <a name="advanced-debugging"></a>Gelişmiş hata ayıklama

Bazı durumlarda, model dağıtımınızda bulunan Python kodunu etkileşimli olarak hata ayıklamanız gerekebilir. Örneğin, giriş komut dosyası başarısız sayılsa ve nedeni ek günlüğe kaydetme yle belirlenemiyorsa. Visual Studio Code ve Python Tools for Visual Studio (PTVSD) araçlarını kullanarak Docker kapsayıcısının içinde çalışan koda ekleyebilirsiniz.

> [!IMPORTANT]
> Bu hata ayıklama yöntemi, `Model.deploy()` bir modeli `LocalWebservice.deploy_configuration` kullanırken ve yerel olarak dağıtmak için çalışmaz. Bunun yerine, [Model.package()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#package-workspace--models--inference-config-none--generate-dockerfile-false-) yöntemini kullanarak bir görüntü oluşturmanız gerekir.

Yerel web hizmeti dağıtımları, yerel sisteminizde çalışan bir Docker yüklemesi gerektirir. Docker kullanma hakkında daha fazla bilgi için [Docker Belgeleri'ne](https://docs.docker.com/)bakın.

### <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

1. Python Araçlar Görsel Studio (PTVSD) için yerel VS Kodu geliştirme ortamı yüklemek için aşağıdaki komutu kullanın:

    ```
    python -m pip install --upgrade ptvsd
    ```

    VS Code ile PTVSD kullanma hakkında daha fazla bilgi için [Uzaktan Hata Ayıklama](https://code.visualstudio.com/docs/python/debugging#_remote-debugging)bölümüne bakın.

1. Docker görüntüsüyle iletişim kuracak şekilde VS Kodunu yapılandırmak için yeni bir hata ayıklama yapılandırması oluşturun:

    1. VS Kodu'ndan __Hata Ayıklama__ menüsünü seçin ve ardından __yapılandırmaları aç'ı__seçin. __launch.json__ adlı bir dosya açılır.

    1. __launch.json__ dosyasında, içeren `"configurations": [`satırı bulun ve aşağıdaki metni ekleyin:

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
        > Yapılandırmalar bölümünde zaten başka girişler varsa, eklediğiniz koddan sonra virgül (,) ekleyin.

        Bu bölüm, 5678 numaralı bağlantı noktasını kullanarak Docker konteynerine bağlanır.

    1. __Launch.json__ dosyasını kaydedin.

### <a name="create-an-image-that-includes-ptvsd"></a>PTVSD içeren bir görüntü oluşturma

1. Dağıtımınız için conda ortamını PTVSD içermesine göre değiştirin. Aşağıdaki örnek, `pip_packages` parametre kullanarak ekleyerek gösterir:

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

1. PTVSD'yi başlatmak ve hizmet başladığında bağlantı beklemek için dosyanızın `score.py` en üstüne aşağıdakileri ekleyin:

    ```python
    import ptvsd
    # Allows other computers to attach to ptvsd on this IP address and port.
    ptvsd.enable_attach(address=('0.0.0.0', 5678), redirect_output = True)
    # Wait 30 seconds for a debugger to attach. If none attaches, the script continues as normal.
    ptvsd.wait_for_attach(timeout = 30)
    print("Debugger attached...")
    ```

1. Ortam tanımına dayalı bir görüntü oluşturun ve görüntüyü yerel kayıt defterine çekin. Hata ayıklama sırasında, yeniden oluşturmanız gerekmeden görüntüdeki dosyalarda değişiklik yapmak isteyebilirsiniz. Docker görüntüsüne bir metin düzenleyicisi (vim) yüklemek için aşağıdakileri `Environment.docker.base_image` ve `Environment.docker.base_dockerfile` özelliklerini kullanın:

    > [!NOTE]
    > Bu örnek, `ws` Azure Machine Learning çalışma alanınızı `model` işaret ettiğini ve dağıtılan modelin bu olduğunu varsayar. Dosya, `myenv.yml` adım 1'de oluşturulan conda bağımlılıklarını içerir.

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

    Görüntü oluşturulduktan ve indirildikten sonra, görüntü yolu (bu durumda da özeti olan depo, ad ve etiket içerir) aşağıdakilere benzer bir iletide görüntülenir:

    ```text
    Status: Downloaded newer image for myregistry.azurecr.io/package@sha256:<image-digest>
    ```

1. Görüntüyle çalışmayı kolaylaştırmak için etiket eklemek için aşağıdaki komutu kullanın. Önceki `myimagepath` adımdaki konum değeriyle değiştirin.

    ```bash
    docker tag myimagepath debug:1
    ```

    Adımların geri kalanı için, yerel görüntüye `debug:1` tam görüntü yolu değeri yerine başvurabilirsiniz.

### <a name="debug-the-service"></a>Hizmeti hata ayıklama

> [!TIP]
> `score.py` Dosyadaki PTVSD bağlantısı için bir zaman aşımı ayarladıysanız, zaman aşımı dolmadan önce VS Kodunu hata ayıklama oturumuna bağlamanız gerekir. VS Kodunu başlatın, yerel `score.py`kopyasını açın, bir kesme noktası ayarlayın ve bu bölümdeki adımları kullanmadan önce kullanıma hazır olsun.
>
> Hata ayıklama ve kesme noktaları ayarlama hakkında daha fazla bilgi için Hata [Ayıklama](https://code.visualstudio.com/Docs/editor/debugging)bölümüne bakın.

1. Görüntüyü kullanarak docker kapsayıcısı başlatmak için aşağıdaki komutu kullanın:

    ```bash
    docker run --rm --name debug -p 8000:5001 -p 5678:5678 debug:1
    ```

1. Vs Kodunu kabın içindeki PTVSD'ye eklemek için VS Kodunu açın ve F5 tuşunu kullanın veya __Hata Ayıklama'yı__seçin. İstendiğinde Azure __Machine Learning: Docker Hata Ayıklama__ yapılandırmasını seçin. Ayrıca yan çubuktan hata ayıklama simgesini, __Azure Machine Learning: Hata Ayıklama__ menüsünden Docker Hata Ayıklama girişini seçebilir ve hata ayıklama aracını takmak için yeşil oku kullanabilirsiniz.

    ![Hata ayıklama simgesi, hata ayıklama düğmesini başlat ve yapılandırma seçici](./media/how-to-troubleshoot-deployment/start-debugging.png)

Bu noktada, VS Kodu Docker kapsayıcısı içinde PTVSD'ye bağlanır ve daha önce ayarladığınız kesme noktasında durur. Artık çalışırken kodun içinden geçebilir, değişkenleri görüntüleyebilir, vb.

Python'u hata ayıklamak için VS Kodunu kullanma hakkında daha fazla bilgi için [Python kodunuzu hata ayıklama](https://docs.microsoft.com/visualstudio/python/debugging-python-in-visual-studio?view=vs-2019)bölümüne bakın.

<a id="editfiles"></a>
### <a name="modify-the-container-files"></a>Kapsayıcı dosyalarını değiştirme

Görüntüdeki dosyalarda değişiklik yapmak için, çalışan kapsayıcıya ekleyebilir ve bir bash kabuğu çalıştırabilirsiniz. Buradan dosyaları yeniden canlandırmak için vim kullanabilirsiniz:

1. Çalışan konteynere bağlanmak ve kapsayıcıya bir bash mermisi fırlatmak için aşağıdaki komutu kullanın:

    ```bash
    docker exec -it debug /bin/bash
    ```

1. Hizmet tarafından kullanılan dosyaları bulmak için, varsayılan dizin farklıysa, kapsayıcıdaki bash kabuğundan aşağıdaki komutu `/var/azureml-app`kullanın:

    ```bash
    cd /var/azureml-app
    ```

    Buradan, dosyayı `score.py` yeniden canlandırmak için vim kullanabilirsiniz. vim kullanma hakkında daha fazla bilgi için [Vim düzenleyicisini kullanma bilgisine](https://www.tldp.org/LDP/intro-linux/html/sect_06_02.html)bakın.

1. Kapsayıcıdaki değişiklikler normalde kalıcı değildir. Yaptığınız değişiklikleri kaydetmek için, yukarıdaki adımda başlatılan kabuktan çıkmadan önce (başka bir kabukta):

    ```bash
    docker commit debug debug:2
    ```

    Bu komut, sizin de `debug:2` yer alan yeni bir görüntü oluşturur.

    > [!TIP]
    > Değişiklikler yürürlüğe girmeden önce geçerli kapsayıcıyı durdurmanız ve yeni sürümü kullanmaya başlamanız gerekir.

1. Kapsayıcıdaki dosyalarda yaptığınız değişiklikleri VS Code'un kullandığı yerel dosyalarla eşit olarak tuttuğundan emin olun. Aksi takdirde, hata ayıklama deneyimi beklendiği gibi çalışmaz.

### <a name="stop-the-container"></a>Kapsayıcıyı durdurma

Kapsayıcıyı durdurmak için aşağıdaki komutu kullanın:

```bash
docker stop debug
```

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım hakkında daha fazla bilgi edinin:

* [Nasıl dağıtılır ve nerede](how-to-deploy-and-where.md)
* [Öğretici: Tren & dağıtmak modelleri](tutorial-train-models-with-aml.md)
