---
title: Azure Medya Hizmetleri ile filtre oluşturmak için CLI'yi kullanın| Microsoft Dokümanlar
description: Bu makalede, Azure Media Services v3 ile filtreler oluşturmak için CLI nasıl kullanılacağı gösterilmektedir.
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
ms.date: 06/13/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 74516aa921e45917f327a193a1c972b021c9c8ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74896067"
---
# <a name="creating-filters-with-cli"></a>CLI ile filtre oluşturma 

İçeriğinizi müşterilere teslim ederken (Canlı etkinlikler veya Talep üzerine Video akışı), istemcinizin varsayılan varlığın bildirim dosyasında açıklanandan daha fazla esnekliğe ihtiyacı olabilir. Azure Medya Hizmetleri, içeriğiniz için hesap filtreleri ve varlık filtreleri tanımlamanıza olanak tanır. 

Bu özelliğin ayrıntılı açıklaması ve kullanıldığı [senaryolar](filters-concept.md)için [Dinamik Bildirimler](filters-dynamic-manifest-overview.md) ve Filtreler bölümüne bakın.

Bu konu, Bir Video on-Demand varlık için bir filtre yapılandırma ve Hesap [Filtreleri](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) ve [Varlık Filtreleri](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)oluşturmak için Medya Hizmetleri v3 için CLI kullanmak nasıl gösterir. 

> [!NOTE]
> Sunuyu gözden geçirdiğinden emin [olunTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Ön koşullar 

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun. 

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-filter"></a>Filtre tanımlama 

Aşağıdaki örnek, son bildirime eklenen parça seçim koşullarını tanımlar. Bu filtre, EC-3 olan tüm ses parçalarını ve 0-1000000 aralığında bit hızına sahip tüm video parçalarını içerir.

> [!TIP]
> REST'te **Filtreler'i** tanımlamayı planlıyorsanız, "Özellikler" sarıcı JSON nesnesini eklemeniz gerektiğine dikkat edin.  

```json
[
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
                "operation": "NotEqual"
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
```

## <a name="create-account-filters"></a>Hesap filtreleri oluşturma

Aşağıdaki [az ams hesap filtresi](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) komutu, [daha önce tanımlanan](#define-a-filter)filtre izleme seçimleri içeren bir hesap filtresi oluşturur. 

Komut, parça seçimlerini temsil `--tracks` eden JSON içeren isteğe bağlı bir parametreyi geçirmenize olanak tanır.  JSON'u bir dosyadan yüklemek için @{file} kullanın. Azure CLI'yi yerel olarak kullanıyorsanız, tüm dosya yolunu belirtin:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Ayrıca, [filtreler için JSON örneklerine](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create-an-account-filter)bakın.

## <a name="create-asset-filters"></a>Varlık filtreleri oluşturma

Aşağıdaki [az ams varlık filtresi](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) komutu, [daha önce tanımlanan](#define-a-filter)filtre izleme seçimlerine sahip bir varlık filtresi oluşturur. 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Ayrıca, [filtreler için JSON örneklerine](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create-an-asset-filter)bakın.

## <a name="associate-filters-with-streaming-locator"></a>Filtreleri Akış Bulucu ile ilişkilendirin

Akış Lı'nız için geçerli olacak varlık veya hesap filtrelerinin listesini belirtebilirsiniz. [Dinamik Paketleyici (Streaming Endpoint),](dynamic-packaging-overview.md) bu filtre listesini istemcinizin URL'de belirttiği filtrelerle birlikte uygular. Bu kombinasyon, Akış Lı Buluc'ta belirttiğiniz URL + filtrelerdeki filtrelere dayanan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. Filtre uygulamak istiyorsanız ancak URL'deki filtre adlarını ortaya çıkarmak istemiyorsanız bu özelliği kullanmanızı öneririz.

Aşağıdaki CLI kodu, Akış Bulucu'nun nasıl `filters`oluşturulup belirtilen. Bu, varlık filtresi adlarının ve/veya hesap filtresi adlarının boşluktan ayrılmış bir listesini alan isteğe bağlı bir özelliktir.

```azurecli
az ams streaming-locator create -a amsAccount -g resourceGroup -n streamingLocatorName \
                                --asset-name assetName \                               
                                --streaming-policy-name policyName \
                                --filters filterName1 filterName2
                                
```

## <a name="stream-using-filters"></a>Filtreleri kullanarak akış

Filtreleri tanımladıktan sonra, istemcileriniz bunları akış URL'sinde kullanabilir. Filtreler uyarlanabilir bitrate akış protokollerine uygulanabilir: Apple HTTP Live Streaming (HLS), MPEG-DASH ve Smooth Streaming.

Aşağıdaki tablo, filtreli URL'lerin bazı örneklerini gösterir:

|Protokol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-step"></a>Sonraki adım

[Videoları akış](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
