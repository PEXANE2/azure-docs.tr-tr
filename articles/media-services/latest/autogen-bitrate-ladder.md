---
title: Media Services-Azure 'da standart kodlayıcıyla videoları kodlayın | Microsoft Docs
description: Bu konu başlığı Media Services altında Standart kodlayıcının, giriş ve bit hızını temel alarak otomatik olarak oluşturulan bir bit hızı el ile bir giriş videosunu kodlamak için nasıl kullanılacağı gösterilmektedir.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "60733326"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Otomatik olarak üretilen bit hızı el ile kodla

## <a name="overview"></a>Genel Bakış

Bu makalede, giriş ve bit hızını temel alarak bir giriş videosunu otomatik olarak oluşturulan bir bit hızı (bit hızı çözme çiftleri) olarak kodlamak için Media Services 'de standart kodlayıcının nasıl kullanılacağı açıklanmaktadır. Bu yerleşik kodlayıcı ayarı veya önayar, hiçbir şekilde giriş çözünürlüğünü ve bit hızını aşmaz. Örneğin, giriş 3 Mbps hızında olursa çıktı en iyi 720p kalır ve 3 Mbps 'den düşük oranlarda başlatılır.

### <a name="encoding-for-streaming"></a>Akış için kodlama

**Dönüşümde** **uyarlamalı akış** ön ayarını kullandığınızda, HLS ve Dash gibi akış protokolleri aracılığıyla teslim için uygun bir çıktı alırsınız. Bu önayarı kullanırken hizmet, kaç video katmanını ve ne kadar bit hızını ve çözümlemeyi belirler. Çıkış içeriği, AAC kodlamalı ses ve H., ile kodlanmış videonun araya eklemeli olduğu MP4 dosyalarını içerir.

Bu önayarın nasıl kullanıldığına ilişkin bir örnek görmek için bkz. [akış a dosyası](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Çıktı

Bu bölümde, Media Services Kodlayıcısı tarafından, **uyarlamalı akış** ön ayarıyla kodlama sonucu olarak üretilen çıkış video katmanlarının üç örneği gösterilmektedir. Her durumda, çıktı 128 Kbps hızında, stereo ses kodlamalı bir salt ses MP4 dosyası içerir.

### <a name="example-1"></a>Örnek 1
"1080" yüksekliğinde ve "29,970" kare hızına sahip kaynak 6 video katmanı üretir:

|Katman|Height|Genişlik|Bit hızı (Kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Örnek 2
"720" yüksekliğinde ve "23,970" kare hızına sahip kaynak 5 video katmanı üretir:

|Katman|Height|Genişlik|Bit hızı (Kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Örnek 3
"360" yüksekliğinde ve "29,970" kare hızına sahip kaynak 3 video katmanı üretir:

|Katman|Height|Genişlik|Bit hızı (Kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
