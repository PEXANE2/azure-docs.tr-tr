---
title: Azure yönetilen disklerinin performansını değiştirme
description: Yönetilen diskler için performans katmanları hakkında bilgi edinin ve Azure PowerShell modülünü ya da Azure CLı kullanarak mevcut yönetilen disklerin performans katmanlarını değiştirmeyi öğrenin.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 11/11/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 923c5970183bd192ac1a2f20fb775d96dcc06865
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94540646"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>Yönetilen diskler için performans katmanları (Önizleme)

Azure Disk Depolama, kısa süreli beklenmedik trafiğin işlenmesine yönelik daha yüksek performans sağlamak için yerleşik olarak kullanılan bir özellik sunar. Premium SSD 'Lerin gerçek disk boyutunu arttırmadan disk performansını artırma esnekliği vardır. Bu özellik, iş yükünün performans ihtiyaçlarını ve maliyetlerini azaltmanızı sağlar. 

> [!NOTE]
> Bu özellik şu anda önizleme sürümündedir. 

Bu özellik, tatil alışverişi, performans testi veya eğitim ortamı çalıştırma gibi düzenli olarak daha yüksek bir performans düzeyi gerektiren olaylar için idealdir. Bu olayları işlemek için, ihtiyacınız olduğu sürece daha yüksek bir performans katmanı kullanabilirsiniz. Daha sonra ek performansa ihtiyacınız kalmadığında Orijinal katmana geri dönebilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir diski ilk kez dağıttığınızda veya sağladığınızda, bu diskin temel performans katmanı sağlanan disk boyutuna göre ayarlanır. Daha yüksek talebi karşılamak için daha yüksek bir performans katmanı kullanabilirsiniz. Artık bu performans düzeyine ihtiyacınız kalmadığında başlangıçtaki taban çizgisi performans katmanına geri dönebilirsiniz.

Cihazınız değiştikçe faturanızı değişiklikleriniz. Örneğin, bir P10 diski (128 GiB) sağlarsanız, taban çizgisi performans katmanınız P10 (500 ıOPS ve 100 MBps) olarak ayarlanır. P10 fiyatı üzerinden faturalandırılırsınız. Disk boyutunu arttırmadan katmanı, P50 (7.500 ıOPS ve 250 MBps) performansını eşleşecek şekilde yükseltebilirsiniz. Yükseltme sırasında, P50 fiyatı üzerinden faturalandırılırsınız. Artık daha yüksek performansa ihtiyacınız kalmadığında P10 katmanına geri dönebilirsiniz. Disk bir kez daha P10 ücret üzerinden faturalandırılır.

| Disk boyutu | Taban çizgisi performans katmanı | , Sürümüne yükseltilebilir |
|----------------|-----|-------------------------------------|
| 4 GiB | P1 | P2, P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 8 GiB | P2 | P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 16 GiB | P3 | P4, P6, P10, P15, P20, P30, P40, P50 | 
| 32 GiB | P4 | P6, P10, P15, P20, P30, P40, P50 |
| 64 GiB | P6 | P10, P15, P20, P30, P40, P50 |
| 128 GiB | P10 | P15, P20, P30, P40, P50 |
| 256 GiB | P15 | P20, P30, P40, P50 |
| 512 GiB | P20 | P30, P40, P50 |
| 1 TiB | P30 | P40, P50 |
| 2 TiB | P40 | P50 |
| 4 TiB | P50 | Yok |
| 8 TiB | P60 |  P70, P80 |
| 16 TiB | P70 | P80 |
| 32 TiB | P80 | Yok |

Fatura bilgileri için bkz. [yönetilen disk fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="restrictions"></a>Kısıtlamalar

- Bu özellik şu anda yalnızca Premium SSD 'Ler için destekleniyor.
- Diskin katmanını değiştirmeden önce VM 'nizi serbest bırakabilir veya diski çalışan bir VM 'den ayırmanız gerekir.
- P60, P70 ve P80 performans katmanlarının kullanılması 4.096 GiB veya üzeri disklerle kısıtlıdır.
- Bir diskin performans katmanı, her 24 saatte bir yalnızca bir kez indirgenir.

## <a name="create-an-empty-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Taban çizgisi katmanından daha yüksek bir katman ile boş bir veri diski oluşturma

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
subscriptionId=<yourSubscriptionIDHere>
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
diskSize=<yourDiskSizeHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus

az login

az account set --subscription $subscriptionId

az disk create -n $diskName -g $resourceGroupName -l $region --sku Premium_LRS --size-gb $diskSize --tier $performanceTier
```
## <a name="create-an-os-disk-with-a-tier-higher-than-the-baseline-tier-from-an-azure-marketplace-image"></a>Azure Marketi görüntüsünden taban çizgisi katmanından daha yüksek katmanı olan bir işletim sistemi diski oluşturma

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus
image=Canonical:UbuntuServer:18.04-LTS:18.04.202002180

az disk create -n $diskName -g $resourceGroupName -l $region --image-reference $image --sku Premium_LRS --tier $performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$subscriptionId='yourSubscriptionID'
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$diskSizeInGiB=4
$performanceTier='P50'
$sku='Premium_LRS'
$region='westcentralus'

Connect-AzAccount

Set-AzContext -Subscription $subscriptionId

$diskConfig = New-AzDiskConfig -SkuName $sku -Location $region -CreateOption Empty -DiskSizeGB $diskSizeInGiB -Tier $performanceTier
New-AzDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
```
---

## <a name="update-the-tier-of-a-disk"></a>Bir diskin katmanını güncelleştirme

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>

az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$performanceTier='P1'

$diskUpdateConfig = New-AzDiskUpdateConfig -Tier $performanceTier

Update-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName -DiskUpdate $diskUpdateConfig
```
---

## <a name="show-the-tier-of-a-disk"></a>Bir diskin katmanını göster

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$disk = Get-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName

$disk.Tier
```
---

## <a name="next-steps"></a>Sonraki adımlar

Daha yüksek performans katmanlarından yararlanmak için bir diski yeniden boyutlandırmanız gerekiyorsa, şu makalelere bakın:

- [Azure CLı ile Linux VM 'de sanal sabit diskler genişletme](linux/expand-disks.md)
- [Windows sanal makinesine bağlı yönetilen diski Genişlet](windows/expand-os-disk.md)
