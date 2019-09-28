---
title: CLı 'dan modelleri eğitme ve dağıtma
titleSuffix: Azure Machine Learning service
description: Komut satırından bir modeli eğitmek, kaydettirmek ve dağıtmak için Azure CLı için Machine Learning uzantısını nasıl kullanacağınızı öğrenin.
ms.author: larryfr
author: Blackmist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: fb46aaf04535c1b44cdd80810fbb6382dc727a67
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350414"
---
# <a name="tutorial-train-and-deploy-a-model-from-the-cli"></a>Öğretici: CLı 'dan model eğitimi ve dağıtma

Bu öğreticide, bir modeli eğitmek, kaydettirmek ve dağıtmak için Azure CLı için Machine Learning uzantısını kullanırsınız.

Bu öğreticideki Python eğitim betikleri [scikit-](https://scikit-learn.org/) temel bir modeli eğitme hakkında bilgi edinin. Bu öğreticinin odağı betiklerin veya modeldeki değildir, ancak Azure Machine Learning çalışmak için CLı kullanma işlemidir.

Aşağıdaki eylemleri nasıl gerçekleştireceğinizi öğrenin:

> [!div class="checklist"]
> * Machine Learning uzantısını yükler
> * Bir Azure Machine Learning çalışma alanı oluşturma
> * Modeli eğitmek için kullanılan işlem kaynağını oluşturma
> * Eğitim çalıştırması başlatma
> * Bir modeli kaydetme ve indirme
> * Modeli bir Web hizmeti olarak dağıtma
> * Web hizmetini kullanarak veri puanı

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* **Yerel ortamınızdan**bu belgedeki CLI komutlarını kullanmak IÇIN [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)gerekir.

    [Azure Cloud Shell](https://azure.microsoft.com//features/cloud-shell/)KULLANıYORSANıZ, CLI tarayıcı aracılığıyla erişilir ve bulutta bulunur.

## <a name="download-the-example-project"></a>Örnek projeyi indirin

Bu öğretici için [https://github.com/microsoft/MLOps](https://github.com/microsoft/MLOps) projesini indirin. @No__t-0 ve `model-deployment` dizinlerindeki dosyalar bu öğreticideki adımlar tarafından kullanılır.

Dosyaların yerel bir kopyasını almak için, [bir. zip arşivi indirin](https://github.com/microsoft/MLOps/archive/master.zip)ya da depoyu kopyalamak Için aşağıdaki git komutunu kullanın:

```azurecli-interactive
git clone https://github.com/microsoft/MLOps.git
```

### <a name="training-files"></a>Eğitim dosyaları

@No__t-0 dizini, bir modeli eğitmek için kullanılan aşağıdaki dosyaları içerir:

* `.azureml\sklearn.runconfig`: Bir __Çalıştırma yapılandırma__ dosyası. Bu dosya, modeli eğitmek için gereken çalışma zamanı ortamını tanımlar.
* `train-sklearn.py`: Eğitim betiği. Bu dosya, modeli tratlar.
* `mylib.py`: @No__t-0 tarafından kullanılan bir yardımcı modül.
* `training-env.yml`: Eğitim betiğini çalıştırmak için gereken yazılım bağımlılıklarını tanımlar.

Eğitim betiği, bir modeli eğitme hakkında bilgi edinmek için scikit-ile sunulan diabetes veri kümesini kullanır.

### <a name="deployment-files"></a>Dağıtım dosyaları

@No__t-0 dizini, eğitilen modeli bir Web hizmeti olarak dağıtmak için kullanılan aşağıdaki dosyaları içerir:

* `aciDeploymentConfig.yml`: Bir __dağıtım yapılandırma__ dosyası. Bu dosya, model için gereken barındırma ortamını tanımlar.
* `inferenceConfig.yml`: Bir çıkarım configuration__ dosyası. Bu dosya, hizmet tarafından modelle verileri Puanlama için kullanılan yazılım ortamını tanımlar.
* `score.py`: Gelen verileri kabul eden bir Python betiği, modeli kullanarak bunu alır ve bir yanıt döndürür.
* `scoring-env.yml`: Model ve `score.py` betiği çalıştırmak için Conda bağımlılıkları gereklidir.

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

CLı 'dan Azure aboneliğinizde kimlik doğrulayabilmeniz için çeşitli yollar vardır. En temel, bir tarayıcı kullanarak etkileşimli olarak kimlik doğrulaması yapmak için kullanılır. Etkileşimli olarak kimlik doğrulaması yapmak için bir komut satırı veya terminali açın ve aşağıdaki komutu kullanın:

```azurecli-interactive
az login
```

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, bir tarayıcı açmanız ve komut satırındaki yönergeleri izlemeniz gerekir. Yönergeler, bir yetkilendirme koduna [https://aka.ms/devicelogin](https://aka.ms/devicelogin) göz atmaya ve girmeye yönelik bilgiler içerir.

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

Kaynak grubu, Azure platformunda temel bir kaynak kapsayıcısıdır. Azure Machine Learning hizmetiyle çalışırken, kaynak grubu Azure Machine Learning hizmeti çalışma alanınızı içerecektir. Ayrıca, çalışma alanı tarafından kullanılan diğer Azure hizmetlerini de içerecektir. Örneğin, modelinize bulut tabanlı bir işlem kaynağı kullanarak eğitebilirsiniz, kaynak grubunda bu kaynak oluşturulur.

__Yeni bir kaynak grubu oluşturmak__için aşağıdaki komutu kullanın. Bu `<resource-group-name>` kaynak grubu için kullanılacak adla değiştirin. Bu `<location>` kaynak grubu için kullanılacak Azure bölgesiyle değiştirin:

> [!TIP]
> Azure Machine Learning hizmetinin kullanılabildiği bir bölge seçmelisiniz. Bilgi için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service).

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

Yeni bir çalışma alanı oluşturmak için aşağıdaki komutu kullanın. @No__t-0 değerini bu çalışma alanı için kullanmak istediğiniz adla değiştirin. @No__t-0 değerini kaynak grubunun adıyla değiştirin:

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

Bir Terminal veya komut isteminden aşağıdaki komutları kullanarak dizinleri `mlops` dizinine değiştirin ve ardından çalışma alanınıza bağlanın:

```azurecli-interactive
cd ~/mlops
az ml folder attach -w <workspace-name> -g <resource-group-name>
```

Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

```json
{
  "Experiment name": "model-training",
  "Project path": "/Code/MLOps/model-training",
  "Resource group": "<resource-group-name>",
  "Subscription id": "<subscription-id>",
  "Workspace name": "<workspace-name>"
}
```

Bu komut, çalışma alanınıza bağlanmak için gereken bilgileri içeren bir `.azureml/config.json` dosyası oluşturur. Bu öğreticide kullanılan `az ml` komutlarının geri kalanı bu dosyayı kullanacaktır. bu nedenle, çalışma alanını ve kaynak grubunu tüm komutlara eklemeniz gerekmez.

## <a name="create-the-compute-target-for-training"></a>Eğitim için işlem hedefi oluşturma

Bu örnek, modeli eğitmek için bir Azure Machine Learning Işlem örneği kullanır. Yeni bir işlem örneği oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml computetarget create amlcompute -n cpu --max-nodes 4 --vm-size Standard_D2_V2
```

Bu komutun çıktısı aşağıdaki JSON 'a benzerdir:

```json
{
  "location": "<location>",
  "name": "cpu",
  "provisioningErrors": null,
  "provisioningState": "Succeeded"
}
```

Bu komut, en fazla dört düğüm ile `cpu` adlı yeni bir işlem hedefi oluşturur. Seçilen VM boyutu, GPU kaynağına sahip bir VM sağlar. VM boyutu hakkında daha fazla bilgi için bkz. [VM türleri ve boyutları].

> [!IMPORTANT]
> İşlem hedefinin adı (Bu örnekte `cpu`), önemlidir; bir sonraki bölümde kullanılan `.azureml/sklearn.runconfig` dosyası tarafından başvurulur.

## <a name="submit-the-training-run"></a>Eğitim çalıştırmasını gönder

@No__t-0 işlem hedefi üzerinde bir eğitim çalıştırmak için, dizinleri `model-training` diziniyle değiştirin ve aşağıdaki komutu kullanın:

```azurecli-interactive
cd ~/mlops/model-training
az ml run submit-script -e myexperiment -c sklearn -d training-env.yml -t runoutput.json train-sklearn.py
```

Bu komut, deneme için bir ad belirtir (`myexperiment`). Deneme, çalışma alanında bu çalıştırma hakkındaki bilgileri depolar.

@No__t-0 parametresi `.azureml/sklearn.runconfig` dosyasını belirtir. Daha önce belirtildiği gibi, bu dosya eğitim çalışması tarafından kullanılan ortamı yapılandırmak için kullanılan bilgileri içerir. Bu dosyayı inceliyorsanız, daha önce oluşturduğunuz `cpu` işlem hedefine başvurmuş olduğunu görürsünüz. Ayrıca, eğitim (`"nodeCount": "4"`) sırasında kullanılacak düğüm sayısını listeler ve eğitim betiğini çalıştırmak için gereken Python paketlerini listeleyen bir `"condaDependenciees"` bölümü içerir.

Yapılandırma dosyalarını Çalıştır hakkında daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama ve kullanma](how-to-set-up-training-targets.md#create-run-configuration-and-submit-run-using-azure-machine-learning-cli).

@No__t-0 parametresi bu çalıştırmaya bir JSON dosyasında bir başvuru depolar ve sonraki adımlarda modeli kaydetmek ve indirmek için kullanılır.

Eğitim işlemleri sırasında, uzaktan işlem kaynağındaki eğitim oturumundan bilgi akışı. Bilgilerin bir kısmı aşağıdaki metne benzer:

```text
alpha is 0.80, and mse is 3340.02
alpha is 0.85, and mse is 3349.36
alpha is 0.90, and mse is 3359.09
alpha is 0.95, and mse is 3369.13


The experiment completed successfully. Finalizing run...
Cleaning up all outstanding Run operations, waiting 300.0 seconds
```

Bu metin eğitim betiğinin (`train-sklearn.py`) günlüğe kaydedilir ve bu modelin performans ölçümlerinin ikisini görüntüler. Bu durumda, modelin en yüksek alfa değeri olmasını istiyoruz. Performans ölçümleri, eğitiminde olduğunuz modele özgüdür. Diğer modeller farklı performans ölçümlerine sahip olacaktır.

@No__t-0 ' ı inceleyerek, eğitilen modelleri dosya olarak depoladığında Alfa değerini de kullandığını fark edeceksiniz. Bu durumda, çeşitli modeller de vardır. En yüksek Alpha ile olan en iyi bir değer olmalıdır. Yukarıdaki çıktıyı ve 0,95 Alpha içeren model @no__t 0 olarak kaydedilmiş olduğunu bakıyor.

Model, eğitilen işlem hedefinde `./outputs` dizinine kaydedildi. Bu durumda, Azure bulutu 'nda Azure Machine Learning Işlem örneği. Eğitim süreci, Azure Machine Learning çalışma alanınıza eğitim gerçekleştiği işlem hedefinden `./outputs` dizininin içeriğini otomatik olarak yükler. Denemenin bir parçası olarak depolanır (Bu örnekteki `myexperiment`).

## <a name="register-the-model"></a>Modeli kaydedin

Modeli denemenizin içindeki saklı sürümden doğrudan kaydetmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml model register -n mymodel -f runoutput.json --asset-path "outputs/ridge_0.95.pkl" -t registeredmodel.json
```

Bu komut, eğitim çalışması tarafından oluşturulan `outputs/ridge_0.95.pkl` dosyasını `mymodel` adlı yeni bir model kaydı olarak kaydeder. @No__t-0, denemelerle bir yola başvurur. Bu durumda, deneme ve çalıştırma bilgileri, eğitim komutu tarafından oluşturulan `runoutput.json` dosyasından yüklenir. @No__t-0, bu komut tarafından oluşturulan yeni kayıtlı modele başvuran ve kayıtlı modellerle çalışan diğer CLı komutları tarafından kullanılan bir JSON dosyası oluşturur.

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
az ml model register -n mymodel -p "ridge_0.95.pkl"
```

İlk komut kayıtlı modeli geçerli dizine indirir. Dosya adı, modeli kaydettiğinizde başvurulan dosya olan `ridge_0.95.pkl` ' dır. İkinci komut, yerel modeli (`-p "ridge_0.95.pkl"`) önceki kayıtla aynı adla (`mymodel`) kaydeder. Bu kez, döndürülen JSON verisi sürümü 2 olarak listeler.

## <a name="deploy-the-model"></a>Modeli dağıtma

Bir modeli dağıtmak için dizinleri `model-deployment` dizinine değiştirin ve ardından aşağıdaki komutu kullanın:

```azurecli-interactive
cd ~/mlops/model-deployment
az ml model deploy -n myservice -m "mymodel:1" --ic inferenceConfig.yml --dc aciDeploymentConfig.yml
```

"Docker istemcisi oluşturulamadı" iletisini alabilirsiniz. Bu iletiyi yoksayabilirsiniz. CLı, yerel bir Docker kapsayıcısına Web hizmeti dağıtabilir ve Docker için denetim yapabilir. Bu durumda, yerel bir dağıtım kullanmıyoruz.

Bu komut, daha önce kaydettiğiniz modelin 1. sürümünü kullanarak `myservice` adlı yeni bir hizmet dağıtır.

@No__t-0 dosyası, giriş betiği (`score.py`) ve yazılım bağımlılıkları gibi çıkarım gerçekleştirme hakkında bilgi sağlar. Bu dosyanın yapısı hakkında daha fazla bilgi için, bkz. [çıkarım yapılandırma şeması](reference-azure-machine-learning-cli.md#inference-configuration-schema). Giriş betikleri hakkında daha fazla bilgi için bkz. [Azure Machine Learning hizmeti ile modelleri dağıtma](how-to-deploy-and-where.md#prepare-to-deploy).

@No__t-0, hizmeti barındırmak için kullanılan dağıtım ortamını açıklar. Dağıtım yapılandırması, dağıtım için kullandığınız işlem türüne özeldir. Bu durumda, bir Azure Kapsayıcı örneği kullanılır. Daha fazla bilgi için bkz. [dağıtım yapılandırma şeması](reference-azure-machine-learning-cli.md#deployment-configuration-schema).

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

Bu komut, `scoringUri` de dahil olmak üzere aynı JSON belgesini döndürür.

REST uç noktası, hizmete veri göndermek için kullanılabilir. Hizmete veri gönderen bir istemci uygulaması oluşturma hakkında bilgi için bkz. [Web hizmeti olarak dağıtılan Azure Machine Learning modelini](how-to-consume-web-service.md) kullanma

### <a name="send-data-to-the-service"></a>Verileri hizmete gönder

Uç noktasını çağırmak için bir istemci uygulaması oluşturabilirsiniz, ancak Machine Learning CLı, test istemcisi olarak davranabilen bir yardımcı program sağlar. Hizmete test verileri göndermek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml service run -n myservice -d '{"data":[[1,2,3,4,5,6,7,8,9,10]]}'
```

Komuttan gelen yanıt `[4684.920839774082]` ' a benzerdir.

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

Azure Machine Learning çalışma alanını kullanmaya devam etmek istiyorsanız, ancak eğitim için oluşturulan `cpu` işlem hedefini kullanarak kurtursanız, aşağıdaki komutu kullanın:

```azurecli-interactive
az ml computetarget delete -n cpu
```

Bu komut, silinen işlem hedefinin KIMLIĞINI içeren bir JSON belgesi döndürür. İşlem hedefi silinmeden önce bu işlem birkaç dakika sürebilir.

### <a name="delete-everything"></a>Her şeyi sil

Oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, ek ücret ödemeniz için bunları silin.

Kaynak grubunu ve bu belgede oluşturulan tüm Azure kaynaklarını silmek için aşağıdaki komutu kullanın. @No__t-0 ' yı daha önce oluşturduğunuz kaynak grubunun adıyla değiştirin:

```azurecli-interactive
az group delete -g <resource-group-name> -y
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure Machine Learning öğreticide, aşağıdaki görevler için Machine Learning CLı 'yi kullandınız:

> [!div class="checklist"]
> * Machine Learning uzantısını yükler
> * Bir Azure Machine Learning çalışma alanı oluşturma
> * Modeli eğitmek için kullanılan işlem kaynağını oluşturma
> * Eğitim çalıştırması başlatma
> * Bir modeli kaydetme ve indirme
> * Modeli bir Web hizmeti olarak dağıtma
> * Web hizmetini kullanarak veri puanı

CLı kullanma hakkında daha fazla bilgi için bkz. [Azure Machine Learning IÇIN CLI uzantısını kullanma](reference-azure-machine-learning-cli.md).