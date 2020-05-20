---
title: Azure SYNAPSE Analytics (eski adıyla SQL DW) mimarisi
description: Azure SYNAPSE Analytics 'in (eski adıyla SQL DW), yüksek performans ve ölçeklenebilirlik elde etmek için Azure depolama ile çok büyük bir paralel işleme (MPP) birleştirdiğini öğrenin.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: d57f02b9aff56c83aa1c12bd441df2863f6d6fa7
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83658492"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-architecture"></a>Azure SYNAPSE Analytics (eski adıyla SQL DW) mimarisi

Azure Synapse kurumsal veri ambarı özellikleriyle Büyük Veri analizini bir araya getiren sınırsız bir analiz hizmetidir. Her ölçekte sunucusuz isteğe bağlı veya sağlanmış kaynakları kullanarak, kendi koşullarınızla verileri sorgulama özgürlüğü getirir. Azure SYNAPSE, bu iki çalışma LDS 'yi, anında ve makine öğrenimi ihtiyaçları için veri alma, hazırlama, yönetme ve sunma konularında birleştirilmiş bir deneyimle birlikte sunar.

 Azure SYNAPSE dört bileşene sahiptir:

- SYNAPSE SQL: tam T-SQL tabanlı analiz

  - SQL Havuzu (sağlanan DWU başına ödeme) – genel olarak kullanılabilir
  - İstek üzerine SQL (işlenen TB başına ödeme) – (Önizleme)
- Spark: derin tümleşik Apache Spark (Önizleme)
- Veri tümleştirme: karma veri tümleştirmesi (Önizleme)
- Studio: birleştirilmiş kullanıcı deneyimi.  (Önizleme)

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="synapse-sql-mpp-architecture-components"></a>SYNAPSE SQL MPP mimari bileşenleri

[SYNAPSE SQL](sql-data-warehouse-overview-what-is.md#synapse-sql-pool-in-azure-synapse) , verilerin birden çok düğüm arasında işlem işlemesini dağıtmak için bir genişleme mimarisinden yararlanır. Ölçek birimi, [veri ambarı birimi](what-is-a-data-warehouse-unit-dwu-cdwu.md)olarak bilinen bir işlem gücü soyutlamasıdır. İşlem depolama alanından ayrıdır ve bu işlem, sisteminizi sisteminizdeki verilerden bağımsız olarak ölçeklendirmenizi sağlar.

![Synapse SQL mimarisi](./media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SYNAPSE SQL, düğüm tabanlı bir mimari kullanır. Uygulamalar, T-SQL komutlarını, SYNAPSE SQL için tek giriş noktası olan bir denetim düğümüne bağlanır ve bu komutlara verebilir. Denetim düğümü, paralel işleme için sorguları en iyi duruma getirir ve sonra işlerini paralel olarak yapmak üzere işlem düğümlerine geçirir.

İşlem düğümleri tüm kullanıcı verilerini Azure Depolama’da depolar ve paralel sorgular çalıştırır. Veri Taşıma Hizmeti (DMS), sorguları paralel olarak çalıştırmak ve doğru sonuçlar döndürmek için verileri düğümler arasında taşıyan, sistem düzeyindeki bir dahili hizmettir.

Ayrılmış depolama ve işlem ile, SYNAPSE SQL havuzu bir arada kullanıldığında şunları yapabilirsiniz:

- Depolama gereksinimlerinizin ne olursa olsun bağımsız işlem gücü.
- Veri taşımadan bir SQL Havuzu (veri ambarı) içinde işlem gücünü büyütün veya küçültün.
- Verileri olduğu gibi bırakıp işlem kapasitesini duraklatır, böylece yalnızca depolama için ödeme yaparsınız.
- Çalışma saatleri içinde işlem kapasitesini sürdürür.

### <a name="azure-storage"></a>Azure Depolama

SYNAPSE SQL, kullanıcı verilerinizi güvende tutmak için Azure Storage 'ı kullanır.  Verileriniz Azure depolama tarafından depolandığından ve yönetildiğinden, depolama tüketiminize yönelik ayrı bir ücret alınır. Veriler, sistem performansını iyileştirmek için **dağıtımlarla** birleştirilir. Tabloyu tanımlarken verileri dağıtmak için kullanılacak parçalama düzeninin arasından seçim yapabilirsiniz. Bu parçalı desenler desteklenir:

- Karma
- Hepsini Bir Kez Deneme
- Çoğaltma

### <a name="control-node"></a>Denetim düğümü

Denetim düğümü, mimarinin beyinidir. Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. MPP altyapısı paralel sorguları iyileştirmek ve koordine etmek için Denetim düğümü üzerinde çalışır. Bir T-SQL sorgusu gönderdiğinizde, denetim düğümü onu paralel olarak her dağıtıma karşı çalışan sorgulara dönüştürür.

### <a name="compute-nodes"></a>İşlem düğümleri

İşlem düğümleri, hesaplama gücü sağlar. Dağıtım, işlenmek üzere Işlem düğümlerine eşlenir. Daha fazla işlem kaynağı için ödeme yaparken, dağıtımlar kullanılabilir Işlem düğümlerine yeniden eşlenir. İşlem düğümlerinin sayısı 1 ile 60 arasında değişir ve SYNAPSE SQL hizmet düzeyiyle belirlenir.

Her Işlem düğümünün sistem görünümlerinde görünür bir düğüm KIMLIĞI vardır. Adları sys. pdw_nodes ile başlayan sistem görünümlerindeki node_id sütununa bakarak Işlem düğümü KIMLIĞINI görebilirsiniz. Bu sistem görünümlerinin listesi için bkz. [MPP sistem görünümleri](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="data-movement-service"></a>Veri Taşıma Hizmeti

Veri taşıma hizmeti (DMS), Işlem düğümleri arasında veri hareketini koordine eden veri aktarımı teknolojisidir. Bazı sorgular paralel sorguların doğru sonuçlar döndürmesini sağlamak için veri hareketi gerektirir. Veri taşıma gerektiğinde, DMS doğru verinin doğru konuma gelmesini sağlar.

## <a name="distributions"></a>Dağıtımları

Dağıtım, dağıtılmış veriler üzerinde çalışan paralel sorgular için temel depolama ve işleme birimidir. SYNAPSE SQL bir sorgu çalıştırdığında, iş paralel olarak çalışan 60 daha küçük sorguya bölünür.

60 küçük bir sorgu, veri dağıtımlarından birinde çalışır. Her Işlem düğümü bir veya daha fazla 60 dağıtımlarını yönetir. En fazla işlem kaynağına sahip bir SQL havuzunda Işlem düğümü başına bir dağıtım vardır. En düşük işlem kaynaklarına sahip bir SQL havuzunda tek bir işlem düğümündeki tüm dağıtımlar vardır.  

## <a name="hash-distributed-tables"></a>Karma dağıtılmış tablolar

Karma dağıtılmış bir tablo, büyük tablolarsa birleştirmeler ve toplamalar için en yüksek sorgu performansını sunabilir.

Verileri karma olarak dağıtılmış bir tabloya bölmek için, her bir satırı bir dağıtıma göre kesin şekilde atamak üzere bir karma işlev kullanılır. Tablo tanımında sütunlardan biri dağıtım sütunu olarak atanır. Karma işlevi, her satırı bir dağıtıma atamak için dağıtım sütunundaki değerleri kullanır.

Aşağıdaki diyagramda, tam (dağıtılmamış bir tablonun) karma olarak dağıtılan tablo olarak nasıl depolandığı gösterilmektedir.

![Dağıtılmış tablo](./media/massively-parallel-processing-mpp-architecture/hash-distributed-table.png "Dağıtılmış tablo")  

- Her satır bir dağıtıma aittir.  
- Belirleyici bir karma algoritması her satırı bir dağıtıma atar.  
- Dağıtım başına tablo satırı sayısı, farklı tablo boyutları tarafından gösterilen şekilde değişir.

Bir dağıtım sütununun seçimi için, ayrımlık, veri eğriltme ve sistemde çalışan sorgu türleri gibi performans konuları vardır.

## <a name="round-robin-distributed-tables"></a>Hepsini bir kez deneme dağıtılmış tabloları

Hepsini bir kez deneme tablosu, yüklemeler için hazırlama tablosu olarak kullanıldığında hızlı performans oluşturmak ve sağlamak için en basit tablodur.

Hepsini bir kez deneme dağıtılmış tablosu, verileri tabloda eşit olarak dağıtır ancak başka bir iyileştirme yapmaz. Önce bir dağıtım rastgele seçilir ve sonra, satır arabellekleri dağıtıma sırayla atanır. Verileri hepsini bir kez deneme tablosuna yüklemek hızlı bir işlemdir ancak sorgu performansı genellikle karma dağıtılmış tablolar ile daha iyi olabilir. Hepsini bir kez deneme tablolarında birleşimler, ek zaman alan reshuffling verileri gerektirir.

## <a name="replicated-tables"></a>Çoğaltılmış Tablolar

Çoğaltılmış bir tablo, küçük tablolar için en hızlı sorgu performansını sağlar.

Çoğaltılan bir tablo, her işlem düğümündeki tablonun tam bir kopyasını önbelleğe alır. Sonuç olarak, bir tablo çoğaltıldığında bir birleştirme veya toplama öncesinde işlem düğümleri arasında verileri aktarma gereksinimi ortadan kalkar. Çoğaltılmış tablolar, küçük tablolar ile en iyi şekilde kullanılır. Ek depolama alanı gereklidir ve büyük tabloları pratik hale getirmek için veri yazarken tahakkuk eden ek yük vardır.  

Aşağıdaki diyagramda, her işlem düğümündeki ilk dağıtım üzerinde önbelleğe alınmış bir çoğaltılan tablo gösterilmektedir.  

![Çoğaltılmış tablo](./media/massively-parallel-processing-mpp-architecture/replicated-table.png "Çoğaltılmış tablo")

## <a name="next-steps"></a>Sonraki adımlar

Azure SYNAPSE hakkında biraz bilgi sahibi olduğunuza göre hızlıca [BIR SQL havuzu oluşturma](create-data-warehouse-portal.md) ve [örnek verileri yükleme](load-data-from-azure-blob-storage-using-polybase.md)hakkında bilgi edinin. Azure’da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü](../../azure-glossary-cloud-terminology.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) yararlı bulabilirsiniz. Ya da diğer Azure SYNAPSE kaynaklarından bazılarına bakın.  

- [Başarı hikayeleri](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Bloglar](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse)
- [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Destek bileti oluşturun](sql-data-warehouse-get-started-create-support-ticket.md)
- [MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse)
- [Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
