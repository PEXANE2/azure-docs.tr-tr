---
title: CLı 'dan modelleri eğitme ve dağıtma
titleSuffix: Azure Machine Learning
description: Komut satırından bir modeli eğitmek, kaydettirmek ve dağıtmak için Azure CLı için Machine Learning uzantısını nasıl kullanacağınızı öğrenin.
ms.author: larryfr
author: Blackmist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: eb181cbf6c647c816886f330502a9a46cb956dee
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75763292"
---
# <a name="tutorial-train-and-deploy-a-model-from-the-cli"></a>Öğretici: CLı 'dan model eğitme ve dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğreticide, bir modeli eğitmek, kaydettirmek ve dağıtmak için Azure CLı için Machine Learning uzantısını kullanırsınız.

Bu öğreticideki Python eğitim betikleri [scikit-](https://scikit-learn.org/) temel bir modeli eğitme hakkında bilgi edinin. Bu öğreticinin odağı betiklerin veya modeldeki değildir, ancak Azure Machine Learning çalışmak için CLı kullanma işlemidir.

Aşağıdaki eylemleri nasıl gerçekleştireceğinizi öğrenin:

> [!div class="checklist"]
> * Machine Learning uzantısını yükler
> * Bir Azure Machine Learning çalışma alanı oluşturma
> * Modeli eğitmek için kullanılan işlem kaynağını oluşturma
> * Modeli eğitmek için kullanılan veri kümesini tanımlama ve kaydetme
> * Eğitim çalıştırması başlatma
> * Bir modeli kaydetme ve indirme
> * Modeli bir Web hizmeti olarak dağıtma
> * Web hizmetini kullanarak veri puanı

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* **Yerel ortamınızdan**bu belgedeki CLI komutlarını kullanmak IÇIN [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)gerekir.

    [Azure Cloud Shell](https://azure.microsoft.com//features/cloud-shell/)KULLANıYORSANıZ, CLI tarayıcı aracılığıyla erişilir ve bulutta bulunur.

## <a name="download-the-example-project"></a>Örnek projeyi indirin

Bu öğretici için [https://github.com/microsoft/MLOps](https://github.com/microsoft/MLOps) projesini indirin. `examples/cli-train-deploy` dizinindeki dosyalar bu öğreticideki adımlar tarafından kullanılır.

Dosyaların yerel bir kopyasını almak için, [bir. zip arşivi indirin](https://github.com/microsoft/MLOps/archive/master.zip)ya da depoyu kopyalamak Için aşağıdaki git komutunu kullanın:

```azurecli-interactive
git clone https://github.com/microsoft/MLOps.git
```

### <a name="training-files"></a>Eğitim dosyaları

Projedeki `examples/cli-train-deploy` Dizin, bir modeli eğitmek için kullanılan aşağıdaki dosyaları içerir:

* `.azureml\mnist.runconfig`: bir __Çalıştırma yapılandırma__ dosyası. Bu dosya, modeli eğitmek için gereken çalışma zamanı ortamını tanımlar. Bu örnekte, modeli eğitim ortamına eğitmek için kullanılan verileri de takar.
* `scripts\train.py`: eğitim betiği. Bu dosya, modeli tratlar.
* `scripts\utils.py`: eğitim betiği tarafından kullanılan bir yardımcı dosyası.
* `.azureml\conda_dependencies.yml`: eğitim betiğini çalıştırmak için gereken yazılım bağımlılıklarını tanımlar.
* `dataset.json`: veri kümesi tanımı. Azure Machine Learning çalışma alanına MNIST veri kümesini kaydetmek için kullanılır.

### <a name="deployment-files"></a>Dağıtım dosyaları

Depo, eğitilen modeli bir Web hizmeti olarak dağıtmak için kullanılan aşağıdaki dosyaları içerir:

* `aciDeploymentConfig.yml`: bir __dağıtım yapılandırma__ dosyası. Bu dosya, model için gereken barındırma ortamını tanımlar.
* `inferenceConfig.yml`: bir çıkarım configuration__ dosyası. Bu dosya, hizmet tarafından modelle verileri Puanlama için kullanılan yazılım ortamını tanımlar.
* `score.py`: gelen verileri kabul eden bir Python betiği, modeli kullanarak bir yanıt verir ve ardından bir yanıt döndürür.
* `scoring-env.yml`: modeli ve `score.py` betiğini çalıştırmak için gereken Conda bağımlılıkları.
* `testdata.json`: dağıtılan Web hizmetini test etmek için kullanılabilecek bir veri dosyası.

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

CLı 'dan Azure aboneliğinizde kimlik doğrulayabilmeniz için çeşitli yollar vardır. En temel, bir tarayıcı kullanarak etkileşimli olarak kimlik doğrulaması yapmak için kullanılır. Etkileşimli olarak kimlik doğrulaması yapmak için bir komut satırı veya terminali açın ve aşağıdaki komutu kullanın:

```azurecli-interactive
az login
```

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, bir tarayıcı açmanız ve komut satırındaki yönergeleri izlemeniz gerekir. Yönergeler [https://aka.ms/devicelogin](https://aka.ms/devicelogin) göz atmayı ve bir yetkilendirme kodu girmeyi içerir.

## <a name="install-the-machine-learning-extension"></a>Machine Learning uzantısını yükler

Machine Learning uzantısını yüklemek için şu komutu kullanın:

```azurecli-interactive
az extension add -n azure-cli-ml
```

Uzantının zaten yüklü olduğunu belirten bir ileti alırsanız, en son sürüme güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension update -n azure-cli-ml
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure platformunda temel bir kaynak kapsayıcısıdır. Azure Machine Learning ile çalışırken, kaynak grubu Azure Machine Learning çalışma alanınızı içerecektir. Ayrıca, çalışma alanı tarafından kullanılan diğer Azure hizmetlerini de içerecektir. Örneğin, modelinize bulut tabanlı bir işlem kaynağı kullanarak eğitebilirsiniz, kaynak grubunda bu kaynak oluşturulur.

__Yeni bir kaynak grubu oluşturmak__için aşağıdaki komutu kullanın. `<resource-group-name>`, bu kaynak grubu için kullanılacak adla değiştirin. `<location>`, bu kaynak grubu için kullanılacak Azure bölgesiyle değiştirin:

> [!TIP]
> Azure Machine Learning kullanılabilir olan bir bölge seçmelisiniz. Bilgi için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service).

```azurecli-interactive
az group create --name <resource-group-name> --location <location>
```

Bu komuttan gelen yanıt aşağıdaki JSON ile benzerdir:

```json
{
  "id": "/subscriptions/<subscription-GUID>/resourceGroups/<resourcegroupname>",
  "location": "<location>",
  "managedBy": null,
  "name": "<resource-group-name>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

Kaynak gruplarıyla çalışma hakkında daha fazla bilgi için bkz. [az Group](https://docs.microsoft.com//cli/azure/group?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Yeni bir çalışma alanı oluşturmak için aşağıdaki komutu kullanın. `<workspace-name>`, bu çalışma alanı için kullanmak istediğiniz adla değiştirin. `<resource-group-name>`, kaynak grubunun adıyla değiştirin:

```azurecli-interactive
az ml workspace create -w <workspace-name> -g <resource-group-name>
```

Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<application-insight-name>",
  "containerRegistry": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.containerregistry/registries/<acr-name>",
  "creationTime": "2019-08-30T20:24:19.6984254+00:00",
  "description": "",
  "friendlyName": "<workspace-name>",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

## <a name="connect-local-project-to-workspace"></a>Yerel projeyi çalışma alanına bağla

Bir Terminal veya komut isteminden aşağıdaki komutları kullanarak dizinleri `cli-train-deploy` dizinine değiştirin ve ardından çalışma alanınıza bağlanın:

```azurecli-interactive
cd ~/MLOps/examples/cli-train-deploy
az ml folder attach -w <workspace-name> -g <resource-group-name>
```

Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

```json
{
  "Experiment name": "model-training",
  "Project path": "/home/user/MLOps/examples/cli-train-deploy",
  "Resource group": "<resource-group-name>",
  "Subscription id": "<subscription-id>",
  "Workspace name": "<workspace-name>"
}
```

Bu komut, çalışma alanınıza bağlanmak için gereken bilgileri içeren bir `.azureml/config.json` dosyası oluşturur. Bu öğreticide kullanılan `az ml` komutlarının geri kalanı bu dosyayı kullanacaktır. bu nedenle, çalışma alanını ve kaynak grubunu tüm komutlara eklemeniz gerekmez.

## <a name="create-the-compute-target-for-training"></a>Eğitim için işlem hedefi oluşturma

Bu örnek, modeli eğitmek için bir Azure Machine Learning Işlem kümesi kullanır. Yeni bir işlem kümesi oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml computetarget create amlcompute -n cpu-cluster --max-nodes 4 --vm-size Standard_D2_V2
```

Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

```json
{
  "location": "<location>",
  "name": "cpu-cluster",
  "provisioningErrors": null,
  "provisioningState": "Succeeded"
}
```

Bu komut, en fazla dört düğüm ile `cpu`adlı yeni bir işlem hedefi oluşturur. Seçilen VM boyutu, GPU kaynağına sahip bir VM sağlar. VM boyutu hakkında daha fazla bilgi için bkz. [VM türleri ve boyutları].

> [!IMPORTANT]
> İşlem hedefinin adı (Bu durumda`cpu`) önemlidir; bir sonraki bölümde kullanılan `.azureml/mnist.runconfig` dosyası tarafından başvurulur.

## <a name="define-the-dataset"></a>Veri kümesini tanımlama

Bir modeli eğitebilmeniz için eğitim verilerini bir veri kümesi kullanarak sağlayabilirsiniz. CLı 'dan bir veri kümesi oluşturmak için bir veri kümesi tanım dosyası sağlamanız gerekir. Depoda belirtilen `dataset.json` dosyası, veri kümesini kullanarak yeni bir veri kümesi oluşturur. Oluşturduğu veri kümesi `mnist-dataset`olarak adlandırılır.

`dataset.json` dosyasını kullanarak veri kümesini kaydetmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml dataset register -f dataset.json --skip-validation
```

Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

```json
{
  "definition": [
    "GetFiles"
  ],
  "registration": {
    "description": "mnist dataset",
    "id": "a13a4034-02d1-40bd-8107-b5d591a464b7",
    "name": "mnist-dataset",
    "tags": {
      "sample-tag": "mnist"
    },
    "version": 1,
    "workspace": "Workspace.create(name='myworkspace', subscription_id='mysubscriptionid', resource_group='myresourcegroup')"
  },
  "source": [
    "http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz",
    "http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz",
    "http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz",
    "http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz"
  ]
}
```

> [!IMPORTANT]
> `id` girdinin değerini, sonraki bölümde kullanıldığı şekilde kopyalayın.

## <a name="reference-the-dataset"></a>Veri kümesine başvur

Veri kümesini eğitim ortamında kullanılabilir hale getirmek için, bu dosyaya runconfig dosyasından başvurmanız gerekir. `.azureml/mnist.runconfig` dosyası aşağıdaki YAML girdilerini içerir:

```yaml
# The arguments to the script file.
arguments:
- --data-folder
- DatasetConsumptionConfig:mnist

.....

# The configuration details for data.
data:
  mnist:
# Data Location
    dataLocation:
# the Dataset used for this run.
      dataset:
# Id of the dataset.
        id: a13a4034-02d1-40bd-8107-b5d591a464b7
# the DataPath used for this run.
      datapath:
# Whether to create new folder.
    createOutputDirectories: false
# The mode to handle
    mechanism: mount
# Point where the data is download or mount or upload.
    environmentVariableName: mnist
# relative path where the data is download or mount or upload.
    pathOnCompute:
# Whether to overwrite the data if existing.
    overwrite: false
```

`id` girdinin değerini, veri kümesini kaydettiğinizde döndürülen değerle eşleşecek şekilde değiştirin. Bu değer, eğitim sırasında verileri işlem hedefine yüklemek için kullanılır.

Bu YAML şunları yapar:

* Veri kümesini (veri kümesinin KIMLIĞINE göre) eğitim ortamında takar ve `mnist` ortam değişkeninde bağlama noktasının yolunu depolar.
* Eğitim ortamının içindeki verinin konumunu (bağlama noktası) `--data-folder` bağımsız değişkenini kullanarak betiğe geçirir.

Runconfig dosyası aynı zamanda eğitim çalıştırması tarafından kullanılan ortamı yapılandırmak için kullanılan bilgileri de içerir. Bu dosyayı inceliyorsanız, daha önce oluşturduğunuz `cpu-compute` işlem hedefine başvurmuş olduğunu görürsünüz. Ayrıca, eğitim (`"nodeCount": "4"`) sırasında kullanılacak düğüm sayısını ve eğitim betiğini çalıştırmak için gereken Python paketlerini listeleyen bir `"condaDependencies"` bölümü içerir.

> [!TIP]
> El ile bir runconfig dosyası oluşturulması mümkün olsa da, bu örnekteki dosya depoya dahil `generate-runconfig.py` dosyası kullanılarak oluşturulmuştur. Bu dosya kayıtlı veri kümesine bir başvuru alır, bir Run config program aracılığıyla oluşturur ve sonra dosyayı devam ettirir.

Yapılandırma dosyalarını Çalıştır hakkında daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama ve kullanma](how-to-set-up-training-targets.md#create-run-configuration-and-submit-run-using-azure-machine-learning-cli)veya bir runconfig 'in tam şemasını görmek Için bu [JSON dosyasına](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json) başvuru.

## <a name="submit-the-training-run"></a>Eğitim çalıştırmasını gönder

`cpu-compute` işlem hedefinde çalışan bir eğitim başlatmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml run submit-script -c mnist -e myexperiment --source-directory scripts -t runoutput.json
```

Bu komut, deneme için bir ad belirtir (`myexperiment`). Deneme, çalışma alanında bu çalıştırma hakkındaki bilgileri depolar.

`-c mnist` parametresi `.azureml/mnist.runconfig` dosyasını belirtir.

`-t` parametresi bir JSON dosyasında bu çalıştırmaya yönelik bir başvuru depolar ve sonraki adımlarda modeli kaydetmek ve indirmek için kullanılır.

Eğitim işlemleri sırasında, uzaktan işlem kaynağındaki eğitim oturumundan bilgi akışı. Bilgilerin bir kısmı aşağıdaki metne benzer:

```text
Predict the test set
Accuracy is 0.9185
```

Bu metin eğitim betiğiyle günlüğe kaydedilir ve modelin doğruluğunu görüntüler. Diğer modeller farklı performans ölçümlerine sahip olacaktır.

Eğitim betiğini inceleyebileceğiniz zaman `outputs/sklearn_mnist_model.pkl`için eğitilen modeli depoladığında Alfa değerini de kullandığını fark edeceksiniz.

Model, eğitilen işlem hedefinde `./outputs` dizinine kaydedildi. Bu durumda, Azure bulutu 'nda Azure Machine Learning Işlem örneği. Eğitim süreci, eğitim Azure Machine Learning çalışma alanınıza gerçekleştiği işlem hedefinden `./outputs` dizininin içeriğini otomatik olarak yükler. Bu, denemenin bir parçası olarak depolanır (Bu örnekteki`myexperiment`).

## <a name="register-the-model"></a>Modeli kaydedin

Modeli denemenizin içindeki saklı sürümden doğrudan kaydetmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml model register -n mymodel -f runoutput.json --asset-path "outputs/sklearn_mnist_model.pkl" -t registeredmodel.json
```

Bu komut, eğitim tarafından oluşturulan `outputs/sklearn_mnist_model.pkl` dosyasını `mymodel`adlı yeni bir model kaydı olarak kaydeder. `--assets-path` denemelerle bir yola başvurur. Bu durumda, deneme ve çalıştırma bilgileri, eğitim komutu tarafından oluşturulan `runoutput.json` dosyasından yüklenir. `-t registeredmodel.json`, bu komut tarafından oluşturulan yeni kayıtlı modele başvuran bir JSON dosyası oluşturur ve kayıtlı modellerle çalışan diğer CLı komutları tarafından kullanılır.

Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

```json
{
  "createdTime": "2019-09-19T15:25:32.411572+00:00",
  "description": "",
  "experimentName": "myexperiment",
  "framework": "Custom",
  "frameworkVersion": null,
  "id": "mymodel:1",
  "name": "mymodel",
  "properties": "",
  "runId": "myexperiment_1568906070_5874522d",
  "tags": "",
  "version": 1
}
```

### <a name="model-versioning"></a>Model sürümü oluşturma

Model için döndürülen sürüm numarasını aklınızda edin. Bu ada sahip yeni bir modeli her kaydettiğinizde sürüm artırılır. Örneğin, modeli indirebilir ve aşağıdaki komutları kullanarak yerel bir dosyadan kaydedebilirsiniz:

```azurecli-interactive
az ml model download -i "mymodel:1" -t .
az ml model register -n mymodel -p "sklearn_mnist_model.pkl"
```

İlk komut kayıtlı modeli geçerli dizine indirir. Dosya adı, modeli kaydettiğinizde başvurulan dosya olan `sklearn_mnist_model.pkl`. İkinci komut, önceki kayıtla (`mymodel`) aynı ada sahip yerel modeli (`-p "sklearn_mnist_model.pkl"`) kaydeder. Bu kez, döndürülen JSON verisi sürümü 2 olarak listeler.

## <a name="deploy-the-model"></a>Modeli dağıtma

Bir modeli dağıtmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml model deploy -n myservice -m "mymodel:1" --ic inferenceConfig.yml --dc aciDeploymentConfig.yml
```

> [!NOTE]
> "LocalWebservice varlığı denetlenemedi" hakkında bir uyarı alabilirsiniz. Yerel bir Web hizmeti dağıtmadıkça bunu güvenle yoksayabilirsiniz.

Bu komut, daha önce kaydettiğiniz modelin 1. sürümünü kullanarak `myservice`adlı yeni bir hizmet dağıtır.

`inferenceConfig.yml` dosyası, giriş betiği (`score.py`) ve yazılım bağımlılıkları gibi çıkarım gerçekleştirme hakkında bilgi sağlar. Bu dosyanın yapısı hakkında daha fazla bilgi için, bkz. [çıkarım yapılandırma şeması](reference-azure-machine-learning-cli.md#inference-configuration-schema). Giriş betikleri hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md#prepare-to-deploy).

`aciDeploymentConfig.yml`, hizmeti barındırmak için kullanılan dağıtım ortamını açıklar. Dağıtım yapılandırması, dağıtım için kullandığınız işlem türüne özeldir. Bu durumda, bir Azure Kapsayıcı örneği kullanılır. Daha fazla bilgi için bkz. [dağıtım yapılandırma şeması](reference-azure-machine-learning-cli.md#deployment-configuration-schema).

Dağıtım işleminin tamamlanması birkaç dakika sürer.

> [!TIP]
> Bu örnekte, Azure Container Instances kullanılır. ACI 'ya dağıtımlar, gerekli ACI kaynağını otomatik olarak oluşturur. Bunun yerine Azure Kubernetes hizmetine dağıtım yapıyorsanız, zaman içinde bir AKS kümesi oluşturmanız ve bunu `az ml model deploy` komutunun bir parçası olarak belirtmeniz gerekir. AKS 'e dağıtmaya bir örnek için bkz. [Azure Kubernetes hizmet kümesine model dağıtma](how-to-deploy-azure-kubernetes-service.md).

Birkaç dakika sonra, aşağıdaki JSON ile benzer bilgiler döndürülür:

```json
ACI service creation operation finished, operation "Succeeded"
{
  "computeType": "ACI",
  {...ommitted for space...}
  "runtimeType": null,
  "scoringUri": "http://6c061467-4e44-4f05-9db5-9f9a22ef7a5d.eastus2.azurecontainer.io/score",
  "state": "Healthy",
  "tags": "",
  "updatedAt": "2019-09-19T18:22:32.227401+00:00"
}
```

### <a name="the-scoring-uri"></a>Puanlama URI 'SI

Dağıtımdan döndürülen `scoringUri`, Web hizmeti olarak dağıtılan bir modelin REST uç noktasıdır. Bu URI 'yi aşağıdaki komutu kullanarak da edinebilirsiniz:

```azurecli-interactive
az ml service show -n myservice
```

Bu komut, `scoringUri`dahil olmak üzere aynı JSON belgesini döndürür.

REST uç noktası, hizmete veri göndermek için kullanılabilir. Hizmete veri gönderen bir istemci uygulaması oluşturma hakkında bilgi için bkz. [Web hizmeti olarak dağıtılan Azure Machine Learning modelini](how-to-consume-web-service.md) kullanma

### <a name="send-data-to-the-service"></a>Verileri hizmete gönder

Uç noktasını çağırmak için bir istemci uygulaması oluşturabilirsiniz, ancak Machine Learning CLı, test istemcisi olarak davranabilen bir yardımcı program sağlar. `testdata.json` dosyadaki verileri hizmete göndermek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml service run -n myservice -d @testdata.json
```

> [!TIP]
> PowerShell kullanıyorsanız, bunun yerine aşağıdaki komutu kullanın:
>
> ```powershell
> az ml service run -n myservice -d `@testdata.json
> ```

Komuttan gelen yanıt `[ 3 ]`benzerdir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

> [!IMPORTANT]
> Oluşturduğunuz kaynaklar, diğer Azure Machine Learning öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir.

### <a name="delete-deployed-service"></a>Dağıtılan hizmeti Sil

Azure Machine Learning çalışma alanını kullanmaya devam etmek, ancak maliyetleri azaltmak için dağıtılan hizmetten kurtulabilmesi istiyorsanız aşağıdaki komutu kullanın:

```azurecli-interactive
az ml service delete -n myservice
```

Bu komut, silinen hizmetin adını içeren bir JSON belgesi döndürür. Hizmet silinmeden önce bu işlem birkaç dakika sürebilir.

### <a name="delete-the-training-compute"></a>Eğitim işlem programını silme

Azure Machine Learning çalışma alanını kullanmaya devam etmek, ancak eğitim için oluşturulan `cpu-compute` işlem hedefinin kurtulığını almak istiyorsanız aşağıdaki komutu kullanın:

```azurecli-interactive
az ml computetarget delete -n cpu
```

Bu komut, silinen işlem hedefinin KIMLIĞINI içeren bir JSON belgesi döndürür. İşlem hedefi silinmeden önce bu işlem birkaç dakika sürebilir.

### <a name="delete-everything"></a>Her şeyi sil

Oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, ek ücret ödemeniz için bunları silin.

Kaynak grubunu ve bu belgede oluşturulan tüm Azure kaynaklarını silmek için aşağıdaki komutu kullanın. `<resource-group-name>`, daha önce oluşturduğunuz kaynak grubunun adıyla değiştirin:

```azurecli-interactive
az group delete -g <resource-group-name> -y
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure Machine Learning öğreticide, aşağıdaki görevler için Machine Learning CLı 'yi kullandınız:

> [!div class="checklist"]
> * Machine Learning uzantısını yükler
> * Bir Azure Machine Learning çalışma alanı oluşturma
> * Modeli eğitmek için kullanılan işlem kaynağını oluşturma
> * Modeli eğitmek için kullanılan veri kümesini tanımlama ve kaydetme
> * Eğitim çalıştırması başlatma
> * Bir modeli kaydetme ve indirme
> * Modeli bir Web hizmeti olarak dağıtma
> * Web hizmetini kullanarak veri puanı

CLı kullanma hakkında daha fazla bilgi için bkz. [Azure Machine Learning IÇIN CLI uzantısını kullanma](reference-azure-machine-learning-cli.md).
