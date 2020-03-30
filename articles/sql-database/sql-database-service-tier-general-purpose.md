---
title: Genel amaçlı hizmet katmanı
description: Azure SQL Veritabanı genel amaçlı katmanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/07/2019
ms.openlocfilehash: 7c57755ae63f8af5a2a4faa4764bc6a9597e8c2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255892"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>Genel amaçlı hizmet katmanı - Azure SQL Veritabanı

> [!NOTE]
> vCore tabanlı satın alma modelindeki genel amaçlı hizmet katmanı, DTU tabanlı satın alma modelinde standart hizmet katmanı olarak adlandırılır. vCore tabanlı satın alma modelinin DTU tabanlı satın alma modeliyle karşılaştırılması için [Azure SQL Veritabanı satın alma modelleri ve kaynaklarına](sql-database-purchase-models.md)bakın.

Azure SQL Veritabanı, altyapı hataları durumunda bile %99,99 kullanılabilirlik sağlamak için bulut ortamına uyarlanmış SQL Server veritabanı altyapısı mimarisine dayanır. Azure SQL Veritabanı'nda her biri farklı mimari modellere sahip üç hizmet katmanı vardır. Bu hizmet katmanları şunlardır:

- Genel amaçlı
- İş kritik
- Hiper Ölçek

Genel amaçlı hizmet katmanı için mimari model, işlem ve depolamanın ayrılmasına dayanır. Bu mimari model, veritabanı dosyalarını saydam bir şekilde çoğaltan ve altyapı hatası nın altında yatan durumda veri kaybını garanti eden Azure Blob depolamasının yüksek kullanılabilirliğine ve güvenilirliğine dayanır.

Aşağıdaki şekil, ayrılmış işlem ve depolama katmanları ile standart mimari modelde dört düğüm gösterir.

![İşlem ve depolamanın ayrılması](media/sql-database-managed-instance/general-purpose-service-tier.png)

Genel amaçlı hizmet katmanının mimari modelinde iki katman vardır:

- `sqlservr.exe` İşlemi çalıştıran ve yalnızca geçici ve önbelleğe alınmış veriler (örneğin , önbellek, arabellek havuzu, sütun deposu havuzu) içeren durum suz bir bilgi işlem katmanı. Bu durum suz SQL Server düğümü, işlemi başlatma, düğümün sistem durumunu kontrol eden ve gerekirse başka bir yerde başarısız olan Azure Service Fabric tarafından işletilmektedir.
- Azure Blob depolama alanında depolanan veritabanı dosyalarına (.mdf/.ldf) sahip özel bir veri katmanı. Azure Blob depolama, herhangi bir veritabanı dosyasına yerleştirilen herhangi bir kaydın veri kaybı olmayacağını garanti eder. Azure Depolama, SQL Server işlemi çökse bile günlük dosyasındaki veya veri dosyasındaki her kaydın korunmasını sağlayan yerleşik veri kullanılabilirliği/artıklığı vardır.

Veritabanı altyapısı veya işletim sistemi yükseltildiğinde, alt altyapının bir kısmı başarısız olduğunda veya SQL Server işleminde bazı kritik bir sorun algılanırsa, Azure Hizmet Dokusu durumsuz SQL Server işlemini başka bir durum ifadesüz bilgi işlem düğümüne taşır. Zaman içinde başarısızlığı en aza indirmek için birincil düğümün bir arıza varsa, yeni bir işlem hizmeti çalıştırmak için bekleyen yedek düğüm kümesi vardır. Azure depolama katmanındaki veriler etkilenmez ve veri/günlük dosyaları yeni başlatılan SQL Server işlemine eklenir. Bu işlem %99,99 kullanılabilirliği garanti eder, ancak geçiş süresi ve yeni SQL Server düğümünün soğuk önbellekle başlaması nedeniyle çalışan ağır iş yükü üzerinde bazı performans etkileri olabilir.

## <a name="when-to-choose-this-service-tier"></a>Bu hizmet katmanıne ne zaman seçilir

Genel Amaçlı hizmet katmanı, Azure SQL Veritabanı'nda genel iş yüklerinin çoğu için tasarlanmış varsayılan bir hizmet katmanıdır. Çoğu durumda Azure SQL IaaS ile eşleşen 5 ile 10 ms arasında depolama gecikmesi olan %99,99 SLA'ya sahip tam olarak yönetilen bir veritabanı altyapısına ihtiyacınız varsa, Genel Amaç katmanı sizin için bir seçenektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen Örnekte](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)Genel Amaç/Standart katmanın kaynak özelliklerini (çekirdek sayısı, IO, bellek) bulun, [vCore modelinde](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4) veya [DTU modelinde](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)tek veritabanı veya [vCore modelinde](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose---provisioned-compute---gen4) ve [DTU modelinde](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits)Elastik havuz.
- İş [Açısından Kritik](sql-database-service-tier-business-critical.md) ve [Hiper Ölçekli](sql-database-service-tier-hyperscale.md) katmanlar hakkında bilgi edinin.
- Servis [Kumaşı](../service-fabric/service-fabric-overview.md)hakkında bilgi edinin.
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için [İş Sürekliliği'ne](sql-database-business-continuity.md)bakın.
