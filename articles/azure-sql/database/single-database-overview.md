---
title: Tek bir veritabanı nedir?
description: Azure SQL veritabanı 'nda tek veritabanı kaynak türü hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 04/08/2019
ms.openlocfilehash: 8f92fe8e4a4ebbc2d970bf28e415859249b9f67c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84343328"
---
# <a name="what-is-a-single-database-in-azure-sql-database"></a>Azure SQL veritabanı 'nda tek bir veritabanı nedir?
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Tek veritabanı kaynak türü, Azure SQL veritabanı 'nda kendi kaynak kümesiyle bir veritabanı oluşturur ve bir [sunucu](logical-servers.md)aracılığıyla yönetilir. Tek bir veritabanı ile, her veritabanı yalıtılmış ve taşınabilir. Her birinin [DTU tabanlı satın alma modeli](service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](service-tiers-vcore.md) ve garantili bir işlem boyutu içinde kendi hizmet katmanı vardır.

> [!IMPORTANT]
> Tek veritabanı, Azure SQL veritabanı için bir kaynak türüdür. Diğer [elastik havuzlarıdır](elastic-pool-overview.md).

## <a name="dynamic-scalability"></a>Dinamik ölçeklenebilirlik

İlk uygulamanızı sunucusuz işlem katmanında düşük maliyetli küçük, tek bir veritabanında veya sağlanan işlem katmanındaki küçük bir işlem boyutu üzerinde oluşturabilirsiniz. [İşlem veya hizmet katmanını](single-database-scale.md) , çözümünüzün ihtiyaçlarını karşılamak üzere dilediğiniz zaman el ile veya programlama yoluyla değiştirirsiniz. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

## <a name="single-databases-and-elastic-pools"></a>Tek veritabanları ve elastik havuzlar

Tek bir veritabanı, kaynak paylaşımı için [elastik havuzun](elastic-pool-overview.md) içine veya dışına taşınabilir. Tek veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Elastik havuzlar bu sorunu çözmek için tasarlanmıştır. Esnek havuzların işleyiş mantığı gayet basittir. Performans kaynaklarını tek bir veritabanı yerine bir havuza ayırır ve tek veritabanı performansı yerine havuzun ortak performans kaynakları için ödeme yaparsınız.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Yerleşik [performans izleme](performance-guidance.md) ve [Uyarı araçlarını](alerts-insights-configure-portal.md), performans derecelendirmeleri ile birlikte kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre ölçek büyütme veya küçültme işlemlerinin etkisini hızlı bir şekilde değerlendirebilirsiniz. Ayrıca, SQL veritabanı daha kolay izleme için [ölçümleri ve kaynak günlüklerini yayabilir](metrics-diagnostic-telemetry-logging-streaming-export-configure.md) .

## <a name="availability-capabilities"></a>Kullanılabilirlik özellikleri

Tek veritabanları ve elastik havuzlar birçok kullanılabilirlik özelliği sağlar. Bilgi için bkz. [kullanılabilirlik özellikleri](sql-database-paas-overview.md#availability-capabilities).

## <a name="transact-sql-differences"></a>Transact-SQL farkları

Uygulamaların kullandığı çoğu Transact-SQL özelliği hem Microsoft SQL Server hem de Azure SQL veritabanı 'nda tam olarak desteklenmektedir. Örneğin, veri türleri, işleçler, dize, aritmetik, mantıksal ve imleç işlevleri gibi çekirdek SQL bileşenleri, SQL Server ve SQL veritabanı 'nda aynı şekilde çalışır. Bununla birlikte, DDL (veri tanımlama dili) ve DML (veri işleme dili) öğelerinde yalnızca kısmen desteklenen (Bu makalede daha sonra tartıştığımız) bir T-SQL farkı vardır.

Ayrıca, Azure SQL veritabanı, özellikleri ana veritabanı ve işletim sistemi bağımlılıklarından yalıtmak üzere tasarlandığından, desteklenmeyen bazı özellikler ve söz dizimi vardır. Bu nedenle, çoğu sunucu düzeyi etkinlik SQL veritabanı için uygun değildir. T-SQL deyimleri ve seçenekleri, sunucu düzeyi seçeneklerini yapılandırır, işletim sistemi bileşenlerini yapılandırır veya dosya sistemi yapılandırması ' nı belirtirseniz kullanılamaz. Bu tür özellikler gerektiğinde, SQL veritabanından veya başka bir Azure özelliğinden ya da hizmetinden başka bir şekilde, uygun bir alternatif vardır.

Daha fazla bilgi için bkz. [SQL veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](transact-sql-tsql-differences-sql-server.md).

## <a name="security"></a>Güvenlik

SQL veritabanı, uygulamanızın çeşitli güvenlik ve uyumluluk gereksinimlerini karşılamasına yardımcı olmak için bir dizi [yerleşik güvenlik ve uyumluluk](security-overview.md) özelliği sağlar.

> [!IMPORTANT]
> Azure SQL veritabanı, bir dizi uyumluluk standartlarına karşı sertifikalandırilmiştir. Daha fazla bilgi için, SQL veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Microsoft Azure Güven Merkezi](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)' ne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanını hızlıca kullanmaya başlamak için [tek veritabanı hızlı başlangıç kılavuzu](quickstart-content-reference-guide.md)ile başlayın.
- SQL Server bir veritabanını Azure 'a geçirme hakkında bilgi edinmek için bkz. [Azure SQL veritabanı 'Na geçiş](migrate-to-database-from-sql-server.md).
- Desteklenen özellikler hakkında bilgi için bkz. [Özellikler](features-comparison.md).
