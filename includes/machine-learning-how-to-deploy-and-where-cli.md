---
author: gvashishtha
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: include
ms.date: 07/31/2020
ms.author: gopalv
ms.openlocfilehash: 38b346b4296ea72f964717220adf762e8d0d6d91
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542830"
---
## <a name="prerequisites"></a>Önkoşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](../articles/machine-learning/how-to-manage-workspace.md).
- Bir model. Eğitilen bir modeliniz yoksa, [Bu öğreticide](https://aka.ms/azml-deploy-cloud)verilen model ve bağımlılık dosyalarını kullanabilirsiniz.
- [Machine Learning hizmeti Için Azure komut satırı arabirimi (CLI) uzantısı](../articles/machine-learning/reference-azure-machine-learning-cli.md)


## <a name="connect-to-your-workspace"></a>Çalışma alanınıza bağlanma

[Abonelik bağlamını ayarlamak](/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription)IÇIN Azure CLI belgelerindeki yönergeleri izleyin.

Ardından şunları yapın:

```azurecli-interactive
az ml workspace list --resource-group=<my resource group>
```

erişiminiz olan çalışma alanlarını görmek için.

## <a name="register-your-model"></a><a id="registermodel"></a>Modelinizi kaydetme

Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları çalışma alanına tek bir model olarak kaydedebilirsiniz. Dosyaları kaydettikten sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kaydettiğiniz tüm dosyaları alabilirsiniz.

> [!TIP]
> Bir modeli kaydettiğinizde, bir bulut konumunun (bir eğitim çalıştırmasında) ya da yerel bir dizinin yolunu sağlarsınız. Bu yol, kayıt işleminin bir parçası olarak karşıya yüklenecek dosyaları bulmak için yeterlidir. Giriş betiğinde kullanılan yol ile eşleşmesi gerekmez. Daha fazla bilgi için bkz. [giriş betiğinizdeki model dosyalarını bulma](../articles/machine-learning/how-to-deploy-advanced-entry-script.md#load-registered-models).

Machine Learning modelleri Azure Machine Learning çalışma alanınıza kaydedilir. Model Azure Machine Learning veya herhangi bir yerden gelebilir. Bir modeli kaydederken, isteğe bağlı olarak model hakkında meta veriler sağlayabilirsiniz. `tags` `properties` Model kaydına uyguladığınız ve sözlükleri, modelleri filtrelemek için kullanılabilir.

Aşağıdaki örneklerde bir modelin nasıl kaydedileceği gösterilmektedir.

### <a name="register-a-model-from-an-azure-ml-training-run"></a>Azure ML eğitim çalıştırmasında bir modeli kaydetme

```azurecli-interactive
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid --tag area=mnist
```

[!INCLUDE [install extension](machine-learning-service-install-extension.md)]

`--asset-path`Parametresi, modelin bulut konumunu ifade eder. Bu örnekte, tek bir dosyanın yolu kullanılır. Model kaydına birden çok dosya eklemek için, `--asset-path` dosyaları içeren bir klasörün yoluna ayarlayın.

### <a name="register-a-model-from-a-local-file"></a>Yerel dosyadan model kaydetme

```azurecli-interactive
az ml model register -n onnx_mnist -p mnist/model.onnx
```

Model kaydına birden çok dosya eklemek için, `-p` dosyaları içeren bir klasörün yoluna ayarlayın.

Hakkında daha fazla bilgi için `az ml model register` [başvuru belgelerine](/cli/azure/ext/azure-cli-ml/ml/model)bakın.

## <a name="define-an-entry-script"></a>Giriş betiği tanımlama

[!INCLUDE [write entry script](machine-learning-entry-script.md)]

## <a name="define-an-inference-configuration"></a>Bir çıkarım yapılandırması tanımlama

[!INCLUDE [inference config](machine-learning-service-inference-config.md)]

Aşağıdaki komut, CLı kullanarak bir modelin nasıl dağıtılacağını göstermektedir:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

Bu örnekte, yapılandırma aşağıdaki ayarları belirtir:

* Modelin Python gerektirdiğini
* Dağıtılan hizmete gönderilen Web isteklerini işlemek için kullanılan [giriş betiği](#define-an-entry-script)
* Çıkarım için gereken Python paketlerini açıklayan Conda dosyası

Bir çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](../articles/machine-learning/how-to-deploy-custom-docker-image.md).

## <a name="choose-a-compute-target"></a>İşlem hedefi seçin

[!INCLUDE [aml-compute-target-deploy](aml-compute-target-deploy.md)]

## <a name="define-a-deployment-configuration"></a>Dağıtım yapılandırması tanımlama

Bir dağıtım yapılandırması için kullanılabilen seçenekler, seçtiğiniz işlem hedefine bağlı olarak farklılık gösterir.

[!INCLUDE [aml-local-deploy-config](machine-learning-service-local-deploy-config.md)]

Daha fazla bilgi için bkz. [az ml model Deploy](/cli/azure/ext/azure-cli-ml/ml/model#ext-azure-cli-ml-az-ml-model-deploy) belgeleri.

## <a name="deploy-your-model"></a>Modelinizi dağıtın

Artık modelinizi dağıtmaya hazırsınız.

### <a name="using-a-registered-model"></a>Kayıtlı model kullanma

Modelinize Azure Machine Learning çalışma alanınızda kaydolduysanız, "MyModel: 1" değerini modelinizin adı ve sürüm numarası ile değiştirin.

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
```

### <a name="using-a-local-model"></a>Yerel model kullanma

Modelinize kaydolmayı tercih ediyorsanız, modelinize sunulacak yerel bir dizin belirtmek için, üzerinde inferenceconfig.js"sourceDirectory" parametresini geçirebilirsiniz.

```azurecli-interactive
az ml model deploy --ic inferenceconfig.json --dc deploymentconfig.json
```
## <a name="delete-resources"></a>Kaynakları silme

Dağıtılan bir Web hizmetini silmek için kullanın `az ml service <name of webservice>` .

Çalışma alanınızdan kayıtlı bir modeli silmek için şunu kullanın`az ml model delete <model id>`

[Bir Web hizmetini silme](/cli/azure/ext/azure-cli-ml/ml/service#ext-azure-cli-ml-az-ml-service-delete) ve [bir modeli silme](/cli/azure/ext/azure-cli-ml/ml/model#ext-azure-cli-ml-az-ml-model-delete) hakkında daha fazla bilgi edinin