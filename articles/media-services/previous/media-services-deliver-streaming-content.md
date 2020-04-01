---
title: Azure Medya Hizmetleri içeriğini .NET kullanarak yayımlama | Microsoft Dokümanlar
description: Akış URL'si oluşturmak için kullanılan bir bulucu oluşturmayı öğrenin. Kod örnekleri C# olarak yazılır ve .NET için Medya Hizmetleri SDK'sını kullanın.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 615a6afb0f7a3e133603db10e7c79add3322070c
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476701"
---
# <a name="publish-media-services-content-using-net"></a>.NET kullanarak Medya Hizmetleri içeriğini yayımla  
> [!div class="op_single_selector"]
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [Portal](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a>Genel Bakış
OnDemand akış bulucusu oluşturup akış URL'si oluşturarak uyarlanabilir bithızı MP4 kümesini aktarabilirsiniz. Bir [varlık konusunun kodlanması,](media-services-encode-asset.md) uyarlanabilir bitrate MP4 kümesine nasıl kodlanacağını gösterir. 

> [!NOTE]
> İçeriğiniz şifrelenmişse, bir bulucu oluşturmadan önce varlık teslim ilkesini [(bu](media-services-dotnet-configure-asset-delivery-policy.md) konuda açıklandığı şekilde) yapılandırın. 
> 
> 

Kademeli olarak indirilebilen MP4 dosyalarını gösteren URL'ler oluşturmak için bir OnDemand akış bulucusu da kullanabilirsiniz.  

Bu konu, varlığınızı yayımlamak ve Düzgün, MPEG DASH ve HLS akış URL'leri oluşturmak için bir OnDemand akış bulucusu oluşturmanın nasıl yapılacağını gösterir. Ayrıca ilerici indirme URL'leri oluşturmak için sıcak gösterir. 

## <a name="create-an-ondemand-streaming-locator"></a>OnDemand akış bulucusu oluşturma
OnDemand akış bulucusu oluşturmak ve URL'leri almak için aşağıdaki leri yapmanız gerekir:

1. İçerik şifrelenmişse, bir erişim ilkesi tanımlayın.
2. OnDemand akış bulucusu oluşturun.
3. Akış yapmayı planlıyorsanız, varlıktaki akış bildirimi dosyasını (.ism) alın. 
   
   Aşamalı olarak indirmeyi planlıyorsanız, kıymetteki MP4 dosyalarının adlarını alın.  
4. URL'leri manifest dosyasına veya MP4 dosyalarına oluşturun. 


>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Her zaman aynı günleri / erişim izinlerini kullanıyorsanız aynı ilke kimliğini kullanın. Örneğin, uzun süre yerinde kalması amaçlanan bulucular için ilkeler (yükleme olmayan ilkeler). Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.

### <a name="use-media-services-net-sdk"></a>Medya Hizmetlerini Kullan .NET SDK
Akış URL'leri Oluşturun 

```csharp
    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.ToList().Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }
```

Çıktılar:

    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> İçeriğinizi TLS bağlantısı üzerinden de aktarabilirsiniz. Bu yaklaşımı yapmak için, akış URL'lerinizin HTTPS ile başladığından emin olun. Şu anda, AMS özel etki alanları ile TLS desteklemiyor.
> 
> 

Aşamalı indirme URL'leri oluşturma 

```csharp
    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }
```
Çıktılar:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a>Medya Hizmetlerini Kullan .NET SDK Uzantıları
Aşağıdaki kod, bir bulucu oluşturan ve uyarlanabilir akış için Düzgün Akış, HLS ve MPEG-DASH URL'lerini oluşturan .NET SDK uzantıları yöntemlerini çağırır.
```csharp
    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Varlıkları indirin](media-services-deliver-asset-download.md)
* [Varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md)

