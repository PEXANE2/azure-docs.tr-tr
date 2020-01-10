---
title: Azure Media Services v3 REST API ile Filtreler oluşturma
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış elde etmek için olan dinamik bildirimler oluşturur.
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
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75780343"
---
# <a name="creating-filters-with-media-services-rest-api"></a>Media Services REST API'si ile filtre oluşturma

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. 

Bu özelliğin ve kullanılan senaryoların ayrıntılı açıklaması için bkz. [dinamik bildirimler](filters-dynamic-manifest-overview.md) ve [Filtreler](filters-concept.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre tanımlar ve oluşturmak için REST API'lerini kullanma işlemi gösterilmektedir [hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) ve [varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters). 

> [!NOTE]
> [Presentationtimerange](filters-concept.md#presentationtimerange)öğesini gözden geçirdiğinizden emin olun.

## <a name="prerequisites"></a>Ön koşullar 

Bu konu başlığı altında açıklanan adımları tamamlamak için için gerekenler:

- Gözden geçirme [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).
- [Azure Media Services REST API çağrıları için Postman yapılandırma](media-rest-apis-with-postman.md).

    [Azure AD belirtecini al](media-rest-apis-with-postman.md#get-azure-ad-token)konusunun son adımını izlediğinizden emin olun. 

## <a name="define-a-filter"></a>Bir filtre tanımlar  

Aşağıdaki **istek gövdesi** bildirimine eklenmesini izleme seçimi koşullarını tanımlayan örnek. Bu filtre, EC-3 olan tüm ses izlerini ve 0-1000000 aralığında bit hızına sahip video izlemelerini içerir.

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

İndirdiğiniz Postman'ın koleksiyonda seçin **hesap filtreleri**->**oluşturma veya güncelleştirme Hesap Filtresi**.

**PUT** HTTP istek yöntemine benzerdir:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/accountFilters/{filterName}?api-version=2018-07-01
```

Seçin **gövdesi** sekmesi ve Yapıştır json kodunu [daha önce tanımlanan](#define-a-filter).

**Gönder**’i seçin. 

Filtre oluşturuldu.

Daha fazla bilgi için [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate). Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create-an-account-filter).

## <a name="create-asset-filters"></a>Varlık filtre oluşturma  

İndirdiğiniz "Media Services v3" Postman koleksiyonunda,->**varlıklar** ' ı seçerek **varlık Filtresi Oluştur veya Güncelleştir**' i seçin.

**PUT** HTTP istek yöntemine benzerdir:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/assets/{assetName}/assetFilters/{filterName}?api-version=2018-07-01
```

Seçin **gövdesi** sekmesi ve Yapıştır json kodunu [daha önce tanımlanan](#define-a-filter).

**Gönder**’i seçin. 

Varlık filtre oluşturuldu.

Oluşturulacak veya güncelleştirilecek varlık filtreleri hakkında ayrıntılı bilgi için bkz. [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate). Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create-an-asset-filter). 

## <a name="associate-filters-with-streaming-locator"></a>Filtreleri akış bulucu ile ilişkilendir

Akış Konumlayıcı için uygulanabilecek varlık veya hesap filtrelerinin bir listesini belirtebilirsiniz. [Dinamik Paketleyici (akış uç noktası)](dynamic-packaging-overview.md) , bu filtre listesini ISTEMCINIZDEKI URL 'de belirttiği değişikliklerle birlikte uygular. Bu bileşim, akış Bulucu üzerinde belirlediğiniz URL + filtrelerdeki filtreleri temel alan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. Filtre uygulamak, ancak URL 'de filtre adlarını göstermek istemiyorsanız bu özelliği kullanmanızı öneririz.

REST kullanarak bir akış bulucu ile filtreler oluşturup ilişkilendirmek için, [akış Konumlandırıcı-API oluştur](https://docs.microsoft.com/rest/api/media/streaminglocators/create) ' u kullanın ve [istek gövdesinde](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body)`properties.filters` belirtin.
                                
## <a name="stream-using-filters"></a>Filtreleri kullanarak akış

Filtreleri tanımladıktan sonra, istemcileriniz bunları akış URL 'sinde kullanabilir. Filtreler, uyarlamalı bit hızı akış protokollerine uygulanabilir: Apple HTTP Canlı Akışı (HLS), MPEG-DASH ve Kesintisiz Akış.

Aşağıdaki tabloda, filtre içeren URL 'lerin bazı örnekleri gösterilmektedir:

|Protokol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-steps"></a>Sonraki adımlar

[Stream videoları](stream-files-tutorial-with-rest.md) 
