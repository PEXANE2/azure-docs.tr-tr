---
title: Azure CLı ve şablonuyla kaynak dağıtma
description: Azure 'a kaynak dağıtmak için Azure Resource Manager ve Azure CLı kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
ms.topic: conceptual
ms.date: 06/04/2020
ms.openlocfilehash: a2a1c1fe63d0a841f57407ed5402d7ddca3fcea4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84432071"
---
# <a name="deploy-resources-with-arm-templates-and-azure-cli"></a>ARM şablonları ve Azure CLı ile kaynak dağıtma

Bu makalede, Azure CLı 'yı Azure Resource Manager (ARM) şablonlarıyla kullanarak kaynaklarınızı Azure 'a nasıl dağıtabileceğiniz açıklanır. Azure çözümlerinizi dağıtma ve yönetme kavramlarını bilmiyorsanız, bkz. [şablon dağıtımına genel bakış](overview.md).

Azure CLı sürüm 2.2.0 'de dağıtım komutları değişmiştir. Bu makaledeki örneklerde Azure CLı sürüm 2.2.0 veya üzeri gerekir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

Azure CLı yüklü değilse, [Cloud Shell](#deploy-template-from-cloud-shell)kullanabilirsiniz.

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınızı bir kaynak grubuna, aboneliğe, yönetim grubuna veya kiracıya hedefleyebilirsiniz. Çoğu durumda, dağıtımı bir kaynak grubuna hedefleyebilirsiniz. Daha büyük bir kapsamda ilke ve rol atamaları uygulamak için abonelik, yönetim grubu veya kiracı dağıtımlarını kullanın. Bir aboneliğe dağıtırken, bir kaynak grubu oluşturabilir ve kaynakları buna dağıtabilirsiniz.

Dağıtımın kapsamına bağlı olarak, farklı komutlar kullanırsınız.

* Bir **kaynak grubuna**dağıtmak için [az Deployment Group Create](/cli/azure/deployment/group?view=azure-cli-latest#az-deployment-group-create)kullanın:

  ```azurecli-interactive
  az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
  ```

* Bir **aboneliğe**dağıtmak için [az Deployment Sub Create](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-create)kullanın:

  ```azurecli-interactive
  az deployment sub create --location <location> --template-file <path-to-template>
  ```

  Abonelik düzeyi dağıtımları hakkında daha fazla bilgi için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](deploy-to-subscription.md).

* Bir **yönetim grubuna**dağıtmak için [az Deployment mg Create](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-create)şunu kullanın:

  ```azurecli-interactive
  az deployment mg create --location <location> --template-file <path-to-template>
  ```

  Yönetim grubu düzeyi dağıtımları hakkında daha fazla bilgi için bkz. [Yönetim grubu düzeyinde kaynak oluşturma](deploy-to-management-group.md).

* Bir **kiracıya**dağıtmak için [az Deployment Tenant Create](/cli/azure/deployment/tenant?view=azure-cli-latest#az-deployment-tenant-create)kullanın:

  ```azurecli-interactive
  az deployment tenant create --location <location> --template-file <path-to-template>
  ```

  Kiracı düzeyinde dağıtımlar hakkında daha fazla bilgi için bkz. [kiracı düzeyinde kaynak oluşturma](deploy-to-tenant.md).

Bu makaledeki örnekler, kaynak grubu dağıtımlarını kullanır.

## <a name="deploy-local-template"></a>Yerel şablon dağıt

Azure 'a kaynak dağıttığınızda şunları yapabilirsiniz:

1. Azure hesabınızda oturum açma
2. Dağıtılan kaynaklar için kapsayıcı görevi gören bir kaynak grubu oluşturun. Kaynak grubunun adı yalnızca alfasayısal karakterler, noktalar, alt çizgiler, kısa çizgiler ve parantezler içerebilir. En fazla 90 karakter olabilir. Nokta ile bitemez.
3. Oluşturulacak kaynakları tanımlayan şablonu kaynak grubuna dağıtın

Bir şablon, dağıtımı özelleştirmenizi sağlayan parametreler içerebilir. Örneğin, belirli bir ortam için uyarlanmış değerler (geliştirme, test ve üretim gibi) sağlayabilirsiniz. Örnek şablon, depolama hesabı SKU 'SU için bir parametre tanımlar.

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren bir ileti görürsünüz:

```output
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

ARM şablonlarını yerel makinenizde depolamak yerine, bunları bir dış konumda depolamayı tercih edebilirsiniz. Şablonları bir kaynak denetimi deposunda (GitHub gibi) saklayabilirsiniz. Ya da, bunları kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında saklayabilirsiniz.

Dış şablon dağıtmak için, **şablon-URI** parametresini kullanın. Örnek şablonu GitHub 'dan dağıtmak için örnekteki URI 'yi kullanın.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

Önceki örnekte, şablon için genel olarak erişilebilir bir URI gerekir ve bu, şablonunuz önemli verileri içermemelidir. Gizli veriler (yönetici parolası gibi) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Ancak, şablonunuzun herkese açık bir şekilde erişilebilir olmasını istemiyorsanız, bunu özel bir depolama kapsayıcısında depolayarak koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](secure-template-with-sas-token.md).

## <a name="preview-changes"></a>Değişiklikleri Önizle

Şablonunuzu dağıtmadan önce, şablonun ortamınızda yapacağız değişiklikleri önizleyebilirsiniz. Şablonun beklediğiniz değişiklikleri yaptığını doğrulamak için [ne yapılır işlemini](template-deploy-what-if.md) kullanın. Ayrıca, şablonu hatalara yönelik olarak doğrular.

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../../includes/resource-manager-cloud-shell-deploy.md)]

Cloud Shell aşağıdaki komutları kullanın:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az deployment group create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="parameters"></a>Parametreler

Parametre değerlerini geçirmek için satır içi parametreleri ya da bir parametre dosyasını kullanabilirsiniz.

### <a name="inline-parameters"></a>Satır içi parametreler

Satır içi parametreleri geçirmek için içindeki değerleri sağlayın `parameters` . Örneğin, bir dizeye dize ve dizi geçirmek bir bash kabuğudur, şunu kullanın:

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Windows komut Istemi (CMD) veya PowerShell ile Azure CLı kullanıyorsanız, diziyi şu biçimde geçirin: `exampleArray="['value1','value2']"` .

Ayrıca dosyanın içeriğini alabilir ve bu içeriği satır içi bir parametre olarak sağlayabilirsiniz.

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Bir dosyadan parametre değeri alma, yapılandırma değerleri sağlamanız gerektiğinde faydalıdır. Örneğin, [bir Linux sanal makinesi için Cloud-init değerleri](../../virtual-machines/linux/using-cloud-init.md)sağlayabilirsiniz.

Biçimdeki arrayContent.js:

```json
[
    "value1",
    "value2"
]
```

### <a name="parameter-files"></a>Parametre dosyaları

Komut dosyanıza satır içi değer olarak parametre geçirmek yerine, parametre değerlerini içeren bir JSON dosyası kullanmayı daha kolay bulabilirsiniz. Parametre dosyası yerel bir dosya olmalıdır. Dış parametre dosyaları Azure CLı ile desteklenmez.

Parametre dosyası hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi parametre dosyası oluşturma](parameter-files.md).

Yerel bir parametre dosyasını geçirmek için, `@` üzerinde storage.parameters.jsadlı yerel bir dosya belirtmek için kullanın.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

## <a name="handle-extended-json-format"></a>Genişletilmiş JSON biçimini işle

Azure CLı 'yi sürüm 2.3.0 veya daha eski bir sürümle kullanarak çok satırlı dizeler veya yorumlarla bir şablon dağıtmak için anahtarını kullanmanız gerekir `--handle-extended-json-format` .  Örneğin:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

## <a name="next-steps"></a>Sonraki adımlar

- Hata aldığınızda başarılı bir dağıtıma geri dönmek için, [başarılı bir dağıtımda hata durumunda geri alma](rollback-on-error.md)konusuna bakın.
- Kaynak grubunda var olan, ancak şablonda tanımlanmamış kaynakların nasıl işleneceğini belirtmek için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).
- Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
- Yaygın dağıtım hatalarını çözümlemeye yönelik ipuçları için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](common-deployment-errors.md).
- SAS belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](secure-template-with-sas-token.md).
