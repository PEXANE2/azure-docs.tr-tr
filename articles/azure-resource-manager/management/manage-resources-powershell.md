---
title: Kaynakları yönetin - Azure PowerShell
description: Kaynaklarınızı yönetmek için Azure PowerShell ve Azure Kaynak Yöneticisi'ni kullanın. Kaynakların nasıl dağıtılsüreceğini ve silinini gösterir.
author: mumian
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: de6f24ea66ef41b5ee4bfdda5948de9639f10a51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485409"
---
# <a name="manage-azure-resources-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure kaynaklarını yönetme

Azure kaynaklarınızı yönetmek için [Azure Kaynak Yöneticisi](overview.md) ile Azure PowerShell'i nasıl kullanacağınızı öğrenin. Kaynak gruplarını yönetmek için Azure [PowerShell'i kullanarak Azure kaynak gruplarını yönet'e](manage-resource-groups-powershell.md)bakın.

Kaynakların yönetimi yle ilgili diğer makaleler:

- [Azure portalını kullanarak Azure kaynaklarını yönetme](manage-resources-portal.md)
- [Azure CLI'yi kullanarak Azure kaynaklarını yönetme](manage-resources-cli.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları varolan bir kaynak grubuna dağıtma

Azure kaynaklarını doğrudan Azure PowerShell'i kullanarak dağıtabilir veya Azure kaynakları oluşturmak için bir Kaynak Yöneticisi şablonu dağıtabilirsiniz.

### <a name="deploy-a-resource"></a>Kaynak dağıtma

Aşağıdaki komut dosyası bir depolama hesabı oluşturur.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
  -Name $storageAccountName `
  -Location $location `
  -SkuName "Standard_LRS"

# Retrieve the context.
$ctx = $storageAccount.Context
```

### <a name="deploy-a-template"></a>Şablon dağıtma

Aşağıdaki komut dosyası, bir depolama hesabı oluşturmak için bir Quickstart şablonu dağıtır. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Visual Studio Code'u kullanarak Azure Kaynak Yöneticisi şablonları oluşturun.](../templates/quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -Location $location
```

Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıt'a](../templates/deploy-powershell.md)bakın.

## <a name="deploy-a-resource-group-and-resources"></a>Kaynak grubu ve kaynakları dağıtma

Bir kaynak grubu oluşturabilir ve kaynakları gruba dağıtabilirsiniz. Daha fazla bilgi için kaynak [grubu oluştur ve kaynakları dağıtı'](../templates/deploy-to-subscription.md#resource-group-and-resources)ya bakın.

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>Kaynakları birden çok abonelik veya kaynak grubuna dağıtma

Genellikle, şablonunuzdaki tüm kaynakları tek bir kaynak grubuna dağıtırsınız. Ancak, bir dizi kaynağı birlikte dağıtmak ancak bunları farklı kaynak gruplarına veya aboneliklere yerleştirmek istediğiniz senaryolar vardır. Daha fazla bilgi için azure [kaynaklarını birden çok abonelik veya kaynak grubuna dağıt'a](../templates/cross-resource-group-deployment.md)bakın.

## <a name="delete-resources"></a>Kaynakları silme

Aşağıdaki komut dosyası, bir depolama hesabının nasıl silinir olduğunu gösterir.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Remove-AzStorageAccount -ResourceGroupName $resourceGroupName -AccountName $storageAccountName
```

Azure Kaynak Yöneticisi'nin kaynakların silinmesini nasıl emrettikleri hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi kaynak silme](delete-resource-group.md)bölümüne bakın.

## <a name="move-resources"></a>Kaynakları taşıma

Aşağıdaki komut dosyası, bir depolama hesabının bir kaynak grubundan başka bir kaynak grubuna nasıl kaldırılır olduğunu gösterir.

```azurepowershell-interactive
$srcResourceGroupName = Read-Host -Prompt "Enter the source Resource Group name"
$destResourceGroupName = Read-Host -Prompt "Enter the destination Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$storageAccount = Get-AzResource -ResourceGroupName $srcResourceGroupName -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $destResourceGroupName -ResourceId $storageAccount.ResourceId
```

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

## <a name="lock-resources"></a>Kaynakları kilitleme

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi kritik kaynakları yanlışlıkla siler veya değiştirmesini engeller. 

Aşağıdaki komut dosyası, hesabın silinmesin diye bir depolama hesabını kilitler.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

New-AzResourceLock -LockName LockStorage -LockLevel CanNotDelete -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts 
```

Aşağıdaki komut dosyası, bir depolama hesabı için tüm kilitleri alır:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts
```

Aşağıdaki komut dosyası, bir depolama hesabının kilitlerini siler:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$lockId = (Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts).LockId
Remove-AzResourceLock -LockId $lockId
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](lock-resources.md).

## <a name="tag-resources"></a>Kaynakları etiketleme

Etiketleme, kaynak grubunuzu ve kaynaklarınızı mantıksal olarak düzenlemeye yardımcı olur. Bilgi için Azure [kaynaklarınızı düzenlemek için etiketleri kullanma'ya](tag-resources.md#powershell)bakın.

## <a name="manage-access-to-resources"></a>Kaynaklara erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure PowerShell'i kullanarak erişimi yönet'e](../../role-based-access-control/role-assignments-powershell.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kaynak Yöneticisi'ni öğrenmek için [Azure Kaynak Yöneticisi'ne genel bakış](overview.md)'a bakın.
- Kaynak Yöneticisi şablonsözdizimini öğrenmek için [bkz.](../templates/template-syntax.md)
- Şablonları nasıl geliştireceklerini öğrenmek için [adım adım öğreticilere](/azure/azure-resource-manager/)bakın.
- Azure Kaynak Yöneticisi şablon şemalarını görüntülemek için [şablon başvurusuna](/azure/templates/)bakın.
