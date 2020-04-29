---
title: Yapılandırma ve yönetme
description: Azure SQL veritabanı 'nı yapılandırmayı ve yönetmeyi öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79209411"
---
# <a name="how-to-use-azure-sql-database"></a>Azure SQL veritabanı 'nı kullanma

Bu bölümde, Azure SQL veritabanınızı yönetmenize ve yapılandırmanıza yardımcı olabilecek çeşitli kılavuzlar, betikler ve açıklamalar bulabilirsiniz. Ayrıca, [tek veritabanı](sql-database-howto-single-database.md) ve [yönetilen örnek](sql-database-howto-managed-instance.md)için özel nasıl yapılır kılavuzlarını bulabilirsiniz.

## <a name="load-data"></a>Veri yükleme

- [Azure 'da tek bir veritabanını veya havuza alınmış bir veritabanını kopyalama](sql-database-copy.md)
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
- [Genişletilmiş olayları olay dosyasına depola](sql-database-xevent-code-event-file.md)
- [Genişletilmiş olayları halka arabelleğine depola](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>Yapılandırma özellikleri

- [Azure AD kimlik doğrulamasını yapılandırma](sql-database-aad-authentication-configure.md)
- [Koşullu erişimi yapılandırma](sql-database-conditional-access.md)
- [Çok faktörlü AAD kimlik doğrulaması](sql-database-ssms-mfa-authentication.md)
- [Çok faktörlü kimlik doğrulamasını yapılandırma](sql-database-ssms-mfa-authentication-configure.md)
- [Zamana bağlı saklama ilkesini yapılandırma](sql-database-temporal-tables-retention-policy.md)
- [KAG ile TDE Yapılandırması](transparent-data-encryption-byok-azure-sql-configure.md)
- [TDE KAG anahtarlarını döndürme](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- [TDE koruyucusunu kaldırma](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
- [Bellek içi OLTP’yi yapılandırma](sql-database-in-memory-oltp-migration.md)
- [Azure Otomasyonu 'Nu yapılandırma](sql-database-manage-automation.md)

## <a name="develop-applications"></a>Uygulama geliştirme

- [Bağlantı](sql-database-libraries.md)
- [Spark Bağlayıcısı kullanma](sql-database-spark-connector.md)
- [Uygulamanın kimliğini doğrulama](sql-database-client-id-keys.md)
- [Daha iyi performans için toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md)
- [Bağlantı kılavuzu](sql-database-connectivity-issues.md)
- [DNS diğer adları](dns-alias-overview.md)
- [DNS diğer ad PowerShell 'i ayarla](dns-alias-powershell.md)
- [Bağlantı noktaları - ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C ve C++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>Uygulamaları tasarlama

- [Olağanüstü durum kurtarmaya yönelik tasarım](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Elastik havuzlara yönelik tasarım](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Uygulama yükseltmelerine yönelik tasarım](sql-database-manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Çok kiracılı SaaS uygulamaları tasarlama

- [SaaS tasarım desenleri](saas-tenancy-app-design-patterns.md)
- [SaaS video dizinleyicisi](saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [SaaS uygulama güvenliği](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen örnekler Için nasıl yapılır kılavuzlarından](sql-database-howto-managed-instance.md)daha fazla bilgi edinin.
- [Tek veritabanları Için nasıl yapılır kılavuzlarından](sql-database-howto-single-database.md)daha fazla bilgi edinin.
