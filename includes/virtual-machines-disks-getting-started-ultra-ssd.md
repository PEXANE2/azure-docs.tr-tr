---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: dfb094bc9f84e7129a3e1c733a054c5f6cd96372
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008662"
---
Azure ultra diskler, Azure IaaS sanal makineleri (VM) için yüksek iş ortası, yüksek IOPS ve tutarlı düşük gecikmeli disk depolama sunar. Bu yeni teklif, mevcut disk tekliflerimizle aynı kullanılabilirlik düzeylerinde en üst düzeyde hat performansı sağlar. Ultra disklerin en önemli yararlarından biri, VM'lerinizi yeniden başlatmaya gerek kalmadan ssd'nin performansını iş yükleri ile birlikte dinamik olarak değiştirebilmenizdir. Ultra diskler SAP HANA, en üst katman veritabanları ve ağır işlem içeren iş yükleri gibi yoğun veri kullanan iş yüklerine uygundur.

## <a name="ga-scope-and-limitations"></a>GA kapsamı ve sınırlamaları

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]

## <a name="determine-vm-size-and-region-availability"></a>VM boyutunu ve bölge kullanılabilirliğini belirleme

### <a name="vms-using-availability-zones"></a>Kullanılabilirlik bölgelerini kullanan VM'ler

Ultra disklerden yararlanmak için hangi kullanılabilirlik bölgesinde olduğunuzu belirlemeniz gerekir. Her bölge her VM boyutunu ultra disklerle desteklemez. Bölgenizin, bölgenizin ve VM boyutunuzun ultra diskleri destekleyip desteklemediğini belirlemek için, aşağıdaki komutlardan birini çalıştırın, **önce bölge,** **vmSize**ve **abonelik** değerlerini değiştirdiğinizden emin olun:

#### <a name="cli"></a>CLI

```azurecli
$subscription = "<yourSubID>"
# example value is southeastasia
$region = "<yourLocation>"
# example value is Standard_E64s_v3
$vmSize = "<yourVMSize>"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].locationInfo[0].zoneDetails[0].Name" --subscription $subscription
```

#### <a name="powershell"></a>PowerShell

```powershell
$region = "southeastasia"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) -and $_.LocationInfo[0].ZoneDetails.Count -gt 0})[0].LocationInfo[0].ZoneDetails
```

Yanıt, X'in seçtiğiniz bölgede dağıtmak için kullanılacak bölge olduğu aşağıdaki forma benzer olacaktır. X 1, 2 veya 3 olabilir.

**Bölgeler** değerini koruyun, kullanılabilirlik bölgenizi temsil eder ve bir Ultra disk dağıtmak için buna ihtiyacınız olur.

|ResourceType  |Adı  |Konum  |Bölgeler  |Kısıtlama  |Özellik  |Değer  |
|---------|---------|---------|---------|---------|---------|---------|
|Disk     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Komuttan yanıt yoksa, seçili VM boyutu seçili bölgedeki ultra disklerle desteklenmez.

Artık hangi bölgeye dağıtılacağınızdan bildiğinize göre, ultra disk takılı bir VM dağıtmak veya varolan bir VM'ye ultra disk eklemek için bu makaledeki dağıtım adımlarını izleyin.

### <a name="vms-with-no-redundancy-options"></a>Artıklık seçeneği olmayan VM'ler

Batı ABD'de dağıtılan ultra diskler şimdilik herhangi bir artıklık seçeneği olmadan dağıtılmalıdır. Ancak, ultra diskleri destekleyen her disk boyutu bu bölgede olmayabilir. Batı ABD'de hangilerinin ultra diskleri desteklediğini belirlemek için aşağıdaki kod parçacıklarından birini kullanabilirsiniz. Önce değerleri `vmSize` ve `subscription` değerleri değiştirdiğinden emin olun:

```azurecli
$subscription = "<yourSubID>"
$region = "westus"
# example value is Standard_E64s_v3
$vmSize = "<yourVMSize>"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].capabilities" --subscription $subscription
```

```azurepowershell
$region = "westus"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) })[0].Capabilities
```

Yanıt aşağıdaki forma benzer olacak, `UltraSSDAvailable   True` VM boyutu bu bölgedeki ultra diskleri destekleyip desteklemediğini gösterir.

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

## <a name="deploy-an-ultra-disk-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak ultra disk dağıtma

İlk olarak, dağıtılabilmek için VM boyutunu belirleyin. Desteklenen VM boyutlarının listesi için [GA kapsamı ve sınırlamaları](#ga-scope-and-limitations) bölümüne bakın.

Birden çok ultra diskiçeren bir VM oluşturmak istiyorsanız, örnek için bakın [birden çok ultra diskli bir VM oluşturun.](https://aka.ms/ultradiskArmTemplate)

Kendi şablonunuzu kullanmayı planlıyorsanız, **apiVersion'un** `Microsoft.Compute/virtualMachines` `Microsoft.Compute/Disks` (veya `2018-06-01` daha sonra) olarak ayarlandığınızdan emin olun.

Disk sku'yu **UltraSSD_LRS**ayarlayın, ardından disk kapasitesini, IOPS'u, kullanılabilirlik bölgesini ve mbps'deki iş buzunu ultra disk oluşturacak şekilde ayarlayın.

VM sağlandıktan sonra, veri disklerini bölümlere alabilir, biçimlendirebilir ve iş yüklerinize göre yapılandırabilirsiniz.


## <a name="deploy-an-ultra-disk-using-the-azure-portal"></a>Azure portalını kullanarak bir ultra disk dağıtma

Bu bölüm, veri diski olarak ultra diskile donatılmış sanal bir makinenin dağıtılmasını kapsar. Sanal bir makinedağıtma konusunda aşina olduğunuzu varsayar, eğer bilmiyorsanız, [Quickstart: Azure portalında bir Windows sanal makine oluşturun.](../articles/virtual-machines/windows/quick-create-portal.md)

- [Azure portalında](https://portal.azure.com/) oturum açın ve sanal bir makineyi (VM) dağıtmak için gidin.
- Desteklenen bir [VM boyutu ve bölge seçtiğinizden](#ga-scope-and-limitations)emin olun.
- **Kullanılabilirlik seçeneklerinde** **Kullanılabilirlik bölgesini** seçin.
- Kalan girişleri seçtiğiniz seçimlerle doldurun.
- **Diskler**'i seçin.

![oluşturma-ultra-disk etkin-vm.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-ultra-disk-enabled-vm.png)

- Diskler bıçağında, Etkinleştir **Ultra Disk uyumluluğu**için **Evet'i** seçin.
- Şimdi bir ultra disk eklemek için **yeni bir disk oluştur'u seçin ve iliştirin.**

![enable-and-attach-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/enable-and-attach-ultra-disk.png)

- Yeni **bir disk** bıçağı oluştur'da bir ad girin ve ardından **boyutu değiştir'i**seçin.
- Hesap **türünü** **Ultra Disk**olarak değiştirin.
- Özel disk **boyutu (GiB)**, **Disk IOPS**ve **Disk iş buzunun** değerlerini seçtiğiniz değerlerle değiştirin.
- Her iki bıçakta da **Tamam'ı** seçin.
- VM dağıtımına devam edin, diğer VM'leri dağıtacağınız gibi olacaktır.

![oluşturma-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-ultra-disk.png)

## <a name="attach-an-ultra-disk-using-the-azure-portal"></a>Azure portalını kullanarak ultra disk ekleme

Alternatif olarak, mevcut VM'niz ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesindeyse, yeni bir VM oluşturmak zorunda kalmadan ultra disklerden yararlanabilirsiniz. Mevcut VM'nizde ultra diskleri etkinleştirerek ve bunları veri diski olarak takarak.

- VM'nize gidin ve **Diskleri**seçin.
- **Edit'i**seçin.

![seçenekler-seçici-ultra-disks.png](media/virtual-machines-disks-getting-started-ultra-ssd/options-selector-ultra-disks.png)

- **Ultra Disk uyumluluğunu etkinleştirmek**için **Evet'i** seçin.

![ultra-options-yes-enable.png](media/virtual-machines-disks-getting-started-ultra-ssd/ultra-options-yes-enable.png)

- **Kaydet'i**seçin.
- **Ad** için açılan açılan da **veri diski ekle'yi** seçin disk **oluştur'u**seçin.

![oluşturma-ve iliştirme-yeni-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-and-attach-new-ultra-disk.png)

- Yeni diskinizin adını doldurun ve **ardından boyutu değiştir'i**seçin.
- Hesap **türünü** **Ultra Disk**olarak değiştirin.
- Özel disk **boyutu (GiB)**, **Disk IOPS**ve **Disk iş buzunun** değerlerini seçtiğiniz değerlerle değiştirin.
- **Tamam'ı** seçin ve ardından **Oluştur'u**seçin.

![yapma-a-yeni-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/making-a-new-ultra-disk.png)

- Diskinizin bıçağına döndürüldünen sonra **Kaydet'i**seçin.

![tasarruf-ve-ekleme-yeni-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/saving-and-attaching-new-ultra-disk.png)

### <a name="adjust-the-performance-of-an-ultra-disk-using-the-azure-portal"></a>Azure portalını kullanarak ultra diskin performansını ayarlama

Ultra diskler, performanslarını ayarlamanızı sağlayan benzersiz bir özellik sunar. Bu ayarlamaları Azure portalından, disklerin üzerinde yapabilirsiniz.

- VM'nize gidin ve **Diskleri**seçin.
- Performansını değiştirmek istediğiniz ultra diski seçin.

![seçme-ultra-disk-to-modify.png](media/virtual-machines-disks-getting-started-ultra-ssd/selecting-ultra-disk-to-modify.png)

- **Yapılandırma'yı** seçin ve ardından değişikliklerinizi yapın.
- **Kaydet'i**seçin.

![yapılandırma-ultra-disk-performans-ve-boyut.png](media/virtual-machines-disks-getting-started-ultra-ssd/configuring-ultra-disk-performance-and-size.png)

## <a name="deploy-an-ultra-disk-using-cli"></a>CLI kullanarak ultra disk dağıtma

İlk olarak, dağıtılabilmek için VM boyutunu belirleyin. Desteklenen VM boyutlarının listesi için [GA kapsamı ve sınırlamaları](#ga-scope-and-limitations) bölümüne bakın.

Ultra disk eklemek için ultra diskkullanabilen bir VM oluşturmanız gerekir.

**$vmname,** **$rgname,** **$diskname,** **$location,** **$password,** **$user** değişkenleri kendi değerlerinizle değiştirin veya ayarlayın. **$zone,** [bu makalenin başlangıcından](#determine-vm-size-and-region-availability)itibaren aldığınız kullanılabilirlik bölgenizin değerine ayarlayın. Ardından ultra etkin bir VM oluşturmak için aşağıdaki CLI komutunu çalıştırın:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --size Standard_D4s_v3 --location $location
```

### <a name="enable-ultra-disk-compatibility-on-an-existing-vm"></a>Varolan bir VM'de ultra disk uyumluluğunu etkinleştirme

VM'niz GA kapsamı [ve sınırlamalarında](#ga-scope-and-limitations) belirtilen gereksinimleri karşılıyorsa ve [hesabınız için uygun bölgedeyse,](#determine-vm-size-and-region-availability)VM'nizde ultra disk uyumluluğunu etkinleştirebilirsiniz.

Ultra disk uyumluluğunu etkinleştirmek için VM'yi durdurmanız gerekir. VM'yi durdurduktan sonra uyumluluğu etkinleştirebilir, ultra disk takabilir ve VM'yi yeniden başlatabilirsiniz:

```azurecli
az vm deallocate -n $vmName -g $rgName
az vm update -n $vmName -g $rgName --ultra-ssd-enabled true
az vm start -n $vmName -g $rgName
```

### <a name="create-an-ultra-disk-using-cli"></a>CLI kullanarak ultra disk oluşturma

Artık ultra diskler takabilen bir VM'iniz olduğuna göre, ona bir ultra disk oluşturabilir ve ekleyebilirsiniz.

```azurecli-interactive
$location="eastus2"
$subscription="xxx"
$rgname="ultraRG"
$diskname="ssd1"
$vmname="ultravm1"
$zone=123

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

## <a name="attach-an-ultra-disk-to-a-vm-using-cli"></a>CLI kullanarak vm'ye ultra disk ekleme

Alternatif olarak, mevcut VM'niz ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesindeyse, yeni bir VM oluşturmak zorunda kalmadan ultra disklerden yararlanabilirsiniz.

```azurecli
$rgName = "<yourResourceGroupName>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$subscriptionId = "<yourSubscriptionID>"

az vm disk attach -g $rgName --vm-name $vmName --disk $diskName --subscription $subscriptionId
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-cli"></a>CLI kullanarak ultra diskin performansını ayarlama

Ultra diskler performanslarını ayarlamanızı sağlayan benzersiz bir özellik sunar, aşağıdaki komut bu özelliğin nasıl kullanılacağını gösterir:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-disk-using-powershell"></a>PowerShell'i kullanarak ultra disk dağıtma

İlk olarak, dağıtılabilmek için VM boyutunu belirleyin. Desteklenen VM boyutlarının listesi için [GA kapsamı ve sınırlamaları](#ga-scope-and-limitations) bölümüne bakın.

Ultra diskleri kullanmak için, ultra diskler kullanabilen bir VM oluşturmanız gerekir. **$resourcegroup** ve **$vmName** değişkenleri kendi değerlerinizle değiştirin veya ayarlayın. **$zone,** [bu makalenin başlangıcından](#determine-vm-size-and-region-availability)itibaren aldığınız kullanılabilirlik bölgenizin değerine ayarlayın. Ardından, ultra etkin bir VM oluşturmak için aşağıdaki [New-AzVm](/powershell/module/az.compute/new-azvm) komutunu çalıştırın:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD $true `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="enable-ultra-disk-compatibility-on-an-existing-vm"></a>Varolan bir VM'de ultra disk uyumluluğunu etkinleştirme

VM'niz GA kapsamı [ve sınırlamalarında](#ga-scope-and-limitations) belirtilen gereksinimleri karşılıyorsa ve [hesabınız için uygun bölgedeyse,](#determine-vm-size-and-region-availability)VM'nizde ultra disk uyumluluğunu etkinleştirebilirsiniz.

Ultra disk uyumluluğunu etkinleştirmek için VM'yi durdurmanız gerekir. VM'yi durdurduktan sonra uyumluluğu etkinleştirebilir, ultra disk takabilir ve VM'yi yeniden başlatabilirsiniz:

```azurepowershell
#stop the VM
$vm1 = Get-AzureRMVM -name $vmName -ResourceGroupName $rgName
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm1 -UltraSSDEnabled 1
#start the VM
```

### <a name="create-an-ultra-disk-using-powershell"></a>PowerShell'i kullanarak ultra disk oluşturma

Artık ultra diskleri kullanabilen bir VM'iniz olduğuna göre, bir ultra disk oluşturabilir ve ekleyebilirsiniz:

```powershell
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
-DiskName 'Disk02' `
-Disk $diskconfig;
```

## <a name="attach-an-ultra-disk-to-a-vm-using-powershell"></a>PowerShell'i kullanarak vm'ye ultra disk ekleme

Alternatif olarak, mevcut VM'niz ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesindeyse, yeni bir VM oluşturmak zorunda kalmadan ultra disklerden yararlanabilirsiniz.

```powershell
# add disk to VM
$subscription = "<yourSubscriptionID>"
$resourceGroup = "<yourResourceGroup>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$lun = 1
Login-AzureRMAccount -SubscriptionId $subscription
$vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
$disk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $diskName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun $lun
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-powershell"></a>PowerShell'i kullanarak ultra diskin performansını ayarlama

Ultra diskler performanslarını ayarlamanızı sağlayan benzersiz bir yeteneğe sahiptir, aşağıdaki komut, diski ayırmak zorunda kalmadan performansı ayarlayan bir örnektir:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```