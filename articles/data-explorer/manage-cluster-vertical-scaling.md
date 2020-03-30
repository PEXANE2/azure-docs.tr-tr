---
title: Azure Veri Gezgini'ndeki talebi eşleştirmek için küme dikey ölçekleme (ölçeklendirme) yönetme
description: Bu makalede, değişen talebe göre bir Azure Veri Gezgini kümesini ölçeklendirme ve küçültme adımları açıklanmaktadır.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 95275598febae2b6b0355a7bc3e512490dae500d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560448"
---
# <a name="manage-cluster-vertical-scaling-scale-up-in-azure-data-explorer-to-accommodate-changing-demand"></a>Değişen talebi karşılamak için Azure Veri Gezgini'nde küme dikey ölçekleme (ölçeklendirme) yönetme

Bir kümeyi uygun şekilde boyutlandırmak, Azure Veri Gezgini'nin performansı için çok önemlidir. Statik küme boyutu, ikisi de ideal olmayan az kullanıma veya aşırı kullanıma yol açabilir.

Bir kümedeki talep mutlak doğrulukla tahmin edilemediğinden, daha *scale* iyi bir yaklaşım, değişen taleple kapasite ve CPU kaynaklarını ekleyerek ve kaldırarak bir kümeyi ölçeklendirmektir. 

Azure Veri Gezgini kümesini ölçekleme için iki iş akışı vardır:

* [Yatay ölçekleme](manage-cluster-horizontal-scaling.md), ayrıca ölçekleme ve dışarı denir.
* Dikey ölçekleme, yukarı ve aşağı ölçekleme olarak da adlandırılır.

Bu makalede, dikey ölçekleme iş akışı açıklanmaktadır:

## <a name="configure-vertical-scaling"></a>Dikey ölçekleme yapılandırma

1. Azure portalında Azure Veri Gezgini küme kaynağınıza gidin. **Ayarlar**altında, **Yukarı Ölçekle'yi**seçin.

1. **Ölçeklendirme** penceresinde, kümeniz için kullanılabilir SNU'ların listesini görürsünüz. Örneğin, aşağıdaki şekilde yalnızca dört SNU kullanılabilir.

    ![Ölçeği artırma](media/manage-cluster-vertical-scaling/scale-up.png)

    SKU'lar geçerli SKU oldukları için devre dışı bırakılır veya kümenin bulunduğu bölgede kullanılamazlar.

1. SKU'nuzu değiştirmek için yeni bir SKU seçin ve **Seç'i**tıklatın.

> [!NOTE]
> * Dikey ölçekleme işlemi birkaç dakika sürebilir ve bu süre içinde kümeniz askıya alınır. 
> * Ölçekleme küme performansınıza zarar verebilir.
> * Fiyat, kümenin sanal makinelerinin ve Azure Veri Gezgini hizmet maliyetlerinin bir tahminidir. Diğer masraflar dahil değildir. Bir tahmin için Azure Veri Gezgini [maliyet tahmin civarı](https://dataexplorer.azure.com/AzureDataExplorerCostEstimator.html) sayfasına ve tam fiyatlandırma bilgileri için Azure Veri Gezgini [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/data-explorer/) bakın.

Azure Veri Gezgini kümeniz için dikey ölçekleme yapılandırıldınız. Yatay ölçekleme için başka bir kural ekleyin. Küme ölçekleme sorunları yla ilgili yardıma ihtiyacınız varsa, Azure portalında [bir destek isteği açın.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

## <a name="next-steps"></a>Sonraki adımlar

* Belirttiğiniz ölçümlere göre örnek sayısını dinamik olarak ölçeklendirmek için [küme yatay ölçeklemesini yönetin.](manage-cluster-horizontal-scaling.md)

* Bu makaleye göre kaynak kullanımınızı izleyin: [Azure Veri Gezgini performansını, sistem durumunu ve kullanımını ölçümlerle izleyin.](using-metrics.md)

