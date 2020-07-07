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
ms.openlocfilehash: ed93ba937a843618f36bac6e88b15ff77355ca75
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82610709"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Depolama seçeneklerini Azure HDInsight kümeleriyle kullanım için karşılaştırın

HDInsight kümeleri oluştururken birkaç farklı Azure depolama hizmeti arasında seçim yapabilirsiniz:

* [Azure Depolama](./overview-azure-storage.md)
* [Azure Data Lake Storage 2. Nesil](./overview-data-lake-storage-gen2.md)
* [Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen1.md)

Bu makale, bu depolama türlerine ve bunların benzersiz özelliklerine genel bir bakış sağlar.

## <a name="storage-types-and-features"></a>Depolama türleri ve özellikleri

Aşağıdaki tabloda farklı HDInsight sürümleriyle desteklenen Azure depolama hizmetleri özetlenmektedir:

| Depolama hizmeti | Hesap türü | Ad alanı türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları | HDInsight sürümü | Küme türü |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Genel amaçlı v2 | Hiyerarşik (dosya sistemi) | Blob | Standart | Sık erişimli, seyrek erişimli Arşiv | 3.6 + | Spark 2,1 ve 2,2 dışında tümü|
|Azure Depolama| Genel amaçlı v2 | Nesne | Blob | Standart | Sık erişimli, seyrek erişimli Arşiv | 3.6 + | Tümü |
|Azure Depolama| Genel amaçlı v1 | Nesne | Blob | Standart | Yok | Tümü | Tümü |
|Azure Depolama| BLOB depolama * * | Nesne | Blok Blobu | Standart | Sık erişimli, seyrek erişimli Arşiv | Tümü | Tümü |
|Azure Data Lake Storage 1. Nesil| Yok | Hiyerarşik (dosya sistemi) | Yok | Yok | Yok | yalnızca 3,6 | HBase dışında tümü |

* * HDInsight kümeleri için yalnızca ikincil depolama hesapları BlobStorage türünde olabilir ve Sayfa Blobu desteklenen bir depolama seçeneği değildir.

Azure depolama hesabı türleri hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md)

Azure depolama erişim katmanları hakkında daha fazla bilgi için bkz [. Azure Blob depolama: Premium (Önizleme), sık erişimli, seyrek erişimli ve arşiv depolama katmanları](../storage/blobs/storage-blob-storage-tiers.md)

Birincil ve isteğe bağlı ikincil depolama için hizmet birleşimlerini kullanarak kümeler oluşturabilirsiniz. Aşağıdaki tabloda, HDInsight 'ta Şu anda desteklenen küme depolama yapılandırmalarının özeti verilmiştir:

| HDInsight sürümü | Birincil depolama alanı | İkincil depolama | Destekleniyor |
|---|---|---|---|
| 3,6 & 4,0 | Genel Amaçlı v1, Genel Amaçlı v2 | Genel Amaçlı v1, Genel Amaçlı v2, BlobStorage (blok Blobları) | Yes |
| 3,6 & 4,0 | Genel Amaçlı v1, Genel Amaçlı v2 | Data Lake Storage 2. Nesil | No |
| 3,6 & 4,0 | Data Lake Storage 2. * | Data Lake Storage 2. Nesil | Yes |
| 3,6 & 4,0 | Data Lake Storage 2. * | Genel Amaçlı v1, Genel Amaçlı v2, BlobStorage (blok Blobları) | Yes |
| 3,6 & 4,0 | Data Lake Storage 2. Nesil | Data Lake Storage 1. Nesil | No |
| 3,6 | Data Lake Storage 1. Nesil | Data Lake Storage 1. Nesil | Yes |
| 3,6 | Data Lake Storage 1. Nesil | Genel Amaçlı v1, Genel Amaçlı v2, BlobStorage (blok Blobları) | Yes |
| 3,6 | Data Lake Storage 1. Nesil | Data Lake Storage 2. Nesil | No |
| 4.0 | Data Lake Storage 1. Nesil | Herhangi biri | No |
| 4.0 | Genel Amaçlı v1, Genel Amaçlı v2 | Data Lake Storage 1. Nesil | No |

* = Bu, bir veya birden çok Data Lake Storage 2. hesabı olabilir, çünkü tüm kurulum, küme erişimi için aynı yönetilen kimliği kullanır.

> [!NOTE]
> Spark 2,1 veya 2,2 kümelerinde birincil depolama Data Lake Storage 2. desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama’ya genel bakış](./overview-azure-storage.md)
* [Azure Data Lake Storage 1. Nesil’e genel bakış](./overview-data-lake-storage-gen1.md)
* [Azure Data Lake Storage 2. Nesil’e genel bakış](./overview-data-lake-storage-gen2.md)
* [Azure Data Lake Storage 2. giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
