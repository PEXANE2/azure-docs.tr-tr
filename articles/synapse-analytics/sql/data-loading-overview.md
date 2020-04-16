---
title: ETL yerine Synapse SQL havuzu için ELT tasarla | Microsoft Dokümanlar
description: ETL yerine, veri veya SQL havuzu yüklemek için bir Extract, Load ve Transform (ELT) işlemi tasarlayın.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 596f4bcf2e3f829430fdc90eb1806a44a84b2bc5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429596"
---
# <a name="designing-a-polybase-data-loading-strategy-for-azure-synapse-sql-pool"></a>Azure Synapse SQL havuzu için Bir PolyBase veri yükleme stratejisi tasarlama

Geleneksel SMP veri ambarları, veri yüklemek için ayıklama, dönüştürme ve yükleme (ETL) işlemini kullanır. Azure SQL havuzu, bilgi işlem ve depolama kaynaklarının ölçeklenebilirliğinden ve esnekliğinden yararlanan büyük ölçüde paralel bir işleme (MPP) mimarisidir. Bir Ayıklama, Yükleme ve Dönüştürme (ELT) işleminden yararlanmak MPP'den yararlanabilir ve yüklemeden önce verileri dönüştürmek için gereken kaynakları ortadan kaldırabilir.

SQL havuzu, BCP ve SQL BulkCopy API gibi Polybase olmayan seçenekler de dahil olmak üzere birçok yükleme yöntemini desteklerken, yükleme tarihi için en hızlı ve en ölçeklenebilir yol PolyBase'tir.  PolyBase, Azure Blob depolama veya Azure Veri Gölü Deposu'nda depolanan harici verilere T-SQL dili üzerinden erişen bir teknolojidir.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>ELT nedir?

Ayıklama, Yükleme ve Dönüştürme (ELT), verilerin kaynak sistemden ayıklandığı, veri ambarına yüklendiği ve sonra dönüştürüldüğü bir işlemdir.

SQL havuzu için Bir PolyBase ELT uygulamak için temel adımlar şunlardır:

1. Kaynak verileri metin dosyalarına ayıklayın.
2. Verileri Azure Blob depolama alanına veya Azure Veri Gölü Deposu'na aktarın.
3. Verileri yüklemeye hazırlayın.
4. PolyBase kullanarak verileri SQL havuz hazırlama tablolarına yükleyin.
5. Verileri dönüştürün.
6. Verileri üretim tablolarına ekleyin.

Yükleme öğreticisi için Azure [blob depolamasından Azure SQL Veri Ambarı'na veri yüklemek için PolyBase'i kullan'a](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

Daha fazla bilgi için yükleme [desenleri bloguna](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/)bakın.

## <a name="1-extract-the-source-data-into-text-files"></a>1. Kaynak verileri metin dosyalarına ayıklama

Verileri kaynak sisteminizden çıkarmak depolama konumuna bağlıdır.  Amaç, verileri PolyBase destekli sınırlandırılmış metin dosyalarına taşımaktır.

### <a name="polybase-external-file-formats"></a>PolyBase harici dosya biçimleri

PolyBase, UTF-8 ve UTF-16 kodlanmış sınırlandırılmış metin dosyalarından veri yükler. Sınırlı metin dosyalarına ek olarak, Hadoop dosya biçimleri RC File, ORC ve Parke yükler. PolyBase ayrıca Gzip ve Snappy sıkıştırılmış dosyalardan veri yükleyebilir. PolyBase şu anda genişletilmiş ASCII, sabit genişlik biçimi ve WinZip, JSON ve XML gibi iç içe biçimleri desteklememektedir.

SQL Server'dan dışa aktarıyorsanız, verileri sınırlı metin dosyalarına aktarmak için [bcp komut satırı aracını](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanabilirsiniz. PARKe-SQL DW veri türü eşleme aşağıdaki gibidir:

| **Parke Veri Türü** |                      **SQL Veri Türü**                       |
| :-------------------: | :----------------------------------------------------------: |
|        tinyint        |                           tinyint                            |
|       smallint        |                           smallint                           |
|          int          |                             int                              |
|        bigint         |                            bigint                            |
|        boole        |                             bit                              |
|        double         |                            float                             |
|         float         |                             gerçek                             |
|        double         |                            Para                             |
|        double         |                          Smallmoney                          |
|        string         |                            Nchar                             |
|        string         |                           nvarchar                           |
|        string         |                             char                             |
|        string         |                           varchar                            |
|        ikili         |                            ikili                            |
|        ikili         |                          Varbinary                           |
|       timestamp       |                             date                             |
|       timestamp       |                        Smalldatetime                         |
|       timestamp       |                          datetime2                           |
|       timestamp       |                           datetime                           |
|       timestamp       |                             time                             |
|       date            |                             date                             |
|        decimal        |                            decimal                           |

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Verileri Azure Blob depolama veya Azure Veri Gölü Deposu'na aktarın

Verileri Azure depolama alanına bgetirmek için Azure [Blob depolamasına](../../storage/blobs/storage-blobs-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) veya [Azure Veri Gölü Deposu'na](../../data-lake-store/data-lake-store-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)taşıyabilirsiniz. Her iki konumda da veriler metin dosyalarında depolanmalıdır. PolyBase her iki konumdan da yüklenebilir.

Verileri Azure Depolama'ya taşımak için kullanabileceğiniz araçlar ve hizmetler:

- [Azure ExpressRoute](../../expressroute/expressroute-introduction.md) hizmeti ağ iş buzunu, performansını ve öngörülebilirliği artırır. ExpressRoute, verilerinizi Azure'a özel bir özel bağlantı üzerinden yönlendiren bir hizmettir. ExpressRoute bağlantıları verileri genel internet üzerinden yönlendirmez. Bağlantılar, genel internet üzerinden tipik bağlantılara göre daha fazla güvenilirlik, daha hızlı hızlar, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar.
- [AZCopy yardımcı programı](../../storage/common/storage-use-azcopy-v10.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) verileri genel internet üzerinden Azure Depolama'ya taşır. Veri boyutlarınız 10 TB'den küçükse bu işe yarar. AZCopy ile düzenli olarak yük gerçekleştirmek için, kabul edilebilir olup olmadığını görmek için ağ hızını test edin.
- [Azure Veri Fabrikası'nda (ADF)](../../data-factory/introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) yerel sunucunuza yükleyebileceğiniz bir ağ geçidi vardır. Ardından, verileri yerel sunucunuzdan Azure Depolama'ya taşımak için bir ardışık kaynak oluşturabilirsiniz. SQL havuzuna sahip Veri Fabrikası'nı kullanmak için, [verileri SQL havuzuna yükle'ye](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

## <a name="3-prepare-the-data-for-loading"></a>3. Verileri yükleme için hazırlama

SQL havuzuna yüklemeden önce depolama hesabınızdaki verileri hazırlamanız ve temizlemeniz gerekebilir. Verileriniz kaynaktayken, verileri metin dosyalarına dışa aktarırken veya veriler Azure Depolama'da yken veri hazırlama işlemi yapılabilir.  Verilerle mümkün olduğunca erken çalışmak en kolayıdır.  

### <a name="define-external-tables"></a>Dış tabloları tanımlama

Verileri yükleyebilmeniz için veri ambarınızdaki dış tabloları tanımlamanız gerekir. PolyBase, Azure Depolama'daki verileri tanımlamak ve erişmek için harici tablolar kullanır. Harici tablo veritabanı görünümüne benzer. Dış tablo tablo şemasını içerir ve veri ambarı dışında depolanan verilere işaret eder.

Dış tabloların tanımlanması, veri kaynağını, metin dosyalarının biçimini ve tablo tanımlarını belirtmeyi içerir. Bunlar, gereksinim edeceğiniz T-SQL sözdizimi konularıdır:

- [DıŞ VERI KAYNAĞı OLUŞTURMA](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [DıŞ TABLO OLUŞTURMA](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

Dış nesneler oluşturma örneği için, yükleme öğreticisinde [dış tablolar oluştur](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-external-tables-for-the-sample-data) adımına bakın.

### <a name="format-text-files"></a>Metin dosyalarını biçimlendirme

Dış nesneler tanımlandıktan sonra, metin dosyalarının satırlarını dış tablo ve dosya biçimi tanımıyla hizalamanız gerekir. Metin dosyasının her satırındaki veriler tablo tanımıyla hizalanmalıdır.
Metin dosyalarını biçimlendirmek için:

- Verileriniz ilişkisel olmayan bir kaynaktan geliyorsa, verilerinizi satır ve sütunlara dönüştürmeniz gerekir. Veriler ilişkisel veya ilişkisel olmayan bir kaynaktan olsun, veriler, verileri yüklemeyi planladığınız tablonun sütun tanımlarıyla hizalanacak şekilde dönüştürülmelidir.
- Metin dosyasındaki verileri SQL havuzu hedef tablosundaki sütunlarla ve veri türleri ile hizalamak için biçimlendirin. Dış metin dosyalarındaki veri türleri ile veri ambarı tablosu arasındaki yanlış hizalama, satırların yük sırasında reddedilmesine neden olur.
- Sonlandırıcı ile metin dosyasında ayrı alanlar.  Kaynak verilerinizde bulunmayan bir karakter veya karakter dizisi kullandığınızdan emin olun. [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ile belirttiğiniz sonlandırıcıyı kullanın.

## <a name="4-load-the-data-into-sql-pool-staging-tables-using-polybase"></a>4. PolyBase kullanarak verileri SQL havuz hazırlama tablolarına yükleyin

Verileri bir hazırlama tablosuna yüklemek en iyi yöntemdir. Hazırlama tabloları, üretim tablolarına müdahale etmeden hataları işlemenizi sağlar. Bir hazırlama tablosu, verileri üretim tablolarına eklemeden önce veri dönüşümleri için SQL havuzu MPP'yi kullanma fırsatı da verir.

### <a name="options-for-loading-with-polybase"></a>PolyBase ile yükleme seçenekleri

PolyBase ile veri yüklemek için şu yükleme seçeneklerinden birini kullanabilirsiniz:

- [T-SQL'li PolyBase,](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) verileriniz Azure Blob depolama veya Azure Veri Gölü Deposu'nda olduğunda iyi çalışır. Yükleme işlemi üzerinde en fazla denetimi sağlar, ancak aynı zamanda dış veri nesnelerini tanımlamanızı da gerektirir. Kaynak tabloları hedef tablolarla eşlerken diğer yöntemler bu nesneleri arka planda tanımlar.  T-SQL yüklerini düzenlemek için Azure Veri Fabrikası, SSIS veya Azure işlevlerini kullanabilirsiniz.
- [SSIS ile PolyBase,](/sql/integration-services/load-data-to-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kaynak verileriniz SQL Server'da, sql server'da veya bulutta olduğunda iyi çalışır. SSIS, hedef tablo eşlemelerinin kaynağını tanımlar ve yükü de yönetir. SSIS paketlerizatenniz varsa, paketleri yeni veri ambarı hedefiyle çalışacak şekilde değiştirebilirsiniz.
- [Azure Veri Fabrikası (ADF) ile PolyBase](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) başka bir düzenleme aracıdır.  Bir ardışık hatlar tanımlar ve işleri planlar.
- [Azure Veri Tuğlaları ile PolyBase](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) verileri SQL Veri Ambarı tablosundan Databricks veri çerçevesine aktarıyor ve/veya Verileri Databricks veri çerçevesinden PolyBase kullanarak bir SQL Veri Ambarı tablosuna yazıyor.

### <a name="non-polybase-loading-options"></a>PolyBase dışı yükleme seçenekleri

Verileriniz PolyBase ile uyumlu değilse, [bCP](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) veya [SQLBulkCopy API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)kullanabilirsiniz. bCP, Azure Blob depolama alanına girmeden doğrudan SQL havuzuna yüklenir ve yalnızca küçük yükler için tasarlanmıştır. Bu seçeneklerin yük performansının PolyBase'den önemli ölçüde daha yavaş olduğunu unutmayın.

## <a name="5-transform-the-data"></a>5. Verileri dönüştürme

Veriler hazırlama tablosundayken, iş yükünüzün gerektirdiği dönüşümleri gerçekleştirin. Ardından verileri üretim tablosuna taşıyın.

## <a name="6-insert-the-data-into-production-tables"></a>6. Verileri üretim tablolarına ekleme

INSERT INTO ... SELECT deyimi verileri evreleme tablosundan kalıcı tabloya taşır.

Bir ETL işlemi tasarlarken, işlemi küçük bir test örneğinde çalıştırmayı deneyin. Tablodan dosyaya 1000 satır ayıklamayı deneyin, Azure'a taşıyın ve ardından bir hazırlama tablosuna yüklemeyi deneyin.

## <a name="partner-loading-solutions"></a>İş ortağı yükleme çözümleri

Ortaklarımızın çoğu yükleme çözümlerine sahiptir. Daha fazla bilgi edinmek için [çözüm ortaklarımızın](../sql-data-warehouse/sql-data-warehouse-partner-business-intelligence.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)listesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Yükleme kılavuzu [için yük verileri için Yönerge'ye](data-loading-best-practices.md)bakın.
