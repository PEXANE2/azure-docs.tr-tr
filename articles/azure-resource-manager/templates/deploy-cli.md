---
title: Azure CLı ve şablonuyla kaynak dağıtma
description: Azure 'a kaynak dağıtmak için Azure Resource Manager ve Azure CLı kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
ms.topic: conceptual
ms.date: 01/26/2021
ms.openlocfilehash: f01409db36ca29deb2f6938ce3118cdcb20cd1a1
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881308"
---
# <a name="deploy-resources-with-arm-templates-and-azure-cli"></a>ARM şablonları ve Azure CLı ile kaynak dağıtma

Bu makalede, Azure CLı 'yi Azure 'a dağıtmak için Azure Resource Manager şablonlar (ARM şablonları) ile birlikte nasıl kullanacağınız açıklanmaktadır. Azure çözümlerinizi dağıtma ve yönetme kavramlarını bilmiyorsanız, bkz. [şablon dağıtımına genel bakış](overview.md).

Azure CLı sürüm 2.2.0 'de dağıtım komutları değişmiştir. Bu makaledeki örneklerde Azure CLı sürüm 2.2.0 veya üzeri gerekir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

Azure CLı yüklü değilse, Azure Cloud Shell kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Cloud Shell ARM şablonları dağıtma](deploy-cloud-shell.md).

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınızı bir kaynak grubuna, aboneliğe, yönetim grubuna veya kiracıya hedefleyebilirsiniz. Dağıtımın kapsamına bağlı olarak, farklı komutlar kullanırsınız.

* Bir **kaynak grubuna** dağıtmak için [az Deployment Group Create](/cli/azure/deployment/group#az-deployment-group-create)kullanın:

  ```azurecli-interactive
  az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
  ```

* Bir **aboneliğe** dağıtmak için [az Deployment Sub Create](/cli/azure/deployment/sub#az-deployment-sub-create)kullanın:

  ```azurecli-interactive
  az deployment sub create --location <location> --template-file <path-to-template>
  ```

  Abonelik düzeyi dağıtımları hakkında daha fazla bilgi için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](deploy-to-subscription.md).

* Bir **yönetim grubuna** dağıtmak için [az Deployment mg Create](/cli/azure/deployment/mg#az-deployment-mg-create)şunu kullanın:

  ```azurecli-interactive
  az deployment mg create --location <location> --template-file <path-to-template>
  ```

  Yönetim grubu düzeyi dağıtımları hakkında daha fazla bilgi için bkz. [Yönetim grubu düzeyinde kaynak oluşturma](deploy-to-management-group.md).

* Bir **kiracıya** dağıtmak için [az Deployment Tenant Create](/cli/azure/deployment/tenant#az-deployment-tenant-create)kullanın:

  ```azurecli-interactive
  az deployment tenant create --location <location> --template-file <path-to-template>
  ```

  Kiracı düzeyinde dağıtımlar hakkında daha fazla bilgi için bkz. [kiracı düzeyinde kaynak oluşturma](deploy-to-tenant.md).

Her kapsam için, şablonu dağıtan kullanıcının kaynak oluşturmak için gerekli izinlere sahip olması gerekir.

## <a name="deploy-local-template"></a>Yerel şablonu dağıtma

Yerel makinenizden veya dışarıdan depolanan bir şablonu dağıtabilirsiniz. Bu bölümde yerel bir şablon dağıtımı açıklanmaktadır.

Mevcut olmayan bir kaynak grubuna dağıtıyorsanız, kaynak grubunu oluşturun. Kaynak grubunun adı yalnızca alfasayısal karakterler, noktalar, alt çizgiler, kısa çizgiler ve parantezler içerebilir. En fazla 90 karakter olabilir. Ad bir noktayla bitemez.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
```

Yerel bir şablon dağıtmak için `--template-file` dağıtım komutundaki parametresini kullanın. Aşağıdaki örnek ayrıca, şablondan gelen bir parametre değerinin nasıl ayarlanacağını gösterir.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file azuredeploy.json \
  --parameters storageAccountType=Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren bir ileti görürsünüz:

```output
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

ARM şablonlarını yerel makinenizde depolamak yerine, bunları bir dış konumda depolamayı tercih edebilirsiniz. Şablonları bir kaynak denetimi deposunda (GitHub gibi) saklayabilirsiniz. İsterseniz kuruluşunuzda paylaşılan erişim sağlamak için bir Azure depolama hesabı kullanabilirsiniz.

Mevcut olmayan bir kaynak grubuna dağıtıyorsanız, kaynak grubunu oluşturun. Kaynak grubunun adı yalnızca alfasayısal karakterler, noktalar, alt çizgiler, kısa çizgiler ve parantezler içerebilir. En fazla 90 karakter olabilir. Ad bir noktayla bitemez.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
```

Dış şablon kullanmak için `template-uri` parametresini kullanın.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

Önceki örnekte, şablon için genel olarak erişilebilir bir URI gerekir ve bu, şablonunuz önemli verileri içermemelidir. Gizli veriler (yönetici parolası gibi) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Bununla birlikte, şablona erişimi yönetmek istiyorsanız, [şablon](#deploy-template-spec)özelliklerini kullanmayı göz önünde bulundurun.

Bir depolama hesabında depolanan göreli yol ile uzak bağlantılı şablonlar dağıtmak için, `query-string` SAS belirtecini belirtmek için kullanın:

```azurepowershell
az deployment group create \
  --name linkedTemplateWithRelativePath \
  --resource-group myResourceGroup \
  --template-uri "https://stage20210126.blob.core.windows.net/template-staging/mainTemplate.json" \
  --query-string $sasToken
```

Daha fazla bilgi için bkz. [bağlantılı şablonlar için göreli yol kullanma](./linked-templates.md#linked-template).

## <a name="deployment-name"></a>Dağıtım adı

ARM şablonunu dağıttığınızda dağıtıma bir ad verebilirsiniz. Bu ad dağıtım geçmişinden dağıtım almanıza yardımcı olabilir. Dağıtım için bir ad sağlamazsanız, şablon dosyasının adı kullanılır. Örneğin, adlı bir şablonu dağıtırsanız `azuredeploy.json` ve dağıtım adı belirtmezseniz, dağıtım adlandırılır `azuredeploy` .

Bir dağıtımı her çalıştırdığınızda, kaynak grubunun dağıtım geçmişine dağıtım adı ile bir giriş eklenir. Başka bir dağıtım çalıştırırsanız ve aynı adı verirseniz, önceki girdi geçerli dağıtımla değiştirilmiştir. Dağıtım geçmişinde benzersiz girişleri sürdürmek istiyorsanız, her dağıtıma benzersiz bir ad verin.

Benzersiz bir ad oluşturmak için rastgele bir sayı atayabilirsiniz.

```azurecli-interactive
deploymentName='ExampleDeployment'$RANDOM
```

Veya bir tarih değeri ekleyin.

```azurecli-interactive
deploymentName='ExampleDeployment'$(date +"%d-%b-%Y")
```

Aynı dağıtım adına sahip aynı kaynak grubuna eşzamanlı dağıtımlar çalıştırırsanız, yalnızca son dağıtım tamamlanmıştır. Tamamlanmayan aynı ada sahip dağıtımlar son dağıtımla değiştirilmiştir. Örneğin, adlı bir depolama hesabı dağıtan adlı bir dağıtım çalıştırırsanız `newStorage` `storage1` ve aynı zamanda adlı bir depolama hesabı dağıtan adlı bir dağıtımı çalıştırırsanız `newStorage` `storage2` , yalnızca bir depolama hesabı dağıtırsınız. Elde edilen depolama hesabının adı `storage2` .

Ancak, adlı bir depolama hesabı dağıtan adlı bir dağıtımı çalıştırırsanız `newStorage` `storage1` ve bu işlemi tamamladıktan hemen sonra adlı bir depolama hesabı dağıtan adlı başka bir dağıtım çalıştırırsanız `newStorage` `storage2` , iki depolama hesabınız olur. Biri olarak adlandırılır `storage1` ve diğeri olarak adlandırılır `storage2` . Ancak, dağıtım geçmişinde yalnızca bir girişiniz vardır.

Her dağıtım için benzersiz bir ad belirttiğinizde, bunları çakışma olmadan eşzamanlı olarak çalıştırabilirsiniz. Adlı bir depolama hesabı dağıtan adlı bir dağıtım çalıştırırsanız `newStorage1` `storage1` ve aynı zamanda adında bir depolama hesabı dağıtan adlı başka bir dağıtımı çalıştırırsanız `newStorage2` `storage2` , dağıtım geçmişinde iki depolama hesabınız ve iki giriş vardır.

Eşzamanlı dağıtımlar ile çakışmalardan kaçınmak ve dağıtım geçmişinde benzersiz girişler sağlamak için her dağıtıma benzersiz bir ad verin.

## <a name="deploy-template-spec"></a>Şablon belirtimini dağıt

Yerel veya uzak şablon dağıtmak yerine, bir [şablon belirtimi](template-specs.md)oluşturabilirsiniz. Şablon belirtimi, Azure aboneliğinizdeki bir ARM şablonu içeren bir kaynaktır. Şablonu kuruluşunuzdaki kullanıcılarla güvenli bir şekilde paylaşmayı kolaylaştırır. Şablon belirtimine erişim sağlamak için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanırsınız. Bu özellik şu anda önizleme aşamasındadır.

Aşağıdaki örneklerde, bir şablon belirtiminin nasıl oluşturulacağı ve dağıtılacağı gösterilmektedir.

İlk olarak, ARM şablonunu sağlayarak şablon belirtimini oluşturun.

```azurecli
az ts create \
  --name storageSpec \
  --version "1.0" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "./mainTemplate.json"
```

Ardından, şablon belirtiminin KIMLIĞINI alın ve dağıtın.

```azurecli
id = $(az ts show --name storageSpec --resource-group templateSpecRG --version "1.0" --query "id")

az deployment group create \
  --resource-group demoRG \
  --template-spec $id
```

Daha fazla bilgi için bkz. [Azure Resource Manager şablonu özellikleri (Önizleme)](template-specs.md).

## <a name="preview-changes"></a>Değişiklikleri Önizle

Şablonunuzu dağıtmadan önce, şablonun ortamınızda yapacağız değişiklikleri önizleyebilirsiniz. Şablonun beklediğiniz değişiklikleri yaptığını doğrulamak için [ne yapılır işlemini](template-deploy-what-if.md) kullanın. Ayrıca, şablonu hatalara yönelik olarak doğrular.

## <a name="parameters"></a>Parametreler

Parametre değerlerini geçirmek için satır içi parametreleri ya da bir parametre dosyasını kullanabilirsiniz.

### <a name="inline-parameters"></a>Satır içi parametreler

Satır içi parametreleri geçirmek için içindeki değerleri sağlayın `parameters` . Örneğin, bir bash kabuğundan bir dizeye dize ve dizi geçirmek için şunu kullanın:

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

Biçimdeki _arrayContent.js_ :

```json
[
    "value1",
    "value2"
]
```

Bir nesneyi geçirmek için örneğin, etiketleri ayarlamak için JSON kullanın. Örneğin, şablonunuz buna benzer bir parametre içerebilir:

```json
    "resourceTags": {
      "type": "object",
      "defaultValue": {
        "Cost Center": "IT Department"
      }
    }
```

Bu durumda, parametreyi aşağıdaki Bash betiğiyle gösterildiği gibi ayarlamak için bir JSON dizesini geçirebilirsiniz:

```bash
tags='{"Owner":"Contoso","Cost Center":"2345-324"}'
az deployment group create --name addstorage  --resource-group myResourceGroup \
--template-file $templateFile \
--parameters resourceName=abcdef4556 resourceTags="$tags"
```

Nesnesine geçirmek istediğiniz JSON etrafında çift tırnak işareti kullanın.

### <a name="parameter-files"></a>Parametre dosyaları

Parametreleri betiğinize satır içi değerler olarak geçirmek yerine parametre değerlerini içeren bir JSON dosyası kullanmak daha kolayınıza gelebilir. Parametre dosyası yerel bir dosya olmalıdır. Dış parametre dosyaları Azure CLı ile desteklenmez.

Parametre dosyası hakkında daha fazla bilgi için bkz. [Resource Manager parametre dosyası oluşturma](parameter-files.md).

Yerel bir parametre dosyasını geçirmek için, `@` _üzerindestorage.parameters.js_ adlı yerel bir dosya belirtmek için kullanın.

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
