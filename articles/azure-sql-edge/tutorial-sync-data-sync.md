---
title: SQL Data Sync kullanarak Azure SQL Edge 'den (Önizleme) veri eşitleme
description: Azure SQL Data Sync kullanarak Azure SQL Edge 'den (Önizleme) veri eşitleme hakkında bilgi edinin
keywords: SQL Edge, SQL Edge 'ten veri eşitleme, SQL Edge veri eşitleme
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 6bcdfc3eb09b6c5ed4f8dde4d48a34bee8746e1e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599658"
---
# <a name="tutorial-sync-data-from-sql-edge-to-azure-sql-database-by-using-sql-data-sync"></a>Öğretici: SQL Data Sync kullanarak SQL Edge 'den Azure SQL veritabanı 'na veri eşitleme

Bu öğreticide, Azure SQL Edge 'ten Azure SQL veritabanı 'na artımlı olarak veri eşitlemek için bir Azure SQL Data Sync *eşitleme grubu* kullanmayı öğreneceksiniz. SQL Data Sync, Azure SQL veritabanı 'nda yerleşik olarak bulunan ve birden çok SQL veritabanı ve SQL Server örneği arasında çift yönlü olan verileri eşitlemenize olanak tanıyan bir hizmettir. SQL Data Sync hakkında daha fazla bilgi için bkz. [Azure SQL Data Sync](../sql-database/sql-database-sync-data.md).

SQL Edge, [SQL Server veritabanı altyapısının](/sql/sql-server/sql-server-technical-documentation/)en son sürümlerinde oluşturulduğundan, bir şirket içi SQL Server örneği için geçerli olan herhangi bir veri eşitleme mekanizması, bir kenar cihazında çalışan bir SQL Edge örneğinden veya buradan veri eşitlemek için de kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, [Azure SQL Data Sync Için veri eşitleme aracısıyla](../sql-database/sql-database-data-sync-agent.md)yapılandırılmış bir Windows bilgisayarı gerektirir.

## <a name="before-you-begin"></a>Başlamadan önce

* Bir Azure SQL veritabanı oluşturun. Azure portal kullanarak Azure SQL veritabanı oluşturma hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda tek bir veritabanı oluşturma](../sql-database/sql-database-single-database-get-started.md?tabs=azure-portal).

* Azure SQL veritabanı dağıtımınızda tabloları ve diğer gerekli nesneleri oluşturun.

* Azure SQL Edge dağıtımınızda gerekli tabloları ve nesneleri oluşturun. Daha fazla bilgi için bkz. SQL [VERITABANı dac PAKETLERINI SQL Edge Ile kullanma](stream-analytics.md).

* Azure SQL Edge örneğini Azure SQL Data Sync için veri eşitleme aracısına kaydedin. Daha fazla bilgi için bkz. Şirket [içi SQL Server veritabanı ekleme](../sql-database/sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="sync-data-between-an-azure-sql-database-and-sql-edge"></a>Azure SQL veritabanı ve SQL Edge arasında verileri eşitleme

SQL Data Sync kullanarak bir Azure SQL veritabanı ile SQL Edge örneği arasında eşitleme ayarlamak üç önemli adımı içerir:  

1. Bir eşitleme grubu oluşturmak için Azure portal kullanın. Daha fazla bilgi için bkz. [eşitleme grubu oluşturma](../sql-database/sql-database-get-started-sql-data-sync.md#create-sync-group). Çeşitli SQL Edge örneklerinden verileri Azure 'daki bir veya daha fazla SQL veritabanı ile eşitlemek için birden çok eşitleme grubu oluşturmak üzere tek bir *hub* veritabanı kullanabilirsiniz.

2. Eşitleme grubuna eşitleme üyeleri ekleyin. Daha fazla bilgi için bkz. [eşitleme üyeleri ekleme](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

3. Eşitlemenin parçası olacak tabloları seçmek için eşitleme grubunu ayarlayın. Daha fazla bilgi için bkz. [eşitleme grubunu yapılandırma](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

Yukarıdaki adımları tamamladıktan sonra, bir Azure SQL veritabanı ve bir SQL Edge örneği içeren bir eşitleme grubunuz olacaktır.

SQL Data Sync hakkında daha fazla bilgi için şu makalelere bakın:

* [Azure SQL Data Sync için veri eşitleme Aracısı](../sql-database/sql-database-data-sync-agent.md)

* [En iyi uygulamalar](../sql-database/sql-database-best-practices-data-sync.md) ve [Azure SQL Data Sync sorunları nasıl giderilir](../sql-database/sql-database-troubleshoot-data-sync.md)

* [Azure Izleyici günlükleriyle SQL Data Sync izleme](../sql-database/sql-database-sync-monitor-oms.md)

* [Transact-SQL veya PowerShell ile eşitleme şemasını güncelleştirme](../sql-database/sql-database-update-sync-schema.md) [PowerShell](../sql-database/scripts/sql-database-sync-update-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SQL veritabanı Ile Azure SQL Edge arasında eşitleme yapmak Için PowerShell 'ı kullanın](../sql-database/scripts/sql-database-sync-data-between-azure-onprem.md). Bu öğreticide, `OnPremiseServer` veritabanı ayrıntılarını Azure SQL Edge ayrıntıları ile değiştirin.
