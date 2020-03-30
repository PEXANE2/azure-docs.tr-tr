---
title: Azure CLI ve şablonuyla kaynakları dağıtma
description: Kaynakları Azure'a dağıtmak için Azure Kaynak Yöneticisi'ni ve Azure CLI'yi kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: 241b84bc7b8c0b213e74cd7ee5f3d7668fe0d808
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282656"
---
# <a name="deploy-resources-with-arm-templates-and-azure-cli"></a>ARM şablonları ve Azure CLI ile kaynakları dağıtma

Bu makalede, kaynaklarınızı Azure'a dağıtmak için Azure Kaynak Yöneticisi (ARM) şablonlarıyla Azure CLI'nin nasıl kullanılacağı açıklanmaktadır. Azure çözümlerinizi dağıtma ve yönetme kavramlarını bilmiyorsanız, [şablon dağıtımına genel bakış](overview.md)bölümüne bakın.

Azure CLI sürüm 2.2.0'da dağıtım komutları değiştirildi. Bu makaledeki örnekler, Azure CLI sürüm 2.2.0 veya sonrası gerektirir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

Azure CLI yüklü [değilseniz, Bulut Kabuğu'nu](#deploy-template-from-cloud-shell)kullanabilirsiniz.

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınızı bir kaynak grubuna, abonelike, yönetim grubuna veya kiracıya hedefleyebilirsiniz. Çoğu durumda, bir kaynak grubuna dağıtım hedefleceksiniz. İlkeleri ve rol ödevlerini daha geniş bir kapsamda uygulamak için abonelik, yönetim grubu veya kiracı dağıtımlarını kullanın. Bir aboneye dağıtılırken, bir kaynak grubu oluşturabilir ve ona kaynak dağıtabilirsiniz.

Dağıtımın kapsamına bağlı olarak, farklı komutlar kullanırsınız.

Bir kaynak **grubuna**dağıtmak için az [dağıtım grubu oluşturma](/cli/azure/deployment/group?view=azure-cli-latest#az-deployment-group-create)kullanın:

```azurecli-interactive
az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
```

Bir **aboneğe**dağıtmak için, [az dağıtım alt oluşturma](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-create)kullanın:

```azurecli-interactive
az deployment sub create --location <location> --template-file <path-to-template>
```

Abonelik düzeyi dağıtımları hakkında daha fazla bilgi için [bkz.](deploy-to-subscription.md)

Bir yönetim **grubuna**dağıtmak için, [az dağıtım mg oluşturmak](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-create)kullanın:

```azurecli-interactive
az deployment mg create --location <location> --template-file <path-to-template>
```

Yönetim grubu düzeyindeki dağıtımlar hakkında daha fazla bilgi için [bkz.](deploy-to-management-group.md)

**Kiracıya**dağıtmak için az [dağıtım kiracı oluşturmak](/cli/azure/deployment/tenant?view=azure-cli-latest#az-deployment-tenant-create)kullanın:

```azurecli-interactive
az deployment tenant create --location <location> --template-file <path-to-template>
```

Kiracı düzeyindeki dağıtımlar hakkında daha fazla bilgi için [bkz.](deploy-to-tenant.md)

Bu makaledeki örneklerkaynak grubu dağıtımlarını kullanır.

## <a name="deploy-local-template"></a>Yerel şablonu dağıtma

Kaynakları Azure'a dağıtırken şunları yapacaksınız:

1. Azure hesabınızda oturum açma
2. Dağıtılan kaynaklar için kapsayıcı olarak hizmet veren bir kaynak grubu oluşturun. Kaynak grubunun adı yalnızca alfasayısal karakterler, dönemler, alt çizerler, tireler ve parantez içerebilir. 90 karaktere kadar olabilir. Bir dönemde bitmez.
3. Oluşturulacak kaynakları tanımlayan şablonu kaynak grubuna dağıtma

Şablon, dağıtımı özelleştirmenize olanak tanıyan parametreler içerebilir. Örneğin, belirli bir ortama (dev, test ve üretim gibi) uygun değerler sağlayabilirsiniz. Örnek şablon, depolama hesabı SKU için bir parametre tanımlar.

Aşağıdaki örnek bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Bittiğinde, sonucu içeren bir ileti görürsünüz:

```output
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

ARM şablonlarını yerel makinenizde depolamak yerine, bunları harici bir konumda saklamayı tercih edebilirsiniz. Şablonları kaynak denetim deposunda (GitHub gibi) depolayabilirsiniz. Veya, kuruluşunuzdaki paylaşılan erişim için bunları bir Azure depolama hesabında saklayabilirsiniz.

Harici bir şablon dağıtmak için **şablon-uri** parametresini kullanın. Örnek şablonu GitHub'dan dağıtmak için uri'yi örnekte kullanın.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

Önceki örnek, şablonunuz hassas verileri içermemelidir, çünkü çoğu senaryo için çalışır şablon için kamuya açık bir URI gerektirir. Hassas verileri (yönetici parolası gibi) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Ancak, şablonunuzun herkese açık olmasını istemiyorsanız, şablonu özel bir depolama kapsayıcısında depolayarak şablonu koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren bir şablonu dağıtma hakkında bilgi [için](secure-template-with-sas-token.md)bkz.

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../../includes/resource-manager-cloud-shell-deploy.md)]

Bulut Kabuğu'nda aşağıdaki komutları kullanın:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az deployment group create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="parameters"></a>Parametreler

Parametre değerlerini geçmek için satır içinde parametreler ilerler veya parametre dosyasını kullanabilirsiniz.

### <a name="inline-parameters"></a>Satır dışı parametreler

Satır dışı parametreleri geçirmek için, 'deki `parameters`değerleri sağlayın Örneğin, bir dize ve diziyi şablona geçirmek için Bir Bash kabuğudur, kullanın:

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Windows Komut Komut Komut Ustem (CMD) veya PowerShell ile Azure CLI `exampleArray="['value1','value2']"`kullanıyorsanız, diziyi biçiminde geçirin: .

Ayrıca dosyanın içeriğini alabilir ve bu içeriği satır satırlı parametre olarak sağlayabilirsiniz.

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Yapılandırma değerleri sağlamanız gerektiğinde dosyadan parametre değeri almak yararlıdır. Örneğin, [bir Linux sanal makinesi için bulut-init değerleri](../../virtual-machines/linux/using-cloud-init.md)sağlayabilirsiniz.

ArrayContent.json biçimi:

```json
[
    "value1",
    "value2"
]
```

### <a name="parameter-files"></a>Parametre dosyaları

Komut dosyanızdaki satır değerleri olarak parametreleri geçirmek yerine, parametre değerlerini içeren bir JSON dosyasını kullanmayı daha kolay bulabilirsiniz. Parametre dosyası yerel bir dosya olmalıdır. Dış parametre dosyaları Azure CLI ile desteklenmez.

Parametre dosyası hakkında daha fazla bilgi için [kaynak yöneticisi parametre dosyası oluştur'a](parameter-files.md)bakın.

Yerel bir parametre dosyasını `@` geçmek için storage.parameters.json adlı yerel bir dosya belirtmek için kullanın.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

## <a name="handle-extended-json-format"></a>Genişletilmiş JSON biçimini işleme

Çok satırlı dizeleri veya açıklamaları içeren bir şablon `--handle-extended-json-format` dağıtmak için anahtarı kullanmanız gerekir.  Örnek:

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

## <a name="test-a-template-deployment"></a>Şablon dağıtımını test edin

Şablonunuzu ve parametre değerlerinizi gerçekten kaynak dağıtmadan sınamak için [az dağıtım grubu doğrulamayı](/cli/azure/group/deployment)kullanın.

```azurecli-interactive
az deployment group validate \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Hata algılanmazsa, komut test dağıtımı yla ilgili bilgileri döndürür. Özellikle, **hata** değerinin null olduğunu unutmayın.

```output
{
  "error": null,
  "properties": {
      ...
```

Bir hata algılanırsa, komut bir hata iletisi döndürür. Örneğin, depolama hesabı SKU için yanlış bir değer geçen, aşağıdaki hatayı döndürür:

```output
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

Şablonunuzda sözdizimi hatası varsa, komut şablonu ayrışdıramayacağını belirten bir hata döndürür. İleti, ayrıştma hatasının satır numarasını ve konumunu gösterir.

```output
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

- Bir hata aldığınızda başarılı bir dağıtıma geri dönmek [için, başarılı dağıtımhatasında Rollback'i](rollback-on-error.md)görün.
- Kaynak grubunda bulunan ancak şablonda tanımlanmayan kaynakların nasıl işleyeceğini belirtmek için Azure [Kaynak Yöneticisi dağıtım modlarına](deployment-modes.md)bakın.
- Şablonunuzdaparametrelerin nasıl tanımlandığını anlamak için [bkz.](template-syntax.md)
- Sık kullanılan dağıtım hatalarını çözme yle ilgili ipuçları için, [Azure Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](common-deployment-errors.md)sorununa bakın.
- SAS belirteci gerektiren bir şablonu dağıtma hakkında bilgi [için](secure-template-with-sas-token.md)bkz.
- Hizmetinizi birden fazla bölgeye güvenli bir şekilde kullanıma sunmak için [Azure Dağıtım Yöneticisi'ne](deployment-manager-overview.md)bakın.
