---
title: Quickstart - Kaynak Yöneticisi şablonu VM Yedekleme
description: Azure Kaynak Yöneticisi şablonu yla sanal makinelerinizi nasıl yedekleyin izleyin
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 05/14/2019
ms.custom: mvc,subject-armqs
ms.openlocfilehash: c40dc7ef8fc55acade709b1ffbbd86ff306f7f0e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79459251"
---
# <a name="back-up-a-virtual-machine-in-azure-with-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla Azure'da sanal bir makineyi yedekleme

[Azure Yedekleme,](backup-overview.md) şirket içi makineleri ve uygulamaları ve Azure VM'leri yedekler. Bu makalede, Kaynak Yöneticisi şablonu ve Azure PowerShell ile bir Azure VM'yi nasıl yedeklediğinizi gösterilmektedir. Bu hızlı başlangıç, Hizmetleri Kurtarma kasası oluşturmak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanır. Kaynak Yöneticisi şablonları geliştirme hakkında daha fazla bilgi için [Kaynak Yöneticisi belgelerine](/azure/azure-resource-manager/) ve [şablon başvurusuna](/azure/templates/microsoft.recoveryservices/allversions)bakın.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Alternatif olarak, [Azure PowerShell](./quick-backup-vm-powershell.md)'i ( [Azure CLI)](quick-backup-vm-cli.md)kullanarak veya [Azure portalında](quick-backup-vm-portal.md)bir VM yedekleyebilirsiniz.

## <a name="create-a-vm-and-recovery-services-vault"></a>VM ve Kurtarma Hizmetleri kasası oluşturma

[Kurtarma Hizmetleri kasası,](backup-azure-recovery-services-vault-overview.md) Azure VM'leri gibi korumalı kaynaklar için yedekleme verilerini depolayan mantıksal bir kapsayıcıdır. Yedekleme işi çalıştığında, Kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/) Bu şablon, Varsayılan Koruma Politikası ile yapılandırılan basit Windows VM ve Kurtarma Hizmetleri Kasası dağıtmanızı sağlar.

:::code language="json" source="~/quickstart-templates/101-recovery-services-create-vm-and-configure-backup/azuredeploy.json" range="1-247" highlight="221-245":::

Şablonda tanımlanan kaynaklar şunlardır:

- [**Microsoft.Storage/storageHesapları**](/azure/templates/microsoft.storage/storageaccounts)
- [**Microsoft.Network/publicIPAdresleri**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Compute/virutalMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft.RecoveryServices/vaults**](/azure/templates/microsoft.recoveryservices/vaults)
- [**Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems**](/azure/templates/microsoft.recoveryservices/vaults/backupfabrics/protectioncontainers/protecteditems)

### <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonu dağıtmak için Azure Bulut Kabuğu'nu açmak için **Deneyin'i** seçin ve ardından aşağıdaki PowerShell komut dosyasını kabuk penceresine yapıştırın. Kodu yapıştırmak için kabuk penceresine sağ tıklayın ve ardından **Yapıştır'ı**seçin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name (limited to eight characters) that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username for the virtual machine"
$adminPassword = Read-Host -Prompt "Enter the administrator password for the virtual machine" -AsSecureString
$dnsPrefix = Read-Host -Prompt "Enter the unique DNS Name for the Public IP used to access the virtual machine"

$resourceGroupName = "${projectName}rg"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-recovery-services-create-vm-and-configure-backup/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -adminUsername $adminUsername -adminPassword $adminPassword -dnsLabelPrefix $dnsPrefix
```

Azure PowerShell, bu hızlı başlangıçta Kaynak Yöneticisi şablonu dağıtmak için kullanılır. [Azure portalı](../azure-resource-manager/templates/deploy-portal.md), [Azure CLI](../azure-resource-manager/templates/deploy-cli.md)ve [Rest API](../azure-resource-manager/templates/deploy-rest.md) şablonları dağıtmak için de kullanılabilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

### <a name="start-a-backup-job"></a>Bir yedekleme işi başlatma

Şablon bir VM oluşturur ve VM'de geri dönmeyi sağlar. Şablonu dağıttıktan sonra bir yedekleme işi başlatmanız gerekir. Daha fazla bilgi için [bkz.](./quick-backup-vm-powershell.md#start-a-backup-job)

### <a name="monitor-the-backup-job"></a>Yedekleme işini izleme

Yedekleme işini izlemek için [bkz.](./quick-backup-vm-powershell.md#monitor-the-backup-job)

## <a name="clean-up-the-deployment"></a>Dağıtımı temizleme

VM'yi artık yedeklemenize gerek yoksa, temizleyebilirsiniz.

- VM'yi geri geri getirmeye çalışmak istiyorsanız, temizlemeyi atlayın.
- Varolan bir VM kullandıysanız, kaynak grubunu ve VM'yi yerinde bırakmak için son [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini atlayabilirsiniz.

Korumayı devre dışı, geri yükleme noktalarını ve kasayı çıkarın. Ardından kaynak grubunu ve ilişkili VM kaynaklarını aşağıdaki gibi silin:

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzRecoveryServicesVault -Vault $vault
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Kurtarma Hizmetleri kasası oluşturdunuz, bir sanal makine için koruma özelliklerini etkinleştirdiniz ve ilk kurtarma noktasını oluşturdunuz.

- Azure portalında VM'leri [nasıl](tutorial-backup-vm-at-scale.md) yedekleyin öğrenin.
- VM'yi hızlı bir şekilde [nasıl](tutorial-restore-disk.md) geri yükleyin
- Kaynak Yöneticisi şablonlarını [nasıl](../azure-resource-manager/templates/template-tutorial-create-first-template.md) oluşturabilirsiniz öğrenin.
