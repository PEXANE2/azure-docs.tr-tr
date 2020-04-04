---
title: ETL yerine ELT tasarla
description: Azure Synapse Analytics'te Synapse SQL havuzu için esnek veri yükleme stratejileri uygulayın
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/19/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 87b33e91076f8f7f31740795f0ec05cea49a1e83
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631197"
---
# <a name="data-loading-strategies-for-synapse-sql-pool"></a>Synapse SQL havuzu için veri yükleme stratejileri

Geleneksel SMP SQL havuzları, veri yüklemek için ayıklama, dönüştürme ve yükleme (ETL) işlemini kullanır. Azure Synapse Analytics bünyesindeki Synapse SQL havuzu, bilgi işlem ve depolama kaynaklarının ölçeklenebilirliğinden ve esnekliğinden yararlanan büyük ölçüde paralel bir işleme (MPP) mimarisine sahiptir.

Ayıklama, Yükleme ve Dönüştürme (ELT) işlemi kullanmak MPP'den yararlanır ve yüklemeden önce veri dönüşümü için gereken kaynakları ortadan kaldırır.

SQL [havuzu, bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ve [SqlBulkCopy API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)gibi popüler SQL Server seçenekleri de dahil olmak üzere birçok yükleme yöntemini desteklerken, verileri yüklemenin en hızlı ve en ölçeklenebilir yolu PolyBase harici tabloları ve [COPY deyimi](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (önizleme) üzerinden dir.

PolyBase ve COPY deyimi yle, Azure Blob depolama veya Azure Veri Gölü Deposu'nda depolanan harici verilere T-SQL dili üzerinden erişebilirsiniz. Yükleme yaparken en fazla esneklik için COPY deyimini kullanmanızı öneririz.

> [!NOTE]  
> COPY deyimi şu anda genel önizlemededir. Geri bildirim sağlamak için aşağıdaki dağıtım sqldwcopypreview@service.microsoft.comlistesine e-posta gönderin: .

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>ELT nedir?

Ayıklama, Yükleme ve Dönüştürme (ELT), verilerin kaynak sistemden ayıklandığı, SQL havuzuna yüklendiği ve sonra dönüştürüldüğü bir işlemdir.

ELT'nin uygulanması için temel adımlar şunlardır:

1. Kaynak verileri metin dosyalarına ayıklayın.
2. Verileri Azure Blob depolama alanına veya Azure Veri Gölü Deposu'na aktarın.
3. Verileri yüklemeye hazırlayın.
4. Verileri PolyBase veya COPY komutuyla hazırlama tablolarına yükleyin.
5. Verileri dönüştürün.
6. Verileri üretim tablolarına ekleyin.

Bir PolyBase yükleme öğreticisi için Azure [blob depolamadan veri yüklemek için PolyBase'i kullanın'a](load-data-from-azure-blob-storage-using-polybase.md)bakın.

Daha fazla bilgi için yükleme [desenleri bloguna](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/)bakın.

## <a name="1-extract-the-source-data-into-text-files"></a>1. Kaynak verileri metin dosyalarına ayıklama

Verileri kaynak sisteminizden çıkarmak depolama konumuna bağlıdır.  Amaç, verileri PolyBase'e ve COPY destekli sınırlı metin veya CSV dosyalarına taşımaktır.

### <a name="polybase-and-copy-external-file-formats"></a>PolyBase ve COPY harici dosya biçimleri

PolyBase ve COPY deyimi ile UTF-8 ve UTF-16 kodlanmış sınırlandırılmış metin veya CSV dosyalarından veri yükleyebilirsiniz. Sınırlı metin veya CSV dosyalarına ek olarak, ORC ve Parke gibi Hadoop dosya biçimlerinden yükler. PolyBase ve COPY deyimi de Gzip ve Snappy sıkıştırılmış dosyalardan veri yükleyebilir.

Genişletilmiş ASCII, sabit genişlik biçimi ve WinZip veya XML gibi iç içe formatlar desteklenmez. SQL Server'dan dışa aktarım yapıyorsunuzsa, verileri sınırlı metin dosyalarına aktarmak için [bcp komut satırı aracını](/sql/tools/bcp-utility?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanabilirsiniz.

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Verileri Azure Blob depolama veya Azure Veri Gölü Deposu'na aktarın

Verileri Azure depolama alanına bgetirmek için Azure [Blob depolama](../../storage/blobs/storage-blobs-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) sına veya [Azure Veri Gölü Deposu Gen2'ye](../../data-lake-store/data-lake-store-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)taşıyabilirsiniz. Her iki konumda da veriler metin dosyalarında depolanmalıdır. PolyBase ve COPY deyimi her iki konumdan yüklenebilir.

Verileri Azure Depolama'ya taşımak için kullanabileceğiniz araçlar ve hizmetler:

- [Azure ExpressRoute](../../expressroute/expressroute-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) hizmeti ağ iş buzunu, performansını ve öngörülebilirliği artırır. ExpressRoute, verilerinizi Azure'a özel bir özel bağlantı üzerinden yönlendiren bir hizmettir. ExpressRoute bağlantıları verileri genel internet üzerinden yönlendirmez. Bağlantılar, genel internet üzerinden tipik bağlantılara göre daha fazla güvenilirlik, daha hızlı hızlar, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar.
- [AZCopy yardımcı programı](../../storage/common/storage-choose-data-transfer-solution.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) verileri genel internet üzerinden Azure Depolama'ya taşır. Veri boyutlarınız 10 TB'den küçükse bu işe yarar. AZCopy ile düzenli olarak yük gerçekleştirmek için, kabul edilebilir olup olmadığını görmek için ağ hızını test edin.
- [Azure Veri Fabrikası'nda (ADF)](../../data-factory/introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) yerel sunucunuza yükleyebileceğiniz bir ağ geçidi vardır. Ardından, verileri yerel sunucunuzdan Azure Depolama'ya taşımak için bir ardışık kaynak oluşturabilirsiniz. SQL Analytics ile Veri Fabrikası'nı kullanmak [için SQL Analytics için veri yükleme bölümüne](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)bakın.

## <a name="3-prepare-the-data-for-loading"></a>3. Verileri yükleme için hazırlama

Yüklemeden önce depolama hesabınızdaki verileri hazırlamanız ve temizlemeniz gerekebilir. Verileriniz kaynaktayken, verileri metin dosyalarına dışa aktarırken veya veriler Azure Depolama'da yken veri hazırlama işlemi yapılabilir.  Verilerle mümkün olduğunca erken çalışmak en kolayıdır.  

### <a name="define-external-tables"></a>Dış tabloları tanımlama

PolyBase kullanıyorsanız, yüklemeden önce SQL havuzunuzda harici tablolar tanımlamanız gerekir. Dış tablolar COPY deyimi tarafından gerekli değildir. PolyBase, Azure Depolama'daki verileri tanımlamak ve erişmek için harici tablolar kullanır.

Harici tablo veritabanı görünümüne benzer. Dış tablo tablo şemasını içerir ve SQL havuzunun dışında depolanan verilere işaret eder.

Dış tabloların tanımlanması, veri kaynağını, metin dosyalarının biçimini ve tablo tanımlarını belirtmeyi içerir. İhtiyacınız olacak T-SQL sözdizimi başvuru makaleleri şunlardır:

- [DıŞ VERI KAYNAĞı OLUŞTURMA](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [DıŞ TABLO OLUŞTURMA](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

Parke yüklerken, SQL veri türü eşleme:

| **Parke Veri Türü** | **SQL Veri Türü** |
| :-------------------: | :---------------: |
|        tinyint        |      tinyint      |
|       smallint        |     smallint      |
|          int          |        int        |
|        bigint         |      bigint       |
|        boole        |        bit        |
|        double         |       float       |
|         float         |       gerçek        |
|        double         |       Para       |
|        double         |    Smallmoney     |
|        string         |       Nchar       |
|        string         |     nvarchar      |
|        string         |       char        |
|        string         |      varchar      |
|        ikili         |      ikili       |
|        ikili         |     Varbinary     |
|       timestamp       |       date        |
|       timestamp       |   Smalldatetime   |
|       timestamp       |     datetime2     |
|       timestamp       |     datetime      |
|       timestamp       |       time        |
|         date          |       date        |
|        decimal        |      decimal      |

Dış nesneler oluşturma örneği için, yükleme öğreticisinde [dış tablolar oluştur](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) adımına bakın.

### <a name="format-text-files"></a>Metin dosyalarını biçimlendirme

PolyBase kullanıyorsanız, tanımlanan dış nesnelerin metin dosyalarının satırlarını dış tablo ve dosya biçimi tanımıyla hizalamanız gerekir. Metin dosyasının her satırındaki veriler tablo tanımıyla hizalanmalıdır.
Metin dosyalarını biçimlendirmek için:

- Verileriniz ilişkisel olmayan bir kaynaktan geliyorsa, verilerinizi satır ve sütunlara dönüştürmeniz gerekir. Veriler ilişkisel veya ilişkisel olmayan bir kaynaktan olsun, veriler, verileri yüklemeyi planladığınız tablonun sütun tanımlarıyla hizalanacak şekilde dönüştürülmelidir.
- Metin dosyasındaki verileri hedef tablodaki sütunlarla ve veri türleriile hizalamak için biçimlendirin. Dış metin dosyalarındaki veri türleri ile SQL bilardo tablosu arasındaki yanlış hizalama, satırların yükleme sırasında reddedilmesine neden olur.
- Sonlandırıcı ile metin dosyasında ayrı alanlar.  Kaynak verilerinizde bulunmayan bir karakter veya karakter dizisi kullandığınızdan emin olun. [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ile belirttiğiniz sonlandırıcıyı kullanın.

## <a name="4-load-the-data-using-polybase-or-the-copy-statement"></a>4. Verileri PolyBase veya COPY deyimini kullanarak yükleyin

Verileri bir hazırlama tablosuna yüklemek en iyi yöntemdir. Hazırlama tabloları, üretim tablolarına müdahale etmeden hataları işlemenizi sağlar. Bir hazırlama tablosu, verileri üretim tablolarına eklemeden önce veri dönüşümleri için SQL havuzu MPP'yi kullanma fırsatı da verir.

TABLONUN COPY ile bir hazırlama tablosuna yüklenirken önceden oluşturulması gerekir.

### <a name="options-for-loading-with-polybase-and-copy-statement"></a>PolyBase ve COPY deyimi ile yükleme seçenekleri

PolyBase ile veri yüklemek için şu yükleme seçeneklerinden birini kullanabilirsiniz:

- [T-SQL'li PolyBase,](load-data-from-azure-blob-storage-using-polybase.md) verileriniz Azure Blob depolama veya Azure Veri Gölü Deposu'nda olduğunda iyi çalışır. Yükleme işlemi üzerinde en fazla denetimi sağlar, ancak aynı zamanda dış veri nesnelerini tanımlamanızı da gerektirir. Kaynak tabloları hedef tablolarla eşlerken diğer yöntemler bu nesneleri arka planda tanımlar.  T-SQL yüklerini düzenlemek için Azure Veri Fabrikası, SSIS veya Azure işlevlerini kullanabilirsiniz.
- [SSIS ile PolyBase,](/sql/integration-services/load-data-to-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kaynak verileriniz SQL Server'da, sql server'da veya bulutta olduğunda iyi çalışır. SSIS, hedef tablo eşlemelerinin kaynağını tanımlar ve yükü de yönetir. SSIS paketlerizatenniz varsa, paketleri yeni veri ambarı hedefiyle çalışacak şekilde değiştirebilirsiniz.
- [Azure Veri Fabrikası (ADF) ile PolyBase ve COPY deyimi](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) başka bir düzenleme aracıdır.  Bir ardışık hatlar tanımlar ve işleri planlar.
- [Azure Databricks ile PolyBase](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) verileri bir tablodan Databricks veri çerçevesine aktarıyor ve/veya Verileri Bir Databricks veri çerçevesinden PolyBase'i kullanarak tabloya yazar.

### <a name="other-loading-options"></a>Diğer yükleme seçenekleri

PolyBase ve COPY deyimine ek olarak [bcp](https://docs.microsoft.com/sql/tools/bcp-utility?view=sql-server-ver15) veya [SqlBulkCopy API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx)kullanabilirsiniz. bCP, Azure Blob depolama alanına girmeden doğrudan veritabanına yüklenir ve yalnızca küçük yükler için tasarlanmıştır.

> [!NOTE]
> Bu seçeneklerin yük performansı Nın PolyBase ve COPY deyiminden daha yavaş olduğunu unutmayın.

## <a name="5-transform-the-data"></a>5. Verileri dönüştürme

Veriler hazırlama tablosundayken, iş yükünüzün gerektirdiği dönüşümleri gerçekleştirin. Ardından verileri üretim tablosuna taşıyın.

## <a name="6-insert-the-data-into-production-tables"></a>6. Verileri üretim tablolarına ekleme

INSERT INTO ... SELECT deyimi verileri evreleme tablosundan kalıcı tabloya taşır.

Bir ETL işlemi tasarlarken, işlemi küçük bir test örneğinde çalıştırmayı deneyin. Tablodan dosyaya 1000 satır ayıklamayı deneyin, Azure'a taşıyın ve ardından bir hazırlama tablosuna yüklemeyi deneyin.

## <a name="partner-loading-solutions"></a>İş ortağı yükleme çözümleri

Ortaklarımızın çoğu yükleme çözümlerine sahiptir. Daha fazla bilgi edinmek için [çözüm ortaklarımızın](sql-data-warehouse-partner-business-intelligence.md)listesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Yüklemeyle ilgili rehber için bkz. [Veri yükleme rehberi](guidance-for-loading-data.md).
