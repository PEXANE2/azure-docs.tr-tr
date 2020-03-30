---
title: Azure Geçiş Sunucusu Geçişi'ni kullanarak VMware sanal makinelerini sunucu tarafı şifreleme (SSE) ve müşteri tarafından yönetilen anahtarlarla (CMK) Azure'a geçirin
description: Azure Geçiş Sunucusu Geçişi'ni kullanarak Sunucu tarafı şifreleme (SSE) ve müşteri tarafından yönetilen anahtarlar (CMK) ile VMware VM'leri Azure'a nasıl geçirebilirsiniz öğrenin
author: bsiva
ms.service: azure-migrate
ms.manager: carmonm
ms.topic: article
ms.date: 03/12/2020
ms.author: raynew
ms.openlocfilehash: c6b791fda43a018a26204b2b43dc1e581ff3a945
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269490"
---
# <a name="migrate-vmware-vms-to-azure-vms-enabled-with-server-side-encryption-and-customer-managed-keys"></a>VMware VM'leri sunucu tarafı şifrelemesi ve müşteri tarafından yönetilen anahtarlarla etkinleştirilen Azure VM'lere geçirin

Bu makalede, VMware VM'lerin, müşteri tarafından yönetilen anahtarlarla (CMK) sunucu tarafı şifrelemesi (CMK) kullanılarak şifrelenmiş diskleri olan Azure sanal makinelerine Azure sanal makinelerine nasıl geçirilen Azure'a nasıl geçirilebilirsiniz, Azure Geçiş Sunucusu Geçişi (aracısız çoğaltma) kullanılır.

Azure Geçir Sunucusu Geçişi portalı deneyimi, [VMware VM'leri aracısız çoğaltma yla Azure'a geçirmenize](tutorial-migrate-vmware.md) olanak tanır. Portal deneyimi şu anda Azure'da çoğaltılan diskleriniz için CMK ile SSE'yi açma olanağı sunmuyor. Çoğaltılan diskler için CMK ile SSE'yi açma özelliği şu anda yalnızca REST API ile kullanılabilir. Bu makalede, bir VMware VM'yi çoğaltmak ve Azure'da çoğaltılan diskleri CMK ile SSE'yi kullanacak şekilde yapılandırmak için bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/templates/overview.md) oluşturmave dağıtmayı göreceksiniz.

Bu makaledeki örnekler, Kaynak Yöneticisi şablonu oluşturmak ve dağıtmak için gereken görevleri gerçekleştirmek için [Azure PowerShell'i](/powershell/azure/new-azureps-module-az) kullanır.

Yönetilen diskler için müşteri yönetilen anahtarlarla (CMK) sunucu tarafı şifrelemesi (SSE) hakkında [daha fazla bilgi edinin.](../virtual-machines/windows/disk-encryption.md)

## <a name="prerequisites"></a>Ön koşullar

- Araç gereksinimlerini anlamak için VMware VM'lerin aracısız çoğaltmayla Azure'a geçişi yle ilgili [öğreticiyi gözden geçirin.](tutorial-migrate-vmware.md)
- Bir Azure Geçiş projesi oluşturmak ve projeye **Azure Geçiş: Sunucu Geçişi** aracını eklemek için bu yönergeleri [izleyin.](how-to-add-tool-first-time.md)
- VMware için Azure Geçir cihazını şirket içi ortamınızda kurmak ve tam keşif yapmak için [bu yönergeleri izleyin.](how-to-set-up-appliance-vmware.md)

## <a name="prepare-for-replication"></a>Çoğaltma için hazırlanın

VM keşfi tamamlandıktan sonra, Sunucu Geçişi döşemesindeki Keşfedilen Sunucular satırı, cihaz tarafından keşfedilen VMware VM sayısını gösterir.

VM'leri çoğaltmaya başlamadan önce çoğaltma altyapısının hazırlanması gerekir.

1. Hedef bölgede bir Hizmet Veri Yolu örneği oluşturun. Hizmet Veri Servisi, çoğaltma ve geçişi koordine etmek için Sunucu Geçişi hizmetiyle iletişim kurmak için şirket içi Azure Geçiş cihazı tarafından kullanılır.
2. Çoğaltmadan işlem günlüklerinin aktarılması için bir depolama hesabı oluşturun.
3. Azure Geçiş cihazının çoğaltma verilerini yüklediği bir depolama hesabı oluşturun.
4. Bir Anahtar Kasası oluşturun ve 3 ve 4.
5. Adım 1'de oluşturulan servis veri kurumu için paylaşılan bir erişim imzası belirteci oluşturun ve önceki adımda oluşturulan Anahtar Kasası'ndaki belirteç için bir sır oluşturun.
6. Şirket içi Azure Geçiş cihazını (cihaz AAD uygulamasını kullanarak) ve Sunucu Geçiş Hizmeti'nin Anahtar Kasası'na erişimini sağlamak için bir Key Vault erişim ilkesi oluşturun.
7. Bir çoğaltma ilkesi oluşturun ve Sunucu Geçişi hizmetini önceki adımda oluşturulan çoğaltma altyapısının ayrıntılarıyla yapılandırın.

Geçiş için hedefleme Azure bölgesinde ve VM'lerin geçirildiği hedef Azure aboneliğinde çoğaltma altyapısı oluşturulmalıdır.

Sunucu Geçişi portalı deneyimi, bir projede ilk kez bir VM'i kopyaladiğinizde bunu sizin için otomatik olarak yaparak çoğaltma altyapısının hazırlanmasını kolaylaştırır. Bu makalede, portal deneyimini kullanarak bir veya daha fazla VM'yi zaten kopyaladığınızı ve çoğaltma altyapısının zaten oluşturulduğunu varsayalım. Varolan çoğaltma altyapısının ayrıntılarını nasıl keşfedeceğimize ve bu ayrıntıları CMK ile çoğaltmayı ayarlamak için kullanılacak Kaynak Yöneticisi şablonuna girdi olarak nasıl kullanacağımızı inceeceğiz.

### <a name="identifying-replication-infrastructure-components"></a>Çoğaltma altyapısı bileşenlerinin tanımlanması

1. Azure portalında kaynak grupları sayfasına gidin ve Azure Geçiş projesinin oluşturulduğu kaynak grubunu seçin.
2. Sol menüden **Dağıtımlar'ı** seçin ve *"Microsoft.MigrateV2.VMwareV2EnableMigrate"* dizesiyle başlayan bir dağıtım adı arayın. Bu projedeki VM'ler için çoğaltma ayarlamak için portal deneyimi tarafından oluşturulan Kaynak Yöneticisi şablonlarının listesini görürsünüz. Böyle bir şablon indireceğiz ve bunu cmk ile çoğaltma için şablonu hazırlamak için temel olarak kullanacağız.
3. Şablonu indirmek için, önceki adımda dize deseniyle eşleşen herhangi bir dağıtımı seçin > sol menüden **Şablon'u** seçin > üst menüden **İndir'e** tıklayın. template.json dosyasını yerel olarak kaydedin. Bu şablon dosyasını son adımda edineceksiniz.

## <a name="create-a-disk-encryption-set"></a>Disk Şifreleme Kümesi Oluşturma

Disk şifreleme, Yönetilen Diskler eşlemlerini SSE için kullanılacak CMK'yı içeren bir Anahtar Kasası olarak ayarlar. CMK ile VM'leri çoğaltmak için bir disk şifreleme kümesi oluşturur ve çoğaltma işlemine giriş olarak aktarırsınız.

Azure PowerShell'i kullanarak bir disk şifreleme kümesi oluşturmak için [aşağıdaki](../virtual-machines/windows/disk-encryption.md#powershell) örneği izleyin. Disk şifreleme kümesinin, VM'lerin geçirildiği hedef abonelikte ve geçiş için hedef Azure bölgesinde oluşturulduğundan emin olun.

```azurepowershell
$Location = "southcentralus"                           #Target Azure region for migration 
$TargetResourceGroupName = "ContosoMigrationTarget"
$KeyVaultName = "ContosoCMKKV"
$KeyName = "ContosoCMKKey"
$KeyDestination = "Software"
$DiskEncryptionSetName = "ContosoCMKDES"

$KeyVault = New-AzKeyVault -Name $KeyVaultName -ResourceGroupName $TargetResourceGroupName -Location $Location -EnableSoftDelete -EnablePurgeProtection

$Key = Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $KeyName -Destination $KeyDestination

$desConfig = New-AzDiskEncryptionSetConfig -Location $Location -SourceVaultId $KeyVault.ResourceId -KeyUrl $Key.Key.Kid -IdentityType SystemAssigned

$des = New-AzDiskEncryptionSet -Name $DiskEncryptionSetName -ResourceGroupName $TargetResourceGroupName -InputObject $desConfig

Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get

New-AzRoleAssignment -ResourceName $KeyVaultName -ResourceGroupName $TargetResourceGroupName -ResourceType "Microsoft.KeyVault/vaults" -ObjectId $des.Identity.PrincipalId -RoleDefinitionName "Reader"
```

## <a name="get-details-of-the-vmware-vm-to-migrate"></a>Geçiş yapmak için VMware VM'nin ayrıntılarını alın

Bu adımda, geçirilmesi gereken VM'nin ayrıntılarını almak için Azure PowerShell'i kullanırsınız. Bu ayrıntılar çoğaltma için Kaynak Yöneticisi şablonu oluşturmak için kullanılır. Özellikle, ilgi iki özellikleri şunlardır:

- Keşfedilen VM'ler için makine Kaynak Kimliği.
- VM ve disk tanımlayıcıları için disklerin listesi.

```azurepowershell

$ProjectResourceGroup = "ContosoVMwareCMK"   #Resource group that the Azure Migrate Project is created in
$ProjectName = "ContosoVMwareCMK"            #Name of the Azure Migrate Project

$solution = Get-AzResource -ResourceGroupName $ProjectResourceGroup -ResourceType Microsoft.Migrate/MigrateProjects/solutions -ExpandProperties -ResourceName $ProjectName | where Name -eq "Servers-Discovery-ServerDis
covery"

# Displays one entry for each appliance in the project mapping the appliance to the VMware sites discovered through the appliance.
$solution.Properties.details.extendedDetails.applianceNameToSiteIdMapV2 | ConvertFrom-Json | select ApplianceName, SiteId
```
```Output
ApplianceName  SiteId
-------------  ------
VMwareApplianc /subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.OffAzure/VMwareSites/VMwareApplianca8basite
```

VM'nin bulunduğu Azure Geçiş cihazına karşılık gelen SiteKimliği dizesinin değerini kopyalayın. Yukarıda gösterilen örnekte, SiteId *"/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.OffAzure/VMwareSites/VMwareApplianca8basite"*

```azurepowershell

#Replace value with SiteId from the previous step
$SiteId = "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.OffAzure/VMwareSites/VMwareApplianca8basite"
$SiteName = Get-AzResource -ResourceId $SiteId -ExpandProperties | Select-Object -ExpandProperty Name
$DiscoveredMachines = Get-AzResource -ResourceGroupName $ProjectResourceGroup -ResourceType Microsoft.OffAzure/VMwareSites/machines  -ExpandProperties -ResourceName $SiteName

#Get machine details
PS /home/bharathram> $MachineName = "FPL-W19-09"     #Replace string with VMware VM name of the machine to migrate
PS /home/bharathram> $machine = $Discoveredmachines | where {$_.Properties.displayName -eq $MachineName}
PS /home/bharathram> $machine.count   #Validate that only 1 VM was found matching this name.
```

Geçirilecek makine için ResourceId, ad ve disk uuid değerlerini kopyalayın.
```Output
PS > $machine.Name
10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_50098f99-f949-22ca-642b-724ec6595210
PS > $machine.ResourceId
/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.OffAzure/VMwareSites/VMwareApplianca8basite/machines/10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_50098f99-f949-22ca-642b-724ec6595210

PS > $machine.Properties.disks | select uuid, label, name, maxSizeInBytes

uuid                                 label       name    maxSizeInBytes
----                                 -----       ----    --------------
6000C291-5106-2aac-7a74-4f33c3ddb78c Hard disk 1 scsi0:0    42949672960
6000C293-39a1-bd70-7b24-735f0eeb79c4 Hard disk 2 scsi0:1    53687091200
6000C29e-cbee-4d79-39c7-d00dd0208aa9 Hard disk 3 scsi0:2    53687091200

```

## <a name="create-resource-manager-template-for-replication"></a>Çoğaltma için Kaynak Yöneticisi şablonu oluşturma

- **Tanımlama çoğaltma altyapısı bileşenlerinde** indirdiğiniz Kaynak Yöneticisi şablon dosyasını seçtiğiniz bir düzenleyicide açın.
- *"Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems"* türündeki kaynaklar dışında şablondaki tüm kaynak tanımlarını kaldırın
- Yukarıdaki türe ait birden çok kaynak tanımı varsa, biri hariç tümünün kaldırılmasını kaldırın. Kaynak tanımından özellik tanımlarına **bağlı** olarak kaldırın.
- Bu adımın sonunda, aşağıdaki örnek gibi görünen ve aynı özellik kümesine sahip bir dosyanız olmalıdır.

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems",
            "apiVersion": "2018-01-10",
            "name": "ContosoMigration7371rsvault/VMware104e4replicationfabric/VMware104e4replicationcontainer/10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_500937f3-805e-9414-11b1-f22923456e08",
            "properties": {
                "policyId": "/Subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/ContosoMigration/providers/Microsoft.RecoveryServices/vaults/ContosoMigration7371rsvault/replicationPolicies/migrateVMware104e4sitepolicy",
                "providerSpecificDetails": {
                    "instanceType": "VMwareCbt",
                    "vmwareMachineId": "/subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/ContosoMigration/providers/Microsoft.OffAzure/VMwareSites/VMware104e4site/machines/10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_500937f3-805e-9414-11b1-f22923456e08",
                    "targetResourceGroupId": "/subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/PayrollRG",
                    "targetNetworkId": "/subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/PayrollRG/providers/Microsoft.Network/virtualNetworks/PayrollNW",
                    "targetSubnetName": "PayrollSubnet",
                    "licenseType": "NoLicenseType",
                    "disksToInclude": [
                        {
                            "diskId": "6000C295-dafe-a0eb-906e-d47cb5b05a1d",
                            "isOSDisk": "true",
                            "logStorageAccountId": "/subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/ContosoMigration/providers/Microsoft.Storage/storageAccounts/migratelsa1432469187",
                            "logStorageAccountSasSecretName": "migratelsa1432469187-cacheSas",
                            "diskType": "Standard_LRS"
                        }
                    ],
                    "dataMoverRunAsAccountId": "/subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/ContosoMigration/providers/Microsoft.OffAzure/VMwareSites/VMware104e4site/runasaccounts/b090bef3-b733-5e34-bc8f-eb6f2701432a",
                    "snapshotRunAsAccountId": "/subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/ContosoMigration/providers/Microsoft.OffAzure/VMwareSites/VMware104e4site/runasaccounts/b090bef3-b733-5e34-bc8f-eb6f2701432a",
                    "targetBootDiagnosticsStorageAccountId": "/subscriptions/6785ea1f-ac40-4244-a9ce-94b12fd832ca/resourceGroups/ContosoMigration/providers/Microsoft.Storage/storageAccounts/migratelsa1432469187",
                    "targetVmName": "PayrollWeb04"
                }
            }
        }
    ]
}
```

- Kaynak tanımındaki **ad** özelliğini edin. Ad özelliğindeki son "/" dizesini $machine değeriyle *değiştirin. Ad*(önceki adımdan).
- özelliklerinin değerini **değiştirin.providerSpecificDetails.vmwareMachineId** özelliği$machine değeri *ile. ResourceId*(önceki adımdan).
- **HedefResourceGroupId,** **targetNetworkId,** **targetSubnetName** için hedef kaynak grubu kimliği, hedef sanal ağ kaynak kimliği ve hedef alt net adı sırasıyla değerleri ayarlayın.
- Bu VM için Azure Karma Avantajı uygulamak için **licenseType** değerini "WindowsServer" olarak ayarlayın. Bu VM Azure Karma Avantajı için uygun değilse, **licenseType** değerini NoLicenseType olarak ayarlayın.
- **HedefVmName** özelliğinin değerini, geçirilen VM için istenen Azure sanal makine adı ile değiştirin.
- İsteğe bağlı olarak **targetVmName** özelliğinin altına **targetVmSize** adlı bir özellik ekleyin. **Hedeflenen VmSize** özelliğinin değerini, geçirilen VM için istenen Azure sanal makine boyutuna ayarlayın.
- **disksToInclude** özelliği, her liste öğesi nin bir şirket içi diski temsil eden çoğaltma için disk girişlerinin listesidir. Şirket içi VM'deki disk sayısı kadar liste öğesi oluşturun. Liste öğesindeki **diskid** özelliğini önceki adımda tanımlanan disklerin uuid'ine değiştirin. **ISOSDisk** değerini VM işletim sistemi diski için "true" ve diğer tüm diskler için "false" olarak ayarlayın. **LogStorageAccountId'i** ve **logStorageAccountSasSecretName** özelliklerini değiştirmeden bırakın. **DiskType** değerini, disk için kullanmak üzere Azure Yönetilen Disk türüne *(Standard_LRS, Premium_LRS StandardSSD_LRS)* ayarlayın. CMK ile şifrelenebilen diskler için **diskEncryptionSetId** adında bir özellik ekleyin ve oluşturulan disk şifreleme kümesinin kaynak kimliğine ($des değerini**ayarlayın. Disk**Şifreleme *Kümesi Oluşturma* adımında Id )
- Düzenlenen şablon dosyasını kaydedin. Yukarıdaki örnekte, düzenlenen şablon dosyası aşağıdaki gibi görünür:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems",
            "apiVersion": "2018-01-10",
            "name": "ContosoVMwareCMK00ddrsvault/VMwareApplianca8bareplicationfabric/VMwareApplianca8bareplicationcontainer/10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_50098f99-f949-22ca-642b-724ec6595210",
            "properties": {
                "policyId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.RecoveryServices/vaults/ContosoVMwareCMK00ddrsvault/replicationPolicies/migrateVMwareApplianca8basitepolicy",
                "providerSpecificDetails": {
                    "instanceType": "VMwareCbt",
                    "vmwareMachineId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.OffAzure/VMwareSites/VMwareApplianca8basite/machines/10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_50098f99-f949-22ca-642b-724ec6595210",
                    "targetResourceGroupId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoMigrationTarget",
                    "targetNetworkId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/cmkRTest/providers/Microsoft.Network/virtualNetworks/cmkvm1_vnet",
                    "targetSubnetName": "cmkvm1_subnet",
                    "licenseType": "NoLicenseType",
                    "disksToInclude": [
                        {
                            "diskId": "6000C291-5106-2aac-7a74-4f33c3ddb78c",
                            "isOSDisk": "true",
                            "logStorageAccountId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.Storage/storageAccounts/migratelsa1671875959",
                            "logStorageAccountSasSecretName": "migratelsa1671875959-cacheSas",
                            "diskEncryptionSetId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/CONTOSOMIGRATIONTARGET/providers/Microsoft.Compute/diskEncryptionSets/ContosoCMKDES",
                            "diskType": "Standard_LRS"
                        },
                        {
                            "diskId": "6000C293-39a1-bd70-7b24-735f0eeb79c4",
                            "isOSDisk": "false",
                            "logStorageAccountId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.Storage/storageAccounts/migratelsa1671875959",
                            "logStorageAccountSasSecretName": "migratelsa1671875959-cacheSas",
                            "diskEncryptionSetId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/CONTOSOMIGRATIONTARGET/providers/Microsoft.Compute/diskEncryptionSets/ContosoCMKDES",
                            "diskType": "Standard_LRS"
                        },
                        {
                            "diskId": "6000C29e-cbee-4d79-39c7-d00dd0208aa9",
                            "isOSDisk": "false",
                            "logStorageAccountId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.Storage/storageAccounts/migratelsa1671875959",
                            "logStorageAccountSasSecretName": "migratelsa1671875959-cacheSas",
                            "diskEncryptionSetId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/CONTOSOMIGRATIONTARGET/providers/Microsoft.Compute/diskEncryptionSets/ContosoCMKDES",
                            "diskType": "Standard_LRS"
                        }
                    ],
                    "dataMoverRunAsAccountId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.OffAzure/VMwareSites/VMwareApplianca8basite/runasaccounts/b090bef3-b733-5e34-bc8f-eb6f2701432a",
                    "snapshotRunAsAccountId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.OffAzure/VMwareSites/VMwareApplianca8basite/runasaccounts/b090bef3-b733-5e34-bc8f-eb6f2701432a",
                    "targetBootDiagnosticsStorageAccountId": "/subscriptions/509099b2-9d2c-4636-b43e-bd5cafb6be69/resourceGroups/ContosoVMwareCMK/providers/Microsoft.Storage/storageAccounts/migratelsa1671875959",
                    "performAutoResync": "true",
                    "targetVmName": "FPL-W19-09"
                }
            }
        }
    ]
}
```

## <a name="set-up-replication"></a>Çoğaltmayı ayarlama

Artık VM için çoğaltma ayarlamak için düzenlenen Kaynak Yöneticisi şablonu proje kaynak grubuna dağıtabilirsiniz. [Azure Kaynak Yöneticisi şablonları ve Azure PowerShell ile kaynak dağıtmayı](../azure-resource-manager/templates/deploy-powershell.md) öğrenin

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName $ProjectResourceGroup -TemplateFile "C:\Users\Administrator\Downloads\template.json"
```

```Output
DeploymentName          : template
ResourceGroupName       : ContosoVMwareCMK
ProvisioningState       : Succeeded
Timestamp               : 3/11/2020 8:52:00 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
Outputs                 :
DeploymentDebugLogLevel :

```

## <a name="next-steps"></a>Sonraki adımlar

Portal deneyimi aracılığıyla çoğaltma durumunu [izleyin](tutorial-migrate-vmware.md#track-and-monitor) ve Test geçişleri ve geçişleri gerçekleştirin.
