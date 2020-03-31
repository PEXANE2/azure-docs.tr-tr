---
title: Media Services v3 REST - Azure | Microsoft Dokümanlar
description: Bu konu, REST kullanarak özel bir dönüşümü kodlamak için Azure Media Services v3'ün nasıl kullanılacağını gösterir.
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
ms.openlocfilehash: 30e22cb786e5dc2a667fe41ca8edf398cf0b7613
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65761804"
---
# <a name="how-to-encode-with-a-custom-transform---rest"></a>Özel bir dönüşümle kodlama nasıl kodlanır - REST

Azure Medya Hizmetleri ile kodlama yaparken, [Akış dosyaları](stream-files-tutorial-with-rest.md#create-a-transform) öğreticisinde gösterildiği gibi, endüstrinin en iyi uygulamalarını temel alan önerilen yerleşik hazır ayarlardan biriyle hızlı bir şekilde işe bilirsiniz. Ayrıca, belirli senaryo veya aygıt gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel ön ayarlar oluşturulurken aşağıdaki hususlar geçerlidir:

* AVC içeriğindeki yükseklik ve genişlik için tüm değerler 4'ün katı olmalıdır.
* Azure Media Services v3'te, tüm kodlama bit hızları saniyede bit şeklindedir. Bu birim olarak kilobit /saniye kullanılan bizim v2 API'lar ile ön ayarlar farklıdır. Örneğin, v2'deki bit hızı 128 (kilobit/saniye) olarak belirtilmişse, v3'te 128000 (bit/saniye) olarak ayarlanır.

## <a name="prerequisites"></a>Ön koşullar 

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) <br/>Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun. 
- [Azure Medya Hizmetleri REST API çağrıları için Postacı yapılandırın.](media-rest-apis-with-postman.md)<br/>[Azure AD Belirteci'ni alın'](media-rest-apis-with-postman.md#get-azure-ad-token)ın son adımını takip edin. 

## <a name="define-a-custom-preset"></a>Özel bir ön ayar tanımlama

Aşağıdaki örnek, yeni bir Dönüşümün istek gövdesini tanımlar. Bu Dönüşüm kullanıldığında oluşturulmasını istediğimiz bir çıktı kümesi tanımlıyoruz. 

Bu örnekte, önce ses kodlaması için bir AacAudio katmanı ve video kodlaması için iki H264Video katmanı ekliyoruz. Video katmanlarında, çıktı dosyası adlarında kullanılabilen etiketler atıyoruz. Ardından, çıktının küçük resimler de içermesini istiyoruz. Aşağıdaki örnekte, giriş videosunun çözünürlüğünün %50'si oranında ve üç zaman damgasında - giriş videosunun uzunluğunun {%25, %50, 75} olarak oluşturulan PNG formatında görüntüleri belirtiriz. Son olarak, çıktı dosyalarının biçimini belirtiriz - biri video + ses için, diğeri de küçük resimler için. Birden fazla H264Layer'a sahip olduğumuzdan, katman başına benzersiz adlar üreten makrolar kullanmalıyız. Biz ya bir `{Label}` `{Bitrate}` veya makro kullanabilirsiniz, örnek eski gösterir.

```json
{
    "properties": {
        "description": "Basic Transform using a custom encoding preset",
        "outputs": [
            {
                "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                "preset": {
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
            }
        ]
    }
}

```

## <a name="create-a-new-transform"></a>Yeni bir dönüşüm oluşturun  

Bu örnekte, daha önce tanımladığımız özel ön aseti temel alan bir **Dönüşüm** oluştururuz. Dönüşüm oluştururken, önce [Get'in](https://docs.microsoft.com/rest/api/media/transforms/get) zaten var olup olmadığını kontrol etmek için al'ı kullanmalısınız. Dönüşüm varsa, yeniden kullanın. 

İndirdiğiniz Postacı koleksiyonunda **Dönüşümler ve İşler**->**Dönüşüm Oluştur veya Güncelleştir'i**seçin.

**PUT** HTTP istek yöntemi aşağıdakilere benzer:

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
```

**Gövde** sekmesini seçin ve gövdeyi daha [önce tanımladığınız](#define-a-custom-preset)json koduyla değiştirin. Medya Hizmetleri'nin Transform'u belirtilen video veya sese uygulayabilmek için bu Dönüşüm'ün altında bir İş göndermeniz gerekir.

**Gönder**’i seçin. 

Medya Hizmetleri'nin Transform'u belirtilen video veya sese uygulayabilmek için bu Dönüşüm'ün altında bir İş göndermeniz gerekir. Dönüşüm altında bir işin nasıl gönderilebildiğini gösteren tam bir örnek için [Bkz. Öğretici: Video dosyalarını akış - REST](stream-files-tutorial-with-rest.md).

## <a name="next-steps"></a>Sonraki adımlar

[Diğer REST işlemlerine](https://docs.microsoft.com/rest/api/media/) bakın
