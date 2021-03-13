---
title: JSON ve Bıcep 'de Azure Resource Manager şablonlarının sözdizimini karşılaştırın
description: JSON ve Bıcep ile geliştirilen Azure Resource Manager şablonlarını karşılaştırır ve diller arasında nasıl dönüştürme yapılacağını gösterir.
ms.topic: conceptual
ms.date: 03/12/2021
ms.openlocfilehash: 85f85e66e69eede68bab847e4bc68514e65115eb
ms.sourcegitcommit: df1930c9fa3d8f6592f812c42ec611043e817b3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103418054"
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

## <a name="next-steps"></a>Sonraki adımlar

* Bıcep hakkında daha fazla bilgi için bkz. [bıcep öğreticisi](./bicep-tutorial-create-first-bicep.md).
* Dilleri arasında şablonları dönüştürme hakkında bilgi edinmek için bkz. [JSON ve Bıcep arasında ARM şablonlarını dönüştürme](bicep-decompile.md).
