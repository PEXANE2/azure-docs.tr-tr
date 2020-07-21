---
title: Media Services v3 .NET-Azure kullanarak özel dönüştürmeyi kodla | Microsoft Docs
description: Bu konuda .NET kullanarak özel bir dönüşüm kodlamak için Azure Media Services v3 'nin nasıl kullanılacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/03/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 2bcd5f0e6229c4130dddb48c1a20de1c711c6fcf
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519890"
---
# <a name="how-to-encode-with-a-custom-transform---net"></a>Özel bir dönüşümle kodlama-.NET

Azure Media Services ile kodlarken, [akış dosyaları](stream-files-tutorial-with-api.md) öğreticisinde gösterildiği gibi, sektörde en iyi uygulamaları temel alan önerilen yerleşik ön ayarlardan biriyle hızlı bir şekilde çalışmaya başlayın. Ayrıca, belirli bir senaryoyu veya cihaz gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel ön ayarlar oluşturulurken aşağıdaki noktalar geçerlidir:

* AVC içeriğinde yükseklik ve genişlik değerlerinin tümü 4 ' ün katı olmalıdır.
* Azure Media Services v3 'de, tüm kodlama bit fiyatları bit/saniye cinsinden. Bu, birim olarak kilobit/saniye kullanan v2 API 'lerimiz olan ön ayarlardan farklıdır. Örneğin, v2 'deki bit hızı 128 (kilobit/saniye) olarak belirtilmişse, v3 'de 128000 (bit/saniye) olarak ayarlanır.

## <a name="prerequisites"></a>Önkoşullar 

[Media Services hesabı oluşturma](create-account-cli-how-to.md)

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak makinenize tam .NET Core örneği içeren bir GitHub deposunu kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
Özel önceden ayarlanmış örnek, [Encodecustomtransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) klasöründe bulunur.

## <a name="create-a-transform-with-a-custom-preset"></a>Özel önayar olan bir dönüşüm oluşturma 

Yeni bir [dönüşüm](https://docs.microsoft.com/rest/api/media/transforms)oluştururken, ne zaman çıkış olarak üretmek istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir [TransformOutput](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#transformoutput) nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Önceden belirlenmiş** , Istenen **Transformoutput**oluşturmak için kullanılacak video ve/veya ses işleme işlemlerinin adım adım talimatlarını açıklar. Aşağıdaki **Transformoutput** , özel codec ve katman çıkış ayarları oluşturur.

Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir. Media Services v3 'de varlık mevcut değilse (ad üzerinde büyük/küçük harfe duyarsız bir denetim), varlıklarda yöntemleri **Al** **null değeri** döndürür.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bu dönüşüm kullanıldığında oluşturulmasını istediğimiz bir çıktı kümesi tanımlar. İlk olarak, video kodlaması için ses kodlaması ve iki H264Video katmanı için bir AacAudio katmanı ekleyeceğiz. Video katmanlarında, çıkış dosyası adlarında kullanılabilmesi için Etiketler atacağız. Ardından, çıktının küçük resimleri de içermesini istiyoruz. Aşağıdaki örnekte, resimleri, giriş videosunun çözümlenmesinden %50 ' de ve üç zaman damgalarına (giriş videonun uzunluğunun %25%, 50%, 75%}) oluşturulan PNG biçiminde belirttik. Son olarak, çıkış dosyalarının biçimini, video + ses için, diğeri ise küçük resimler için belirttik. Birden çok H264Layers olduğundan, her katman için benzersiz adlar üreten makrolar kullandık. Bir `{Label}` veya `{Bitrate}` makrosunu kullanabilir, örnek eski ' yi gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Sonraki adımlar

[Akış dosyaları](stream-files-tutorial-with-api.md) 
