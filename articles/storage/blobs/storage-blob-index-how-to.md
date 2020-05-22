---
title: Azure Blob depolamada verileri yönetmek ve bulmak için blob dizinini kullanın
description: Blob nesnelerini bulma, yönetme ve sorgulama için blob Dizin etiketlerinin nasıl kullanılacağına ilişkin örneklere bakın.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2020
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: cc82b6578b06323d8cf9a09644d50043dba8e554
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83774326"
---
# <a name="utilize-blob-index-tags-preview-to-manage-and-find-data-on-azure-blob-storage"></a>Azure Blob depolama 'daki verileri yönetmek ve bulmak için blob Dizin etiketlerini (Önizleme) kullanın

Blob dizin etiketleri, anahtar-değer etiketi özniteliklerinin kullanıldığı Depolama hesabınızdaki verileri kategorilere ayırın. Bu Etiketler otomatik olarak dizinlenir ve verileri kolayca bulmak için sorgulanabilir çok boyutlu bir dizin olarak gösterilir. Bu makalede blob Dizin etiketlerini kullanarak verileri ayarlama, alma ve bulma işlemlerinin nasıl yapılacağı gösterilir.

Blob dizini hakkında daha fazla bilgi edinmek için bkz. [blob dizini (Önizleme) Ile Azure Blob depolama üzerinde verileri yönetme ve bulma](storage-manage-find-blobs.md).

> [!NOTE]
> Blob dizini genel önizlemededir ve **Fransa orta** ve **Fransa Güney** bölgelerinde kullanılabilir. Bu özellik hakkında bilinen sorunlar ve sınırlamalar hakkında daha fazla bilgi edinmek için bkz. [blob dizini (Önizleme) Ile Azure Blob depolama üzerinde verileri yönetme ve bulma](storage-manage-find-blobs.md).

## <a name="prerequisites"></a>Ön koşullar
# <a name="portal"></a>[Portal](#tab/azure-portal)
- Abonelik, blob dizini önizlemesine erişim için kaydedildi ve onaylandı
- [Azure Portal](https://portal.azure.com/) erişim

# <a name="net"></a>[.NET](#tab/net)
Blob dizini genel önizlemede olduğundan, .NET depolama paketi önizleme NuGet akışında serbest bırakılır. Bu kitaplık, şu anda ve resmi olduğunda değişir. 

1. Visual Studio 'da, `https://azuresdkartifacts.blob.core.windows.net/azure-sdk-for-net/index.json` NuGet paket kaynaklarınıza URL 'yi ekleyin. 

   Nasıl yapılacağını öğrenmek için bkz. [paket kaynakları](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources).

2. NuGet Paket Yöneticisi ' nde, **Azure. Storage. blob** paketini bulun ve **12.5.0-dev. 20200422.2** sürümünü projenize yüklemek için. Komutunu da çalıştırabilirsiniz```Install-Package Azure.Storage.Blobs -Version 12.5.0-dev.20200422.2```

   Nasıl yapılacağını öğrenmek için bkz. [paket bulma ve yüklemeyi oluşturma](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#find-and-install-a-package).

3. Aşağıdaki using deyimlerini, kod dosyanızın en üstüne ekleyin.
```csharp
using Azure;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using Azure.Storage.Blobs.Specialized;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
```
---

## <a name="upload-a-new-blob-with-index-tags"></a>Dizin etiketlerine sahip yeni bir blobu karşıya yükleme
# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com/)depolama hesabınızı seçin 

2. **BLOB hizmeti**altındaki **kapsayıcılar** seçeneğine gidin, kapsayıcınızı seçin

3. Karşıya yükleme dikey penceresini açmak **için karşıya yükle düğmesini seçin** ve Blok Blobu olarak karşıya yüklenecek dosyayı bulmak için yerel dosya sisteminize gidin.

4. **Gelişmiş** açılan listeyi genişletin ve **blob dizini etiketleri** bölümüne gidin

5. Verilerinize uygulanmasını istediğiniz anahtar/değer blobu Dizin etiketlerini girin

6. Blobu karşıya yüklemek için **karşıya yükle** düğmesini seçin

![Blob dizini etiketleriyle veri yükleme](media/storage-blob-index-concepts/blob-index-upload-data-with-tags.png)

# <a name="net"></a>[.NET](#tab/net)
Aşağıdaki örnek, oluşturma sırasında etiketleri ayarlanmış bir ekleme blobu oluşturmayı gösterir.
```csharp
static async Task BlobIndexTagsOnCreate()
   {
      BlobServiceClient serviceClient = new BlobServiceClient(ConnectionString);
      BlobContainerClient container = serviceClient.GetBlobContainerClient("mycontainer");

      try
      {
          // Create a container
          await container.CreateIfNotExistsAsync();

          // Create an append blob
          AppendBlobClient appendBlobWithTags = container.GetAppendBlobClient("myAppendBlob0.logs");
         
          // Blob Index tags to upload
          CreateAppendBlobOptions appendOptions = new CreateAppendBlobOptions();
          appendOptions.Tags = new Dictionary<string, string>
          {
              { "Sealed", "false" },
              { "Content", "logs" },
              { "Date", "2020-04-20" }
          };

          // Upload data with tags set on creation
          await appendBlobWithTags.CreateAsync(appendOptions);
      }
      finally
      {
      }
   }
```

---

## <a name="get-set-and-update-blob-index-tags"></a>Blob dizini etiketlerini al, ayarla ve Güncelleştir
# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com/)depolama hesabınızı seçin 

2. **BLOB hizmeti**altındaki **kapsayıcılar** seçeneğine gidin, kapsayıcınızı seçin

3. Seçili kapsayıcı içindeki blob 'ların listesinden istediğiniz blobu seçin

4. Blob Genel Bakış sekmesi, blob **Dizin etiketleri** dahil olmak üzere Blobun özelliklerini görüntüler

5. Blobun anahtar/değer dizini etiketlerini alabilir, ayarlayabilir, değiştirebilir veya silebilirsiniz

6. Bloba ilgili tüm güncelleştirmeleri onaylamak için **Kaydet** düğmesini seçin

![Nesnelerde blob Dizin etiketlerini al, ayarla, Güncelleştir ve Sil](media/storage-blob-index-concepts/blob-index-get-set-tags.png)

# <a name="net"></a>[.NET](#tab/net)
```csharp
static async Task BlobIndexTagsExample()
   {
      BlobServiceClient serviceClient = new BlobServiceClient(ConnectionString);
      BlobContainerClient container = serviceClient.GetBlobContainerClient("mycontainer");

      try
      {
          // Create a container
          await container.CreateIfNotExistsAsync();

          // Create a new append blob
          AppendBlobClient appendBlob = container.GetAppendBlobClient("myAppendBlob1.logs");
          await appendBlob.CreateAsync();

          // Set or Update Blob Index tags on existing blob
          Dictionary<string, string> tags = new Dictionary<string, string>
          {
              { "Project", "Contoso" },
              { "Status", "Unprocessed" },
              { "Sealed", "true" }
          };
          await appendBlob.SetTagsAsync(tags);

          // Get Blob Index tags
          Response<IDictionary<string, string>> tagsResponse = await appendBlob.GetTagsAsync();
          Console.WriteLine(appendBlob.Name);
          foreach (KeyValuePair<string, string> tag in tagsResponse.Value)
          {
              Console.WriteLine($"{tag.Key}={tag.Value}");
          }

          // List Blobs with all options returned including Blob Index tags
          await foreach (BlobItem blobItem in container.GetBlobsAsync(BlobTraits.All))
          {
              Console.WriteLine(Environment.NewLine + blobItem.Name);
              foreach (KeyValuePair<string, string> tag in blobItem.Tags)
              {
                  Console.WriteLine($"{tag.Key}={tag.Value}");
              }
          }

          // Delete existing Blob Index tags by replacing all tags
          Dictionary<string, string> noTags = new Dictionary<string, string>();
          await appendBlob.SetTagsAsync(noTags);

      }
      finally
      {
      }
   }
```

---

## <a name="filter-and-find-data-with-blob-index-tags"></a>Blob dizini etiketleriyle verileri filtreleme ve bulma

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal içinde, blob dizini etiketleri filtresi, `@container` parametreyi seçtiğiniz kapsayıcının kapsamına otomatik olarak uygular. Tüm depolama hesabınızda etiketlendirilmiş verileri filtrelemek ve bulmak istiyorsanız lütfen REST API, SDK 'larımızı veya araçlarınızı kullanın.

1. [Azure Portal](https://portal.azure.com/)depolama hesabınızı seçin. 

2. **BLOB hizmeti**altındaki **kapsayıcılar** seçeneğine gidin, kapsayıcınızı seçin

3. Seçili kapsayıcı içinde filtrelemek için **BLOB dizin etiketleri filtresi** düğmesini seçin

4. Blob dizini etiketi anahtarı ve etiket değeri girin

5. Ek etiket filtreleri eklemek için **blob dizini Etiketleri filtre** düğmesini seçin (10 ' a kadar)

![Blob Dizin etiketlerini kullanarak etiketli nesneleri filtreleme ve bulma](media/storage-blob-index-concepts/blob-index-tag-filter-within-container.png)

# <a name="net"></a>[.NET](#tab/net)
```csharp
static async Task FindBlobsByTagsExample()
   {
      BlobServiceClient serviceClient = new BlobServiceClient(ConnectionString);
      BlobContainerClient container1 = serviceClient.GetBlobContainerClient("mycontainer");
      BlobContainerClient container2 = serviceClient.GetBlobContainerClient("mycontainer2");

      // Blob Index queries and selection
      String singleEqualityQuery = @"""Archive"" = 'false'";
      String andQuery = @"""Archive"" = 'false' AND ""Priority"" = '01'";
      String rangeQuery = @"""Date"" >= '2020-04-20' AND ""Date"" <= '2020-04-30'";
      String containerScopedQuery = @"@container = 'mycontainer' AND ""Archive"" = 'false'";

      String queryToUse = containerScopedQuery;

      try
      {
          // Create a container
          await container1.CreateIfNotExistsAsync();
          await container2.CreateIfNotExistsAsync();

          // Create append blobs
          AppendBlobClient appendBlobWithTags0 = container1.GetAppendBlobClient("myAppendBlob00.logs");
          AppendBlobClient appendBlobWithTags1 = container1.GetAppendBlobClient("myAppendBlob01.logs");
          AppendBlobClient appendBlobWithTags2 = container1.GetAppendBlobClient("myAppendBlob02.logs");
          AppendBlobClient appendBlobWithTags3 = container2.GetAppendBlobClient("myAppendBlob03.logs");
          AppendBlobClient appendBlobWithTags4 = container2.GetAppendBlobClient("myAppendBlob04.logs");
          AppendBlobClient appendBlobWithTags5 = container2.GetAppendBlobClient("myAppendBlob05.logs");
           
          // Blob Index tags to upload
          CreateAppendBlobOptions appendOptions = new CreateAppendBlobOptions();
          appendOptions.Tags = new Dictionary<string, string>
          {
              { "Archive", "false" },
              { "Priority", "01" },
              { "Date", "2020-04-20" }
          };
          
          CreateAppendBlobOptions appendOptions2 = new CreateAppendBlobOptions();
          appendOptions2.Tags = new Dictionary<string, string>
          {
              { "Archive", "true" },
              { "Priority", "02" },
              { "Date", "2020-04-24" }
          };

          // Upload data with tags set on creation
          await appendBlobWithTags0.CreateAsync(appendOptions);
          await appendBlobWithTags1.CreateAsync(appendOptions);
          await appendBlobWithTags2.CreateAsync(appendOptions2);
          await appendBlobWithTags3.CreateAsync(appendOptions);
          await appendBlobWithTags4.CreateAsync(appendOptions2);
          await appendBlobWithTags5.CreateAsync(appendOptions2);

          // Find Blobs given a tags query
          Console.WriteLine("Find Blob by Tags query: " + queryToUse + Environment.NewLine);

          List<FilterBlobItem> blobs = new List<FilterBlobItem>();
          await foreach (FilterBlobItem filterBlobItem in serviceClient.FindBlobsByTagsAsync(queryToUse))
          {
              blobs.Add(filterBlobItem);
          }

          foreach (var filteredBlob in blobs)
          {
              Console.WriteLine($"BlobIndex result: ContainerName= {filteredBlob.ContainerName}, " +
                  $"BlobName= {filteredBlob.Name}");
          }

      }
      finally
      {
      }
   }
```

---

## <a name="lifecycle-management-with-blob-index-tag-filters"></a>Blob dizini etiket filtreleriyle yaşam döngüsü yönetimi

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com/)depolama hesabınızı seçin. 

2. **BLOB hizmeti** altındaki **yaşam döngüsü yönetimi** seçeneğine gidin

3. *Kural Ekle* ' yi seçin ve ardından eylem kümesi form alanlarını doldurun

4. Önek eşleşmesi için isteğe bağlı Filtre Ekle ve blob dizini eşleşmesi için filtre kümesi seçin ![ yaşam döngüsü yönetimi için blob dizini etiketi filtreleri ekleyin](media/storage-blob-index-concepts/blob-index-match-lifecycle-filter-set.png)

5. Blob dizini Etiketleri filtre örneği ile kural ayarları yaşam döngüsü yönetim kuralını gözden geçirmek için **gözden geçir + Ekle** ' yi seçin ![](media/storage-blob-index-concepts/blob-index-lifecycle-management-example.png)

6. Yeni kuralı yaşam döngüsü yönetimi ilkesine uygulamak için **Ekle** ' yi seçin

# <a name="net"></a>[.NET](#tab/net)
[Yaşam döngüsü yönetimi](storage-lifecycle-management-concepts.md) ilkeleri, her depolama hesabı için denetim düzlemi düzeyinde uygulanır. .NET için, bir yaşam döngüsü yönetim kuralı içindeki blob dizini eşleşme filtresinden faydalanmak için [Microsoft Azure Management Storage Library sürüm 16.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/) veya üstünü yüklemeyi gerçekleştirin.

---

## <a name="next-steps"></a>Sonraki adımlar

Blob dizini hakkında daha fazla bilgi edinin. Bkz. [BLOB diziniyle Azure Blob depolama 'da verileri yönetme ve bulma (Önizleme)](storage-manage-find-blobs.md )

Yaşam döngüsü yönetimi hakkında daha fazla bilgi edinin. Bkz [. Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)
