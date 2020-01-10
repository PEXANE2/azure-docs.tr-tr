---
title: Java kullanan Azure depolama örnekleri | Microsoft Docs
description: Azure depolama için örnek kod ve uygulamaları görüntüleyin, indirin ve çalıştırın. Java depolama istemci kitaplıklarını kullanarak Bloblar, kuyruklar, tablolar ve dosyalar için çalışmaya başlama örneklerini bulun.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/06/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: dfdde1ae981dcd2d539dec3667e44e90cef4d1c8
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75748037"
---
# <a name="azure-storage-samples-using-java"></a>Java kullanan Azure depolama örnekleri

Aşağıdaki tabloda, örnek havuzumuza ve her örnekte ele alınan senaryolara bir genel bakış sunulmaktadır. GitHub 'da karşılık gelen örnek kodu görüntülemek için bağlantılara tıklayın.

> [!NOTE]
> Bu örnekler Azure Storage Java v11 kitaplığını kullanır. V12 kodu için GitHub deposundaki [örneklere](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-blob/src/samples/java/com/azure/storage/blob) bakın.

## <a name="blob-samples-v11"></a>Blob örnekleri (v11)

| **Senaryo** | **Örnek Kod** |
|--------------|-----------------|
| Blob Ekle | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| Blok Blobu | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| İstemci Tarafında Şifreleme | [Java 'da Azure Istemci tarafı şifrelemesini kullanmaya başlama](https://github.com/Azure-Samples/storage-java-client-side-encryption) |
| Kopya blob'u | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| Kapsayıcı oluşturma | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| BLOB silme | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| Kapsayıcıyı Sil | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| Blob meta verileri/özellikleri/Istatistikleri | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) |
| Kapsayıcı ACL/meta veriler/Özellikler | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) |
| Sayfa aralıklarını al | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399) |
| Kira blobu/kapsayıcı | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| Blob/kapsayıcı Listele | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| Sayfa Blobu | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| SAS | [SAS testleri örneği](https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513) |
| Hizmet Özellikleri | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) |
| Anlık görüntü blobu | [Java 'da Azure Blob hizmetini kullanmaya başlama](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |

## <a name="file-samples-v11"></a>Dosya örnekleri (v11)

| **Senaryo** | **Örnek Kod** |
|--------------|-----------------|
| Paylaşımlar/dizinler/dosyalar oluşturma | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| Paylaşımları/dizinleri/dosyaları sil | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| Dizin özellikleri/meta veriler | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |
| Dosyaları indir | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| Dosya özellikleri/meta veriler/ölçümler | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |
| Dosya hizmeti özellikleri | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |
| Dizinleri ve dosyaları listeleme | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| Paylaşımları Listele | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| Özellikleri/meta verileri/Istatistikleri paylaşma | [Java 'da Azure dosya hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |

## <a name="queue-samples-v11"></a>Sıra örnekleri (v11)

| **Senaryo** | **Örnek Kod** |
|--------------|-----------------|
| İleti Ekle | [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63) |
| İstemci Tarafında Şifreleme | [Java 'da Azure Istemci tarafı şifrelemesini kullanmaya başlama](https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java) |
| Kuyruklar Oluşturma | [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| Iletiyi/kuyruğu Sil | [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| Iletiye Gözat | [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| Kuyruk ACL/Metadata/stats | [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java) |
| Kuyruk hizmeti özellikleri | [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java) |
| Güncelleştirme Iletisi | [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java)
|
## <a name="table-samples-v11"></a>Tablo örnekleri (v11)

| **Senaryo** | **Örnek Kod** |
|--------------|-----------------|
| Tablo Oluşturma | [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| Varlığı/tabloyu Sil | [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| Varlığı Ekle/Birleştir/Değiştir | [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| Sorgu varlıkları | [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| Sorgu tabloları | [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| Tablo ACL 'SI/özellikleri | [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java) |
| Varlığı Güncelleştir | [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
## <a name="azure-code-samples-library"></a>Azure kod örnekleri kitaplığı

Tüm örnek kitaplığı görüntülemek için, yerel olarak indirebileceğiniz ve çalıştırabileceğiniz Azure Storage örneklerini içeren [Azure kod örnekleri](https://azure.microsoft.com/resources/samples/?service=storage) kitaplığına gidin. Kod örneği kitaplığı,. zip biçiminde örnek kod sağlar. Alternatif olarak, her bir örnek için GitHub deposuna gözatıp kopyalayabilirsiniz.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Başlarken kılavuzlarını

Azure Storage Istemci kitaplıklarını yüklemek ve kullanmaya başlamak için yönergeler arıyorsanız aşağıdaki kılavuzlara göz atın.

* [Java 'da Azure Blob hizmetini kullanmaya başlama](../blobs/storage-quickstart-blobs-java.md)
* [Java 'da Azure kuyruk hizmeti ile çalışmaya başlama](../queues/storage-java-how-to-use-queue-storage.md)
* [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](../../cosmos-db/table-storage-how-to-use-java.md)
* [Java 'da Azure dosya hizmeti ile çalışmaya başlama](../files/storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer dillere yönelik örnekler hakkında bilgi için:

* .NET: [.NET kullanan Azure depolama örnekleri](storage-samples-dotnet.md)
* JavaScript/Node. js: [JavaScript kullanan Azure depolama örnekleri](storage-samples-javascript.md)
* Python: [Python kullanan Azure depolama örnekleri](storage-samples-python.md)
* Diğer tüm diller: [Azure Storage örnekleri](storage-samples.md)
