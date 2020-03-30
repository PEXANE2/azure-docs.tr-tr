---
title: İşin sonuçlarını indirin - Azure Medya Hizmetleri
description: Bu makalede, bir işin sonuçları nasıl indirilir gösterilebilir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/04/2020
ms.author: juliako
ms.openlocfilehash: bae7104eaded8c2ed153bc141faf7eba0bb86bae
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346312"
---
# <a name="download-the-results-of-a-job"></a>Bir işin sonuçlarını indirin

Azure Medya Hizmetleri'nde, videolarınızı işlerken (örneğin, kodlama veya analiz) [işinizin](transforms-jobs-concept.md)sonucunu depolamak için bir çıktı [varlığı](assets-concept.md) oluşturmanız gerekir. Daha sonra, Medya Hizmeti ve Depolama API'lerini kullanarak bu sonuçları yerel bir klasöre indirebilirsiniz. 

Bu makalede, Java ve .NET SDK'ları kullanarak sonuçların nasıl indirilen nasıl gösterildiği gösterilmiştir.

## <a name="prerequisites"></a>Ön koşullar 

Varlıkları yönetmeyi gözden [geçirin.](manage-asset-concept.md)

## <a name="java"></a>Java

```java
/**
    * Use Media Service and Storage APIs to download the output files to a local folder
    * @param manager               The entry point of Azure Media resource management
    * @param resourceGroup         The name of the resource group within the Azure subscription
    * @param accountName           The Media Services account name
    * @param assetName             The asset name
    * @param outputFolder          The output folder for downloaded files.
    * @throws StorageException
    * @throws URISyntaxException
    * @throws IOException
    */
private static void downloadResults(MediaManager manager, String resourceGroup, String accountName,
        String assetName, File outputFolder) throws StorageException, URISyntaxException, IOException {
    ListContainerSasInput parameters = new ListContainerSasInput()
        .withPermissions(AssetContainerPermission.READ)
        .withExpiryTime(DateTime.now().plusHours(1));
    AssetContainerSas assetContainerSas = manager.assets()
        .listContainerSasAsync(resourceGroup, accountName, assetName, parameters).toBlocking().first();
    
    String strSas = assetContainerSas.assetContainerSasUrls().get(0);
    CloudBlobContainer container = new CloudBlobContainer(new URI(strSas));

    File directory = new File(outputFolder, assetName);
    directory.mkdir();

    ArrayList<ListBlobItem>  blobs = container.listBlobsSegmented(null, true, EnumSet.noneOf(BlobListingDetails.class), 200, null, null, null).getResults();

    for (ListBlobItem blobItem: blobs) {
        if (blobItem instanceof CloudBlockBlob) {
            CloudBlockBlob blob = (CloudBlockBlob)blobItem;
            File downloadTo = new File(directory, blob.getName());

            blob.downloadToFile(downloadTo.getPath());
        }
    }

    System.out.println("Download complete.");
}
```

Tam kod örneğine bakın: [EncodingWithMESPredefinedPreset](https://github.com/Azure-Samples/media-services-v3-java/blob/master/VideoEncoding/EncodingWithMESPredefinedPreset/src/main/java/sample/EncodingWithMESPredefinedPreset.java)

## <a name="net"></a>.NET

```csharp
/// <summary>
/// Use Media Service and Storage APIs to download the output files to a local folder
/// </summary>
/// <param name="client">The Media Services client.</param>
/// <param name="resourceGroupName">The name of the resource group within the Azure subscription.</param>
/// <param name="accountName">The Media Services account name.</param>
/// <param name="assetName">The asset name.</param>
/// <param name="resultsFolder">The output folder name for downloaded files.</param>
/// <returns>A task.</returns>
private async static Task DownloadResults(IAzureMediaServicesClient client, string resourceGroupName, string accountName, string assetName, string resultsFolder)
{
    AssetContainerSas assetContainerSas = client.Assets.ListContainerSas(
                    resourceGroupName, 
                    accountName, 
                    assetName,
                    permissions: AssetContainerPermission.Read, 
                    expiryTime: DateTime.UtcNow.AddHours(1).ToUniversalTime()
                    );

    Uri containerSasUrl = new Uri(assetContainerSas.AssetContainerSasUrls.FirstOrDefault());
    CloudBlobContainer container = new CloudBlobContainer(containerSasUrl);

    string directory = Path.Combine(resultsFolder, assetName);
    Directory.CreateDirectory(directory);

    Console.WriteLine("Downloading results to {0}.", directory);
    
    var blobs = container.ListBlobsSegmentedAsync(null,true, BlobListingDetails.None,200,null,null,null).Result;
    
    foreach (var blobItem in blobs.Results)
    {
        if (blobItem is CloudBlockBlob)
        {
            CloudBlockBlob blob = blobItem as CloudBlockBlob;
            string filename = Path.Combine(directory, blob.Name);

            await blob.DownloadToFileAsync(filename, FileMode.Create);
        }
    }

    Console.WriteLine("Download complete.");
}
```

Tam kod örneğine bakın: [EncodingWithMESPredefinedPreset](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/master/VideoEncoding/EncodingWithMESPredefinedPreset/Program.cs)

## <a name="next-steps"></a>Sonraki adımlar

[HTTPS URL'sinden iş girişi oluşturun.](job-input-from-http-how-to.md)
