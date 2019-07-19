---
title: Azure CLı ve şablonuyla kaynak dağıtma | Microsoft Docs
description: Azure 'a kaynak dağıtmak için Azure Resource Manager ve Azure CLı kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/12/2019
ms.author: tomfitz
ms.openlocfilehash: 93b1b16776bac6cb24996d6fa08a547318802f32
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67853840"
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

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden Dağıt

Bu özellik *hata durumunda geri alma*olarak da bilinir. Bir dağıtım başarısız olduğunda, dağıtım geçmişinizden daha önceki ve başarılı bir dağıtımı otomatik olarak yeniden dağıtabilirsiniz. Yeniden dağıtımı belirtmek için dağıtım komutundaki `--rollback-on-error` parametresini kullanın. Bu işlevsellik, altyapı dağıtımınız için bilinen iyi bir durumanız varsa ve bu duruma dönmek istiyorsanız yararlıdır. Çeşitli uyarılar ve kısıtlamalar vardır:

- Yeniden dağıtım, daha önce aynı parametrelerle çalıştırıldığı için tam olarak çalıştırılır. Parametreleri değiştiremezsiniz.
- Önceki dağıtım, [Tüm modu](./deployment-modes.md#complete-mode)kullanılarak çalıştırılır. Önceki dağıtıma dahil olmayan tüm kaynaklar silinir ve tüm kaynak konfigürasyonları önceki durumlarına ayarlanır. [Dağıtım modlarını](./deployment-modes.md)tam olarak anladığınızdan emin olun.
- Yeniden dağıtım yalnızca kaynakları etkiler, tüm veri değişiklikleri etkilenmez.
- Bu özellik yalnızca kaynak grubu dağıtımlarında desteklenir, abonelik düzeyinde dağıtımlar değildir. Abonelik düzeyi dağıtımı hakkında daha fazla bilgi için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](./deploy-to-subscription.md).

Bu seçeneği kullanmak için, dağıtımlarınızın geçmişte tanımlanabilmeleri için benzersiz adlara sahip olması gerekir. Benzersiz adlarınız yoksa geçerli başarısız dağıtım, geçmişte daha önce başarılı olan dağıtımın üzerine yazabilir. Bu seçeneği yalnızca kök düzeyinde dağıtımlar ile kullanabilirsiniz. İç içe geçmiş bir şablondan dağıtımlar yeniden dağıtım için kullanılamaz.

Son başarılı dağıtımı yeniden dağıtmak için `--rollback-on-error` parametreyi bir bayrak olarak ekleyin.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Belirli bir dağıtımı yeniden dağıtmak için `--rollback-on-error` parametresini kullanın ve dağıtımın adını sağlayın.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

Belirtilen dağıtım başarılı olmalıdır.

## <a name="parameters"></a>Parametreler

Parametre değerlerini geçirmek için satır içi parametreleri ya da bir parametre dosyasını kullanabilirsiniz. Bu makaledeki Yukarıdaki örneklerde satır içi parametreler gösterilmektedir.

### <a name="inline-parameters"></a>Satır içi parametreler

Satır içi parametreleri geçirmek için içindeki `parameters`değerleri sağlayın. Örneğin, bir dizeye dize ve dizi geçirmek bir bash kabuğudur, şunu kullanın:

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

Parametre dosyası aşağıdaki biçimde olmalıdır:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Parametreler bölümünün, şablonunuzda tanımlanan parametreyle (storageAccountType) eşleşen bir parametre adı içerdiğine dikkat edin. Parametre dosyası, parametre için bir değer içerir. Bu değer, dağıtım sırasında otomatik olarak şablona geçirilir. Birden fazla parametre dosyası oluşturabilir ve sonra senaryo için uygun parametre dosyasını geçirebilirsiniz. 

Önceki örneği kopyalayın ve adlı `storage.parameters.json`bir dosya olarak kaydedin.

Yerel bir parametre dosyasını geçirmek için, depolama `@` . Parameters. JSON adlı yerel bir dosya belirtmek için öğesini kullanın.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

### <a name="parameter-precedence"></a>Parametre önceliği

Aynı dağıtım işleminde satır içi parametreleri ve yerel bir parametre dosyasını kullanabilirsiniz. Örneğin, yerel parametre dosyasında bazı değerler belirtebilir ve dağıtım sırasında satır içi diğer değerleri ekleyebilirsiniz. Hem yerel parametre dosyasında hem de satır içi bir parametre için değerler sağlarsanız, satır içi değer önceliklidir.

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters @demotemplate.parameters.json \
  --parameters exampleArray=@arrtest.json
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

- Bu makaledeki örneklerde, varsayılan aboneliğinizde kaynakları bir kaynak grubuna dağıtır. Farklı bir abonelik kullanmak için bkz. [birden çok Azure aboneliğini yönetme](/cli/azure/manage-azure-subscriptions-azure-cli).
- Kaynak grubunda var olan, ancak şablonda tanımlanmamış kaynakların nasıl işleneceğini belirtmek için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).
- Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).
- Yaygın dağıtım hatalarını çözümlemeye yönelik ipuçları için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
- SAS belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](resource-manager-cli-sas-token.md).
- Hizmetinizi birden fazla bölgeye güvenle kullanıma almak için bkz. [Azure dağıtım Yöneticisi](deployment-manager-overview.md).
