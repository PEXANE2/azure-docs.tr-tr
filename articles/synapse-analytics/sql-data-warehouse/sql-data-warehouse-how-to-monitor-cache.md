---
title: Gen2 önbelleğinizi iyileştirin
description: Azure portal kullanarak Gen2 önbelleğinizi izlemeyi öğrenin.
services: synapse-analytics
author: gaursa
manager: craigg
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: gaursa
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: fed3ed2c87342d557872e97bfc2a6c4d142b5a3a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104585630"
---
# <a name="how-to-monitor-the-adaptive-cache"></a>Uyarlamalı önbelleği izleme

Bu makalede, iş yükünüzün adanmış SQL havuzları için uyarlamalı önbellekten en iyi şekilde yararlanıp yararlanmadığını belirleyerek yavaş sorgu performansının nasıl izleneceği ve giderileceği açıklanmaktadır.

Adanmış SQL havuzu depolama mimarisi, en sık sorgulanan columnstore segmentlerinizi NVMe tabanlı SSD 'lerde bulunan bir önbellekte otomatik olarak katmanlar. Sorgularınız önbellekte bulunan segmentleri alırken daha fazla performansa sahip olursunuz.
 
## <a name="troubleshoot-using-the-azure-portal"></a>Azure portal kullanarak sorun giderme

Sorgu performansının sorunlarını gidermek için önbellek ölçümlerini görüntülemek üzere Azure Izleyici 'yi kullanabilirsiniz. Önce Azure portal gidin ve **izleyici**, **ölçümler** ve **+ bir kapsam seçin**' e tıklayın:

![Ekran görüntüsü, Azure portal ölçümlerden seçilen bir kapsamı seçin.](./media/sql-data-warehouse-how-to-monitor-cache/cache-0.png)

Adanmış SQL havuzunuzu bulmak için arama ve aşağı açılan çubuklarını kullanın. Ardından Uygula ' yı seçin.

![Ekran görüntüsü, veri Ambarınızı seçebileceğiniz bir kapsam seçin bölmesi gösterir.](./media/sql-data-warehouse-how-to-monitor-cache/cache-1.png)

Önbellekte sorun gidermeye yönelik temel ölçümler **önbellek isabet yüzdesi** ve **kullanılan önbellek yüzdesidir**. **Önbellek isabet yüzdesi** ' ni seçin ve ardından **kullanılan önbellek yüzdesini** eklemek için **ölçüm Ekle** düğmesini kullanın. 

![Önbellek ölçümleri](./media/sql-data-warehouse-how-to-monitor-cache/cache-2.png)

## <a name="cache-hit-and-used-percentage"></a>Önbellek isabeti ve kullanılan yüzde

Aşağıdaki matris, önbellek ölçümlerinin değerlerine göre senaryolar anlatmaktadır:

|                                | **Yüksek önbellek isabet yüzdesi** | **Düşük önbellek isabet yüzdesi** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Yüksek önbellek kullanılan yüzde** |          1\. Senaryo           |          2\. Senaryo          |
| **Düşük önbellek kullanılan yüzde**  |          3\. Senaryo           |          4\. Senaryo          |

**Senaryo 1:** Önbelleğinizi en iyi şekilde kullanıyorsunuz. Sorgularınızın yavaşlatılabilen diğer alanlarla [Ilgili sorunları giderin](sql-data-warehouse-manage-monitor.md) .

**Senaryo 2:** Geçerli çalışma verileri kümesi, fiziksel okumalar nedeniyle düşük önbellek okuması yüzdesine neden olan önbelleğe sığmayacak. Performans düzeyinizi ölçeklendirmenizi ve önbelleği doldurmak için iş yükünüzü yeniden çalıştırmayı göz önünde bulundurun.

**Senaryo 3:** Önbelleğiniz, önbellekte ilgisiz olan nedenlerden dolayı yavaş çalışıyor olabilir. Sorgularınızın yavaşlatılabilen diğer alanlarla [Ilgili sorunları giderin](sql-data-warehouse-manage-monitor.md) . Ayrıca, maliyetleri kaydetmek için önbelleğin boyutunu azaltmak üzere [örneğinizi ölçeklendirmeye](sql-data-warehouse-manage-monitor.md) de göz önünde bulundurmanız gerekir. 

**Senaryo 4:** Sorgunuzun yavaşlamasına neden olabilecek bir soğuk önbelleğiniz vardı. Çalışma Veri kümeniz şimdi önbelleğe alınmış olması gerektiği için sorgunuzu yeniden çalıştırmayı deneyin. 

> [!IMPORTANT]
> İş yükünüzü yeniden çalıştırdıktan sonra önbellek isabet yüzdesi veya kullanılan önbellek yüzdesi güncelleştirilmemişse, çalışma kümesi bellekte zaten bulunabilir. Yalnızca kümelenmiş columnstore tabloları önbelleğe alınır.

## <a name="next-steps"></a>Sonraki adımlar
Genel sorgu performansı ayarlama hakkında daha fazla bilgi için bkz. [izleyici sorgu yürütme](sql-data-warehouse-manage-monitor.md#monitor-query-execution).
