---
title: Azure Synapse Analytics (eski adıyla SQL DW) nedir?
description: Azure Synapse Analytics (eski adıyla SQL DW), kurumsal veri depolama ve Büyük Veri analitiğini bir araya getiren sınırsız bir analiz hizmetidir.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: a9506d45350a567e3643b6edd6afc7668662f6e5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416020"
---
# <a name="what-is-azure-synapse-analytics-formerly-sql-dw"></a>Azure Synapse Analytics (eski adıyla SQL DW) nedir?

Azure Synapse, kurumsal veri depolama ve Büyük Veri analitiğini bir araya getiren bir analiz hizmetidir. Her ölçekte sunucusuz isteğe bağlı veya sağlanmış kaynakları kullanarak, kendi koşullarınızla verileri sorgulama özgürlüğü getirir. Azure Synapse, anında BI ve makine öğrenimi ihtiyaçları için verileri yutmak, hazırlamak, yönetmek ve sunmak için bu iki dünyayı birleşik bir deneyimle bir araya getirir.

Azure Synapse'nin dört bileşeni vardır:

- Synapse SQL: T-SQL tabanlı analizi tamamlayın – Genel Olarak Kullanılabilir
  - SQL havuzu (DWU başına ödeme)
  - İsteğe bağlı SQL (işlenen TB başına ödeme) – (Önizleme)
- Kıvılcım: Derinentegre Apache Spark (Önizleme)
- Synapse Pipelines: Karma veri entegrasyonu (Önizleme)
- Stüdyo: Birleşik kullanıcı deneyimi.  (Önizleme)

> [!NOTE]
> Azure Synapse'nin önizleme özelliklerine erişmek için [buradan](https://aka.ms/synapsepreview)erişim isteyin. Microsoft tüm istekleri ni triyaj ve en kısa sürede yanıt verecektir.
>
> Azure [Synapse önizleme belgelerini](../overview-what-is.md)görüntüleyin.

## <a name="synapse-sql-pool-in-azure-synapse"></a>Azure Synapse'de Synapse SQL havuzu

Synapse SQL havuzu, genellikle Azure Synapse'de bulunan kurumsal veri depolama özelliklerini ifade eder.

SQL havuzu, Synapse SQL kullanırken sağlanan analitik kaynakların bir koleksiyonunu temsil eder. SQL havuzunun boyutu Veri Depolama Birimleri (DWU) tarafından belirlenir.

Basit [PolyBase](/sql/relational-databases/polybase/polybase-guide?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) T-SQL sorgularıyla büyük verileri içe aktarın ve ardından yüksek performanslı analizleri çalıştırmak için MPP'nin gücünü kullanın. Siz tümleştirip analiz ettikçe, Synapse SQL havuzu işletmenizin daha hızlı ve daha sağlam öngörüler için güvenebileceği tek bir gerçek sürümü haline gelecektir.  

## <a name="key-component-of-a-big-data-solution"></a>Büyük bir veri çözümünün temel bileşeni

Veri depolama, bulut tabanlı, uçtan uca büyük veri çözümünün önemli bir bileşenidir.

![Veri ambarı çözümü](./media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png)

Bulut veri çözümünde veriler farklı veri kaynaklarından büyük veri depolarına alınır. Büyük veri depolarına giren veriler Hadoop, Spark ve makine öğrenimi algoritmaları tarafından hazırlanır ve eğitilir. Veriler karmaşık çözümleme için hazır olduğunda, Synapse SQL havuzu büyük veri depolarını sorgulamak için PolyBase'i kullanır. PolyBase, verileri Synapse SQL bilardo tablolarına getirmek için standart T-SQL sorgularını kullanır.

Synapse SQL havuzu verileri columnar depolama ile ilişkisel tablolarda depolar. Bu biçim veri depolama maliyetlerini önemli ölçüde düşürürken sorgu performansını artırır. Veriler depolandıktan sonra geniş ölçekte analiz işlemleri gerçekleştirebilirsiniz. Analizler geleneksel veri sistemlerine kıyasla dakikalar yerine saniyeler, günler yerine saatler içinde tamamlanır.

Analiz sonuçları dünya çapındaki raporlama veritabanlarına veya uygulamalarına iletilebilir. Ardından iş analistleri işlerle ilgili destekli kararlar almak üzere öngörü sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse mimarisini](massively-parallel-processing-mpp-architecture.md) keşfedin
- Hızlı [bir](create-data-warehouse-portal.md) şekilde bir SQL havuzu oluşturun
- [Örnek verileri yükleyin.](load-data-from-azure-blob-storage-using-polybase.md)
- [Videoları](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse) Keşfedin

Veya diğer Azure Synapse kaynaklarından bazılarına bakın.

- [Blogları](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/) Ara
- Özellik [istekleri gönderme](https://feedback.azure.com/forums/307516-sql-data-warehouse)
- [Destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md)
- [MSDN forumunu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse) ara
- [Arama Yığını Taşma forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)
