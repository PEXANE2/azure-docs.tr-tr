---
title: ETL yerine, Azure SQL veri ambarı için ELT tasarlayın | Microsoft Docs
description: ETL yerine, verileri veya Azure SQL veri ambarını yüklemek için bir ayıklama, yükleme ve dönüştürme (ELT) işlemi tasarlayın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load-data
ms.date: 07/28/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: c90deefba75cd8bbeda126c9da8a05e1069831d4
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597475"
---
# <a name="designing-a-polybase-data-loading-strategy-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için PolyBase veri yükleme stratejisi tasarlama

Geleneksel SMP veri ambarları, verileri yüklemek için bir ayıklama, dönüştürme ve yükleme (ETL) işlemi kullanır. Azure SQL veri ambarı, bilgi işlem ve depolama kaynaklarının ölçeklenebilirlik ve esnekliğinden yararlanan, yüksek düzeyde paralel işleme (MPP) mimarisidir. Bir ayıklama, yükleme ve dönüştürme (ELT) işleminin kullanılmasıyla, MPP 'den yararlanabilir ve yüklemeden önce verileri dönüştürmek için gereken kaynakları ortadan kaldırabilir. SQL veri ambarı BCP ve SQL BulkCopy API gibi PolyBase seçenekleri de dahil olmak üzere birçok yükleme yöntemini destekleirken, yükleme tarihinin en hızlı ve en ölçeklenebilir yolu PolyBase aracılığıyla yapılır.  PolyBase, Azure Blob depolamada depolanan dış verilere veya T-SQL dili üzerinden Azure Data Lake Store erişen bir teknolojidir.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]


## <a name="what-is-elt"></a>ELT nedir?

Ayıklama, yükleme ve dönüştürme (ELT), verilerin bir kaynak sistemden ayıklandığı, bir veri ambarına yüklendiği ve sonra dönüştürülebileceği bir işlemdir. 

SQL veri ambarı için PolyBase ELT uygulamaya yönelik temel adımlar şunlardır:

1. Kaynak verileri metin dosyalarına ayıklayın.
2. Verileri Azure Blob depolama alanına veya Azure Data Lake Store ekleyin.
3. Verileri yükleme için hazırlayın.
4. PolyBase kullanarak verileri SQL veri ambarı hazırlama tablolarına yükleyin. 
5. Verileri dönüştürün.
6. Verileri üretim tablolarına ekleyin.


Yükleme öğreticisi için bkz. [Azure Blob depolamadan Azure SQL veri ambarı 'na veri yüklemek Için PolyBase kullanma](load-data-from-azure-blob-storage-using-polybase.md).

Daha fazla bilgi için bkz. [desenleri web günlüğü yükleme](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/). 


## <a name="1-extract-the-source-data-into-text-files"></a>1. Kaynak verileri metin dosyalarına Ayıkla

Kaynak sisteminizden veri alma, depolama konumuna bağlıdır.  Amaç, verileri PolyBase tarafından desteklenen sınırlandırılmış metin dosyalarına taşımaktır. 

### <a name="polybase-external-file-formats"></a>PolyBase dış dosya biçimleri

PolyBase UTF-8 ve UTF-16 kodlamalı sınırlandırılmış metin dosyalarından veri yükler. Sınırlandırılmış metin dosyalarına ek olarak, bu dosya, RC dosyası, ORC ve Parquet Hadoop dosya biçimlerini yükler. PolyBase, gzip ve Snappy sıkıştırılmış dosyalarındaki verileri de yükleyebilir. PolyBase Şu anda genişletilmiş ASCII, sabit genişlikli biçim ve WinZip, JSON ve XML gibi iç içe geçmiş biçimleri desteklemez. SQL Server dışarı aktarıyorsanız, verileri sınırlandırılmış metin dosyalarına aktarmak için [bcp komut satırı aracını](/sql/tools/bcp-utility) kullanabilirsiniz. SQL DW veri türü eşlemesine Parquet aşağıdaki şekilde verilmiştir:

| **Parquet veri türü** |                      **SQL veri türü**                       |
| :-------------------: | :----------------------------------------------------------: |
|        tinyint        |                           tinyint                            |
|       smallint        |                           smallint                           |
|          int          |                             int                              |
|        bigint         |                            bigint                            |
|        boolean        |                             bit                              |
|        double         |                            float                             |
|         float         |                             real                             |
|        double         |                            money                             |
|        double         |                          smallmoney                          |
|        dize         |                            nchar                             |
|        dize         |                           nvarchar                           |
|        dize         |                             char                             |
|        dize         |                           varchar                            |
|        binary         |                            binary                            |
|        binary         |                          varbinary                           |
|       timestamp       |                             date                             |
|       timestamp       |                        smalldatetime                         |
|       timestamp       |                          datetime2                           |
|       timestamp       |                           datetime                           |
|       timestamp       |                             time                             |
|       date            |                             date                             |
|        decimal        |                            decimal                           |

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Verileri Azure Blob depolama alanına veya Azure Data Lake Store ekleyin

Verileri Azure depolama 'ya taşımak için [Azure Blob depolama](../storage/blobs/storage-blobs-introduction.md) alanına veya [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)taşıyabilirsiniz. Her iki konumda da veriler metin dosyalarında depolanmalıdır. PolyBase her iki konumdan de yüklenebilir.

Verileri Azure depolama 'ya taşımak için kullanabileceğiniz araçlar ve hizmetler:

- [Azure ExpressRoute](../expressroute/expressroute-introduction.md) hizmeti ağ aktarım hızını, performansı ve öngörülebilirlik geliştirir. ExpressRoute adanmış özel bağlantı üzerinden verilerinizi Azure'a yönlendiren bir hizmettir. ExpressRoute bağlantıları, verileri genel İnternet üzerinden yönlendirmez. Bağlantılar, genel İnternet üzerinden tipik bağlantılardan daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar.
- [AZCopy yardımcı programı](../storage/common/storage-moving-data.md) , verileri genel Internet üzerinden Azure depolama 'ya taşıtabilecek. Bu, veri boyutlarınızın 10 TB 'den küçük olması durumunda geçerlidir. AZCopy ile düzenli olarak yükleme gerçekleştirmek için, kabul edilebilir olup olmadığını görmek için ağ hızını test edin. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) , yerel sunucunuza yükleyebileceğiniz bir ağ geçidine sahip olabilir. Daha sonra, yerel sunucunuzdaki verileri Azure depolama 'ya taşımak için bir işlem hattı oluşturabilirsiniz. SQL veri ambarı ile Data Factory kullanmak için bkz. [SQL Data Warehouse 'a veri yükleme](/azure/data-factory/load-azure-sql-data-warehouse).


## <a name="3-prepare-the-data-for-loading"></a>3. Verileri yükleme için hazırlama

Depolama hesabınızdaki verileri SQL veri ambarı 'na yüklemeden önce hazırlamanız ve temizlemeniz gerekebilir. Veriler kaynakta olduğunda veri hazırlama gerçekleştirilebilir, verileri metin dosyalarına dışarı aktardığınızda veya veriler Azure Storage 'dayken olur.  Mümkün olduğunca önce verilerle çalışmak en kolay yoldur.  

### <a name="define-external-tables"></a>Dış tabloları tanımlama

Veri yükleyebilmeniz için önce veri Ambarınızda dış tablolar tanımlamanız gerekir. PolyBase, Azure depolama 'daki verileri tanımlamak ve verilere erişmek için dış tabloları kullanır. Dış tablo, veritabanı görünümüne benzer. Dış tablo tablo şemasını içerir ve veri ambarının dışında depolanan verileri gösterir. 

Dış tabloları tanımlama, veri kaynağını, metin dosyalarının biçimini ve tablo tanımlarını belirtmeyi içerir. Bunlar, ihtiyacınız olacak T-SQL sözdizimi konulardır:
- [DIŞ VERI KAYNAĞI OLUŞTUR](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [DIŞ TABLO OLUŞTUR](/sql/t-sql/statements/create-external-table-transact-sql)

Dış nesne oluşturma hakkında bir örnek için, yükleme öğreticisindeki [dış tablo oluşturma](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) adımına bakın.

### <a name="format-text-files"></a>Metin dosyalarını biçimlendirme

Dış nesneler tanımlandıktan sonra, metin dosyalarının satırlarını dış tablo ve dosya biçimi tanımıyla hizalamanız gerekir. Metin dosyasının her satırındaki veriler tablo tanımıyla hizalanmalıdır.
Metin dosyalarını biçimlendirmek için:

- Verileriniz ilişkisel olmayan bir kaynaktan geliyorsa, onu satırlara ve sütunlara dönüştürmeniz gerekir. Verilerin ilişkisel veya ilişkisel olmayan bir kaynaktan olup olmadığı, verileri yüklemeyi planladığınız tablonun sütun tanımlarına göre hizalanacak şekilde dönüştürülmesi gerekir. 
- Metin dosyasındaki verileri, SQL veri ambarı hedef tablosundaki sütunlar ve veri türleriyle hizalamak için biçimlendirin. Dış metin dosyalarındaki veri türleri arasındaki hatalı hizalanmış ve veri ambarı tablosu, yük sırasında satırların reddedilmesine neden olur.
- Metin dosyasındaki alanları Sonlandırıcı ile ayırın.  Kaynak verilerinizde bulunmayan bir karakter veya karakter dizisi kullandığınızdan emin olun. [Dış dosya biçimi oluştur](/sql/t-sql/statements/create-external-file-format-transact-sql)ile belirttiğiniz sonlandırıcıyı kullanın.


## <a name="4-load-the-data-into-sql-data-warehouse-staging-tables-using-polybase"></a>4. PolyBase kullanarak verileri SQL veri ambarı hazırlama tablolarına yükleme

Hazırlama tablosuna veri yüklemek en iyi uygulamadır. Hazırlama tabloları, üretim tablolarının kesintiye uğramadan hataları işleyebilmeniz için izin verir. Hazırlama tablosu, verileri üretim tablolarına eklemeden önce veri dönüştürmeleri için SQL veri ambarı MPP kullanma fırsatını de sağlar.

### <a name="options-for-loading-with-polybase"></a>PolyBase ile yükleme seçenekleri

PolyBase ile veri yüklemek için, bu yükleme seçeneklerinden herhangi birini kullanabilirsiniz:

- [T-SQL Ile PolyBase](load-data-from-azure-blob-storage-using-polybase.md) , verileriniz Azure Blob depolama veya Azure Data Lake Store olduğunda iyi bir şekilde çalışabilir. Bu, yükleme işlemi üzerinde en fazla denetim sağlar, ancak dış veri nesneleri tanımlamanızı da gerektirir. Diğer yöntemler, kaynak tablolarını hedef tablolarla eşleştirdiğinizde arka planda bu nesneleri tanımlar.  T-SQL yüklemelerini düzenlemek için Azure Data Factory, SSIS veya Azure işlevlerini kullanabilirsiniz. 
- [SSIS Ile PolyBase](/sql/integration-services/load-data-to-sql-data-warehouse) , kaynak verileriniz SQL Server, şirket içinde veya bulutta SQL Server olduğunda iyi çalışacaktır. SSIS, kaynağı hedef tablo eşlemelerine tanımlar ve ayrıca yükü düzenler. SSIS paketleriniz zaten varsa, yeni veri ambarı hedefle çalışacak şekilde paketleri değiştirebilirsiniz. 
- [Azure Data Factory (ADF) Ile PolyBase](sql-data-warehouse-load-with-data-factory.md) başka bir düzenleme aracıdır.  İşlem hattını tanımlar ve işleri zamanlar. 
- [Azure databricks Ile PolyBase,](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) VERILERI bir SQL veri ambarı tablosundan Databricks veri çerçevesine aktarır ve/veya databricks veri çerçevesindeki verileri PolyBase kullanarak SQL veri ambarı tablosuna yazar.

### <a name="non-polybase-loading-options"></a>PolyBase yükleme seçenekleri

Verileriniz PolyBase ile uyumlu değilse [bcp](/sql/tools/bcp-utility) veya [SqlBulkCopy API 'sini](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx)kullanabilirsiniz. BCP, Azure Blob depolama alanına geçmeden doğrudan SQL veri ambarı 'na yüklenir ve yalnızca küçük yüklemeler için tasarlanmıştır. Bu seçeneklerin yük performansı, PolyBase 'den önemli ölçüde daha yavaştır. 


## <a name="5-transform-the-data"></a>5. Verileri dönüştürme

Veriler hazırlama tablodayken, iş yükünüzün gerektirdiği dönüştürmeleri gerçekleştirin. Sonra verileri bir üretim tablosuna taşıyın.


## <a name="6-insert-the-data-into-production-tables"></a>6. Verileri üretim tablolarına ekleme

INSERT INTO... SELECT ifadesinin verileri hazırlama tablosundan kalıcı tabloya taşınıyor. 

Bir ETL işlemi tasarlarken, işlemi küçük bir test örneğinde çalıştırmayı deneyin. Tablodaki 1000 satırı bir dosyaya ayıklamayı deneyin, Azure 'a taşıyın ve sonra hazırlama tablosuna yüklemeyi deneyin. 


## <a name="partner-loading-solutions"></a>İş ortağı yükleme çözümleri

İş ortaklarımızın birçoğu çözüm yüklüyor. Daha fazla bilgi edinmek için [çözüm iş ortaklarımızın](sql-data-warehouse-partner-business-intelligence.md)listesini inceleyin. 


## <a name="next-steps"></a>Sonraki adımlar

Yükleme Kılavuzu için bkz. [veri yükleme kılavuzu](guidance-for-loading-data.md).


