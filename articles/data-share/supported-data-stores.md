---
title: Azure veri paylaşımında desteklenen veri depoları
description: Azure veri paylaşımının kullanımı için desteklenen veri depoları hakkında bilgi edinin.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: e0daa2b02c16d8d5a65b5e7e0f983a4f47181d40
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84635977"
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
| Azure SQL Veritabanı |Genel Önizleme | |
| Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) |Genel Önizleme | |
| Azure Veri Gezgini | |Genel Önizleme |

## <a name="data-store-support-matrix"></a>Veri deposu destek matrisi

Azure veri paylaşımında, ' deki verileri kabul etmek için bir veri deposunda karar verirken veri tüketicileri esnekliği sunulmaktadır. Örneğin, Azure SQL veritabanı 'ndan paylaşılmakta olan veriler Azure Data Lake Store Gen2, Azure SQL veritabanı veya Azure SYNAPSE Analytics ' e alınabilir. Müşteriler, alınan bir veri paylaşımının yapılandırılmasında hangi biçimin veri alacağını seçebilirler. 

Aşağıdaki tabloda veri tüketicilerinin veri payını kabul edip yapılandırırken kullandığı farklı birleşimler ve seçimler ayrıntılı olarak verilmiştir. Veri kümesi eşlemelerini yapılandırma hakkında daha fazla bilgi için bkz. [veri kümesi eşlemelerini yapılandırma](how-to-configure-mapping.md).

|  | Azure Blob Depolama | Azure Data Lake Storage Gen1 | Azure Data Lake Storage Gen2 | Azure SQL Veritabanı | Azure Synapse Analytics | Azure Veri Gezgini
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama | ✓ || ✓ ||
| Azure Data Lake Storage Gen1 | ✓ | | ✓ ||
| Azure Data Lake Storage Gen2 | ✓ | | ✓ ||
| Azure SQL Veritabanı | ✓ | | ✓ | ✓ | ✓ ||
| Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) | ✓ | | ✓ | ✓ | ✓ ||
| Azure Veri Gezgini |||||| ✓ |

## <a name="share-from-a-storage-account"></a>Depolama hesabından paylaşma
Azure veri paylaşımı, dosya, klasör ve dosya sistemlerinin Azure Data Lake gen1 ve Azure Data Lake Gen2 ' den paylaşılmasını destekler. Ayrıca, Azure Blob depolamadan blob, klasör ve kapsayıcı paylaşımını da destekler. Şu anda yalnızca Blok Blobu destekleniyor. Dosya sistemleri, kapsayıcılar veya klasörler anlık görüntü tabanlı paylaşımda paylaşıldığında, veri tüketicisi paylaşma verilerinin tam bir kopyasını yapmayı seçebilir veya yalnızca yeni veya güncelleştirilmiş dosyaları kopyalamak için Artımlı anlık görüntü özelliğinden yararlanın. Artımlı anlık görüntü, dosyaların son değiştirilme saatini temel alır. Aynı ada sahip varolan dosyaların üzerine yazılacak.

## <a name="share-from-a-sql-based-source"></a>SQL tabanlı bir kaynaktan paylaşma
Azure veri paylaşımı, Azure SQL veritabanı ve Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) tabloları veya görünümlerinin paylaşılmasını destekler. Veri tüketicileri, verileri Azure Data Lake Store Gen2 veya Azure Blob depolama alanına CSV veya Parquet dosyası olarak kabul edebilir. Varsayılan olarak, dosya biçimlerinin CSV olduğunu unutmayın. Veri TÜKETİCİSİNDE, isterseniz verileri Parquet biçiminde almayı tercih edebilirsiniz. Bu, verileri alırken veri kümesi eşleme ayarlarında yapılabilir. 

Azure Data Lake Store Gen2 veya Azure Blob depolama alanına veri kabul edildiğinde, tam anlık görüntüler hedef dosyanın içeriğinin üzerine yazar. 

Bir veri tüketicisi, kendi tercih ettiği bir tabloya veri almayı tercih edebilir. Bu senaryoda, hedef tablo henüz yoksa, Azure veri paylaşımında kaynak şeması ile SQL tablosu oluşturulur. Aynı ada sahip bir hedef tablo zaten varsa, en son tam anlık görüntüyle bırakılır ve üzerine yazılır. Hedef tablo eşlerken, alternatif bir şema ve tablo adı belirtilebilir. Artımlı anlık görüntüler şu anda desteklenmiyor. 

SQL tabanlı kaynaklardan paylaşım, güvenlik duvarı kuralları ve izinleriyle ilgili önkoşulları içerir. Ayrıntılar için lütfen [verilerinizi paylaşma](share-your-data.md) öğreticisinin ön koşul bölümüne bakın.

## <a name="share-from-azure-data-explorer"></a>Azure Veri Gezgini paylaşma
Azure veri paylaşımında, Azure Veri Gezgini kümelerinden veritabanlarını yerinde paylaşma özelliği de desteklenir. Veri sağlayıcısı veritabanı veya küme düzeyinde paylaşabilir. Veritabanı düzeyinde paylaşıldığında, veri tüketicisi yalnızca veri sağlayıcısı tarafından paylaşılan belirli veritabanına erişebilir. Küme düzeyinde paylaşıldığında veri tüketicisi, veri sağlayıcısı tarafından oluşturulan gelecekteki veritabanları da dahil olmak üzere sağlayıcının kümesindeki tüm veritabanlarına erişebilir.

Paylaşılan veritabanlarına erişmek için veri tüketicisinin kendi Azure Veri Gezgini kümesine sahip olması gerekir. Veri tüketicisinin Azure Veri Gezgini kümesinin, veri sağlayıcının Azure Veri Gezgini kümesiyle aynı Azure veri merkezinde bulunması gerekir. İlişki paylaşımı oluşturulduğunda Azure veri paylaşımı, sağlayıcı ve tüketicinin Azure Veri Gezgini kümeleri arasında bir sembolik bağlantı oluşturur.

Azure Veri Gezgini, iki veri alma modunu destekler: Batch ve streaming. Paylaşılan veritabanındaki Batch 'ten alınan veriler, veri tüketici tarafında birkaç saniye ile birkaç dakika arasında görünür. Akışdan alınan verilerin veri tüketici tarafında görünmesi 24 saate kadar sürebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.
