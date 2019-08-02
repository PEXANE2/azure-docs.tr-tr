---
title: Azure SQL veritabanı sürüm notları | Microsoft Docs
description: Azure SQL veritabanı hizmetindeki ve Azure SQL veritabanı belgelerindeki yeni özellikler ve geliştirmeler hakkında bilgi edinin
services: sql-database
author: stevestein
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: sstein
ms.openlocfilehash: 55e8121152aa281ab1025f1e8ded53c4b00c3d44
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566936"
---
# <a name="sql-database-release-notes"></a>SQL veritabanı sürüm notları

Bu makalede, şu anda genel önizleme aşamasında olan SQL veritabanı özellikleri listelenmektedir. SQL veritabanı güncelleştirmeleri ve geliştirmeleri için bkz. [SQL Database Service Updates](https://azure.microsoft.com/updates/?product=sql-database). Diğer Azure hizmetlerine yönelik güncelleştirmeler ve geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Genel önizlemede Özellikler

| Özellik | Ayrıntılar |
| ---| --- |
| Tek veritabanları ve elastik havuzlarla hızlandırılmış veritabanı kurtarma | Bilgi için bkz. [hızlandırılmış veritabanı kurtarma](sql-database-accelerated-database-recovery.md).|
|Yaklaşık sayı farklı|Daha fazla bilgi için bkz. [yaklaşık Count DISTINCT](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing).|
|Rowstore 'da Batch modu (uyumluluk düzeyi 150 altında)|Bilgi için bkz. [rowstore 'Da Batch modu](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore).|
| Veri bulma ve sınıflandırma  |Bilgi için bkz. [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırması](sql-database-data-discovery-and-classification.md).|
| Elastik veritabanı işleri | Bilgi için bkz. [elastik Işler oluşturma, yapılandırma ve yönetme](elastic-jobs-overview.md). |
| Esnek sorgular | Daha fazla bilgi için bkz. [elastik sorguya genel bakış](sql-database-elastic-query-overview.md). |
| Elastik işlemler | [Bulut veritabanları arasında dağıtılmış işlemler](sql-database-elastic-transactions-overview.md). |
|Bellek Izni geri bildirimi (satır modu) (uyumluluk düzeyi 150 altında)|Bilgi için bkz. [bellek verme geri bildirimi (satır modu)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Azure portal sorgu Düzenleyicisi |Bilgi için bkz. [Azure Portal SQL sorgu düzenleyicisini kullanarak bağlanma ve veri sorgulama](sql-database-connect-query-portal.md).|
| Tek veritabanları ve elastik havuzlarla R Hizmetleri/makine öğrenimi |Bilgi için bkz. [Azure SQL veritabanı 'nda Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database).|
| Yönetilen örneklerle bırakılan veritabanlarını yeniden oluşturma |Daha fazla bilgi için bkz. [Azure SQL yönetilen örneği 'nde bırakılan veritabanlarını yeniden oluşturma](https://medium.com/azure-sqldb-managed-instance/re-create-dropped-databases-in-azure-sql-managed-instance-dc369ed60266).|
| Yönetilen örneklerle çoğaltma |Daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örnek veritabanında çoğaltmayı yapılandırma](replication-with-sql-database-managed-instance.md).|
| Sunucusuz işlem katmanı | Daha fazla bilgi için bkz. [SQL veritabanı sunucusuz (Önizleme)](sql-database-serverless.md).|
|SQL Analytics|Bilgi için bkz. [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).|
|Tablo değişkeni ertelenmiş derlemesi (uyumluluk düzeyi 150 altında)|Bilgi için bkz. [tablo değişkeni ertelenmiş derleme](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| Yönetilen örneklerle tehdit algılama |Daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örneği 'nde tehdit algılamayı yapılandırma](sql-database-managed-instance-threat-detection.md).|
| Yönetilen örneklerle Kendi Anahtarını Getir (BYOK) ile saydam veri şifrelemesi (TDE) |Bilgi için, bkz [. Azure SQL saydam veri şifrelemesi Azure Key Vault müşteri tarafından yönetilen anahtarlar: Kendi Anahtarını Getir desteği](transparent-data-encryption-byok-azure-sql.md).|
| &nbsp; |

## <a name="updates"></a>Güncelleştirmeler

SQL veritabanı güncelleştirmelerinin ve geliştirmelerin bir listesi için bkz. [SQL Database Service Updates](https://azure.microsoft.com/updates/?product=sql-database).

Tüm Azure hizmetleri için güncelleştirmeler ve geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="contribute-to-content"></a>İçeriğe katkıda bulunma

Azure SQL veritabanı belgelerine katkıda bulunmak için [docs katılımcısı Kılavuzu](https://docs.microsoft.com/contribute/)' na bakın.
