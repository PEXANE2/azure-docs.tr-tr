---
title: Çok kiracılı uygulama örneği - Wingtip SaaS
description: Wingtip SaaS örneği olan Azure SQL Veritabanı'nı kullanan örnek çok kiracılı bir uygulama kullanarak öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 09/24/2018
ms.openlocfilehash: 46cdcd5f768278dbc729f48e450c68a63be604be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256503"
---
# <a name="introduction-to-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>SQL Veritabanı ile kiracı başına veritabanı deseni kullanan çok kiracılı bir SaaS uygulamasına giriş

Wingtip SaaS uygulaması örnek bir çok kiracı uygulamasıdır. Uygulama, birden çok kiracıya hizmet vermek için kiracı başına veritabanı SaaS uygulama deseni kullanır. Uygulama, Azure SQL Veritabanı'nın çeşitli SaaS tasarım ve yönetim desenleri kullanarak SaaS senaryolarını etkinleştiren özelliklerini sergiler. Wingtip SaaS uygulaması, hızlı bir şekilde çalışmaya ve çalıştırmaya beş dakikadan kısa bir süre içinde devreye saçılır.

Uygulama kaynak kodu ve yönetim komut ları [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub repo mevcuttur. Başlamadan önce Wingtip Tickets yönetim komut dosyalarını indirmek ve engelini kaldırmak için adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) bakın.

## <a name="application-architecture"></a>Uygulama mimarisi

Wingtip SaaS uygulaması, kiracı başına veritabanı modelini kullanır. Verimliliği en üst düzeye çıkarmak için SQL elastik havuzları kullanır. Kiracıların verilerine sağlama ve eşleme için bir katalog veritabanı kullanılır. Çekirdek Wingtip SaaS uygulaması üç örnek kiracı, artı katalog veritabanı ile bir havuz kullanır. Katalog ve kiracı sunucuları DNS takma adlarıyla birlikte sağlanmıştır. Bu diğer adlar Wingtip uygulaması tarafından kullanılan etkin kaynaklara başvuruyu korumak için kullanılır. Bu diğer adlar, olağanüstü durum kurtarma öğreticilerinde kurtarma kaynaklarına işaret etmek için güncelleştirilir. Wingtip SaaS öğreticilerinin çoğunutamamlamak, ilk dağıtıma eklentiler sağlar. Analitik veritabanları ve çapraz veritabanı şeması yönetimi gibi eklentiler tanıtılır.


![Wingtip SaaS mimarisi](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Öğreticiler aracılığıyla gitmek ve uygulama ile çalışmak gibi, veri katmanı ile ilgili olarak SaaS desenleri odaklanmak. Başka bir deyişle, veri katmanına odaklanın ve uygulamanın kendisini aşırı analiz etmeyin. Bu SaaS desenlerinin uygulanmasını anlamak, uygulamalarınızda bu desenleri uygulamanın anahtarıdır. Ayrıca, özel iş gereksinimleriniz için gerekli değişiklikleri de göz önünde bulundurun.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL Veritabanı Wingtip SaaS eğitimleri

Uygulamayı dağıttıktan sonra, ilk dağıtıma dayanan aşağıdaki öğreticileri keşfedin. Bu öğreticiler, SQL Veritabanı, Azure SQL Veri Ambarı ve diğer Azure hizmetlerinin yerleşik özelliklerinden yararlanan yaygın SaaS desenlerini inceler. Öğreticiler ayrıntılı açıklamalar ile PowerShell komut içerir. Açıklamalar, uygulamalarınızda aynı SaaS yönetim modellerinin anlaşılmasını ve uygulanmasını kolaylaştırır.


| Öğretici | Açıklama |
|:--|:--|
| [SQL Veritabanı çok kiracılı SaaS uygulaması örneği için kılavuz ve ipuçları](saas-tenancy-wingtip-app-guidance-tips.md) | Uygulamanın bölümlerini hazırlamak için PowerShell komut dosyalarını indirin ve çalıştırın. |
|[Wingtip SaaS uygulamasını dağıtın ve keşfedin](saas-dbpertenant-get-started-deploy.md)|  Azure aboneliğinizle Wingtip SaaS uygulamasını dağıtın ve keşfedin. |
|[Hüküm ve katalog kiracılar](saas-dbpertenant-provision-and-catalog.md)| Bir katalog veritabanı kullanarak uygulamanın kiracılara nasıl bağdaştirdığını ve kataloğun kiracıları verilerine nasıl eşleşdirmelerini öğrenin. |
|[Performansı izleme ve yönetme](saas-dbpertenant-performance-monitoring.md)| SQL Veritabanı'nın izleme özelliklerini nasıl kullanacağınızı ve performans eşikleri aşıldığında uyarıları nasıl ayarlayizleyeceğinizi öğrenin. |
|[Azure İzleyici günlükleri ile izleme](saas-dbpertenant-log-analytics.md) | Birden çok havuzda büyük miktarda kaynağı izlemek için [Azure Monitor günlüklerini](../log-analytics/log-analytics-overview.md) nasıl kullanacağınızı öğrenin. |
|[Tek bir kiracıyı geri yükleme](saas-dbpertenant-restore-single-tenant.md)| Kiracı veritabanını önceki bir noktaya nasıl geri yükleyin. Ayrıca, varolan kiracı veritabanını çevrimiçi bırakan paralel bir veritabanına nasıl geri yükleyin. |
|[Kiracı veritabanı şemalarını yönetme](saas-tenancy-schema-management.md)| Şemayı nasıl güncelleştirip tüm kiracı veritabanlarında başvuru verilerini güncelleştireceklerini öğrenin. |
|[Kiracılar arası dağıtılmış sorguları çalıştırma](saas-tenancy-cross-tenant-reporting.md) | Geçici bir analitik veritabanı oluşturun ve tüm kiracılar arasında gerçek zamanlı dağıtılmış sorgular çalıştırın.  |
|[Çıkarılan kiracı verilerinde analiz çalıştırma](saas-tenancy-tenant-analytics.md) | Çevrimdışı analitik sorguları için kiracı verilerini bir analitiği veritabanına veya veri ambarına ayıklayın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Wingtip Tickets SaaS uygulaması örneğini dağıttığınızda ve kullandığınızda genel rehberlik ve ipuçları](saas-tenancy-wingtip-app-guidance-tips.md)
- [Wingtip SaaS uygulamasını dağıtın](saas-dbpertenant-get-started-deploy.md)
