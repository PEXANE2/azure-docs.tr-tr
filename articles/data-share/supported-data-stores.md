---
title: Azure veri paylaşımında desteklenen veri depoları
description: Azure veri paylaşımının kullanımı için desteklenen veri depoları hakkında bilgi edinin.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 5d4b1282b0a08657aea6f8a13aae7ed1fe49079b
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964218"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Azure veri paylaşımında desteklenen veri depoları

Azure veri paylaşımı, ve farklı veri depolarından paylaşma özelliği de dahil olmak üzere açık ve esnek veri paylaşımı sağlar. Veri sağlayıcıları verileri bir veri deposu türünden paylaşabilir ve veri tüketicileri verileri hangi veri deposuna alacağını seçebilirler. 

Bu makalede, Azure veri paylaşımında desteklenen zengin Azure veri depoları kümesi hakkında bilgi edineceksiniz. Ayrıca, veri sağlayıcılarının ve veri tüketicilerinin yararlanılabilir olabilecek veri depolarının birleşimleri hakkında bilgi bulabilirsiniz. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Azure veri paylaşımında hangi veri depoları desteklenir? 

Aşağıdaki tabloda, Azure veri paylaşımında desteklenen veri kaynaklarının ayrıntıları verilmiştir. 

| Veri deposu | Anlık görüntü tabanlı paylaşım | Yerinde paylaşım 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama |✓ | |
| Azure Data Lake Storage Gen1 |✓ | |
| Azure Data Lake Storage Gen2 |✓ ||
| Azure SQL Veritabanı |Genel önizlemeye sunuldu | |
| Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) |Genel önizlemeye sunuldu | |
| Azure Veri Gezgini | |Genel önizlemeye sunuldu |

## <a name="data-store-support-matrix"></a>Veri deposu destek matrisi

Azure veri paylaşımında, ' deki verileri kabul etmek için bir veri deposunda karar verirken veri tüketicileri esnekliği sunulmaktadır. Örneğin, Azure SQL veritabanı 'ndan paylaşılmakta olan veriler Azure Data Lake Store Gen2, Azure SQL veritabanı veya Azure SYNAPSE Analytics ' e alınabilir. Müşteriler, alınan bir veri paylaşımının yapılandırılmasında hangi biçimin veri alacağını seçebilirler. 

Aşağıdaki tabloda veri tüketicilerinin veri payını kabul edip yapılandırırken kullandığı farklı birleşimler ve seçimler ayrıntılı olarak verilmiştir. Veri kümesi eşlemelerini yapılandırma hakkında daha fazla bilgi için bkz. [veri kümesi eşlemelerini yapılandırma](how-to-configure-mapping.md).

|  | Azure Blob Depolama | Azure Data Lake Storage Gen1 | Azure Data Lake Storage Gen2 | Azure SQL Veritabanı | Azure Synapse Analytics 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama | ✓ || ✓|
| Azure Data Lake Storage Gen1 | ✓ | | ✓|
| Azure Data Lake Storage Gen2 | ✓ | | ✓|
| Azure SQL Veritabanı | ✓ | | ✓| ✓| ✓|
| Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) | ✓ | | ✓| ✓| ✓|

## <a name="share-from-a-storage-account"></a>Depolama hesabından paylaşma
Azure veri paylaşımı, dosya, klasör ve dosya sistemlerinin Azure Data Lake gen1 ve Azure Data Lake Gen2 ' den paylaşılmasını destekler. Ayrıca, Azure Blob depolamadan blob, klasör ve kapsayıcı paylaşımını da destekler. Klasörler anlık görüntü tabanlı paylaşımda paylaşıldığında, veri tüketicisinin paylaşım verilerinin tam bir kopyasını yapmayı seçebilir veya yalnızca yeni veya güncelleştirilmiş dosyaları kopyalamak için Artımlı anlık görüntü özelliğinden faydalanabilirsiniz. Aynı ada sahip varolan dosyaların üzerine yazılacak.

## <a name="share-from-a-sql-based-source"></a>SQL tabanlı bir kaynaktan paylaşma
Azure veri paylaşımı, Azure SQL veritabanı ve Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) tabloları veya görünümlerinin paylaşılmasını destekler. Veri tüketicisi, verileri Azure Data Lake Storage 2. veya Azure Blob depolama alanına CSV veya Parquet dosyası olarak kabul edebilir. Tam anlık görüntü, hedef dosyanın içeriğinin üzerine yazar. Alternatif olarak, veri tüketicisi verileri bir SQL tablosuna kabul edebilir. Hedef SQL tablosu veri tüketicisi tarafında yoksa, Azure veri paylaşımında kaynak şeması ile SQL tablosu oluşturulur. Tam anlık görüntü, kaynak tablonun içeriğini hedef SQL tablosuna ekler. Artımlı anlık görüntü şu anda desteklenmiyor.

## <a name="share-from-azure-data-explorer"></a>Azure Veri Gezgini paylaşma
Azure veri paylaşımında, Azure Veri Gezgini kümelerinden veritabanlarını yerinde paylaşma özelliği de desteklenir. Veri sağlayıcısı veritabanı veya küme düzeyinde paylaşabilir. Veritabanı düzeyinde paylaşıldığında, veri tüketicisi yalnızca veri sağlayıcısı tarafından paylaşılan belirli veritabanına erişebilir. Küme düzeyinde paylaşıldığında veri tüketicisi, veri sağlayıcısı tarafından oluşturulan gelecekteki veritabanları da dahil olmak üzere sağlayıcının kümesindeki tüm veritabanlarına erişebilir.

Paylaşılan veritabanlarına erişmek için veri tüketicisinin kendi Azure Veri Gezgini kümesine sahip olması gerekir. Veri tüketicisinin Azure Veri Gezgini kümesinin, veri sağlayıcının Azure Veri Gezgini kümesiyle aynı Azure veri merkezinde bulunması gerekir. İlişki paylaşımı oluşturulduğunda Azure veri paylaşımı, sağlayıcı ve tüketicinin Azure Veri Gezgini kümeleri arasında bir sembolik bağlantı oluşturur.

Azure Veri Gezgini, iki veri alma modunu destekler: Batch ve streaming. Paylaşılan veritabanındaki Batch 'ten alınan veriler, veri tüketici tarafında birkaç saniye ile birkaç dakika arasında görünür. Akışdan alınan verilerin veri tüketici tarafında görünmesi 24 saate kadar sürebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.
