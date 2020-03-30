---
title: Kaynak gruplarını yönetme - Azure PowerShell
description: Azure Kaynak Yöneticisi aracılığıyla kaynak gruplarınızı yönetmek için Azure PowerShell'i kullanın. Kaynak gruplarının nasıl oluşturulup listelenebildiğini ve silineceklerini gösterir.
author: mumian
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 25e001927d5c06b4a7e5639faaa054ae18b12bb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248365"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure Kaynak Yöneticisi kaynak gruplarını yönetme

Azure kaynak gruplarınızı yönetmek için [Azure Kaynak Yöneticisi](overview.md) ile Azure PowerShell'i nasıl kullanacağınızı öğrenin. Azure kaynaklarını yönetmek için Azure [PowerShell'i kullanarak Azure kaynaklarını yönet'e](manage-resources-powershell.md)bakın.

Kaynak gruplarının yönetimi yle ilgili diğer makaleler:

- [Azure portalını kullanarak Azure kaynak gruplarını yönetme](manage-resources-portal.md)
- [Azure CLI'yi kullanarak Azure kaynak gruplarını yönetme](manage-resources-cli.md)

## <a name="what-is-a-resource-group"></a>Kaynak grubu nedir

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genel olarak, aynı kaynak grubuna aynı yaşam döngüsünü paylaşan kaynaklar ekleyin, böylece bunları grup olarak kolayca dağıtabilir, güncelleyebilir ve silebilirsiniz.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğiniz zaman, bu meta verilerin nerede depolandığınızı belirtirsiniz.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

Aşağıdaki PowerShell komut dosyası bir kaynak grubu oluşturur ve sonra kaynak grubunu gösterir.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location

Get-AzResourceGroup -Name $resourceGroupName
```

## <a name="list-resource-groups"></a>Kaynak gruplarını listele

Aşağıdaki PowerShell komut dosyası, aboneliğiniz altındaki kaynak gruplarını listeler.

```azurepowershell-interactive
Get-AzResourceGroup
```

Bir kaynak grubu elde etmek için:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Get-AzResourceGroup -Name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Kaynak gruplarını silme

Aşağıdaki PowerShell komut dosyası bir kaynak grubunu siler:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Remove-AzResourceGroup -Name $resourceGroupName
```

Azure Kaynak Yöneticisi'nin kaynakların silinmesini nasıl emrettikleri hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi kaynak silme](delete-resource-group.md)bölümüne bakın.

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları varolan bir kaynak grubuna dağıtma

Bkz. [Kaynakları varolan bir kaynak grubuna dağıt.](manage-resources-powershell.md#deploy-resources-to-an-existing-resource-group)

Kaynak grubu dağıtımını doğrulamak için [test-AzResourceGroupDeployment'a](https://docs.microsoft.com/powershell/module/Az.Resources/Test-AzResourceGroupDeployment?view=azps-1.3.0)bakın.

## <a name="deploy-a-resource-group-and-resources"></a>Kaynak grubu ve kaynakları dağıtma

Kaynak Yöneticisi şablonu kullanarak bir kaynak grubu oluşturabilir ve kaynakları gruba dağıtabilirsiniz. Daha fazla bilgi için kaynak [grubu oluştur ve kaynakları dağıtı'](../templates/deploy-to-subscription.md#resource-group-and-resources)ya bakın.

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bu özellik hata *üzerine Rollback*olarak da bilinir. Daha fazla bilgi için [dağıtım başarısız olduğunda Yeniden Dağıt'a](../templates/rollback-on-error.md)bakın.

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneye taşıma

Gruptaki kaynakları başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

## <a name="lock-resource-groups"></a>Kaynak gruplarını kilitleme

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi kritik kaynakları yanlışlıkla siler veya değiştirmesini engeller. 

Aşağıdaki komut dosyası, kaynak grubunun silinmesin diye bir kaynak grubunu kilitler.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName $resourceGroupName 
```

Aşağıdaki komut dosyası, kaynak grubu için tüm kilitleri alır:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Get-AzResourceLock -ResourceGroupName $resourceGroupName 
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](lock-resources.md).

## <a name="tag-resource-groups"></a>Kaynak gruplarını etiketleme

Varlıklarınızı mantıksal olarak düzenlemek için kaynak gruplarına ve kaynaklara etiketler uygulayabilirsiniz. Bilgi için Azure [kaynaklarınızı düzenlemek için etiketleri kullanma'ya](tag-resources.md#powershell)bakın.

## <a name="export-resource-groups-to-templates"></a>Kaynak gruplarını şablonlara dışa aktarma

Kaynak grubunuzu ayarladıktan sonra, kaynak grubu için bir Kaynak Yöneticisi şablonunu görüntüleyebilirsiniz. Şablonu dışa aktarmanın iki avantajı vardır:

- Şablon tam altyapı içerdiğinden çözümün gelecekteki dağıtımlarını otomatikleştirin.
- Çözümünüzü temsil eden JavaScript Nesne Gösterimi'ne (JSON) bakarak şablon sözdizimini öğrenin.

Kaynak grubundaki tüm kaynakları dışa aktarmak için [Dışa Aktarma-AzResourceGroup](/powershell/module/az.resources/Export-AzResourceGroup) cmdlet'ini kullanın ve kaynak grubu adını sağlayın.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Export-AzResourceGroup -ResourceGroupName $resourceGroupName
```

Şablonu yerel bir dosya olarak kaydeder.

Kaynak grubundaki tüm kaynakları dışa aktarmak yerine, hangi kaynakların dışa aktarılacak olduğunu seçebilirsiniz.

Bir kaynak dışa aktarmak için bu kaynak kimliğini aktarın.

```azurepowershell-interactive
$resource = Get-AzResource `
  -ResourceGroupName <resource-group-name> `
  -ResourceName <resource-name> `
  -ResourceType <resource-type>
Export-AzResourceGroup `
  -ResourceGroupName <resource-group-name> `
  -Resource $resource.ResourceId
```

Birden fazla kaynak dışa aktarmak için kaynak işllerini bir dizide geçire

```azurepowershell-interactive
Export-AzResourceGroup `
  -ResourceGroupName <resource-group-name> `
  -Resource @($resource1.ResourceId, $resource2.ResourceId)
```

Şablonu dışa aktarırken, şablonda parametrelerin kullanılıp kullanılmadığını belirtebilirsiniz. Varsayılan olarak, kaynak adları için parametreler dahildir, ancak varsayılan bir değeri yoktur. Dağıtım sırasında bu parametre değerini geçmeniz gerekir.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "defaultValue": null,
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "defaultValue": null,
    "type": "String"
  }
}
```

Kaynakta, parametre ad için kullanılır.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "[parameters('serverfarms_demoHostPlan_name')]",
    ...
  }
]
```

Şablonu `-IncludeParameterDefaultValue` dışa aktarırken parametreyi kullanırsanız, şablon parametresi geçerli değere ayarlanmış varsayılan bir değer içerir. Bu varsayılan değeri kullanabilir veya farklı bir değer geçirerek varsayılan değerin üzerine yazabilirsiniz.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "defaultValue": "demoHostPlan",
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "defaultValue": "webSite3bwt23ktvdo36",
    "type": "String"
  }
}
```

Şablonu `-SkipResourceNameParameterization` dışa aktarırken parametreyi kullanıyorsanız, kaynak adları için parametreler şablona dahil olmaz. Bunun yerine, kaynak adı doğrudan kaynağın geçerli değerine ayarlanır. Dağıtım sırasında adı özelleştiremezsiniz.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "demoHostPlan",
    ...
  }
]
```

Dışa aktarma şablonu özelliği Azure Veri Fabrikası kaynaklarını dışa aktarmayı desteklemez. Veri Fabrikası kaynaklarını nasıl dışa aktarabileceğiniz hakkında bilgi edinmek için Azure [Veri Fabrikası'nda veri fabrikasıkopyala veya klonlama'ya](https://aka.ms/exportTemplateViaAdf)bakın.

Klasik dağıtım modeli yle oluşturulan kaynakları dışa aktarmak [için kaynak yöneticisi dağıtım modeline geçirmeniz](https://aka.ms/migrateclassicresourcetoarm)gerekir.

Daha fazla bilgi için Azure [portalında şablona tek ve çok kaynak lı dışa aktarma](../templates/export-template-portal.md)bilgisine bakın.

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure PowerShell'i kullanarak erişimi yönet'e](../../role-based-access-control/role-assignments-powershell.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kaynak Yöneticisi'ni öğrenmek için [Azure Kaynak Yöneticisi'ne genel bakış](overview.md)'a bakın.
- Kaynak Yöneticisi şablonsözdizimini öğrenmek için [bkz.](../templates/template-syntax.md)
- Şablonları nasıl geliştireceklerini öğrenmek için [adım adım öğreticilere](/azure/azure-resource-manager/)bakın.
- Azure Kaynak Yöneticisi şablon şemalarını görüntülemek için [şablon başvurusuna](/azure/templates/)bakın.