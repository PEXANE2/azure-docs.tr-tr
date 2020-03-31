---
title: Azure Media Encoder'dan Media Encoder Standardına geçiş | Microsoft Dokümanlar
description: Bu konu, Azure Media Encoder'dan Media Encoder Standart ortam işlemcisine nasıl geçirilir tartışılır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2019
ms.author: juliako
ms.openlocfilehash: f8fe1b13db6473e80f0d7cdc638b775a0c8062c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513510"
---
# <a name="migrate-from-azure-media-encoder-to-media-encoder-standard"></a>Azure Media Encoder'dan Media Encoder Standardına geçiş

Bu makalede, eski Azure Media Encoder (AME) ortam işlemcisinden (kullanımdan kaldırılan) Media Encoder Standart medya işlemcisine geçiş adımları anlatılmaktadır. Emeklilik tarihleri için bu [eski bileşenler](legacy-components.md) konusuna bakın.

AME ile dosyaları kodlarken, müşteriler genellikle `H264 Adaptive Bitrate MP4 Set 1080p`. Geçiş yapabilmek için, kodunuzun AME yerine **Media Encoder Standart** ortam işlemcisini ve '' gibi `H264 Multiple Bitrate 1080p`eşdeğer [sistem hazır ayarlarından](media-services-mes-presets-overview.md) birini kullanmak için güncelleştirilmeniz gerekir. 

## <a name="migrating-to-media-encoder-standard"></a>Ortam Kodlayıcı Standardına Geçiş

Burada eski ortam işlemcikullanan tipik bir C# kodu örneği verem. 

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("AME Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Adaptive Bitrate MP4 Set 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    " H264 Adaptive Bitrate MP4 Set 1080p", 
    TaskOptions.None); 
```

Burada Media Encoder Standard kullanan güncelleştirilmiş sürümü.

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("Media Encoder Standard Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Multiple Bitrate 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    "H264 Multiple Bitrate 1080p", 
    TaskOptions.None); 
```

### <a name="advanced-scenarios"></a>Gelişmiş senaryolar 

Şema kullanarak AME için kendi kodlama ön ayarını oluşturmuş olsaydınız, [Media Encoder Standard için eşdeğer](media-services-mes-schema.md)bir şema vardır. Eski ayarları yeni kodlayıcıyla nasıl eşlendirdiğinize ilişkin sorularınız varsa, lütfen bizemailto:amshelp@microsoft.com  
## <a name="known-differences"></a>Bilinen farklar 

Media Encoder Standard daha sağlam, güvenilir, daha iyi performansa sahip ve eski AME kodlayıcısından daha kaliteli çıktı üretir. Buna ek olarak: 

* Media Encoder Standard, AME'den farklı bir adlandırma kuralına sahip çıktı dosyaları üretir.
* Media Encoder [Standard, giriş dosyası meta verilerini](media-services-input-metadata-schema.md) ve çıktı [dosyası(lar) meta verilerini](media-services-output-metadata-schema.md)içeren dosyalar gibi yapılar üretir.

## <a name="next-steps"></a>Sonraki adımlar

* [Eski bileşenler](legacy-components.md)
* [Fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/#encoding)
