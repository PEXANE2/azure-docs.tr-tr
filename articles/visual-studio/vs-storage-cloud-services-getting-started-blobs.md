---
title: Visual Studio (bulut hizmetleri) kullanarak blob depolama ile başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki bir bulut hizmeti projesinde Azure Blob depolamasını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d151e55f627166d8ad7d8affa53740e86cd1e501
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298799"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure Blob Depolama ve Visual Studio’ya bağlı hizmetleri kullanmaya başlama (bulut hizmeti projeleri)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Visual Studio Bulut Hizmetleri projesinde Visual Studio **Add Connected Services** iletişim kutusunu kullanarak bir Azure Depolama hesabı oluşturduktan veya başvurulan sonra Azure Blob Depolama'ya nasıl başlanırsınız. Blob kapsayıcılarına nasıl erişeceğiniz ve oluşturulacağını ve blob yükleme, listeleme ve indirme gibi yaygın görevleri nasıl gerçekleştireceğinizi göstereceğiz. Örnekler C\# ile yazılır ve [.NET için Microsoft Azure Depolama İstemci Kitaplığı'nı](https://msdn.microsoft.com/library/azure/dn261237.aspx)kullanın.

Azure Blob Depolama, dünyanın her yerinden http veya HTTPS üzerinden erişilebilen büyük miktarlarda yapılandırılmamış veri depolamak için bir hizmettir. Tek bir leke herhangi bir boyutta olabilir. Blobs görüntüler, ses ve video dosyaları, ham veri ve belge dosyaları gibi şeyler olabilir.

Dosyalar klasörlerde yaşadığı gibi, depolama lekeleri de kapsayıcılarda yaşar. Bir depolama alanı oluşturduktan sonra, depolama alanında bir veya daha fazla kapsayıcı oluşturursunuz. Örneğin, "Not Defteri" adlı bir depolama alanında, resimleri depolamak için "görüntüler" adı verilen eksitler ve ses dosyalarını depolamak için "ses" adı verilen başka bir depo oluşturabilirsiniz. Kapsayıcıları oluşturduktan sonra, tek tek blob dosyaları yükleyebilirsiniz.

* Blob'ları programlamak üzere işleme hakkında daha fazla bilgi için [.NET'i kullanarak Azure Blob depolama alanına başlayın'](../storage/blobs/storage-dotnet-how-to-use-blobs.md)a bakın.
* Azure Depolama hakkında genel bilgi için [Bkz. Depolama belgeleri.](https://azure.microsoft.com/documentation/services/storage/)
* Azure Bulut Hizmetleri hakkında genel bilgi için [Bulut Hizmetleri belgelerine](https://azure.microsoft.com/documentation/services/cloud-services/)bakın.
* Programlama ASP.NET uygulamaları hakkında daha fazla bilgi için [ASP.NET.](https://www.asp.net)

## <a name="access-blob-containers-in-code"></a>Blob kaplarını kodda erişin
Bulut hizmeti projelerindeki blob'lara programlı olarak erişmek için, zaten mevcut değillerse aşağıdaki öğeleri eklemeniz gerekir.

1. Azure Depolama'ya programlı olarak erişmek istediğiniz c# dosyasının üst bölümüne aşağıdaki kod ad alanı bildirimlerini ekleyin.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizenizi ve depolama hesabı bilgilerinizi almak için aşağıdaki kodu kullanın.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. Depolama hesabınızdaki mevcut bir kapsayıcıya başvurmak için bir **CloudBlobClient** nesnesi alın.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. Belirli bir blob kapsayıcısına başvurmak için bir **CloudBlobContainer** nesnesi alın.
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> Önceki yordamda gösterilen kodun tümünü aşağıdaki bölümlerde gösterilen kodun önünde kullanın.
> 
> 

## <a name="create-a-container-in-code"></a>Kodda kapsayıcı oluşturma
> [!NOTE]
> ASP.NET'da Azure Depolama'ya çağrı yapan bazı API'ler eşzamanlıdır. Daha fazla bilgi için [Async ve Await ile Asynchronous programlamaya](https://msdn.microsoft.com/library/hh191443.aspx) bakın. Aşağıdaki örnekteki kod, async programlama yöntemleri kullandığınızı varsayar.
> 
> 

Depolama hesabınızda bir kapsayıcı oluşturmak için tek yapmanız gereken aşağıdaki koddaki gibi **CreateIfNotExistsAsync'e** bir çağrı eklemektir:

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


Kapsayıcıiçindeki dosyaları herkesin kullanımına açmak için, aşağıdaki kodu kullanarak kapsayıcıyı herkese açık olarak ayarlayabilirsiniz.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Internet'teki herkes ortak bir kapsayıcıda lekeler görebilir, ancak yalnızca uygun erişim anahtarına sahipseniz bunları değiştirebilir veya silebilirsiniz.

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Azure Depolama blok lekelerini ve sayfa lekelerini destekler. Çoğu durumda kullanılması önerilen blob türü blok blobudur.

Bir dosyayı bir blok blobuna yüklemek için bir kapsayıcı başvurusu alın ve blok blob başvurusu almak için kullanın. Bir blob başvurusu edindiğinizde **UploadFromStream** yöntemini çağırarak istediğiniz veri akışını yükleyebilirsiniz. Bu işlemle, eğer önceden oluşturulmadıysa bir blob oluşturulacaktır, aksi takdirde üzerine yazılacaktır. Aşağıdaki örnek kapsayıcının önceden oluşturulduğunu varsayarak bir blobun bir kapsayıcıya nasıl yükleneceğini gösterir.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
Blob’ları bir kapsayıcıda listelemek için ilk olarak bir kapsayıcı başvurusu edinin. Ardından içindeki blobları ve/veya dizinleri almak için kapsayıcının **ListBlobs** yöntemini kullanabilirsiniz. İade edilen **bir IListBlobItem**için zengin özellik ve metotlar kümesine erişmek için, bir **CloudBlockBlob,** **CloudPageBlob**veya **CloudBlobDirectory** nesnesine dökmeniz gerekir. Tür bilinmiyorsa, hangisine yayınlayacağınızı belirlemek için bir tür denetimi kullanabilirsiniz. Aşağıdaki kod, **resimler** kapsayıcısındaki her nesnenin URI’sinin nasıl alınacağını ve çıkacağını gösterir:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Önceki kod örneğinde gösterildiği gibi, blob hizmeti kapsayıcılar içinde dizinler kavramı da vardır. Bu, lekelerinizi daha klasör benzeri bir yapıda düzenleyebilmeniz içindir. Örneğin bir kapsayıcıda yer alan ve **resimler** olarak adlandırılan aşağıdaki blok blobları kümesine göz atın:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

**Kapsayıcıda ListBlobs'u** aradiğinizde (önceki örnekte olduğu gibi), döndürülen koleksiyon, en üst düzeyde bulunan dizinleri ve lekeleri temsil eden **CloudBlobDirectory** ve **CloudBlockBlob** nesnelerini içerir. İşte ortaya çıkan çıktı:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


İsteğe bağlı olarak **ListBlobs** yönteminin **UseFlatBlobListing** parametresini **true** olarak ayarlayabilirsiniz. Bu, dizine bakılmaksızın her blob'un **CloudBlockBlob**olarak döndürülmesinde kaynaklanır. İşte **ListBlobs**için çağrı:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

ve burada sonuçlar şunlardır:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Daha fazla bilgi için [CloudBlobContainer.ListBlobs'a](https://msdn.microsoft.com/library/azure/dd135734.aspx)bakın.

## <a name="download-blobs"></a>Blob’ları indirme
Blob’ları indirmek için ilk olarak bir blob başvurusu alın ve ardından **DownloadToStream** yöntemini çağırın. Aşağıdaki örnek, blob içeriklerini bir akış nesnesine aktarmak ve ardından yerel bir dosyaya kalıcı olarak almak için **DownloadToStream** yöntemini kullanır:

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Bunun yanında bir blobun içeriklerini metin dizesi olarak indirmek için **DownloadToStream** yöntemini kullanabilirsiniz.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Blob’ları silme
Bir blob'u silmek için önce bir blob başvurusu alın ve ardından **Sil** yöntemini arayın.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Blob’ları sayfalarda zaman uyumsuz olarak listeleme
Çok sayıda blob listeliyorsanız veya bir listeleme işlemi ile dönen sonuç sayısını denetlemek isterseniz sonuç sayfalarında blobları listeleyebilirsiniz. Bu örnek, geniş bir sonuç kümesinin dönmesini beklerken çalıştırmanın engellenmemesi için sayfalardaki sonuçların zaman uyumsuz olarak nasıl döneceğini gösterir.

Bu örnek düz bir blob listesi gösterir, ancak **ListBlobsSegmentedAsync** metodunun **useFlatBlobListing** parametresini **false** olarak ayarlayarak hiyerarşik bir listeleme gerçekleştirebilirsiniz.

Örnek metot, zaman uyumsuz bir metot çağırdığı için **async** anahtar sözcüğüyle başlamalı ve bir **Görev** nesnesi döndürmelidir. Belirtilen **ListBlobsSegmentedAsync** yöntemi için belirlenen bekleme anahtar kelimesi, listeleme görevi tamamlanana kadar örnek yöntemin çalıştırılmasını askıya alır.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

