---
title: Şablon özelliklerine genel bakış
description: Şablon özelliklerinin nasıl oluşturulduğunu ve kuruluşunuzdaki diğer kullanıcılarla nasıl paylaşılacağını açıklar.
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: f5151550b9f23ba63380688f53325f8976f14a51
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87921887"
---
# <a name="azure-resource-manager-template-specs-preview"></a>Azure Resource Manager şablonu özellikleri (Önizleme)

Şablon belirtimi, daha sonra dağıtım için Azure 'da bir Azure Resource Manager şablonu (ARM şablonu) depolamaya yönelik yeni bir kaynak türüdür. Bu kaynak türü, ARM şablonlarını kuruluşunuzdaki diğer kullanıcılarla paylaşmanıza olanak sağlar. Tıpkı diğer Azure kaynakları gibi, şablon belirtimini paylaşmak için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz.

**Microsoft. resources/Templatespec** , şablon özelliklerine yönelik yeni kaynak türüdür. Ana şablondan ve herhangi bir sayıda bağlantılı şablondan oluşur. Azure, şablon özelliklerini kaynak gruplarında güvenli bir şekilde depolar. Şablon özellikleri [sürümü oluşturmayı](#versioning)destekler.

Şablon belirtimini dağıtmak için PowerShell, Azure CLı, Azure portal, REST ve diğer desteklenen SDK 'lar ve istemciler gibi standart Azure araçlarını kullanırsınız. Şablonla aynı komutları kullanın.

> [!NOTE]
> Şablon özellikleri şu anda önizleme aşamasındadır. Bunu kullanmak için, [bekleme listesine kaydolmanız](https://aka.ms/templateSpecOnboarding)gerekir.

## <a name="why-use-template-specs"></a>Şablon özellikleri neden kullanılmalıdır?

Şu anda bir GitHub deposu veya depolama hesabında şablonunuz varsa, şablonları paylaşmaya ve kullanmaya çalışırken çeşitli güçlükler çalıştırın. Bir kullanıcının dağıtması için, şablonun yerel olması ya da şablonun URL 'SI herkese açık olmalıdır. Bu kısıtlamayı aşmak için, şablonun kopyalarını dağıtması gereken kullanıcılarla paylaşabilir veya depo veya depolama hesabına erişim açmanız gerekebilir. Kullanıcılar bir şablonun yerel kopyalarına sahip olduğunda, bu kopyalar sonunda özgün şablondan ayrılmış olabilir. Bir depoyu veya depolama hesabını genel olarak erişilebilir yaptığınızda, istenmeyen kullanıcıların şablona erişmesine izin verebilirsiniz.

Şablon özelliklerini kullanmanın avantajı, kurallı şablonlar oluşturabileceğiniz ve bunları kuruluşunuzdaki ekiplerle paylaşabilir. Şablon özellikleri güvenli hale getiriyoruz çünkü dağıtım için Azure Resource Manager, ancak RBAC izni olmayan kullanıcılar tarafından erişilemez durumda. Kullanıcıların şablonunu dağıtmak için yalnızca şablon belirtimine okuma erişimi olması gerekir, böylece şablonu başkalarının değiştirmesine izin vermeden paylaşabilirsiniz.

Bir Template spec 'e dahil ettiğiniz şablonlar, kuruluşunuzun gereksinimlerini ve kılavuzunu izlemek için kuruluşunuzdaki yöneticiler tarafından doğrulanmalıdır.

## <a name="create-template-spec"></a>Şablon belirtimi oluşturma

Aşağıdaki örnekte, Azure 'da bir depolama hesabı oluşturmak için basit bir şablon gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
                "Standard_LRS",
                "Standard_GRS",
                "Standard_ZRS",
                "Premium_LRS"
            ]
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[concat('store', uniquestring(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "kind": "StorageV2",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            }
        }
    ]
}
```

Şablon belirtimini oluşturduğunuzda, PowerShell veya CLı komutları ana şablon dosyasına geçirilir. Ana şablon bağlantılı şablonlara başvuruyorsa, komutlar şablon belirtimini oluşturacak şekilde bulup paketleyebilir. Daha fazla bilgi için bkz. [bağlantılı şablonlarla şablon belirtimi oluşturma](#create-a-template-spec-with-linked-templates).

Kullanarak bir şablon belirtimi oluşturun:

```azurepowershell
New-AzTemplateSpec -Name storageSpec -Version 1.0 -ResourceGroupName templateSpecsRg -TemplateJsonFile ./mainTemplate.json
```

Aboneliğinizdeki tüm şablon özelliklerini kullanarak şu şekilde görüntüleyebilirsiniz:

```azurepowershell
Get-AzTemplateSpec
```

İle sürümleri de dahil olmak üzere bir şablon belirtiminin ayrıntılarını görüntüleyebilirsiniz:

```azurepowershell
Get-AzTemplateSpec -ResourceGroupName templateSpecsRG -Name storageSpec
```

## <a name="deploy-template-spec"></a>Şablon belirtimini dağıt

Şablon belirtimini oluşturduktan sonra, şablon belirtimine **okuma** erişimi olan kullanıcılar onu dağıtabilir. Erişim verme hakkında bilgi için bkz. [öğretici: Azure PowerShell kullanarak Azure kaynaklarına grup erişimi verme](../../role-based-access-control/tutorial-role-assignments-group-powershell.md).

Şablon Özellikleri Portal, PowerShell, Azure CLı veya daha büyük bir şablon dağıtımında bağlantılı bir şablon aracılığıyla dağıtılabilir. Bir kuruluştaki kullanıcılar, bir şablon belirtimini Azure 'daki herhangi bir kapsama (kaynak grubu, abonelik, yönetim grubu veya kiracı) dağıtabilir.

Bir şablon için bir yol veya URI geçirmek yerine, kaynak KIMLIĞINI sağlayarak bir şablon belirtimi dağıtırsınız. Kaynak KIMLIĞI aşağıdaki biçimdedir:

**/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Resources/templateSpecs/{template-spec-name}/versions/{template-spec-version}**

Kaynak KIMLIĞININ, şablon belirtimi için bir sürüm numarası içerdiğine dikkat edin.

Örneğin, bir şablon belirtimini aşağıdaki PowerShell komutuyla dağıtırsınız.

```azurepowershell
$id = "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/templateSpecsRG/providers/Microsoft.Resources/templateSpecs/storageSpec/versions/1.0"

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG
```

Uygulamada, genellikle `Get-AzTemplateSpec` dağıtmak istediğiniz şablon BELIRTIMININ kimliğini almak için çalıştırabileceksiniz.

```azurepowershell
$id = (Get-AzTemplateSpec -Name storageSpec -ResourceGroupName templateSpecsRg -Version 1.0).Version.Id

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG
```

## <a name="parameters"></a>Parametreler

Parametreleri Template spec 'e geçirmek, parametreleri bir ARM şablonuna geçirme gibidir. Parametre değerlerini satır içi veya bir parametre dosyasına ekleyin.

Bir parametreyi satır içine geçirmek için şunu kullanın:

```azurepowershell
New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG `
  -StorageAccountType Standard_GRS
```

Yerel bir parametre dosyası oluşturmak için şunu kullanın:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "StorageAccountType": {
      "value": "Standard_GRS"
    }
  }
}
```

Ve bu parametre dosyasını şu ile geçirin:

```azurepowershell
New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG `
  -TemplateParameterFile ./mainTemplate.parameters.json
```

## <a name="create-a-template-spec-with-linked-templates"></a>Bağlantılı şablonlarla şablon belirtimi oluşturma

Şablon belirtiminin ana şablonu bağlantılı şablonlara başvuruyorsa, PowerShell ve CLı komutları, bağlantılı şablonları yerel sürücünüzden otomatik olarak bulabilir ve paketleyebilir. Şablon özelliklerini barındırmak için depolama hesaplarını veya depoları el ile yapılandırmanız gerekmez; şablon belirtimi kaynağında her şey kendi kendine dahil edilir.

Aşağıdaki örnek, iki bağlantılı şablonla bir ana şablondan oluşur. Örnek yalnızca şablonun bir alıntısıdır. Diğer şablonlara bağlamak için adlı bir özellik kullandığına dikkat edin `relativePath` . `apiVersion` `2020-06-01` Dağıtımlar kaynağı için veya sonraki bir sürümünü kullanmanız gerekir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "relativePath": "artifacts/webapp.json"
                }
            }
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "relativePath": "artifacts/database.json"
                }
            }
        }
    ],
    "outputs": {}
}

```

Önceki örnekte şablon belirtimini oluşturmak için PowerShell veya CLı komutu yürütüldüğünde, komut üç dosya bulur-ana şablon, Web uygulaması şablonu ( `webapp.json` ) ve veritabanı şablonu ( `database.json` ) ve bunları şablon belirtimine göre paketler.

Daha fazla bilgi için bkz. [öğretici: bağlantılı şablonlarla şablon belirtimi oluşturma](template-specs-create-linked.md).

## <a name="deploy-template-spec-as-a-linked-template"></a>Şablon belirtimini bağlantılı şablon olarak dağıtma

Şablon belirtimini oluşturduktan sonra, bir ARM şablonundan veya başka bir şablon belirtiminde yeniden kullanmak kolaydır. Şablon belirtimine, kaynak KIMLIĞINI şablonunuza ekleyerek bağlarsınız. Bağlı şablon belirtimi, ana şablonu dağıtırken otomatik olarak dağıtılır. Bu davranış, modüler şablon özellikleri geliştirmenize ve bunları gerektiği gibi yeniden kullanmanıza olanak tanır.

Örneğin, ağ kaynaklarını dağıtan bir şablon belirtimi ve depolama kaynaklarını dağıtan başka bir şablon özelliği oluşturabilirsiniz. ARM şablonlarında, her zaman ağ veya depolama kaynaklarını yapılandırmak için bu iki şablon özelliklerine bağlantı oluşturmanız gerekir.

Aşağıdaki örnek, önceki örneğe benzerdir, ancak `id` özelliğini kullanarak bir `relativePath` Yerel şablona bağlanacak özelliği yerine bir şablon belirtimine bağlayabilirsiniz. `2020-06-01`Dağıtımlar kaynağı IÇIN API sürümü için kullanın. Örnekte, şablon özellikleri **Templatespecsrg**adlı bir kaynak grubunda bulunur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "networkingDeployment",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'networkingSpec', '1.0')]"
                }
            }
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "storageDeployment",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'storageSpec', '1.0')]"
                }
            }
        }
    ],
    "outputs": {}
}
```

Şablon özelliklerinin bağlanması hakkında daha fazla bilgi için bkz. [öğretici: şablon belirtimini bağlantılı şablon olarak dağıtma](template-specs-deploy-linked-template.md).

## <a name="versioning"></a>Sürüm oluşturma

Bir şablon belirtimi oluşturduğunuzda, bunun için bir sürüm numarası sağlarsınız. Şablon kodu üzerinde yineleme yaparken, var olan bir sürümü güncelleştirebilir (düzeltmeler için) veya yeni bir sürüm yayımlayabilirsiniz. Sürüm bir metin dizesidir. Anlamsal sürüm oluşturma da dahil olmak üzere tüm sürüm oluşturma sistemini izlemeyi tercih edebilirsiniz. Şablon belirtiminin kullanıcıları, dağıtma sırasında kullanmak istedikleri sürüm numarasını sağlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Bir şablon belirtimi oluşturup dağıtmak için bkz. [hızlı başlangıç: Create and Deploy Template spec](quickstart-create-template-specs.md).

* Şablon özelliklerine bağlama şablonları hakkında daha fazla bilgi için bkz. [öğretici: bağlantılı şablonlarla şablon belirtimi oluşturma](template-specs-create-linked.md).

* Şablon belirtimini bağlantılı şablon olarak dağıtma hakkında daha fazla bilgi için bkz. [öğretici: şablon belirtimini bağlantılı şablon olarak dağıtma](template-specs-deploy-linked-template.md).
