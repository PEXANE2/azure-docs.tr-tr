---
title: Windows Azure Media Encoder'dan Media Encoder Standardına geçiş | Microsoft Dokümanlar
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
ms.date: 10/17/2019
ms.author: juliako
ms.openlocfilehash: e75e3f3eecf6c34050aeaa7fe387fffb0de58a74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513210"
---
# <a name="migrate-from-windows-azure-media-encoder-to-media-encoder-standard"></a>Windows Azure Media Encoder'dan Media Encoder Standardına geçiş

Bu makalede, eski Windows Azure Media Encoder (WAME) medya işlemcisinden (kullanımdan kaldırılan) Media Encoder Standart medya işlemcisine geçiş adımları açıklanmaktadır. Emeklilik tarihleri için bu [eski bileşenler](legacy-components.md) konusuna bakın.

WAME ile dosyaları kodlarken, müşteriler genellikle `H264 Adaptive Bitrate MP4 Set 1080p`. Geçiş yapabilmek için, kodunuzu WAME yerine **Media Encoder Standart** medya işlemcisi ve eşdeğer `H264 Multiple Bitrate 1080p`sistem önceden [ayarlarından](media-services-mes-presets-overview.md) birini kullanmak için güncelleştirilmelidir. 

## <a name="migrating-to-media-encoder-standard"></a>Ortam Kodlayıcı Standardına Geçiş

Burada, eski bileşeni kullanan tipik bir C# kodu örneği verem. 

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("WAME Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Windows Azure Media Encoder"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Adaptive Bitrate MP4 Set 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    "H264 Adaptive Bitrate MP4 Set 1080p", 
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

Eğer kendi şema kullanarak WAME için kendi kodlama önayar oluşturmuş olsaydı, [Media Encoder Standart için eşdeğer](media-services-mes-schema.md)bir şema vardır.

## <a name="known-differences"></a>Bilinen farklar 

Media Encoder Standard daha sağlam, güvenilir, daha iyi performansa sahip ve eski WAME kodlayıcısından daha kaliteli çıktı üretir. Ayrıca: 

* Media Encoder Standard, WAME'den farklı bir adlandırma kuralına sahip çıktı dosyaları üretir.
* Media Encoder [Standard, giriş dosyası meta verilerini](media-services-input-metadata-schema.md) ve çıktı [dosyası(lar) meta verilerini](media-services-output-metadata-schema.md)içeren dosyalar gibi yapılar üretir.
* [Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/media-services/#encoding) (özellikle SSS bölümünde) belgelendirildiği gibi, Media Encoder Standard'ı kullanarak videoları kodlarken, çıktı olarak üretilen dosyaların süresine bağlı olarak faturalandırılırsınız. WAME ile, giriş video dosyası(lar) ve çıktı video dosyası(lar) boyutlarına göre faturalandırılırsınız.

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

[Yeni destek isteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) yönlendirerek destek bileti açabilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

* [Eski bileşenler](legacy-components.md)
* [Fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/#encoding)
