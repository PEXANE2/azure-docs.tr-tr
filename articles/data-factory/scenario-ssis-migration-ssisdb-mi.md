---
title: Azure SQL Veritabanı ile SSIS geçişi veritabanı iş yükü hedefi olarak örnek yönetilen
description: Azure SQL Veritabanı ile SSIS geçişi, veritabanı iş yükü hedefi olarak örnek yönetildi.
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
ms.openlocfilehash: 38010e3aaa2d0544dfbfe19135d25250d2b021a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74929774"
---
# <a name="ssis-migration-with-azure-sql-database-managed-instance-as-the-database-workload-destination"></a>Azure SQL Veritabanı ile SSIS geçişi veritabanı iş yükü hedefi olarak örnek yönetilen

Veritabanı iş yüklerini SQL Server'dan azure SQL Veritabanı yönetilen örneğe aktarırken, [Azure Veri Geçiş Hizmeti'ni](https://docs.microsoft.com/azure/dms/dms-overview)(DMS) ve Azure SQL Veritabanı [ağ topolojilerini DMS kullanarak örnek geçişleri yönetmiş](https://docs.microsoft.com/azure/dms/resource-network-topologies)olmalısınız.

Bu makalede, SSIS kataloğunda (SSISDB) depolanan SQL Server Integration Service (SSIS) paketlerinin geçişi ve SSIS paket yürütmelerini zamanlayan SQL Server Agent işleri üzerinde duruluyor.

## <a name="migrate-ssis-catalog-ssisdb"></a>Göçmen SSIS kataloğu (SSISDB)

SSISDB geçişi, makalede açıklandığı gibi DMS kullanılarak yapılabilir: [SSIS paketlerini Azure SQL Veritabanı yönetilen bir örneğe geçirin.](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="ssis-jobs-to-azure-sql-database-managed-instance-agent"></a>Azure SQL Veritabanı yönetilen örnek aracısına SSIS işleri

Azure SQL Veritabanı yönetilen örnek, sql server agent gibi bir yerel, birinci sınıf zamanlayıcı vardır.  SSIS işleri için bir geçiş aracı henüz kullanılamadığından, sql server agent'dan komut dosyaları/manuel kopya yoluyla azure SQL Veritabanı yönetilen örnek agent'a geçirilmeleri gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Azure-SSIS Tümleştirme Çalışma Süresi](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)
- [Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/dms-overview)
- [Azure SQL Veritabanı için ağ topolojileri DMS kullanarak örnek geçişleri yönetti](https://docs.microsoft.com/azure/dms/resource-network-topologies)
- [SSIS paketlerini Azure SQL Veritabanı yönetilen örneğine geçirme](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da SSISDB'ye bağlanın](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure'da dağıtılan SSIS paketlerini çalıştırma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
