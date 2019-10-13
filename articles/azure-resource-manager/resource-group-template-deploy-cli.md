---
title: Azure CLı ve şablonuyla kaynak dağıtma | Microsoft Docs
description: Azure 'a kaynak dağıtmak için Azure Resource Manager ve Azure CLı kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: tomfitz
ms.openlocfilehash: c5a07d8b52e83215b2fdc220d76557ca45e1eae9
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286021"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma

Bu makalede, Azure CLı 'yi Azure 'a dağıtmak için Kaynak Yöneticisi şablonlarıyla Azure CLı 'nın nasıl kullanılacağı açıklanmaktadır. Azure çözümlerinizi dağıtma ve yönetme kavramlarını bilmiyorsanız bkz. [Azure Resource Manager genel bakış](resource-group-overview.md).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Azure CLı yüklü değilse, [Cloud Shell](#deploy-template-from-cloud-shell)kullanabilirsiniz.

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınızı bir Azure aboneliğine ya da bir abonelik içindeki bir kaynak grubuna hedefleyebilirsiniz. Çoğu durumda, dağıtımı bir kaynak grubuna hedefleyebilirsiniz. Abonelik genelinde ilke ve rol atamaları uygulamak için abonelik dağıtımlarını kullanın. Ayrıca, abonelik dağıtımlarını bir kaynak grubu oluşturmak ve kaynakları dağıtmak için de kullanabilirsiniz. Dağıtımın kapsamına bağlı olarak, farklı komutlar kullanırsınız.

Bir **kaynak grubuna**dağıtmak için [az Group Deployment Create](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)kullanın:

```azurecli
az group deployment create --resource-group <resource-group-name> --template-file <path-to-template>
```

Bir **aboneliğe**dağıtmak için [az Deployment Create](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create)kullanın:

```azurecli
az deployment create --location <location> --template-file <path-to-template>
```

Şu anda yönetim grubu dağıtımları yalnızca REST API aracılığıyla desteklenir. Bkz. [Kaynak Yöneticisi şablonları ve Kaynak Yöneticisi REST API ile kaynak dağıtma](resource-group-template-deploy-rest.md).

Bu makaledeki örnekler, kaynak grubu dağıtımlarını kullanır. Abonelik dağıtımları hakkında daha fazla bilgi için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](deploy-to-subscription.md).

## <a name="deploy-local-template"></a>Yerel şablon dağıt

Azure 'a kaynak dağıttığınızda şunları yapabilirsiniz:

1. Azure hesabınızda oturum açma
2. Dağıtılan kaynaklar için kapsayıcı görevi gören bir kaynak grubu oluşturun. Kaynak grubunun adı yalnızca alfasayısal karakterler, noktalar, alt çizgiler, kısa çizgiler ve parantezler içerebilir. En fazla 90 karakter olabilir. Nokta ile bitemez.
3. Oluşturulacak kaynakları tanımlayan şablonu kaynak grubuna dağıtın

Bir şablon, dağıtımı özelleştirmenizi sağlayan parametreler içerebilir. Örneğin, belirli bir ortam için uyarlanmış değerler (geliştirme, test ve üretim gibi) sağlayabilirsiniz. Örnek şablon, depolama hesabı SKU 'SU için bir parametre tanımlar.

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren bir ileti görürsünüz:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

Kaynak Yöneticisi şablonlarını yerel makinenizde depolamak yerine, bunları bir dış konumda depolamayı tercih edebilirsiniz. Şablonları bir kaynak denetimi deposunda (GitHub gibi) saklayabilirsiniz. Ya da, bunları kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında saklayabilirsiniz.

Dış şablon dağıtmak için, **şablon-URI** parametresini kullanın. Örnek şablonu GitHub 'dan dağıtmak için örnekteki URI 'yi kullanın.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

Önceki örnekte, şablon için genel olarak erişilebilir bir URI gerekir ve bu, şablonunuz önemli verileri içermemelidir. Gizli veriler (yönetici parolası gibi) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Ancak, şablonunuzun herkese açık bir şekilde erişilebilir olmasını istemiyorsanız, bunu özel bir depolama kapsayıcısında depolayarak koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Cloud Shell aşağıdaki komutları kullanın:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az group deployment create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="parameters"></a>Parametreler

Parametre değerlerini geçirmek için satır içi parametreleri ya da bir parametre dosyasını kullanabilirsiniz.

### <a name="inline-parameters"></a>Satır içi parametreler

Satır içi parametreleri geçirmek için `parameters` ' daki değerleri sağlayın. Örneğin, bir dizeye dize ve dizi geçirmek bir bash kabuğudur, şunu kullanın:

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Windows komut Istemi (CMD) veya PowerShell ile Azure CLı kullanıyorsanız, diziyi şu biçimde geçirin: `exampleArray="['value1','value2']"`.

Ayrıca dosyanın içeriğini alabilir ve bu içeriği satır içi bir parametre olarak sağlayabilirsiniz.

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Bir dosyadan parametre değeri alma, yapılandırma değerleri sağlamanız gerektiğinde faydalıdır. Örneğin, [bir Linux sanal makinesi için Cloud-init değerleri](../virtual-machines/linux/using-cloud-init.md)sağlayabilirsiniz.

ArrayContent. JSON biçimi:

```json
[
    "value1",
    "value2"
]
```

### <a name="parameter-files"></a>Parametre dosyaları

Komut dosyanıza satır içi değer olarak parametre geçirmek yerine, parametre değerlerini içeren bir JSON dosyası kullanmayı daha kolay bulabilirsiniz. Parametre dosyası yerel bir dosya olmalıdır. Dış parametre dosyaları Azure CLı ile desteklenmez.

Parametre dosyası hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi parametre dosyası oluşturma](resource-manager-parameter-files.md).

Yerel bir parametre dosyasını geçirmek için, `@` kullanarak Storage. Parameters. JSON adlı yerel bir dosya belirtin.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

## <a name="handle-extended-json-format"></a>Genişletilmiş JSON biçimini işle

Çok satırlı dizeler veya açıklamalarla bir şablon dağıtmak için `--handle-extended-json-format` anahtarını kullanmanız gerekir.  Örnek:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  "apiVersion": "2018-10-01",
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

## <a name="test-a-template-deployment"></a>Şablon dağıtımını test etme

Şablon ve parametre değerlerinizi hiçbir kaynağı gerçekten dağıtmadan test etmek için [az Group Deployment Validate](/cli/azure/group/deployment#az-group-deployment-validate)kullanın.

```azurecli-interactive
az group deployment validate \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Herhangi bir hata algılanmazsa, komut test dağıtımı hakkında bilgi döndürür. Özellikle, **hata** değerinin null olduğuna dikkat edin.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Bir hata algılanırsa, komut bir hata mesajı döndürür. Örneğin, depolama hesabı SKU 'SU için yanlış bir değer geçirilerek aşağıdaki hata döndürülür:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Şablonunuzda bir sözdizimi hatası varsa, komut, şablonu ayrıştıramadığını belirten bir hata döndürür. İleti, ayrıştırma hatasının satır numarasını ve konumunu gösterir.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Hata aldığınızda başarılı bir dağıtıma geri dönmek için, [başarılı bir dağıtımda hata durumunda geri alma](rollback-on-error.md)konusuna bakın.
- Kaynak grubunda var olan, ancak şablonda tanımlanmamış kaynakların nasıl işleneceğini belirtmek için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).
- Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).
- Yaygın dağıtım hatalarını çözümlemeye yönelik ipuçları için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
- SAS belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](resource-manager-cli-sas-token.md).
- Hizmetinizi birden fazla bölgeye güvenle kullanıma almak için bkz. [Azure dağıtım Yöneticisi](deployment-manager-overview.md).
