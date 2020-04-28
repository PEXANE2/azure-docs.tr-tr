---
title: Azure Media Services ile filtre oluşturmak için CLı kullanma | Microsoft Docs
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74896067"
---
# <a name="creating-filters-with-cli"></a>CLı ile filtre oluşturma 

İçeriğinizi müşterilere sunarken (canlı etkinlikler veya Isteğe bağlı video akışı), istemciniz varsayılan varlığın bildirim dosyasında açıklananlardan daha fazla esneklik gerektirebilir. Azure Media Services, içeriğiniz için hesap filtrelerini ve varlık filtrelerini tanımlamanızı sağlar. 

Bu özelliğin ve kullanılan senaryoların ayrıntılı açıklaması için bkz. [dinamik bildirimler](filters-dynamic-manifest-overview.md) ve [Filtreler](filters-concept.md).

Bu konuda, bir video Isteğe bağlı varlığı için bir filtrenin nasıl yapılandırılacağı ve [hesap filtreleri](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) ve [varlık filtreleri](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)oluşturmak IÇIN Media Services v3 için CLI kullanımı gösterilmektedir. 

> [!NOTE]
> [Presentationtimerange](filters-concept.md#presentationtimerange)öğesini gözden geçirdiğinizden emin olun.

## <a name="prerequisites"></a>Ön koşullar 

- [Media Services hesabı oluşturun](create-account-cli-how-to.md). Kaynak grubu adını ve Media Services hesap adını hatırlayacağınızdan emin olun. 

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-filter"></a>Filtre tanımlama 

Aşağıdaki örnek, son bildirime eklenen izleme seçim koşullarını tanımlar. Bu filtre, EC-3 olan tüm ses izlerini ve 0-1000000 aralığında bit hızına sahip video izlemelerini içerir.

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

Aşağıdaki [az AMS Account-Filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) komutu, [daha önce tanımlanan](#define-a-filter)filtre izleme seçimleriyle bir hesap filtresi oluşturur. 

Komutu, izleme seçimlerini temsil eden JSON içeren `--tracks` isteğe bağlı bir parametreyi geçirmenize olanak sağlar.  JSON 'ı bir dosyadan yüklemek için @ {File} kullanın. Azure CLı 'yi yerel olarak kullanıyorsanız, tüm dosya yolunu belirtin:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Ayrıca bkz. [Filtreler Için JSON örnekleri](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create-an-account-filter).

## <a name="create-asset-filters"></a>Varlık filtreleri oluşturma

Aşağıdaki [az AMS varlık-Filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) komutu, [daha önce tanımlanan](#define-a-filter)filtre izleme seçimleriyle bir varlık filtresi oluşturur. 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Ayrıca bkz. [Filtreler Için JSON örnekleri](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create-an-asset-filter).

## <a name="associate-filters-with-streaming-locator"></a>Filtreleri akış bulucu ile ilişkilendir

Akış Konumlayıcı için uygulanabilecek varlık veya hesap filtrelerinin bir listesini belirtebilirsiniz. [Dinamik Paketleyici (akış uç noktası)](dynamic-packaging-overview.md) , bu filtre listesini ISTEMCINIZDEKI URL 'de belirttiği değişikliklerle birlikte uygular. Bu bileşim, akış Bulucu üzerinde belirlediğiniz URL + filtrelerdeki filtreleri temel alan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. Filtre uygulamak, ancak URL 'de filtre adlarını göstermek istemiyorsanız bu özelliği kullanmanızı öneririz.

Aşağıdaki CLı kodu, bir akış Bulucu oluşturma ve belirtme `filters`işlemlerinin nasıl yapılacağını gösterir. Bu, varlık filtresi adlarının ve/veya hesap filtresi adlarının boşlukla ayrılmış bir listesini alan isteğe bağlı bir özelliktir.

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

[Video akışı](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
