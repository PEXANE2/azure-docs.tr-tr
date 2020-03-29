---
title: Medya Hizmetlerinde Standart Kodlayıcı ile videoları kodlayın - Azure | Microsoft Dokümanlar
description: Bu konu, giriş çözünürlüğü ve bitrate'yi temel alan otomatik olarak oluşturulan bitrate merdiveniyle bir giriş videosunu kodlamak için Medya Hizmetleri'ndeki Standart Kodlayıcı'nın nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: c25c32f35adc1c017f0f4c012c82bd7e0af8d452
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60733326"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Otomatik olarak oluşturulan bithızı merdiveni ile kodlama

## <a name="overview"></a>Genel Bakış

Bu makalede, giriş çözünürlüğü ve bitrate dayalı otomatik olarak oluşturulan bitrate merdiven (bitrate çözünürlüğü çiftleri) içine bir giriş video kodlamak için Medya Hizmetleri Standart Encoder nasıl kullanılacağı açıklanır. Bu yerleşik kodlayıcı ayarı veya önceden ayarlanmış, giriş çözünürlüğünü ve bitrate'yi asla aşmaz. Örneğin, giriş 3 Mbps'de 720p ise, çıkış en iyi ihtimalle 720p kalır ve 3 Mbps'den düşük hızlarda başlar.

### <a name="encoding-for-streaming"></a>Akış için kodlama

**Transform'da** **AdaptiveStreaming** hazır kümesini kullandığınızda, HLS ve DASH gibi akış protokolleri aracılığıyla teslim için uygun bir çıktı elde elabilirsiniz. Bu önceden ayarlanmışı kullanırken, hizmet kaç video katmanı oluşturacağı ve hangi bit hızında ve çözünürlükte oluşturacağı konusunda akıllıca belirler. Çıktı içeriği, AAC kodlu ses ve H.264 kodlu videonun ara ayrılmadığı MP4 dosyalarını içerir.

Bu ön akışın nasıl kullanıldığına bir örnek görmek için [bir dosyayı akış'a](stream-files-dotnet-quickstart.md)bakın.

## <a name="output"></a>Çıktı

Bu bölümde, **AdaptiveStreaming** ön akışına kodlanması sonucunda Medya Hizmetleri kodlayıcısı tarafından üretilen çıktı video katmanlarının üç örneği gösterilmektedir. Her durumda, çıkış 128 kbps kodlanmış stereo ses ile bir ses sadece MP4 dosyası içerir.

### <a name="example-1"></a>Örnek 1
Yükseklik "1080" ve kare hızı "29.970" ile kaynak 6 video katmanları üretir:

|Katman|Height|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Örnek 2
Yüksekliği "720" ve kare hızı "23.970" olan kaynak 5 video katmanı üretir:

|Katman|Height|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Örnek 3
Yükseklik "360" ve kare hızı "29.970" ile kaynak 3 video katmanları üretir:

|Katman|Height|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
