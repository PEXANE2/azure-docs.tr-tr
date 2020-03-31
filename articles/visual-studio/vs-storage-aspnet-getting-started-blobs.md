---
title: Visual Studio 'u (ASP.NET) kullanarak Azure Blob depolama alanına başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki ASP.NET bir projede Azure Blob depolama sını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d6591cdb8f19484c87bb05f9007521adc34778f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298875"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Azure Blob depolama ve Visual Studio'ya bağlı hizmetlerle (ASP.NET) başlayın

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure Blob depolama, bulutta yapılandırılmamış verileri nesne veya blob olarak depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır. Blob depolama hakkında daha fazla bilgi edinmek için Azure [Blob depolamasına Giriş 'e](../storage/blobs/storage-blobs-introduction.md)bakın.

Bu öğretici, Blob depolama kullanan bazı yaygın senaryolar için ASP.NET kod nasıl yazılalışyapılacağını gösterir. Senaryolar arasında bir blob kapsayıcısı oluşturma ve blob'ları yükleme, listeleme, indirme ve silme yer alıyor.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

## <a name="create-an-mvc-controller"></a>MVC denetleyicisi oluşturma 

1. **Çözüm Gezgini'nde,** **Denetleyicileri**sağ tıklatın.

2. Bağlam menüsünden**Denetleyici** **Ekle'yi** > seçin.

    ![Ekle ve Denetleyici vurgulanmış Solution Explorer ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. İskele **Ekle** iletişim kutusunda **MVC 5 Controller 'ı**seçin - Boş ve **Ekle'yi**seçin.

    ![İskele Ekle iletişim kutusu ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. Denetleyici **Ekle** iletişim kutusunda, denetleyiciblobsController adını ve **Ekle'yi**seçin. *BlobsController*

    ![Denetleyici Ekle iletişim kutusunun ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. `BlobsController.cs` Dosyaya aşağıdaki `using` yönergeleri ekleyin:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Depolama hesabına bağlanın ve konteyner başvurusu alın

Blob kapsayıcısı, blob'lar ve klasörlerden oluşan iç içe bir hiyerarşidir. Bu belgedeki adımların geri kalanı bir blob kapsayıcısına başvuru gerektirir, böylece kod yeniden kullanılabilirlik için kendi yöntemine yerleştirilmelidir.

Aşağıdaki **adımlar, Web.config'deki**bağlantı dizesini kullanarak depolama hesabına bağlanmak için bir yöntem oluşturur. Adımlar da bir kapsayıcı için bir başvuru oluşturmak.  **Web.config'deki** bağlantı dizesi ayarı biçimiyle `<storageaccountname>_AzureStorageConnectionString`adlandırılır. 

1. `BlobsController.cs` dosyasını açın.

1. **CloudBlobContainer'ı**döndüren **GetCloudBlobContainer** adlı bir yöntem ekleyin.  `<storageaccountname>_AzureStorageConnectionString` **Web.config'deki**anahtarın gerçek adı ile değiştirdiğinden emin olun.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
                CloudConfigurationManager.GetSetting("<storageaccountname>_AzureStorageConnectionString"));
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> *Test-blob-kapsayıcı* henüz var olmasa da, bu kod bir başvuru oluşturur. Bu, kapsayıcının bir sonraki `CreateIfNotExists` adımda gösterilen yöntemle oluşturulabilmesi içindir.

## <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma

Aşağıdaki adımlar, bir blob kapsayıcınasıl oluşturulacak gösteriş:

1. Bir ' `CreateBlobContainer` döndürür `ActionResult`adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İstenilen blob kapsayıcı adına başvuruyu temsil eden bir `CloudBlobContainer` nesne alın. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Henüz `CloudBlobContainer.CreateIfNotExists` yoksa kapsayıcıyı oluşturmak için yöntemi çağırın. Kapsayıcı `CloudBlobContainer.CreateIfNotExists` yoksa ve başarıyla oluşturulursa yöntem **doğru** döndürür. Aksi takdirde, yöntem **yanlış**döndürür.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Blob kapsayıcısının adı ile güncelleştirin. `ViewBag`

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    Tamamlanan yöntem aşağıda `CreateBlobContainer` veda edilebilmektedir:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExists();
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. **Çözüm Gezgini'nde,** **Görünümler** klasörüne sağ tıklayın.

1. **Blobs** klasörü yoksa, bir klasör oluşturun. Bağlam menüsünden**Yeni Klasör** **Ekle'yi** > seçin. Yeni klasör *Blobs*adı. 
 
1. **Çözüm Gezgini'nde** **Görünümler** klasörünü genişletin ve **Blobs'ı**sağ tıklatın.

1. Bağlam menüsünden**Görünüm** **Ekle'yi** > seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **CreateBlobContainer'ı** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `CreateBlobContainer.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. **Solution Explorer'da,** **Paylaşılan Görünümler** > **Shared** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **Blob Kapsayıcısı Oluştur'u** seçin:
  
    ![Blob kapsayıcısı oluştur ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Daha önce de belirtildiği `CloudBlobContainer.CreateIfNotExists` gibi, yöntem yalnızca kapsayıcı yok ve oluşturulduğunda **doğru** döndürür. Bu nedenle, kapsayıcı olduğunda uygulama çalıştırılırsa, yöntem **yanlış**döndürür.

## <a name="upload-a-blob-into-a-blob-container"></a>Blob kabına bir damla yükleme

[Blob kapsayıcısı oluşturulduğunda,](#create-a-blob-container)dosyaları bu kapsayıcıya yükleyin. Bu bölüm, yerel bir dosyayı blob konteynerine yüklemeden geçer. Adımlar *test-blob-konteyner*adlı bir blob konteyner olduğunu varsayalım. 

1. `BlobsController.cs` dosyasını açın.

1. Dize döndüren bir yöntem ekleyin. `UploadBlob`

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. `UploadBlob` Yöntem içinde, istenen `CloudBlobContainer` blob kapsayıcı adına bir başvuru temsil eden bir nesne almak. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Azure depolama farklı blob türlerini destekler. Bu öğretici blok lekeleri kullanır. Blok blob'una başvuru almak `CloudBlobContainer.GetBlockBlobReference` için yöntemi arayın.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Blob adı, bir blob almak için kullanılan URL'nin bir parçasıdır ve dosyanın adı da dahil olmak üzere herhangi bir dize olabilir.

1. Bir blob başvurusu olduktan sonra, blob başvuru nesnesinin `UploadFromStream` yöntemini çağırarak herhangi bir veri akışını yükleyebilirsiniz. Yöntem, `UploadFromStream` yoksa blob oluşturur veya varsa üzerine yazar. * &lt;(Dosyadan yüklemeye>,* yüklenecek bir dosyaya tam nitelikli bir yol olarak değiştirin.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStream(fileStream);
    }
    ```
    
    Aşağıdaki tamamlanan `UploadBlob` yöntemi gösterir (dosyanın yüklenmesi için tam nitelikli bir yol ile):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStream(fileStream);
        }
        return "success!";
    }
    ```

1. **Solution Explorer'da,** **Paylaşılan Görünümler** > **Shared** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırın ve **Upload blob'u**seçin.  Başarı *kelimesi!* görünmelidir.
    
    ![Başarı doğrulama ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
## <a name="list-the-blobs-in-a-blob-container"></a>Lekeleri blob kabında listele

Bu bölümde, bir blob kapsayıcısında lekelernasıl listelenebildiğini gösteriş. Örnek kod, bölümde oluşturulan *test-blob-konteyner* başvurur, [bir blob kapsayıcı oluşturun.](#create-a-blob-container)

1. `BlobsController.cs` dosyasını açın.

1. Bir ' `ListBlobs` döndürür `ActionResult`adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. `ListBlobs` Yöntem içinde, blob kapsayıcısına başvuruyu temsil eden bir `CloudBlobContainer` nesne alın. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Lekeleri blob kabında listelemek için `CloudBlobContainer.ListBlobs` yöntemi kullanın. Yöntem, `CloudBlobContainer.ListBlobs` bir `IListBlobItem` , `CloudPageBlob`veya `CloudBlobDirectory` nesneye `CloudBlockBlob`döküm olabilir bir nesne döndürür. Aşağıdaki kod snippet bir blob kaptaki tüm lekeleri oyuvarlar. Her blob, türüne göre uygun nesneye atılır. Adı (veya **Bir CloudBlobDirectory**durumunda URI) listeye eklenir.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs())
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Blobs ek olarak, blob kaplar dizinleri içerebilir. *Test-blob-konteyner*olarak adlandırılan bir blob kapsayıcı olduğunu varsayalım , aşağıdaki hiyerarşi ile:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Önceki kod örneğini kullanarak, **blobs** string listesi aşağıdakilere benzer değerler içerir:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Gösterildiği gibi, liste sadece üst düzey varlıklar değil, iç içe olanlar *(bar.png* ve *baz.png)* içerir. Blob kapsayıcıiçindeki tüm varlıkları listelemek için, **CloudBlobContainer.ListBlobs** **yönteminin useFlatBlobListing** parametresi için **doğru** geçmesi için kodu değiştirin.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    **UseFlatBlobListing** parametresini **true** returns olarak ayarlama blob kapsayıcısındaki tüm varlıkların düz bir listesini verir. Bu, aşağıdaki sonuçları verir:

        foo.png
        dir1/bar.png
        dir2/baz.png
    
    Aşağıdaki tamamlanmış **ListBlobs** yöntemini gösterir:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing: true))
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. **Çözüm Gezgini'nde** **Görünümler** klasörünü genişletin ve **Blobs'ı**sağ tıklatın.

2. Bağlam menüsünden**Görünüm** **Ekle'yi** > seçin.

1. Görünüm **Ekle** iletişim kutusunda, `ListBlobs` görünüm adı için girin ve **Ekle'yi**seçin.

1. İçeriği `ListBlobs.cshtml`aşağıdaki kodla açın ve değiştirin:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. **Solution Explorer'da,** **Paylaşılan Görünümler** > **Shared** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **Liste bloblarını** seçin:
  
    ![Liste blobs ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blob’ları indirme

Bu bölümde bir blob nasıl indirilir gösteriş. Yerel depolama da devam edebilir veya bir dize halinde içeriğini okuyabilirsiniz. Örnek kod, bölümde oluşturulan *test-blob-konteyner* başvurur, [bir blob kapsayıcı oluşturun.](#create-a-blob-container)

1. `BlobsController.cs` dosyasını açın.

1. Dize döndüren bir yöntem ekleyin. `DownloadBlob`

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. `DownloadBlob` Yöntem içinde, blob kapsayıcısına başvuruyu temsil eden bir `CloudBlobContainer` nesne alın.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Yöntemi arayarak bir blob `CloudBlobContainer.GetBlockBlobReference` başvuru nesnesi alın. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Bir blob indirmek için `CloudBlockBlob.DownloadToStream` yöntemi kullanın. Aşağıdaki kod, bir blob'un içeriğini bir akış nesnesine aktarır. Bu nesne daha sonra yerel bir dosyaya kalıcı olarak verilir. (Yerel * &lt;dosya adı>* blob'un indirilecek yeri gösteren tam nitelikli dosya adı ile değiştirin.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```
    
    Aşağıdaki tamamlanan `DownloadBlob` yöntemi gösterir (oluşturulan yerel dosya için tam nitelikli bir yol ile):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStream(fileStream);
        }
        return "success!";
    }
    ```

1. **Solution Explorer'da,** **Paylaşılan Görünümler** > **Shared** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırın ve blob'u indirmek için **İndir blob'u** seçin. `CloudBlobContainer.GetBlockBlobReference` Yöntemde belirtilen blob, `File.OpenWrite` yöntem çağrısında belirtilen konuma indirme leri çağırır.  Metin *başarı!* tarayıcıda görünmelidir. 

## <a name="delete-blobs"></a>Blob’ları silme

Aşağıdaki adımlar, bir lekenin nasıl silinir olduğunu gösterir:

1. `BlobsController.cs` dosyasını açın.

1. Dize döndüren bir yöntem ekleyin. `DeleteBlob`

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. `DeleteBlob` Yöntem içinde, blob kapsayıcısına başvuruyu temsil eden bir `CloudBlobContainer` nesne alın.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Yöntemi arayarak bir blob `CloudBlobContainer.GetBlockBlobReference` başvuru nesnesi alın. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Bir blob silmek `Delete` için yöntemi kullanın.

    ```csharp
    blob.Delete();
    ```
    
    Tamamlanan `DeleteBlob` yöntem aşağıdaki gibi görünmelidir:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.Delete();
        return "success!";
    }
    ```

1. **Solution Explorer'da,** **Paylaşılan Görünümler** > **Shared** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırın ve `CloudBlobContainer.GetBlockBlobReference` arama yönteminde belirtilen blob'u silmek için **blob'u** sil'i seçin. Metin *başarı!* tarayıcıda görünmelidir. Tarayıcının Geri **Düğmesini** seçin ve ardından blob'un artık kapsayıcıda olmadığını doğrulamak için **Liste bloblarını** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, ASP.NET kullanarak Azure Depolama'da lekeleri nasıl depolayabileceğinizi, listelemeyi ve alınabildiğini öğrendiniz. Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure Tablo depolama ve Visual Studio'ya bağlı hizmetlerle (ASP.NET) başlayın](vs-storage-aspnet-getting-started-tables.md)
  * [Azure Kuyruk depolama ve Visual Studio'ya bağlı hizmetlerle başlayın (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
