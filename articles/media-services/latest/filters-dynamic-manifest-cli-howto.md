---
title: Azure Media Services ile filtre oluşturma için CLI kullanma | Microsoft Docs
description: Bu makalede, Azure Media Services v3 ile filtre oluşturmak için CLı 'nin nasıl kullanılacağı gösterilmektedir.
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
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896067"
---
# <a name="creating-filters-with-cli"></a>CLI ile filtre oluşturma 

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. 

Bu özelliğin ve kullanılan senaryoların ayrıntılı açıklaması için bkz. [dinamik bildirimler](filters-dynamic-manifest-overview.md) ve [Filtreler](filters-concept.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre yapılandırın ve oluşturmak için Media Services v3 için CLI'yi kullanma işlemi gösterilmektedir [hesap filtreleri](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) ve [varlık filtreleri](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

> [!NOTE]
> [Presentationtimerange](filters-concept.md#presentationtimerange)öğesini gözden geçirdiğinizden emin olun.

## <a name="prerequisites"></a>Önkoşullar 

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-filter"></a>Bir filtre tanımlar 

Aşağıdaki örnek, son bildirimine eklenmesini izleme seçimi koşulları tanımlar. Bu filtre, EC-3 olan tüm ses izlerini ve 0-1000000 aralığında bit hızına sahip video izlemelerini içerir.

> [!TIP]
> BEKLEYEN **filtreleri** tanımlamanızı planlıyorsanız, "Özellikler" sarmalayıcı JSON nesnesini eklemeniz gerektiğini unutmayın.  

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

Aşağıdaki [az ams hesabına-filtre](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) komut olan izleme seçimlerini Filtresi ile bir hesabı filtresi oluşturur [daha önce tanımlanan](#define-a-filter). 

Komutu, izleme seçimlerini temsil eden JSON içeren isteğe bağlı bir `--tracks` parametresini geçirmenize olanak sağlar.  JSON 'ı bir dosyadan yüklemek için @ {File} kullanın. Azure CLı 'yi yerel olarak kullanıyorsanız, tüm dosya yolunu belirtin:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create-an-account-filter).

## <a name="create-asset-filters"></a>Varlık filtre oluşturma

Aşağıdaki [az ams varlığı-filtre](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) komut olan izleme seçimlerini Filtresi ile bir varlık filtresi oluşturur [daha önce tanımlanan](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create-an-asset-filter).

## <a name="associate-filters-with-streaming-locator"></a>Filtreleri akış bulucu ile ilişkilendir

Akış Konumlayıcı için uygulanabilecek varlık veya hesap filtrelerinin bir listesini belirtebilirsiniz. [Dinamik Paketleyici (akış uç noktası)](dynamic-packaging-overview.md) , bu filtre listesini ISTEMCINIZDEKI URL 'de belirttiği değişikliklerle birlikte uygular. Bu bileşim, akış Bulucu üzerinde belirlediğiniz URL + filtrelerdeki filtreleri temel alan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. Filtre uygulamak, ancak URL 'de filtre adlarını göstermek istemiyorsanız bu özelliği kullanmanızı öneririz.

Aşağıdaki CLı kodu, bir akış Bulucu oluşturmayı ve `filters`belirtmeyi gösterir. Bu, varlık filtresi adlarının ve/veya hesap filtresi adlarının boşlukla ayrılmış bir listesini alan isteğe bağlı bir özelliktir.

```azurecli
az ams streaming-locator create -a amsAccount -g resourceGroup -n streamingLocatorName \
                                --asset-name assetName \                               
                                --streaming-policy-name policyName \
                                --filters filterName1 filterName2
                                
```

## <a name="stream-using-filters"></a>Filtreleri kullanarak akış

Filtreleri tanımladıktan sonra, istemcileriniz bunları akış URL 'sinde kullanabilir. Filtreler, uyarlamalı bit hızı akış protokollerine uygulanabilir: Apple HTTP Canlı Akışı (HLS), MPEG-DASH ve Kesintisiz Akış.

Aşağıdaki tabloda, filtre içeren URL 'lerin bazı örnekleri gösterilmektedir:

|Protokol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-step"></a>Sonraki adım

[Stream videoları](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
