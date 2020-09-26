---
title: Azure yönetilen disk performansını artırma
description: Yönetilen diskler için performans katmanları hakkında bilgi edinin ve yönetilen disklerinizin performans katmanlarını nasıl yükselteceğinizi öğrenin.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 09/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 3d6b243ab517f3663f779d01569acf3d46ad8411
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91328131"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>Yönetilen diskler için performans katmanları (Önizleme)

Azure Disk Depolama şu anda, kısa süreli beklenmedik trafiğin işlenmesine yönelik daha yüksek performans elde etmek için yerleşik olarak yerleşik olarak sunulan özellikleri sunmaktadır. Premium SSD 'Lerin gerçek disk boyutunu arttırmadan disk performansını artırma esnekliği vardır. Bu özellik, iş yükü performans ihtiyaçlarını ve maliyetleri düşürmenize olanak tanır. Bu özellik şu anda önizleme aşamasındadır. Bu, tatil alışverişi, performans testi veya eğitim ortamı çalıştırma gibi düzenli olarak daha yüksek bir performans düzeyi gerektiren olaylar için idealdir. Bu olayları işlemek için, gerekli olduğu sürece daha yüksek bir performans katmanı seçebilir ve ek performans artık gerekli olmadığında özgün katmana geri dönebilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir diski ilk kez dağıttığınızda veya sağladığınızda, bu diskin temel performans katmanı sağlanan disk boyutuna göre ayarlanır. Daha yüksek talebi karşılamak için daha yüksek bir performans katmanı seçilebilir ve bu performansa artık gerek kalmadığında, başlangıçtaki taban çizgisi performans katmanına geri dönebilirsiniz.

Cihazınız değiştikçe faturanızı değişiklikleriniz. Örneğin, bir P10 diski (128 GiB) sağlarsanız, taban çizgisi performans katmanınız P10 (500 ıOPS ve 100 MB/s) olarak ayarlanır ve P10 fiyatı üzerinden faturalandırılırsınız. Katmanı, disk boyutunu arttırmadan P50 (7500 ıOPS ve 250 MB/s) performansını eşleşecek şekilde güncelleştirebilirsiniz. bu süre, P50 fiyatı üzerinden faturalandırılacaksınız. Daha yüksek performans artık gerekli olmadığında, P10 katmanına geri dönebilir ve P10 ücretine göre disk yeniden faturalandırılacaktır.

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

- Şu anda yalnızca Premium SSD 'Ler için desteklenir.
- Katmanlar değiştirilmeden önce diskler çalışan bir VM 'den ayrılmalıdır.
- P60, P70 ve P80 performans katmanlarının kullanılması 4096 GiB veya üzeri disklerle kısıtlıdır.
- Bir disk performans katmanı, her 24 saatte bir yalnızca bir kez değiştirilebilir.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Yönetilen bir diskin performans katmanını ayarlamak Şu anda yalnızca şu bölgelerde Premium SSD 'Ler için kullanılabilir:

- Orta Batı ABD 

## <a name="create-an-empty-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Taban çizgisi katmanından daha yüksek bir katman ile boş bir veri diski oluşturma

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
     
## <a name="update-the-tier-of-a-disk"></a>Bir diskin katmanını güncelleştirme

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>

az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```
## <a name="show-the-tier-of-a-disk"></a>Bir diskin katmanını göster

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

## <a name="next-steps"></a>Sonraki adımlar

Daha büyük performans katmanlarından yararlanmak için bir diski yeniden boyutlandırmanız gerekiyorsa, konudaki makalelerimize bakın:

- [Azure CLı ile Linux VM 'de sanal sabit diskler genişletme](linux/expand-disks.md)
- [Windows sanal makinesine bağlı yönetilen diski Genişlet](windows/expand-os-disk.md)
