---
title: Kaynakları yönetme - Azure CLI
description: Kaynaklarınızı yönetmek için Azure CLI ve Azure Kaynak Yöneticisi'ni kullanın. Kaynakların nasıl dağıtılsüreceğini ve silinini gösterir.
author: mumian
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: f00c1725201b0e49f80fec64e5d69b375ec7a233
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485539"
---
# <a name="manage-azure-resources-by-using-azure-cli"></a>Azure CLI'yi kullanarak Azure kaynaklarını yönetme

Azure kaynaklarınızı yönetmek için [Azure Kaynak Yöneticisi](overview.md) ile Azure CLI'yi nasıl kullanacağınızı öğrenin. Kaynak gruplarını yönetmek için Azure [CLI'yi kullanarak Azure kaynak gruplarını yönet'e](manage-resource-groups-cli.md)bakın.

Kaynakların yönetimi yle ilgili diğer makaleler:

- [Azure portalını kullanarak Azure kaynaklarını yönetme](manage-resources-portal.md)
- [Azure PowerShell'i kullanarak Azure kaynaklarını yönetme](manage-resources-powershell.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları varolan bir kaynak grubuna dağıtma

Azure kaynaklarını doğrudan Azure CLI kullanarak dağıtabilir veya Azure kaynakları oluşturmak için bir Kaynak Yöneticisi şablonu dağıtabilirsiniz.

### <a name="deploy-a-resource"></a>Kaynak dağıtma

Aşağıdaki komut dosyası bir depolama hesabı oluşturur.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account create --resource-group $resourceGroupName --name $storageAccountName --location $location --sku Standard_LRS --kind StorageV2 &&
az storage account show --resource-group $resourceGroupName --name $storageAccountName 
```

### <a name="deploy-a-template"></a>Şablon dağıtma

Aşağıdaki komut dosyası, bir depolama hesabı oluşturmak için bir Quickstart şablonu dağıtır. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Visual Studio Code'u kullanarak Azure Kaynak Yöneticisi şablonları oluşturun.](../templates/quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group deployment create --resource-group $resourceGroupName --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt'a](../templates/deploy-cli.md)bakın.

## <a name="deploy-a-resource-group-and-resources"></a>Kaynak grubu ve kaynakları dağıtma

Bir kaynak grubu oluşturabilir ve kaynakları gruba dağıtabilirsiniz. Daha fazla bilgi için kaynak [grubu oluştur ve kaynakları dağıtı'](../templates/deploy-to-subscription.md#resource-group-and-resources)ya bakın.

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>Kaynakları birden çok abonelik veya kaynak grubuna dağıtma

Genellikle, şablonunuzdaki tüm kaynakları tek bir kaynak grubuna dağıtırsınız. Ancak, bir dizi kaynağı birlikte dağıtmak ancak bunları farklı kaynak gruplarına veya aboneliklere yerleştirmek istediğiniz senaryolar vardır. Daha fazla bilgi için azure [kaynaklarını birden çok abonelik veya kaynak grubuna dağıt'a](../templates/cross-resource-group-deployment.md)bakın.

## <a name="delete-resources"></a>Kaynakları silme

Aşağıdaki komut dosyası, bir depolama hesabının nasıl silinir olduğunu gösterir.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account delete --resource-group $resourceGroupName --name $storageAccountName 
```

Azure Kaynak Yöneticisi'nin kaynakların silinmesini nasıl emrettikleri hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi kaynak silme](delete-resource-group.md)bölümüne bakın.

## <a name="move-resources"></a>Kaynakları taşıma

Aşağıdaki komut dosyası, bir depolama hesabının bir kaynak grubundan başka bir kaynak grubuna nasıl kaldırılır olduğunu gösterir.

```azurecli-interactive
echo "Enter the source Resource Group name:" &&
read srcResourceGroupName &&
echo "Enter the destination Resource Group name:" &&
read destResourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
storageAccount=$(az resource show --resource-group $srcResourceGroupName --name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --query id --output tsv) &&
az resource move --destination-group $destResourceGroupName --ids $storageAccount
```

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

## <a name="lock-resources"></a>Kaynakları kilitleme

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi kritik kaynakları yanlışlıkla siler veya değiştirmesini engeller. 

Aşağıdaki komut dosyası, hesabın silinmesin diye bir depolama hesabını kilitler.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock create --name LockSite --lock-type CanNotDelete --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts 
```

Aşağıdaki komut dosyası, bir depolama hesabı için tüm kilitleri alır:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock list --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --parent ""
```

Aşağıdaki komut dosyası, bir depolama hesabının kilitlerini siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
lockId=$(az lock show --name LockSite --resource-group $resourceGroupName --resource-type Microsoft.Storage/storageAccounts --resource-name $storageAccountName --output tsv --query id)&&
az lock delete --ids $lockId
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](lock-resources.md).

## <a name="tag-resources"></a>Kaynakları etiketleme

Etiketleme, kaynak grubunuzu ve kaynaklarınızı mantıksal olarak düzenlemeye yardımcı olur. Bilgi için Azure [kaynaklarınızı düzenlemek için etiketleri kullanma'ya](tag-resources.md#azure-cli)bakın.

## <a name="manage-access-to-resources"></a>Kaynaklara erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure CLI'yi kullanarak erişimi yönet'e](../../role-based-access-control/role-assignments-cli.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kaynak Yöneticisi'ni öğrenmek için [Azure Kaynak Yöneticisi'ne genel bakış](overview.md)'a bakın.
- Kaynak Yöneticisi şablonsözdizimini öğrenmek için [bkz.](../templates/template-syntax.md)
- Şablonları nasıl geliştireceklerini öğrenmek için [adım adım öğreticilere](/azure/azure-resource-manager/)bakın.
- Azure Kaynak Yöneticisi şablon şemalarını görüntülemek için [şablon başvurusuna](/azure/templates/)bakın.
