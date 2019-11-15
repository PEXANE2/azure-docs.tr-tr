---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 63045bf1b836215b00b9b7c1b46dd208152fa772
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2019
ms.locfileid: "74101594"
---
Azure Ultra diskler, Azure IaaS sanal makineleri (VM 'Ler) için yüksek aktarım hızı, yüksek ıOPS ve tutarlı düşük gecikme süreli disk depolama alanı sunar. Bu yeni teklif, var olan diskler tekliflerimiz ile aynı Kullanılabilirlik düzeylerinde satır performansının üst kısmında yer sağlar. Ultra disklerin büyük bir avantajı, sanal makinelerinizi yeniden başlatmanıza gerek kalmadan SSD 'nin performansını ve iş yüklerinizde dinamik olarak değiştirme yeteneğidir. Ultra diskler SAP HANA, üst katman veritabanları ve işlem açısından ağır iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur.

## <a name="ga-scope-and-limitations"></a>GA kapsamı ve sınırlamaları

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]

## <a name="determine-vm-size-and-region-availability"></a>VM boyutunu ve bölge kullanılabilirliğini belirleme

Ultra disklerden yararlanmak için, hangi kullanılabilirlik bölgesinin bulunduğunu belirlemeniz gerekir. Her bölge, Ultra disklerle her VM boyutunu desteklememektedir. Bölgeniz, bölgeniz ve VM boyutunuzu Ultra diskleri destekleyip desteklemediğini öğrenmek için aşağıdaki komutlardan birini çalıştırın, önce **Region**, **VMSize**ve **abonelik** değerlerini değiştirdiğinizden emin olun:

CLı

```bash
$subscription = "<yourSubID>"
$region = "<yourLocation>, example value is southeastasia"
$vmSize = "<yourVMSize>, example value is Standard_E64s_v3"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].locationInfo[0].zoneDetails[0].Name" --subscription $subscription
```

PowerShell:

```powershell
$region = "southeastasia"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) -and $_.LocationInfo[0].ZoneDetails.Count -gt 0})[0].LocationInfo[0].ZoneDetails
```

Yanıt aşağıdaki biçimde olacaktır; burada X, seçtiğiniz bölgede dağıtım için kullanılacak bölgedir. X 1, 2 veya 3 olabilir.

Bölge değerini koru, kullanılabilirlik **bölgenizi** temsil eder ve bir ultra disk dağıtmak için bu alana ihtiyacınız olur.

|ResourceType  |Ad  |Konum  |Bölgeler  |Kısıtlama  |Özellik  |Değer  |
|---------|---------|---------|---------|---------|---------|---------|
|disklerinden     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Komuttan yanıt yoksa, seçilen VM boyutu seçili bölgedeki Ultra disklerle desteklenmez.

Hangi bölgeyi dağıtacağınızı bildiğinize göre, bu makaledeki dağıtım adımlarını izleyerek bir ultra disk eklenmiş bir VM dağıtın veya var olan bir VM 'ye bir ultra disk takın.

## <a name="deploy-an-ultra-disk-using-azure-resource-manager"></a>Azure Resource Manager kullanarak bir ultra disk dağıtma

İlk olarak, dağıtılacak VM boyutunu saptayın. Desteklenen VM boyutlarının listesi için bkz. [ga kapsam ve sınırlamalar](#ga-scope-and-limitations) bölümü.

Birden çok Ultra disk içeren bir VM oluşturmak isterseniz, örneğe [birden çok Ultra disk içeren BIR VM oluşturma](https://aka.ms/ultradiskArmTemplate)konusuna bakın.

Kendi şablonunuzu kullanmayı düşünüyorsanız, `Microsoft.Compute/virtualMachines` ve `Microsoft.Compute/Disks` **Apiversion** ' ın `2018-06-01` (veya üzeri) olarak ayarlandığından emin olun.

Disk SKU 'sunu **UltraSSD_LRS**olarak ayarlayın, ardından bir ultra disk oluşturmak için disk KAPASITESINI, IOPS 'yi, kullanılabilirlik alanını ve aktarım hızını MB cinsinden ayarlayın.

VM sağlandıktan sonra veri disklerini bölümleyebilir ve biçimlendirebilir ve iş yükleriniz için yapılandırabilirsiniz.

## <a name="deploy-an-ultra-disk-using-cli"></a>CLı kullanarak bir ultra disk dağıtma

İlk olarak, dağıtılacak VM boyutunu saptayın. Desteklenen VM boyutlarının listesi için [ga kapsam ve sınırlamalar](#ga-scope-and-limitations) bölümüne bakın.

Bir ultra disk eklemek için, Ultra diskler kullanabilen bir sanal makine oluşturmanız gerekir.

**$VMName**, **$RgName**, **$diskname**, **$Location**, **$Password**, **$User** değişkenlerini kendi değerlerinizle değiştirin veya ayarlayın. [Bu makalenin başlangıcından](#determine-vm-size-and-region-availability)aldığınız kullanılabilirlik bölgenizin değerine **$Zone** ayarlayın. Ardından, bir ultra etkin VM oluşturmak için aşağıdaki CLı komutunu çalıştırın:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --size Standard_D4s_v3 --location $location
```

### <a name="create-an-ultra-disk-using-cli"></a>CLı kullanarak bir ultra disk oluşturma

Artık Ultra diskler iliştirebilen bir VM 'ye sahip olduğunuza göre, buna bir ultra disk oluşturup ekleyebilirsiniz.

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

## <a name="attach-an-ultra-disk-to-a-vm-using-cli"></a>CLı kullanarak bir VM 'ye Ultra disk iliştirme

Alternatif olarak, mevcut sanal makinenizin Ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesi varsa, yeni bir VM oluşturmak zorunda kalmadan Ultra disklerin kullanımını sağlayabilirsiniz.

```bash
$rgName = "<yourResourceGroupName>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$subscriptionId = "<yourSubscriptionID>"

az vm disk attach -g $rgName --vm-name $vmName --disk $diskName --subscription $subscriptionId
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-cli"></a>CLı kullanarak bir ultra diskin performansını ayarlama

Ultra diskler, performansını ayarlamanıza olanak tanıyan benzersiz bir özellik sunar ve aşağıdaki komutta bu özelliğin nasıl kullanılacağı gösterilmektedir:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-disk-using-powershell"></a>PowerShell kullanarak bir ultra disk dağıtma

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

### <a name="create-an-ultra-disk-using-powershell"></a>PowerShell kullanarak bir ultra disk oluşturma

Artık Ultra diskler kullanabilen bir VM 'ye sahip olduğunuza göre, buna bir ultra disk oluşturup ekleyebilirsiniz:

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

## <a name="attach-an-ultra-disk-to-a-vm-using-powershell"></a>PowerShell kullanarak bir VM 'ye Ultra disk iliştirme

Alternatif olarak, mevcut sanal makinenizin Ultra diskler kullanabilen bir bölge/kullanılabilirlik bölgesi varsa, yeni bir VM oluşturmak zorunda kalmadan Ultra disklerin kullanımını sağlayabilirsiniz.

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

### <a name="adjust-the-performance-of-an-ultra-disk-using-powershell"></a>PowerShell kullanarak bir ultra diskin performansını ayarlama

Ultra disklerin, performansını ayarlamanıza olanak tanıyan benzersiz bir özelliği vardır ve aşağıdaki komut, diski ayırmak zorunda kalmadan performansı ayarlayan bir örnektir:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```

## <a name="next-steps"></a>Sonraki adımlar

[Bu anketle](https://aka.ms/UltraDiskSignup)yeni disk türü isteği erişimini denemek istiyorsanız.
