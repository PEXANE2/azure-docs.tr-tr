---
title: Azure Veritabanı Geçiş Hizmeti araçları matrisi
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
ms.openlocfilehash: 2170612f9365ec9645b70c24236b865f106b646e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254823"
---
# <a name="services-and-tools-available-for-data-migration-scenarios"></a>Veri geçişi senaryolarında kullanılabilen hizmetler ve araçlar

Bu makalede, çeşitli veritabanı ve veri geçiş senaryoları ve özel görevler de size yardımcı olmak için kullanılabilir Microsoft ve üçüncü taraf hizmetleri ve araçları bir matris sağlar.

Aşağıdaki tablolar, veri geçişi için başarılı bir şekilde planlamak ve çeşitli aşamalarını tamamlamak için kullanabileceğiniz hizmetleri ve araçları tanımlar.

> [!NOTE]
> Aşağıdaki tablolarda yıldız işaretiyle işaretlenmiş öğeler (*) üçüncü taraf araçları temsil eder.

## <a name="business-justification-phase"></a>İş yaslama aşaması

| Kaynak | Hedef | Keşfet /<br/>Envanter | Hedef ve SKU<br/>Öneri | TCO/ROI ve<br/>İş durumu |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [MAP Araç Seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
 SQL Server | Azure SQL DB MI | [MAP Araç Seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure SQL VM | [MAP Araç Seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | SQL DW | [MAP Araç Seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize*](https://www.cloudamize.com/) |  | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS SQL | Azure SQL DB, MI, VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| Oracle | Azure SQL DB, MI, VM | [MAP Araç Seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[MigVisor*](https://www.migvisor.com/) |  |
| Oracle | SQL DW | [MAP Araç Seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | PostgreSQL için Azure DB -<br/>Tek sunucu | [MAP Araç Seti](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) |  |  |
| MongoDB | Cosmos DB | [Cloudamize*](https://www.cloudamize.com/) | [Cloudamize*](https://www.cloudamize.com/) |  |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, MI, VM | [Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/) | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| MySQL | MySQL için Azure DB | [Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS MySQL | MySQL için Azure DB |  |  | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu | [Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu |  |  | [TCO Hesaplama](https://azure.microsoft.com/pricing/tco/calculator/) |
| DB2 | Azure SQL DB, MI, VM | [Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Erişim | Azure SQL DB, MI, VM | [Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase - SAP ASE | Azure SQL DB, MI, VM | [Azure Geçiş](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase - SAP IQ | Azure SQL DB, MI, VM |  |  |  |
| | | | | |

## <a name="pre-migration-phase"></a>Geçiş öncesi aşama

| Kaynak | Hedef | Uygulama Veri Erişimi<br/>Katman Değerlendirmesi | Database<br/>Değerlendirme | Performans<br/>Değerlendirme |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [Dea](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize*](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [Dea](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize*](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [Dea](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize*](https://www.cloudamize.com/) |
| SQL Server | SQL DW | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) |  |  |
| RDS SQL | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [Dea](https://www.microsoft.com/download/details.aspx?id=54090) |
| Oracle | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | SQL DW | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | PostgreSQL için Azure DB -<br/>Tek sunucu |  | [Ora2Pg*](http://ora2pg.darold.net/start.html) |  |
| MongoDB | Cosmos DB |  | [Cloudamize*](https://www.cloudamize.com/) | [Cloudamize*](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/) |  |
| MySQL | MySQL için Azure DB |  |  |  |
| RDS MySQL | MySQL için Azure DB |  |  |  |
| PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu |  |  |  |
| RDS PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu |  |  |  |
| DB2 | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Erişim | Azure SQL DB, MI, VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase - SAP ASE | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) / [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase - SAP IQ | Azure SQL DB, MI, VM |  | |  |
| | | | | |

## <a name="migration-phase"></a>Geçiş aşaması

| Kaynak | Hedef | Şema | Veri<br/>(Çevrimdışı) | Veri<br/>(Çevrimiçi) |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize*](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL DB MI | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/)<br>[Cloudamize*](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/)<br>[Cloudamize*](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | SQL DW |  |  |  |
| RDS SQL | Azure SQL DB, MI, VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | Azure SQL DB, MI, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | SQL DW | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | PostgreSQL için Azure DB -<br/>Tek sunucu | [İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [DMS](https://azure.microsoft.com/services/database-migration/) |
| MongoDB | Cosmos DB | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/)<br/>[Imanis Veri*](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/)<br/>[Imanis Veri*](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize*](https://www.cloudamize.com/)<br/>[Imanis Veri*](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Cassandra | Cosmos DB | [Imanis Veri*](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Imanis Veri*](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Imanis Veri*](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) |
| MySQL | Azure SQL DB, MI, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| MySQL | MySQL için Azure DB | [MySQL dökümü*](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS MySQL | MySQL için Azure DB | [MySQL dökümü*](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu | [PG dökümü*](https://www.postgresql.org/docs/11/static/app-pgdump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu | [PG dökümü*](https://www.postgresql.org/docs/11/static/app-pgdump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| DB2 | Azure SQL DB, MI, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Erişim | Azure SQL DB, MI, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Sybase - SAP ASE | Azure SQL DB, MI, VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[İspirer*](https://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [Attunity*](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Sybase - SAP IQ | Azure SQL DB, MI, VM | [İspirer*](http://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | [İspirer*](http://www.ispirer.com/solutions/migration-to-the-microsoft-technology-stack) | |
| | | | | |

## <a name="post-migration-phase"></a>Göç sonrası evre

| Kaynak | Hedef | İyileştirme |
| --- | --- | --- |
| SQL Server | Azure SQL DB | [Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI | [Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM | [Bulut Atlası*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize*](https://www.cloudamize.com/) |
| SQL Server | SQL DW |  |
| RDS SQL | Azure SQL DB, MI, VM |  |
| Oracle | Azure SQL DB, MI, VM |  |
| Oracle | SQL DW |  |
| Oracle | PostgreSQL için Azure DB -<br/>Tek sunucu |  |
| MongoDB | Cosmos DB | [Cloudamize*](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |
| MySQL | Azure SQL DB, MI, VM |  |
| MySQL | MySQL için Azure DB |  |
| RDS MySQL | MySQL için Azure DB |  |
| PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu |  |
| RDS PostgreSQL | PostgreSQL için Azure DB -<br/>Tek sunucu |  |
| DB2 | Azure SQL DB, MI, VM |  |
| Erişim | Azure SQL DB, MI, VM |  |
| Sybase - SAP ASE | Azure SQL DB, MI, VM |  |
| Sybase - SAP IQ | Azure SQL DB, MI, VM |  |
| | | |

## <a name="next-steps"></a>Sonraki adımlar

Azure Veritabanı Geçiş Hizmeti'ne genel bir bakış için, [Azure Veritabanı Geçiş Hizmeti nedir makalesine](dms-overview.md)bakın.
