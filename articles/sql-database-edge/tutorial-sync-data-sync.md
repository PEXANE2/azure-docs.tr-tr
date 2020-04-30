---
title: SQL Data Sync kullanarak Azure SQL veritabanı kenarından verileri eşitleyin | Microsoft Docs
description: Azure SQL Data Sync kullanarak Azure SQL veritabanı kenarından veri eşitleme hakkında bilgi edinin
keywords: SQL veritabanı Edge, SQL veritabanı kenarından veri eşitleme, SQL veritabanı Edge veri eşitleme
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 7c38ba6dbabef4affd8672295a93d46fd4b0e494
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74384176"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-sql-database-by-using-sql-data-sync"></a>Öğretici: SQL Data Sync kullanarak SQL veritabanı kenarından Azure SQL veritabanı 'na veri eşitleme

Bu öğreticide, Azure SQL veritabanı kenarından Azure SQL veritabanı 'na artımlı olarak veri eşitlemek için bir Azure SQL Data Sync *eşitleme grubu* kullanmayı öğreneceksiniz. SQL Data Sync, Azure SQL veritabanı 'nda yerleşik olarak bulunan ve birden çok SQL veritabanı ve SQL Server örneği arasında çift yönlü olan verileri eşitlemenize olanak tanıyan bir hizmettir. SQL Data Sync hakkında daha fazla bilgi için bkz. [Azure SQL Data Sync](../sql-database/sql-database-sync-data.md).

SQL veritabanı Edge, [SQL Server veritabanı altyapısının](/sql/sql-server/sql-server-technical-documentation/)en son sürümlerinde oluşturulduğundan, bir şirket içi SQL Server örneği için geçerli olan herhangi bir veri eşitleme mekanizması, bir kenar cihazında çalışan bir SQL veritabanı Edge örneğinden veya bu veri ile verileri eşitlemek için de kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, [Azure SQL Data Sync Için veri eşitleme aracısıyla](../sql-database/sql-database-data-sync-agent.md)yapılandırılmış bir Windows bilgisayarı gerektirir.

## <a name="before-you-begin"></a>Başlamadan önce

* Bir Azure SQL veritabanı oluşturun. Azure portal kullanarak Azure SQL veritabanı oluşturma hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda tek bir veritabanı oluşturma](../sql-database/sql-database-single-database-get-started.md?tabs=azure-portal).

* Azure SQL veritabanı dağıtımınızda tabloları ve diğer gerekli nesneleri oluşturun.

* Azure SQL veritabanı Edge dağıtımınızda gerekli tabloları ve nesneleri oluşturun. Daha fazla bilgi için bkz. SQL veritabanı [Edge Ile SQL VERITABANı dac paketlerini kullanma](stream-analytics.md).

* Azure SQL veritabanı Edge örneğini Azure SQL Data Sync için veri eşitleme aracısına kaydedin. Daha fazla bilgi için bkz. Şirket [içi SQL Server veritabanı ekleme](../sql-database/sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="sync-data-between-an-azure-sql-database-and-sql-database-edge"></a>Verileri bir Azure SQL veritabanı ve SQL veritabanı kenarı arasında eşitleme

SQL Data Sync kullanarak bir Azure SQL veritabanı ile SQL veritabanı Edge örneği arasında eşitlemeyi ayarlamak, üç temel adımı içerir:  

1. Bir eşitleme grubu oluşturmak için Azure portal kullanın. Daha fazla bilgi için bkz. [eşitleme grubu oluşturma](../sql-database/sql-database-get-started-sql-data-sync.md#create-sync-group). Çeşitli SQL veritabanı Edge örneklerinden verileri Azure 'daki bir veya daha fazla SQL veritabanı ile eşitlemek için birden çok eşitleme grubu oluşturmak üzere tek bir *hub* veritabanı kullanabilirsiniz.

2. Eşitleme grubuna eşitleme üyeleri ekleyin. Daha fazla bilgi için bkz. [eşitleme üyeleri ekleme](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

3. Eşitlemenin parçası olacak tabloları seçmek için eşitleme grubunu ayarlayın. Daha fazla bilgi için bkz. [eşitleme grubunu yapılandırma](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

Yukarıdaki adımları tamamladıktan sonra, Azure SQL veritabanı ve SQL veritabanı Edge örneği içeren bir eşitleme grubunuz olacaktır.

SQL Data Sync hakkında daha fazla bilgi için şu makalelere bakın:

* [Azure SQL Data Sync için veri eşitleme Aracısı](../sql-database/sql-database-data-sync-agent.md)

* [En iyi uygulamalar](../sql-database/sql-database-best-practices-data-sync.md) ve [Azure SQL Data Sync sorunları nasıl giderilir](../sql-database/sql-database-troubleshoot-data-sync.md)

* [Azure Izleyici günlükleriyle SQL Data Sync izleme](../sql-database/sql-database-sync-monitor-oms.md)

* [Transact-SQL veya PowerShell ile eşitleme şemasını güncelleştirme](../sql-database/sql-database-update-sync-schema.md) [PowerShell](../sql-database/scripts/sql-database-sync-update-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SQL veritabanı ve Azure SQL veritabanı kenarı arasında eşitleme yapmak Için PowerShell 'ı kullanın](../sql-database/scripts/sql-database-sync-data-between-azure-onprem.md). Bu öğreticide, `OnPremiseServer` veritabanı AYRıNTıLARıNı Azure SQL veritabanı Edge ayrıntıları ile değiştirin.
