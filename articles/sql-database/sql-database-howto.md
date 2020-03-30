---
title: Yapılandırma ve yönetme
description: Azure SQL Veritabanı'nı nasıl yapılandırıp yöneteceklerinizi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 11/14/2019
ms.openlocfilehash: c3f7b33e4b42b08334cfb687024985c878dc3713
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79209411"
---
# <a name="how-to-use-azure-sql-database"></a>Azure SQL Veritabanı nasıl kullanılır?

Bu bölümde, Azure SQL Veritabanınızı yönetmenize ve yapılandırmanıza yardımcı olabilecek çeşitli kılavuzlar, komut dosyaları ve açıklamalar bulabilirsiniz. Ayrıca, [tek bir veritabanı](sql-database-howto-single-database.md) ve Yönetilen [Örnek](sql-database-howto-managed-instance.md)için belirli nasıl yapılacağını kılavuzları bulabilirsiniz.

## <a name="load-data"></a>Veri yükleme

- [Azure içinde tek bir veritabanı nı veya havuza veritabanını kopyalama](sql-database-copy.md)
- [BACPAC’ten veritabanını içeri aktarma](sql-database-import.md)
- [Veritabanını BACPAC’e dışarı aktarma](sql-database-export.md)
- [BCP ile veri yükleme](sql-database-load-from-csv-with-bcp.md)
- [ADF ile veri yükleme](../data-factory/connector-azure-sql-database.md?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Veri eşitleme

- [SQL Data Sync](sql-database-sync-data.md)
- [Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)
- [Şema değişikliklerini çoğaltma](sql-database-update-sync-schema.md)
- [OMS ile izleme](sql-database-sync-monitor-oms.md)
- [Data Sync için en iyi deneyimler](sql-database-best-practices-data-sync.md)
- [Data Sync’de sorun giderme](sql-database-troubleshoot-data-sync.md)

## <a name="monitoring-and-tuning"></a>İzleme ve ayarlama

- [El ile ayarlama](sql-database-performance-guidance.md)
- [Performansı izlemek için DMV’leri kullanma](sql-database-monitoring-with-dmvs.md)
- [Performansı izlemek için Sorgu deposunu kullanma](https://docs.microsoft.com/sql/relational-databases/performance/best-practice-with-the-query-store#Insight)
- [Akıllı İçgörüler ile performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md)
- [Akıllı İçgörüler tanılama günlüğünü kullanma](sql-database-intelligent-insights-use-diagnostics-log.md)
- [Bellek İçi OLTP alanını izleme](sql-database-in-memory-oltp-monitoring.md)

### <a name="extended-events"></a>Genişletilmiş olaylar

- [Genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md)
- [Genişletilmiş olayları etkinlik dosyasına depola](sql-database-xevent-code-event-file.md)
- [Mağaza Genişletilmiş olayları halka arabelleği içine](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>Yapılandırma özellikleri

- [Azure AD kimlik doğrulamasını yapılandırma](sql-database-aad-authentication-configure.md)
- [Koşullu Erişimi Yapılandırma](sql-database-conditional-access.md)
- [Çok faktörlü AAD kimlik doğrulaması](sql-database-ssms-mfa-authentication.md)
- [Çok faktörlü kimlik doğrulamasını yapılandırma](sql-database-ssms-mfa-authentication-configure.md)
- [Zamana bağlı saklama ilkesini yapılandırma](sql-database-temporal-tables-retention-policy.md)
- [KAG ile TDE Yapılandırması](transparent-data-encryption-byok-azure-sql-configure.md)
- [TDE KAG anahtarlarını döndürme](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- [TDE koruyucusunu kaldırma](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
- [Bellek içi OLTP’yi yapılandırma](sql-database-in-memory-oltp-migration.md)
- [Azure Otomasyonu Yapılandırma](sql-database-manage-automation.md)

## <a name="develop-applications"></a>Uygulama geliştirme

- [Bağlantı](sql-database-libraries.md)
- [Spark Bağlayıcısı kullanma](sql-database-spark-connector.md)
- [Uygulamayı kimlik doğrulaması](sql-database-client-id-keys.md)
- [Daha iyi performans için toplu işlemeyi kullanma](sql-database-use-batching-to-improve-performance.md)
- [Bağlantı kılavuzu](sql-database-connectivity-issues.md)
- [DNS diğer adlar](dns-alias-overview.md)
- [Kurulum DNS takma powershell](dns-alias-powershell.md)
- [Bağlantı noktaları - ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C ve C++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>Uygulamaları tasarlama

- [Olağanüstü durum kurtarmaya yönelik tasarım](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Elastik havuzlara yönelik tasarım](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Uygulama yükseltmelerine yönelik tasarım](sql-database-manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Çok kiracılı SaaS uygulamaları tasarla

- [SaaS tasarım desenleri](saas-tenancy-app-design-patterns.md)
- [SaaS video dizinleyicisi](saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [SaaS uygulama güvenliği](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen örnekler için Nasıl Yap'ın yap'ı kılavuzları](sql-database-howto-managed-instance.md)hakkında daha fazla bilgi edinin.
- [Tek veritabanları için Nasıl YapIlenLer kılavuzları](sql-database-howto-single-database.md)hakkında daha fazla bilgi edinin.
