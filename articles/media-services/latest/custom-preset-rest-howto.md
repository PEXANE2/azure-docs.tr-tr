---
title: Media Services v3 REST-Azure kullanarak özel dönüştürmeyi kodla | Microsoft Docs
description: Bu konu, REST kullanarak özel bir dönüşüm kodlamak için Azure Media Services v3 'in nasıl kullanılacağını gösterir.
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
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "65761804"
---
# <a name="how-to-encode-with-a-custom-transform---rest"></a>Özel bir dönüşümle kodlama-REST

Azure Media Services ile kodlarken, [akış dosyaları](stream-files-tutorial-with-rest.md#create-a-transform) öğreticisinde gösterildiği gibi, sektörde en iyi uygulamaları temel alan önerilen yerleşik ön ayarlardan biriyle hızlı bir şekilde çalışmaya başlayın. Ayrıca, belirli bir senaryoyu veya cihaz gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel ön ayarlar oluşturulurken aşağıdaki noktalar geçerlidir:

* AVC içeriğinde yükseklik ve genişlik değerlerinin tümü 4 ' ün katı olmalıdır.
* Azure Media Services v3 'de, tüm kodlama bit fiyatları bit/saniye cinsinden. Bu, birim olarak kilobit/saniye kullanan v2 API 'lerimiz olan ön ayarlardan farklıdır. Örneğin, v2 'deki bit hızı 128 (kilobit/saniye) olarak belirtilmişse, v3 'de 128000 (bit/saniye) olarak ayarlanır.

## <a name="prerequisites"></a>Ön koşullar 

- [Media Services hesabı oluşturun](create-account-cli-how-to.md). <br/>Kaynak grubu adını ve Media Services hesap adını hatırlayacağınızdan emin olun. 
- [Azure Media Services REST API'si çağrıları Için Postman 'ı yapılandırın](media-rest-apis-with-postman.md).<br/>[Azure AD belirtecini al](media-rest-apis-with-postman.md#get-azure-ad-token)konusunun son adımını izlediğinizden emin olun. 

## <a name="define-a-custom-preset"></a>Özel bir önayar tanımlama

Aşağıdaki örnek, yeni bir dönüşümün istek gövdesini tanımlar. Bu dönüşüm kullanıldığında oluşturulmasını istediğimiz bir çıktı kümesi tanımladık. 

Bu örnekte, önce ses kodlaması ve video kodlaması için iki H264Video katmanı için bir AacAudio katmanı ekleyeceğiz. Video katmanlarında, çıkış dosyası adlarında kullanılabilmesi için Etiketler atacağız. Ardından, çıktının küçük resimleri de içermesini istiyoruz. Aşağıdaki örnekte, resimleri, giriş videonun çözümlenme %50 ' de ve üç zaman 75 50 damgalarına (örneğin, giriş videosunun uzunluğuna) göre, PNG biçiminde belirttik. Son olarak, çıkış dosyalarının biçimini, video + ses için, diğeri ise küçük resimler için belirttik. Birden çok H264Layers olduğundan, her katman için benzersiz adlar üreten makrolar kullandık. Bir `{Label}` veya `{Bitrate}` makrosunu kullanabilir, örnek eski ' yi gösterir.

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

## <a name="create-a-new-transform"></a>Yeni dönüşüm oluştur  

Bu örnekte, daha önce tanımladığımız özel hazır ayarı temel alan bir **dönüşüm** oluşturacağız. Bir dönüşüm oluştururken önce, zaten mevcut olup olmadığını denetlemek için [Al](https://docs.microsoft.com/rest/api/media/transforms/get) ' ı kullanmalısınız. Dönüşüm varsa, yeniden kullanın. 

İndirdiğiniz Postman 'ın koleksiyonunda **dönüşümler ve işler**->**Oluştur veya Güncelleştir**' i seçin.

**PUT** http istek yöntemi şuna benzerdir:

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
```

**Gövde** sekmesini seçin ve gövdesi, [daha önce tanımladığınız](#define-a-custom-preset)JSON kodu ile değiştirin. Media Services belirtilen videoya veya sese dönüştürmeyi uygulamak için, o dönüşüm altında bir Iş göndermeniz gerekir.

**Gönder**’i seçin. 

Media Services belirtilen videoya veya sese dönüştürmeyi uygulamak için, o dönüşüm altında bir Iş göndermeniz gerekir. Bir dönüştürme altında iş göndermeyi gösteren tam bir örnek için bkz. [öğretici: Stream video Files-Rest](stream-files-tutorial-with-rest.md).

## <a name="next-steps"></a>Sonraki adımlar

[DIĞER Rest işlemlerine](https://docs.microsoft.com/rest/api/media/) bakın
