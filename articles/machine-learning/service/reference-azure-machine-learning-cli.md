---
title: Machine Learning CLı uzantısı
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning CLI uzantı hakkında Azure CLI için öğrenin. Azure CLI'yı Azure buluttaki kaynaklar ile çalışmanıza olanak sağlayan bir platformlar arası komut satırı yardımcı programıdır. Machine Learning uzantısı, Azure Machine Learning hizmeti ile çalışmanıza olanak sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 08/20/2019
ms.custom: seodec18
ms.openlocfilehash: 6c5068512f8ba26f7710bca7c0fccb98e0a5be33
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69996740"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için CLI uzantısını kullanma

Azure Machine Learning CLI uzantısıdır [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), Azure platformu için platformlar arası komut satırı arabirimi. Bu uzantı Azure Machine Learning hizmeti ile çalışmaya yönelik komutlar sağlar. Machine Learning etkinliklerinizi otomatikleştirmenize olanak tanır. Aşağıdaki listede CLı uzantısıyla yapabileceğiniz bazı örnek eylemler verilmiştir:

+ Makine öğrenimi modelleri oluşturma çalıştırın

+ Makine öğrenimi modelleri müşteri kullanım için kaydolun

+ Paketlemeyi, dağıtmayı ve makine öğrenimi Modellerinizi ömrünü izleme

CLI, Azure Machine Learning SDK'sı yerine değil. Otomasyonuna uygun olan yüksek parametreli görevleri işlemek için optimize edilmiş, tamamlayıcı bir araçtır.

## <a name="prerequisites"></a>Önkoşullar

* CLI kullanmak için Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

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


## <a name="remove-the-extension"></a>Uzantıyı kaldırın

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

+ Bir Azure Machine Learning hizmeti çalışma alanında oluşturun:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    Daha fazla bilgi için bkz. [az ml çalışma alanı oluştur](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create).

+ CLı bağlamsal tanımayı etkinleştirmek için bir klasöre çalışma alanı yapılandırması ekleyin.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Bu komut, örnek `.azureml` runconfig ve Conda ortam dosyalarını içeren bir alt dizin oluşturur. Ayrıca, Azure Machine Learning çalışma `config.json` alanınız ile iletişim kurmak için kullanılan bir dosya içerir.

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

* Çalıştırma denemenizi başlatın. Bu komutu kullanırken,-c parametresine karşı runconfig dosyasının adını (dosya sisteminize bakıyorsanız \*. runconfig öğesinden önceki metin) belirtin.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > Komut, iki örnek `.azureml` runconfig dosyası içeren bir alt dizin oluşturur. `az ml folder attach` 
    >
    > Programlı olarak çalıştırılan bir yapılandırma nesnesi oluşturan bir Python betiğinizin olması halinde, runconfig [. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) komutunu bir runconfig dosyası olarak kaydetmek için kullanabilirsiniz.
    >
    > Daha fazla örnek runconfig dosyası için bkz [https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Daha fazla bilgi için bkz. [az ml Run gönderme-betiği](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

* Denemeleri listesini görüntüleyin:

    ```azurecli-interactive
    az ml experiment list
    ```

    Daha fazla bilgi için bkz. [az ml deneme listesi](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

## <a name="environment-management"></a>Ortam yönetimi

Aşağıdaki komutlarda, çalışma alanınız için Azure Machine Learning hizmet [ortamlarının](how-to-configure-environment.md) nasıl oluşturulacağı, kaydedileceği ve listelebileceği gösterilmektedir:

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

[!INCLUDE [inferenceconfig](../../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Dağıtım yapılandırma şeması

### <a name="local-deployment-configuration-schema"></a>Yerel dağıtım yapılandırma şeması

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Azure Container Instance dağıtım yapılandırma şeması 

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Azure Kubernetes hizmet dağıtımı yapılandırma şeması

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [MACHINE LEARNING CLI uzantısı Için komut başvurusu](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest).

* [Azure Pipelines kullanarak makine öğrenimi modellerini eğitme ve dağıtma](/azure/devops/pipelines/targets/azure-machine-learning)
