---
title: Hızlı başlangıç-Kaynak Yöneticisi şablonu VM yedeklemesi
description: Azure Resource Manager şablonuyla sanal makinelerinizi nasıl yedekleyeceğinizi öğrenin
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 05/14/2019
ms.custom: mvc,subject-armqs
ms.openlocfilehash: d6fb73801f0f460daf2ed70f8dc88187e41ea887
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81458854"
---
# <a name="back-up-a-virtual-machine-in-azure-with-resource-manager-template"></a>Azure 'da Kaynak Yöneticisi şablonuyla bir sanal makineyi yedekleme

[Azure Backup](backup-overview.md) , şirket içi makineleri ve uygulamaları ve Azure VM 'lerini yedekler. Bu makalede, bir Azure VM 'yi Kaynak Yöneticisi şablonuyla ve Azure PowerShell nasıl yedekleyeceğiniz gösterilmektedir. Bu hızlı başlangıç, bir kurtarma hizmetleri Kasası oluşturmak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanmaktadır. Kaynak Yöneticisi şablonları geliştirme hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager/) ve [şablon başvurusu](/azure/templates/microsoft.recoveryservices/allversions).

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Alternatif olarak, [Azure PowerShell](./quick-backup-vm-powershell.md), [Azure CLI](quick-backup-vm-cli.md)veya [Azure Portal](quick-backup-vm-portal.md)kullanarak bir VM 'yi yedekleyebilirsiniz.

## <a name="create-a-vm-and-recovery-services-vault"></a>VM ve kurtarma hizmetleri Kasası oluşturma

[Kurtarma Hizmetleri Kasası](backup-azure-recovery-services-vault-overview.md) , Azure VM 'leri gibi korunan kaynaklar için yedekleme verilerini depolayan bir mantıksal kapsayıcıdır. Bir yedekleme işi çalıştırıldığında, kurtarma hizmetleri kasasının içinde bir kurtarma noktası oluşturur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/). Bu şablon, koruma için DefaultPolicy ile yapılandırılmış basit Windows VM ve kurtarma hizmetleri kasasını dağıtmanıza olanak tanır.

:::code language="json" source="~/quickstart-templates/101-recovery-services-create-vm-and-configure-backup/azuredeploy.json" range="1-247" highlight="221-245":::

Şablonda tanımlanan kaynaklar şunlardır:

- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageaccounts)
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft. COMPUTE/virutalMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. RecoveryServices/kasa**](/azure/templates/microsoft.recoveryservices/2016-06-01/vaults)
- [**Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/korunabilir**](/azure/templates/microsoft.recoveryservices/2016-06-01/vaults/backupfabrics/protectioncontainers/protecteditems)

### <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonu dağıtmak için **dene** ' yi seçerek Azure Cloud Shell açın ve sonra aşağıdaki PowerShell betiğini kabuk penceresine yapıştırın. Kodu yapıştırmak için kabuk penceresine sağ tıklayıp **Yapıştır**' ı seçin.

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

Azure PowerShell, bu hızlı başlangıçta Kaynak Yöneticisi şablonunu dağıtmak için kullanılır. [Azure Portal](../azure-resource-manager/templates/deploy-portal.md), [Azure CLı](../azure-resource-manager/templates/deploy-cli.md)ve [REST API](../azure-resource-manager/templates/deploy-rest.md) , şablon dağıtmak için de kullanılabilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

### <a name="start-a-backup-job"></a>Bir yedekleme işi başlatma

Şablon bir VM oluşturur ve VM 'yi yeniden sunar. Şablonu dağıttıktan sonra bir yedekleme işi başlatmanız gerekir. Daha fazla bilgi için bkz. [bir yedekleme Işi başlatma](./quick-backup-vm-powershell.md#start-a-backup-job).

### <a name="monitor-the-backup-job"></a>Yedekleme işini izleme

Yedekleme işini izlemek için bkz. [yedekleme Işini izleme](./quick-backup-vm-powershell.md#monitor-the-backup-job).

## <a name="clean-up-the-deployment"></a>Dağıtımı temizle

Artık VM 'yi yedeklemeniz gerekmiyorsa, temizleyebilirsiniz.

- VM 'yi geri yüklemeyi denemek istiyorsanız, temizleme işlemini atlayın.
- Var olan bir VM kullandıysanız, kaynak grubunu ve VM 'yi yerinde bırakmak için son [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet 'ini atlayabilirsiniz.

Korumayı devre dışı bırakın, geri yükleme noktalarını ve kasasını kaldırın. Daha sonra kaynak grubunu ve ilişkili VM kaynaklarını aşağıdaki gibi silin:

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzRecoveryServicesVault -Vault $vault
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Kurtarma Hizmetleri kasası oluşturdunuz, bir sanal makine için koruma özelliklerini etkinleştirdiniz ve ilk kurtarma noktasını oluşturdunuz.

- Azure portal VM 'Leri [nasıl yedekleyeceğinizi öğrenin](tutorial-backup-vm-at-scale.md) .
- Bir VM 'yi hızlıca geri yüklemeyi [öğrenin](tutorial-restore-disk.md)
- Kaynak Yöneticisi şablonları oluşturmayı [öğrenin](../azure-resource-manager/templates/template-tutorial-create-first-template.md) .
