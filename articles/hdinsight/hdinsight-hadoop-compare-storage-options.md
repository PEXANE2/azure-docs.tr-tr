---
title: Kullanım için depolama seçeneklerini Azure HDInsight kümeleriyle karşılaştırın
description: Azure HDInsight ile depolama türlerine ve bunların nasıl çalıştığına genel bir bakış sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: b351b9a4edd81da2983cca93127513f9041716bd
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869835"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Kullanım için depolama seçeneklerini Azure HDInsight kümeleriyle karşılaştırın

HDInsight kümeleri oluştururken birkaç farklı Azure depolama hizmeti arasında seçim yapabilirsiniz:

* [Azure Storage](./overview-azure-storage.md)
* [Azure Data Lake Storage Gen2](./overview-data-lake-storage-gen1.md)
* [Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen2.md)

Bu makalede, bu depolama türleri ve benzersiz özellikleri genel bir bakış sağlar.

## <a name="storage-types-and-features"></a>Depolama türleri ve özellikleri

Aşağıdaki tablo, HDInsight'ın farklı sürümleriyle desteklenen Azure Depolama hizmetlerini özetleyilmiştir:

| Depolama hizmeti | Hesap türü | Ad Alanı Türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları | HDInsight Sürümü | Küme türü |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Genel amaçlı V2 | Hiyerarşik (dosya sistemi) | Blob | Standart | Sıcak, Serin, Arşiv | 3.6+ | Kıvılcım 2.1 ve 2.2 hariç tüm|
|Azure Storage| Genel amaçlı V2 | Nesne | Blob | Standart | Sıcak, Serin, Arşiv | 3.6+ | Tümü |
|Azure Storage| Genel amaçlı V1 | Nesne | Blob | Standart | Yok | Tümü | Tümü |
|Azure Storage| Blob Depolama** | Nesne | Blok Blob | Standart | Sıcak, Serin, Arşiv | Tümü | Tümü |
|Azure Data Lake Storage Gen1| Yok | Hiyerarşik (dosya sistemi) | Yok | Yok | Yok | 3.6 Yalnızca | HBase hariç tüm |

**HDInsight kümeleri için yalnızca ikincil depolama hesapları BlobStorage türünde olabilir ve Sayfa Blob desteklenen bir depolama seçeneği değildir.

Azure Depolama hesap türleri hakkında daha fazla bilgi için Azure [depolama hesabına genel bakış](../storage/common/storage-account-overview.md)

Azure Depolama erişim katmanları hakkında daha fazla bilgi için Azure [Blob depolama alanı: Premium (önizleme), Sıcak, Cool ve Arşiv depolama katmanlarına](../storage/blobs/storage-blob-storage-tiers.md) bakın

Birincil ve isteğe bağlı ikincil depolama için hizmet birleşimlerini kullanarak kümeler oluşturabilirsiniz. Aşağıdaki tablo, şu anda HDInsight'ta desteklenen küme depolama yapılandırmalarını özetleyecektir:

| HDInsight Sürümü | Birincil Depolama | İkincil Depolama | Destekleniyor |
|---|---|---|---|
| 3.6 & 4.0 | Genel Amaçlı V1, Genel Amaç V2 | Genel Amaçlı V1, Genel Amaçlı V2, BlobStorage(Blok Blobs) | Evet |
| 3.6 & 4.0 | Genel Amaçlı V1, Genel Amaç V2 | Data Lake Storage Gen2 | Hayır |
| 3.6 & 4.0 | Veri Gölü Depolama Gen2* | Data Lake Storage Gen2 | Evet |
| 3.6 & 4.0 | Veri Gölü Depolama Gen2* | Genel Amaçlı V1, Genel Amaçlı V2, BlobStorage(Blok Blobs) | Evet |
| 3.6 & 4.0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | Hayır |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Evet |
| 3.6 | Data Lake Storage Gen1 | Genel Amaçlı V1, Genel Amaçlı V2, BlobStorage(Blok Blobs) | Evet |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | Hayır |
| 4.0 | Data Lake Storage Gen1 | Herhangi biri | Hayır |
| 4.0 | Genel Amaçlı V1, Genel Amaç V2 | Data Lake Storage Gen1 | Hayır |

*=Bu, küme erişimi için aynı yönetilen kimliği kullanmak için kurulum olduğu sürece, bir veya birden çok Veri Gölü Depolama Gen2 hesabı olabilir.

> [!NOTE]
> Data Lake Storage Gen2 birincil depolama Spark 2.1 veya 2.2 kümeleri için desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama'ya genel bakış](./overview-azure-storage.md)
* [Azure Veri Gölü Depolama Gen1 genel bakış](./overview-data-lake-storage-gen1.md)
* [Azure Veri Gölü Depolama Gen2 genel bakış](./overview-data-lake-storage-gen2.md)
* [Azure Veri Gölü Depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
