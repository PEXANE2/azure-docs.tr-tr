---
title: Azure PowerShell aracılığıyla Azure Stack Edge GPU cihazınızda VM 'Leri dağıtma
description: Azure PowerShell kullanarak bir Azure Stack Edge GPU cihazında sanal makinelerin (VM 'Ler) nasıl oluşturulacağını ve yönetileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 2c8d9a27725259c34060552494a56f14621f54b4
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147040"
---
# <a name="deploy-vms-on-your-azure-stack-edge-device-via-azure-powershell"></a>Azure PowerShell aracılığıyla Azure Stack Edge cihazınızda VM 'Leri dağıtma

<!--[!INCLUDE [azure-stack-edge-gateway-deploy-vm-overview](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-overview.md)]-->

Bu öğretici, Azure PowerShell kullanarak Azure Stack Edge cihazınızda bir sanal makinenin nasıl oluşturulduğunu ve yönetileceğini açıklar.

## <a name="vm-deployment-workflow"></a>VM dağıtımı iş akışı

Dağıtım iş akışı aşağıdaki diyagramda gösterilmiştir.

![VM dağıtımı iş akışı](media/azure-stack-edge-j-series-deploy-virtual-machine-powershell/vm-workflow_r.svg)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [azure-stack-edge-gateway-deploy-vm-prerequisites](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-prerequisites.md)]



## <a name="query-for-built-in-subscription-on-the-device"></a>Cihazda yerleşik abonelik için sorgu

Azure Resource Manager için yalnızca tek bir kullanıcı tarafından görülebilen sabit abonelik desteklenir. Bu abonelik cihaz başına benzersizdir ve bu abonelik adı veya abonelik KIMLIĞI değiştirilemez.

Bu abonelik, VM oluşturma için gerekli olan tüm kaynakları içerir. 

> [!IMPORTANT]
> Bu abonelik bağlı değil veya Azure aboneliğiniz ile ilgili değil ve cihazınızda yerel olarak yer alır.

Bu abonelik, VM 'Leri dağıtmak için kullanılacaktır.

1.  Bu aboneliği listelemek için şunu yazın:

    ```powershell
    Get-AzureRmSubscription
    ```
    
    Örnek çıktı aşağıda gösterilmiştir.

    ```powershell
    PS C:\windows\system32> Get-AzureRmSubscription
    
    Name                 Id                 TenantId          State
    ----                 --                --------           -----
    Default Provider Subscription A4257FDE-B946-4E01-ADE7-674760B8D1A3 c0257de7-538f-415c-993a-1b87a031879d Enabled
    
    PS C:\windows\system32>
    ```
        
3.  Cihazda çalışan kayıtlı kaynak sağlayıcılarının listesini alın. Bu liste genellikle Işlem, ağ ve depolamayı içerir.

    ```powershell
    Get-AzureRMResourceProvider
    ```

    > [!NOTE]
    > Kaynak sağlayıcıları önceden kaydedilir ve değiştirilemez veya değiştirilemez.
    
    Örnek bir çıktı aşağıda gösterilmiştir:

    ```powershell
    Get-AzureRmResourceProvider
    ProviderNamespace : Microsoft.Compute
    RegistrationState : Registered
    ResourceTypes     : {virtualMachines, virtualMachines/extensions, locations, operations...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Network
    RegistrationState : Registered
    ResourceTypes     : {operations, locations, locations/operations, locations/usages...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Resources
    RegistrationState : Registered
    ResourceTypes     : {tenants, locations, providers, checkresourcename...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Storage
    RegistrationState : Registered
    ResourceTypes     : {storageaccounts, storageAccounts/blobServices, storageAccounts/tableServices,
                        storageAccounts/queueServices...}
    Locations         : {DBELocal}
    ZoneMappings      :
    ```
    
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, depolama hesabı, disk, yönetilen disk gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

> [!IMPORTANT]
> Tüm kaynaklar cihazla aynı konumda oluşturulur ve konum **Dbelocal**olarak ayarlanır.

```powershell
New-AzureRmResourceGroup -Name <Resource group name> -Location DBELocal
```

Örnek çıktı aşağıda gösterilmiştir.

```powershell
New-AzureRmResourceGroup -Name rg191113014333 -Location DBELocal 
Successfully created Resource Group:rg191113014333
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Önceki adımda oluşturulan kaynak grubunu kullanarak yeni bir depolama hesabı oluşturun. Bu, sanal makıne için sanal disk görüntüsünü karşıya yüklemek üzere kullanılacak **yerel bir depolama hesabıdır** .

```powershell
New-AzureRmStorageAccount -Name <Storage account name> -ResourceGroupName <Resource group name> -Location DBELocal -SkuName Standard_LRS
```

> [!NOTE]
> Yalnızca yerel olarak yedekli depolama (Standard_LRS veya Premium_LRS) gibi yerel depolama hesapları Azure Resource Manager aracılığıyla oluşturulabilir. Katmanlı depolama hesapları oluşturmak için, [Azure Stack kenarlarınızdaki depolama hesaplarına bağlanma, ekleme](azure-stack-edge-j-series-deploy-add-storage-accounts.md)bölümündeki adımlara bakın.

Örnek çıktı aşağıda gösterilmiştir.

```powershell
New-AzureRmStorageAccount -Name sa191113014333  -ResourceGroupName rg191113014333 -SkuName Standard_LRS -Location DBELocal

ResourceGroupName      : rg191113014333
StorageAccountName     : sa191113014333
Id                     : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Microsoft.Storage/storageaccounts/sa191113014333
Location               : DBELocal
Sku                    : Microsoft.Azure.Management.Storage.Models.Sku
Kind                   : Storage
Encryption             : Microsoft.Azure.Management.Storage.Models.Encryption
AccessTier             :
CreationTime           : 11/13/2019 9:43:49 PM
CustomDomain           :
Identity               :
LastGeoFailoverTime    :
PrimaryEndpoints       : Microsoft.Azure.Management.Storage.Models.Endpoints
PrimaryLocation        : DBELocal
ProvisioningState      : Succeeded
SecondaryEndpoints     :
SecondaryLocation      :
StatusOfPrimary        : Available
StatusOfSecondary      :
Tags                   :
EnableHttpsTrafficOnly : False
NetworkRuleSet         :
Context                : Microsoft.WindowsAzure.Commands.Common.Storage.LazyAzureStorageContext
ExtendedProperties     : {}
```

Depolama hesabı anahtarını almak için `Get-AzureRmStorageAccountKey` komutunu çalıştırın. Bu komutun örnek bir çıktısı burada gösterilmiştir.

```powershell
PS C:\Users\Administrator> Get-AzureRmStorageAccountKey

cmdlet Get-AzureRmStorageAccountKey at command pipeline position 1
Supply values for the following parameters:
(Type !? for Help.)
ResourceGroupName: my-resource-ase
Name:myasestoracct

KeyName Value
------- -----
key1 /IjVJN+sSf7FMKiiPLlDm8mc9P4wtcmhhbnCa7...
key2 gd34TcaDzDgsY9JtDNMUgLDOItUU0Qur3CBo6Q...
```

## <a name="add-blob-uri-to-hosts-file"></a>Ana bilgisayar dosyasına blob URI Ekle

Blob URI 'sini, [uç nokta adı çözümlemesi için konak dosyası değiştirme](azure-stack-edge-j-series-connect-resource-manager.md#step-5-modify-host-file-for-endpoint-name-resolution)bölümünde bulunan BLOB depolamaya bağlanmak için kullandığınız istemcinin Hosts dosyasına eklemiş olursunuz. Bu, blob URI 'SI için girdidir:

\<Azure consistent network services VIP \>\<storage name\>. blob. \<appliance name\>\<dnsdomain\>


## <a name="install-certificates"></a>Sertifikaları yükler

*Https*kullanıyorsanız, cihazınıza uygun sertifikaları yüklemeniz gerekir. Bu durumda, blob uç noktası sertifikasını yükler. Daha fazla bilgi için bkz. [sertifikaları yönetme](azure-stack-edge-j-series-manage-certificates.md)bölümünde sertifika oluşturma ve karşıya yükleme.

## <a name="upload-a-vhd"></a>VHD’yi karşıya yükleme

Önceki adımlarda oluşturduğunuz yerel depolama hesabındaki sayfa Bloblarında kullanılacak tüm disk görüntülerini kopyalayın. Daha önceki adımlarda oluşturduğunuz depolama hesabına VHD 'yi yüklemek için [AzCopy](../storage/common/storage-use-azcopy-v10.md) gibi bir araç kullanabilirsiniz. 

AzCopy kullanmadan önce, AzCopy 'ın Azure Stack Edge cihazlarınız ile kullandığınız BLOB depolama REST API sürümü ile kullanılmak üzere [doğru yapılandırıldığından](#configure-azcopy) emin olun.

```powershell
AzCopy /Source:<sourceDirectoryForVHD> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Y /S /V /NC:32  /BlobType:page /destType:blob 
```

> [!NOTE]
> `BlobType`VHD dışında yönetilen bir disk oluşturmak için sayfa olarak ayarlayın. `BlobType`AzCopy kullanılarak katmanlı depolama hesaplarına yazarken blok olarak ayarlayın.

Market 'ten disk görüntülerini indirebilirsiniz. Ayrıntılı adımlar için [Azure Marketi 'nden sanal disk görüntüsünü alın](azure-stack-edge-j-series-create-virtual-machine-image.md)bölümüne gidin.

AzCopy 7,3 kullanan bir örnek çıktı aşağıda gösterilmiştir. Bu komut hakkında daha fazla bilgi için [AzCopy kullanarak VHD dosyasını depolama hesabına yükleme](../devtest-labs/devtest-lab-upload-vhd-using-azcopy.md)bölümüne gidin.


```powershell
AzCopy /Source:\\hcsfs\scratch\vm_vhds\linux\ /Dest:http://sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages /DestKey:gJKoyX2Amg0Zytd1ogA1kQ2xqudMHn7ljcDtkJRHwMZbMK== /Y /S /V /NC:32 /BlobType:page /destType:blob /z:2e7d7d27-c983-410c-b4aa-b0aa668af0c6
```


## <a name="create-managed-disks-from-the-vhd"></a>VHD 'den yönetilen diskler oluşturma

Karşıya yüklenen VHD 'den yönetilen disk oluşturun.

```powershell
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import -SourceUri "Source URL for your VHD"
```
Örnek bir çıktı aşağıda gösterilmiştir: 

$DiskConfig = New-AzureRmDiskConfig-location DBELocal-CreateOption Import – SourceUri http://sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages/ubuntu13.vhd 

```powershell
New-AzureRMDisk -ResourceGroupName <Resource group name> -DiskName <Disk name> -Disk $DiskConfig
```

Örnek çıktı aşağıda gösterilmiştir. Bu cmdlet hakkında daha fazla bilgi için [New-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk?view=azurermps-6.13.0)sayfasına gidin.

```powershell
Tags               :
New-AzureRmDisk -ResourceGroupName rg191113014333 -DiskName ld191113014333 -Disk $DiskConfig

ResourceGroupName  : rg191113014333
ManagedBy          :
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              :
TimeCreated        : 11/13/2019 1:49:07 PM
OsType             :
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 30
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micros
                     oft.Compute/disks/ld191113014333
Name               : ld191113014333
Type               : Microsoft.Compute/disks
Location           : DBELocal
Tags               : {}
```

## <a name="create-a-vm-image-from-the-image-managed-disk"></a>Görüntü yönetilen diskinden bir VM görüntüsü oluşturma

Yönetilen diskten bir VM görüntüsü oluşturmak için aşağıdaki komutu kullanın. İçindeki değerleri, \< \> seçtiğiniz adlarla değiştirin.

```powershell
$imageConfig = New-AzureRmImageConfig -Location DBELocal
$ManagedDiskId = (Get-AzureRmDisk -Name <Disk name> -ResourceGroupName <Resource group name>).Id
Set-AzureRmImageOsDisk -Image $imageConfig -OsType '<OS type>' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId 

The supported OS types are Linux and Windows.

For OS Type=Linux, for example:
Set-AzureRmImageOsDisk -Image $imageConfig -OsType 'Linux' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId
New-AzureRmImage -Image $imageConfig -ImageName <Image name>  -ResourceGroupName <Resource group name>
```

Örnek çıktı aşağıda gösterilmiştir. Bu cmdlet hakkında daha fazla bilgi için [New-Azurermımage](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage?view=azurermps-6.13.0)sayfasına gidin.

```powershell
New-AzureRmImage -Image Microsoft.Azure.Commands.Compute.Automation.Models.PSImage -ImageName ig191113014333  -ResourceGroupName rg191113014333
ResourceGroupName    : rg191113014333
SourceVirtualMachine :
StorageProfile       : Microsoft.Azure.Management.Compute.Models.ImageStorageProfile
ProvisioningState    : Succeeded
Id                   : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micr
                       osoft.Compute/images/ig191113014333
Name                 : ig191113014333
Type                 : Microsoft.Compute/images
Location             : dbelocal
Tags                 : {}
```

## <a name="create-vm-with-previously-created-resources"></a>Önceden oluşturulmuş kaynaklarla VM oluşturma

VM 'yi oluşturup dağıtmadan önce bir sanal ağ oluşturmanız ve bir sanal ağ arabirimini ilişkilendirmeniz gerekir.

> [!IMPORTANT]
> Sanal ağ ve sanal ağ arabirimi oluştururken aşağıdaki kurallar geçerlidir:
> - Yalnızca bir sanal ağ oluşturulabilir (kaynak grupları arasında bile) ve adres alanı açısından tam olarak mantıksal ağla eşleşmesi gerekir.
> -   VNET 'te yalnızca bir alt ağa izin verilir. Alt ağ, VNET ile tam olarak aynı adres alanı olmalıdır.
> -   VNIC oluşturma sırasında yalnızca statik ayırma yöntemine izin verilir ve kullanıcının özel bir IP adresi sağlaması gerekir.

 
**VNET oluşturma**

```powershell
$subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name <Subnet name> -AddressPrefix <Address Prefix>
$aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName <Resource group name> -Name <Vnet name> -Location DBELocal -AddressPrefix <Address prefix> -Subnet $subNetId
```

**VNET alt ağ KIMLIĞINI kullanarak bir vNIC oluşturma**

```powershell
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <IP config Name> -SubnetId $aRmVN.Subnets[0].Id -PrivateIpAddress <Private IP>
$Nic = New-AzureRmNetworkInterface -Name <Nic name> -ResourceGroupName <Resource group name> -Location DBELocal -IpConfiguration $ipConfig
```

Bu komutların örnek çıktısı aşağıda gösterilmiştir:

```powershell
PS C:\Users\Administrator> $subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name my-ase-subnet -AddressPrefix "5.5.0.0/16"

PS C:\Users\Administrator> $aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName Resource-my-ase -Name my-ase-virtualnetwork -Location DBELocal -AddressPrefix "5.5.0.0/16" -Subnet $subNetId
WARNING: The output object type of this cmdlet will be modified in a future release.
PS C:\Users\Administrator> $ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name my-ase-ip -SubnetId $aRmVN.Subnets[0].Id
PS C:\Users\Administrator> $Nic = New-AzureRmNetworkInterface -Name my-ase-nic -ResourceGroupName Resource-my-ase -Location DBELocal -IpConfiguration $ipConfig
WARNING: The output object type of this cmdlet will be modified in a future release.

PS C:\Users\Administrator> $Nic

PS C:\Users\Administrator> (Get-AzureRmNetworkInterface)[0]

Name                        : nic200108020444
ResourceGroupName           : rg200108020444
Location                    : dbelocal
Id                          : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Network/networ
                              kInterfaces/nic200108020444
Etag                        : W/"f9d1759d-4d49-42fa-8826-e218e0b1d355"
ResourceGuid                : 3851ae62-c13e-4416-9386-e21d9a2fef0f
ProvisioningState           : Succeeded
Tags                        :
VirtualMachine              : {
                                "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Compu
                              te/virtualMachines/VM200108020444"
                              }
IpConfigurations            : [
                                {
                                  "Name": "ip200108020444",
                                  "Etag": "W/\"f9d1759d-4d49-42fa-8826-e218e0b1d355\"",
                                  "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Net
                              work/networkInterfaces/nic200108020444/ipConfigurations/ip200108020444",
                                  "PrivateIpAddress": "5.5.166.65",
                                  "PrivateIpAllocationMethod": "Static",
                                  "Subnet": {
                                    "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/DbeSystemRG/providers/Microsoft.Netw
                              ork/virtualNetworks/vSwitch1/subnets/subnet123",
                                    "ResourceNavigationLinks": [],
                                    "ServiceEndpoints": []
                                  },
                                  "ProvisioningState": "Succeeded",
                                  "PrivateIpAddressVersion": "IPv4",
                                  "LoadBalancerBackendAddressPools": [],
                                  "LoadBalancerInboundNatRules": [],
                                  "Primary": true,
                                  "ApplicationGatewayBackendAddressPools": [],
                                  "ApplicationSecurityGroups": []
                                }
                              ]
DnsSettings                 : {
                                "DnsServers": [],
                                "AppliedDnsServers": []
                              }
EnableIPForwarding          : False
EnableAcceleratedNetworking : False
NetworkSecurityGroup        : null
Primary                     : True
MacAddress                  : 00155D18E432                :
```

İsteğe bağlı olarak, bir VM için vNIC oluştururken genel IP 'yi geçirebilirsiniz. Bu örnekte, genel IP özel IP 'yi döndürür. 

```powershell
New-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <ResourceGroupName> -AllocationMethod Static -Location DBELocal
$publicIP = (Get-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <Resource group name>).Id
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <ConfigName> -PublicIpAddressId $publicIP -SubnetId $subNetId
```


**VM oluşturma**

Artık VM görüntüsünü kullanarak bir VM oluşturabilir ve daha önce oluşturduğunuz sanal ağa iliştirebilirsiniz.

```powershell
$pass = ConvertTo-SecureString "<Password>" -AsPlainText -Force;
$cred = New-Object System.Management.Automation.PSCredential("<Enter username>", $pass)

You will use this username, password to login to the VM, once it is created and powered up.

$VirtualMachine = New-AzureRmVMConfig -VMName <VM name> -VMSize "Standard_D1_v2"

$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -<OS type> -ComputerName <Your computer Name> -Credential $cred

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name <OS Disk Name> -Caching "ReadWrite" -CreateOption "FromImage" -Linux -StorageAccountType StandardLRS

$nicID = (Get-AzureRmNetworkInterface -Name <nic name> -ResourceGroupName <Resource Group Name>).Id

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nicID

$image = (Get-AzureRmImage -ResourceGroupName <Resource Group Name> -ImageName $ImageName).Id

$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -Id $image

New-AzureRmVM -ResourceGroupName <Resource Group Name> -Location DBELocal -VM $VirtualMachine -Verbose
```

## <a name="connect-to-a-vm"></a>Bir VM’ye bağlanma

VM oluşturma sırasında geçirilen özel IP 'yi kullanarak VM 'ye bağlanın.

IP adresiyle bağlantı kurmak için bir SSH oturumu açın.

`ssh -l <username> <ip address>`

İstendiğinde, VM oluştururken kullandığınız parolayı belirtin.

SSH anahtarını sağlamanız gerekiyorsa, bu komutu kullanın.

SSH-ı c:/kullanıcılar/yönetici/. ssh/id_rsa Administrator@5.5.41.236

VM oluşturma sırasında genel bir IP adresi kullandıysanız, bu IP 'yi sanal makineye bağlanmak için kullanabilirsiniz. Genel IP 'yi almak için: 

```powershell
$publicIp = Get-AzureRmPublicIpAddress -Name <Public IP> -ResourceGroupName <Resource group name>
```
Bu durumda genel IP, sanal ağ arabirimi oluşturma sırasında geçirilen özel IP ile aynı olacaktır.


## <a name="manage-vm"></a>VM 'yi yönetme

Aşağıdaki bölümde, Azure Stack Edge cihazınızda oluşturacağınız sanal makine çevresindeki bazı yaygın işlemler açıklanmaktadır.

### <a name="list-vms-running-on-the-device"></a>Cihazda çalışan VM 'Leri listeleyin

Azure Stack Edge cihazınızda çalışan tüm VM 'lerin bir listesini döndürmek için aşağıdaki komutu çalıştırın.


`Get-AzureRmVM -ResourceGroupName <String> -Name <String>`
 

### <a name="turn-on-the-vm"></a>VM 'yi aç

Cihazınızda çalışan bir sanal makineyi açmak için aşağıdaki cmdlet 'i çalıştırın:


`Start-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>`


Bu cmdlet hakkında daha fazla bilgi için [Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm?view=azurermps-6.13.0)' ye gidin.

### <a name="suspend-or-shut-down-the-vm"></a>VM 'yi askıya alma veya kapatma

Cihazınızda çalışan bir sanal makineyi durdurmak veya kapatmak için aşağıdaki cmdlet 'i çalıştırın:


```powershell
Stop-AzureRmVM [-Name] <String> [-StayProvisioned] [-ResourceGroupName] <String>
```


Bu cmdlet hakkında daha fazla bilgi için [stop-AzureRmVM cmdlet 'ine](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm?view=azurermps-6.13.0)gidin.

### <a name="add-a-data-disk"></a>Veri diski ekleme

VM 'nizin iş yükü gereksinimleri artdıkça, bir veri diski eklemeniz gerekebilir.

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" -VhdUri "https://contoso.blob.core.windows.net/vhds/diskstandard03.vhd" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty 
 
Update-AzureRmVM -ResourceGroupName "<Resource Group Name string>" -VM $VirtualMachine
```

### <a name="delete-the-vm"></a>VM’yi silin

Bir sanal makineyi cihazınızdan kaldırmak için aşağıdaki cmdlet 'i çalıştırın:

```powershell
Remove-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>
```

Bu cmdlet hakkında daha fazla bilgi için [Remove-AzureRmVm cmdlet 'ine](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm?view=azurermps-6.13.0)gidin.


## <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

VM boyutu, sanal makine için kullanılabilir hale getirilen CPU, GPU ve bellek gibi işlem kaynaklarının miktarını belirler. Sanal makinelerin iş yükü için uygun bir VM boyutu kullanılarak oluşturulması gerekir. Tüm makineler aynı donanımda çalışmaya rağmen, makine boyutları disk erişimi için farklı sınırlara sahiptir ve bu, VM 'leriniz genelinde genel disk erişimini yönetmenize yardımcı olabilir. İş yükü artarsa, mevcut sanal makine de yeniden boyutlandırılabilir.

Azure Stack Edge cihazında oluşturma için aşağıdaki standart Dv2 serisi VM 'Ler desteklenir.

### <a name="dv2-series"></a>Dv2 Serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3,5 |50   |500 |3000  |4/4x500   |2 |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8/8x500   |2 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16/16x500 |4 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32/32x500 |8 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64/64x500 |8 |

### <a name="dsv2-series"></a>DSv2 serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3,5 |7   |1000 |4000  |4/4x2300   |2 |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8/8x2300   |2 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16/16x2300 |4 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32/32x2300 |8 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64/64x2300 |8 |

### <a name="dv2-series"></a>Dv2 Serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8/8x500    |2 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16/16x500  |4 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32/32x500  |8 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64/64x500  |8 |


### <a name="dsv2-series"></a>DSv2 serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |4/4x2300    |2 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |8/8x2300    |4 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |16/16x2300  |8 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |32/32x2300  |8 |

Daha fazla bilgi için [genel AMAÇLı VM boyutlarında Dv2 serisi](../virtual-machines/dv2-dsv2-series.md#dv2-series)' ne gidin.

## <a name="unsupported-vm-operations-and-cmdlets"></a>Desteklenmeyen VM işlemleri ve cmdlet 'leri

Uzantı, ölçek kümeleri, kullanılabilirlik kümeleri, anlık görüntüler desteklenmez.

## <a name="configure-azcopy"></a>AzCopy yapılandırma

AzCopy 'in en son sürümünü yüklediğinizde, AzCopy ' ı, Azure Stack Edge cihazınızın BLOB depolama REST API sürümü ile eşleştiğinden emin olmak için yapılandırmanız gerekir.

Azure Stack Edge cihazınıza erişmek için kullanılan istemcide, BLOB depolama REST API sürümüyle eşleşecek şekilde bir genel değişken ayarlayın.

### <a name="on-windows-client"></a>Windows istemcisinde 

`$Env:AZCOPY_DEFAULT_SERVICE_API_VERSION = "2017-11-09"`

### <a name="on-linux-client"></a>Linux istemcisinde

`export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09`

AzCopy ortam değişkeninin doğru şekilde ayarlandığından emin olmak için aşağıdaki adımları uygulayın:

1. "AzCopy env" komutunu çalıştırın
2. `AZCOPY_DEFAULT_SERVICE_API_VERSION`Parametre bul. Bu, önceki adımlarda ayarladığınız değere sahip olmalıdır.


## <a name="next-steps"></a>Sonraki adımlar

[Azure Resource Manager cmdlet 'leri](https://docs.microsoft.com/powershell/module/azurerm.resources/?view=azurermps-6.13.0)
