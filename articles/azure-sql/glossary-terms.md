---
title: Terimler sözlüğü
titleSuffix: Azure SQL
description: Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure VM 'de SQL ile çalışmaya yönelik koşulların bir sözlüğü.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=4
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/22/2020
ms.openlocfilehash: f05871c41e2deec3f6a52446844c0b8fd2c9038d
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84221394"
---
# <a name="azure-sql-database-glossary-of-terms"></a>Azure SQL veritabanı şart sözlüğü
[!INCLUDE[appliesto-asf](includes/appliesto-asf.md)]

## <a name="azure-sql-database"></a>Azure SQL Veritabanı

|Bağlam|Terim|Daha fazla bilgi|
|:---|:---|:---|
|Azure hizmeti|Azure SQL veritabanı veya SQL veritabanı|[Azure SQL Veritabanı](database/sql-database-paas-overview.md)|
|Satın alma modeli|DTU tabanlı satın alma modeli|[DTU tabanlı satın alma modeli](database/service-tiers-dtu.md)|
||Sanal çekirdek tabanlı satın alma modeli|[Sanal çekirdek tabanlı satın alma modeli](database/service-tiers-vcore.md)|
|Dağıtım seçeneği |Tek veritabanı|[Tek veritabanları](database/single-database-overview.md)|
||Elastik havuz|[Elastik havuz](database/elastic-pool-overview.md)|
|Hizmet katmanı|Temel, standart, Premium, Genel Amaçlı, hiper ölçek, İş Açısından Kritik|Sanal çekirdek modelindeki hizmet katmanları için bkz. [SQL veritabanı hizmet katmanları](database/service-tiers-vcore.md#service-tiers). DTU modelindeki hizmet katmanları için bkz. [DTU modeli](database/service-tiers-dtu.md#compare-the-dtu-based-service-tiers).|
|İşlem Katmanı|Sunucusuz işlem|[Sunucusuz işlem](database/service-tiers-vcore.md#compute-tiers)
||Sağlanan işlem|[Sağlanan işlem](database/service-tiers-vcore.md#compute-tiers)
|İşlem oluşturma|5. nesil, d serisi, Fsv2 serisi|[Donanım nesilleri](database/service-tiers-vcore.md#hardware-generations)
|Sunucu varlığı| Sunucu |[Mantıksal SQL sunucuları](database/logical-servers.md)|
|Kaynak türü|Sanal Çekirdek|Tek bir veritabanı ve elastik havuz için işlem kaynağına sunulan bir CPU çekirdeği. |
||İşlem boyutu ve depolama miktarı|İşlem boyutu, tek bir veritabanı veya elastik havuz için kullanılabilen en fazla CPU, bellek ve depolama birimi olmayan diğer kaynak miktarıdır.  Depolama boyutu, tek bir veritabanı veya elastik havuz için kullanılabilir maksimum depolama miktarıdır. Sanal çekirdek modelindeki boyutlandırma seçenekleri için bkz. [sanal çekirdek tek veritabanları](database/resource-limits-vcore-single-databases.md)ve [sanal çekirdek elastik havuzlar](database/resource-limits-vcore-elastic-pools.md).  (.. /Managed-Instance/Resource-Limits.MD).  DTU modelindeki boyutlandırma seçenekleri için bkz. [DTU tek veritabanları](database/resource-limits-dtu-single-databases.md) ve [DTU elastik havuzları](database/resource-limits-dtu-elastic-pools.md).

## <a name="azure-sql-managed-instance"></a>Azure SQL Yönetilen Örnek

|Bağlam|Terim|Daha fazla bilgi|
|:---|:---|:---|
|Azure hizmeti|Azure SQL Yönetilen Örnek|[SQL Yönetilen Örnek](managed-instance/sql-managed-instance-paas-overview.md)|
|Satın alma modeli|Sanal çekirdek tabanlı satın alma modeli|[Sanal çekirdek tabanlı satın alma modeli](database/service-tiers-vcore.md)|
|Dağıtım seçeneği |Tek Örnek|[Tek Örnek](managed-instance/sql-managed-instance-paas-overview.md)|
||Örnek Havuzu (Önizleme)|[Örnek havuzları](managed-instance/instance-pools-overview.md)|
|Hizmet katmanı|Genel Amaçlı, İş Açısından Kritik|[SQL yönetilen örnek hizmeti katmanları](managed-instance/sql-managed-instance-paas-overview.md#service-tiers)|
|İşlem Katmanı|Sağlanan işlem|[Sağlanan işlem](database/service-tiers-vcore.md#compute-tiers)|
|İşlem oluşturma|5. nesil|[Donanım nesilleri](database/service-tiers-vcore.md#hardware-generations)
|Sunucu varlığı|Yönetilen örnek veya örnek| SQL yönetilen örneği sunucuda olan N/A |
|Kaynak türü|Sanal Çekirdek|SQL yönetilen örneği için işlem kaynağına sunulan bir CPU çekirdeği.|
||İşlem boyutu ve depolama miktarı|İşlem boyutu, SQL yönetilen örneği için en yüksek CPU, bellek ve depolama olmayan diğer kaynaklarla ilgilidir.  Depolama boyutu, bir SQL yönetilen örneği için kullanılabilir maksimum depolama miktarıdır.  Boyutlandırma seçenekleri için, [SQL tarafından yönetilen örnekler](managed-instance/resource-limits.md). |

## <a name="sql-on-azure-vm"></a>Azure VM üzerinde SQL

Burada daha fazla bilgi gerekiyor
