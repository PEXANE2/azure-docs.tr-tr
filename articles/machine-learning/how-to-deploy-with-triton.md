---
title: Triton ile hizmet veren yüksek performanslı model (Önizleme)
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 'da Üçlü çıkarım sunucusuyla model dağıtmayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.date: 09/23/2020
ms.topic: conceptual
ms.reviewer: larryfr
ms.custom: deploy
ms.openlocfilehash: 3a3600c4065d331ca1cfc129cd55dd56add21424
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92428350"
---
# <a name="high-performance-serving-with-triton-inference-server-preview"></a>Triton çıkarım sunucusuyla yüksek performanslı hizmet (Önizleme) 

Model çıkarımı için kullanılan Web hizmetinin performansını artırmak için [NVIDIA Triton çıkarım sunucusunu](https://developer.nvidia.com/nvidia-triton-inference-server) nasıl kullanacağınızı öğrenin.

Çıkarımı için model dağıtma yöntemlerinden biri Web hizmeti olarak belirlenir. Örneğin, Azure Kubernetes hizmetine veya Azure Container Instances bir dağıtım. Varsayılan olarak, Azure Machine Learning Web hizmeti dağıtımları için tek iş parçacıklı, *genel amaçlı* Web çerçevesi kullanır.

Triton, *çıkarım için iyileştirilmiş*bir çerçevedir. GPU 'ların daha iyi kullanımını ve düşük maliyetli çıkarımı sağlar. Sunucu tarafında, gelen istekleri işler ve bu toplu işlemleri çıkarımı için gönderir. Toplu işleme GPU kaynaklarını daha iyi kullanır ve önemli bir performans parçasıdır.

> [!IMPORTANT]
> Azure Machine Learning dağıtım için Triton kullanımı Şu anda __Önizleme__aşamasındadır. Önizleme işlevselliği müşteri desteğinin kapsamına alınmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

> [!TIP]
> Bu belgedeki kod parçacıkları tanım amaçlıdır ve bir çözümü tamamen gösteremeyebilir. Çalışan örnek kod için [Azure Machine Learning üç aylık dönemin uçtan uca örneklerine](https://github.com/Azure/azureml-examples/tree/main/tutorials)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Bir **Azure aboneliği**. Bir tane yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.
* Azure Machine Learning [bir modelin nasıl ve nasıl dağıtılacağı hakkında](how-to-deploy-and-where.md) benzerlik.
* [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py) **veya** [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) ve [Machine Learning uzantısı](reference-azure-machine-learning-cli.md).
* Yerel test için çalışan bir Docker yüklemesi. Docker 'ı yükleme ve doğrulama hakkında daha fazla bilgi için bkz. Docker belgelerindeki [Yönlendirme ve kurulum](https://docs.docker.com/get-started/) .

## <a name="architectural-overview"></a>Mimariye genel bakış

Kendi modeliniz için Triton kullanmayı denemeden önce, Azure Machine Learning nasıl çalıştığını ve varsayılan bir dağıtım ile nasıl Karşılaştırıldığı anlamak önemlidir.

**Triton olmadan varsayılan dağıtım**

* Birden çok [Gunic,](https://gunicorn.org/) gelen istekleri eşzamanlı olarak işleyecek şekilde başlatılır.
* Bu çalışanlar ön işleme, modeli çağırma ve işlem sonrası işlemleri işler. 
* Çıkarım istekleri __Puanlama URI__'sini kullanır. Örneğin, `https://myserevice.azureml.net/score`.

:::image type="content" source="./media/how-to-deploy-with-triton/normal-deploy.png" alt-text="Normal, üç aylık olmayan dağıtım mimarisi diyagramı":::

**Triton ile çıkarım yapılandırma dağıtımı**

* Birden çok [Gunic,](https://gunicorn.org/) gelen istekleri eşzamanlı olarak işleyecek şekilde başlatılır.
* İstekler, **Triton sunucusuna**iletilir. 
* Triton, GPU kullanımını en üst düzeye çıkarmak için istekleri toplu halde işler.
* İstemci, istek yapmak için __Puanlama URI__ 'sini kullanır. Örneğin, `https://myserevice.azureml.net/score`.

:::image type="content" source="./media/how-to-deploy-with-triton/inferenceconfig-deploy.png" alt-text="Normal, üç aylık olmayan dağıtım mimarisi diyagramı":::

Model dağıtımınız için Triton kullanmak üzere iş akışı:

1. Triton 'in modelinize hizmeti verebildiğini doğrulayın.
1. İstekleri, Triton dağıtılan modelinize gönderebildiğinizi doğrulayın.
1. Triton 'e özgü kodunuzu AML dağıtımınıza ekleyin.

## <a name="optional-define-a-model-config-file"></a>Seçim Model yapılandırma dosyası tanımlama

Model yapılandırma dosyası, kaç giriş olduğunu ve bu girişlerin hangi boyutlarda olacağını gösterir. Yapılandırma dosyasını oluşturma hakkında daha fazla bilgi için bkz. NVıDıA belgelerindeki [model yapılandırması](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/model_configuration.html) .

> [!TIP]
> `--strict-model-config=false`Triton çıkarım sunucusunu başlatırken seçeneğini kullanıyoruz, bu da `config.pbtxt` Onnx veya TensorFlow modelleri için bir dosya sağlamanız gerekmediği anlamına gelir.
> 
> Bu seçenek hakkında daha fazla bilgi için bkz. NVıDıA belgelerinde [oluşturulan model yapılandırması](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/model_configuration.html#generated-model-configuration) .

## <a name="directory-structure"></a>Dizin yapısı

Azure Machine Learning bir modeli kaydederken, tek tek dosyaları veya dizin yapısını kaydedebilirsiniz. Triton kullanmak için model kaydı, adlı dizini içeren bir dizin yapısı için olmalıdır `triton` . Bu dizinin genel yapısı:

```bash
models
    - triton
        - model_1
            - model_version
                - model_file
                - config_file
        - model_2
            ...
```

> [!IMPORTANT]
> Bu dizin yapısı bir Triton model deposudur ve modelinlerinizin Triton ile çalışması için gereklidir. Daha fazla bilgi için bkz. NVıDıA belgelerindeki [Triton modeli depoları](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/model_repository.html) .

## <a name="test-with-triton-and-docker"></a>Triton ve Docker ile test etme

Modelle çalıştığından emin olmak üzere modelinizi test etmek için Docker kullanabilirsiniz. Aşağıdaki komutlar, üç aylık kapsayıcıyı yerel bilgisayarınıza çeker ve ardından Triton sunucusunu başlatır:

1. Triton sunucusunun görüntüsünü yerel bilgisayarınıza çekmek için aşağıdaki komutu kullanın:

    ```bash
    docker pull nvcr.io/nvidia/tritonserver:20.09-py3
    ```

1. Triton sunucusunu başlatmak için aşağıdaki komutu kullanın. `<path-to-models/triton>`Modellerinizi Içeren Triton modeli deposunun yoluyla değiştirin:

    ```bash
    docker run --rm -ti -v<path-to-models/triton>:/models nvcr.io/nvidia/tritonserver:20.09-py3 tritonserver --model-repository=/models --strict-model-config=false
    ```

    > [!IMPORTANT]
    > Windows kullanıyorsanız, komutu ilk kez çalıştırdığınızda bu işleme yönelik ağ bağlantılarına izin vermeniz istenebilir. Öyleyse, erişimi etkinleştirmek için seçin.

    Başlatıldıktan sonra aşağıdaki metne benzer bilgiler komut satırına kaydedilir:

    ```bash
    I0923 19:21:30.582866 1 http_server.cc:2705] Started HTTPService at 0.0.0.0:8000
    I0923 19:21:30.626081 1 http_server.cc:2724] Started Metrics Service at 0.0.0.0:8002
    ```

    İlk satır, Web hizmetinin adresini gösterir. Bu durumda, ile `0.0.0.0:8000` aynıdır `localhost:8000` .

1. Sistem durumu uç noktasına erişmek için kıvrımlı gibi bir yardımcı program kullanın.

    ```bash
    curl -L -v -i localhost:8000/v2/health/ready
    ```

    Bu komut şuna benzer bilgiler döndürür. Göz önünde, `200 OK` Bu durum Web sunucusunun çalıştığı anlamına gelir.

    ```bash
    *   Trying 127.0.0.1:8000...
    * Connected to localhost (127.0.0.1) port 8000 (#0)
    > GET /v2/health/ready HTTP/1.1
    > Host: localhost:8000
    > User-Agent: curl/7.71.1
    > Accept: */*
    >
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 200 OK
    HTTP/1.1 200 OK
    ```

Temel bir sistem durumu denetiminin ötesinde, çıkarım için Triton 'a veri göndermek üzere bir istemci oluşturabilirsiniz. İstemci oluşturma hakkında daha fazla bilgi için NVıDıA belgelerindeki [istemci örneklerine](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_example.html) bakın. Ayrıca [, Üçlü GitHub 'Da Python örnekleri](https://github.com/triton-inference-server/server/tree/master/src/clients/python/examples)de vardır.

Docker kullanma hakkında daha fazla bilgi için bkz. GPU [ile bir sistemde bir sistem üzerinde Triton çalıştırma](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/run.html#running-triton-on-a-system-with-a-gpu) ve [GPU olmadan bir sistem üzerinde Triton](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/run.html#running-triton-on-a-system-without-a-gpu)çalıştırma.

## <a name="register-your-model"></a>Modelinizi kaydetme

Modelinizin Triton ile çalıştığını doğruladığınıza göre, bunu Azure Machine Learning kaydedin. Model kaydı, model dosyalarınızı Azure Machine Learning çalışma alanında depolar ve Python SDK ve Azure CLı ile dağıtma sırasında kullanılır.

Aşağıdaki örneklerde, modelinin nasıl kaydedileceği gösterilmektedir:

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.model import Model

model = Model.register(
    model_path=os.path.join("..", "triton"),
    model_name="bidaf_onnx",
    tags={'area': "Natural language processing", 'type': "Question answering"},
    description="Question answering model from ONNX model zoo",
    workspace=ws
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az ml model register --model-path='triton' \
--name='bidaf_onnx' \
--workspace-name='<my_workspace>'
```
---

<a id="processing"></a>

## <a name="add-pre-and-post-processing"></a>Ön ve son işlem Ekle

Web hizmetinin çalıştığını doğruladıktan sonra, bir _giriş betiği_tanımlayarak, ön ve son işleme kodu ekleyebilirsiniz. Bu dosya adı `score.py` . Giriş betikleri hakkında daha fazla bilgi için bkz. [bir giriş betiği tanımlama](how-to-deploy-and-where.md#define-an-entry-script).

İki ana adım, yönteinizde bir üç aylık HTTP istemcisini başlatmak `init()` ve işlevinizde bu istemciye çağırmak için kullanılır `run()` .

### <a name="initialize-the-triton-client"></a>Triton Istemcisini başlatma

Dosyanıza aşağıdaki örneğe benzer bir kod ekleyin `score.py` . Azure Machine Learning, localhost, bağlantı noktası 8000 ' de değinilmesi beklenir. Bu durumda, localhost yerel makinenizde bir bağlantı noktası değil, bu dağıtım için Docker görüntüsünün içindedir:

> [!TIP]
> `tritonhttpclient`PIP paketi, seçkin `AzureML-Triton` ortamda bulunur, bu nedenle bunu bir PIP bağımlılığı olarak belirtmeye gerek yoktur.

```python
import tritonhttpclient

def init():
    global triton_client
    triton_client = tritonhttpclient.InferenceServerClient(url="localhost:8000")
```

### <a name="modify-your-scoring-script-to-call-into-triton"></a>Üç aylık arama yapmak için Puanlama betiğinizi değiştirin

Aşağıdaki örnek, model için meta verilerin dinamik olarak nasıl isteneceğini gösterir:

> [!TIP]
> Triton istemcisinin yöntemini kullanarak, üç aylık olarak yüklenmiş modellerin meta verilerini dinamik olarak isteyebilirsiniz `.get_model_metadata` . Kullanım örneği için [örnek Not defteri](https://aka.ms/triton-aml-sample) ' ne bakın.

```python
input = tritonhttpclient.InferInput(input_name, data.shape, datatype)
input.set_data_from_numpy(data, binary_data=binary_data)

output = tritonhttpclient.InferRequestedOutput(
         output_name, binary_data=binary_data, class_count=class_count)

# Run inference
res = triton_client.infer(model_name,
                          [input]
                          request_id='0',
                          outputs=[output])

```

<a id="redeploy"></a>

## <a name="redeploy-with-an-inference-configuration"></a>Çıkarım yapılandırmasıyla yeniden dağıtın

Bir çıkarım yapılandırması, Python SDK veya Azure CLı kullanarak Azure Machine Learning dağıtım işleminin yanı sıra bir giriş betiği kullanmanıza olanak sağlar.

> [!IMPORTANT]
> `AzureML-Triton` [Seçkin ortamı](./resource-curated-environments.md)belirtmeniz gerekir.
>
> Python kod örneği `AzureML-Triton` adlı başka bir ortama klonlar `My-Triton` . Azure CLı kodu bu ortamı da kullanır. Ortam kopyalama hakkında daha fazla bilgi için [Environment. Clone ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true#clone-new-name-) başvurusuna bakın.

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.webservice import LocalWebservice
from azureml.core import Environment
from azureml.core.model import InferenceConfig


local_service_name = "triton-bidaf-onnx"
env = Environment.get(ws, "AzureML-Triton").clone("My-Triton")

for pip_package in ["nltk"]:
    env.python.conda_dependencies.add_pip_package(pip_package)

inference_config = InferenceConfig(
    entry_script="score_bidaf.py",  # This entry script is where we dispatch a call to the Triton server
    source_directory=os.path.join("..", "scripts"),
    environment=env
)

local_config = LocalWebservice.deploy_configuration(
    port=6789
)

local_service = Model.deploy(
    workspace=ws,
    name=local_service_name,
    models=[model],
    inference_config=inference_config,
    deployment_config=local_config,
    overwrite=True)

local_service.wait_for_deployment(show_output = True)
print(local_service.state)
# Print the URI you can use to call the local deployment
print(local_service.scoring_uri)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!TIP]
> Çıkarım yapılandırması oluşturma hakkında daha fazla bilgi için bkz. [çıkarım yapılandırma şeması](./reference-azure-machine-learning-cli.md#inference-configuration-schema).

```azurecli
az ml model deploy -n triton-densenet-onnx \
-m densenet_onnx:1 \
--ic inference-config.json \
-e My-Triton --dc deploymentconfig.json \
--overwrite --compute-target=aks-gpu
```

---

Dağıtım tamamlandıktan sonra Puanlama URI 'SI görüntülenir. Bu yerel dağıtım için, olacaktır `http://localhost:6789/score` . Buluta dağıtırsanız, Puanlama URI 'sini almak için [az ml Service CLI göster](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext_azure_cli_ml_az_ml_service_show) komutunu kullanabilirsiniz.

Puanlama URI 'sine çıkarım istekleri gönderen bir istemci oluşturma hakkında daha fazla bilgi için bkz. [Web hizmeti olarak dağıtılan bir modeli](how-to-consume-web-service.md)kullanma.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure Machine Learning çalışma alanını kullanmaya devam etmek, ancak dağıtılan hizmetten kurtulabilmesi istiyorsanız, aşağıdaki seçeneklerden birini kullanın:

# <a name="python"></a>[Python](#tab/python)

```python
local_service.delete()
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az ml service delete -n triton-densenet-onnx
```

---

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning üç aylık dönemin uçtan uca örneklerine bakın](https://aka.ms/aml-triton-sample)
* [Triton istemci örneklerine](https://github.com/triton-inference-server/server/tree/master/src/clients/python/examples) göz atın
* [Üç aylık çıkarım sunucusu belgelerini](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/index.html) okuyun
* [Başarısız bir dağıtımda sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetini güncelleştirme](how-to-deploy-update-web-service.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
