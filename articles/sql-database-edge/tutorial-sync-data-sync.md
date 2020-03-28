---
title: SQL Data Sync kullanarak Azure SQL Database Edge'den verileri eşitleme | Microsoft Dokümanlar
description: Azure SQL Veri Eşitlemeyi'ni kullanarak Azure SQL Veritabanı Edge'den verileri eşitleme hakkında bilgi edinin
keywords: sql veritabanı kenarı,sql veritabanı kenarından senkronizasyon verileri, sql veritabanı kenarı veri senkronizasyonu
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 7c38ba6dbabef4affd8672295a93d46fd4b0e494
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74384176"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-sql-database-by-using-sql-data-sync"></a>Öğretici: SQL Veri Eşitlemeyi kullanarak SQL Database Edge'den Azure SQL Veritabanı'na verileri senkronize etme

Bu eğitimde, Azure SQL Veritabanı Edge'den Azure SQL Veritabanı Veritabanı'na kadar verileri artımlı olarak eşitlemek için Azure SQL Veri *Eşitleme grubu* nasıl kullanılacağını öğreneceksiniz. SQL Data Sync, seçtiğiniz verileri birden çok SQL veritabanı ve SQL Server örnekleri arasında çift yönlü olarak eşitlemenize olanak tanıyan Azure SQL Veritabanı'nda oluşturulmuş bir hizmettir. SQL Veri Eşitleme hakkında daha fazla bilgi için [Azure SQL Veri Eşitleme'ye](../sql-database/sql-database-sync-data.md)bakın.

SQL Database [Edge, SQL Server Veritabanı Altyapısının](/sql/sql-server/sql-server-technical-documentation/)en son sürümlerinde yerleşik olduğundan, şirket içi SQL Server örneğine uygulanabilen tüm veri eşitleme mekanizması, bir kenar aygıtında çalışan bir SQL Database Edge örneğine veya bu verilerden veri eşitlemek için de kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, [Azure SQL Veri Eşitleme için Veri Eşitleme Aracısı](../sql-database/sql-database-data-sync-agent.md)ile yapılandırılan bir Windows bilgisayarı gerektirir.

## <a name="before-you-begin"></a>Başlamadan önce

* Bir Azure SQL veritabanı oluşturun. Azure portalını kullanarak Azure SQL veritabanı oluşturma hakkında bilgi [için](../sql-database/sql-database-single-database-get-started.md?tabs=azure-portal)bkz.

* Azure SQL Veritabanı dağıtımınızda tabloları ve diğer gerekli nesneleri oluşturun.

* Azure SQL Veritabanı Kenarı dağıtımınızda gerekli tabloları ve nesneleri oluşturun. Daha fazla bilgi için [bkz.](stream-analytics.md)

* Azure SQL Veritabanı Kenarı örneğini Azure SQL Veri Eşitlemi için Veri Eşitleme Aracısı ile kaydedin. Daha fazla bilgi için [bkz.](../sql-database/sql-database-get-started-sql-data-sync.md#add-on-prem)

## <a name="sync-data-between-an-azure-sql-database-and-sql-database-edge"></a>Verileri Azure SQL veritabanı ve SQL Veritabanı Kenarı arasında eşitleme

SQL Veri Eşitlemesi kullanarak Bir Azure SQL veritabanı ile SQL Database Edge örneği arasında eşitleme ayarlama üç temel adım içerir:  

1. Eşitleme grubu oluşturmak için Azure portalını kullanın. Daha fazla bilgi için [bkz.](../sql-database/sql-database-get-started-sql-data-sync.md#create-sync-group) Çeşitli SQL Veritabanı Kenarı örneklerinden verileri Azure'daki bir veya daha fazla SQL veritabanıyla eşitlemek için birden çok eşitleme grubu oluşturmak için tek bir *hub* veritabanı kullanabilirsiniz.

2. Eşitleme grubuna eşitleme üyeleri ekleyin. Daha fazla bilgi için [bkz.](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members)

3. Eşitlemenin bir parçası olacak tabloları seçmek için eşitleme grubunu ayarlayın. Daha fazla bilgi için [bkz.](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members)

Önceki adımları tamamladıktan sonra, bir Azure SQL veritabanı ve bir SQL Veritabanı Kenarı örneği içeren bir eşitleme grubunuz olur.

SQL Veri Eşitlemi hakkında daha fazla bilgi için şu makalelere bakın:

* [Azure SQL Veri Eşitlemi için Veri Eşitleme Aracısı](../sql-database/sql-database-data-sync-agent.md)

* [En iyi uygulamalar](../sql-database/sql-database-best-practices-data-sync.md) ve [Azure SQL Veri Eşitlemenile ilgili sorunları giderme](../sql-database/sql-database-troubleshoot-data-sync.md)

* [Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin](../sql-database/sql-database-sync-monitor-oms.md)

* [Eşitleme şeasını Transact-SQL](../sql-database/sql-database-update-sync-schema.md) veya [PowerShell](../sql-database/scripts/sql-database-sync-update-schema.md) ile güncelleştirme

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SQL Veritabanı ve Azure SQL Veritabanı Kenarı arasında eşitlemek için PowerShell'i kullanın.](../sql-database/scripts/sql-database-sync-data-between-azure-onprem.md) Bu eğitimde, `OnPremiseServer` veritabanı ayrıntılarını Azure SQL Veritabanı Kenarı ayrıntılarıyla değiştirin.
