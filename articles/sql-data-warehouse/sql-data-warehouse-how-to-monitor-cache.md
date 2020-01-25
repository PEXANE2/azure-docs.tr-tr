---
title: Gen2 önbelleğinizi iyileştirin
description: Azure portal kullanarak Gen2 önbelleğinizi izlemeyi öğrenin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: performance
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 924705b7ce1d324583077797714491bdf3fc5cc9
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721226"
---
# <a name="how-to-monitor-the-gen2-cache"></a>Gen2 önbelleğini izleme
Gen2 depolama mimarisi, en sık sorgulanan columnstore segmentlerinizi, Gen2 veri ambarları için tasarlanan NVMe tabanlı SSD 'lerde bulunan bir önbellekte otomatik olarak katmanlar. Sorgularınızı önbellekte bulunan segmentleri aldığında daha fazla performans yapılır. Bu makalede, iş yükünüzün Gen2 önbelleğini en iyi şekilde kullanıp kullanmadığını belirleyerek yavaş sorgu performansının nasıl izleneceği ve giderileceği açıklanır.  
## <a name="troubleshoot-using-the-azure-portal"></a>Azure portal kullanarak sorun giderme
Sorgu performansının sorunlarını gidermek için Azure Izleyici 'yi Gen2 önbellek ölçümlerini görüntülemek için kullanabilirsiniz. Önce Azure portal gidin ve Izleyiciyi tıklatın:

![Azure İzleyici](./media/sql-data-warehouse-cache-portal/cache_0.png)

Ölçümler düğmesini seçin ve **abonelik**, **kaynak** **grubu**, **kaynak türü**ve veri ambarınızın **kaynak adını** girin.

Gen2 önbelleğinde sorun gidermeye yönelik temel ölçümler, **önbellek isabet yüzdesi** ve **kullanılan önbellek yüzdesidir**. Bu iki ölçümü göstermek için Azure ölçüm grafiğini yapılandırın.

![Önbellek ölçümleri](./media/sql-data-warehouse-cache-portal/cache_1.png)


## <a name="cache-hit-and-used-percentage"></a>Önbellek isabeti ve kullanılan yüzde
Aşağıdaki matris, önbellek ölçümlerinin değerlerine göre senaryolar anlatmaktadır:

|                                | **Yüksek önbellek isabet yüzdesi** | **Düşük önbellek isabet yüzdesi** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Yüksek önbellek kullanılan yüzde** |          Senaryo 1           |          Senaryo 2          |
| **Düşük önbellek kullanılan yüzde**  |          Senaryo 3           |          Senaryo 4          |

**Senaryo 1:** Önbelleğinizi en iyi şekilde kullanıyorsunuz. Sorgularınızın yavaşlatılabilen diğer alanlarla [Ilgili sorunları giderin](sql-data-warehouse-manage-monitor.md) .

**Senaryo 2:** Geçerli çalışma verileri kümesi, fiziksel okumalar nedeniyle düşük önbellek okuması yüzdesine neden olan önbelleğe sığmayacak. Performans düzeyinizi ölçeklendirmenizi ve önbelleği doldurmak için iş yükünüzü yeniden çalıştırmayı göz önünde bulundurun.

**Senaryo 3:** Önbelleğiniz, önbellekte ilgisiz olan nedenlerden dolayı yavaş çalışıyor olabilir. Sorgularınızın yavaşlatılabilen diğer alanlarla [Ilgili sorunları giderin](sql-data-warehouse-manage-monitor.md) . Ayrıca, maliyetleri kaydetmek için önbelleğin boyutunu azaltmak üzere [örneğinizi ölçeklendirmeye](sql-data-warehouse-manage-monitor.md) de göz önünde bulundurmanız gerekir. 

**Senaryo 4:** Sorgunuzun yavaşlamasına neden olabilecek bir soğuk önbelleğiniz vardı. Çalışma Veri kümeniz şimdi önbelleğe alınmış olması gerektiği için sorgunuzu yeniden çalıştırmayı deneyin. 

> [!IMPORTANT]
> İş yükünüzü yeniden çalıştırdıktan sonra önbellek isabet yüzdesi veya kullanılan önbellek yüzdesi güncelleştirilmemişse, çalışma kümesi bellekte zaten bulunabilir. Yalnızca kümelenmiş columnstore tabloları önbelleğe alınır.

## <a name="next-steps"></a>Sonraki adımlar
Genel sorgu performansı ayarlama hakkında daha fazla bilgi için bkz. [izleyici sorgu yürütme](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md#monitor-query-execution).
