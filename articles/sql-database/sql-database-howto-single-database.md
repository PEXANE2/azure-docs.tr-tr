---
title: Tek bir veritabanı nasıl yapılandırılabilen
description: Azure SQL Veritabanını nasıl yapılandırıp yöneteceklerinizi öğrenin - tek veritabanı
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 01/14/2020
ms.openlocfilehash: 4283e1a2c92b7fe738fb57a8103cea1deb0015a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76027708"
---
# <a name="how-to-use-a-single-database-in-azure-sql-database"></a>Azure SQL Veritabanı'nda tek bir veritabanı nasıl kullanılır?

Bu bölümde, Azure SQL Veritabanı'nda tek veritabanınızı yönetmenize ve yapılandırmanıza yardımcı olabilecek çeşitli kılavuzlar, komut dosyaları ve açıklamalar bulabilirsiniz.

## <a name="migrate"></a>Geçiş

- [SQL Veritabanına Geçir](sql-database-single-database-migrate.md) – Yönetilen bir örne geçiş için önerilen geçiş işlemi ve araçları hakkında bilgi edinin.
- [Geçişten sonra SQL veritabanını](sql-database-manage-after-migration.md)nasıl yöneteceklerini öğrenin.

## <a name="configure-features"></a>Yapılandırma özellikleri

- İşlem çoğaltmaişlemini, tarihünüzü veritabanları arasında çoğaltmak üzere [yapılandırın.](replication-to-sql-database.md)
- Azure SQL Veritabanı'nın SQL Injection veya şüpheli konumlardan erişim gibi şüpheli etkinlikleri tanımlamasına izin vermek için [tehdit algılamayı yapılandırın.](sql-database-threat-detection.md)
- Hassas verilerinizi korumak için [dinamik veri maskelemesini yapılandırın.](sql-database-dynamic-data-masking-get-started-portal.md)
- Yedeklemelerinizi Azure Blob Depolama'da tutmak için bir veritabanı için [yedekleme bekletme](sql-database-long-term-backup-retention-configure.md) yapılandırması. Alternatif olarak [Azure vault (amortismana alınmış) yaklaşımını kullanarak yedekleme bekletme yapılandırması](sql-database-long-term-backup-retention-configure-vault.md) vardır.
- Veritabanınızın bir kopyasını başka bir bölgede tutmak için [coğrafi çoğaltmayı yapılandırın.](sql-database-geo-replication-portal.md)
- [Coğrafi yinelemeler için güvenliği yapılandırın.](sql-database-geo-replication-security-config.md)

## <a name="monitor-and-tune-your-database"></a>Veritabanınızı izleyin ve ayarlayın

- Azure SQL Veritabanı'nın iş yükünüzün performansını optimize etmesine izin vermek için [otomatik ayar'ı etkinleştirin.](sql-database-automatic-tuning-enable.md)
- Otomatik ayar önerileri hakkında bilgi almak [için e-posta bildirimlerini etkinleştirin.](sql-database-automatic-tuning-email-notifications.md)
- [Performans önerileri uygulayın](sql-database-advisor-portal.md) ve veritabanınızı optimize edin.
- Azure SQL Veritabanı'ndan bildirim almak için [uyarılar oluşturun.](sql-database-insights-alerts-portal.md)
- Uygulamalar ve veritabanı arasında bazı bağlantı sorunları fark [ederseniz, bağlantı](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md) sorunlarını giderin. Bağlantı sorunları [için Kaynak Durumu'nun](sql-database-resource-health.md)kullanımını da kullanabilirsiniz.
- Veritabanınızdaki depolama kullanımını izlemek için [dosya alanını yönetin.](sql-database-file-space-management.md)

## <a name="query-distributed-data"></a>Sorgu dağıtılmış verileri

- [Dikey olarak bölümlenmiş verileri](sql-database-elastic-query-getting-started-vertical.md) birden çok veritabanı arasında sorgula.
- [Ölçeklenmiş veri katmanı nda rapor](sql-database-elastic-query-horizontal-partitioning.md)verin.
- [Farklı şemalara sahip tablolar arasında sorgula.](sql-database-elastic-query-vertical-partitioning.md)

## <a name="elastic-database-jobs"></a>Elastik Veritabanı İşleri

- [Oluşturma ve yönetme](elastic-jobs-powershell.md) PowerShell kullanarak Elastik Veritabanı İşleri.
- [Oluşturma ve yönetme](elastic-jobs-tsql.md) Transact-SQL kullanarak Elastik Veritabanı İşleri.
- [Eski Elastik işten göç](elastic-jobs-migrate.md).

## <a name="database-sharding"></a>Veritabanı parçalama

- [Esnek veritabanı istemci kitaplığını yükseltin.](sql-database-elastic-scale-upgrade-client-library.md)
- [Kırık uygulama oluşturun.](sql-database-elastic-scale-get-started.md)
- [Yatay olarak parçalanan verileri sorgula.](sql-database-elastic-query-getting-started.md)
- [Çok parçalı sorguları çalıştırın.](sql-database-elastic-scale-multishard-querying.md)
- [Kırık verileri taşıyın.](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
- Veritabanı kırıklarında [güvenliği yapılandırın.](sql-database-elastic-scale-split-merge-security-configuration.md)
- Geçerli veritabanı parçaları kümesine [bir parça ekleyin.](sql-database-elastic-scale-add-a-shard.md)
- [Parça harita sorunlarını düzeltin.](sql-database-elastic-database-recovery-manager.md)
- [Göç kırıkd B .](sql-database-elastic-convert-to-use-elastic-tools.md)
- [Sayaçlar oluşturun.](sql-database-elastic-database-perf-counters.md)
- Parçalanmış verileri sorgulamak için [varlık çerçevesini kullanın.](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
- Parçalanmış verileri sorgulamak için [Dapper çerçevesini kullanın.](sql-database-elastic-scale-working-with-dapper.md)

## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen örnek için Nasıl Nasıl Yap' kılavuzları](sql-database-howto-managed-instance.md) hakkında daha fazla bilgi edinin
