---
title: Azure Medya Hizmetleri ile kodlama yaparken bir videonun alt klibi
description: Bu konu, .NET SDK kullanarak Azure Medya Hizmetleri ile kodlama yaparken bir videonun nasıl alt klip kesilen nasıl açıklanır
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/09/2019
ms.author: juliako
ms.openlocfilehash: 3d584ee742aa93cdecf4b04d942afb2ed83a7357
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67305058"
---
# <a name="subclip-a-video-when-encoding-with-media-services---net"></a>Medya Hizmetleri ile kodlama yaparken bir videonun alt klibi - .NET

Bir [İş'i](https://docs.microsoft.com/rest/api/media/jobs)kullanarak kodlarken videoyu kırpabilir veya alt kesebilirsiniz. Bu [işlevsellik, BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) hazır ayarları veya [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) hazır ayarları kullanılarak oluşturulan herhangi bir [Transform](https://docs.microsoft.com/rest/api/media/transforms) ile çalışır.

Aşağıdaki C# örneği, bir kodlama işi gönderirken Bir Varlık'taki videoyu kırpan bir iş oluşturur. 

## <a name="prerequisites"></a>Ön koşullar

Bu konuda açıklanan adımları tamamlamak için şunları yapmak zorundasınız:

- [Azure Media Services hesabı oluşturma](create-account-cli-how-to.md)
- Bir Dönüşüm ve giriş ve çıktı Varlıkları oluşturun. .NET öğreticisini [kullanarak Yükleme, kodlama ve akış videolarında](stream-files-tutorial-with-api.md) Varlıkları Dönüştür ve giriş ve çıktı olarak nasıl oluşturabileceğinizi görebilirsiniz.
- Kodlama kavramı konusunu gözden [geçirin.](encoding-concept.md)

## <a name="example"></a>Örnek

```csharp
/// <summary>
/// Submits a request to Media Services to apply the specified Transform to a given input video.
/// </summary>
/// <param name="client">The Media Services client.</param>
/// <param name="resourceGroupName">The name of the resource group within the Azure subscription.</param>
/// <param name="accountName"> The Media Services account name.</param>
/// <param name="transformName">The name of the transform.</param>
/// <param name="jobName">The (unique) name of the job.</param>
/// <param name="inputAssetName">The name of the input asset.</param>
/// <param name="outputAssetName">The (unique) name of the  output asset that will store the result of the encoding job. </param>
// <SubmitJob>
private static async Task<Job> JobWithBuiltInStandardEncoderWithSingleClipAsync(
    IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string transformName,
    string jobName,
    string inputAssetName,
    string outputAssetName)
{
    var jobOutputs = new List<JobOutputAsset>
    {
        new JobOutputAsset(state: JobState.Queued, progress: 0, assetName: outputAssetName)
    };

    var clipStart = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 20)
    };

    var clipEnd = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 30)
    };

    var jobInput = new JobInputAsset(assetName: inputAssetName, start: clipStart, end: clipEnd);

    Job job = await client.Jobs.CreateAsync(
        resourceGroupName,
        accountName,
        transformName,
        jobName,
        new Job(input: jobInput, outputs: jobOutputs.ToArray(), name: jobName)
        {
            Description = $"A Job with transform {transformName} and single clip.",
            Priority = Priority.Normal,
        });

    return job;
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Özel bir dönüştürme ile kodlama](customize-encoder-presets-how-to.md) 
