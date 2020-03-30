---
title: Media Services v3 Azure CLI kullanarak özel dönüşümü kodlayın | Microsoft Dokümanlar
description: Bu konu, Azure CLI kullanarak özel bir dönüşümü kodlamak için Azure Media Services v3'ün nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/14/2019
ms.author: juliako
ms.openlocfilehash: 7c1b446ccf04199449f012e738f6a03660735f50
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382962"
---
# <a name="how-to-encode-with-a-custom-transform---azure-cli"></a>Özel bir dönüşümle kodlama - Azure CLI

Azure Medya Hizmetleri ile kodlama yaparken, [Akış dosyaları](stream-files-cli-quickstart.md#create-a-transform-for-adaptive-bitrate-encoding) hızlı başlatıldığında gösterildiği gibi, endüstrinin en iyi uygulamalarını temel alan önerilen yerleşik hazır ayarlardan biriyle hızlı bir şekilde başlayabilirsiniz. Ayrıca, belirli senaryo veya aygıt gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel ön ayarlar oluşturulurken aşağıdaki hususlar geçerlidir:

* AVC içeriğindeki yükseklik ve genişlik için tüm değerler 4'ün katı olmalıdır.
* Azure Media Services v3'te, tüm kodlama bit hızları saniyede bit şeklindedir. Bu birim olarak kilobit /saniye kullanılan bizim v2 API'lar ile ön ayarlar farklıdır. Örneğin, v2'deki bit hızı 128 (kilobit/saniye) olarak belirtilmişse, v3'te 128000 (bit/saniye) olarak ayarlanır.

## <a name="prerequisites"></a>Ön koşullar

[Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)

Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun.

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-custom-preset"></a>Özel bir ön ayar tanımlama

Aşağıdaki örnek, yeni bir Dönüşümün istek gövdesini tanımlar. Bu Dönüşüm kullanıldığında oluşturulmasını istediğimiz bir çıktı kümesi tanımlıyoruz.

Bu örnekte, önce ses kodlaması için bir AacAudio katmanı ve video kodlaması için iki H264Video katmanı ekliyoruz. Video katmanlarında, çıktı dosyası adlarında kullanılabilen etiketler atıyoruz. Ardından, çıktının küçük resimler de içermesini istiyoruz. Aşağıdaki örnekte, giriş videosunun çözünürlüğünün %50'si oranında ve üç zaman damgasında - giriş videosunun uzunluğunun {%25, %50, 75} olarak oluşturulan PNG formatında görüntüleri belirtiriz. Son olarak, çıktı dosyalarının biçimini belirtiriz - biri video + ses için, diğeri de küçük resimler için. Birden fazla H264Layer'a sahip olduğumuzdan, katman başına benzersiz adlar üreten makrolar kullanmalıyız. Biz ya bir `{Label}` `{Bitrate}` veya makro kullanabilirsiniz, örnek eski gösterir.

Bu dönüşümü bir dosyaya kaydedeceğiz. Bu örnekte, dosyayı `customPreset.json`adlandırıyoruz.

```json
{
    "@odata.type": "#Microsoft.Media.StandardEncoderPreset",
    "codecs": [
        {
            "@odata.type": "#Microsoft.Media.AacAudio",
            "channels": 2,
            "samplingRate": 48000,
            "bitrate": 128000,
            "profile": "AacLc"
        },
        {
            "@odata.type": "#Microsoft.Media.H264Video",
            "keyFrameInterval": "PT2S",
            "stretchMode": "AutoSize",
            "sceneChangeDetection": false,
            "complexity": "Balanced",
            "layers": [
                {
                    "width": "1280",
                    "height": "720",
                    "label": "HD",
                    "bitrate": 3400000,
                    "maxBitrate": 3400000,
                    "bFrames": 3,
                    "slices": 0,
                    "adaptiveBFrame": true,
                    "profile": "Auto",
                    "level": "auto",
                    "bufferWindow": "PT5S",
                    "referenceFrames": 3,
                    "entropyMode": "Cabac"
                },
                {
                    "width": "640",
                    "height": "360",
                    "label": "SD",
                    "bitrate": 1000000,
                    "maxBitrate": 1000000,
                    "bFrames": 3,
                    "slices": 0,
                    "adaptiveBFrame": true,
                    "profile": "Auto",
                    "level": "auto",
                    "bufferWindow": "PT5S",
                    "referenceFrames": 3,
                    "entropyMode": "Cabac"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Media.PngImage",
            "stretchMode": "AutoSize",
            "start": "25%",
            "step": "25%",
            "range": "80%",
            "layers": [
                {
                    "width": "50%",
                    "height": "50%"
                }
            ]
        }
    ],
    "formats": [
        {
            "@odata.type": "#Microsoft.Media.Mp4Format",
            "filenamePattern": "Video-{Basename}-{Label}-{Bitrate}{Extension}",
            "outputFiles": []
        },
        {
            "@odata.type": "#Microsoft.Media.PngFormat",
            "filenamePattern": "Thumbnail-{Basename}-{Index}{Extension}"
        }
    ]
}
```

## <a name="create-a-new-transform"></a>Yeni bir dönüşüm oluşturun  

Bu örnekte, daha önce tanımladığımız özel ön aseti temel alan bir **Dönüşüm** oluştururuz. Dönüşüm oluştururken, önce bir dönüşüm olup olmadığını denetlemeniz gerekir. Dönüşüm varsa, yeniden kullanın. Aşağıdaki `show` komut varsa `customTransformName` dönüşümü döndürür:

```azurecli-interactive
az ams transform show -a amsaccount -g amsResourceGroup -n customTransformName
```

Aşağıdaki Azure CLI komutu, özel ön aset'i (daha önce tanımlanmış) temel alan Dönüşüm'ü oluşturur.

```azurecli-interactive
az ams transform create -a amsaccount -g amsResourceGroup -n customTransformName --description "Basic Transform using a custom encoding preset" --preset customPreset.json
```

Medya Hizmetleri'nin Transform'u belirtilen video veya sese uygulayabilmek için bu Dönüşüm'ün altında bir İş göndermeniz gerekir. Dönüşüm altında bir işin nasıl gönderilebildiğini gösteren tam bir örnek için Bkz. [Hızlı Başlangıç: Video dosyalarını akış - Azure CLI](stream-files-cli-quickstart.md).

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](/cli/azure/ams)
