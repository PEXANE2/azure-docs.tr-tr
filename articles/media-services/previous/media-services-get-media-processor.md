---
title: .NET| için Azure Medya Hizmetleri SDK'sını kullanarak medya işlemcisi oluşturma Microsoft Dokümanlar
description: Azure Medya Hizmetleri için medya içeriğini kodlamak, biçimlendirmek, şifrelemek veya şifresini çözmek için medya işlemcisi bileşenini nasıl oluşturabileceğinizi öğrenin. Kod örnekleri C# olarak yazılır ve .NET için Medya Hizmetleri SDK'sını kullanın.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: af6badda426f1bb81d8528cfda9b8c02d55712b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61463863"
---
# <a name="how-to-get-a-media-processor-instance"></a>Nasıl yapılsın: Medya İşlemcisi örneği alın
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [Geri kalanı](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Medya Hizmetleri'nde ortam işlemcisi, medya içeriğini kodlama, biçim dönüştürme, şifreleme veya şifre çözme gibi belirli bir işleme görevini işleyen bir bileşendir. Medya içeriğinin biçimini kodlamak, şifrelemek veya dönüştürmek için bir görev oluştururken genellikle bir ortam işlemcisi oluşturursunuz.

## <a name="azure-media-processors"></a>Azure ortam işlemcileri 

Aşağıdaki konu, ortam işlemcilerinin listesini sağlar:

* [Kodlama medya işleyicileri](scenarios-and-availability.md#encoding-media-processors)
* [Analiz medya işlemcileri](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Ortam İşlemcisi Alın

Aşağıdaki yöntem, bir ortam işlemci örneği almak için nasıl gösterir. Kod örneği, sunucu bağlamına başvurmak için **_context** adlı modül düzeyinde bir değişkenin kullanımını varsayar nasıl [kullanılır: Medya Hizmetlerine Programlı olarak bağlanma.](media-services-use-aad-auth-to-access-ams-api.md)

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Artık bir medya işlemciörneğini nasıl elde edeceğinizkonusunda, bir varlığı kodlamak için Media Encoder Standardını nasıl kullanacağınızı gösterecek bir Varlık konusunu nasıl [kodlarsınız](media-services-dotnet-encode-with-media-encoder-standard.md) konusuna gidin.

