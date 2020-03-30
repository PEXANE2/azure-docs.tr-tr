---
title: Tek bir veritabanı nedir
description: Azure SQL Veritabanı'nda tek veritabanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 04/08/2019
ms.openlocfilehash: fc63de4057def632d3ac1980e8cb3eaedbff2175
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79500748"
---
# <a name="what-is-a-single-database-in-azure-sql-database"></a>Azure SQL Veritabanı'nda tek bir veritabanı nedir

Tek veritabanı dağıtım seçeneği, Azure SQL Veritabanı'nda kendi kaynakları yla bir veritabanı oluşturur ve bir SQL Veritabanı sunucusu üzerinden yönetilir. Tek bir veritabanı ile, her veritabanı birbirinden izole ve taşınabilir, [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) ve garantili bilgi işlem boyutu içinde her biri kendi hizmet katmanı vardır.

> [!IMPORTANT]
> Tek veritabanı, Azure SQL Veritabanı için üç dağıtım seçeneğinden biridir. Diğer iki [elastik havuzları](sql-database-elastic-pool.md) ve [yönetilen örneğivardır.](sql-database-managed-instance.md)
> [!NOTE]
> Azure SQL Veritabanı'ndaki terimler sözlüğü için SQL [Veritabanı terimleri sözlüğüne](sql-database-glossary-terms.md) bakın

## <a name="dynamic-scalability"></a>Dinamik ölçeklenebilirlik

İlk uygulamanızı, sunucusuz bilgi işlem katmanında düşük maliyetle küçük, tek bir veritabanında veya sağlanan bilgi işlem katmanında küçük bir bilgi işlem boyutunda oluşturabilirsiniz. Çözümünüzün gereksinimlerini karşılamak için [işlem veya hizmet katmanını](sql-database-single-database-scale.md) istediğiniz zaman el ile veya programlı olarak değiştirirsiniz. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

## <a name="single-databases-and-elastic-pools"></a>Tek veritabanları ve elastik havuzlar

Tek bir veritabanı kaynak paylaşımı için elastik bir [havuza](sql-database-elastic-pool.md) veya dışına taşınabilir. Tek veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Elastik havuzlar bu sorunu çözmek için tasarlanmıştır. Esnek havuzların işleyiş mantığı gayet basittir. Performans kaynaklarını tek bir veritabanı yerine bir havuza ayırır ve tek bir veritabanı performansı yerine havuzun toplu performans kaynakları için ödeme yapabilirsiniz.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Yerleşik [performans izleme](sql-database-performance-guidance.md) ve uyarı [araçlarını,](sql-database-insights-alerts-portal.md)performans derecelendirmeleriyle birlikte kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre ölçek büyütme veya küçültme işlemlerinin etkisini hızlı bir şekilde değerlendirebilirsiniz. Ayrıca, SQL Veritabanı daha kolay izleme için [ölçümler ve kaynak günlükleri yatabilir.](sql-database-metrics-diag-logging.md)

## <a name="availability-capabilities"></a>Kullanılabilirlik özellikleri

Tek veritabanları, elastik havuzlar ve yönetilen örneklerin tümü birçok kullanılabilirlik özelliği sağlar. Daha fazla bilgi için [Kullanılabilirlik özelliklerine](sql-database-technical-overview.md#availability-capabilities)bakın.

## <a name="transact-sql-differences"></a>Transact-SQL farkları

Uygulamaların kullandığı çoğu Transact-SQL özelliği hem Microsoft SQL Server hem de Azure SQL Veritabanı'nda tam olarak desteklenir. Örneğin, veri türleri, işleçler, dize, aritmetik, mantıksal ve imleç işlevleri gibi temel SQL bileşenleri SQL Server ve SQL Veritabanı'nda aynı şekilde çalışır. Ancak, DDL (veri tanımlı dil) ve DML (veri işleme dili) öğelerinde, yalnızca kısmen desteklenen T-SQL deyimleri ve sorgularla sonuçlanan birkaç T-SQL farkı vardır (bu makalede daha sonra tartışırız).
Buna ek olarak, Azure SQL Veritabanı özellikleri ana veritabanı ve işletim sistemine olan bağımlılıklardan yalıtmak üzere tasarlandığından, hiç desteklenmeyen bazı özellikler ve sözdizimi vardır. Bu nedenle, çoğu sunucu düzeyindeki etkinlikler SQL Veritabanı için uygun değildir. Sunucu düzeyindeki seçenekleri, işletim sistemi bileşenlerini yapılandırdıkları veya dosya sistemi yapılandırması belirtmeleri durumunda T-SQL deyimleri ve seçenekleri kullanılamaz. Bu tür özellikler gerektiğinde, sql veritabanından veya başka bir Azure özelliğinden veya hizmetinden uygun bir alternatif genellikle başka bir şekilde kullanılabilir.

Daha fazla bilgi için [bkz.](sql-database-transact-sql-information.md)

## <a name="security"></a>Güvenlik

SQL Veritabanı, uygulamanızın çeşitli güvenlik ve uyumluluk gereksinimlerini karşılamasına yardımcı olmak için bir dizi [yerleşik güvenlik](sql-database-security-overview.md) ve uyumluluk özelliği sağlar.

> [!IMPORTANT]
> Azure SQL Veritabanı (tüm dağıtım seçenekleri), bir dizi uyumluluk standardına göre onaylanmıştır. Daha fazla bilgi için, SQL Veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Microsoft Azure Güven Merkezi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Hızlı bir şekilde tek bir veritabanı ile başlamak için, [Tek veritabanı quickstart kılavuzu](sql-database-single-database-quickstart-guide.md)ile başlayın.
- BIR SQL Server veritabanını Azure'a geçirme hakkında bilgi edinmek [için](sql-database-single-database-migrate.md)bkz.
- Desteklenen özellikler hakkında bilgi için [bkz.](sql-database-features.md)
