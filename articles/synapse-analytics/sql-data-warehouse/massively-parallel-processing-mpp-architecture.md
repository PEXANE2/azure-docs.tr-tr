---
title: Azure Synapse Analytics (eski adıyla SQL DW) mimarisi
description: Azure Synapse Analytics'in (eski adıyla SQL DW) yüksek performans ve ölçeklenebilirlik elde etmek için azure depolama ile büyük ölçüde paralel işleme (MPP) nasıl birleştirir öğrenin.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 0d768581b8f1a177190da08986af8b60b4861432
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80744206"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-architecture"></a>Azure Synapse Analytics (eski adıyla SQL DW) mimarisi

Azure Synapse kurumsal veri ambarı özellikleriyle Büyük Veri analizini bir araya getiren sınırsız bir analiz hizmetidir. Her ölçekte sunucusuz isteğe bağlı veya sağlanmış kaynakları kullanarak, kendi koşullarınızla verileri sorgulama özgürlüğü getirir. Azure Synapse, anında BI ve makine öğrenimi ihtiyaçları için verileri yutmak, hazırlamak, yönetmek ve sunmak için bu iki dünyayı birleşik bir deneyimle bir araya getirir.

 Azure Synapse'nin dört bileşeni vardır:

- SQL Analytics: T-SQL tabanlı analitik

  - SQL havuzu (DWU başına ödeme) – Genellikle Kullanılabilir
  - İsteğe bağlı SQL (işlenen TB başına ödeme) – (Önizleme)
- Kıvılcım: Derinentegre Apache Spark (Önizleme)
- Veri Tümleştirmesi: Karma veri tümleştirmesi (Önizleme)
- Stüdyo: birleşik kullanıcı deneyimi.  (Önizleme)

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="synapse-sql-mpp-architecture-components"></a>Synapse SQL MPP mimari bileşenleri

[Synapse SQL,](sql-data-warehouse-overview-what-is.md#synapse-sql-pool-in-azure-synapse) birden çok düğümüzerinden verilerin hesaplamalı işlemesini dağıtmak için ölçeklendirici bir mimariden yararlanır. Ölçek birimi, [veri ambarı birimi](what-is-a-data-warehouse-unit-dwu-cdwu.md)olarak bilinen bilgi işlem gücünün soyutlamasIdır. İşlem, bilgisayarınızdaki verilerden bağımsız olarak hesaplamayı ölçeklendirmenize olanak tanıyan depolamadan ayrıdır.

![Synapse SQL mimarisi](./media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SQL Analytics düğüm tabanlı bir mimari kullanır. Uygulamalar, T-SQL komutlarını SQL Analytics'in tek giriş noktası olan Bir Denetim düğümüne bağlar ve sorun. Denetim düğümü, paralel işleme sorgularını en iyi duruma getiren MPP altyapısını çalıştırır ve daha sonra işlerini paralel olarak yapmak için işlemleri İşlem düğümlerine geçirir.

İşlem düğümleri tüm kullanıcı verilerini Azure Depolama’da depolar ve paralel sorgular çalıştırır. Veri Taşıma Hizmeti (DMS), sorguları paralel olarak çalıştırmak ve doğru sonuçlar döndürmek için verileri düğümler arasında taşıyan, sistem düzeyindeki bir dahili hizmettir.

Ayrılmış depolama ve işlem ile, Synapse SQL havuzu kullanırken bir olabilir:

- Depolama gereksinimlerinize bakılmaksızın bağımsız boyut hesaplama gücü.
- Verileri taşımadan, bir SQL havuzu (veri ambarı) içinde bilgi işlem gücünü büyütün veya küçültün.
- Verileri olduğu gibi bırakıp işlem kapasitesini duraklatır, böylece yalnızca depolama için ödeme yaparsınız.
- Çalışma saatleri içinde işlem kapasitesini sürdürür.

### <a name="azure-storage"></a>Azure Storage

Synapse SQL, kullanıcı verilerinizi güvende tutmak için Azure Depolama'dan yararlanır.  Verileriniz Azure Depolama tarafından depolanıp yönetildiği için, depolama tüketiminiz için ayrı bir ücret ödenir. Veriler, sistemin performansını optimize etmek için **dağıtımlara** ayrılmıştır. Tabloyu tanımlarken verileri dağıtmak için hangi parçalama deseni kullanacağınızı seçebilirsiniz. Bu parçalama desenleri desteklenir:

- Karma
- Hepsini Bir Kez Deneme
- Çoğaltma

### <a name="control-node"></a>Denetim düğümü

Kontrol düğümü mimarinin beynidir. Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. MPP altyapısı paralel sorguları iyileştirmek ve koordine etmek için Denetim düğümü üzerinde çalışır. Bir T-SQL sorgusu gönderdiğinizde, Denetim düğümü onu her dağıtıma paralel çalışan sorgulara dönüştürür.

### <a name="compute-nodes"></a>İşlem düğümleri

İşlem düğümleri, hesaplama gücü sağlar. Dağıtımlar işleme için bilgi işlem düğümleri için eşleme. Daha fazla işlem kaynağı için ödeme yaptığınızda, dağıtımlar kullanılabilir İşlem düğümlerine yeniden eşlenir. İşlem düğümlerinin sayısı 1 ile 60 arasında değişir ve Synapse SQL için hizmet düzeyine göre belirlenir.

Her İşlem düğümünde sistem görünümlerinde görünen bir düğüm kimliği vardır. Adları sys.pdw_nodes ile başlayan sistem görünümlerinde node_id sütununa bakarak İşlem düğümü kimliğini görebilirsiniz. Bu sistem görünümlerinin bir listesi için [MPP sistem görünümlerini görün.](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

### <a name="data-movement-service"></a>Veri Taşıma Hizmeti

Veri Hareketi Hizmeti (DMS), Bilgi İşlem düğümleri arasındaki veri hareketini koordine eden veri aktarım teknolojisidir. Bazı sorgular, paralel sorguların doğru sonuçları döndürmesini sağlamak için veri hareketi gerektirir. Veri hareketi gerektiğinde, DMS doğru verilerin doğru konuma ulaştığından emin olur.

## <a name="distributions"></a>Dağıtım

Dağıtım, dağıtılmış veriler üzerinde çalışan paralel sorgular için temel depolama ve işleme birimidir. SQL Analytics bir sorgu çalıştırdığında, çalışma paralel olarak çalışan 60 küçük sorguya ayrılır.

60 küçük sorgunun her biri veri dağıtımlarından birinde çalışır. Her İşlemci düğümü 60 dağıtımdan birini veya daha fazlasını yönetir. Maksimum işlem kaynağına sahip bir SQL havuzunun Işlem düğümü başına bir dağıtımı vardır. En az işlem kaynaklarına sahip bir SQL havuzu, tek bir işlem düğümündeki tüm dağılımlara sahiptir.  

## <a name="hash-distributed-tables"></a>Karma dağıtılmış tablolar

Karma dağıtılmış bir tablo, büyük tablolarsa birleştirmeler ve toplamalar için en yüksek sorgu performansını sunabilir.

Karma dağıtılmış bir tablo, büyük tablolarsa birleştirmeler ve toplamalar için en yüksek sorgu performansını sunabilir.

Verileri karma dağıtılmış bir tabloya parçalamak için, her satırı deterministically bir dağıtıma atamak için karma işlev kullanılır. Tablo tanımında sütunlardan biri dağıtım sütunu olarak atanır. Karma işlevi, her satırı bir dağıtıma atamak için dağıtım sütunundaki değerleri kullanır.

Aşağıdaki diyagram, tam (dağıtılmamış tablonun) karma dağıtılmış tablo olarak nasıl depolandığını göstermektedir.

![Dağıtılmış tablo](./media/massively-parallel-processing-mpp-architecture/hash-distributed-table.png "Dağıtılmış tablo")  

- Her satır bir dağıtıma aittir.  
- Deterministik karma algoritmaher satırı bir dağıtıma atar.  
- Dağıtı başına tablo satırlarının sayısı tabloların farklı boyutlarına göre gösterildiği gibi değişir.

Bir dağıtım sütununun seçimi için fark, veri eğriliği ve sistemde çalışan sorgu türleri gibi performans konuları vardır.

## <a name="round-robin-distributed-tables"></a>Hepsini bir kez deneme dağıtılmış tabloları

Round-robin tablo oluşturmak için en basit tablo ve yükler için bir evreleme tablosu olarak kullanıldığında hızlı performans sunar.

Hepsini bir kez deneme dağıtılmış tablosu, verileri tabloda eşit olarak dağıtır ancak başka bir iyileştirme yapmaz. Bir dağılım önce rasgele seçilir ve ardından satır arabellekleri sırayla dağılımlara atanır. Verileri hepsini bir kez deneme tablosuna yüklemek hızlı bir işlemdir ancak sorgu performansı genellikle karma dağıtılmış tablolar ile daha iyi olabilir. Round-robin tablolarına katılırsa, verileri değiştirme gerekir ve bu da ek zaman alır.

## <a name="replicated-tables"></a>Çoğaltılmış Tablolar

Çoğaltılmış bir tablo, küçük tablolar için en hızlı sorgu performansını sağlar.

Çoğaltılan bir tablo, her işlem düğümünde tablonun tam bir kopyasını önbelleğe alar. Sonuç olarak, bir tablo çoğaltıldığında bir birleştirme veya toplama öncesinde işlem düğümleri arasında verileri aktarma gereksinimi ortadan kalkar. Çoğaltılmış tablolar, küçük tablolar ile en iyi şekilde kullanılır. Extra storage is required and there is additional overhead that is incurred when writing data, which make large tables impractical.  

Aşağıdaki diyagram, her bir işlem düğümünde ilk dağıtımda önbelleğe alınmış çoğaltılmış bir tabloyu gösterir.  

![Çoğaltılmış tablo](./media/massively-parallel-processing-mpp-architecture/replicated-table.png "Çoğaltılmış tablo")

## <a name="next-steps"></a>Sonraki adımlar

Azure Synapse hakkında biraz bilgi edindiğinize göre, hızlı bir şekilde [SQL havuzu oluşturmayı](create-data-warehouse-portal.md) ve [örnek verileri nasıl yükleyin](load-data-from-azure-blob-storage-using-polybase.md)öğrenin. Azure’da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü](../../azure-glossary-cloud-terminology.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) yararlı bulabilirsiniz. Veya diğer Azure Synapse Kaynaklarından bazılarına bakın.  

- [Başarı hikayeleri](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Bloglar](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse)
- [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Destek bileti oluşturun](sql-data-warehouse-get-started-create-support-ticket.md)
- [MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse)
- [Stack Overflow forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
