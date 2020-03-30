---
title: CLI'den modelleri eğitin ve dağıtın
titleSuffix: Azure Machine Learning
description: Komut satırından bir modeli eğitmek, kaydetmek ve dağıtmak için Azure CLI için makine öğrenimi uzantısını nasıl kullanacağınızı öğrenin.
ms.author: larryfr
author: Blackmist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: 401ce2aed2c783169592f0dc664a3a7baea415b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336618"
---
# <a name="tutorial-train-and-deploy-a-model-from-the-cli"></a>Öğretici: CLI'den bir model eğitin ve dağıtın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu eğitimde, bir modeli eğitmek, kaydetmek ve dağıtmak için Azure CLI için makine öğrenimi uzantısını kullanırsınız.

Bu öğretici Python eğitim komut dosyaları temel bir model eğitmek için [scikit-learn](https://scikit-learn.org/) kullanın. Bu öğreticinin odak noktası komut dosyaları veya model değil, Azure Machine Learning ile çalışmak için CLI'yi kullanma işlemidir.

Aşağıdaki eylemleri nasıl gerçekleştirdiğini öğrenin:

> [!div class="checklist"]
> * Makine öğrenimi uzantısını yükleyin
> * Azure Machine Learning çalışma alanı oluşturma
> * Modeli eğitmek için kullanılan bilgi işlem kaynağını oluşturma
> * Modeli eğitmek için kullanılan veri kümesini tanımlayın ve kaydedin
> * Eğitim çalıştırmayı başlatma
> * Bir model kaydedin ve indirin
> * Modeli web hizmeti olarak dağıtma
> * Web hizmetini kullanarak veri puan

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Bu belgedeki CLI komutlarını **yerel ortamınızdan**kullanmak için [Azure CLI'ye](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ihtiyacınız vardır.

    Azure Bulut [Kabuğu'nu](https://azure.microsoft.com//features/cloud-shell/)kullanıyorsanız, CLI'ye tarayıcı üzerinden erişilir ve bulutta yaşar.

## <a name="download-the-example-project"></a>Örnek projeyi indirin

Bu öğretici için [https://github.com/microsoft/MLOps](https://github.com/microsoft/MLOps) projeyi indirin. Dizindeki `examples/cli-train-deploy` dosyalar bu öğreticideki adımlar tarafından kullanılır.

Dosyaların yerel bir kopyasını almak için [bir .zip arşivi indirin](https://github.com/microsoft/MLOps/archive/master.zip)veya depoyu klonlamak için aşağıdaki Git komutunu kullanın:

```azurecli-interactive
git clone https://github.com/microsoft/MLOps.git
```

### <a name="training-files"></a>Eğitim dosyaları

Projedeki `examples/cli-train-deploy` dizin, bir modeli eğitirken kullanılan aşağıdaki dosyaları içerir:

* `.azureml\mnist.runconfig`: __Çalıştırılan yapılandırma__ dosyası. Bu dosya, modeli eğitmek için gereken çalışma zamanı ortamını tanımlar. Bu örnekte, modeli eğitim ortamına eğitmek için kullanılan verileri de bağlar.
* `scripts\train.py`: Eğitim senaryosu. Bu dosya modeli eğitir.
* `scripts\utils.py`: Eğitim komut dosyası tarafından kullanılan bir yardımcı dosya.
* `.azureml\conda_dependencies.yml`: Eğitim komut dosyasını çalıştırmak için gereken yazılım bağımlılıklarını tanımlar.
* `dataset.json`: Dataset tanımı. MNIST veri kümesini Azure Machine Learning çalışma alanına kaydetmek için kullanılır.

### <a name="deployment-files"></a>Dağıtım dosyaları

Depo, eğitilmiş modeli bir web hizmeti olarak dağıtmak için kullanılan aşağıdaki dosyaları içerir:

* `aciDeploymentConfig.yml`: Dağıtım __yapılandırma__ dosyası. Bu dosya, model için gereken barındırma ortamını tanımlar.
* `inferenceConfig.json`: __Çıkarım yapılandırma__ dosyası. Bu dosya, modelle veri puanlamak için hizmet tarafından kullanılan yazılım ortamını tanımlar.
* `score.py`: Gelen verileri kabul eden, modeli kullanarak puanlayan ve ardından yanıt döndüren bir python komut dosyası.
* `scoring-env.yml`: Modeli ve `score.py` komut dosyasını çalıştırmak için gerekli conda bağımlılıkları.
* `testdata.json`: Dağıtılan web hizmetini sınamak için kullanılabilecek bir veri dosyası.

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

Azure aboneliğinizi CLI'den doğrulamanın birkaç yolu vardır. En temel içinde bir tarayıcı kullanarak interaktif olarak kimlik sağlamaktır. Etkileşimli olarak kimlik doğrulaması yapmak için bir komut satırı veya terminal açın ve aşağıdaki komutu kullanın:

```azurecli-interactive
az login
```

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, bir tarayıcı açmanız ve komut satırındaki yönergeleri izlemeniz gerekir. Yönergeler, yetkilendirme [https://aka.ms/devicelogin](https://aka.ms/devicelogin) koduna göz atma ve girmeyi içerir.

[!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)]

## <a name="install-the-machine-learning-extension"></a>Makine öğrenimi uzantısını yükleyin

Makine öğrenimi uzantısını yüklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension add -n azure-cli-ml
```

Uzantın zaten yüklü olduğuna dair bir ileti alırsanız, en son sürüme güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension update -n azure-cli-ml
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure platformundaki temel bir kaynak kapsayıcısır. Azure Machine Learning ile çalışırken, kaynak grubu Azure Machine Learning çalışma alanınızı içerir. Ayrıca, çalışma alanı tarafından kullanılan diğer Azure hizmetlerini de içerir. Örneğin, modelinizi bulut tabanlı bir bilgi işlem kaynağı kullanarak eğittiyseniz, bu kaynak kaynak grubunda oluşturulur.

__Yeni bir kaynak grubu oluşturmak__için aşağıdaki komutu kullanın. Bu `<resource-group-name>` kaynak grubu için kullanılacak adla değiştirin. Bu `<location>` kaynak grubu için kullanılacak Azure bölgesiyle değiştirin:

> [!TIP]
> Azure Machine Learning'in kullanılabildiği bir bölge seçmelisiniz. Daha fazla bilgi [için, bölgeye göre mevcut Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service)bakın.

```azurecli-interactive
az group create --name <resource-group-name> --location <location>
```

Bu komutun yanıtı aşağıdaki JSON'a benzer:

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

Kaynak gruplarıyla çalışma hakkında daha fazla bilgi için [az grubuna](https://docs.microsoft.com//cli/azure/group?view=azure-cli-latest)bakın.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Yeni bir çalışma alanı oluşturmak için aşağıdaki komutu kullanın. Bu `<workspace-name>` çalışma alanı için kullanmak istediğiniz adla değiştirin. Kaynak `<resource-group-name>` grubunun adı ile değiştirin:

```azurecli-interactive
az ml workspace create -w <workspace-name> -g <resource-group-name>
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

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

## <a name="connect-local-project-to-workspace"></a>Yerel projeyi çalışma alanına bağlama

Bir terminal veya komut isteminden, aşağıdaki komutları `cli-train-deploy` dizin değiştirmek ve ardından çalışma alanınıza bağlanın:

```azurecli-interactive
cd ~/MLOps/examples/cli-train-deploy
az ml folder attach -w <workspace-name> -g <resource-group-name>
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

```json
{
  "Experiment name": "model-training",
  "Project path": "/home/user/MLOps/examples/cli-train-deploy",
  "Resource group": "<resource-group-name>",
  "Subscription id": "<subscription-id>",
  "Workspace name": "<workspace-name>"
}
```

Bu komut, `.azureml/config.json` çalışma alanınıza bağlanmak için gereken bilgileri içeren bir dosya oluşturur. Bu öğreticide `az ml` kullanılan komutların geri kalanı bu dosyayı kullanır, böylece çalışma alanını ve kaynak grubunu tüm komutlara eklemek zorunda kalmamanız gerekir.

## <a name="create-the-compute-target-for-training"></a>Eğitim için işlem hedefini oluşturma

Bu örnek, modeli eğitmek için bir Azure Machine Learning Compute kümesi kullanır. Yeni bir işlem kümesi oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml computetarget create amlcompute -n cpu-cluster --max-nodes 4 --vm-size Standard_D2_V2
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

```json
{
  "location": "<location>",
  "name": "cpu-cluster",
  "provisioningErrors": null,
  "provisioningState": "Succeeded"
}
```

Bu komut, en fazla dört `cpu-cluster`düğüm içeren yeni bir işlem hedefi oluşturur. Seçilen VM boyutu, GPU kaynağı na sahip bir VM sağlar. VM boyutu hakkında bilgi için [VM türleri ve boyutları] bakın.

> [!IMPORTANT]
> Bilgi işlem hedefinin adı`cpu-cluster` (bu durumda), önemlidir; bir sonraki bölümde `.azureml/mnist.runconfig` kullanılan dosya tarafından başvurulmusur.

## <a name="define-the-dataset"></a>Veri kümesini tanımlama

Bir modeli eğitmek için, bir veri kümesi kullanarak eğitim verilerini sağlayabilirsiniz. CLI'den bir veri kümesi oluşturmak için bir veri kümesi tanım dosyası sağlamanız gerekir. Repo'da sağlanan dosya, `dataset.json` MNIST verilerini kullanarak yeni bir veri kümesi oluşturur. Oluşturduğu veri kümesinin adı. `mnist-dataset`

Dosyayı kullanarak veri `dataset.json` kümesini kaydetmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml dataset register -f dataset.json --skip-validation
```

Bu komutun çıktısı aşağıdaki JSON'a benzer:

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
> Bir sonraki bölümde `id` kullanıldığı gibi girişdeğerini kopyalayın.

Bir veri kümesi için daha kapsamlı bir şablon görmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml dataset register --show-template
```

## <a name="reference-the-dataset"></a>Veri kümesine başvurun

Veri kümesini eğitim ortamında kullanılabilir hale getirmek için runconfig dosyasından başvurulmalısınız. Dosya `.azureml/mnist.runconfig` aşağıdaki YAML girişlerini içerir:

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

Veri kümesini `id` kaydettirdiğinizde döndürülen değerle eşleşecek şekilde girişin değerini değiştirin. Bu değer, eğitim sırasında verileri bilgi işlem hedefine yüklemek için kullanılır.

Bu YAML eğitim sırasında aşağıdaki eylemleri sonuçlanır:

* Veri kümesini (veri kümesinin kimliğine göre) eğitim ortamında bağlar ve `mnist` ortam değişkenindeki montaj noktasına giden yolu depolar.
* Bağımsız değişkeni kullanarak, eğitim ortamı içindeki verilerin (montaj `--data-folder` noktası) konumunu komut dosyasına geçirir.

Runconfig dosyası, eğitim çalışması tarafından kullanılan ortamı yapılandırmak için kullanılan bilgileri de içerir. Bu dosyayı incelerseniz, daha önce oluşturduğunuz işlem hedefine `cpu-compute` başvurulacağını görürsünüz. Ayrıca, eğitim ()`"nodeCount": "4"`veya kullanılırken kullanılacak düğüm sayısını `"condaDependencies"` listeler ve eğitim komut dosyasını çalıştırmak için gereken Python paketlerini listeleyen bir bölüm içerir.

> [!TIP]
> Bir runconfig dosyası nın el ile oluşturulması mümkün olsa da, `generate-runconfig.py` bu örnekteki dosya depoda yer alan dosya kullanılarak oluşturulmuştur. Bu dosya kayıtlı veri kümesine bir başvuru alır, programlı bir çalıştırma config oluşturur ve sonra dosyaya devam eder.

Çalıştırılan yapılandırma dosyaları hakkında daha fazla bilgi için [bkz.](how-to-set-up-training-targets.md#create-run-configuration-and-submit-run-using-azure-machine-learning-cli) Tam bir JSON referans için, [runconfigschema.json](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json)bakın.

## <a name="submit-the-training-run"></a>Eğitim çalışmasını gönderme

İşlem hedefiüzerinde `cpu-cluster` bir eğitim çalışması başlatmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml run submit-script -c mnist -e myexperiment --source-directory scripts -t runoutput.json
```

Bu komut, deneme için bir`myexperiment`ad belirtir ( ). Deneme, çalışma alanında bu çalışma yla ilgili bilgileri depolar.

Parametre `-c mnist` dosyayı `.azureml/mnist.runconfig` belirtir.

Parametre, `-t` json dosyasında bu çalıştırmaya bir başvuru depolar ve modeli kaydetmek ve indirmek için sonraki adımlarda kullanılır.

Eğitim süreçleri çalıştırDın, uzaktan bilgi işlem kaynağındaki eğitim oturumundan bilgi akışı sağlar. Bilgilerin bir kısmı aşağıdaki metne benzer:

```output
Predict the test set
Accuracy is 0.9185
```

Bu metin eğitim komut dosyasından günlüğe kaydedilir ve modelin doğruluğunu görüntüler. Diğer modeller farklı performans ölçümlerine sahip olacaktır.

Eğitim komut dosyasını incelerseniz, eğitilen modeli `outputs/sklearn_mnist_model.pkl`' ye depoladığında alfa değerini de kullandığını fark edersiniz.

Model, eğitildiği `./outputs` bilgisayar hedefindeki dizin için kaydedildi. Bu durumda, Azure bulutundaki Azure Machine Learning Compute örneği. Eğitim süreci, eğitimin gerçekleştiği bilgi `./outputs` işlem hedefindeki dizin içeriğini Azure Machine Learning çalışma alanınıza otomatik olarak yükler. Denemenin bir parçası olarak depolanır`myexperiment` (bu örnekte).

## <a name="register-the-model"></a>Modeli kaydedin

Modeli doğrudan denemenizdeki depolanan sürümden kaydetmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml model register -n mymodel -f runoutput.json --asset-path "outputs/sklearn_mnist_model.pkl" -t registeredmodel.json
```

Bu komut, `outputs/sklearn_mnist_model.pkl` eğitim çalıştırılanın oluşturduğu dosyayı `mymodel`yeni bir model kaydı olarak kaydeder. Bir `--assets-path` denemedeki bir yola başvurur. Bu durumda, deneme ve çalıştır bilgileri `runoutput.json` eğitim komutu tarafından oluşturulan dosyadan yüklenir. Bu `-t registeredmodel.json` komut tarafından oluşturulan yeni kayıtlı modele başvurur ve kayıtlı modeller ile çalışan diğer CLI komutları tarafından kullanılan bir JSON dosyası oluşturur.

Bu komutun çıktısı aşağıdaki JSON'a benzer:

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

### <a name="model-versioning"></a>Model sürümü

Model için döndürülen sürüm numarasını not edin. Bu ada sahip yeni bir modeli her kaydettirdiğinizde sürüm artımlanır. Örneğin, aşağıdaki komutları kullanarak modeli indirebilir ve yerel bir dosyadan kaydedebilirsiniz:

```azurecli-interactive
az ml model download -i "mymodel:1" -t .
az ml model register -n mymodel -p "sklearn_mnist_model.pkl"
```

İlk komut kayıtlı modeli geçerli dizine indirir. Dosya adı, `sklearn_mnist_model.pkl`modeli kaydettirdiğinizde başvurulan dosyadır. İkinci komut, önceki kayıtla`-p "sklearn_mnist_model.pkl"`aynı ada sahip yerel`mymodel`modeli ( ) kaydeder . Bu kez, JSON verileri döndürülen sürümü 2 olarak listeler.

## <a name="deploy-the-model"></a>Modeli dağıtma

Bir modeli dağıtmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml model deploy -n myservice -m "mymodel:1" --ic inferenceConfig.json --dc aciDeploymentConfig.yml
```

> [!NOTE]
> "Yerel Web Hizmeti varlığını denetlemede başarısız olunması" veya "Docker istemcisi oluşturulamadı" uyarısı alabilirsiniz. Yerel bir web hizmeti dağıtmadığınız için bunu güvenle yok sayabilirsiniz.

Bu komut, daha önce `myservice`kaydettiğiniz modelin sürüm 1'ini kullanarak adlı yeni bir hizmet dağıtır.

Dosya `inferenceConfig.yml` çıkarım için modelin nasıl kullanılacağı hakkında bilgi sağlar. Örneğin, giriş komut dosyasına`score.py`( ) ve yazılım bağımlılıklarına başvurur.

Bu dosyanın yapısı hakkında daha fazla bilgi için [Çıkarım yapılandırma şemasına](reference-azure-machine-learning-cli.md#inference-configuration-schema)bakın. Giriş komut dosyaları hakkında daha fazla bilgi için Azure [Machine Learning ile modelleri dağıt'a](how-to-deploy-and-where.md#prepare-to-deploy)bakın.

Hizmet `aciDeploymentConfig.yml` barındırmak için kullanılan dağıtım ortamını açıklar. Dağıtım yapılandırması, dağıtım için kullandığınız bilgi işlem türüne özgüdür. Bu durumda, bir Azure Kapsayıcı Örneği kullanılır. Daha fazla bilgi için [Dağıtım yapılandırma şemasına](reference-azure-machine-learning-cli.md#deployment-configuration-schema)bakın.

Dağıtım işleminin tamamlanması birkaç dakika sürer.

> [!TIP]
> Bu örnekte, Azure Kapsayıcı Örnekleri kullanılır. ACI'ye yapılan dağıtımlar otomatik olarak gerekli ACI kaynağını oluşturur. Bunun yerine Azure Kubernetes Hizmeti'ne dağıtacaksanız, önceden bir AKS kümesi oluşturmanız `az ml model deploy` ve komutun bir parçası olarak belirtmeniz gerekir. AKS'ye dağıtım örneği için, [bir Azure Kubernetes Hizmet kümesine bir model dağıt'a](how-to-deploy-azure-kubernetes-service.md)bakın.

Birkaç dakika sonra, aşağıdaki JSON benzer bilgiler döndürülür:

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

### <a name="the-scoring-uri"></a>Puanlama URI

Dağıtımdan `scoringUri` döndürülen, web hizmeti olarak dağıtılan bir modelin REST bitiş noktasıdır. Ayrıca aşağıdaki komutu kullanarak bu URI alabilirsiniz:

```azurecli-interactive
az ml service show -n myservice
```

Bu komut, `scoringUri`aynı JSON belgesini döndürür.

REST bitiş noktası hizmete veri göndermek için kullanılabilir. Hizmete veri gönderen bir istemci uygulaması oluşturma hakkında bilgi için [bkz.](how-to-consume-web-service.md)

### <a name="send-data-to-the-service"></a>Hizmete veri gönderme

Bitiş noktasını aramak için bir istemci uygulaması oluşturabilirsiniz, ancak makine öğrenme CLI bir test istemcisi olarak hareket edebilir bir yardımcı program sağlar. `testdata.json` Dosyadaki verileri hizmete göndermek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml service run -n myservice -d @testdata.json
```

> [!TIP]
> PowerShell kullanıyorsanız, bunun yerine aşağıdaki komutu kullanın:
>
> ```azurecli-interactive
> az ml service run -n myservice -d `@testdata.json
> ```

Komutun yanıtı `[ 3 ]`' na benzer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

> [!IMPORTANT]
> Oluşturduğunuz kaynaklar, diğer Azure Machine Learning öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir.

### <a name="delete-deployed-service"></a>Dağıtılan hizmeti silme

Azure Machine Learning çalışma alanını kullanmaya devam etmeyi planlıyorsanız, ancak maliyetleri azaltmak için dağıtılan hizmetten kurtulmak istiyorsanız aşağıdaki komutu kullanın:

```azurecli-interactive
az ml service delete -n myservice
```

Bu komut, silinen hizmetin adını içeren bir JSON belgesini döndürür. Hizmetin silinmesi birkaç dakika sürebilir.

### <a name="delete-the-training-compute"></a>Eğitim bilgisayarını silme

Azure Machine Learning çalışma alanını kullanmaya devam etmeyi planlıyorsanız, ancak `cpu-cluster` eğitim için oluşturulan bilgi işlem hedefinden kurtulmak istiyorsanız aşağıdaki komutu kullanın:

```azurecli-interactive
az ml computetarget delete -n cpu-cluster
```

Bu komut, silinen işlem hedefinin kimliğini içeren bir JSON belgesini döndürür. İşlem hedefinin silinmeden önce birkaç dakika sürebilir.

### <a name="delete-everything"></a>Her şeyi silme

Oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, ek ücretödememek için bunları silin.

Kaynak grubunu ve bu belgede oluşturulan tüm Azure kaynaklarını silmek için aşağıdaki komutu kullanın. Daha `<resource-group-name>` önce oluşturduğunuz kaynak grubunun adı ile değiştirin:

```azurecli-interactive
az group delete -g <resource-group-name> -y
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure Machine Learning öğreticisinde, makine öğrenimi CLI'yi aşağıdaki görevler için kullandınız:

> [!div class="checklist"]
> * Makine öğrenimi uzantısını yükleyin
> * Azure Machine Learning çalışma alanı oluşturma
> * Modeli eğitmek için kullanılan bilgi işlem kaynağını oluşturma
> * Modeli eğitmek için kullanılan veri kümesini tanımlayın ve kaydedin
> * Eğitim çalıştırmayı başlatma
> * Bir model kaydedin ve indirin
> * Modeli web hizmeti olarak dağıtma
> * Web hizmetini kullanarak veri puan

CLI'yi kullanma hakkında daha fazla bilgi için Azure [Machine Learning için CLI uzantısını kullanın'a](reference-azure-machine-learning-cli.md)bakın.
