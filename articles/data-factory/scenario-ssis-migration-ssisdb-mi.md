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
ms.openlocfilehash: 184cd7ec0dd490152e7234383bffe4f0fd822913
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92635720"
---
# <a name="ssis-migration-with-azure-sql-managed-instance-as-the-database-workload-destination"></a>SSIS, veritabanı iş yükü hedefi olarak Azure SQL yönetilen örneği ile geçiş

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Veritabanı iş yüklerini bir SQL Server örneğinden Azure SQL yönetilen örneği 'ne geçirirken, [Azure Data Migration hizmeti](../dms/dms-overview.md)(DMS) ve [DMS kullanarak SQL yönetilen örnek geçişleri için ağ topolojileri](../dms/resource-network-topologies.md)hakkında bilgi sahibi olmanız gerekir.

Bu makalede, SSIS kataloğunda (SSSıSDB) depolanan SQL Server Integration Service (SSIS) paketlerinin geçirilmesi ve SSIS paket yürütmelerinin zamanlaması olan SQL Server Agent işleri ele alınmaktadır.

## <a name="migrate-ssis-catalog-ssisdb"></a>SSIS kataloğunu (SSSıSDB) geçirme

Ssssıs [PAKETLERINI SQL yönetilen örneği 'Ne geçirme](../dms/how-to-migrate-ssis-packages-managed-instance.md)makalesinde açıklandığı gıbı, SSISDB geçişi DMS kullanılarak yapılabilir.

## <a name="ssis-jobs-to-sql-managed-instance-agent"></a>SSIS işleri SQL yönetilen örnek aracısına

SQL yönetilen örneği, yalnızca şirket içi SQL Server Agent gibi yerel, birinci sınıf bir Scheduler 'a sahiptir.  [SSIS paketlerini Azure SQL yönetilen örnek Aracısı aracılığıyla çalıştırabilirsiniz](how-to-invoke-ssis-package-managed-instance-agent.md).

SSIS işlerinin geçiş aracı henüz kullanılamadığından, betikler/el ile kopyalama aracılığıyla şirket içi SQL Server Agent içinden SQL yönetilen örnek aracısına geçirilmesi gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure Data Factory](./introduction.md)
- [Azure-SSIS Integration Runtime](./create-azure-ssis-integration-runtime.md)
- [Azure Veritabanı Geçiş Hizmeti](../dms/dms-overview.md)
- [DMS kullanarak SQL yönetilen örnek geçişleri için ağ topolojileri](../dms/resource-network-topologies.md)
- [SSIS paketlerini SQL yönetilen örneği 'ne geçirme](../dms/how-to-migrate-ssis-packages-managed-instance.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da SSıSDB 'ye bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure 'da dağıtılan SSIS paketlerini çalıştırma](/sql/integration-services/lift-shift/ssis-azure-run-packages)