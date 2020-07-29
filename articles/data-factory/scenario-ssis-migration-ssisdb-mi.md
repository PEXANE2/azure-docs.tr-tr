---
title: SSIS, veritabanı iş yükü hedefi olarak Azure SQL yönetilen örneği ile geçiş
description: SSIS, veritabanı iş yükü hedefi olarak Azure SQL yönetilen örneği ile geçiş.
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
ms.openlocfilehash: c91731d70d5db59ecf5daa726fd42ee42c58f51e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84116305"
---
# <a name="ssis-migration-with-azure-sql-managed-instance-as-the-database-workload-destination"></a>SSIS, veritabanı iş yükü hedefi olarak Azure SQL yönetilen örneği ile geçiş

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Veritabanı iş yüklerini bir SQL Server örneğinden Azure SQL yönetilen örneği 'ne geçirirken, [Azure Data Migration hizmeti](https://docs.microsoft.com/azure/dms/dms-overview)(DMS) ve [DMS kullanarak SQL yönetilen örnek geçişleri için ağ topolojileri](https://docs.microsoft.com/azure/dms/resource-network-topologies)hakkında bilgi sahibi olmanız gerekir.

Bu makalede, SSIS kataloğunda (SSSıSDB) depolanan SQL Server Integration Service (SSIS) paketlerinin geçirilmesi ve SSIS paket yürütmelerinin zamanlaması olan SQL Server Agent işleri ele alınmaktadır.

## <a name="migrate-ssis-catalog-ssisdb"></a>SSIS kataloğunu (SSSıSDB) geçirme

Ssssıs [PAKETLERINI SQL yönetilen örneği 'Ne geçirme](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)makalesinde açıklandığı gıbı, SSISDB geçişi DMS kullanılarak yapılabilir.

## <a name="ssis-jobs-to-sql-managed-instance-agent"></a>SSIS işleri SQL yönetilen örnek aracısına

SQL yönetilen örneği, yalnızca şirket içi SQL Server Agent gibi yerel, birinci sınıf bir Scheduler 'a sahiptir.  SSIS işlerinin geçiş aracı henüz kullanılamadığından, betikler/el ile kopyalama aracılığıyla şirket içi SQL Server Agent içinden SQL yönetilen örnek aracısına geçirilmesi gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)
- [Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/dms-overview)
- [DMS kullanarak SQL yönetilen örnek geçişleri için ağ topolojileri](https://docs.microsoft.com/azure/dms/resource-network-topologies)
- [SSIS paketlerini SQL yönetilen örneği 'ne geçirme](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da SSıSDB 'ye bağlanma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure 'da dağıtılan SSIS paketlerini çalıştırma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
