---
title: ETL yerine,, ELT tasarlayın
description: Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu için esnek veri yükleme stratejileri uygulama
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/13/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: fc5316e2d6509f3e4db9a6cba150efc42c8bc548
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266381"
---
# <a name="data-loading-strategies-for-synapse-sql-pool"></a>SYNAPSE SQL havuzu için veri yükleme stratejileri

Geleneksel SMP SQL havuzları verileri yüklemek için bir Ayıkla, dönüştürme ve yükleme (ETL) işlemi kullanır. Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu, bilgi işlem ve depolama kaynaklarının ölçeklenebilirlik ve esnekliğinden faydalanan, yüksek düzeyde paralel işleme (MPP) mimarisine sahiptir.

Ayıklama, yükleme ve dönüştürme (ELT) işleminin kullanımı, MPP özelliğinden yararlanır ve yüklemeden önce veri dönüştürme için gereken kaynakları ortadan kaldırır.

SQL havuzu [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ve [SqlBulkCopy apı](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)gibi popüler SQL Server seçenekleri de dahil olmak üzere birçok yükleme yöntemini desteklese de, verileri yüklemenin en hızlı ve en ölçeklenebilir yolu PolyBase dış tabloları ve [Copy deyimleridir](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (Önizleme).

PolyBase ve COPY ifadesiyle, Azure Blob depolamada depolanan dış verilere veya T-SQL dili üzerinden Azure Data Lake Store erişebilirsiniz. Yükleme sırasında en fazla esneklik için, COPY ifadesini kullanmanızı öneririz.

> [!NOTE]  
> COPY deyimleri Şu anda genel önizlemededir. Geri bildirim sağlamak için şu dağıtım listesine e-posta gönderin: sqldwcopypreview@service.microsoft.com .

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>ELT nedir?

Ayıklama, yükleme ve dönüştürme (ELT), verilerin bir kaynak sistemden ayıklandığı, bir SQL havuzuna yüklendiği ve sonra dönüştürülebileceği bir işlemdir.

ELT 'ı uygulamaya yönelik temel adımlar şunlardır:

1. Kaynak verileri metin dosyalarına ayıklayın.
2. Verileri Azure Blob depolama alanına veya Azure Data Lake Store ekleyin.
3. Verileri yükleme için hazırlayın.
4. Verileri PolyBase veya COPY komutuyla hazırlama tablolarına yükleyin.
5. Verileri dönüştürün.
6. Verileri üretim tablolarına ekleyin.

Yükleme öğreticisi için bkz. [Azure Blob depolamadan veri yükleme](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="1-extract-the-source-data-into-text-files"></a>1. kaynak verileri metin dosyalarına ayıklayın

Kaynak sisteminizden veri alma, depolama konumuna bağlıdır. Amaç, verileri desteklenen sınırlandırılmış metin veya CSV dosyalarına taşımaktır.

### <a name="supported-file-formats"></a>Desteklenen dosya biçimleri

PolyBase ve COPY ifadesiyle, UTF-8 ve UTF-16 kodlamalı sınırlandırılmış metin veya CSV dosyalarından veri yükleyebilirsiniz. Ayrılmış metin veya CSV dosyalarına ek olarak, ORC ve Parquet gibi Hadoop dosya biçimlerinden yüklenir. PolyBase ve COPY deyimleri, verileri gzip ve Snappy sıkıştırılmış dosyalarından de yükleyebilir.

Genişletilmiş ASCII, sabit genişlikli biçim ve WinZip veya XML gibi iç içe biçimler desteklenmez. SQL Server dışarı aktarıyorsanız, verileri sınırlandırılmış metin dosyalarına aktarmak için [bcp komut satırı aracını](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanabilirsiniz.

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. verileri Azure Blob depolama alanına veya Azure Data Lake Store ekleyin

Verileri Azure depolama 'ya taşımak için [Azure Blob depolama](../../storage/blobs/storage-blobs-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) alanına veya [Azure Data Lake Store Gen2](../../data-lake-store/data-lake-store-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)taşıyabilirsiniz. Her iki konumda da veriler metin dosyalarında depolanmalıdır. PolyBase ve COPY deyimleri her iki konumdan de yüklenebilir.

Verileri Azure depolama 'ya taşımak için kullanabileceğiniz araçlar ve hizmetler:

- [Azure ExpressRoute](../../expressroute/expressroute-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) hizmeti ağ aktarım hızını, performansı ve öngörülebilirlik geliştirir. ExpressRoute, verilerinizi Azure 'a adanmış bir özel bağlantıyla yönlendiren bir hizmettir. ExpressRoute bağlantıları, verileri genel İnternet üzerinden yönlendirmez. Bağlantılar, genel İnternet üzerinden tipik bağlantılardan daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar.
- [AZCopy yardımcı programı](../../storage/common/storage-choose-data-transfer-solution.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) , verileri genel Internet üzerinden Azure depolama 'ya taşıtabilecek. Bu, veri boyutlarınızın 10 TB 'den küçük olması durumunda geçerlidir. AZCopy ile düzenli olarak yükleme gerçekleştirmek için, kabul edilebilir olup olmadığını görmek için ağ hızını test edin.
- [Azure Data Factory (ADF)](../../data-factory/introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) , yerel sunucunuza yükleyebileceğiniz bir ağ geçidine sahip olabilir. Daha sonra, yerel sunucunuzdaki verileri Azure depolama 'ya taşımak için bir işlem hattı oluşturabilirsiniz. SQL havuzu ile Data Factory kullanmak için bkz. [SQL havuzu için verileri yükleme](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="3-prepare-the-data-for-loading"></a>3. verileri yükleme için hazırlama

Yüklemeden önce Depolama hesabınızdaki verileri hazırlamanız ve temizlemeniz gerekebilir. Veriler kaynakta olduğunda veri hazırlama gerçekleştirilebilir, verileri metin dosyalarına dışarı aktardığınızda veya veriler Azure Storage 'dayken olur.  Mümkün olduğunca önce verilerle çalışmak en kolay yoldur.  

### <a name="define-the-tables"></a>Tabloları tanımlama

COPY ifadesini kullanırken, önce SQL havuzunuza yüklediğiniz tablo (ler) i tanımladığınız tabloları tanımlamalısınız.

PolyBase kullanıyorsanız, yüklemeden önce SQL havuzunuzdaki dış tabloları tanımlamanız gerekir. PolyBase, Azure depolama 'daki verileri tanımlamak ve verilere erişmek için dış tabloları kullanır. Dış tablo, veritabanı görünümüne benzer. Dış tablo, tablo şemasını içerir ve SQL havuzunun dışında depolanan verileri gösterir.

Dış tabloları tanımlama, veri kaynağını, metin dosyalarının biçimini ve tablo tanımlarını belirtmeyi içerir. T-SQL sözdizimi başvuru makaleleri ihtiyacınız olacak:

- [DıŞ VERI KAYNAĞı OLUŞTUR](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [DıŞ TABLO OLUŞTUR](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

Parquet dosyalarını yüklerken aşağıdaki SQL veri türü eşlemesini kullanın:

|                         Parquet türü                         |   Parquet mantıksal türü (ek açıklama)   |  SQL veri türü   |
| :----------------------------------------------------------: | :-----------------------------------: | :--------------: |
|                           BOOLEAN                            |                                       |       bit        |
|                     IKILI/BYTE_ARRAY                      |                                       |    ikili     |
|                            ÇIFT                            |                                       |      float       |
|                            FLOAT                             |                                       |       real       |
|                            INT32                             |                                       |       int        |
|                            INT64                             |                                       |      bigint      |
|                            INT96                             |                                       |    datetime2     |
|                     FIXED_LEN_BYTE_ARRAY                     |                                       |      ikili      |
|                            Ý                            |                 UTF8                  |     nvarchar     |
|                            Ý                            |                DIZISINDE                 |     nvarchar     |
|                            Ý                            |                 YARDıMıNıN                  |     nvarchar     |
|                            Ý                            |                 EDIN                  | uniqueidentifier |
|                            Ý                            |                KATEGORI                |     decimal      |
|                            Ý                            |                 JSON                  |  nvarchar(MAX)   |
|                            Ý                            |                 BSON                  |  varbinary (max)  |
|                     FIXED_LEN_BYTE_ARRAY                     |                KATEGORI                |     decimal      |
|                          BYTE_ARRAY                          |               ARALıĞıNDA                |  varchar (max),   |
|                            INT32                             |             TAMSAYı (8, doğru)              |     smallint     |
|                            INT32                             |            INT (16, doğru)            |     smallint     |
|                            INT32                             |             INT (32, doğru)             |       int        |
|                            INT32                             |            INT (8, false)            |     tinyint      |
|                            INT32                             |            INT (16, false)             |       int        |
|                            INT32                             |           INT (32, false)            |      bigint      |
|                            INT32                             |                 DATE                  |       date       |
|                            INT32                             |                KATEGORI                |     decimal      |
|                            INT32                             |            SAAT (MILIMETRE)             |       time       |
|                            INT64                             |            INT (64, true)            |      bigint      |
|                            INT64                             |           INT (64, false)            |  ondalık (20, 0)   |
|                            INT64                             |                KATEGORI                |     decimal      |
|                            INT64                             |         SAAT (MIKRO S/NANOS)         |       time       |
|                            INT64                             | ZAMAN DAMGASı (MILIMETRE/MIKRO S/NANOS) |    datetime2     |
| [Karmaşık tür](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2Fapache%2Fparquet-format%2Fblob%2Fmaster%2FLogicalTypes.md%23lists&data=02\|01\|kevin%40microsoft.com\|19f74d93f5ca45a6b73c08d7d7f5f111\|72f988bf86f141af91ab2d7cd011db47\|1\|0\|637215323617803168&sdata=6Luk047sK26ijTzfvKMYc%2FNu%2Fz0AlLCX8lKKTI%2F8B5o%3D&reserved=0) |                 LISTELE                  |   varchar(maks.)   |
| [Karmaşık tür](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2Fapache%2Fparquet-format%2Fblob%2Fmaster%2FLogicalTypes.md%23maps&data=02\|01\|kevin%40microsoft.com\|19f74d93f5ca45a6b73c08d7d7f5f111\|72f988bf86f141af91ab2d7cd011db47\|1\|0\|637215323617803168&sdata=FiThqXxjgmZBVRyigHzfh5V7Z%2BPZHjud2IkUUM43I7o%3D&reserved=0) |                  HARITA                  |   varchar(maks.)   |



Dış nesne oluşturma örneği için bkz. [dış tablo oluşturma](https://docs.microsoft.com/azure/synapse-analytics/sql/develop-tables-external-tables?tabs=sql-pool).

### <a name="format-text-files"></a>Metin dosyalarını biçimlendirme

PolyBase kullanıyorsanız, tanımlanan dış nesnelerin metin dosyalarının satırlarını dış tablo ve dosya biçimi tanımıyla hizalamaları gerekir. Metin dosyasının her satırındaki veriler tablo tanımıyla hizalanmalıdır.
Metin dosyalarını biçimlendirmek için:

- Verileriniz ilişkisel olmayan bir kaynaktan geliyorsa, onu satırlara ve sütunlara dönüştürmeniz gerekir. Verilerin ilişkisel veya ilişkisel olmayan bir kaynaktan olup olmadığı, verileri yüklemeyi planladığınız tablonun sütun tanımlarına göre hizalanacak şekilde dönüştürülmesi gerekir.
- Metin dosyasındaki verileri, hedef tablodaki sütunlar ve veri türleriyle hizalamak için biçimlendirin. Dış metin dosyalarındaki veri türleri arasındaki hatalı hizalanmış ve SQL havuzu tablosu, yük sırasında satırların reddedilmesine neden olur.
- Metin dosyasındaki alanları Sonlandırıcı ile ayırın.  Kaynak verilerinizde bulunmayan bir karakter veya karakter dizisi kullandığınızdan emin olun. [Dış dosya biçimi oluştur](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ile belirttiğiniz sonlandırıcıyı kullanın.

## <a name="4-load-the-data-using-polybase-or-the-copy-statement"></a>4. PolyBase veya COPY ifadesini kullanarak verileri yükleme

Hazırlama tablosuna veri yüklemek en iyi uygulamadır. Hazırlama tabloları, üretim tablolarının kesintiye uğramadan hataları işleyebilmeniz için izin verir. Hazırlama tablosu, verileri üretim tablolarına eklemeden önce veri dönüştürmeleri için SQL havuzu paralel işlem mimarisini kullanma fırsatını de sağlar.

### <a name="options-for-loading"></a>Yükleme seçenekleri

Verileri yüklemek için bu yükleme seçeneklerinden herhangi birini kullanabilirsiniz:

- [Copy deyimleri](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) , verileri sorunsuzca ve esnek bir şekilde yükleme imkanı sağladığından önerilen yükleme yardımcı programıdır. Bildiriminde, PolyBase 'in sağlamayan birçok ek yükleme özelliği bulunur. 
- [T-SQL Ile PolyBase](load-data-from-azure-blob-storage-using-polybase.md) , dış veri nesneleri tanımlamanızı gerektirir.
- [Azure Data Factory (ADF) Ile PolyBase ve Copy deyimleri](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) başka bir düzenleme aracıdır.  İşlem hattını tanımlar ve işleri zamanlar.
- [SSIS Ile PolyBase](/sql/integration-services/load-data-to-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) , kaynak verileriniz SQL Server olduğunda iyi sonuç verir. SSIS, kaynağı hedef tablo eşlemelerine tanımlar ve ayrıca yükü düzenler. SSIS paketleriniz zaten varsa, yeni veri ambarı hedefle çalışacak şekilde paketleri değiştirebilirsiniz.
- [Azure Databricks Ile PolyBase,](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) verileri bir tablodan Databricks veri çerçevesine aktarır ve/veya databricks veri çerçevesindeki verileri PolyBase kullanarak bir tabloya yazar.

### <a name="other-loading-options"></a>Diğer yükleme seçenekleri

PolyBase ve COPY deyimlerinin yanı sıra [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) veya [SqlBulkCopy API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)'sini de kullanabilirsiniz. BCP, Azure Blob depolama alanına geçmeden doğrudan veritabanına yüklenir ve yalnızca küçük yüklere yöneliktir.

> [!NOTE]
> Bu seçeneklerin yük performansı PolyBase ve COPY deyiminden daha yavaştır.

## <a name="5-transform-the-data"></a>5. verileri dönüştürme

Veriler hazırlama tablodayken, iş yükünüzün gerektirdiği dönüştürmeleri gerçekleştirin. Sonra verileri bir üretim tablosuna taşıyın.

## <a name="6-insert-the-data-into-production-tables"></a>6. verileri üretim tablolarına ekleyin

INSERT INTO... SELECT ifadesinin verileri hazırlama tablosundan kalıcı tabloya taşınıyor.

Bir ETL işlemi tasarlarken, işlemi küçük bir test örneğinde çalıştırmayı deneyin. Tablodaki 1000 satırı bir dosyaya ayıklamayı deneyin, Azure 'a taşıyın ve sonra hazırlama tablosuna yüklemeyi deneyin.

## <a name="partner-loading-solutions"></a>İş ortağı yükleme çözümleri

İş ortaklarımızın birçoğu çözüm yüklüyor. Daha fazla bilgi edinmek için [çözüm iş ortaklarımızın](sql-data-warehouse-partner-business-intelligence.md)listesini inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

Yüklemeyle ilgili rehber için bkz. [Veri yükleme rehberi](guidance-for-loading-data.md).
