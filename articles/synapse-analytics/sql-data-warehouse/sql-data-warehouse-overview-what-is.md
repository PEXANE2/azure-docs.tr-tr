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
ms.openlocfilehash: a118d028cc85eb858dd0dc1fa6d5d2268f7db43b
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350399"
---
# <a name="what-is-azure-synapse-analytics-formerly-sql-dw"></a>Azure Synapse Analytics (eski adıyla SQL DW) nedir?

Azure Synapse kurumsal veri ambarı özellikleriyle Büyük Veri analizini bir araya getiren sınırsız bir analiz hizmetidir. Her ölçekte sunucusuz isteğe bağlı veya sağlanmış kaynakları kullanarak, kendi koşullarınızla verileri sorgulama özgürlüğü getirir. Azure Synapse, anında BI ve makine öğrenimi ihtiyaçları için verileri yutmak, hazırlamak, yönetmek ve sunmak için bu iki dünyayı birleşik bir deneyimle bir araya getiriyor

Azure Synapse'nin dört bileşeni vardır:
- SQL Analytics: T-SQL tabanlı analitik - Genel Olarak Kullanılabilir
    - SQL havuzu (DWU başına ödeme) 
    - İsteğe bağlı SQL (işlenen TB başına ödeme) – (Önizleme)
- Kıvılcım: Derinentegre Apache Spark (Önizleme) 
- Veri Tümleştirmesi: Karma veri tümleştirmesi (Önizleme)
- Stüdyo: Birleşik kullanıcı deneyimi.  (Önizleme)

> [!NOTE]
> Azure Synapse'nin önizleme özelliklerine erişmek için [buradan](https://aka.ms/synapsepreview)erişim isteyin. Microsoft tüm istekleri ni triyaj ve en kısa sürede yanıt verecektir.

## <a name="sql-analytics-and-sql-pool-in-azure-synapse"></a>Azure Synapse'de SQL Analytics ve SQL havuzu

SQL Analytics, genellikle Azure Synapse'de bulunan kurumsal veri depolama özelliklerini ifade eder. 

SQL havuzu, SQL Analytics kullanırken sağlanan analitik kaynakların bir koleksiyonunu temsil eder. SQL havuzunun boyutu Veri Depolama Birimleri (DWU) tarafından belirlenir.

Basit [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) T-SQL sorgularıyla büyük verileri içe aktarın ve ardından yüksek performanslı analizleri çalıştırmak için MPP'nin gücünü kullanın. Siz tümleştirip analiz ettikçe, SQL Analytics, işletmenizin daha hızlı ve daha sağlam öngörüler için güvenebileceği tek bir doğruluk sürümü haline gelecektir.  

## <a name="key-component-of-a-big-data-solution"></a>Büyük bir veri çözümünün temel bileşeni

Veri depolama, bulut tabanlı, uçtan uca büyük veri çözümünün önemli bir bileşenidir.

![Veri ambarı çözümü](./media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

Bulut veri çözümünde veriler farklı veri kaynaklarından büyük veri depolarına alınır. Büyük veri depolarına giren veriler Hadoop, Spark ve makine öğrenimi algoritmaları tarafından hazırlanır ve eğitilir. Veriler karmaşık analiz için hazır olduğunda SQL Analiz, PolyBase'i kullanarak büyük veri depolarını sorgular. PolyBase, verileri SQL Analiz tablolarına getirmek için standart T-SQL sorgularını kullanır.
 
SQL Analytics verileri sütunlar halindeki ilişkisel tablolarda depolar. Bu biçim veri depolama maliyetlerini önemli ölçüde düşürürken sorgu performansını artırır. Veriler depolandıktan sonra geniş ölçekte analiz işlemleri gerçekleştirebilirsiniz. Analizler geleneksel veri sistemlerine kıyasla dakikalar yerine saniyeler, günler yerine saatler içinde tamamlanır. 

Analiz sonuçları dünya çapındaki raporlama veritabanlarına veya uygulamalarına iletilebilir. Ardından iş analistleri işlerle ilgili destekli kararlar almak üzere öngörü sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse mimarisini](massively-parallel-processing-mpp-architecture.md) keşfedin
- Hızlı [bir](create-data-warehouse-portal.md) şekilde bir SQL havuzu oluşturun
- [Örnek verileri yükleyin.](load-data-from-azure-blob-storage-using-polybase.md)
- [Videoları](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse) Keşfedin

Veya diğer Azure Synapse kaynaklarından bazılarına bakın.  
* [Blogları](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/) Ara
* Özellik [istekleri gönderme](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md)
* [MSDN forumunu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse) ara
* [Arama Yığını Taşma forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)
