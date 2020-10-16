---
title: Değiştirilen ve silinen blob 'lar
titleSuffix: Azure Cognitive Search
description: Azure Blob depolamadan içeri aktaran bir ilk arama dizini oluşturulduktan sonra, sonraki dizin oluşturma yalnızca değiştirilen veya silinen Blobları alabilir. Bu makalede Ayrıntılar açıklanmaktadır.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 2e73039418233c97fc20242ed7af7df14c5b47ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91534786"
---
# <a name="how-to-set-up-change-and-deletion-detection-for-blobs-in-azure-cognitive-search-indexing"></a>Azure Bilişsel Arama Dizin oluşturma 'da blob 'lar için değişiklik ve silme algılamasını ayarlama

İlk arama dizini oluşturulduktan sonra, sonraki Dizin Oluşturucu işlerini, ilk çalıştırmasından bu yana yalnızca oluşturulan veya silinen belgeleri seçmek üzere yapılandırmak isteyebilirsiniz. Azure Blob depolamadan kaynaklanan arama içerikleri için, dizin oluşturmayı tetiklemek üzere bir zamanlama kullandığınızda değişiklik algılama otomatik olarak gerçekleşir. Varsayılan olarak, hizmet blob 'un zaman damgasıyla belirlendiği şekilde yalnızca değiştirilen Blobları yeniden dizinler `LastModified` . Arama Dizin oluşturucular tarafından desteklenen diğer veri kaynaklarının aksine, Bloblar her zaman bir zaman damgasına sahiptir ve bu da bir değişiklik algılama ilkesini el ile ayarlama ihtiyacını ortadan kaldırır.

Değişiklik algılama belirli bir olsa da, silme algılaması değildir. Silinen belgeleri algılamak isterseniz, "geçici silme" yaklaşımını kullandığınızdan emin olun. Blob 'ları sağ silme, ilgili belgeler arama dizininden kaldırılmaz.

Geçici silme yaklaşımını uygulamak için iki yol vardır. Her ikisi de aşağıda açıklanmıştır.

## <a name="native-blob-soft-delete-preview"></a>Yerel blob geçici silme (Önizleme)

> [!IMPORTANT]
> Yerel blob geçici silme desteği önizlemededir. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2020-06-30-önizleme](./search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

> [!NOTE]
> Yerel blob geçici silme ilkesini kullanırken, dizininizdeki belgelerin belge anahtarlarının bir blob özelliği veya blob meta verileri olması gerekir.

Bu yöntemde, Azure Blob depolama tarafından sunulan [Yerel blob geçici silme](../storage/blobs/soft-delete-blob-overview.md) özelliğini kullanacaksınız. Depolama hesabınızda yerel blob geçici silme etkinse, veri kaynağınız yerel bir geçici silme ilkesi kümesine sahiptir ve Dizin Oluşturucu, geçici olarak silinen bir duruma geçiş yapılmış bir blob buluyor, Dizin Oluşturucu bu belgeyi dizinden kaldırır. Blob 'ları Azure Data Lake Storage 2. dizinlerken yerel blob geçici silme ilkesi desteklenmez.

Aşağıdaki adımları kullanın:

1. [Azure Blob depolaması için yerel geçici silme](../storage/blobs/soft-delete-blob-overview.md)özelliğini etkinleştirin. Bekletme ilkesini, Dizin Oluşturucu aralığı zamanlamadan çok daha yüksek bir değere ayarlamanız önerilir. Bu şekilde, Dizin oluşturucuyu çalıştıran bir sorun varsa veya dizinlemek için çok sayıda belgeniz varsa, dizin oluşturucunun, geçici olarak silinen Blobları işlemesi çok fazla zaman vardır. Azure Bilişsel Arama Dizin oluşturucular, bir belgeyi, geçici olarak silinen bir durumdayken blobu işliyorsa siler.

1. Veri kaynağında yerel bir blob geçici silme algılama ilkesi yapılandırın. Aşağıda bir örnek gösterilmiştir. Bu özellik önizlemede olduğundan, önizleme REST API kullanmanız gerekir.

1. Dizin oluşturucuyu çalıştırın veya dizin Oluşturucuyu bir zamanlamaya göre çalışacak şekilde ayarlayın. Dizin Oluşturucu çalıştırıldığında ve blobu işlediğinde belge dizinden kaldırılır.

    ```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

### <a name="reindexing-un-deleted-blobs-using-native-soft-delete-policies"></a>Silinen Blobların yeniden oluşturulması (yerel geçici silme ilkelerini kullanarak)

Depolama hesabınızda yerel geçici silme özelliği etkin olan bir blob 'u Azure Blob depolamadan silerseniz, blob geçici olarak silinen bir duruma geçer ve bu Blobun bekletme döneminde silmeyi kaldırma seçeneği sunar. Dizin oluşturucunun onu işledikten sonra bir silmeyi tersine çevirmek için Dizin Oluşturucu, her zaman geri yüklenen Blobun dizinini oluşturmayacak. Bunun nedeni, dizin oluşturucunun blob zaman damgasına göre hangi Blobların dizine eklenebileceğini belirler `LastModified` . Geçici olarak silinen bir blob `LastModified` silinmemişse, zaman damgası güncellenmez. bu nedenle, Dizin Oluşturucu daha son zaman damgalarıyla blob 'ları zaten işdiyse `LastModified` , silinmemiş blobu yeniden oluşturmaz. 

Silinen bir Blobun yeniden dizinlendiğinden emin olmak için Blobun `LastModified` zaman damgasını güncelleştirmeniz gerekir. Bunu yapmanın bir yolu, söz konusu Blobun meta verilerinin yeniden kaydedilerek yapılır. Meta verileri değiştirmeniz gerekmez, ancak meta verilerin yeniden kaydedilmesi Blobun `LastModified` zaman damgasını güncelleştirecek, böylece dizin oluşturucunun bu blobu yeniden eklemesi gerektiğini bilmesini sağlar.

## <a name="soft-delete-using-custom-metadata"></a>Özel meta verileri kullanarak geçici silme

Bu yöntemde, bir belgenin arama dizininden ne zaman kaldırılması gerektiğini göstermek için bir Blobun meta verilerini kullanacaksınız. Bu yöntem iki ayrı eylem gerektirir, arama belgesini dizinden silebilir ve ardından Azure Storage 'da blob silme işlemi gelir.

Aşağıdaki adımları kullanın:

1. Bilişsel Arama Azure 'da mantıksal olarak silindiğini göstermek için blob 'a özel bir meta veri anahtar-değer çifti ekleyin.

1. Veri kaynağında geçici silme sütunu algılama ilkesi yapılandırın. Aşağıda bir örnek gösterilmiştir.

1. Dizin Oluşturucu blobu işledikten ve belgeyi dizinden sildikten sonra, blob 'u Azure Blob depolama alanında silebilirsiniz.

Örneğin, aşağıdaki ilke, bir blob 'un şu değere sahip bir meta veri özelliği varsa silineceğini kabul eder `IsDeleted` `true` :

```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }
```

### <a name="reindexing-un-deleted-blobs-using-custom-metadata"></a>Silinen Blobların yeniden oluşturulması (özel meta veriler kullanılarak)

Bir Dizin Oluşturucu silinen bir blobu işleyerek ve karşılık gelen arama belgesini dizinden kaldırdığında, daha sonra Blobun `LastModified` zaman damgası son dizin oluşturucudan daha eskiyse, bu blobu geri görmez.

Bu belgeyi yeniden eklemek istiyorsanız, `"softDeleteMarkerValue" : "false"` o blob için öğesini değiştirin ve Dizin oluşturucuyu yeniden çalıştırın.

## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama daha iyi hale getirmemize yardımcı olun

Geliştirmeler için özellik istekleriniz veya fikirler varsa, bu girişi [UserVoice](https://feedback.azure.com/forums/263029-azure-search/)üzerinde belirtin. Mevcut özelliği kullanarak yardıma ihtiyacınız varsa sorunuzu [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870)gönderin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Bilişsel Arama'daki Dizin Oluşturucular](search-indexer-overview.md)
* [Blob Dizin oluşturucuyu yapılandırma](search-howto-indexing-azure-blob-storage.md)
* [Blob dizinlemeye genel bakış](search-blob-storage-integration.md)