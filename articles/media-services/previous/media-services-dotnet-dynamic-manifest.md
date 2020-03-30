---
title: Azure Media Services .NET SDK ile Filtre Oluşturma
description: Bu konu, istemcinizin bir akışın belirli bölümlerini akışı için bunları kullanabilmesi için filtrelerin nasıl oluşturulabileceğini açıklar. Medya Hizmetleri, bu seçici akışı elde etmek için dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.reviewer: cenkdin
ms.openlocfilehash: c60b223f91a151bf63cabc5e95816f2545022503
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69016597"
---
# <a name="creating-filters-with-media-services-net-sdk"></a>Medya Hizmetleri ile Filtre Oluşturma .NET SDK 
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [Geri kalanı](media-services-rest-dynamic-manifest.md)
> 
> 

2.17 sürümüyle başlayarak, Medya Hizmetleri varlıklarınız için filtreler tanımlamanıza olanak tanır. Bu filtreler, müşterilerinizin aşağıdaki gibi şeyler yapmayı seçmelerine olanak tanıyan sunucu tarafı kurallarıdır: videonun yalnızca bir bölümünü oynatma (videonun tamamını oynatmak yerine) veya yalnızca müşterinizin cihazının işleyebilir ses ve video yorumlamalarının bir alt kümesini belirtin (bunun yerine varlıkla ilişkili tüm yorumlamalar). Varlıklarınızın bu filtrelemi, müşterinizin belirtilen filtre(ler'e dayalı bir video akışı için istek üzerine oluşturulan **Dinamik Bildirimler)** aracılığıyla gerçekleştirilir.

Filtreler ve Dinamik Bildirim ile ilgili daha ayrıntılı bilgi için [Dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md)bakın.

Bu makalede, filtre oluşturmak, güncelleştirmek ve silmek için Media Services .NET SDK'nın nasıl kullanılacağı gösterilmektedir. 

Bir filtreyi güncellerseniz, kuralların yenilenmesi için bitiş noktası akışının iki dakika kadar sürebileceğini unutmayın. İçerik bu filtre kullanılarak sunulduysa (ve yakınlıkve CDN önbelleklerinde önbelleğe alınmışsa), bu filtreyi güncelleştirmek oynatıcı hatalarına neden olabilir. Filtreyi güncelledikten sonra önbelleği her zaman temizleyin. Bu seçenek mümkün değilse, farklı bir filtre kullanmayı düşünün. 

## <a name="types-used-to-create-filters"></a>Filtre oluşturmak için kullanılan türler
Filtreler oluşturulurken aşağıdaki türler kullanılır: 

* **IStreamingFiltresi**.  Bu tür aşağıdaki REST API [Filtresi](https://docs.microsoft.com/rest/api/media/operations/filter) dayanmaktadır
* **iStreamingAssetFiltresi**. Bu tür aşağıdaki REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter) dayanmaktadır
* **SunumZaman Aralığı**. Bu tür aşağıdaki REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange) dayanmaktadır
* **FilterTrackSelectStatement** ve **IFilterTrackPropertyCondition**. Bu türler aşağıdaki REST API'leri [FilterTrackSelect ve FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect) dayanmaktadır

## <a name="createupdatereaddelete-global-filters"></a>Genel filtreler oluşturma/güncelleme/Okuma/Silme
Aşağıdaki kod, varlık filtreleri oluşturmak, güncelleştirmek, okumak ve silmek için .NET'in nasıl kullanılacağını gösterir.

```csharp
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();
```

## <a name="createupdatereaddelete-asset-filters"></a>Varlık filtreleri oluşturma/güncelleme/Okuma/Silme
Aşağıdaki kod, varlık filtreleri oluşturmak, güncelleştirmek, okumak ve silmek için .NET'in nasıl kullanılacağını gösterir.

```csharp
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();

```


## <a name="build-streaming-urls-that-use-filters"></a>Filtreler kullanan akış URL'leri oluşturma
Varlıklarınızın nasıl yayımlayacağı ve teslim edilen hakkında bilgi için [bkz.](media-services-deliver-content-overview.md)

Aşağıdaki örnekler, akış URL'lerinize nasıl filtre ekleyeceğinizgösterilmektedir.

**MPEG DASH** 

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Canlı Akış (HLS) V4**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Canlı Akış (HLS) V3**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Kesintisiz Akış**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Dinamik manifestolara genel bakış](media-services-dynamic-manifest-overview.md)

