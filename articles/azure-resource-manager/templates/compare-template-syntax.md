---
title: JSON ve Bıcep 'de Azure Resource Manager şablonlarının sözdizimini karşılaştırın
description: JSON ve Bıcep ile geliştirilen Azure Resource Manager şablonlarını karşılaştırır ve diller arasında nasıl dönüştürme yapılacağını gösterir.
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 29c2b9948957ebc10a26f22f0fe3daf383dfe5ba
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102036223"
---
# <a name="comparing-json-and-bicep-for-templates"></a>Şablonlar için JSON ve Bıcep karşılaştırması

Bu makalede, Bıcep sözdizimini Azure Resource Manager şablonları (ARM şablonları) için JSON sözdizimiyle karşılaştırılmaktadır. Çoğu durumda Bıcep, JSON 'daki eşdeğer değerden daha az ayrıntılı bir sözdizimi sağlar.

## <a name="syntax-equivalents"></a>Söz dizimi eşdeğerleri

ARM şablonları geliştirmek için JSON kullanmaya alışdıysanız, Bıcep için eşdeğer sözdizimi hakkında bilgi edinmek için aşağıdaki tabloyu kullanın.

| Senaryo | Bicep | JSON |
| -------- | ------------ | ----- |
| Bir ifade yazın | `func()` | `"[func()]"` |
| Parametre değerini Al | `exampleParameter` | `[parameters('exampleParameter'))]` |
| Değişken değerini Al | `exampleVar` | `[variables('exampleVar'))]` |
| Dizeleri birleştirme | `'${namePrefix}-vm'` | `[concat(parameters('namePrefix'), '-vm')]` |
| Kaynak özelliğini ayarla | `sku: '2016-Datacenter'` | `"sku": "2016-Datacenter",` |
| Mantıksal ve | `isMonday && isNovember` | `[and(parameter('isMonday'), parameter('isNovember'))]` |
| Şablondaki kaynağın kaynak KIMLIĞINI al | `nic1.id` | `[resourceId('Microsoft.Network/networkInterfaces', variables('nic1Name'))]` |
| Şablondaki kaynaktaki özelliği al | `diagsAccount.properties.primaryEndpoints.blob` | `[reference(resourceId('Microsoft.Storage/storageAccounts', variables('diagStorageAccountName'))).primaryEndpoints.blob]` |
| Koşullu bir değer ayarla | `isMonday ? 'valueIfTrue' : 'valueIfFalse'` | `[if(parameters('isMonday'), 'valueIfTrue', 'valueIfFalse')]` |
| Bir çözümü birden çok dosyaya ayırma | Modülleri kullanma | Bağlı şablonları kullanma |
| Dağıtımın hedef kapsamını ayarlama | `targetScope = 'subscription'` | `"$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#"` |
| Bağımlılığı ayarla | Bağımlılıklar için otomatik algılamayı veya el ile bağımlılığı ayarlamayı güvenin `dependsOn: [ stg ]` | `"dependsOn": ["[resourceId('Microsoft.Storage/storageAccounts', 'parameters('storageAccountName'))]"]` |
| Kaynak bildirimi | `resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = {...}` | `"resources": [ { "type": "Microsoft.Compute/virtualMachines", "apiVersion": "2020-06-01", ... } ]` |

## <a name="recommendations"></a>Öneriler

* Mümkün olduğunda, Bıcep dosyanızdaki [Reference](template-functions-resource.md#reference) ve [RESOURCEID](template-functions-resource.md#resourceid) işlevlerini kullanmaktan kaçının. Aynı bicep dağıtımında bir kaynağa başvurduğunuzda, bunun yerine kaynak tanımlayıcıyı kullanın. Örneğin, Bıcep dosyanızda kaynak tanımlayıcısı olarak bulunan bir kaynak dağıttıysanız `stg` , `stg.id` `stg.properties.primaryEndpoints.blob` özellik değerlerini almak için veya gibi bir sözdizimi kullanın. Kaynak tanımlayıcısını kullanarak kaynaklar arasında örtük bir bağımlılık oluşturursunuz. Bağımlılığı Bağımlıdson özelliği ile açıkça ayarlamanız gerekmez.
* Tanımlayıcılar için tutarlı büyük harf kullanın. Ne tür bir büyük küçük harfe kullanılacağı konusunda emin değilseniz, ortası büyük küçük harf 'i deneyin. Örneğin, `param myCamelCasedParameter string`.
* Yalnızca açıklama kullanıcılara önemli bilgiler sağlıyorsa parametreye bir açıklama ekleyin. `//`Bazı bilgiler için açıklamaları kullanabilirsiniz.

## <a name="decompile-json-to-bicep"></a>JSON 'ı Bıcep 'ye derlemeyi kaldırma

Bıcep CLı, var olan bir ARM şablonunu bir bicep dosyasına derlemeyi sağlayan bir komut sağlar. Bir JSON dosyasını derlemek için şunu kullanın: `bicep decompile "path/to/file.json"`

Bu komut, Bıcep yazma için bir başlangıç noktası sağlar, ancak komut tüm şablonlar için çalışmaz. Komut başarısız olabilir veya derlemeden sonra sorunları çözmeniz gerekebilir. Şu anda, iç içe geçmiş şablonlar yalnızca ' Inner ' ifade değerlendirme kapsamını kullandıklarında derlenerek parçalanamaz.

Bir kaynak grubu için şablonu dışa aktarabilir ve ardından doğrudan bıcep assembler diline komutuna geçirebilirsiniz. Aşağıdaki örnek, bir aktarılmış şablonun nasıl derlenmeyeceğini gösterir.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az group export --name "your_resource_group_name" > main.json
bicep decompile main.json
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Export-AzResourceGroup -ResourceGroupName "your_resource_group_name" -Path ./main.json
bicep decompile main.json
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Şablonu Portal üzerinden dışarı aktarın](export-template-portal.md) . `bicep decompile <filename>`İndirilen dosyada kullanın.

---

## <a name="build-json-from-bicep"></a>Bıcep 'den JSON oluştur

Bıcep CLı, Bıcep 'yi JSON 'a dönüştürmek için bir komut de sağlar. JSON dosyası oluşturmak için şunu kullanın: `bicep build "path/to/file.json"`

## <a name="side-by-side-view"></a>Yan yana görünüm

[Bicep playzemin](https://aka.ms/bicepdemo) , eşdeğer JSON ve bicep dosyalarını yan yana görüntülemenize olanak sağlar. Her iki sürümü de görmek için örnek bir şablon seçebilirsiniz. Ya da `Decompile` kendı JSON şablonunuzu karşıya yüklemeyi seçin ve eşdeğer Bıcep dosyasını görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bıcep hakkında daha fazla bilgi için bkz. [bıcep öğreticisi](./bicep-tutorial-create-first-bicep.md).
