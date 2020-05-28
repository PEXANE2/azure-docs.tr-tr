---
title: Genel amaçlı hizmet katmanı
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği için genel amaçlı hizmet katmanı hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/07/2019
ms.openlocfilehash: 7fd446e6894c3c88701527bb2d3668a570888a06
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84049914"
---
# <a name="general-purpose-service-tier---azure-sql-database--sql-managed-instance"></a>Genel amaçlı hizmet katmanı-SQL yönetilen örnek & Azure SQL veritabanı
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

> [!NOTE]
> Sanal çekirdek tabanlı satın alma modelindeki genel amaçlı hizmet katmanına, DTU tabanlı satın alma modelinde standart hizmet katmanı adı verilir. DTU tabanlı satın alma modeliyle sanal çekirdek tabanlı satın alma modeli karşılaştırması için bkz. [model ve kaynak satın alma](purchasing-models.md).

Azure SQL veritabanı ve Azure SQL yönetilen örneği, altyapı hatalarının durumlarında bile% 99,99 kullanılabilirlik sağlamak için bulut ortamı için SQL Server veritabanı altyapısı mimarisi uyarlanmasını temel alır. 

Azure SQL veritabanı ve SQL yönetilen örneği tarafından kullanılan iki hizmet katmanı vardır: 

- Genel amaçlı
- İş açısından kritik

Azure SQL veritabanı, Azure SQL yönetilen örneği için şu anda kullanılamayan üçüncü bir hizmet katmanına de sahiptir: 

- Hiper Ölçek

Genel amaçlı hizmet katmanının mimari modeli, işlem ve depolamanın bir ayrımını temel alır. Bu mimari model, veritabanı dosyalarını saydam bir şekilde çoğaltan ve temel alınan altyapı arızası durumunda veri kaybı garanti eden Azure Blob Storage 'ın yüksek kullanılabilirliğe ve güvenilirliğini kullanır.

Aşağıdaki şekilde, standart mimari modelde, ayrılmış işlem ve Depolama katmanlarına sahip dört düğüm gösterilmektedir.

![İşlem ve depolama ayrımı](./media/service-tier-general-purpose/general-purpose-service-tier.png)

Genel amaçlı hizmet katmanının mimari modelinde iki katman vardır:

- `sqlservr.exe`İşlemi çalıştıran ve yalnızca geçici ve önbelleğe alınmış verileri içeren, durum bilgisi olmayan bir işlem katmanı (örneğin, plan önbelleği, arabellek havuzu, sütun depolama havuzu). Bu durum bilgisiz düğüm, işlem başlatan Azure Service Fabric tarafından işletilebilir, düğümün sistem durumunu denetler ve gerekirse başka bir yere yük devretme gerçekleştirir.
- Azure Blob depolamada depolanan veritabanı dosyaları (. mdf/. ldf) ile durum bilgisi olan bir veri katmanı. Azure Blob depolama, herhangi bir veritabanı dosyasına yerleştirilmiş herhangi bir kaydın veri kaybı olmasını garanti eder. Azure depolama 'nın, işlem kilitlense bile, veri dosyasındaki her kaydın günlük dosyasında veya sayfasında korunmasını sağlayan yerleşik veri kullanılabilirliği/artıklığı vardır.

Veritabanı altyapısı veya işletim sistemi yükseltildiğinde, temeldeki altyapının bir bölümü başarısız olur veya işlemde bir kritik sorun algılanırsa `sqlservr.exe` Azure Service Fabric, durum bilgisi olmayan işlemi başka bir durum bilgisiz işlem düğümüne taşır. Yük devretme süresini en aza indirmek için birincil düğümün yük devretmesi olursa yeni bir işlem hizmeti çalıştırmak için bekleyen bir yedek düğüm kümesi vardır. Azure depolama katmanındaki veriler etkilenmez ve veri/günlük dosyaları yeni başlatılmış işleme eklenir. Bu işlem% 99,99 kullanılabilirliği garanti eder, ancak geçiş süresi nedeniyle çalışmakta olan ağır iş yükleri üzerinde bazı performans etkileri olabilir ve yeni düğüm soğuk önbellek ile başlar.

## <a name="when-to-choose-this-service-tier"></a>Bu hizmet katmanını seçme

Genel Amaçlı hizmet katmanı, Azure SQL veritabanı 'nda varsayılan bir hizmet katmanı ve genel iş yükleri için tasarlanan Azure SQL yönetilen örneği. % 99,99 SLA ile tam olarak yönetilen bir veritabanı altyapısına ihtiyacınız varsa, bir Azure VM üzerinde SQL Server ile eşleşen 5 ve 10 MS arasında depolama gecikmesi olması durumunda Genel Amaçlı katmanı sizin için seçenektir.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL yönetilen örneği](../managed-instance/resource-limits.md#service-tier-characteristics)'nde genel amaçlı/standart katmanının kaynak özelliklerini (çekirdek sayısı, GÇ, bellek), [Vcore modeli](resource-limits-vcore-single-databases.md#general-purpose---provisioned-compute---gen4) veya [DTU modelinde](resource-limits-dtu-single-databases.md#single-database-storage-sizes-and-compute-sizes)tek veritabanı ya da [Vcore modeli](resource-limits-vcore-elastic-pools.md#general-purpose---provisioned-compute---gen4) ve [DTU modelinde](resource-limits-dtu-elastic-pools.md#standard-elastic-pool-limits)esnek havuz bulun.
- [İş açısından kritik](service-tier-business-critical.md) ve [hiper ölçek](service-tier-hyperscale.md) katmanları hakkında bilgi edinin.
- [Service Fabric](../../service-fabric/service-fabric-overview.md)hakkında bilgi edinin.
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için bkz. [Iş sürekliliği](business-continuity-high-availability-disaster-recover-hadr-overview.md).
