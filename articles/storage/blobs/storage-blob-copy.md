---
title: .NET ile blob kopyalama - Azure Depolama
description: .NET istemci kitaplığını kullanarak Azure Depolama hesabınızdaki bir blob'u nasıl kopyalayacağım öğrenin.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/20/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 9ffa69980f020580376aea447f40ac615f26cf03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79135896"
---
# <a name="copy-a-blob-with-net"></a>.NET ile bir blob kopyalayın

Bu makalede, bir blob'un Azure Depolama hesabıyla nasıl kopyalanış edilebildiğini gösterin. Ayrıca, bir eşzamanlı kopyalama işleminin nasıl iptal edilebildiğini de gösterir. Örnek [kod,.NET için Azure Depolama istemci kitaplığını](/dotnet/api/overview/azure/storage?view=azure-dotnet)kullanır.

## <a name="about-copying-blobs"></a>Lekeleri kopyalama hakkında

Aynı depolama hesabı içinde bir blob kopyaladığığınızda, bu bir senkron işlemdir. Hesaplar arasında kopyaladiğinizde bu bir eşzamanlı işlemdir. [StartCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy?view=azure-dotnet) ve [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync?view=azure-dotnet) yöntemleri, durumu denetlemek veya kopyalama işlemini iptal etmek için kullanılan bir kopya kimliği değerini döndürür.

Kopyalama işlemi için kaynak blob bir blok blob, bir ek blob, bir sayfa blob veya anlık görüntü olabilir. Hedef blob zaten varsa, kaynak blob aynı blob türünde olmalıdır. Varolan herhangi bir hedef blob üzerine yazılır. 

Bir kopyalama işlemi devam ederken hedef blob değiştirilemez. Bir hedef blob yalnızca bir olağanüstü kopya blob işlemi olabilir. Başka bir deyişle, bir blob bekleyen birden çok kopyalama işlemleri için hedef olamaz.

Tüm kaynak blob veya dosya her zaman kopyalanır. Bir dizi bayt veya blok kopyalama sı desteklenmez.

Bir blob kopyalandığında, sistem özellikleri aynı değerlere sahip hedef blob kopyalanır.

Tüm blob türleri için, kopyalama işleminin durumunu almak için hedef blob'daki [CopyState.Status](/dotnet/api/microsoft.azure.storage.blob.copystate.status?view=azure-dotnet) özelliğini kontrol edebilirsiniz. Kopya tamamlandığında son leke işlenir.

Kopyalama işlemi aşağıdaki formlardan herhangi birini alabilir:

  - Bir kaynak blob'u farklı bir ada sahip bir hedef blob'a kopyalayabilirsiniz. Hedef blob aynı blob türünde (blok, ek veya sayfa) varolan bir blob olabilir veya kopyalama işlemi tarafından oluşturulan yeni bir blob olabilir.
  - Bir kaynak blob'u aynı ada sahip bir hedef blob'una kopyalayarak hedef blob'u etkin bir şekilde değiştirebilirsiniz. Böyle bir kopyalama işlemi, işlenmemiş blokları kaldırır ve hedef blob meta verilerinin üzerine yazar.
  - Azure Dosyası hizmetindeki bir kaynak dosyayı hedef blob'a kopyalayabilirsiniz. Hedef blob varolan bir blok blob olabilir veya kopyalama işlemi tarafından oluşturulan yeni bir blok blob olabilir. Dosyalardan sayfa blobs veya ek blobs kopyalama desteklenmez.
  - Bir anlık resmi taban damlasının üzerine kopyalayabilirsiniz. Temel blob konumuna bir anlık görüntü tanıtarak, bir blob önceki bir sürümünü geri yükleyebilirsiniz.
  - Anlık görüntü, farklı bir ada sahip bir hedef blob'a kopyalayabilirsiniz. Ortaya çıkan hedef blob yazılabilir bir leke değil, bir anlık görüntüdür.

## <a name="copy-a-blob"></a>Bir leke kopyalama

Bir blob kopyalamak için aşağıdaki yöntemlerden birini arayın:

 - [Başlangıç Fotokopisi](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy?view=azure-dotnet)
 - [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync?view=azure-dotnet)

Aşağıdaki kod örneği, daha önce oluşturulmuş bir blob için bir başvuru alır ve aynı kapsayıcıda yeni bir blob kopyalar:

```csharp
private static async Task CopyBlockBlobAsync(CloudBlobContainer container)
{
    CloudBlockBlob sourceBlob = null;
    CloudBlockBlob destBlob = null;
    string leaseId = null;

    try
    {
        // Get a block blob from the container to use as the source.
        sourceBlob = container.ListBlobs().OfType<CloudBlockBlob>().FirstOrDefault();

        // Lease the source blob for the copy operation to prevent another client from modifying it.
        // Specifying null for the lease interval creates an infinite lease.
        leaseId = await sourceBlob.AcquireLeaseAsync(null);

        // Get a reference to a destination blob (in this case, a new blob).
        destBlob = container.GetBlockBlobReference("copy of " + sourceBlob.Name);

        // Ensure that the source blob exists.
        if (await sourceBlob.ExistsAsync())
        {
            // Get the ID of the copy operation.
            string copyId = await destBlob.StartCopyAsync(sourceBlob);

            // Fetch the destination blob's properties before checking the copy state.
            await destBlob.FetchAttributesAsync();

            Console.WriteLine("Status of copy operation: {0}", destBlob.CopyState.Status);
            Console.WriteLine("Completion time: {0}", destBlob.CopyState.CompletionTime);
            Console.WriteLine("Bytes copied: {0}", destBlob.CopyState.BytesCopied.ToString());
            Console.WriteLine("Total bytes: {0}", destBlob.CopyState.TotalBytes.ToString());
            Console.WriteLine();
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
    finally
    {
        // Break the lease on the source blob.
        if (sourceBlob != null)
        {
            await sourceBlob.FetchAttributesAsync();

            if (sourceBlob.Properties.LeaseState != LeaseState.Available)
            {
                await sourceBlob.BreakLeaseAsync(new TimeSpan(0));
            }
        }
    }
}
```

## <a name="abort-a-blob-copy-operation"></a>Blob kopyalama işlemini iptal etme

Kopyalama işleminin iptali, blok lekeleri, ek blob'lar ve sayfa lekeleri için sıfır uzunlukta bir hedef blob ile sonuçlanır. Ancak, hedef blob için meta veriler kaynak blob kopyalanan veya [StartCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy?view=azure-dotnet) veya [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync?view=azure-dotnet) arama açıkça ayarlanmış yeni değerlere sahip olacaktır. Orijinal meta verileri kopyalamadan önce tutmak için, aramadan `StartCopy` önce hedef `StartCopyAsync`blob'un anlık görüntüsünü yapın veya .

Devam eden bir blob kopyalama işlemini iptal ettiğinizde, hedef blob'un [CopyState.Status'u](/dotnet/api/microsoft.azure.storage.blob.copystate.status?view=azure-dotnet#Microsoft_Azure_Storage_Blob_CopyState_Status) [CopyStatus.Aborted](/dotnet/api/microsoft.azure.storage.blob.copystatus?view=azure-dotnet)olarak ayarlanır.

[AbortCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopy?view=azure-dotnet) ve [AbortCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopyasync?view=azure-dotnet) yöntemleri devam eden bir blob kopyalama işlemini iptal eder ve sıfır uzunlukta ve tam meta veriiçeren bir hedef blob bırakır.

```csharp
// Fetch the destination blob's properties before checking the copy state.
await destBlob.FetchAttributesAsync();

// Check the copy status. If it is still pending, abort the copy operation.
if (destBlob.CopyState.Status == CopyStatus.Pending)
{
    await destBlob.AbortCopyAsync(copyId);
    Console.WriteLine("Copy operation {0} has been aborted.", copyId);
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki konular, Azure REST API'lerini kullanarak blob'ları kopyalama ve devam eden kopyalama işlemlerini iptal etme hakkında bilgiler içerir.

- [İkili Büyük Nesneyi Kopyalama](/rest/api/storageservices/copy-blob)
- [Abort Kopya Blob](/rest/api/storageservices/abort-copy-blob)
