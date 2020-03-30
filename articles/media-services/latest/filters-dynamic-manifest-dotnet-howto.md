---
title: Azure Medya Hizmetleri v3 .NET SDK ile filtre oluşturma
description: Bu konu, istemcinizin bir akışın belirli bölümlerini akışı için bunları kullanabilmesi için filtrelerin nasıl oluşturulabileceğini açıklar. Medya Hizmetleri, bu seçici akışı elde etmek için dinamik bildirimler oluşturur.
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
ms.date: 06/03/2019
ms.author: juliako
ms.openlocfilehash: ef04b1b7b5030189482e89e26e4565397cbdd7c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75779255"
---
# <a name="create-filters-with-media-services-net-sdk"></a>Medya Hizmetleri .NET SDK ile filtre oluşturma

İçeriğinizi müşterilere teslim ederken (Canlı etkinlikler veya Talep Üzerine Video akışı) müşterinizin varsayılan varlığın bildirim dosyasında açıklanandan daha fazla esnekliğe ihtiyacı olabilir. Azure Medya Hizmetleri, içeriğiniz için hesap filtreleri ve varlık filtreleri tanımlamanıza olanak tanır. 

Bu özelliğin ayrıntılı açıklaması ve kullanıldığı [senaryolar](filters-concept.md)için [Dinamik Bildirimler](filters-dynamic-manifest-overview.md) ve Filtreler bölümüne bakın.

Bu konu, Talep üzerine Video kıymeti için bir filtre tanımlamak ve [Hesap Filtreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.accountfilter?view=azure-dotnet) ve [Varlık Filtreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.assetfilter?view=azure-dotnet)oluşturmak için Medya Hizmetleri .NET SDK'nın nasıl kullanılacağını gösterir. 

> [!NOTE]
> Sunuyu gözden geçirdiğinden emin [olunTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Ön koşullar 

- [Filtreleri ve dinamik bildirimleri gözden geçirin.](filters-dynamic-manifest-overview.md)
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun. 
- [API'lere erişmek](access-api-cli-how-to.md) için gereken bilgileri alın
- [.NET SDK'yı kullanmaya](stream-files-tutorial-with-api.md#start_using_dotnet) nasıl başlayacağımı görmek için [Azure Medya Hizmetlerini kullanarak Upload'u, kodlamayı ve akışı](stream-files-tutorial-with-api.md) gözden geçirin

## <a name="define-a-filter"></a>Filtre tanımlama  

.NET'te, izleme seçimlerini [FilterTrackSelection](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.filtertrackselection?view=azure-dotnet) ve [FilterTrackPropertyCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.filtertrackpropertycondition?view=azure-dotnet) sınıfları ile yapılandırabilirsiniz. 

Aşağıdaki kod, EC-3 olan ses parçalarını ve 0-1000000 aralığında bit hızıolan video parçalarını içeren bir filtre tanımlar.

```csharp
var audioConditions = new List<FilterTrackPropertyCondition>()
{
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Type, "Audio", FilterTrackPropertyCompareOperation.Equal),
    new FilterTrackPropertyCondition(FilterTrackPropertyType.FourCC, "EC-3", FilterTrackPropertyCompareOperation.Equal)
};

var videoConditions = new List<FilterTrackPropertyCondition>()
{
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Type, "Video", FilterTrackPropertyCompareOperation.Equal),
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Bitrate, "0-1000000", FilterTrackPropertyCompareOperation.Equal)
};

List<FilterTrackSelection> includedTracks = new List<FilterTrackSelection>()
{
    new FilterTrackSelection(audioConditions),
    new FilterTrackSelection(videoConditions)
};
```

## <a name="create-account-filters"></a>Hesap filtreleri oluşturma

Aşağıdaki kod, yukarıda tanımlanan tüm parça seçimlerini içeren bir hesap filtresi oluşturmak için .NET'in nasıl [kullanılacağını](#define-a-filter)gösterir. 

```csharp
AccountFilter accountFilterParams = new AccountFilter(tracks: includedTracks);
client.AccountFilters.CreateOrUpdate(config.ResourceGroup, config.AccountName, "accountFilterName1", accountFilter);
```

## <a name="create-asset-filters"></a>Varlık filtreleri oluşturma

Aşağıdaki kod, yukarıda tanımlanan tüm parça seçimlerini içeren bir varlık filtresi oluşturmak için .NET'in nasıl [kullanılacağını](#define-a-filter)gösterir. 

```csharp
AssetFilter assetFilterParams = new AssetFilter(tracks: includedTracks);
client.AssetFilters.CreateOrUpdate(config.ResourceGroup, config.AccountName, encodedOutputAsset.Name, "assetFilterName1", assetFilterParams);
```

## <a name="associate-filters-with-streaming-locator"></a>Filtreleri Akış Bulucu ile ilişkilendirin

Akış Lı'nız için geçerli olacak varlık veya hesap filtrelerinin listesini belirtebilirsiniz. [Dinamik Paketleyici (Streaming Endpoint),](dynamic-packaging-overview.md) bu filtre listesini istemcinizin URL'de belirttiği filtrelerle birlikte uygular. Bu kombinasyon, Akış Lı Buluc'ta belirttiğiniz URL + filtrelerdeki filtrelere dayanan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. Filtre uygulamak istiyorsanız ancak URL'deki filtre adlarını ortaya çıkarmak istemiyorsanız bu özelliği kullanmanızı öneririz.

Aşağıdaki C# kodu, Akış Bulucu'nun nasıl `StreamingLocator.Filters`oluşturulup belirtilen. Bu, filtre adlarından `IList<string>` oluşan isteğe bağlı bir özelliktir.

```csharp
IList<string> filters = new List<string>();
filters.Add("filterName");

StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly,
        Filters = filters
    });
```
      
## <a name="stream-using-filters"></a>Filtreleri kullanarak akış

Filtreleri tanımladıktan sonra, istemcileriniz bunları akış URL'sinde kullanabilir. Filtreler uyarlanabilir bitrate akış protokollerine uygulanabilir: Apple HTTP Live Streaming (HLS), MPEG-DASH ve Smooth Streaming.

Aşağıdaki tablo, filtreli URL'lerin bazı örneklerini gösterir:

|Protokol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-steps"></a>Sonraki adımlar

[Videoları akış](stream-files-tutorial-with-api.md) 


