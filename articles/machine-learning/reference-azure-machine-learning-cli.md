---
title: CLı uzantısı
titleSuffix: Azure Machine Learning
description: Azure Machine Learning CLI uzantı hakkında Azure CLI için öğrenin. Azure CLI'yı Azure buluttaki kaynaklar ile çalışmanıza olanak sağlayan bir platformlar arası komut satırı yardımcı programıdır. Machine Learning uzantısı, Azure Machine Learning çalışmanıza olanak sağlar. ML CLı, çalışma alanınız, veri depoları, veri kümeleri, işlem hatları, modeller ve dağıtımlarınız gibi kaynakları oluşturur ve yönetir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 11/05/2019
ms.custom: seodec18
ms.openlocfilehash: 5a2acb3a6bd7401de2a5b934f539adbb63c3f129
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75930391"
---
# <a name="use-the-cli-extension-for-azure-machine-learning"></a>Azure Machine Learning için CLı uzantısını kullanın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning CLI uzantısıdır [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), Azure platformu için platformlar arası komut satırı arabirimi. Bu uzantı Azure Machine Learning ile çalışmaya yönelik komutlar sağlar. Machine Learning etkinliklerinizi otomatikleştirmenize olanak tanır. Aşağıdaki listede CLı uzantısıyla yapabileceğiniz bazı örnek eylemler verilmiştir:

+ Makine öğrenimi modelleri oluşturma çalıştırın

+ Makine öğrenimi modelleri müşteri kullanım için kaydolun

+ Paketlemeyi, dağıtmayı ve makine öğrenimi Modellerinizi ömrünü izleme

CLI, Azure Machine Learning SDK'sı yerine değil. Otomasyonuna uygun olan yüksek parametreli görevleri işlemek için optimize edilmiş, tamamlayıcı bir araçtır.

## <a name="prerequisites"></a>Ön koşullar

* CLI kullanmak için Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="full-reference-docs"></a>Tam başvuru belgeleri

[Azure CLI 'nın Azure-CLI-ml uzantısı için tüm başvuru belgelerini](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/?view=azure-cli-latest)bulun.

## <a name="install-the-extension"></a>Uzantıyı yükleme

Machine Learning CLI uzantısını yüklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Aşağıdaki komutlarla kullanabileceğiniz örnek dosyaları [burada](https://aka.ms/azml-deploy-cloud)bulabilirsiniz.

Sorulduğunda, `y` uzantıyı yüklemek için.

Uzantı yüklü olduğunu doğrulamak için ML özgü alt komutları listesini görüntülemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml -h
```

## <a name="update-the-extension"></a>Uzantıyı güncelleştirme

Machine Learning CLı uzantısını güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension update -n azure-cli-ml
```


## <a name="remove-the-extension"></a>Uzantıyı kaldırma

CLI uzantısını kaldırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Kaynak yönetimi

Aşağıdaki komutları, Azure Machine Learning tarafından kullanılan kaynakları yönetmek için CLI'yı kullanmayı göstermektedir.

+ Henüz bir tane yoksa, bir kaynak grubu oluşturun:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Azure Machine Learning çalışma alanı oluşturun:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    > [!TIP]
    > Bu komut, temel bir sürüm çalışma alanı oluşturur. Kurumsal çalışma alanı oluşturmak için, `az ml workspace create` komutuyla `--sku enterprise` anahtarını kullanın. Azure Machine Learning sürümleri hakkında daha fazla bilgi için bkz. [Azure Machine Learning nedir](overview-what-is-azure-ml.md#sku).

    Daha fazla bilgi için bkz. [az ml çalışma alanı oluştur](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create).

+ CLı bağlamsal tanımayı etkinleştirmek için bir klasöre çalışma alanı yapılandırması ekleyin.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Bu komut, örnek runconfig ve Conda ortam dosyalarını içeren bir `.azureml` alt dizini oluşturur. Ayrıca, Azure Machine Learning çalışma alanınız ile iletişim kurmak için kullanılan bir `config.json` dosyası da içerir.

    Daha fazla bilgi için bkz. [az ml Folder Attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

+ Bir Azure Blob kapsayıcısını veri deposu olarak ekleyin.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Daha fazla bilgi için bkz. [az ml DataStore Attach-blob](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-attach-blob).

+ Dosyaları bir veri deposuna yükleyin.

    ```azurecli-interactive
    az ml datastore upload  -n datastorename -p sourcepath
    ```

    Daha fazla bilgi için bkz. [az ml DataStore upload](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-upload).

+ Bir AKS kümesini Işlem hedefi olarak ekleyin.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Daha fazla bilgi için bkz. [az ml computetarget Attach aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks)

+ Yeni bir AMLcompute hedefi oluşturun.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```

    Daha fazla bilgi için bkz. [az ml computetarget Create amlcompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute).

## <a id="experiments"></a>Denemeleri Çalıştır

* Çalıştırma denemenizi başlatın. Bu komutu kullanırken,-c parametresine karşı runconfig dosyasının adını (dosya sisteminize bakıyorsanız \*. runconfig öğesinden önce metin) belirtin.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > `az ml folder attach` komutu, iki örnek runconfig dosyası içeren bir `.azureml` alt dizini oluşturur. 
    >
    > Programlı olarak çalıştırılan bir yapılandırma nesnesi oluşturan bir Python betiğinizin olması halinde, [runconfig. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) komutunu bir runconfig dosyası olarak kaydetmek için kullanabilirsiniz.
    >
    > Tam runconfig şeması bu [json dosyasında](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json)bulunabilir.

    Daha fazla bilgi için bkz. [az ml Run gönderme-betiği](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

* Denemeleri listesini görüntüleyin:

    ```azurecli-interactive
    az ml experiment list
    ```

    Daha fazla bilgi için bkz. [az ml deneme listesi](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

## <a name="dataset-management"></a>Veri kümesi Yönetimi

Aşağıdaki komutlarda Azure Machine Learning içindeki veri kümeleriyle nasıl çalışılacağı gösterilmektedir:

+ Veri kümesini kaydetme:

    ```azurecli-interactive
    az ml dataset register -f mydataset.json
    ```

    Veri kümesini tanımlamak için kullanılan JSON dosyasının biçimi hakkında daha fazla bilgi için `az ml dataset --show-template`kullanın.

    Daha fazla bilgi için bkz. [az ml DataSet Register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Etkin veya kullanım dışı bırakılmış bir veri kümesini arşivle:

    ```azurecli-interactive
    az ml dataset archive -n dataset-name
    ```

    Daha fazla bilgi için bkz. [az ml DataSet Archive](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Bir veri kümesini kullanımdan kaldırma:

    ```azurecli-interactive
    az ml dataset deprecate -d replacement-dataset-id -n dataset-to-deprecate
    ```

    Daha fazla bilgi için bkz. [az ml veri kümesi kullanımdan](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive)kaldırıldı.

+ Çalışma alanındaki tüm veri kümelerini listeleyin:

    ```azurecli-interactive
    az ml dataset list
    ```

    Daha fazla bilgi için bkz. [az ml DataSet List](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Bir veri kümesinin ayrıntılarını alın:

    ```azurecli-interactive
    az ml dataset show -n dataset-name
    ```

    Daha fazla bilgi için bkz. [az ml DataSet Show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Arşivlenmiş veya kullanım dışı bırakılmış bir veri kümesini yeniden etkinleştirme:

    ```azurecli-interactive
    az ml dataset reactivate -n dataset-name
    ```

    Daha fazla bilgi için bkz. [az ml DataSet yeniden etkinleştirme](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Veri kümesinin kaydını sil:

    ```azurecli-interactive
    az ml dataset unregister -n dataset-name
    ```

    Daha fazla bilgi için bkz. [az ml DataSet Unregister](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

## <a name="environment-management"></a>Ortam yönetimi

Aşağıdaki komutlarda, çalışma alanınız için Azure Machine Learning [ortamlarının](how-to-configure-environment.md) nasıl oluşturulacağı, kaydedileceği ve listenin nasıl yapılacağı gösterilmektedir:

+ Bir ortam için bir yapı iskelesi dosyaları oluşturun:

    ```azurecli-interactive
    az ml environment scaffold -n myenv -d myenvdirectory
    ```

    Daha fazla bilgi için bkz. [az ml Environment iskele](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-scaffold).

+ Bir ortamı kaydedin:

    ```azurecli-interactive
    az ml environment register -d myenvdirectory
    ```

    Daha fazla bilgi için bkz. [az ml Environment Register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-register).

+ Kayıtlı ortamları listeleyin:

    ```azurecli-interactive
    az ml environment list
    ```

    Daha fazla bilgi için bkz. [az ml Environment List](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-list).

+ Kayıtlı bir ortamı indirin:

    ```azurecli-interactive
    az ml environment download -n myenv -d downloaddirectory
    ```

    Daha fazla bilgi için bkz. [az ml Environment Download](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-download).

### <a name="environment-configuration-schema"></a>Ortam yapılandırma şeması

`az ml environment scaffold` komutunu kullandıysanız, CLı ile özel ortam yapılandırması oluşturmak için değiştirilebilen ve kullanılabilecek bir şablon `azureml_environment.json` dosyası oluşturur. En üst düzey nesne, Python SDK 'daki [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) sınıfıyla gevşek olarak eşlenir. 

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

Aşağıdaki tabloda JSON dosyasındaki her üst düzey alan, türü ve bir açıklama ayrıntıları verilmiştir. Bir nesne türü Python SDK 'dan bir sınıfa bağlanmışsa, Python sınıfında her bir JSON alanı ve genel değişken adı arasında gevşek bir 1:1 eşleşmesi vardır. Bazı durumlarda alan, sınıf değişkeni yerine bir Oluşturucu bağımsız değişkenine de eşlenir. Örneğin, `environmentVariables` alanı [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) sınıfındaki `environment_variables` değişkenine eşlenir.

| JSON alanı | Tür | Açıklama |
|---|---|---|
| `name` | `string` | Ortamın adı. **Microsoft** veya **AzureML**ile ad başlatma. |
| `version` | `string` | Ortamın sürümü. |
| `environmentVariables` | `{string: string}` | Ortam değişkeni adlarının ve değerlerinin karma eşlemesi. |
| `python` | [`PythonSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py) | Hedef işlem kaynağında kullanılacak Python ortamını ve yorumlayıcı 'yı tanımlayan nesne. |
| `docker` | [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) | Ortamın belirtimlerine göre oluşturulan Docker görüntüsünü özelleştirmek için ayarları tanımlar. |
| `spark` | [`SparkSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.sparksection?view=azure-ml-py) | Bölüm Spark ayarlarını yapılandırır. Yalnızca Framework, PySpark olarak ayarlandığında kullanılır. |
| `databricks` | [`DatabricksSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.databricks.databrickssection?view=azure-ml-py) | Databricks kitaplığı bağımlılıklarını yapılandırır. |
| `inferencingStackVersion` | `string` | Görüntüye eklenen ınırm yığın sürümünü belirtir. İkinci dereceden bir yığın eklemekten kaçınmak için, bu alanı `null`bırakın. Geçerli değer: "en son". |

## <a name="ml-pipeline-management"></a>ML işlem hattı yönetimi

Aşağıdaki komutlarda makine öğrenimi ardışık düzenleri ile nasıl çalışılacağı gösterilmektedir:

+ Machine Learning işlem hattı oluşturma:

    ```azurecli-interactive
    az ml pipeline create -n mypipeline -y mypipeline.yml
    ```

    Daha fazla bilgi için bkz. [az ml işlem hattı oluşturma](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create).

    İşlem hattı YAML dosyası hakkında daha fazla bilgi için bkz. [YAML 'de makine öğrenimi işlem hatlarını tanımlama](reference-pipeline-yaml.md).

+ İşlem hattı çalıştırma:

    ```azurecli-interactive
    az ml run submit-pipeline -n myexperiment -y mypipeline.yml
    ```

    Daha fazla bilgi için bkz. [az ml Run gönderme-işlem hattı](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-pipeline).

    İşlem hattı YAML dosyası hakkında daha fazla bilgi için bkz. [YAML 'de makine öğrenimi işlem hatlarını tanımlama](reference-pipeline-yaml.md).

+ İşlem hattı zamanlama:

    ```azurecli-interactive
    az ml pipeline create-schedule -n myschedule -e myexpereiment -i mypipelineid -y myschedule.yml
    ```

    Daha fazla bilgi için bkz. [az ml işlem hattı oluşturma-zamanlama](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create-schedule).

    İşlem hattı zamanlaması YAML dosyası hakkında daha fazla bilgi için bkz. [YAML 'de makine öğrenimi işlem hatlarını tanımlama](reference-pipeline-yaml.md#schedules).

## <a name="model-registration-profiling-deployment"></a>Model kaydı, profil oluşturma, dağıtım

Aşağıdaki komutlar, eğitilen bir modeli kaydedin ve ardından bunu bir üretim hizmeti dağıtacağız göstermektedir:

+ Bir modeli Azure Machine Learning ile kaydedin:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Daha fazla bilgi için bkz. [az ml model Register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register).

+ **Isteğe bağlı** Dağıtım için en iyi CPU ve bellek değerlerini almak üzere modelinizin profilini yapın.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Daha fazla bilgi için bkz. [az ml model profili](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

+ Modelinizi AKS 'e dağıtın
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Çıkarım yapılandırma dosyası şeması hakkında daha fazla bilgi için bkz. [çıkarım yapılandırma şeması](#inferenceconfig).
    
    Dağıtım yapılandırma dosya şeması hakkında daha fazla bilgi için bkz. [dağıtım yapılandırma şeması](#deploymentconfig).

    Daha fazla bilgi için bkz. [az ml model dağıtımı](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).

<a id="inferenceconfig"></a>

## <a name="inference-configuration-schema"></a>Çıkarım yapılandırma şeması

[!INCLUDE [inferenceconfig](../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Dağıtım yapılandırma şeması

### <a name="local-deployment-configuration-schema"></a>Yerel dağıtım yapılandırma şeması

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Azure Container Instance dağıtım yapılandırma şeması 

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Azure Kubernetes hizmet dağıtımı yapılandırma şeması

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [MACHINE LEARNING CLI uzantısı Için komut başvurusu](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest).

* [Azure Pipelines kullanarak makine öğrenimi modellerini eğitme ve dağıtma](/azure/devops/pipelines/targets/azure-machine-learning)
