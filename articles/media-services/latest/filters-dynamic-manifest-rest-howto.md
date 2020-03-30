---
title: Azure Media Services v3 REST API ile filtre oluşturma
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
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: f9134dd3bc926e6e2f454e5187e03365e91ed22a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75780343"
---
# <a name="creating-filters-with-media-services-rest-api"></a>Media Services REST API ile filtre oluşturma

İçeriğinizi müşterilere teslim ederken (Canlı etkinlikler veya Talep Üzerine Video akışı) müşterinizin varsayılan varlığın bildirim dosyasında açıklanandan daha fazla esnekliğe ihtiyacı olabilir. Azure Medya Hizmetleri, içeriğiniz için hesap filtreleri ve varlık filtreleri tanımlamanıza olanak tanır. 

Bu özelliğin ayrıntılı açıklaması ve kullanıldığı [senaryolar](filters-concept.md)için [Dinamik Bildirimler](filters-dynamic-manifest-overview.md) ve Filtreler bölümüne bakın.

Bu konu, Talep Üzerine Video varlığı için bir filtrenin nasıl tanımlandığını ve [Hesap Filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) ve [Varlık Filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters)oluşturmak için REST API'lerini nasıl kullanacağımı gösterir. 

> [!NOTE]
> Sunuyu gözden geçirdiğinden emin [olunTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Ön koşullar 

Bu konuda açıklanan adımları tamamlamak için şunları yapmak zorundasınız:

- [Filtreleri ve dinamik bildirimleri gözden geçirin.](filters-dynamic-manifest-overview.md)
- [Azure Medya Hizmetleri REST API çağrıları için Postacı yapılandırın.](media-rest-apis-with-postman.md)

    [Azure AD Belirteci'ni alın'](media-rest-apis-with-postman.md#get-azure-ad-token)ın son adımını takip edin. 

## <a name="define-a-filter"></a>Filtre tanımlama  

Aşağıda, bildirime eklenen parça seçim koşullarını tanımlayan **İstek gövdesi** örneği verilmiştir. Bu filtre, EC-3 olan tüm ses parçalarını ve 0-1000000 aralığında bit hızına sahip tüm video parçalarını içerir.

```json
{
    "properties": {
        "tracks": [
          {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Audio",
                        "operation": "Equal"
                    },
                    {
                        "property": "FourCC",
                        "value": "EC-3",
                        "operation": "Equal"
                    }
                ]
            },
            {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Video",
                        "operation": "Equal"
                    },
                    {
                        "property": "Bitrate",
                        "value": "0-1000000",
                        "operation": "Equal"
                    }
                ]
            }
        ]
     }
}
```

## <a name="create-account-filters"></a>Hesap filtreleri oluşturma

İndirdiğiniz Postacı koleksiyonunda **Hesap Filtreleri**->**Oluştur'u seçin veya Hesap Filtresini güncelleştirin.**

**PUT** HTTP istek yöntemi aşağıdakilere benzer:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/accountFilters/{filterName}?api-version=2018-07-01
```

**Gövde** sekmesini seçin ve [daha önce tanımladığınız](#define-a-filter)json kodunu yapıştırın.

**Gönder**’i seçin. 

Filtre oluşturuldu.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate) Ayrıca, [filtreler için JSON örneklerine](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create-an-account-filter)bakın.

## <a name="create-asset-filters"></a>Varlık filtreleri oluşturma  

İndirdiğiniz "Medya Hizmetleri v3" Postacı koleksiyonunda **Varlık**->**Oluştur veya Güncelleştir'i**seçin.

**PUT** HTTP istek yöntemi aşağıdakilere benzer:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/assets/{assetName}/assetFilters/{filterName}?api-version=2018-07-01
```

**Gövde** sekmesini seçin ve [daha önce tanımladığınız](#define-a-filter)json kodunu yapıştırın.

**Gönder**’i seçin. 

Varlık filtresi oluşturuldu.

Varlık filtrelerinin nasıl oluşturulacağı veya güncelleştirilenhakkında ayrıntılı bilgi [için](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate)bkz. Ayrıca, [filtreler için JSON örneklerine](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create-an-asset-filter)bakın. 

## <a name="associate-filters-with-streaming-locator"></a>Filtreleri Akış Bulucu ile ilişkilendirin

Akış Lı'nız için geçerli olacak varlık veya hesap filtrelerinin listesini belirtebilirsiniz. [Dinamik Paketleyici (Streaming Endpoint),](dynamic-packaging-overview.md) bu filtre listesini istemcinizin URL'de belirttiği filtrelerle birlikte uygular. Bu kombinasyon, Akış Lı Buluc'ta belirttiğiniz URL + filtrelerdeki filtrelere dayanan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. Filtre uygulamak istiyorsanız ancak URL'deki filtre adlarını ortaya çıkarmak istemiyorsanız bu özelliği kullanmanızı öneririz.

REST kullanarak bir Akış Bulucu ile filtreler oluşturmak ve ilişkilendirmek [için, Akış Lı Konum belirleyiciler](https://docs.microsoft.com/rest/api/media/streaminglocators/create) kullanın - API oluştur ve İstek `properties.filters` [Gövdesi'nde](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body)belirtin.
                                
## <a name="stream-using-filters"></a>Filtreleri kullanarak akış

Filtreleri tanımladıktan sonra, istemcileriniz bunları akış URL'sinde kullanabilir. Filtreler uyarlanabilir bitrate akış protokollerine uygulanabilir: Apple HTTP Live Streaming (HLS), MPEG-DASH ve Smooth Streaming.

Aşağıdaki tablo, filtreli URL'lerin bazı örneklerini gösterir:

|Protokol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-steps"></a>Sonraki adımlar

[Videoları akış](stream-files-tutorial-with-rest.md) 
