---
title: SSIS, veritabanı iş yükü hedefi olarak Azure SQL veritabanı yönetilen örneği ile geçiş
description: SSIS, veritabanı iş yükü hedefi olarak Azure SQL veritabanı yönetilen örneği ile geçiş.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 9/12/2019
ms.openlocfilehash: 2e35e4eb750aa2244df920111b201d886599eaf6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419060"
---
# <a name="ssis-migration-with-azure-sql-database-managed-instance-as-the-database-workload-destination"></a>SSIS, veritabanı iş yükü hedefi olarak Azure SQL veritabanı yönetilen örneği ile geçiş

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Şirket içindeki SQL Server veritabanı iş yüklerini Azure SQL veritabanı yönetilen örneği 'ne geçirirken, [Azure Data Migration hizmeti](https://docs.microsoft.com/azure/dms/dms-overview)(DMS) ve [DMS kullanarak Azure SQL veritabanı yönetilen örnek geçişleri için ağ topolojileri](https://docs.microsoft.com/azure/dms/resource-network-topologies)hakkında bilgi sahibi olmanız gerekir.

Bu makalede, SSIS kataloğunda (SSSıSDB) depolanan SQL Server Integration Service (SSIS) paketlerinin geçirilmesi ve SSIS paket yürütmelerinin zamanlaması olan SQL Server Agent işleri ele alınmaktadır.

## <a name="migrate-ssis-catalog-ssisdb"></a>SSIS kataloğunu (SSSıSDB) geçirme

Ssssıs [paketlerini Azure SQL veritabanı yönetilen örneği 'Ne geçirme](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)makalesinde açıklandığı gıbı, SSıSDB geçişi DMS kullanılarak yapılabilir.

## <a name="ssis-jobs-to-azure-sql-database-managed-instance-agent"></a>SSIS işleri Azure SQL veritabanı yönetilen örnek aracısına

Azure SQL veritabanı yönetilen örneği, yalnızca şirket içi SQL Server Agent gibi yerel, birinci sınıf bir Scheduler 'a sahiptir.  SSIS işlerinin geçiş aracı henüz kullanılamadığından, Azure SQL veritabanı yönetilen örnek aracısına betikler/el ile kopyalama aracılığıyla SQL Server Agent geçirilmesi gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)
- [Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/dms-overview)
- [DMS kullanarak Azure SQL veritabanı yönetilen örnek geçişleri için ağ topolojileri](https://docs.microsoft.com/azure/dms/resource-network-topologies)
- [SSIS paketlerini Azure SQL Veritabanı yönetilen örneğine geçirme](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da SSıSDB 'ye bağlanma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure 'da dağıtılan SSIS paketlerini çalıştırma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
