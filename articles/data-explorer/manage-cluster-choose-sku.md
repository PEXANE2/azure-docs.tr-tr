---
title: Azure Veri Gezgini kümeniz için doğru VM SKU 'sunu seçin
description: Bu makalede, Azure Veri Gezgini kümesi için en uygun SKU boyutunu seçme açıklanmaktadır.
author: avnera
ms.author: avnera
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: b0cf6eab86b0b932e44b6824305c23df01f35808
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383825"
---
# <a name="select-the-correct-vm-sku-for-your-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümeniz için doğru VM SKU 'sunu seçin 

Azure Veri Gezgini, yeni bir küme oluştururken veya değişen bir iş yükü için kümeyi iyileştirerek seçebileceğiniz birden çok VM SKU 'su vardır. VM 'Ler, her iş yükü için en iyi maliyete izin vermek üzere dikkatle seçildi. 

Veri yönetimi kümesinin boyutu ve VM SKU 'SU Azure Veri Gezgini hizmeti tarafından tam olarak yönetilir. Altyapının VM boyutu ve Alım iş yükü gibi faktörlerle belirlenir. 

Altyapı kümesine yönelik VM SKU 'SU, [küme ölçeğini ölçeklendirerek](manage-cluster-vertical-scaling.md)dilediğiniz zaman değiştirilebilir. Bu nedenle, ilk senaryoya uyan minimum SKU boyutuyla başlamak en iyisidir. Küme, yeni VM SKU 'SU ile yeniden oluşturulduğu sırada kümenin ölçeğini 30 dakikaya varan bir süre içinde azaltmaya dikkat edin.

> [!TIP]
> İşlem [RI (ayrılmış örnekler)](https://docs.microsoft.com/azure/virtual-machines/windows/prepay-reserved-vm-instances) Azure Veri Gezgini kümesi için geçerlidir.  

Bu makalede farklı VM SKU 'SU seçenekleri açıklanmakta ve en iyi seçim yapmanıza yardımcı olabilecek teknik ayrıntılar sağlanmaktadır.

## <a name="select-the-cluster-type"></a>Küme türünü seçin

Azure Veri Gezgini iki tür küme sunar:

* **Üretim**: Üretim kümeleri, motor ve veri yönetimi kümeleri için iki düğüm içerir ve Azure Veri Gezgini [SLA](https://azure.microsoft.com/support/legal/sla/data-explorer/v1_0/)altında işletilebilir.

* **Geliştirme/test (SLA yok)** : Geliştirme ve test kümelerinde, altyapı kümesi için tek bir D11_v2 düğümü ve veri yönetimi kümesi için tek bir D1 düğümü vardır. Düşük örnek sayısı ve altyapının işaretleme ücreti olmadığından, bu küme türü en düşük maliyetli bir yapılandırmadır. Artıklık içermediğinden bu küme yapılandırması için SLA yok.

## <a name="sku-types"></a>SKU türleri

Azure Veri Gezgini kümesi oluştururken, planlı iş yükü için *en iyi* VM SKU 'sunu seçin. Azure Veri Gezgini 'in arasından seçim yapabileceğiniz iki SKU ailesi vardır:

* **D_V2**: D SKU, işlem için iyileştirilmiş ve iki şekilde sağlanır.
    * VM 'nin kendisi
    * Premium Depolama diskleriyle paketlenmiş VM

* **LS**: L SKU, depolama için iyileştirildi. Benzer fiyatlandırıld SKU 'sundan daha büyük bir SSD boyutuna  sahiptir.

Aşağıdaki tabloda, kullanılabilir SKU türleri arasındaki temel farklılıklar verilmiştir:
 
|**Özniteliğe** | **D SKU 'SU** | **L SKU 'SU**
|---|---|---
|**Küçük SKU 'Lar**|En az iki çekirdekli boyut ' 11 '|En küçük boyut, dört çekirdekli ' L4 '
|**Kullanılabilirlik**|Tüm bölgelerde kullanılabilir (DS + PS sürümü daha sınırlı kullanılabilirliğe sahiptir)|Birkaç bölgede kullanılabilir
|**Çekirdek başına GB başına maliyet**|D SKU 'SU ile yüksek, DS + PS sürümü ile düşük|*Kullandıkça Öde* seçeneği ile Cheapest
|**RI (ayrılmış örnekler) fiyatlandırması**|Yüksek indirim (üç yıllık taahhüt için% 55 üzerinden)|Düşük indirim (üç yıllık taahhüt için% 20)  

## <a name="select-your-cluster-vm"></a>Küme VM 'nizi seçme 

Küme VM 'nizi seçmek için [Dikey ölçeklendirmeyi yapılandırın](manage-cluster-vertical-scaling.md#configure-vertical-scaling). 

Farklı VM SKU 'SU seçenekleri, istenen senaryo için gerekli performans ve sık erişimli önbellek gereksinimlerinin maliyetlerini iyileştirmenize olanak tanır. Senaryo yüksek bir sorgu birimi için en iyi performansı gerektiriyorsa ideal SKU, işlem için iyileştirilmiş olmalıdır. Diğer taraftan, senaryo görece daha düşük sorgu yüküne sahip büyük hacimlerde veri sorgulamayı gerektiriyorsa, depolama için iyileştirilmiş SKU, mükemmel performans sağlarken maliyetleri azaltır.

Küçük SKU 'Lar için küme başına örnek sayısı sınırlı olduğundan, daha fazla RAM 'e sahip daha büyük VM 'Lerin kullanılması tercih edilir. RAM boyutu, kullanılan `joins`sorgular gibi RAM kaynağına daha fazla talep döndüren bazı sorgu türleri için gereklidir. Bu nedenle, ölçekleme seçeneklerini düşünürken daha fazla örnek ekleyerek ölçeği genişleme özelliğinden daha büyük bir SKU 'ya ölçeklendirmeniz önerilir.

## <a name="vm-options"></a>VM seçenekleri

Aşağıdaki tabloda Azure Veri Gezgini kümesi VM 'Leri için teknik belirtimler sunulmaktadır:

|**Name**| **Kategori** | **SSD boyutu** | **Sayısı** | **KOÇ** | **Premium depolama diskleri (1 TB)**| **Küme başına en az örnek sayısı** | **Küme başına en fazla örnek sayısı**
|---|---|---|---|---|---|---|---
|D11_v2| işlem için iyileştirilmiş | 75 GB    | 2 | 14 GB | 0 | 1\. | 8 (1 olduğu geliştirme ve test SKU 'SU hariç)
|D12_v2| işlem için iyileştirilmiş | 150 GB   | 4 | 28 GB | 0 | 2 | 16
|D13_v2| işlem için iyileştirilmiş | 307 GB   | 8 | 56 GB | 0 | 2 | 1000
|D14_v2| işlem için iyileştirilmiş | 614 GB   | 16| 112 GB | 0 | 2 | 1000
|DS13_v2 + 1TB PS| depolama için iyileştirilmiş | 1 TB | 8 | 56 GB | 1 | 2 | 1000
|DS13_v2 + 2TB PS| depolama için iyileştirilmiş | 2 TB | 8 | 56 GB | 2 | 2 | 1000
|DS14_v2 + 3TB PS| depolama için iyileştirilmiş | 3 TB | 16 | 112 GB | 2 | 2 | 1000
|DS14_v2 + 4TB PS| depolama için iyileştirilmiş | 4 TB | 16 | 112 GB | 4 | 2 | 1000
|L4s_v1| depolama için iyileştirilmiş | 650 GB | 4 | 32 GB | 0 | 2 | 16
|L8s_v1| depolama için iyileştirilmiş | 1,3 TB | 8 | 64 GB | 0 | 2 | 1000
|L16s_1| depolama için iyileştirilmiş | 2,6 TB | 16| 128 GB | 0 | 2 | 1000

* Azure Veri Gezgini [Listsku 'larını](/dotnet/api/microsoft.azure.management.kusto.clustersoperationsextensions.listskus?view=azure-dotnet)kullanarak bölge BAŞıNA GÜNCELLEŞTIRILMIŞ VM SKU listesini görüntüleyin. 
* [Farklı Işlem SKU 'ları](/azure/virtual-machines/windows/sizes-compute)hakkında daha fazla bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar

* Altyapı kümesi, farklı gereksinimler için VM SKU 'SU değiştirilerek dilediğiniz zaman [yukarı veya aşağı ölçeklendirilebilir](manage-cluster-vertical-scaling.md) . 

* Motor kümesinin boyutu, değişen taleplerle kapasiteyi değiştirmek için, [içinde ölçeklendirilebilir ve dışarı](manage-cluster-horizontal-scaling.md) değiştirilebilir.

