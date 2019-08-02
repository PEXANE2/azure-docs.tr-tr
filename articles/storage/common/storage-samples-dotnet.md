---
title: .NET kullanarak Azure Storage örnekleri | Microsoft Docs
description: Azure depolama için örnek kod ve uygulamaları görüntüleyin, indirin ve çalıştırın. .NET depolama istemci kitaplıklarını kullanarak blob, kuyruk, tablo ve dosya kullanmaya başlama örneklerini bulun.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/03/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: cec6b0498adf55428fade7ae00db6550496a20bd
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721846"
---
# <a name="azure-storage-samples-using-net"></a>.NET kullanan Azure depolama örnekleri

## <a name="net-sample-index"></a>.NET örnek dizini

Aşağıdaki tabloda, örnek havuzumuza ve her örnekte ele alınan senaryolara bir genel bakış sunulmaktadır. GitHub 'da karşılık gelen örnek kodu görüntülemek için bağlantılara tıklayın.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Uç Nokta</th><th style="font-size:110%">Senaryo</th><th style="font-size:110%">Örnek Kod</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Blob</b></td>
<td>Ekleme Blobu</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs#L1144">Bloblarla çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Blok Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td>
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Blob şifreleme örnekleri</a></td>
</tr> 
<tr> 
<td>Kopya blob'u</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Bloblarla çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>BLOB silme</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>Kapsayıcıyı Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Bloblarla çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Blob meta verileri/özellikleri/Istatistikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Bloblarla çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı ACL/meta veriler/Özellikler</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>Sayfa aralıklarını al</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Bloblarla çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kira blobu/kapsayıcı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Bloblarla çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Blob/kapsayıcı Listele</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Bloblarla çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Sayfa Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Bloblarla çalışmaya başlama</a></td>
</tr>
<tr> 
<td>'LARININ</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Bloblarla çalışmaya başlama</a></td>
</tr>   
<tr> 
<td>Hizmet Özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Bloblarla çalışmaya başlama</a></td>
</tr>           
<tr> 
<td>Anlık görüntü blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Artımlı anlık görüntülerle Azure sanal makine disklerini yedekleme</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Dosya</b></td>
<td>Paylaşımlar/dizinler/dosyalar oluşturma</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td>Paylaşımları/dizinleri/dosyaları sil</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">.NET ' te Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dizin özellikleri/meta veriler</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Dosyaları indir</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Dosya özellikleri/meta veriler/ölçümler</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Dosya hizmeti özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Dizinleri ve dosyaları listeleme</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td>Paylaşımları Listele</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td>Özellikleri/meta verileri/Istatistikleri paylaşma</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Sıradaki</b></td>
<td>Ileti Ekle</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET kuyruğu Istemci tarafı şifrelemesi</a></td> 
</tr> 
<tr> 
<td>Kuyruklar Oluşturma</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Iletiyi/kuyruğu Sil</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Iletiye Gözat</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Kuyruk ACL/Metadata/stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Kuyruk hizmeti özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Güncelleştirme Iletisi</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tablo</b></td>
<td>Tablo Oluştur</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure depolama kullanarak eşzamanlılık yönetme-örnek uygulama</a></td> 
</tr> 
<tr> 
<td>Varlığı/tabloyu Sil</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Varlığı Ekle/Birleştir/Değiştir</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure depolama kullanarak eşzamanlılık yönetme-örnek uygulama</a></td> 
</tr> 
<tr> 
<td>Sorgu varlıkları</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Sorgu tabloları</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Tablo ACL 'SI/özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Varlığı Güncelleştir</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure depolama kullanarak eşzamanlılık yönetme-örnek uygulama</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure kod örnekleri kitaplığı

Tüm örnek kitaplığı görüntülemek için, yerel olarak indirebileceğiniz ve çalıştırabileceğiniz Azure Storage örneklerini içeren [Azure kod örnekleri](https://azure.microsoft.com/resources/samples/?service=storage) kitaplığına gidin. Kod örneği kitaplığı,. zip biçiminde örnek kod sağlar. Alternatif olarak, her bir örnek için GitHub deposuna gözatıp kopyalayabilirsiniz.

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a>Başlarken kılavuzlarını

Azure Storage Istemci kitaplıklarını yüklemek ve kullanmaya başlamak için yönergeler arıyorsanız aşağıdaki kılavuzlara göz atın.

* [.NET ' te Azure Blob hizmetini kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md)
* [.NET ' te Azure kuyruk hizmeti ile çalışmaya başlama](../storage-dotnet-how-to-use-queues.md)
* [.NET ' te Azure Tablo hizmeti ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [.NET ' te Azure dosya hizmeti ile çalışmaya başlama](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer dillere yönelik örnekler hakkında bilgi için:

* Java: [Java kullanan Azure Depolama örnekleri](storage-samples-java.md)
* Diğer tüm diller: [Azure depolama örnekleri](../storage-samples.md)
