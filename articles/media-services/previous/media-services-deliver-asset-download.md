---
title: Medya Hizmetleri varlıklarını bilgisayarınıza indirin - Azure | Microsoft Dokümanlar
description: Varlıkları bilgisayarınıza indirme hakkında bilgi edinin. Kod örnekleri C# olarak yazılır ve .NET için Medya Hizmetleri SDK'sını kullanın.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 21fcc6ae09718ffbb22e1d438926586dd3cde71d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61465669"
---
# <a name="how-to-deliver-an-asset-by-download"></a>Nasıl yapılsın: Bir varlığı indirerek teslim etme  
Bu makalede, Medya Hizmetleri'ne yüklenen medya varlıklarını teslim etme seçenekleri açıklanır. Çok sayıda uygulama senaryosunda Medya Hizmetleri içeriği sunabilirsiniz. Kodladıktan sonra, oluşturulan ortam varlıklarını indirin veya bir akış bulucu kullanarak bunlara erişin. Daha iyi performans ve ölçeklenebilirlik için, İçerik Dağıtım Ağı (CDN) kullanarak içerik de sağlayabilirsiniz.

Bu örnek, medya varlıklarınıN Medya Hizmetlerinden yerel bilgisayarınıza nasıl indirilebildiğini gösterir. Kod, Medya Hizmetleri hesabıyla ilişkili işleri iş kimliğiyle sorgular ve **ÇıktıMediaAssets** koleksiyonuna erişir (bir işi çalıştırmadan kaynaklanan bir veya daha fazla çıktı ortamı varlığı kümesidir). Bu örnek, çıktı medya varlıklarının bir işten nasıl indirilebildiğini gösterir, ancak diğer varlıkları indirmek için aynı yaklaşımı uygulayabilirsiniz.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Her zaman aynı gün / erişim izinleri kullanıyorsanız, örneğin, uzun bir süre (yükleme olmayan ilkeler) yerinde kalması amaçlanan bulucular için ilkeler kullanıyorsanız aynı ilke kimliği kullanın. Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

```csharp
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference to the job. 
        IJob job = GetJob(jobId);

        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }
```


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Akış içeriği sunma](media-services-deliver-streaming-content.md)

