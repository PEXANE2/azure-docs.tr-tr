---
title: JSON ile Bıcep arasında Azure Resource Manager şablonları dönüştürme
description: JSON ve Bıcep ile geliştirilen Azure Resource Manager şablonlarını karşılaştırır.
ms.topic: conceptual
ms.date: 02/19/2021
ms.openlocfilehash: 9388ed50f13d6885d0a0668b61a9141dae375244
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746132"
---
# <a name="comparing-json-and-bicep-for-templates"></a>Şablonlar için JSON ve Bıcep karşılaştırması

Bu makalede, Bıcep sözdizimini Azure Resource Manager şablonları (ARM şablonları) için JSON sözdizimiyle karşılaştırılmaktadır. Çoğu durumda Bıcep, JSON 'daki eşdeğer değerden daha az ayrıntılı bir sözdizimi sağlar.

## <a name="syntax-equivalents"></a>Söz dizimi eşdeğerleri

ARM şablonları geliştirmek için JSON kullanmaya alışdıysanız, Bıcep için eşdeğer sözdizimi hakkında bilgi edinmek için aşağıdaki tabloyu kullanın.

| Senaryo | ARM şablonu | Bicep |
| -------- | ------------ | ----- |
| Bir ifade yazın | `"[func()]"` | `func()` |
| Parametre değerini Al | `[parameters('exampleParameter'))]` | `exampleParameter` |
| Değişken değerini Al | `[variables('exampleVar'))]` | `exampleVar` |
| Dizeleri birleştirme | `[concat(parameters('namePrefix'), '-vm')]` | `'${namePrefix}-vm'` |
| Kaynak özelliğini ayarla | `"sku": "2016-Datacenter",` | `sku: '2016-Datacenter'` |
| Mantıksal ve | `[and(parameter('isMonday'), parameter('isNovember'))]` | `isMonday && isNovember` |
| Şablondaki kaynağın kaynak KIMLIĞINI al | `[resourceId('Microsoft.Network/networkInterfaces', variables('nic1Name'))]` | `nic1.id` |
| Şablondaki kaynaktaki özelliği al | `[reference(resourceId('Microsoft.Storage/storageAccounts', variables('diagStorageAccountName'))).primaryEndpoints.blob]` | `diagsAccount.properties.primaryEndpoints.blob` |
| Koşullu bir değer ayarla | `[if(parameters('isMonday'), 'valueIfTrue', 'valueIfFalse')]` | `isMonday ? 'valueIfTrue' : 'valueIfFalse'` |
| Bir çözümü birden çok dosyaya ayırma | Bağlı şablonları kullanma | Modülleri kullanma |
| Dağıtımın hedef kapsamını ayarlama | `"$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#"` | `targetScope = 'subscription'` |
| Bağımlılığı ayarla | `"dependsOn": ["[resourceId('Microsoft.Storage/storageAccounts', 'parameters('storageAccountName'))]"]` | Bağımlılıklar için otomatik algılamayı veya el ile bağımlılığı ayarlamayı güvenin `dependsOn: [ stg ]` |

Bir kaynağın türünü ve sürümünü bildirmek için Bıcep içinde şunu kullanın:

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = {
  ...
}
```

JSON 'daki eşdeğer sözdizimi yerine:

```json
"resources": [
  {
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2020-06-01",
    ...
  }
]
```

## <a name="recommendations"></a>Öneriler

* Mümkün olduğunda, Bıcep dosyanızdaki [Reference](template-functions-resource.md#reference) ve [RESOURCEID](template-functions-resource.md#resourceid) işlevlerini kullanmaktan kaçının. Aynı bicep dağıtımında bir kaynağa başvurduğunuzda, bunun yerine kaynak tanımlayıcıyı kullanın. Örneğin, Bıcep dosyanızda kaynak tanımlayıcısı olarak bulunan bir kaynak dağıttıysanız `stg` , `stg.id` `stg.properties.primaryEndpoints.blob` özellik değerlerini almak için veya gibi bir sözdizimi kullanın. Kaynak tanımlayıcısını kullanarak kaynaklar arasında örtük bir bağımlılık oluşturursunuz. Bağımlılığı Bağımlıdson özelliği ile açıkça ayarlamanız gerekmez.
* Tanımlayıcılar için tutarlı büyük harf kullanın. Ne tür bir büyük küçük harfe kullanılacağı konusunda emin değilseniz, ortası büyük küçük harf 'i deneyin. Örneğin, `param myCamelCasedParameter string`.
* Yalnızca açıklama kullanıcılara önemli bilgiler sağlıyorsa parametreye bir açıklama ekleyin. `//`Bazı bilgiler için açıklamaları kullanabilirsiniz.

## <a name="decompile-json-to-bicep"></a>JSON 'ı Bıcep 'ye derlemeyi kaldırma

Bıcep CLı, var olan bir ARM şablonunu bir bicep dosyasına derlemeyi sağlayan bir komut sağlar. Bir JSON dosyasını derlemek için şunu kullanın: `bicep decompile "path/to/file.json"`

Bu komut, Bıcep yazma için bir başlangıç noktası sağlar, ancak komut tüm şablonlar için çalışmaz. Komut başarısız olabilir veya derlemeden sonra sorunları çözmeniz gerekebilir. Şu anda komut aşağıdaki sınırlamalara sahiptir:

* Kopyalama döngüleri kullanan şablonlar derlenemiyor.
* İç içe geçmiş şablonlar yalnızca ' Inner ' ifade değerlendirme kapsamını kullanıyorsa, parçalanamaz.

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

Bıcep projesi hakkında daha fazla bilgi için bkz. [Proje bıcep](https://github.com/Azure/bicep).
