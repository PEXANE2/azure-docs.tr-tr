---
title: Azure Yönetilen Disklerin sunucu tarafı şifrelemesi - PowerShell
description: Azure Depolama, depolama kümeleri için kalıcı hale gelmeden önce verilerinizi istirahatte şifreleyerek korur. Yönetilen disklerinizin şifrelemesi için Microsoft tarafından yönetilen anahtarlara güvenebilir veya şifrelemeyi kendi anahtarlarınızla yönetmek için müşteri tarafından yönetilen anahtarları kullanabilirsiniz.
author: roygara
ms.date: 04/21/2020
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines-windows
ms.subservice: disks
ms.openlocfilehash: 3f6039382b970ca3b0d77042c60cb720f96466d2
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759204"
---
# <a name="server-side-encryption-of-azure-managed-disks"></a>Azure yönetilen disklerin sunucu tarafı şifrelemesi

Azure yönetilen diskler, verilerinizi bulutta kalıcı hale getirirken varsayılan olarak verilerinizi otomatik olarak şifreler. Sunucu tarafı şifreleme verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. Azure yönetilen disklerde veriler, mevcut en güçlü blok şifrelerinden biri olan 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak saydam olarak şifrelenir ve FIPS 140-2 uyumludur.

Şifreleme yönetilen disklerin performansını etkilemez. Şifreleme için ek bir maliyet yoktur.

Azure yönetilen disklerin altında yatan şifreleme modülleri hakkında daha fazla bilgi için [şifreleme API: Yeni Nesil](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Yönetilen diskinizin şifrelemesi için platform tarafından yönetilen anahtarlara güvenebilir veya kendi anahtarlarınızı kullanarak şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınızla yönetmeyi seçerseniz, yönetilen disklerde tüm verileri şifrelemek ve çözmek için kullanılacak *müşteri tarafından yönetilen* bir anahtar belirtebilirsiniz. 

Aşağıdaki bölümlerde anahtar yönetimi seçeneklerinin her biri daha ayrıntılı olarak açıklayınız.

## <a name="platform-managed-keys"></a>Platform yönetilen tuşlar

Varsayılan olarak, yönetilen diskler platform tarafından yönetilen şifreleme anahtarlarını kullanır. 10 Haziran 2017 itibariyle, mevcut yönetilen disklere yazılan tüm yeni yönetilen diskler, anlık görüntüler, görüntüler ve yeni veriler platform tarafından yönetilen tuşlarla otomatik olarak şifrelenir.

## <a name="customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar

Şifrelemeyi yönetilen her disk düzeyinde kendi anahtarlarınızla yönetmeyi seçebilirsiniz. Müşteri tarafından yönetilen anahtarlara sahip yönetilen diskler için sunucu tarafı şifrelemesi, Azure Key Vault ile entegre bir deneyim sunar. [RSA anahtarlarınızı](../../key-vault/keys/hsm-protected-keys.md) Key Vault'unuza aktarabilir veya Azure Key Vault'ta yeni RSA anahtarları oluşturabilirsiniz. 

Azure yönetilen [diskler, zarf şifrelemesini](../../storage/common/storage-client-side-encryption.md#encryption-and-decryption-via-the-envelope-technique)kullanarak şifreleme ve şifre çözme işlemlerini tamamen saydam bir şekilde işler. Verileri [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 256 tabanlı veri şifreleme anahtarı (DEK) kullanarak şifreler ve bu da anahtarlarınızı kullanarak korunur. Depolama hizmeti veri şifreleme anahtarları oluşturur ve RSA şifrelemekullanarak müşteri tarafından yönetilen anahtarlarla şifreler. Zarf şifreleme, anahtarlarınızı VM'lerinizi etkilemeden uyumluluk ilkelerinize göre düzenli aralıklarla döndürmenize (değiştirmenize) olanak tanır. Anahtarlarınızı döndürdüğünüzde, Depolama hizmeti veri şifreleme anahtarlarını yeni müşteri tarafından yönetilen anahtarlarla yeniden şifreler. 

DEK'i şifrelemek ve şifresini çözmek için anahtarlarınızı kullanmak için Key Vault'unuzdaki yönetilen disklere erişim izni verebilirsiniz. Bu, verilerinizin ve anahtarlarınızın tam denetimini sağlar. Anahtarlarınızı devre dışı kılabilir veya yönetilen disklere erişimi istediğiniz zaman iptal edebilirsiniz. Yalnızca yönetilen disklerin veya diğer güvenilen Azure hizmetlerinin anahtarlarınıza erişdiğinden emin olmak için Şifreleme anahtarı kullanımını Azure Key Vault izleme ile de denetleyebilirsiniz.

Birinci sınıf SSD'ler, standart SSD'ler ve standart HDD'ler için: Anahtarınızı devre dışı kaldırdığınızda veya sildiğinizde, bu anahtarı kullanan disklere sahip tüm VM'ler otomatik olarak kapanır. Bundan sonra, anahtar yeniden etkinleştirilmedikçe veya yeni bir anahtar atamadıkça VM'ler kullanılabilir olmayacaktır.

Ultra diskler için, bir anahtarı devre dışı veya sildiğinizde, anahtarı kullanan ultra diskli VM'ler otomatik olarak kapanmaz. VM'leri bulup yeniden başlattığınızda diskler anahtarı kullanmayı durdurur ve ardından VM'ler yeniden çevrimiçi olmaz. VM'leri yeniden çevrimiçi duruma getirmek için yeni bir anahtar atamanız veya varolan anahtarı etkinleştirmeniz gerekir.

Aşağıdaki diyagram, yönetilen disklerin müşteri tarafından yönetilen anahtarı kullanarak istekte bulunmak için Azure Etkin Dizin ve Azure Anahtar Kasası'nı nasıl kullandığını gösterir:

![Yönetilen disk ve müşteri tarafından yönetilen anahtarlar iş akışı. Yönetici bir Azure Anahtar Kasası oluşturur, ardından bir disk şifreleme kümesi oluşturur ve disk şifreleme kümesini ayarlar. Set, diskin kimlik doğrulaması için Azure AD'den yararlanmasını sağlayan bir VM ile ilişkilidir](media/disk-storage-encryption/customer-managed-keys-sse-managed-disks-workflow.png)


Aşağıdaki liste diyagramı daha ayrıntılı olarak açıklar:

1. Azure Key Vault yöneticisi önemli kasa kaynakları oluşturur.
1. Anahtar kasa yöneticisi ya Key Vault onların RSA anahtarları ithal veya Key Vault yeni RSA anahtarları oluşturmak.
1. Bu yönetici, Azure Anahtar Vault Kimliği ve önemli bir URL belirterek Disk Şifreleme Kümesi kaynağının bir örneğini oluşturur. Disk Şifreleme Kümesi, yönetilen diskler için anahtar yönetimini basitleştirmek için sunulan yeni bir kaynaktır. 
1. Bir disk şifreleme kümesi oluşturulduğunda, Azure Active Directory 'de (AD) [sistem tarafından atanmış yönetilen](../../active-directory/managed-identities-azure-resources/overview.md) bir kimlik oluşturulur ve disk şifreleme kümesiyle ilişkilendirilir. 
1. Azure anahtar kasası yöneticisi daha sonra anahtar kasasında işlemleri gerçekleştirmek için yönetilen kimlik izni verir.
1. Bir VM kullanıcısı, disk şifreleme kümesiyle ilişkilendirerek diskler oluşturur. VM kullanıcısı, varolan kaynaklar için müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesini de disk şifreleme kümesiyle ilişkilendirerek etkinleştirebilir. 
1. Yönetilen diskler, Azure Anahtar Kasası'na istek göndermek için yönetilen kimliği kullanır.
1. Verileri okumak veya yazmak için yönetilen diskler, verilerin şifrelemesi ve şifresini çözmek için veri şifreleme anahtarını şifrelemek (kaydırmak) ve şifresini çözmek (açmak) için Azure Key Vault'a istekgönderir. 

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) ve Azure [Key Vault CLI'ye](https://docs.microsoft.com/cli/azure/keyvault)bakın. Şifreleme anahtarına Azure Depolama tarafından erişilemeden erişilebildiğinden, erişimi iptal etmek depolama hesabındaki tüm verilere erişimi etkili bir şekilde engeller.

### <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-encryption-regions](../../../includes/virtual-machines-disks-encryption-regions.md)]

### <a name="restrictions"></a>Kısıtlamalar

Şimdilik, müşteri tarafından yönetilen anahtarlar aşağıdaki kısıtlamalara sahiptir:

- Bu özellik diskiniz için etkinleştirilmişse, devre dışı kalamazsınız.
    Bu işi halletmeniz gerekiyorsa, tüm verileri müşteri tarafından yönetilen anahtarları kullanmayan tamamen farklı yönetilen bir diske [kopyalamanız](disks-upload-vhd-to-managed-disk-powershell.md#copy-a-managed-disk) gerekir.
- Sadece ["yumuşak" ve "sert" RSA boyutları](../../key-vault/keys/about-keys.md) 2080, başka anahtar lar veya boyutlarda desteklenir.
- Sunucu tarafı şifrelemesi ve müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenen özel görüntülerden oluşturulan diskler, aynı müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmelidir ve aynı abonelikte olmalıdır.
- Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlarla şifrelenen disklerden oluşturulan anlık görüntüler, aynı müşteri tarafından yönetilen anahtarlarla şifrelenmelidir.
- Paylaşılan resim galerisinde sunucu tarafı şifrelemesi ve müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenen özel görüntüler kullanılamaz.
- Müşteri tarafından yönetilen anahtarlarınızla ilgili tüm kaynaklar (Azure Anahtar Kasaları, disk şifreleme kümeleri, VM'ler, diskler ve anlık görüntüler) aynı abonelik te ve bölgede olmalıdır.
- Müşteri tarafından yönetilen anahtarlarla şifrelenmiş diskler, anlık görüntüler ve görüntüler başka bir aboneye geçemez.
- Disk şifreleme setinizi oluşturmak için Azure portalını kullanıyorsanız, anlık görüntüleri şimdilik kullanamazsınız.
- Müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenen yönetilen diskler de Azure Disk Şifreleme ile şifrelenemez.

### <a name="powershell"></a>PowerShell

#### <a name="setting-up-your-azure-key-vault-and-diskencryptionset"></a>Azure Anahtar Kasanızı ve DiskŞifreleme Setinizi Ayarlama

1. En son [Azure PowerShell sürümünü](/powershell/azure/install-az-ps)yüklediğinizden ve Connect-AzAccount'ta bir Azure hesabında oturum açmış olduğunuzdan emin olun

1. Azure Key Vault ve şifreleme anahtarı örneği oluşturun.

    Key Vault örneğini oluştururken, yumuşak silme ve temizleme korumasını etkinleştirmelisiniz. Yumuşak silme, Anahtar Kasası'nın belirli bir bekletme süresi (90 günlük varsayılan) için silinmiş bir anahtar tutmasını sağlar. Temizleme koruması, silinen anahtarın bekletme süresi sona erinceye kadar kalıcı olarak silinmemesini sağlar. Bu ayarlar, yanlışlıkla silinme nedeniyle veri kaybetmenizi korur. Yönetilen diskleri şifrelemek için Bir Anahtar Kasası kullanırken bu ayarlar zorunludur.

    > [!IMPORTANT]
    > Azure portalındaki kaynağa ek diskler atarken sorunlarla karşılaşabilirsiniz.
    
    ```powershell
    $ResourceGroupName="yourResourceGroupName"
    $LocationName="westcentralus"
    $keyVaultName="yourKeyVaultName"
    $keyName="yourKeyName"
    $keyDestination="Software"
    $diskEncryptionSetName="yourDiskEncryptionSetName"

    $keyVault = New-AzKeyVault -Name $keyVaultName -ResourceGroupName $ResourceGroupName -Location $LocationName -EnableSoftDelete -EnablePurgeProtection

    $key = Add-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName -Destination $keyDestination  
    ```

1.    DiskEncryptionSet örneği oluşturun. 
    
        ```powershell
        $desConfig=New-AzDiskEncryptionSetConfig -Location $LocationName -SourceVaultId $keyVault.ResourceId -KeyUrl $key.Key.Kid -IdentityType SystemAssigned
        
        $des=New-AzDiskEncryptionSet -Name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName -InputObject $desConfig 
        ```

1.    DiskEncryptionSet kaynak erişimini anahtar kasasına ver.

        > [!NOTE]
        > Azure'un Azure Etkin Dizininizde DiskEncryptionSet'inizin kimliğini oluşturması birkaç dakika sürebilir. Aşağıdaki komutu çalıştırırken "Etkin Dizin nesnesini bulamıyor" gibi bir hata alırsanız, birkaç dakika bekleyin ve yeniden deneyin.
        
        ```powershell
        $identity = Get-AzADServicePrincipal -DisplayName myDiskEncryptionSet1  
         
        Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
         
        New-AzRoleAssignment -ResourceName $keyVaultName -ResourceGroupName $ResourceGroupName -ResourceType "Microsoft.KeyVault/vaults" -ObjectId $des.Identity.PrincipalId -RoleDefinitionName "Reader" 
        ```

#### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>İşletim sistemi ve veri disklerini müşteri tarafından yönetilen anahtarlarla şifreleyerek Market görüntüsünü kullanarak Bir VM oluşturun

```powershell
$VMLocalAdminUser = "yourVMLocalAdminUserName"
$VMLocalAdminSecurePassword = ConvertTo-SecureString <password> -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerName = "yourComputerName"
$VMName = "yourVMName"
$VMSize = "Standard_DS3_v2"
$diskEncryptionSetName="yourdiskEncryptionSetName"
    
$NetworkName = "yourNetworkName"
$NICName = "yourNICName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix
$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet
$NIC = New-AzNetworkInterface -Name $NICName -ResourceGroupName $ResourceGroupName -Location $LocationName -SubnetId $Vnet.Subnets[0].Id
    
$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);
    
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $($VMName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage

$VirtualMachine = Add-AzVMDataDisk -VM $VirtualMachine -Name $($VMName +"DataDisk1") -DiskSizeInGB 128 -StorageAccountType Premium_LRS -CreateOption Empty -Lun 0 -DiskEncryptionSetId $diskEncryptionSet.Id 
    
New-AzVM -ResourceGroupName $ResourceGroupName -Location $LocationName -VM $VirtualMachine -Verbose
```

#### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesini kullanarak şifrelenmiş boş bir disk oluşturun ve vm'ye takın

```PowerShell
$vmName = "yourVMName"
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$diskName = "yourDiskName"
$diskSKU = "Premium_LRS"
$diskSizeinGiB = 30
$diskLUN = 1
$diskEncryptionSetName="yourDiskEncryptionSetName"


$vm = Get-AzVM -Name $vmName -ResourceGroupName $ResourceGroupName 

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Empty -DiskSizeInGB $diskSizeinGiB -StorageAccountType $diskSKU -Lun $diskLUN -DiskEncryptionSetId $diskEncryptionSet.Id 

Update-AzVM -ResourceGroupName $ResourceGroupName -VM $vm

```

#### <a name="encrypt-existing-unattached-managed-disks"></a>Varolan eklenmemiş yönetilen diskleri şifreleme 

Aşağıdaki komut dosyasını kullanarak şifrelemeniz için varolan diskleriniz çalışan bir VM'ye eklenmemelidir:

```PowerShell
$rgName = "yourResourceGroupName"
$diskName = "yourDiskName"
$diskEncryptionSetName = "yourDiskEncryptionSetName"
 
$diskEncryptionSet = Get-AzDiskEncryptionSet -ResourceGroupName $rgName -Name $diskEncryptionSetName
 
New-AzDiskUpdateConfig -EncryptionType "EncryptionAtRestWithCustomerKey" -DiskEncryptionSetId $diskEncryptionSet.Id | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
```

#### <a name="create-a-virtual-machine-scale-set-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>İşletim sistemi ve veri disklerini müşteri tarafından yönetilen anahtarlarla şifreleyerek Pazar Yeri görüntüsünü kullanarak sanal bir makine ölçeği kümesi oluşturun

```PowerShell
$VMLocalAdminUser = "yourLocalAdminUser"
$VMLocalAdminSecurePassword = ConvertTo-SecureString Password@123 -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerNamePrefix = "yourComputerNamePrefix"
$VMScaleSetName = "yourVMSSName"
$VMSize = "Standard_DS3_v2"
$diskEncryptionSetName="yourDiskEncryptionSetName"
    
$NetworkName = "yourVNETName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix

$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet

$ipConfig = New-AzVmssIpConfig -Name "myIPConfig" -SubnetId $Vnet.Subnets[0].Id 

$VMSS = New-AzVmssConfig -Location $LocationName -SkuCapacity 2 -SkuName $VMSize -UpgradePolicyMode 'Automatic'

$VMSS = Add-AzVmssNetworkInterfaceConfiguration -Name "myVMSSNetworkConfig" -VirtualMachineScaleSet $VMSS -Primary $true -IpConfiguration $ipConfig

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

# Enable encryption at rest with customer managed keys for OS disk by setting DiskEncryptionSetId property 

$VMSS = Set-AzVmssStorageProfile $VMSS -OsDiskCreateOption "FromImage" -DiskEncryptionSetId $diskEncryptionSet.Id -ImageReferenceOffer 'WindowsServer' -ImageReferenceSku '2012-R2-Datacenter' -ImageReferenceVersion latest -ImageReferencePublisher 'MicrosoftWindowsServer'

$VMSS = Set-AzVmssOsProfile $VMSS -ComputerNamePrefix $ComputerNamePrefix -AdminUsername $VMLocalAdminUser -AdminPassword $VMLocalAdminSecurePassword

# Add a data disk encrypted at rest with customer managed keys by setting DiskEncryptionSetId property 

$VMSS = Add-AzVmssDataDisk -VirtualMachineScaleSet $VMSS -CreateOption Empty -Lun 1 -DiskSizeGB 128 -StorageAccountType Premium_LRS -DiskEncryptionSetId $diskEncryptionSet.Id

$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);

New-AzVmss -VirtualMachineScaleSet $VMSS -ResourceGroupName $ResourceGroupName -VMScaleSetName $VMScaleSetName
```

#### <a name="change-the-key-of-a-diskencryptionset-to-rotate-the-key-for-all-the-resources-referencing-the-diskencryptionset"></a>DiskEncryptionSet'e başvuran tüm kaynaklar için anahtarı döndürmek için Bir DiskEncryptionSet'in anahtarını değiştirme

```PowerShell
$ResourceGroupName="yourResourceGroupName"
$keyVaultName="yourKeyVaultName"
$keyName="yourKeyName"
$diskEncryptionSetName="yourDiskEncryptionSetName"

$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $ResourceGroupName

$keyVaultKey = Get-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName

Update-AzDiskEncryptionSet -Name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName -SourceVaultId $keyVault.ResourceId -KeyUrl $keyVaultKey.Id
```

#### <a name="find-the-status-of-server-side-encryption-of-a-disk"></a>Bir diskin sunucu tarafı şifreleme durumunu bulma

```PowerShell
$ResourceGroupName="yourResourceGroupName"
$DiskName="yourDiskName"

$disk=Get-AzDisk -ResourceGroupName $ResourceGroupName -DiskName $DiskName
$disk.Encryption.Type

```

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure Etkin Dizin (Azure AD) özelliği olan Azure kaynakları için yönetilen kimliklere güvenir. Müşteri tarafından yönetilen anahtarları yapılandırdığınızda, yönetilen kimlik kapaklarının altında kaynaklarınıza otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya yönetilen diski bir Azure REKLAM dizininden diğerine taşırsanız, yönetilen disklerle ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşteri tarafından yönetilen anahtarlar artık çalışmayabilir. Daha fazla bilgi için azure [REKLAM dizinleri arasında abonelik aktarma'ya](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories)bakın.

[!INCLUDE [virtual-machines-disks-encryption-portal](../../../includes/virtual-machines-disks-encryption-portal.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure Etkin Dizin (Azure AD) özelliği olan Azure kaynakları için yönetilen kimliklere güvenir. Müşteri tarafından yönetilen anahtarları yapılandırdığınızda, yönetilen kimlik kapaklarının altında kaynaklarınıza otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya yönetilen diski bir Azure REKLAM dizininden diğerine taşırsanız, yönetilen disklerle ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşteri tarafından yönetilen anahtarlar artık çalışmayabilir. Daha fazla bilgi için azure [REKLAM dizinleri arasında abonelik aktarma'ya](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories)bakın.

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Azure disk şifrelemeye karşı sunucu tarafı şifreleme

[Azure Disk Şifreleme,](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) yönetilen diskleri konuk VM'de müşteri tarafından yönetilen anahtarlarla şifrelemek için Windows'un [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) özelliğinden ve Linux'un [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğinden yararlanır.  Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme, Depolama hizmetindeki verileri şifreleyerek Sanal Tom'larınız için işletim sistemi türlerini ve görüntülerini kullanmanızı sağlayarak ADE'de iyileşir.

## <a name="next-steps"></a>Sonraki adımlar

- [Müşteri tarafından yönetilen anahtarlarla şifreli diskler oluşturmak için Azure Kaynak Yöneticisi şablonlarını keşfedin](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Azure Anahtar Kasası nedir?](../../key-vault/general/overview.md)
- [Müşteri tarafından yönetilen anahtarlarla makineleri çoğaltma](../../site-recovery/azure-to-azure-how-to-enable-replication-cmk-disks.md)
- [PowerShell ile VMware VM'lerin Azure'a olağanüstü durum kurtarmasını ayarlama](../../site-recovery/vmware-azure-disaster-recovery-powershell.md#replicate-vmware-vms)
- [PowerShell ve Azure Kaynak Yöneticisi'ni kullanarak Hyper-V V MM'ler için Azure'da olağanüstü durum kurtarma ayarlama](../../site-recovery/hyper-v-azure-powershell-resource-manager.md#step-7-enable-vm-protection)
