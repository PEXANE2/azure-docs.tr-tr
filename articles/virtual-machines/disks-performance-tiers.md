---
title: Azure yönetilen disk performansını artırma
description: Yönetilen diskler için performans katmanları hakkında bilgi edinin ve yönetilen disklerinizin performans katmanlarını nasıl yükselteceğinizi öğrenin.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 09/22/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: aa188babf56d4a825059fe6103e2e07745eb134f
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90974134"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>Yönetilen diskler için performans katmanları (Önizleme)

Azure Disk Depolama şu anda, kısa süreli beklenmedik trafiğin işlenmesine yönelik daha yüksek performans elde etmek için yerleşik olarak yerleşik olarak sunulan özellikleri sunmaktadır. Premium SSD 'Lerin gerçek disk boyutunu arttırmadan disk performansını artırma esnekliği vardır. Bu özellik, iş yükü performans ihtiyaçlarını ve maliyetleri düşürmenize olanak tanır. Bu özellik şu anda önizleme aşamasındadır. Bu, tatil alışverişi, performans testi veya eğitim ortamı çalıştırma gibi düzenli olarak daha yüksek bir performans düzeyi gerektiren olaylar için idealdir. Bu olayları işlemek için, gerekli olduğu sürece daha yüksek bir performans katmanı seçebilir ve ek performans artık gerekli olmadığında özgün katmana geri dönebilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir diski ilk kez dağıttığınızda veya sağladığınızda, bu diskin temel performans katmanı sağlanan disk boyutuna göre ayarlanır. Daha yüksek talebi karşılamak için daha yüksek bir performans katmanı seçilebilir ve bu performansa artık gerek kalmadığında, başlangıçtaki taban çizgisi performans katmanına geri dönebilirsiniz. Örneğin, bir P10 diski (128 GiB) sağlarsanız, taban çizgisi performans katmanınız P10 (500 ıOPS ve 100 MB/s) olarak ayarlanır. Katmanı, disk boyutunu arttırmadan, P50 (7500 ıOPS ve 250 MB/s) performansı ile eşleşecek şekilde güncelleştirebilirsiniz ve daha yüksek performans artık gerekmiyorsa P10 'e geri dönebilirler.

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

## <a name="restrictions"></a>Kısıtlamalar

- Şu anda yalnızca Premium SSD 'Ler için desteklenir.
- Katmanlar değiştirilmeden önce diskler çalışan bir VM 'den ayrılmalıdır.
- P60, P70 ve P80 performans katmanlarının kullanılması 4096 GiB veya üzeri disklerle kısıtlıdır.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Yönetilen bir diskin performans katmanını ayarlamak Şu anda yalnızca şu bölgelerde Premium SSD 'Ler için kullanılabilir:

- Orta Batı ABD 
- Doğu 2 ABD 
- Batı Avrupa
- Doğu Avustralya 
- Güney Doğu Avustralya 
- Güney Hindistan

## <a name="createupdate-a-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Taban çizgisi katmanından daha yüksek bir katman ile veri diski oluşturma/güncelleştirme

1. Taban çizgisi katmanından daha yüksek bir katman ile boş bir veri diski oluşturun veya [CreateUpdateDataDiskWithTier.js](https://github.com/Azure/azure-managed-disks-performance-tiers/blob/main/CreateUpdateDataDiskWithTier.json) örnek şablon kullanarak taban çizgisi katmanından daha yüksek olan bir diskin katmanını güncelleştirin

     ```cli
     subscriptionId=<yourSubscriptionIDHere>
     resourceGroupName=<yourResourceGroupNameHere>
     diskName=<yourDiskNameHere>
     diskSize=<yourDiskSizeHere>
     performanceTier=<yourDesiredPerformanceTier>
     region=<yourRegionHere>
    
     az login
    
     az account set --subscription $subscriptionId
    
     az group deployment create -g $resourceGroupName \
     --template-uri "https://raw.githubusercontent.com/Azure/azure-managed-disks-performance-tiers/main/CreateUpdateDataDiskWithTier.json" \
     --parameters "region=$region" "diskName=$diskName" "performanceTier=$performanceTier" "dataDiskSizeInGb=$diskSize"
     ```

1. Diskin katmanını onaylama

    ```cli
    az resource show -n $diskName -g $resourceGroupName --namespace Microsoft.Compute --resource-type disks --api-version 2020-06-30 --query [properties.tier] -o tsv
     ```

## <a name="createupdate-an-os-disk-with-a-tier-higher-than-the-baseline-tier"></a>Taban çizgisi katmanından daha yüksek bir katman ile işletim sistemi diski oluşturma/güncelleştirme

1. Market görüntüsünden bir işletim sistemi diski oluşturun veya [CreateUpdateOSDiskWithTier.js](https://github.com/Azure/azure-managed-disks-performance-tiers/blob/main/CreateUpdateOSDiskWithTier.json) örnek şablonu kullanarak bir işletim sistemi diskinin katmanını taban çizgisi katmanından daha yükseğe güncelleştirin

     ```cli
     resourceGroupName=<yourResourceGroupNameHere>
     diskName=<yourDiskNameHere>
     performanceTier=<yourDesiredPerformanceTier>
     region=<yourRegionHere>
    
     az group deployment create -g $resourceGroupName \
     --template-uri "https://raw.githubusercontent.com/Azure/azure-managed-disks-performance-tiers/main/CreateUpdateOSDiskWithTier.json" \
     --parameters "region=$region" "diskName=$diskName" "performanceTier=$performanceTier"
     ```
 
 1. Diskin katmanını onaylama
 
     ```cli
     az resource show -n $diskName -g $resourceGroupName --namespace Microsoft.Compute --resource-type disks --api-version 2020-06-30 --query [properties.tier] -o tsv
     ```

## <a name="next-steps"></a>Sonraki adımlar

Daha büyük performans katmanlarından yararlanmak için bir diski yeniden boyutlandırmanız gerekiyorsa, konudaki makalelerimize bakın:

- [Azure CLı ile Linux VM 'de sanal sabit diskler genişletme](linux/expand-disks.md)
- [Windows sanal makinesine bağlı yönetilen diski Genişlet](windows/expand-os-disk.md)