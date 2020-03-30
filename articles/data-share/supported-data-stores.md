---
title: Azure Veri Paylaşımı'nda desteklenen veri depoları
description: Azure Veri Paylaşımı'nı kullanmak üzere desteklenen veri depoları hakkında bilgi edinin.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 624bb45de3e2ff184326949611d437f71f3e2def
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501813"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Azure Veri Paylaşımı'nda desteklenen veri depoları

Azure Veri Paylaşımı, farklı veri depolarından ve farklı veri depolarından paylaşım yapma olanağı da dahil olmak üzere açık ve esnek veri paylaşımı sağlar. Veri sağlayıcıları bir veri deposu türünden veri paylaşabilir ve veri tüketicileri hangi veri deposuna veri alacaklarını seçebilirler. 

Bu makalede, Azure Veri Paylaşımı'nda desteklenen zengin Azure veri depoları kümesi hakkında bilgi edineceksiniz. Ayrıca, veri sağlayıcıları ve veri tüketicileri tarafından yararlanılabilen veri depolarının kombinasyonları hakkında da bilgi bulabilirsiniz. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Azure Veri Paylaşımı'nda hangi veri depoları desteklenir? 

Aşağıdaki tabloda Azure Veri Paylaşımı için desteklenen veri kaynakları ayrıntılı olarak anlatılır. 

| Veri deposu | Anlık görüntü tabanlı paylaşım | Yerinde paylaşım 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama |✓ | |
| Azure Data Lake Storage Gen1 |✓ | |
| Azure Data Lake Storage Gen2 |✓ ||
| Azure SQL Veritabanı |Genel Önizleme | |
| Azure Synapse Analytics (eski adıyla Azure SQL DW) |Genel Önizleme | |
| Azure Veri Gezgini | |Genel Önizleme |

## <a name="data-store-support-matrix"></a>Veri depolama destek matrisi

Azure Veri Paylaşımı, veri tüketicilerine veri deposunda veri kabul etmeye karar verirken esneklik sunar. Örneğin, Azure SQL Veritabanı'ndan paylaşılan veriler Azure Data Lake Store Gen2, Azure SQL Veritabanı veya Azure Synapse Analytics'e alınabilir. Müşteriler alınan bir veri paylaşımını yapılandırırken hangi formatta veri alınabileceğini seçebilir. 

Aşağıdaki tablo, tüketicilerin veri paylarını kabul ederken ve yapılandırırken sahip olduğu farklı kombinasyonları ve seçenekleri ayrıntılarıyla belirtin. Veri kümesi eşlemeleri yapılandırma hakkında daha fazla bilgi [için, veri kümesi eşlemelerini nasıl yapılandırılabilirsiniz.](how-to-configure-mapping.md)

|  | Azure Blob Depolama | Azure Data Lake Storage Gen1 | Azure Data Lake Storage Gen2 | Azure SQL Veritabanı | Azure Synapse Analytics 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama | ✓ || ✓|
| Azure Data Lake Storage Gen1 | ✓ | | ✓|
| Azure Data Lake Storage Gen2 | ✓ | | ✓|
| Azure SQL Veritabanı | ✓ | | ✓| ✓| ✓|
| Azure Synapse Analytics (eski adıyla Azure SQL DW) | ✓ | | ✓| ✓| ✓|

## <a name="share-from-a-storage-account"></a>Depolama hesabından paylaşma
Azure Veri Paylaşımı, Azure Data Lake Gen1 ve Azure Data Lake Gen2'den dosya, klasör ve dosya sistemlerinin paylaşımını destekler. Ayrıca Azure Blob Depolama'dan blobs, klasörler ve kapsayıcılar paylaşımını destekler. Yalnızca blok blob şu anda desteklenir. Klasörler anlık görüntü tabanlı paylaşımda paylaşıldığında, veri tüketicisi paylaşım verilerinin tam kopyasını yapmayı seçebilir veya yalnızca yeni veya güncelleştirilmiş dosyaları kopyalamak için artımlı anlık görüntü yeteneğinden yararlanabilirsiniz. Aynı ada sahip varolan dosyalar üzerine yazılır.

## <a name="share-from-a-sql-based-source"></a>SQL tabanlı bir kaynaktan paylaşım
Azure Veri Paylaşımı, Azure SQL Veritabanı ve Azure Synapse Analytics'ten (eski adıyla Azure SQL DW) tabloların veya görünümlerin paylaşılmasını destekler. Veri tüketicileri verileri Azure Veri Gölü Deposu Gen2 veya Azure Blob Depolama'da csv veya parke dosyası olarak kabul etmeyi seçebilir. Varsayılan olarak, dosya biçimlericsv olduğunu unutmayın. Veri tüketicisi istenirse verileri parke formatında almayı seçebilir. Bu, verileri alırken veri kümesi eşleme ayarlarında yapılabilir. 

Azure Veri Gölü Deposu Gen2 veya Azure Blob Depolama'da veri kabul ederken, tam anlık görüntüler hedef dosyanın içeriğinin üzerine yazar. 

Veri tüketicisi, kendi seçtikleri bir tabloya veri almayı seçebilir. Bu senaryoda, hedef tablo zaten yoksa, Azure Veri Paylaşımı kaynak şemasıyla SQL tablosunu oluşturur. Bir hedef tablo zaten aynı ada sahipse, en son tam anlık görüntüyle bırakılır ve üzerine yazılır. Hedef tabloeşleme yaparken, alternatif bir şema ve tablo adı belirtilebilir. Artımlı anlık görüntüler şu anda desteklenmez. 

SQL tabanlı kaynaklardan paylaşım, güvenlik duvarı kuralları ve izinleri ile ilgili ön koşullara sahiptir. Ayrıntılar için [lütfen veri öğreticinizi paylaşmanın](share-your-data.md) ön koşullar bölümüne bakın.

## <a name="share-from-azure-data-explorer"></a>Azure Veri Gezgini'nden Paylaş
Azure Veri Paylaşımı, veritabanlarını Azure Veri Gezgini kümelerinden yerinde paylaşma özelliğini destekler. Veri sağlayıcısı veritabanı veya küme düzeyinde paylaşabilirsiniz. Veritabanı düzeyinde paylaşıldığında, veri tüketicisi yalnızca veri sağlayıcısı tarafından paylaşılan belirli veritabanına(lar) erişebilir. Küme düzeyinde paylaşıldığında, veri tüketicisi veri sağlayıcısı tarafından oluşturulan gelecekteki veritabanları da dahil olmak üzere sağlayıcının kümesinden tüm veritabanlarına erişebilir.

Paylaşılan veritabanlarına erişmek için veri tüketicisinin kendi Azure Veri Gezgini kümesine sahip olması gerekir. Veri tüketicisinin Azure Veri Gezgini kümesinin veri sağlayıcısının Azure Veri Gezgini kümesiyle aynı Azure veri merkezinde bulunması gerekir. Paylaşım ilişkisi kurulduğunda, Azure Veri Paylaşımı sağlayıcı ile tüketicinin Azure Veri Gezgini kümeleri arasında sembolik bir bağlantı oluşturur.

Azure Veri Gezgini iki veri alma modunu destekler: toplu iş ve akış. Paylaşılan veritabanında toplu işlemden alınan veriler, veri tüketici tarafında birkaç saniye ile birkaç dakika arasında görünür. Akıştan alınan verilerin veri tüketici tarafında görünmesi 24 saat kadar sürebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Veri paylaşmaya nasıl başlayacağınızı öğrenmek [için, veri öğreticinizi paylaşmaya devam edin.](share-your-data.md)
