---
title: Kubernetes hizmetine ML modellerini dağıtma
titleSuffix: Azure Machine Learning
description: Azure Kubernetes hizmetini kullanarak Azure Machine Learning modellerinizi bir Web hizmeti olarak dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, contperfq1
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 09/01/2020
ms.openlocfilehash: bfc285f68e8a44b6b09fc63d9b2775a047955a37
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604674"
---
# <a name="deploy-a-model-to-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesine model dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Kubernetes Service (AKS) üzerinde bir modeli Web hizmeti olarak dağıtmak için Azure Machine Learning kullanmayı öğrenin. Azure Kubernetes hizmeti, yüksek ölçekli üretim dağıtımları için uygundur. Aşağıdaki özelliklerde bir veya daha fazlasına ihtiyacınız varsa Azure Kubernetes hizmetini kullanın:

- __Hızlı yanıt süresi__
- Dağıtılan hizmetin __Otomatik ölçeklendirilmesi__
- __Günlüğe kaydetme__
- __Model veri koleksiyonu__
- __Kimlik Doğrulaması__
- __TLS sonlandırma__
- GPU ve alan-programlanabilir kapı dizileri (FPGA) gibi __donanım hızlandırma__ seçenekleri

Azure Kubernetes hizmetine dağıtırken, __çalışma alanınıza bağlı__bir aks kümesine dağıtırsınız. Bir AKS kümesini çalışma alanınıza bağlama hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmet kümesi oluşturma ve iliştirme](how-to-create-attach-kubernetes.md).

> [!IMPORTANT]
> Web hizmetine dağıtım yapmadan önce yerel olarak hata ayıklamanızı öneririz. Daha fazla bilgi için bkz. [yerel olarak hata ayıklama](https://docs.microsoft.com/azure/machine-learning/how-to-troubleshoot-deployment#debug-locally)
>
> Azure Machine Learning - [Yerel Not Defterine Dağıtma](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/deploy-to-local) konusuna da bakabilirsiniz

## <a name="prerequisites"></a>Önkoşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

- Bir Machine Learning modeli, çalışma alanınıza kaydedildi. Kayıtlı bir modeliniz yoksa, bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

- Bu makaledeki __Python__ kod parçacıkları aşağıdaki değişkenlerin ayarlandığı varsayılır:

    * `ws` -Çalışma alanınıza ayarlayın.
    * `model` -Kayıtlı modelinize ayarlanır.
    * `inference_config` -Modelin çıkarım yapılandırmasına ayarlayın.

    Bu değişkenleri ayarlama hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

- Bu makaledeki __CLI__ kod parçacıkları bir belge oluşturduğunuzu varsayar `inferenceconfig.json` . Bu belgeyi oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

- Çalışma alanınıza bağlı bir Azure Kubernetes hizmeti kümesi. Daha fazla bilgi için bkz. [Azure Kubernetes hizmet kümesi oluşturma ve iliştirme](how-to-create-attach-kubernetes.md).

    - Modelleri GPU düğümlerine veya FPGA düğümlerine (ya da belirli bir SKU) dağıtmak istiyorsanız, belirli SKU 'ya sahip bir küme oluşturmanız gerekir. Mevcut bir kümede ikincil düğüm havuzu oluşturma ve ikincil düğüm havuzunda modelleri dağıtma desteği yoktur.

## <a name="understand-the-deployment-processes"></a>Dağıtım süreçlerini anlama

"Dağıtım" sözcüğü hem Kubernetes hem de Azure Machine Learning için kullanılır. "Dağıtım" Bu iki bağlamda farklı anlamlara sahiptir. Kubernetes içinde, `Deployment` bildirim temelli YAML dosyası ile belirtilen somut bir varlıktır. Bir Kubernetes `Deployment` , ve gibi diğer Kubernetes varlıklarına tanımlı bir yaşam döngüsüne ve somut ilişkilerine `Pods` sahiptir `ReplicaSets` . Kubernetes nedir [?,](https://aka.ms/k8slearning)docs ve videolardan Kubernetes hakkında bilgi edinebilirsiniz.

Azure Machine Learning, "dağıtım" kullanılabilir hale getirme ve proje kaynaklarınızı temizleme konusunda genel anlamda kullanılır. Azure Machine Learning dağıtımın bir parçasını dikkate alan adımlar şunlardır:

1. . Amlignore veya. gitignore içinde belirtilen dosyaları yoksayarak proje klasörünüzdeki dosyaları zipden gönderin
1. İşlem kümenizi ölçeklendirme (Kubernetes ile Ilgilidir)
1. Dockerfile 'ı işlem düğümüne derleme veya indirme (Kubernetes ile Ilgilidir)
    1. Sistem bir karma değerini hesaplar: 
        - Temel görüntü 
        - Özel Docker adımları (bkz. [özel bir Docker temel görüntüsü kullanarak model dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-custom-docker-image))
        - Conda tanımı YAML (bkz. [oluşturma & yazılım ortamlarını Azure Machine Learning kullanma](https://docs.microsoft.com/azure/machine-learning/how-to-use-environments))
    1. Sistem bu karmayı, çalışma alanının aramasında anahtar olarak kullanır Azure Container Registry (ACR)
    1. Bulunmazsa, genel ACR 'de bir eşleşme arar
    1. Bulunamadıysanız, sistem yeni bir görüntü oluşturur (önbelleğe alınır ve çalışma alanına gönderilir ACR)
1. Daraltılmış proje dosyanızı işlem düğümündeki geçici depolamaya indirme
1. Proje dosyasının sıkıştırması kaldırılıyor
1. Yürütülen işlem düğümü `python <entry script> <arguments>`
1. Günlükler, model dosyaları ve `./outputs` çalışma alanıyla ilişkili depolama hesabına yazılan diğer dosyalar kaydediliyor
1. Geçici depolamayı kaldırma dahil olmak üzere ölçeği azaltma işlemi (Kubernetes ile Ilgilidir)

### <a name="azure-ml-router"></a>Azure ML yönlendiricisi

Gelen çıkarım isteklerini dağıtılan hizmetlere yönlendiren ön uç bileşeni (azureml-Fe) gerektiği şekilde otomatik olarak ölçeklendirilir. Azureml-Fe ölçeklendirme, AKS kümesi amacını ve boyutunu (düğüm sayısı) temel alır. Küme amacı ve düğümleri [BIR AKS kümesi oluşturduğunuzda veya](how-to-create-attach-kubernetes.md)eklediğinizde yapılandırılır. Her küme için birden çok sayıda pods üzerinde çalışan bir azureml-FE hizmeti vardır.

> [!IMPORTANT]
> __Geliştirme-test__olarak yapılandırılmış bir küme kullanılırken, kendi kendine **devre dışı bırakılır**.

Azureml-Fe, daha fazla çekirdekler kullanmak için hem yukarı (dikey) hem de (yatay olarak) daha fazla yer kullanır. Ölçeği artırma kararı verirken, gelen çıkarım isteklerini yönlendirmek için gereken süre kullanılır. Bu süre eşiği aşarsa, bir ölçek oluşur. Gelen istekleri yönlendirme süresi eşiği aşmaya devam ederse, bir genişleme meydana gelir.

Ölçeği, ve içinde ölçeklendirirken CPU kullanımı kullanılır. CPU kullanım eşiği karşılanıyorsa ön uç öncelikle aşağı ölçeklendirilir. CPU kullanımı, ölçek genişletme eşiğine düşerse, bir ölçeklendirme işlemi gerçekleşir. Ölçeği artırma ve genişletme yalnızca yeterli kullanılabilir küme kaynağı varsa oluşur.

## <a name="deploy-to-aks"></a>AKS’ye dağıtma

Azure Kubernetes hizmetine bir model dağıtmak için, gereken işlem kaynaklarını açıklayan bir __dağıtım yapılandırması__ oluşturun. Örneğin, çekirdek ve bellek sayısı. Ayrıca, modeli ve Web hizmetini barındırmak için gereken ortamı açıklayan bir __çıkarım yapılandırmasına__ihtiyacınız vardır. Çıkarım yapılandırmasını oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

> [!NOTE]
> Dağıtılacak model sayısı, dağıtım başına 1.000 modellerle sınırlıdır (kapsayıcı başına).

<a id="using-the-cli"></a>

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.webservice import AksWebservice, Webservice
from azureml.core.model import Model

aks_target = AksCompute(ws,"myaks")
# If deploying to a cluster configured for dev/test, ensure that it was created with enough
# cores and memory to handle this deployment configuration. Note that memory is also used by
# things such as dependencies and AML components.
deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config, aks_target)
service.wait_for_deployment(show_output = True)
print(service.state)
print(service.get_logs())
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AksCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute?view=azure-ml-py&preserve-view=true)
* [AksWebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py&preserve-view=true)
* [Model. deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)
* [WebService. wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#&preserve-view=truewait-for-deployment-show-output-false-)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. `myaks`AKS işlem hedefinin adıyla değiştirin. `mymodel:1`Kayıt, kayıtlı modelin adı ve sürümü ile değiştirin. `myservice`Bu hizmete verilecek adla değiştirin:

```azurecli-interactive
az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

Daha fazla bilgi için, [az ml model dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) başvurusuna bakın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

VS Code kullanımı hakkında bilgi için bkz. [vs Code uzantısı aracılığıyla AKS 'e dağıtma](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model).

> [!IMPORTANT]
> VS Code aracılığıyla dağıtmak, AKS kümesinin önceden oluşturulmasını veya çalışma alanınıza eklenmesini gerektirir.

---

### <a name="autoscaling"></a>Otomatik ölçeklendirme

Azure ML model dağıtımları için otomatik ölçeklendirmeyi işleyen bileşen, bir akıllı istek yönlendiricisi olan azureml-Fe ' dir. Tüm çıkarım istekleri üzerinden gezindiğinden, dağıtılan modelleri otomatik olarak ölçeklendirmek için gerekli veriler vardır.

> [!IMPORTANT]
> * **Model dağıtımları Için Kubernetes yatay Pod otomatik Scaler (HPA)**' i etkinleştirmeyin. Bunun yapılması, iki otomatik ölçeklendirme bileşeninin birbirleriyle yarışmasına neden olur. Azureml-Fe, Azure ML tarafından dağıtılan modelleri otomatik ölçeklendirmek üzere tasarlanmıştır. burada HPA, CPU kullanımı veya özel ölçüm yapılandırması gibi genel bir ölçüden model kullanımını tahmin etmek veya yaklaşık olarak tahmin etmek zorunda olacaktır.
> 
> * **Azureml-Fe, BIR AKS kümesindeki düğümlerin sayısını ölçeklendirmez**çünkü bu, beklenmeyen maliyet artışına yol açabilir. Bunun yerine, fiziksel küme sınırları içindeki **modelin çoğaltma sayısını ölçeklendirir** . Küme içindeki düğüm sayısını ölçeklendirmeniz gerekiyorsa, kümeyi el ile ölçeklendirebilir veya [aks kümesi otomatik Scaler 'ı yapılandırabilirsiniz](/azure/aks/cluster-autoscaler).

Otomatik ölçeklendirme `autoscale_target_utilization` ,, `autoscale_min_replicas` ve `autoscale_max_replicas` aks Web hizmeti için ayarlanarak denetlenebilir. Aşağıdaki örnek, otomatik ölçeklendirmeyi nasıl etkinleştireceğinizi göstermektedir:

```python
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=True, 
                                                autoscale_target_utilization=30,
                                                autoscale_min_replicas=1,
                                                autoscale_max_replicas=4)
```

Ölçeği artırma/azaltma kararları, geçerli kapsayıcı çoğaltmalarının kullanımına dayanır. Meşgul olan çoğaltma sayısı (bir isteği işleme) geçerli çoğaltmanın toplam sayısına bölünmüş geçerli kullanımdır. Bu sayı aşarsa `autoscale_target_utilization` , daha fazla çoğaltma oluşturulur. Daha düşükse çoğaltmalar azalır. Varsayılan olarak, hedef kullanımı %70 ' dir.

Çoğaltmaları ekleme kararları, ekip ve hızlı (1 saniye içinde). Çoğaltmaları kaldırma kararları (yaklaşık 1 dakika).

Aşağıdaki kodu kullanarak gerekli çoğaltmaları hesaplayabilirsiniz:

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

, Ve ayarı hakkında daha fazla bilgi için, `autoscale_target_utilization` `autoscale_max_replicas` `autoscale_min_replicas` bkz. [akswebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) modül başvurusu.

## <a name="deploy-models-to-aks-using-controlled-rollout-preview"></a>Denetimli dağıtımı kullanarak AKS 'e model dağıtma (Önizleme)

Uç noktaları kullanarak, denetimli bir biçimde model sürümlerini çözümleyin ve yükseltin. Tek bir uç noktanın arkasında altı adede kadar sürüm dağıtabilirsiniz. Uç noktalar aşağıdaki özellikleri sağlar:

* Her bir __uç noktaya gönderilen Puanlama trafiği yüzdesini__yapılandırın. Örneğin, trafiğin %20 ' sini ' test ' ve %80 ' i ' üretim ' noktasına yönlendirin.

    > [!NOTE]
    > Trafiğin %100 ' i için hesap yapmazsanız, kalan tüm yüzde __varsayılan__ uç nokta sürümüne yönlendirilir. Örneğin, ' test ' uç nokta sürümünü trafiğin %10 ' u ve %30 ' u için ' üretim ' olarak yapılandırırsanız, kalan %60, varsayılan uç nokta sürümüne gönderilir.
    >
    > Oluşturulan ilk uç nokta sürümü otomatik olarak varsayılan olarak yapılandırılır. Bu ayarı, `is_default=True` bir uç nokta sürümü oluştururken veya güncelleştirirken değiştirebilirsiniz.
     
* Bir uç nokta sürümünü __Denetim__ veya __işleme__olarak etiketleyin. Örneğin, geçerli üretim uç noktası sürümü denetim olabilir, ancak olası yeni modeller bir işleme sürümü olarak dağıtılır. İşlem sürümlerinin performansını değerlendirdikten sonra, bir çıktı geçerli denetimi gerçekleştirdiğinde, yeni üretime/denetime yükseltilebilir.

    > [!NOTE]
    > Yalnızca __bir__ denetiminiz olabilir. Birden çok anlaşma olabilir.

Uç noktaların ve dağıtılan sürümlerin işletimsel ölçümlerini görüntülemek için App Insights 'ı etkinleştirebilirsiniz.

### <a name="create-an-endpoint"></a>Uç nokta oluşturma
Modellerinizi dağıtmaya hazırsanız, bir Puanlama uç noktası oluşturun ve ilk sürümünüzü dağıtın. Aşağıdaki örnek, SDK kullanarak bitiş noktasının nasıl dağıtılacağını ve oluşturulacağını göstermektedir. İlk dağıtım varsayılan sürüm olarak tanımlanacak, yani tüm sürümlerde belirtilmeyen trafik yüzdelik değeri varsayılan sürüme gidecektir.  

> [!TIP]
> Aşağıdaki örnekte, yapılandırma ilk uç nokta sürümünü trafiğin %20 ' sini işleyecek şekilde ayarlar. Bu ilk uç nokta olduğundan, aynı zamanda varsayılan sürümdür. Trafiğin diğer %80 ' i için başka bir sürüm olmadığı için, bu, varsayılan olarak da yönlendirilir. Trafik yüzdesi alan diğer sürümler dağıtılana kadar bu, trafiğin %100 ' u etkili bir şekilde alır.

```python
import azureml.core,
from azureml.core.webservice import AksEndpoint
from azureml.core.compute import AksCompute
from azureml.core.compute import ComputeTarget
# select a created compute
compute = ComputeTarget(ws, 'myaks')
namespace_name= endpointnamespace
# define the endpoint and version name
endpoint_name = "mynewendpoint"
version_name= "versiona"
# create the deployment config and define the scoring traffic percentile for the first deployment
endpoint_deployment_config = AksEndpoint.deploy_configuration(cpu_cores = 0.1, memory_gb = 0.2,
                                                              enable_app_insights = True,
                                                              tags = {'sckitlearn':'demo'},
                                                              description = "testing versions",
                                                              version_name = version_name,
                                                              traffic_percentile = 20)
 # deploy the model and endpoint
 endpoint = Model.deploy(ws, endpoint_name, [model], inference_config, endpoint_deployment_config, compute)
 # Wait for he process to complete
 endpoint.wait_for_deployment(True)
 ```

### <a name="update-and-add-versions-to-an-endpoint"></a>Bir uç noktaya güncelleştirme ve sürüm ekleme

Uç noktanıza başka bir sürüm ekleyin ve Puanlama trafiği yüzdebirlik sürümünü sürüme göre yapılandırın. İki tür sürüm, bir denetim ve bir işleme sürümü vardır. Tek bir denetim sürümüne karşı karşılaştırmanıza yardımcı olmak için birden çok işleme sürümü olabilir.

> [!TIP]
> Aşağıdaki kod parçacığı tarafından oluşturulan ikinci sürüm, trafiğin %10 ' u kabul eder. İlk sürüm %20 için yapılandırılmıştır, bu nedenle trafiğin yalnızca %30 ' u belirli sürümler için yapılandırılmıştır. Kalan %70, aynı zamanda varsayılan sürüm olduğundan, ilk uç nokta sürümüne gönderilir.

 ```python
from azureml.core.webservice import AksEndpoint

# add another model deployment to the same endpoint as above
version_name_add = "versionb"
endpoint.create_version(version_name = version_name_add,
                        inference_config=inference_config,
                        models=[model],
                        tags = {'modelVersion':'b'},
                        description = "my second version",
                        traffic_percentile = 10)
endpoint.wait_for_deployment(True)
```

Mevcut sürümleri güncelleştirin veya bir uç noktada silin. Sürümün varsayılan türünü, denetim türünü ve trafik Yüzdeliğini değiştirebilirsiniz. Aşağıdaki örnekte, ikinci sürüm trafiği %40 olarak artırır ve artık varsayılandır.

> [!TIP]
> Aşağıdaki kod parçacığı sonrasında ikinci sürüm artık varsayılandır. Artık %40 için yapılandırılmıştır, ancak özgün sürüm %20 için yapılandırılmakta. Bu, trafiğin %40 ' un sürüm yapılandırmalarına göre hesaba katılmaz anlamına gelir. Artık varsayılan olduğu için soltover trafiği ikinci sürüme yönlendirilir. Trafiğin %80 ' ü etkili bir şekilde alır.

 ```python
from azureml.core.webservice import AksEndpoint

# update the version's scoring traffic percentage and if it is a default or control type
endpoint.update_version(version_name=endpoint.versions["versionb"].name,
                        description="my second version update",
                        traffic_percentile=40,
                        is_default=True,
                        is_control_version_type=True)
# Wait for the process to complete before deleting
endpoint.wait_for_deployment(true)
# delete a version in an endpoint
endpoint.delete_version(version_name="versionb")

```

## <a name="web-service-authentication"></a>Web hizmeti kimlik doğrulaması

Azure Kubernetes hizmetine dağıtım yaparken, __anahtar tabanlı__ kimlik doğrulaması varsayılan olarak etkindir. __Belirteç tabanlı__ kimlik doğrulamasını da etkinleştirebilirsiniz. Belirteç tabanlı kimlik doğrulaması, istemcilerin dağıtılan hizmette istek yapmak için kullanılan bir kimlik doğrulama belirteci istemek için bir Azure Active Directory hesabı kullanmasını gerektirir.

Kimlik doğrulamasını __devre dışı bırakmak__ için, `auth_enabled=False` dağıtım yapılandırmasını oluştururken parametresini ayarlayın. Aşağıdaki örnek, SDK 'Yı kullanarak kimlik doğrulamasını devre dışı bırakır:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, auth_enabled=False)
```

İstemci uygulamasından kimlik doğrulama hakkında daha fazla bilgi için, [Web hizmeti olarak dağıtılan Azure Machine Learning modelini](how-to-consume-web-service.md)kullanma konusuna bakın.

### <a name="authentication-with-keys"></a>Anahtarlar ile kimlik doğrulama

Anahtar kimlik doğrulaması etkinleştirilirse, `get_keys` birincil ve ikincil kimlik doğrulama anahtarını almak için yöntemini kullanabilirsiniz:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarı yeniden oluşturmanız gerekiyorsa, şunu kullanın [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py&preserve-view=true)

### <a name="authentication-with-tokens"></a>Belirteçlerle kimlik doğrulama

Belirteç kimlik doğrulamasını etkinleştirmek için, `token_auth_enabled=True` bir dağıtımı oluştururken veya güncelleştirirken parametreyi ayarlayın. Aşağıdaki örnek SDK kullanarak belirteç kimlik doğrulamasını mümkün bir şekilde sunar:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, token_auth_enabled=True)
```

Belirteç kimlik doğrulaması etkinleştirilirse, `get_token` BIR JWT belirteci almak için yöntemini ve bu belirtecin sona erme süresini kullanabilirsiniz:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirtecin zamanından sonra yeni bir belirteç istemeniz gerekir `refresh_by` .
>
> Microsoft, Azure Machine Learning çalışma alanınızı Azure Kubernetes hizmet kümeniz ile aynı bölgede oluşturmanızı kesinlikle önerir. Bir belirteçle kimlik doğrulaması yapmak için Web hizmeti, Azure Machine Learning çalışma alanınızın oluşturulduğu bölgeye bir çağrı yapar. Çalışma alanınızın bölgesi kullanılamıyorsa, kümeniz çalışma alanınızdan farklı bir bölgedeyse, Web hizmetiniz için de bir belirteç getirimeyeceksiniz. Bu, çalışma alanınızın bölgesi yeniden kullanılabilir olana kadar belirteç tabanlı kimlik doğrulamanın kullanılamamasına neden olur. Ayrıca, kümenizin bölgesi ve çalışma alanınızın bölgesi arasındaki mesafe ne kadar fazlaysa bir belirteci getirmek için bu daha uzun sürer.
>
> Bir belirteci almak için Azure Machine Learning SDK veya [az ml Service Get-Access-Token](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext-azure-cli-ml-az-ml-service-get-access-token) komutunu kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal ağı ile güvenli ınkrime ortamı](how-to-secure-inferencing-vnet.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Web hizmetini güncelleştirme](how-to-deploy-update-web-service.md)
* [TLS kullanarak Azure Machine Learning aracılığıyla web hizmetinin güvenliğini sağlama](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir ML modelini kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
