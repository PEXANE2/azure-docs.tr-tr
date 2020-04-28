---
title: Depolama seçeneklerini Azure HDInsight kümeleriyle kullanım için karşılaştırın
description: Depolama türlerine genel bir bakış ve bunların Azure HDInsight ile nasıl çalıştıkları hakkında bilgi sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: bfbe311d9768923eee8c1b0cc4f3b4ec1a7ad69a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82186314"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Depolama seçeneklerini Azure HDInsight kümeleriyle kullanım için karşılaştırın

HDInsight kümeleri oluştururken birkaç farklı Azure depolama hizmeti arasında seçim yapabilirsiniz:

* [Azure Storage](./overview-azure-storage.md)
* [Azure Data Lake Storage Gen2](./overview-data-lake-storage-gen1.md)
* [Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen2.md)

Bu makale, bu depolama türlerine ve bunların benzersiz özelliklerine genel bir bakış sağlar.

## <a name="storage-types-and-features"></a>Depolama türleri ve özellikleri

Aşağıdaki tabloda farklı HDInsight sürümleriyle desteklenen Azure depolama hizmetleri özetlenmektedir:

| Depolama hizmeti | Hesap türü | Ad alanı türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları | HDInsight sürümü | Küme türü |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Genel amaçlı v2 | Hiyerarşik (dosya sistemi) | Blob | Standart | Sık erişimli, seyrek erişimli Arşiv | 3.6 + | Spark 2,1 ve 2,2 dışında tümü|
|Azure Storage| Genel amaçlı v2 | Nesne | Blob | Standart | Sık erişimli, seyrek erişimli Arşiv | 3.6 + | Tümü |
|Azure Storage| Genel amaçlı v1 | Nesne | Blob | Standart | Yok | Tümü | Tümü |
|Azure Storage| BLOB depolama * * | Nesne | Blok Blobu | Standart | Sık erişimli, seyrek erişimli Arşiv | Tümü | Tümü |
|Azure Data Lake Storage Gen1| Yok | Hiyerarşik (dosya sistemi) | Yok | Yok | Yok | yalnızca 3,6 | HBase dışında tümü |

* * HDInsight kümeleri için yalnızca ikincil depolama hesapları BlobStorage türünde olabilir ve Sayfa Blobu desteklenen bir depolama seçeneği değildir.

Azure depolama hesabı türleri hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md)

Azure depolama erişim katmanları hakkında daha fazla bilgi için bkz [. Azure Blob depolama: Premium (Önizleme), sık erişimli, seyrek erişimli ve arşiv depolama katmanları](../storage/blobs/storage-blob-storage-tiers.md)

Birincil ve isteğe bağlı ikincil depolama için hizmet birleşimlerini kullanarak kümeler oluşturabilirsiniz. Aşağıdaki tabloda, HDInsight 'ta Şu anda desteklenen küme depolama yapılandırmalarının özeti verilmiştir:

| HDInsight sürümü | Birincil depolama alanı | İkincil depolama | Destekleniyor |
|---|---|---|---|
| 3,6 & 4,0 | Genel Amaçlı v1, Genel Amaçlı v2 | Genel Amaçlı v1, Genel Amaçlı v2, BlobStorage (blok Blobları) | Yes |
| 3,6 & 4,0 | Genel Amaçlı v1, Genel Amaçlı v2 | Data Lake Storage Gen2 | Hayır |
| 3,6 & 4,0 | Data Lake Storage 2. * | Data Lake Storage Gen2 | Yes |
| 3,6 & 4,0 | Data Lake Storage 2. * | Genel Amaçlı v1, Genel Amaçlı v2, BlobStorage (blok Blobları) | Yes |
| 3,6 & 4,0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | Hayır |
| 3,6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Yes |
| 3,6 | Data Lake Storage Gen1 | Genel Amaçlı v1, Genel Amaçlı v2, BlobStorage (blok Blobları) | Yes |
| 3,6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | Hayır |
| 4.0 | Data Lake Storage Gen1 | Herhangi biri | Hayır |
| 4.0 | Genel Amaçlı v1, Genel Amaçlı v2 | Data Lake Storage Gen1 | Hayır |

* = Bu, bir veya birden çok Data Lake Storage 2. hesabı olabilir, çünkü tüm kurulum, küme erişimi için aynı yönetilen kimliği kullanır.

> [!NOTE]
> Spark 2,1 veya 2,2 kümelerinde birincil depolama Data Lake Storage 2. desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama’ya genel bakış](./overview-azure-storage.md)
* [Azure Data Lake Storage 1. Nesil’e genel bakış](./overview-data-lake-storage-gen1.md)
* [Azure Data Lake Storage 2. Nesil’e genel bakış](./overview-data-lake-storage-gen2.md)
* [Azure Data Lake Storage 2. giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
