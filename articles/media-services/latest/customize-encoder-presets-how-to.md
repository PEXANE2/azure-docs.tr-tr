---
title: Media Services v3 .NET - Azure | Microsoft Dokümanlar
description: Bu konu, .NET kullanarak özel bir dönüşümü kodlamak için Azure Media Services v3'ün nasıl kullanılacağını gösterir.
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
ms.openlocfilehash: ebe701032e6416b3e007a28db62f5a8235bb1bb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80068039"
---
# <a name="how-to-encode-with-a-custom-transform---net"></a>Özel bir dönüşümle kodlama - .NET

Azure Medya Hizmetleri ile kodlama yaparken, [Akış dosyaları](stream-files-tutorial-with-api.md) öğreticisinde gösterildiği gibi endüstrinin en iyi uygulamalarını temel alan önerilen yerleşik hazır ayarlardan biriyle hızlı bir şekilde işe bilirsiniz. Ayrıca, belirli senaryo veya aygıt gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel ön ayarlar oluşturulurken aşağıdaki hususlar geçerlidir:

* AVC içeriğindeki yükseklik ve genişlik için tüm değerler 4'ün katı olmalıdır.
* Azure Media Services v3'te, tüm kodlama bit hızları saniyede bit şeklindedir. Bu birim olarak kilobit /saniye kullanılan bizim v2 API'lar ile ön ayarlar farklıdır. Örneğin, v2'deki bit hızı 128 (kilobit/saniye) olarak belirtilmişse, v3'te 128000 (bit/saniye) olarak ayarlanır.

## <a name="prerequisites"></a>Ön koşullar 

[Media Services hesabı oluşturma](create-account-cli-how-to.md)

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak makinenize tam .NET Core örneğini içeren bir GitHub deposunu klonla:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
Özel önceden ayarlanmış örnek [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) klasöründe yer alır.

## <a name="create-a-transform-with-a-custom-preset"></a>Özel bir ön ayarla dönüştürme oluşturma 

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms)oluştururken, çıktı olarak ne üretmek istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir [TransformOutput](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#transformoutput) nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Önceden Ayar,** istenen **TransformOutput'ü**oluşturmak için kullanılacak video ve/veya ses işleme işlemlerinin adım adım yönergelerini açıklar. Aşağıdaki **TransformOutput** özel codec ve katman çıktı ayarları oluşturur.

Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir. Medya Hizmetleri v3'te, varlık yoksa varlıklardaki yöntemleri **null** return (ad üzerinde büyük/küçük harf duyarsız denetim) **alın.**

### <a name="example"></a>Örnek

Aşağıdaki örnek, bu Dönüşüm kullanıldığında oluşturulmak istediğimiz bir çıktı kümesini tanımlar. Biz ilk ses kodlama ve video kodlama için iki H264Video katmanları için bir AacAudio katmanı ekleyin. Video katmanlarında, çıktı dosyası adlarında kullanılabilen etiketler atıyoruz. Ardından, çıktının küçük resimler de içermesini istiyoruz. Aşağıdaki örnekte, giriş videosunun çözünürlüğünün %50'si oranında ve üç zaman damgasında - giriş videosunun uzunluğunun {%25, %50, 75} olarak oluşturulan PNG formatında görüntüleri belirtiriz. Son olarak, çıktı dosyalarının biçimini belirtiriz - biri video + ses için, diğeri de küçük resimler için. Birden fazla H264Layer'a sahip olduğumuzdan, katman başına benzersiz adlar üreten makrolar kullanmalıyız. Biz ya bir `{Label}` `{Bitrate}` veya makro kullanabilirsiniz, örnek eski gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Sonraki adımlar

[Akış dosyaları](stream-files-tutorial-with-api.md) 
