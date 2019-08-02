---
title: Java kullanan Azure depolama örnekleri | Microsoft Docs
description: Azure depolama için örnek kod ve uygulamaları görüntüleyin, indirin ve çalıştırın. Java depolama istemci kitaplıklarını kullanarak Bloblar, kuyruklar, tablolar ve dosyalar için çalışmaya başlama örneklerini bulun.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/03/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: 4348400e66e4b975d1eeb899cab8bb5024260149
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721829"
---
# <a name="azure-storage-samples-using-java"></a>Java kullanan Azure depolama örnekleri

## <a name="java-sample-index"></a>Java örnek dizini

Aşağıdaki tabloda, örnek havuzumuza ve her örnekte ele alınan senaryolara bir genel bakış sunulmaktadır. GitHub 'da karşılık gelen örnek kodu görüntülemek için bağlantılara tıklayın.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Uç Nokta</th><th style="font-size:110%">Senaryo</th><th style="font-size:110%">Örnek Kod</th></tr></thead><tbody>
<tr>
<td rowspan="16"><b>Blob</b></td>
<td>Ekleme Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Blok Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>İstemci Tarafında Şifreleme</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Java 'da Azure Istemci tarafı şifrelemesini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Kopya blob'u</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Kapsayıcı oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>BLOB silme</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Kapsayıcıyı Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Blob meta verileri/özellikleri/Istatistikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Kapsayıcı ACL/meta veriler/Özellikler</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Sayfa aralıklarını al</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Kira blobu/kapsayıcı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Blob/kapsayıcı Listele</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Sayfa Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>'LARININ</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513">SAS testleri örneği</a></td>
</tr>   
<tr>
<td>Hizmet Özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Anlık görüntü blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java 'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr>
<td rowspan="9"><b>Dosya</b></td>
<td>Paylaşımlar/dizinler/dosyalar oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Paylaşımları/dizinleri/dosyaları sil</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Dizin özellikleri/meta veriler</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Dosyaları indir</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Dosya özellikleri/meta veriler/ölçümler</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Dosya hizmeti özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Dizinleri ve dosyaları listeleme</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Paylaşımları Listele</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Özellikleri/meta verileri/Istatistikleri paylaşma</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java 'da Azure dosya hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td rowspan="8"><b>Sıradaki</b></td>
<td>Ileti Ekle</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63">Java 'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>İstemci Tarafında Şifreleme</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java">Java 'da Azure Istemci tarafı şifrelemesini kullanmaya başlama</a></td>
</tr>
<tr>
<td>Kuyruklar Oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java 'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Iletiyi/kuyruğu Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java 'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Iletiye Gözat</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java 'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Kuyruk ACL/Metadata/stats</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Java 'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Kuyruk hizmeti özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Java 'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td>Güncelleştirme Iletisi</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java 'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr>
<td rowspan="7"><b>Tablo</b></td>
<td>Tablo Oluştur</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td>
</tr>
<tr>
<td>Varlığı/tabloyu Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td>
</tr>
<tr>
<td>Varlığı Ekle/Birleştir/Değiştir</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td>
</tr>
<tr>
<td>Sorgu varlıkları</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td>
</tr>
<tr>
<td>Sorgu tabloları</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td>
</tr>
<tr>
<td>Tablo ACL 'SI/özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td>
</tr>
<tr>
<td>Varlığı Güncelleştir</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td>
</tr>
</tbody>
</table>
<br/>

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

* .NET: [.NET kullanan Azure Depolama örnekleri](storage-samples-dotnet.md)
* Diğer tüm diller: [Azure depolama örnekleri](storage-samples.md)
