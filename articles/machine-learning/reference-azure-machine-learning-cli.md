---
title: CLI uzantısı
titleSuffix: Azure Machine Learning
description: Azure CLI için Azure Machine Learning CLI uzantısı hakkında bilgi edinin. Azure CLI, Azure bulutundaki kaynaklarla çalışmanızı sağlayan bir çapraz platform komut satırı yardımcı programıdır. Machine Learning uzantısı Azure Machine Learning ile çalışmanızı sağlar. ML CLI, çalışma alanınız, veri depolarınız, veri kümeleriniz, ardışık hatlar, modeller ve dağıtımlarınız gibi kaynakları oluşturur ve yönetir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 471b26ebc4bd4aecb814ec43c7eba56e3d764fa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402483"
---
# <a name="use-the-cli-extension-for-azure-machine-learning"></a>Azure Machine Learning için CLI uzantısını kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning CLI, Azure platformu için bir çapraz platform komut satırı arabirimi olan [Azure CLI'nin](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)bir uzantısıdır. Bu uzantı, Azure Machine Learning ile çalışmak için komutlar sağlar. Makine öğrenimi etkinliklerinizi otomatikleştirmenizi sağlar. Aşağıdaki liste, CLI uzantısı ile yapabileceğiniz bazı örnek eylemler sağlar:

+ Makine öğrenimi modelleri oluşturmak için denemeler çalıştırın

+ Müşteri kullanımı için makine öğrenimi modellerini kaydedin

+ Makine öğrenimi modellerinizin kullanım ömrünü paketleyip dağıtın ve izleyin

CLI, Azure Machine Learning SDK'nın yerini almıyor. Otomasyona uygun yüksek parametreli görevleri işlemek için optimize edilmiş tamamlayıcı bir araçtır.

## <a name="prerequisites"></a>Ön koşullar

* CLI'yi kullanmak için bir Azure aboneliğiniz olması gerekir. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="full-reference-docs"></a>Tam başvuru dokümanları

Azure [CLI'nin azure-cli-ml uzantısı için tam başvuru dokümanlarını](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/?view=azure-cli-latest)bulun.

## <a name="install-the-extension"></a>Uzantıyı yükleme

Machine Learning CLI uzantısını yüklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Aşağıdaki komutlarla kullanabileceğiniz örnek dosyaları [burada](https://aka.ms/azml-deploy-cloud)bulabilirsiniz.

İstendiğinde, `y` uzantıyı yüklemeyi seçin.

Uzantının yüklü olduğunu doğrulamak için ML'ye özgü alt komutların listesini görüntülemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml -h
```

## <a name="update-the-extension"></a>Uzantıyı güncelleştir

Machine Learning CLI uzantısını güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension update -n azure-cli-ml
```


## <a name="remove-the-extension"></a>Uzantıyı kaldırma

CLI uzantısını kaldırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Kaynak yönetimi

Aşağıdaki komutlar, Azure Machine Learning tarafından kullanılan kaynakları yönetmek için CLI'nin nasıl kullanılacağını gösterir.

+ Zaten bir kaynağınız yoksa, bir kaynak grubu oluşturun:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Azure Makine Öğrenimi çalışma alanı oluşturun:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    > [!TIP]
    > Bu komut, temel bir sürüm çalışma alanı oluşturur. Kurumsal çalışma alanı oluşturmak için `--sku enterprise` `az ml workspace create` komutla anahtarı kullanın. Azure Machine Learning sürümleri hakkında daha fazla bilgi için Azure [Machine Learning nedir'e](overview-what-is-azure-ml.md#sku)bakın.

    Daha fazla bilgi için az [ml çalışma alanı oluşturma bilgisine](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create)bakın.

+ CLI bağlamsal farkındalığını etkinleştirmek için bir klasöre çalışma alanı yapılandırması takın.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Bu komut, `.azureml` örnek runconfig ve conda ortamı dosyaları içeren bir alt dizin oluşturur. Ayrıca, Azure `config.json` Machine Learning çalışma alanınızla iletişim kurmak için kullanılan bir dosya da içerir.

    Daha fazla bilgi için [az ml klasöre iliştirin.](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach)

+ Veri deposu olarak Azure blob kapsayıcısı takın.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Daha fazla bilgi için az [ml datastore attach-blob'a](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-attach-blob)bakın.

+ Bir Datastore'a dosya yükleyin.

    ```azurecli-interactive
    az ml datastore upload  -n datastorename -p sourcepath
    ```

    Daha fazla bilgi için [az ml datastore upload'a](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-upload)bakın.

+ Bir AKS kümesini İşlem Hedefi olarak takın.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Daha fazla bilgi için bkz: [az ml computetarget eklemek aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks)

+ Yeni bir AMLcompute hedefi oluşturun.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```

    Daha fazla bilgi için, [az ml computetarget amlcompute oluşturmak](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute)bakın.

## <a name="run-experiments"></a><a id="experiments"></a>Denemeleri çalıştırma

* Denemenizin bir çalışmasını başlatın. Bu komutu kullanırken, -c parametresi karşı \*runconfig dosyasının adını (dosya sisteminize bakıyorsanız .runconfig'den önceki metin) belirtin.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > Komut, `az ml folder attach` iki `.azureml` örnek runconfig dosyaları içeren bir alt dizin oluşturur. 
    >
    > Programlı bir çalıştır yapılandırma nesnesi oluşturan bir Python komut dosyası varsa, [runconfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) kullanarak runconfig dosyası olarak kaydedebilirsiniz.
    >
    > Tam runconfig şema bu [JSON dosyasında](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json)bulunabilir. Şema, her nesnenin `description` anahtarı yla kendini belgelemektir. Ayrıca, olası değerler için enums ve sonunda bir şablon snippet vardır.

    Daha fazla bilgi için az [ml run submit-script'e](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script)bakın.

* Denemelerin listesini görüntüleyin:

    ```azurecli-interactive
    az ml experiment list
    ```

    Daha fazla bilgi için [az ml deney listesine](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list)bakın.

## <a name="dataset-management"></a>Dataset yönetimi

Aşağıdaki komutlar Azure Machine Learning'de veri kümeleriyle nasıl çalışılalı yapılacağını gösterir:

+ Bir veri kümesi kaydedin:

    ```azurecli-interactive
    az ml dataset register -f mydataset.json
    ```

    Veri kümesini tanımlamak için kullanılan JSON dosyasının biçimi `az ml dataset register --show-template`hakkında bilgi için .

    Daha fazla bilgi için [az ml dataset register'a](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)bakın.

+ Etkin veya amortismana alınan bir veri kümesini arşivleyin:

    ```azurecli-interactive
    az ml dataset archive -n dataset-name
    ```

    Daha fazla bilgi için [az ml dataset arşivine](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)bakın.

+ Bir veri kümesini amortismana verin:

    ```azurecli-interactive
    az ml dataset deprecate -d replacement-dataset-id -n dataset-to-deprecate
    ```

    Daha fazla bilgi için [az ml dataset amortismanbölümüne](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)bakın.

+ Çalışma alanındaki tüm veri kümelerini listele:

    ```azurecli-interactive
    az ml dataset list
    ```

    Daha fazla bilgi için [az ml dataset listesine](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)bakın.

+ Bir veri kümesinin ayrıntılarını alın:

    ```azurecli-interactive
    az ml dataset show -n dataset-name
    ```

    Daha fazla bilgi için [az ml dataset show'a](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)bakın.

+ Arşivlenmiş veya amortismana nisize edilmiş bir veri kümesini yeniden etkinleştirin:

    ```azurecli-interactive
    az ml dataset reactivate -n dataset-name
    ```

    Daha fazla bilgi için [az ml dataset reactivate'e](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)bakın.

+ Bir veri kümesini kayıt dışı bırak:

    ```azurecli-interactive
    az ml dataset unregister -n dataset-name
    ```

    Daha fazla bilgi için [az ml dataset unregister](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)bakın.

## <a name="environment-management"></a>Çevre yönetimi

Aşağıdaki komutlar, çalışma alanınız için Azure Machine Learning [ortamlarını](how-to-configure-environment.md) nasıl oluşturup kaydedilen ve listeleneniz gerektiğini gösterir:

+ Bir ortam için iskele dosyaları oluşturma:

    ```azurecli-interactive
    az ml environment scaffold -n myenv -d myenvdirectory
    ```

    Daha fazla bilgi için az [ml çevre iskelesi.](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-scaffold)

+ Bir ortam kaydedin:

    ```azurecli-interactive
    az ml environment register -d myenvdirectory
    ```

    Daha fazla bilgi için [az ml çevre kaydına](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-register)bakın.

+ Kayıtlı ortamları listele:

    ```azurecli-interactive
    az ml environment list
    ```

    Daha fazla bilgi için [az ml çevre listesine](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-list)bakın.

+ Kayıtlı bir ortamı karşıdan yükleyin:

    ```azurecli-interactive
    az ml environment download -n myenv -d downloaddirectory
    ```

    Daha fazla bilgi için az [ml ortamı indir.](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-download)

### <a name="environment-configuration-schema"></a>Çevre yapılandırma şeması

Komutu `az ml environment scaffold` kullandıysanız, cli ile `azureml_environment.json` özel ortam yapılandırmaları oluşturmak için değiştirilebilir ve kullanılan bir şablon dosyası oluşturur. Üst düzey nesne Python SDK'daki sınıfla [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) gevşek bir şekilde eşler. 

```json
{
    "name": "testenv",
    "version": null,
    "environmentVariables": {
        "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
    },
    "python": {
        "userManagedDependencies": false,
        "interpreterPath": "python",
        "condaDependenciesFile": null,
        "baseCondaEnvironment": null
    },
    "docker": {
        "enabled": false,
        "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
        "baseDockerfile": null,
        "sharedVolumes": true,
        "shmSize": "2g",
        "arguments": [],
        "baseImageRegistry": {
            "address": null,
            "username": null,
            "password": null
        }
    },
    "spark": {
        "repositories": [],
        "packages": [],
        "precachePackages": true
    },
    "databricks": {
        "mavenLibraries": [],
        "pypiLibraries": [],
        "rcranLibraries": [],
        "jarLibraries": [],
        "eggLibraries": []
    },
    "inferencingStackVersion": null
}
```

Aşağıdaki tablo, JSON dosyasındaki her üst düzey alanı, türü ve açıklaması ayrıntılarıyla ayrıntılarıyla anlatır. Bir nesne türü Python SDK'dan bir sınıfa bağlıysa, Python sınıfındaki her JSON alanı ile ortak değişken adı arasında 1:1'lik gevşek bir eşleşme vardır. Bazı durumlarda alan, sınıf değişkeni yerine bir oluşturucu bağımsız değişkenle eşlenebilir. Örneğin, `environmentVariables` alan `environment_variables` [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) sınıftaki değişkenle eşler.

| JSON alanı | Tür | Açıklama |
|---|---|---|
| `name` | `string` | Çevrenin adı. **Microsoft** veya **AzureML**ile ad başlatmayın. |
| `version` | `string` | Ortamın sürümü. |
| `environmentVariables` | `{string: string}` | Çevre değişken adları ve değerlerinin karma haritası. |
| `python` | [`PythonSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py) | Python ortamını tanımlayan nesne ve hedef bilgi işlem kaynağında kullanılacak yorumlayıcı. |
| `docker` | [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) | Ortam özelliklerine göre oluşturulmuş Docker görüntüsünü özelleştirmek için ayarları tanımlar. |
| `spark` | [`SparkSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.sparksection?view=azure-ml-py) | Bölüm, Kıvılcım ayarlarını yapılandırır. Yalnızca çerçeve PySpark olarak ayarlandığında kullanılır. |
| `databricks` | [`DatabricksSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.databricks.databrickssection?view=azure-ml-py) | Databricks kitaplık bağımlılıklarını yapılandırır. |
| `inferencingStackVersion` | `string` | Görüntüye eklenen çıkarlayıcı yığın sürümünü belirtir. Çıkarverici bir yığın eklemekten kaçınmak için `null`bu alanı bırakın. Geçerli değer: "en son". |

## <a name="ml-pipeline-management"></a>ML boru hattı yönetimi

Aşağıdaki komutlar, makine öğrenimi boru hatlarıyla nasıl çalışılabildiğini gösterir:

+ Makine öğrenimi ardışık hattı oluşturun:

    ```azurecli-interactive
    az ml pipeline create -n mypipeline -y mypipeline.yml
    ```

    Daha fazla bilgi için az [ml boru hattı oluşturmak](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create)bakın.

    Boru hattı YAML dosyası hakkında daha fazla bilgi için [bkz.](reference-pipeline-yaml.md)

+ Bir ardışık hat lar çalıştır:

    ```azurecli-interactive
    az ml run submit-pipeline -n myexperiment -y mypipeline.yml
    ```

    Daha fazla bilgi için az [ml run submit-pipeline'a](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-pipeline)bakın.

    Boru hattı YAML dosyası hakkında daha fazla bilgi için [bkz.](reference-pipeline-yaml.md)

+ Bir ardışık kaynak programı:

    ```azurecli-interactive
    az ml pipeline create-schedule -n myschedule -e myexpereiment -i mypipelineid -y myschedule.yml
    ```

    Daha fazla bilgi için az [ml boru hattı oluşturma-zamanlamaya](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create-schedule)bakın.

    Boru hattı zamanlaması YAML dosyası hakkında daha fazla bilgi için [bkz.](reference-pipeline-yaml.md#schedules)

## <a name="model-registration-profiling-deployment"></a>Model kaydı, profil oluşturma, dağıtım

Aşağıdaki komutlar, eğitilmiş bir modelin nasıl kaydedilen ve sonra bir üretim hizmeti olarak nasıl dağıtılanın gösterilmektedir:

+ Azure Machine Learning ile bir model kaydedin:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Daha fazla bilgi için [az ml model kaydına](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register)bakın.

+ **İsteğE Bağlı** Dağıtım için en iyi CPU ve bellek değerlerini elde etmek için modelinizin profilini çıkar.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Daha fazla bilgi için [az ml model profiline](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile)bakın.

+ Modelinizi AKS'ye dağıtın
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Çıkarım yapılandırma dosyası şeması hakkında daha fazla bilgi için [çıkarım yapılandırma şemasına](#inferenceconfig)bakın.
    
    Dağıtım yapılandırma dosyası şeması hakkında daha fazla bilgi için [dağıtım yapılandırma şemasına](#deploymentconfig)bakın.

    Daha fazla bilgi için az [ml modeli nin dağıtımına](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy)bakın.

<a id="inferenceconfig"></a>

## <a name="inference-configuration-schema"></a>Çıkarım yapılandırma şeması

[!INCLUDE [inferenceconfig](../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Dağıtım yapılandırma şeması

### <a name="local-deployment-configuration-schema"></a>Yerel dağıtım yapılandırma şeması

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Azure Kapsayıcı Örneği dağıtım yapılandırma şeması 

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Azure Kubernetes Hizmet dağıtım yapılandırma şeması

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Machine Learning CLI uzantısı için komut başvurusu.](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest)

* [Azure Boru Hatları'nı kullanarak makine öğrenimi modellerini eğitin ve dağıtın](/azure/devops/pipelines/targets/azure-machine-learning)
