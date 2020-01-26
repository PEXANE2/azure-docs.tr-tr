---
title: Azure SYNAPSE Analytics (eski adıyla SQL DW) nedir?
description: Azure SYNAPSE Analytics (eski adıyla SQL DW), kurumsal veri depolama ve büyük veri analizlerini birlikte getiren sınırsız bir analiz hizmetidir.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 40f779fa5545e0e9c8fd1aa2cc2689b29dffdb82
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760768"
---
# <a name="what-is-azure-synapse-analytics-formerly-sql-dw"></a>Azure SYNAPSE Analytics (eski adıyla SQL DW) nedir?

Azure Synapse, kurumsal veri ambarı özellikleriyle Büyük Veri analizini bir araya getiren sınırsız bir analiz hizmetidir. Dilediğiniz ölçekteki sunucusuz, isteğe bağlı veya sağlanmış kaynakları kullanarak verileri istediğiniz gibi sorgulama özgürlüğü sağlar. Azure SYNAPSE, bu iki çalışma LDS 'yi, anında ve makine öğrenimi ihtiyaçları için veri alma, hazırlama, yönetme ve sunma konularında birleştirilmiş bir deneyimle birlikte sunar

Azure SYNAPSE dört bileşene sahiptir:
- SQL Analytics: tam T-SQL tabanlı analiz – genel kullanıma sunuldu
    - SQL Havuzu (sağlanan DWU başına ödeme) 
    - İstek üzerine SQL (işlenen TB başına ödeme) – (Önizleme)
- Spark: derin tümleşik Apache Spark (Önizleme) 
- Veri tümleştirme: karma veri tümleştirmesi (Önizleme)
- Studio: birleştirilmiş kullanıcı deneyimi.  (Önizleme)

> [!NOTE]
> Azure SYNAPSE 'ın önizleme özelliklerine erişmek için [buradan](https://aka.ms/synapsepreview)erişim isteyin. Microsoft tüm istekleri önceliklendirme ve mümkün olan en kısa sürede yanıt verir.

## <a name="sql-analytics-and-sql-pool-in-azure-synapse"></a>Azure 'da SQL Analytics ve SQL havuzu SYNAPSE

SQL Analytics, Azure SYNAPSE ile genel kullanıma sunulan kurumsal veri ambarı özelliklerini ifade eder. 

SQL havuzu, SQL Analytics kullanılırken sağlanmakta olan analitik kaynakların bir koleksiyonunu temsil eder. SQL havuzunun boyutu, veri ambarı birimleri (DWU) tarafından belirlenir.

Basit [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) T-SQL sorgularıyla büyük verileri içeri aktarın ve ardından yüksek performanslı analiz ÇALıŞTıRMAK için MPP gücünü kullanın. Tümleştirmeniz ve analiz etmeniz sırasında SQL Analytics, işletmenizin daha hızlı ve daha sağlam Öngörüler için üzerine gelebilmesi için tek bir Truth sürümü olacaktır.  

## <a name="key-component-of-a-big-data-solution"></a>Büyük veri çözümünün anahtar bileşeni

Veri depolama, bulut tabanlı, uçtan uca büyük veri çözümünün temel bir bileşenidir.

![Veri ambarı çözümü](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

Bulut veri çözümünde veriler farklı veri kaynaklarından büyük veri depolarına alınır. Büyük veri depolarına giren veriler Hadoop, Spark ve makine öğrenimi algoritmaları tarafından hazırlanır ve eğitilir. Veriler karmaşık Analize hazırlanmaya hazırsanız, SQL Analytics büyük veri depolarını sorgulamak için PolyBase 'i kullanır. PolyBase, verileri SQL Analytics tablolarına getirmek için Standart T-SQL sorgularını kullanır.
 
SQL Analytics, verileri sütunlu depolama ile ilişkisel tablolarda depolar. Bu biçim veri depolama maliyetlerini önemli ölçüde düşürürken sorgu performansını artırır. Veriler depolandıktan sonra Analytics 'i büyük ölçekli olarak çalıştırabilirsiniz. Analizler geleneksel veri sistemlerine kıyasla dakikalar yerine saniyeler, günler yerine saatler içinde tamamlanır. 

Analiz sonuçları dünya çapındaki raporlama veritabanlarına veya uygulamalarına iletilebilir. Ardından iş analistleri işlerle ilgili destekli kararlar almak üzere öngörü sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SYNAPSE mimarisini](massively-parallel-processing-mpp-architecture.md) keşfet
- Hızlıca [BIR SQL havuzu oluşturun](create-data-warehouse-portal.md)
- [Örnek verileri yükleyin](sql-data-warehouse-load-sample-databases.md).
- [Videoları](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse) keşfet

Ya da diğer Azure SYNAPSE kaynaklarından bazılarına bakın.  
* [Bloglarda](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/) ara
* [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse) gönderme
* [Destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md)
* [MSDN forumuna](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse) ara
* [Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw) ara