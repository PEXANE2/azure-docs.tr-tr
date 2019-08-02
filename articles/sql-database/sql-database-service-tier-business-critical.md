---
title: İş Açısından Kritik katmanı-Azure SQL veritabanı hizmeti | Microsoft Docs
description: Azure SQL veritabanı İş Açısından Kritik katmanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: 208224e10c3acfb17bc5fd89d2d66152943811dc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566752"
---
# <a name="business-critical-tier---azure-sql-database"></a>İş Açısından Kritik katmanı-Azure SQL veritabanı

> [!NOTE]
> İş Açısından Kritik katmana, DTU satın alma modelinde Premium adı verilir. DTU tabanlı satın alma modeliyle sanal çekirdek tabanlı satın alma modeli karşılaştırması için bkz. [Azure SQL veritabanı satın alma modelleri ve kaynakları](sql-database-purchase-models.md).

Azure SQL veritabanı, altyapı hatalarının durumlarında bile% 99,99 kullanılabilirlik sağlamak için bulut ortamı için ayarlanmış SQL Server veritabanı motoru mimarisini temel alır. Azure SQL veritabanı 'nda kullanılan üç mimari modeli vardır:
- Genel Amaçlı/standart 
- İş Açısından Kritik/Premium
- Hiper ölçek

Premium/İş Açısından Kritik hizmet katmanı modeli, bir veritabanı altyapısı işlemlerinin kümesini temel alır. Bu mimari model, kullanılabilir veritabanı altyapısı düğümlerinin her zaman bir çekirdeği olduğunu ve bakım etkinlikleri sırasında bile iş yükünüzde en az performans etkisi olduğunu bir olgusuna bağımlıdır.

Azure, son kullanıcılar için en düşük düzeyde çalışan işletim sistemi, sürücü ve SQL Server veritabanı altyapısını şeffaf bir şekilde yükseltir ve düzeltme eklerini ekler. 

Premium kullanılabilirlik, Azure SQL veritabanı 'nın Premium ve İş Açısından Kritik hizmet katmanlarında etkinleştirilmiştir ve devam eden bakım işlemleri nedeniyle herhangi bir performans etkisi için işlem yapmadan yoğun iş yükleri için tasarlanmıştır.

Premium modelde, Azure SQL veritabanı, işlem ve depolamayı tek düğümde tümleştirir. Bu mimari modelde yüksek kullanılabilirlik, [her zaman açık kullanılabilirlik grupları SQL Server benzer teknolojiyi kullanarak, işlem (SQL Server veritabanı altyapısı işlemi) ve depolama (yerel olarak bağlı SSD) ile dört düğüm kümesine dağıtılan depolama (yerel olarak eklenmiş SSD) tarafından sağlanır ](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server).

![Veritabanı altyapısı düğümlerinin kümesi](media/sql-database-managed-instance/business-critical-service-tier.png)

Hem SQL veritabanı altyapısı işlemi hem de temel alınan MDF/ldf dosyaları, iş yükünüze düşük gecikme süresi sağlayan yerel olarak eklenmiş SSD depolama ile aynı düğüme yerleştirilir. Yüksek kullanılabilirlik, [her zaman açık kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)SQL Server benzer teknolojiler kullanılarak uygulanır. Her veritabanı, müşteri iş yükü için erişilebilen tek bir birincil veritabanına sahip bir veritabanı düğümleri kümesi ve verilerin kopyalarını içeren üç ikincil işlem. Birincil düğüm, birincil düğüm herhangi bir nedenden dolayı çöktüğünde, verilerin ikincil çoğaltmalarda kullanılabilir olmasını sağlamak için değişiklikleri ikincil düğümlere sürekli olarak iter. Yük devretme SQL Server veritabanı altyapısı tarafından işlenir – bir ikincil çoğaltma birincil düğüm haline gelir ve kümede yeterli düğüm sağlamak için yeni bir ikincil çoğaltma oluşturulur. İş yükü otomatik olarak yeni birincil düğüme yönlendirilir.

Ayrıca, İş Açısından Kritik kümede, birincil ağınızın performansını etkilememesi gereken salt okuma sorgularını (örneğin, raporlar) çalıştırmak için kullanılabilen, ücretsiz olarak yerleşik salt okunurdur bir düğüm sağlayan yerleşik bir yerleşik [okuma](sql-database-read-scale-out.md) özelliği vardır. yüküne.

## <a name="when-to-choose-this-service-tier"></a>Bu hizmet katmanını ne zaman seçmelisiniz?

İş Açısından Kritik hizmet katmanı, temeldeki SSD depolamadan (ortalama 1-2 ms) düşük Gecikmeli yanıt gerektiren uygulamalar için tasarlanmıştır, temel alınan altyapı başarısız olursa hızlı kurtarma ve rapor, analiz ve salt okunurdur. birincil veritabanının ücretsiz okunabilir ikincil çoğaltmasını ücretsiz olarak sorgular.

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen örnekteki](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)iş açısından kritik katmanının kaynak özelliklerini (çekirdek sayısı, GÇ, bellek), [Vcore modeli](sql-database-vcore-resource-limits-single-databases.md#business-critical-service-tier-for-provisioned-compute-tier) veya [DTU modelinde](sql-database-dtu-resource-limits-single-databases.md#premium-service-tier)tek veritabanı ya da [Vcore modeli](sql-database-vcore-resource-limits-elastic-pools.md#business-critical-service-tier-storage-sizes-and-compute-sizes) ve [DTU modelinde](sql-database-dtu-resource-limits-elastic-pools.md#premium-elastic-pool-limits)esnek havuz bulun.
- [Genel amaçlı](sql-database-service-tier-general-purpose.md) ve [hiper ölçek](sql-database-service-tier-hyperscale.md) katmanları hakkında bilgi edinin.
- [Service Fabric](../service-fabric/service-fabric-overview.md)hakkında bilgi edinin.
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için bkz. [Iş sürekliliği](sql-database-business-continuity.md).
