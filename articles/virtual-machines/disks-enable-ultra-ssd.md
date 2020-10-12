---
title: VM 'Ler için Ultra diskler-Azure yönetilen diskler
description: Azure VM 'Leri için Ultra diskler hakkında bilgi edinin
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 09/28/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: e57317dce64b58e5c92684152d840955a30df660
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91441197"
---
# <a name="using-azure-ultra-disks"></a>Azure Ultra disklerini kullanma

Bu makalede, Ultra diskler hakkında kavramsal bilgi için, [Azure 'da kullanılabilen disk türlerini](disks-types.md#ultra-disk)gösteren bir ultra diskin nasıl dağıtılacağı ve kullanılacağı açıklanmaktadır.

Azure Ultra diskler, Azure IaaS sanal makineleri (VM 'Ler) için yüksek aktarım hızı, yüksek ıOPS ve tutarlı düşük gecikme süreli disk depolama alanı sunar. Bu yeni teklif, var olan diskler tekliflerimiz ile aynı Kullanılabilirlik düzeylerinde satır performansının üst kısmında yer sağlar. Ultra disklerin büyük bir avantajı, sanal makinelerinizi yeniden başlatmanıza gerek kalmadan SSD 'nin performansını ve iş yüklerinizde dinamik olarak değiştirme yeteneğidir. Ultra diskler SAP HANA, en üst katman veritabanları ve ağır işlem içeren iş yükleri gibi yoğun veri kullanan iş yüklerine uygundur.

## <a name="ga-scope-and-limitations"></a>GA kapsamı ve sınırlamaları

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](../../includes/managed-disks-ultra-disks-GA-scope-and-limitations.md)]

## <a name="determine-vm-size-and-region-availability"></a>VM boyutunu ve bölge kullanılabilirliğini belirleme

### <a name="vms-using-availability-zones"></a>Kullanılabilirlik alanlarını kullanan VM 'Ler

Ultra disklerden yararlanmak için, hangi kullanılabilirlik bölgesinin bulunduğunu belirlemeniz gerekir. Her bölge, Ultra disklerle her VM boyutunu desteklememektedir. Bölgeniz, bölgeniz ve VM boyutlarınızın Ultra diskleri desteklediğini öğrenmek için aşağıdaki komutlardan birini çalıştırın, önce **bölge**, **VMSize**ve **abonelik** değerlerini değiştirdiğinizden emin olun:

#### <a name="cli"></a>CLI

```azurecli
subscription="<yourSubID>"
# example value is southeastasia
region="<yourLocation>"
# example value is Standard_E64s_v3
vmSize="<yourVMSize>"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].locationInfo[0].zoneDetails[0].Name" --subscription $subscription
```

#### <a name="powershell"></a>PowerShell

```powershell
$region = "southeastasia"
$vmSize = "Standard_E64s_v3"
$sku = (Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) -and $_.LocationInfo[0].ZoneDetails.Count -gt 0})
if($sku){$sku[0].LocationInfo[0].ZoneDetails} Else {Write-host "$vmSize is not supported with Ultra Disk in $region region"}
```

Yanıt aşağıdaki biçimde olacaktır; burada X, seçtiğiniz bölgede dağıtım için kullanılacak bölgedir. X 1, 2 veya 3 olabilir.

Bölge değerini koru, kullanılabilirlik **bölgenizi** temsil eder ve bir ultra disk dağıtmak için bu alana ihtiyacınız olur.

|ResourceType  |Ad  |Konum  |Bölgeler  |Kısıtlama  |Özellik  |Değer  |
|---------|---------|---------|---------|---------|---------|---------|
|disklerinden     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Komuttan yanıt yoksa, seçilen VM boyutu seçili bölgedeki Ultra disklerle desteklenmez.

Hangi bölgeyi dağıtacağınızı bildiğinize göre, bu makaledeki dağıtım adımlarını izleyerek bir ultra disk eklenmiş bir VM dağıtın veya var olan bir VM 'ye bir ultra disk takın.

### <a name="vms-with-no-redundancy-options"></a>Artıklık seçeneği olmayan VM 'Ler

Seçim bölgelerinde dağıtılan Ultra disklerin şimdilik hiçbir artıklık seçeneği olmadan dağıtılması gerekir. Ancak, Ultra diskleri destekleyen her disk boyutu bu bölgede olabilir. Hangi disk boyutlarının Ultra diskleri desteklediğini anlamak için aşağıdaki kod parçacıklarında birini kullanabilirsiniz. `vmSize`Önce ve değerlerini değiştirdiğinizden emin olun `subscription` :

```azurecli
subscription="<yourSubID>"
region="westus"
# example value is Standard_E64s_v3
vmSize="<yourVMSize>"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].capabilities" --subscription $subscription
```

```azurepowershell
$region = "westus"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) })[0].Capabilities
```

Yanıt aşağıdaki biçime benzer olacaktır, `UltraSSDAvailable   True` VM boyutunun bu bölgedeki Ultra diskleri destekleyip desteklemediğini gösterir.

```
Name                                         Value
----                                         -----
MaxResourceVolumeMB                          884736
OSVhdSizeMB                                  1047552
vCPUs                                        64
HyperVGenerations                            V1,V2
MemoryGB                                     432
MaxDataDiskCount                             32
LowPriorityCapable                           True
PremiumIO                                    True
VMDeploymentTypes                            IaaS
vCPUsAvailable                               64
ACUs                                         160
vCPUsPerCore                                 2
CombinedTempDiskAndCachedIOPS                128000
CombinedTempDiskAndCachedReadBytesPerSecond  1073741824
CombinedTempDiskAndCachedWriteBytesPerSecond 1073741824
CachedDiskBytes                              1717986918400
UncachedDiskIOPS                             80000
UncachedDiskBytesPerSecond                   1258291200
EphemeralOSDiskSupported                     True
AcceleratedNetworkingEnabled                 True
RdmaEnabled                                  False
MaxNetworkInterfaces                         8
UltraSSDAvailable                            True
```

## <a name="deploy-an-ultra-disk-using-azure-resource-manager"></a>Azure Resource Manager kullanarak bir ultra disk dağıtma

İlk olarak, dağıtılacak VM boyutunu saptayın. Desteklenen VM boyutlarının listesi için bkz. [ga kapsam ve sınırlamalar](#ga-scope-and-limitations) bölümü.

Birden çok Ultra disk içeren bir VM oluşturmak isterseniz, örneğe [birden çok Ultra disk içeren BIR VM oluşturma](https://aka.ms/ultradiskArmTemplate)konusuna bakın.

Kendi şablonunuzu kullanmayı düşünüyorsanız, ve için **Apiversion** ' `Microsoft.Compute/virtualMachines` `Microsoft.Compute/Disks` ın `2018-06-01` (veya üzeri) olarak ayarlandığından emin olun.

Disk SKU 'sunu **UltraSSD_LRS**olarak ayarlayın, ardından bir ultra disk oluşturmak için disk KAPASITESINI, IOPS 'yi, kullanılabilirlik alanını ve aktarım hızını MB cinsinden ayarlayın.

VM sağlandıktan sonra veri disklerini bölümleyebilir ve biçimlendirebilir ve iş yükleriniz için yapılandırabilirsiniz.


## <a name="deploy-an-ultra-disk"></a>Bir ultra disk dağıtma

# <a name="portal"></a>[Portal](#tab/azure-portal)

Bu bölümde, bir veri diski olarak bir ultra disk ile donatılmış bir sanal makinenin dağıtımı ele alınmaktadır. Bir sanal makineyi dağıtma konusunda bilgi sahibi olduğunuz varsayılmaktadır. [hızlı başlangıç: Azure Portal bir Windows sanal makinesi oluşturun](./windows/quick-create-portal.md).

- [Azure Portal](https://portal.azure.com/) oturum açın ve sanal makıne (VM) dağıtma ' ya gidin.
- [Desteklenen BIR VM boyutu ve bölgesi](#ga-scope-and-limitations)seçtiğinizden emin olun.
- Kullanılabilirlik **seçenekleri**' nde **kullanılabilirlik alanı** ' nı seçin.
- Kalan girdileri tercih ettiğiniz seçimlere göre girin.
- **Diskler**'i seçin.

![VM oluşturma akışının temel dikey penceresinin ekran görüntüsü.](media/virtual-machines-disks-getting-started-ultra-ssd/create-ultra-disk-enabled-vm.png)

- Diskler dikey penceresinde, **Ultra disk uyumluluğunu etkinleştirmek**için **Evet** ' i seçin.
- Şimdi bir ultra disk eklemek için **Oluştur ve yeni bir disk Ekle** öğesini seçin.

![VM oluşturma akışı, disk dikey penceresi, Ultra etkin ve oluşturma ve iliştirme yeni bir disk ekran görüntüsü vurgulanmıştır.](media/virtual-machines-disks-getting-started-ultra-ssd/enable-and-attach-ultra-disk.png)

- **Yeni disk oluştur** dikey penceresinde, bir ad girin ve **boyutu Değiştir**' i seçin.

    :::image type="content" source="media/virtual-machines-disks-getting-started-ultra-ssd/ultra-disk-create-new-disk-flow.png" alt-text="Yeni disk oluştur dikey penceresinin ekran görüntüsü, değişiklik boyutu vurgulandı.":::


- **Depolama türünü** **Ultra disk**olarak değiştirin.
- **Özel disk boyutu (GiB)**, **disk IOPS**ve **disk aktarım hızı** değerlerini tercih ettiğiniz olanlarla değiştirin.
- Her iki dikey penceresinde **Tamam ' ı** seçin.

    :::image type="content" source="media/virtual-machines-disks-getting-started-ultra-ssd/ultra-disk-select-new-disk.png" alt-text="Yeni disk oluştur dikey penceresinin ekran görüntüsü, değişiklik boyutu vurgulandı.":::

- VM dağıtımıyla devam edin, diğer VM 'leri dağıttığınız ile aynı olacaktır.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, dağıtılacak VM boyutunu saptayın. Desteklenen VM boyutlarının listesi için [ga kapsam ve sınırlamalar](#ga-scope-and-limitations) bölümüne bakın.

Bir ultra disk eklemek için, Ultra diskler kullanabilen bir sanal makine oluşturmanız gerekir.

**$VMName**, **$RgName**, **$diskname**, **$Location**, **$Password**, **$User** değişkenlerini kendi değerlerinizle değiştirin veya ayarlayın. [Bu makalenin başlangıcından](#determine-vm-size-and-region-availability)aldığınız kullanılabilirlik bölgenizin değerine **$Zone** ayarlayın. Ardından, bir ultra etkin VM oluşturmak için aşağıdaki CLı komutunu çalıştırın:

```azurecli-interactive
az disk create --subscription $subscription -n $diskname -g $rgname --size-gb 1024 --location $location --sku UltraSSD_LRS --disk-iops-read-write 8192 --disk-mbps-read-write 400
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --size Standard_D4s_v3 --location $location --attach-data-disks $diskname
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak, dağıtılacak VM boyutunu saptayın. Desteklenen VM boyutlarının listesi için [ga kapsam ve sınırlamalar](#ga-scope-and-limitations) bölümüne bakın.

Ultra diskler kullanmak için, Ultra diskler kullanabilen bir sanal makine oluşturmanız gerekir. **$Resourcegroup** ve **$vmName** değişkenlerini kendi değerlerinizle değiştirin veya ayarlayın. [Bu makalenin başlangıcından](#determine-vm-size-and-region-availability)aldığınız kullanılabilirlik bölgenizin değerine **$Zone** ayarlayın. Ardından, bir ultra etkin VM oluşturmak için aşağıdaki [New-AzVm](/powershell/module/az.compute/new-azvm) komutunu çalıştırın:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="create-and-attach-the-disk"></a>Diski oluşturma ve iliştirme

VM 'niz dağıtıldıktan sonra, buna bir ultra disk oluşturup ekleyebilirsiniz, aşağıdaki betiği kullanabilirsiniz:

```powershell
# Set parameters and select subscription
$subscription = "<yourSubscriptionID>"
$resourceGroup = "<yourResourceGroup>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$lun = 1
Connect-AzAccount -SubscriptionId $subscription

# Create the disk
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName $diskName `
-Disk $diskconfig;

# add disk to VM
$vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
$disk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $diskName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun $lun
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

---
## <a name="attach-an-ultra-disk"></a>Bir ultra disk iliştirme

# <a name="portal"></a>[Portal](#tab/azure-portal)

Alternatif olarak, mevcut sanal makinenizin Ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesi varsa, yeni bir VM oluşturmak zorunda kalmadan Ultra disklerin kullanımını sağlayabilirsiniz. Mevcut sanal makinenizde Ultra diskler ' i etkinleştirip bunları veri diskleri olarak ekleme. Ultra disk uyumluluğunu etkinleştirmek için VM 'yi durdurmanız gerekir. VM 'yi durdurduktan sonra uyumluluğu etkinleştirebilir ve ardından VM 'yi yeniden başlatabilirsiniz. Uyumluluk etkinleştirildikten sonra, bir ultra disk iliştirebilirsiniz:

- Sanal makinenize gidin ve bunu durdurun, serbest gelmesini bekleyin.
- VM 'niz serbest bırakıldıktan sonra **diskler**' i seçin.
- **Düzenle**’yi seçin.

![Mevcut bir VM diski dikey penceresinin ekran görüntüsü, düzenleme vurgulanır.](media/virtual-machines-disks-getting-started-ultra-ssd/options-selector-ultra-disks.png)

- **Ultra disk uyumluluğunu etkinleştirmek**için **Evet** ' i seçin.

![Ultra disk uyumluluğunu etkinleştirme ekran görüntüsü.](media/virtual-machines-disks-getting-started-ultra-ssd/ultra-options-yes-enable.png)

- **Kaydet**’i seçin.
- **Veri diski Ekle** ' yi seçin ve ardından **ad** açılan menüsünde **disk oluştur**' u seçin.

![Yeni bir disk ekleyen disk dikey penceresinin ekran görüntüsü.](media/virtual-machines-disks-getting-started-ultra-ssd/create-and-attach-new-ultra-disk.png)

- Yeni diskiniz için bir ad girin ve **boyutu Değiştir**' i seçin.
- **Hesap türünü** **Ultra disk**olarak değiştirin.
- **Özel disk boyutu (GiB)**, **disk IOPS**ve **disk aktarım hızı** değerlerini tercih ettiğiniz olanlarla değiştirin.

    :::image type="content" source="media/virtual-machines-disks-getting-started-ultra-ssd/ultra-disk-select-new-disk.png" alt-text="Yeni disk oluştur dikey penceresinin ekran görüntüsü, değişiklik boyutu vurgulandı.":::

- **Tamam** ' ı ve ardından **Oluştur**' u seçin.
- Diskinizin dikey penceresine geri dönmeden sonra **Kaydet**' i seçin.
- Sanal makineyi yeniden başlatın.

![VM 'nizin diskler dikey penceresinin ekran görüntüsü.](media/virtual-machines-disks-getting-started-ultra-ssd/saving-and-attaching-new-ultra-disk.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Alternatif olarak, mevcut sanal makinenizin Ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesi varsa, yeni bir VM oluşturmak zorunda kalmadan Ultra disklerin kullanımını sağlayabilirsiniz.

### <a name="enable-ultra-disk-compatibility-on-an-existing-vm---cli"></a>Mevcut VM 'de Ultra disk uyumluluğunu etkinleştirme-CLı

VM 'niz, [ga kapsamında ve kısıtlamalarında](#ga-scope-and-limitations) özetlenen gereksinimleri karşılıyorsa ve [hesabınız için uygun bölgeindeyse](#determine-vm-size-and-region-availability), VM 'niz üzerinde Ultra disk uyumluluğunu etkinleştirebilirsiniz.

Ultra disk uyumluluğunu etkinleştirmek için VM 'yi durdurmanız gerekir. VM 'yi durdurduktan sonra uyumluluğu etkinleştirebilir ve ardından VM 'yi yeniden başlatabilirsiniz. Uyumluluk etkinleştirildikten sonra, bir ultra disk iliştirebilirsiniz:

```azurecli
az vm deallocate -n $vmName -g $rgName
az vm update -n $vmName -g $rgName --ultra-ssd-enabled true
az vm start -n $vmName -g $rgName
```

### <a name="create-an-ultra-disk---cli"></a>Ultra disk oluşturma-CLı

Artık Ultra diskler iliştirebilen bir VM 'ye sahip olduğunuza göre, buna bir ultra disk oluşturup ekleyebilirsiniz.

```azurecli-interactive
location="eastus2"
subscription="xxx"
rgname="ultraRG"
diskname="ssd1"
vmname="ultravm1"
zone=123

#create an ultra disk
az disk create `
--subscription $subscription `
-n $diskname `
-g $rgname `
--size-gb 4 `
--location $location `
--zone $zone `
--sku UltraSSD_LRS `
--disk-iops-read-write 1000 `
--disk-mbps-read-write 50
```

### <a name="attach-the-disk---cli"></a>Diski iliştirme-CLı

```azurecli
rgName="<yourResourceGroupName>"
vmName="<yourVMName>"
diskName="<yourDiskName>"
subscriptionId="<yourSubscriptionID>"

az vm disk attach -g $rgName --vm-name $vmName --disk $diskName --subscription $subscriptionId
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Alternatif olarak, mevcut sanal makinenizin Ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesi varsa, yeni bir VM oluşturmak zorunda kalmadan Ultra disklerin kullanımını sağlayabilirsiniz.

### <a name="enable-ultra-disk-compatibility-on-an-existing-vm---powershell"></a>Var olan bir VM 'de Ultra disk uyumluluğunu etkinleştirme-PowerShell

VM 'niz, [ga kapsamında ve kısıtlamalarında](#ga-scope-and-limitations) özetlenen gereksinimleri karşılıyorsa ve [hesabınız için uygun bölgeindeyse](#determine-vm-size-and-region-availability), VM 'niz üzerinde Ultra disk uyumluluğunu etkinleştirebilirsiniz.

Ultra disk uyumluluğunu etkinleştirmek için VM 'yi durdurmanız gerekir. VM 'yi durdurduktan sonra uyumluluğu etkinleştirebilir ve ardından VM 'yi yeniden başlatabilirsiniz. Uyumluluk etkinleştirildikten sonra, bir ultra disk iliştirebilirsiniz:

```azurepowershell
#Stop the VM
Stop-AzVM -Name $vmName -ResourceGroupName $rgName
#Enable ultra disk compatibility
$vm1 = Get-AzVM -name $vmName -ResourceGroupName $rgName
Update-AzVM -ResourceGroupName $rgName -VM $vm1 -UltraSSDEnabled $True
#Start the VM
Start-AzVM -Name $vmName -ResourceGroupName $rgName
```

### <a name="create-and-attach-an-ultra-disk---powershell"></a>Bir ultra disk oluşturma ve iliştirme-PowerShell

Artık Ultra diskler kullanabilen bir VM 'ye sahip olduğunuza göre, buna bir ultra disk oluşturup ekleyebilirsiniz:

```powershell
# Set parameters and select subscription
$subscription = "<yourSubscriptionID>"
$resourceGroup = "<yourResourceGroup>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$lun = 1
Connect-AzAccount -SubscriptionId $subscription

# Create the disk
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName $diskName `
-Disk $diskconfig;

# add disk to VM
$vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
$disk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $diskName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun $lun
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

---
## <a name="adjust-the-performance-of-an-ultra-disk"></a>Bir ultra diskin performansını ayarlama

# <a name="portal"></a>[Portal](#tab/azure-portal)

Ultra diskler, performansını ayarlamanıza olanak tanıyan benzersiz bir özellik sunar. Bu ayarlamaları, disklerin kendileri üzerinde Azure portal yapabilirsiniz.

- Sanal makinenize gidin ve **diskler**' i seçin.
- Performansını değiştirmek istediğiniz Ultra diski seçin.

![VM 'nizin diskler dikey penceresinin ekran görüntüsü, Ultra disk vurgulanır.](media/virtual-machines-disks-getting-started-ultra-ssd/selecting-ultra-disk-to-modify.png)

- **Yapılandırma** ' yı seçin ve ardından değişikliklerinizi yapın.
- **Kaydet**’i seçin.

![Ultra diskinizdeki yapılandırma dikey penceresinin ekran görüntüsü, disk boyutu, IOPS ve aktarım hızı vurgulanmıştır, kaydetme vurgulanır.](media/virtual-machines-disks-getting-started-ultra-ssd/configuring-ultra-disk-performance-and-size.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Ultra diskler, performansını ayarlamanıza olanak tanıyan benzersiz bir özellik sunar ve aşağıdaki komutta bu özelliğin nasıl kullanılacağı gösterilmektedir:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

## <a name="adjust-the-performance-of-an-ultra-disk-using-powershell"></a>PowerShell kullanarak bir ultra diskin performansını ayarlama

Ultra disklerin, performansını ayarlamanıza olanak tanıyan benzersiz bir özelliği vardır ve aşağıdaki komut, diski ayırmak zorunda kalmadan performansı ayarlayan bir örnektir:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```
---

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Kubernetes hizmeti (Önizleme) üzerinde Azure Ultra diskleri kullanın](../aks/use-ultra-disks.md).
- [Günlük diskini bir ultra diske geçirin](../azure-sql/virtual-machines/windows/storage-migrate-to-ultradisk.md).