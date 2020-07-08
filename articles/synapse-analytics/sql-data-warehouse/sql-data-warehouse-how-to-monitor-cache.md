---
title: Gen2 önbelleğinizi iyileştirin
description: Azure portal kullanarak Gen2 önbelleğinizi izlemeyi öğrenin.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 4d66a1174b1b4adc94b24c6aecd55b2b8679f2f7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85211893"
---
# <a name="how-to-monitor-the-gen2-cache"></a>Gen2 önbelleğini izleme

Bu makalede, iş yükünüzün Gen2 önbelleğini en iyi şekilde kullanıp kullanmadığını belirleyerek yavaş sorgu performansının nasıl izleneceği ve giderileceği açıklanır.

Gen2 depolama mimarisi, en sık sorgulanan columnstore segmentlerinizi, Gen2 veri ambarları için tasarlanan NVMe tabanlı SSD 'lerde bulunan bir önbellekte otomatik olarak katmanlar. Sorgularınızı önbellekte bulunan segmentleri aldığında daha fazla performans yapılır.
 
## <a name="troubleshoot-using-the-azure-portal"></a>Azure portal kullanarak sorun giderme

Sorgu performansının sorunlarını gidermek için Azure Izleyici 'yi Gen2 önbellek ölçümlerini görüntülemek için kullanabilirsiniz. Önce Azure portal gidin ve **izleyici**, **ölçümler** ve **+ bir kapsam seçin**' e tıklayın:

![Azure İzleyici](./media/sql-data-warehouse-how-to-monitor-cache/cache-0.png)

Veri Ambarınızı bulmak için arama ve aşağı açılan çubukları kullanın. Ardından Uygula ' yı seçin.

![Azure İzleyici](./media/sql-data-warehouse-how-to-monitor-cache/cache-1.png)

Gen2 önbelleğinde sorun gidermeye yönelik temel ölçümler, **önbellek isabet yüzdesi** ve **kullanılan önbellek yüzdesidir**. **Önbellek isabet yüzdesi** ' ni seçin ve ardından **kullanılan önbellek yüzdesini**eklemek için **ölçüm Ekle** düğmesini kullanın. 

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
