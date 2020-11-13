---
title: Azure Veri Paylaşımı'nda desteklenen veri depoları
description: Azure veri paylaşımının kullanımı için desteklenen veri depoları hakkında bilgi edinin.
ms.service: data-share
author: jifems
ms.author: jife
ms.topic: conceptual
ms.date: 11/12/2020
ms.openlocfilehash: 6289395b5d508de8da3e5c8c89caebb4b0e9b817
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94592439"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Azure Veri Paylaşımı'nda desteklenen veri depoları

Azure veri paylaşımı, ve farklı veri depolarından paylaşma özelliği de dahil olmak üzere açık ve esnek veri paylaşımı sağlar. Veri sağlayıcıları verileri bir veri deposu türünden paylaşabilir ve veri tüketicileri verileri hangi veri deposuna alacağını seçebilirler. 

Bu makalede, Azure veri paylaşımında desteklenen zengin Azure veri depoları kümesi hakkında bilgi edineceksiniz. Ayrıca, veri sağlayıcılarının ve veri tüketicilerinin yararlanılabilir olabilecek veri depolarının birleşimleri hakkında bilgi bulabilirsiniz. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Azure veri paylaşımında hangi veri depoları desteklenir? 

Aşağıdaki tabloda, Azure veri paylaşımında desteklenen veri kaynaklarının ayrıntıları verilmiştir. 

| Veri deposu | Anlık görüntü tabanlı paylaşım (tam anlık görüntü) | Anlık görüntü tabanlı paylaşım (Artımlı anlık görüntü) | Yerinde paylaşım 
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama |✓ |✓ | |
| Azure Data Lake Storage Gen1 |✓ |✓ | |
| Azure Data Lake Storage Gen2 |✓ |✓ ||
| Azure SQL Veritabanı |✓ | | |
| Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) |✓ | | |
| Azure SYNAPSE Analytics (çalışma alanı) SQL havuzu | Genel Önizleme | | |
| Azure Veri Gezgini | | |✓ |

## <a name="data-store-support-matrix"></a>Veri deposu destek matrisi

Azure veri paylaşımında, ' deki verileri kabul etmek için bir veri deposunda karar verirken veri tüketicileri esnekliği sunulmaktadır. Örneğin, Azure SQL veritabanı 'ndan paylaşılmakta olan veriler Azure Data Lake Store Gen2, Azure SQL veritabanı veya Azure SYNAPSE Analytics ' e alınabilir. Müşteriler, alınan bir veri paylaşımının yapılandırılmasında hangi biçimin veri alacağını seçebilirler. 

Aşağıdaki tabloda veri tüketicilerinin veri payını kabul edip yapılandırırken kullandığı farklı birleşimler ve seçimler ayrıntılı olarak verilmiştir. Veri kümesi eşlemelerini yapılandırma hakkında daha fazla bilgi için bkz. [veri kümesi eşlemelerini yapılandırma](how-to-configure-mapping.md).

| Veri deposu | Azure Blob Depolama | Azure Data Lake Storage Gen1 | Azure Data Lake Storage Gen2 | Azure SQL Veritabanı | Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) | Azure SYNAPSE Analytics (çalışma alanı) SQL havuzu | Azure Veri Gezgini
|:--- |:--- |:--- |:--- |:--- |:--- |:--- | :--- |
| Azure Blob depolama | ✓ || ✓ |||
| Azure Data Lake Storage Gen1 | ✓ | | ✓ |||
| Azure Data Lake Storage Gen2 | ✓ | | ✓ |||
| Azure SQL Veritabanı | ✓ | | ✓ | ✓ | ✓ | ✓ ||
| Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) | ✓ | | ✓ | ✓ | ✓ | ✓ ||
| Azure SYNAPSE Analytics (çalışma alanı) SQL havuzu | ✓ | | ✓ | ✓ | ✓ | ✓ ||
| Azure Veri Gezgini ||||||| ✓ |

## <a name="share-from-a-storage-account"></a>Depolama hesabından paylaşma
Azure veri paylaşımı, dosya, klasör ve dosya sistemlerinin Azure Data Lake gen1 ve Azure Data Lake Gen2 ' den paylaşılmasını destekler. Ayrıca, Azure Blob depolamadan blob, klasör ve kapsayıcı paylaşımını da destekler. Şu anda yalnızca Blok Blobu destekleniyor. Dosya sistemleri, kapsayıcılar veya klasörler anlık görüntü tabanlı paylaşımda paylaşıldığında, veri tüketicisi paylaşma verilerinin tam bir kopyasını yapmayı seçebilir veya yalnızca yeni veya güncelleştirilmiş dosyaları kopyalamak için Artımlı anlık görüntü özelliğinden yararlanın. Artımlı anlık görüntü, dosyaların son değiştirilme saatini temel alır. Aynı ada sahip varolan dosyaların üzerine yazılacak.

Lütfen [Azure Blob depolama alanından paylaşma ve veri alma ve](how-to-share-from-storage.md) ayrıntılar için Azure Data Lake Storage bakın.

## <a name="share-from-a-sql-based-source"></a>SQL tabanlı bir kaynaktan paylaşma
Azure veri paylaşımı, Azure SQL veritabanı ve Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) ve Azure SYNAPSE Analytics (çalışma alanı) adanmış SQL havuzundan tablo ve görünümlerin paylaşılmasını destekler. Azure SYNAPSE Analytics (çalışma alanı) sunucusuz SQL havuzunun paylaşılması Şu anda desteklenmiyor. Veri tüketicileri, verileri Azure Data Lake Storage 2. veya Azure Blob depolama alanına CSV veya Parquet dosyası olarak kabul edip Azure SQL veritabanı ve Azure SYNAPSE Analytics 'i de tablolar olarak kabul edebilir.

Azure Data Lake Store Gen2 veya Azure Blob depolama alanına veri kabul edildiğinde, tam anlık görüntüler zaten varsa hedef dosyanın içeriğinin üzerine yazar.
Veriler tabloya alındığında ve hedef tablo henüz yoksa, Azure veri paylaşımında kaynak şeması ile SQL tablosu oluşturulur. Aynı ada sahip bir hedef tablo zaten varsa, en son tam anlık görüntüyle bırakılır ve üzerine yazılır. Artımlı anlık görüntüler şu anda desteklenmiyor.

Ayrıntılar için lütfen [Azure SQL veritabanı ve Azure SYNAPSE Analytics 'Ten paylaşma ve veri alma](how-to-share-from-sql.md) ' ya bakın.

## <a name="share-from-azure-data-explorer"></a>Azure Veri Gezgini'nden paylaşma
Azure veri paylaşımında, Azure Veri Gezgini kümelerinden veritabanlarını yerinde paylaşma özelliği de desteklenir. Veri sağlayıcısı veritabanı veya küme düzeyinde paylaşabilir. Veritabanı düzeyinde paylaşıldığında, veri tüketicisi yalnızca veri sağlayıcısı tarafından paylaşılan belirli veritabanına erişebilir. Küme düzeyinde paylaşıldığında veri tüketicisi, veri sağlayıcısı tarafından oluşturulan gelecekteki veritabanları da dahil olmak üzere sağlayıcının kümesindeki tüm veritabanlarına erişebilir.

Paylaşılan veritabanlarına erişmek için veri tüketicisinin kendi Azure Veri Gezgini kümesine sahip olması gerekir. Veri tüketicisinin Azure Veri Gezgini kümesinin, veri sağlayıcının Azure Veri Gezgini kümesiyle aynı Azure veri merkezinde bulunması gerekir. İlişki paylaşımı oluşturulduğunda Azure veri paylaşımı, sağlayıcı ve tüketicinin Azure Veri Gezgini kümeleri arasında bir sembolik bağlantı oluşturur. Kaynak Azure Veri Gezgini kümesine toplu işlem modu kullanılarak alınan veriler, birkaç saniye içinde birkaç dakika içinde hedef kümede görünür.

Ayrıntılar için lütfen [Azure Veri Gezgini paylaşma ve veri alma](/azure/data-explorer/data-share) bilgilerini inceleyin. 

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.
