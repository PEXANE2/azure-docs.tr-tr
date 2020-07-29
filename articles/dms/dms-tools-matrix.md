---
title: Azure veritabanı geçiş hizmeti araçları matrisi
description: Veritabanlarını geçirmek ve geçiş sürecinin çeşitli aşamalarını desteklemek için kullanılabilen hizmetler ve araçlar hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/03/2020
ms.openlocfilehash: de1e00d92661c3768efa49e5809751bf4f2327d8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84267877"
---
# <a name="services-and-tools-available-for-data-migration-scenarios"></a>Veri geçişi senaryolarında kullanılabilen hizmetler ve araçlar

Bu makalede çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerde size yardımcı olmak için kullanabileceğiniz Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi sunulmaktadır.

Aşağıdaki tablolar, veri geçişini başarıyla planlamak ve çeşitli aşamaları gerçekleştirmek için kullanabileceğiniz Hizmetleri ve araçları belirler.

> [!NOTE]
> Aşağıdaki tablolarda, yıldız işareti (*) ile işaretlenen öğeler üçüncü taraf araçları temsil eder.

## <a name="business-justification-phase"></a>İş gerekçe aşaması

| Kaynak | Hedef | Bulmak<br/>Sayım | Hedef ve SKU<br/>Önerilen | TCO/ROı ve<br/>İş örneği |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [HARITA araç seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
 SQL Server | Azure SQL DB MI | [HARITA araç seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure SQL VM | [HARITA araç seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | SQL DW | [HARITA araç seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) |  | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS SQL | Azure SQL DB, mı, VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| Oracle | Azure SQL DB, mı, VM | [HARITA araç seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Migvizörü *](https://www.migvisor.com/) |  |
| Oracle | SQL DW | [HARITA araç seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | PostgreSQL için Azure DB-<br/>Tek sunucu | [HARITA araç seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) |  |  |
| MongoDB | Cosmos DB | [Cloudamize](https://www.cloudamize.com/) | [Cloudamize](https://www.cloudamize.com/) |  |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, mı, VM | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/) | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| MySQL | MySQL için Azure DB | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS MySQL | MySQL için Azure DB |  |  | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu |  |  | [TCO Hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) |
| DB2 | Azure SQL DB, mı, VM | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Access | Azure SQL DB, mı, VM | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP ATıCı | Azure SQL DB, mı, VM | [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP IQ | Azure SQL DB, mı, VM |  |  |  |
| | | | | |

## <a name="pre-migration-phase"></a>Geçiş öncesi aşaması

| Kaynak | Hedef | Uygulama verileri erişimi<br/>Katman değerlendirmesi | Veritabanı<br/>Değerlendirme | Performans<br/>Değerlendirme |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | SQL DW | [DAVMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) |  |  |
| RDS SQL | Azure SQL DB, mı, VM | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090) |
| Oracle | Azure SQL DB, mı, VM | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) dili | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | SQL DW | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) dili | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | PostgreSQL için Azure DB-<br/>Tek sunucu |  | [Ora2Pg*](http://ora2pg.darold.net/start.html) |  |
| MongoDB | Cosmos DB |  | [Cloudamize](https://www.cloudamize.com/) | [Cloudamize](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, mı, VM | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) dili | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/) |  |
| MySQL | MySQL için Azure DB |  |  |  |
| RDS MySQL | MySQL için Azure DB |  |  |  |
| PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu |  |  |  |
| RDS PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu |  |  |  |
| DB2 | Azure SQL DB, mı, VM | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) dili | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Access | Azure SQL DB, mı, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP ATıCı | Azure SQL DB, mı, VM | [Davmt](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) dili | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP IQ | Azure SQL DB, mı, VM |  | |  |
| | | | | |

## <a name="migration-phase"></a>Geçiş aşaması

| Kaynak | Hedef | Şema | Veriler<br/>Çevrimdışı | Veriler<br/>Çevrimiç |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL DB MI | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/)<br>[Cloudamize](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/)<br>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | SQL DW |  |  |  |
| RDS SQL | Azure SQL DB, mı, VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex *](https://www.quest.com/products/shareplex/)<br/>[Ispirer](https://www.ispirer.com/solutions) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex *](https://www.quest.com/products/shareplex/)<br/>[Ispirer](https://www.ispirer.com/solutions) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex *](https://www.quest.com/products/shareplex/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | SQL DW | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex *](https://www.quest.com/products/shareplex/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | PostgreSQL için Azure DB-<br/>Tek sunucu | [Ispirer](https://www.ispirer.com/solutions) | [Ispirer](https://www.ispirer.com/solutions) | [DMS](https://azure.microsoft.com/services/database-migration/) |
| MongoDB | Cosmos DB | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[İmansıs verileri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[İmansıs verileri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[İmansıs verileri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Cassandra | Cosmos DB | [İmansıs verileri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [İmansıs verileri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [İmansıs verileri *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) |
| MySQL | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| MySQL | MySQL için Azure DB | [MySQL dökümü *](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS MySQL | MySQL için Azure DB | [MySQL dökümü *](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu | [SAYFA dökümü *](https://www.postgresql.org/docs/11/static/app-pgdump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu | [SAYFA dökümü *](https://www.postgresql.org/docs/11/static/app-pgdump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| DB2 | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Access | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Sybase-SAP ATıCı | Azure SQL DB, mı, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer](https://www.ispirer.com/solutions) | [Attunity](https://www.attunity.com/products/replicate/)<br/>[Anlık ileti *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Sybase-SAP IQ | Azure SQL DB, mı, VM | [Ispirer](https://www.ispirer.com/solutions) | [Ispirer](https://www.ispirer.com/solutions) | |
| | | | | |

## <a name="post-migration-phase"></a>Geçiş sonrası aşaması

| Kaynak | Hedef | İyileştirme |
| --- | --- | --- |
| SQL Server | Azure SQL DB | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM | [Bulut Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | SQL DW |  |
| RDS SQL | Azure SQL DB, mı, VM |  |
| Oracle | Azure SQL DB, mı, VM |  |
| Oracle | SQL DW |  |
| Oracle | PostgreSQL için Azure DB-<br/>Tek sunucu |  |
| MongoDB | Cosmos DB | [Cloudamize](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |
| MySQL | Azure SQL DB, mı, VM |  |
| MySQL | MySQL için Azure DB |  |
| RDS MySQL | MySQL için Azure DB |  |
| PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu |  |
| RDS PostgreSQL | PostgreSQL için Azure DB-<br/>Tek sunucu |  |
| DB2 | Azure SQL DB, mı, VM |  |
| Access | Azure SQL DB, mı, VM |  |
| Sybase-SAP ATıCı | Azure SQL DB, mı, VM |  |
| Sybase-SAP IQ | Azure SQL DB, mı, VM |  |
| | | |

## <a name="next-steps"></a>Sonraki adımlar

Azure veritabanı geçiş hizmeti 'ne genel bakış için [Azure veritabanı geçiş hizmeti nedir](dms-overview.md)makalesine bakın.
