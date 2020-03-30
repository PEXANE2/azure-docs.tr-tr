---
title: Gen2 önbelleğinizi optimize edin
description: Azure portalını kullanarak Gen2 önbelleğinizi nasıl izleyeceğinizi öğrenin.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.subservice: ''
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: bb5560164af2b573e6aaffd4e4c62bbe0dc24a51
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350416"
---
# <a name="how-to-monitor-the-gen2-cache"></a>Gen2 önbelleği nasıl izlenir?

Bu makalede, iş yükünüzün Gen2 önbelleğinden en iyi şekilde yararlanıp yararlanmadığını belirleyerek yavaş sorgu performansını nasıl izleyeceğiniz ve sorun gidereceğiniz açıklanmaktadır.

Gen2 depolama mimarisi, gen2 veri ambarları için tasarlanmış NVMe tabanlı SSD'lerde bulunan bir önbellekte en sık sorgulanan sütun mağaza segmentlerinizi otomatik olarak katmanlar. Sorgularınız önbellekte bulunan kesimleri aldığında daha fazla performans gerçekleştirilir.
 
## <a name="troubleshoot-using-the-azure-portal"></a>Azure portalını kullanarak sorun giderme

Sorgu performansını gidermek için Gen2 önbellek ölçümlerini görüntülemek için Azure Monitor'u kullanabilirsiniz. Önce Azure portalına gidin ve **Monitör,** **Ölçümler** ve **+ Kapsam seçin'e**tıklayın:

![Azure İzleyici](./media/sql-data-warehouse-how-to-monitor-cache/cache-0.png)

Veri ambarınızı bulmak için arama ve bırakma çubuklarını kullanın. Sonra uygula'yı seçin.

![Azure İzleyici](./media/sql-data-warehouse-how-to-monitor-cache/cache-1.png)

Gen2 önbelleğini giderme için önemli ölçümler **Önbellek isabet yüzdesi** ve **Önbellek kullanılan yüzdesi.** **Önbellek isabet yüzdesini** seçin ve **önbellek kullanılan yüzdeyi**eklemek için **metrik ekle** düğmesini kullanın. 

![Önbellek Ölçümleri](./media/sql-data-warehouse-how-to-monitor-cache/cache-2.png)

## <a name="cache-hit-and-used-percentage"></a>Önbellek isabet ve kullanılan yüzdesi

Aşağıdaki matris, önbellek ölçümlerinin değerlerini temel alan senaryoları açıklar:

|                                | **Yüksek Önbellek isabet yüzdesi** | **Düşük Önbellek isabet yüzdesi** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Yüksek Önbellek kullanılan yüzdesi** |          Senaryo 1           |          Senaryo 2          |
| **Düşük Önbellek kullanılan yüzdesi**  |          3. Senaryo           |          4. Senaryo          |

**Senaryo 1:** Önbelleğinizi en iyi şekilde kullanıyorsunuz. Sorgularınızı yavaşlatan diğer alanları [sorun giderin.](sql-data-warehouse-manage-monitor.md)

**Senaryo 2:** Geçerli çalışma veri kümeniz önbelleğe sığamaz ve bu da fiziksel okumalar nedeniyle önbellek isabet yüzdesinin düşmesine neden olur. Önbelleği doldurmak için performans düzeyinizi yükseltmeyi ve iş yükünüzü yeniden çalıştırmayı düşünün.

**Senaryo 3:** Önbellekle ilgisi olmayan nedenlerden dolayı sorgunuzun yavaş çalışıyor olması olasıdır. Sorgularınızı yavaşlatan diğer alanları [sorun giderin.](sql-data-warehouse-manage-monitor.md) Ayrıca, maliyetlerden tasarruf etmek için önbellek boyutunu küçültmek için [örneğinizin küçültülmesi](sql-data-warehouse-manage-monitor.md) de düşünebilirsiniz. 

**Senaryo 4:** Sorgunuzun yavaş olmasının nedeni bu olabilir soğuk bir önbelleğiniz vardı. Çalışma veri kümeniz artık önbelleğe alınmış olması gerektiği için sorgunuzu yeniden çalıştırmayı düşünün. 

> [!IMPORTANT]
> Önbellek isabet yüzdesi veya önbellek kullanılan yüzdesi iş yükünüzü yeniden çalıştırdıktan sonra güncelleştirmiyorsa, çalışma kümeniz zaten bellekte ikamet ediyor olabilir. Yalnızca kümelenmiş sütun deposu tabloları önbelleğe alınır.

## <a name="next-steps"></a>Sonraki adımlar
Genel sorgu performans hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-manage-monitor.md#monitor-query-execution)
