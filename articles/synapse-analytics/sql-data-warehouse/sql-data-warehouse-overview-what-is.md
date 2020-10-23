---
title: Azure SYNAPSE Analytics (eski adıyla SQL DW) nedir?
description: Azure SYNAPSE Analytics (eski adıyla SQL DW), kurumsal veri depolama ve büyük veri analizlerini birlikte getiren sınırsız bir analiz hizmetidir.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql-dw
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 0ec5b5a5ee5c29893f47560d57e78c281825453e
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92370398"
---
# <a name="what-is-azure-synapse-analytics-formerly-sql-dw"></a>Azure SYNAPSE Analytics (eski adıyla SQL DW) nedir?

> [!NOTE]
>[Azure Synapse (çalışma alanları Önizleme) belgelerini](../overview-what-is.md)inceleyin.
>

Azure SYNAPSE, kurumsal veri depolama ve büyük veri analizlerini birlikte getiren bir analiz hizmetidir. Her ölçekte sunucusuz isteğe bağlı veya sağlanmış kaynakları kullanarak, kendi koşullarınızla verileri sorgulama özgürlüğü getirir. Azure Synapse Analytics, anında BI ve makine öğrenmesi gereksinimleri için veri almanızı, hazırlamanızı, yönetmenizi ve sunmanızı sağlayan birleşik bir deneyimle bu iki dünyayı bir araya getirir.

Azure SYNAPSE dört bileşene sahiptir:

- SYNAPSE SQL: tam T-SQL tabanlı analiz – genel kullanıma sunuldu
  - SQL Havuzu (sağlanan DWU başına ödeme)
  - İstek üzerine SQL (işlenen TB başına ödeme) (Önizleme)
- Spark: derin tümleşik Apache Spark (Önizleme)
- SYNAPSE işlem hatları: karma veri tümleştirmesi (Önizleme)
- Studio: birleştirilmiş kullanıcı deneyimi. (önizleme)

## <a name="synapse-sql-pool-in-azure-synapse"></a>Azure SYNAPSE 'de SYNAPSE SQL havuzu

SYNAPSE SQL havuzu, Azure SYNAPSE 'de genel kullanıma sunulan kurumsal veri ambarı özelliklerini ifade eder.

SQL havuzu, SYNAPSE SQL kullanılırken sağlanmakta olan analitik kaynakların koleksiyonunu temsil eder. SQL havuzunun boyutu, veri ambarı birimleri (DWU) tarafından belirlenir.

Basit [PolyBase](/sql/relational-databases/polybase/polybase-guide?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) T-SQL sorgularıyla büyük verileri içeri aktarın ve ardından yüksek performanslı analiz ÇALıŞTıRMAK için MPP gücünü kullanın. Tümleştirdiğinizde ve analiz edilirken, SYNAPSE SQL havuzu, işletmenizin daha hızlı ve daha sağlam Öngörüler için üzerinde sayıgelebilmesi için tek bir sürümü olacaktır.  

## <a name="key-component-of-a-big-data-solution"></a>Büyük veri çözümünün anahtar bileşeni

Veri depolama, bulut tabanlı, uçtan uca büyük veri çözümünün temel bir bileşenidir.

![Veri ambarı çözümü](./media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png)

Bulut veri çözümlerinde, farklı kaynaklarda yer alan veriler büyük veri depolarına alınır. Büyük veri deposuna alınan veriler Hadoop, Spark ve makine öğrenmesi algoritmaları ile hazırlanır ve eğitilir. Veriler karmaşık Analize hazırlanmaya hazırsanız, SYNAPSE SQL havuzu büyük veri depolarını sorgulamak için PolyBase 'i kullanır. PolyBase, verileri SYNAPSE SQL havuzu tablolarına getirmek için Standart T-SQL sorgularını kullanır.

SYNAPSE SQL Pool, verileri sütunlu depolama ile ilişkisel tablolarda depolar. Bu biçim veri depolama maliyetlerini önemli ölçüde düşürürken sorgu performansını artırır. Veriler depolandıktan sonra geniş ölçekte analiz işlemleri gerçekleştirebilirsiniz. Analiz işlemleri geleneksel veritabanı sistemlerine kıyasla dakikalar yerine saniyeler veya günler yerine saatler içinde tamamlanır.

Analiz sonuçları dünya çapındaki raporlama veritabanlarına veya uygulamalarına iletilebilir. Ardından iş analistleri işlerle ilgili destekli kararlar almak üzere öngörü sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SYNAPSE mimarisini](massively-parallel-processing-mpp-architecture.md) keşfet
- Hızlıca [BIR SQL havuzu oluşturun](create-data-warehouse-portal.md)
- [Örnek verileri yükleyin](load-data-from-azure-blob-storage-using-polybase.md).
- [Videoları](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse) keşfet

Ya da diğer Azure SYNAPSE kaynaklarından bazılarına bakın.

- [Bloglarda](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/) ara
- [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse) gönderme
- [Destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md)
- [Soru sayfasında Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) arama
- [Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw) ara
