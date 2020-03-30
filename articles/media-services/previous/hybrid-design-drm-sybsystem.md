---
title: Azure Medya Hizmetleri kullanarak DRM alt sisteminin karma tasarımı | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri'ni kullanarak DRM alt sisteminin karma tasarımını tartışır.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: willzhan
ms.reviewer: juliako
ms.openlocfilehash: d2f4ddfbff791fbfeb2eb006a628c0fdeb4fdce1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975202"
---
# <a name="hybrid-design-of-drm-subsystems"></a>DRM alt sistemlerinin hibrid tasarımı 

Bu konu, Azure Medya Hizmetleri'ni kullanarak DRM alt sisteminin karma tasarımını tartışır.

## <a name="overview"></a>Genel Bakış

Azure Media Services aşağıdaki üç DRM sistemi için destek sağlar:

* PlayReady
* Widevine (Modüler)
* FairPlay

DRM desteği DRM şifreleme (dinamik şifreleme) ve lisans teslimini içerir ve Azure Media Player tarayıcı oynatıcı SDK olarak 3 DRM'nin tümini destekler.

DRM/CENC alt sistem tasarımı ve uygulamasının ayrıntılı bir tedavisi için, [multi-DRM ve Erişim Kontrolü ile CENC](media-services-cenc-with-multidrm-access-control.md)başlıklı belgeye bakın.

Üç DRM sistemi için tam destek sunsak da, bazen müşterilerin karma bir DRM alt sistemi oluşturmak için Azure Medya Hizmetleri'ne ek olarak kendi altyapı/alt sistemlerinin çeşitli bölümlerini kullanmaları gerekir.

Aşağıda müşteriler tarafından sorulan bazı sık sorulan sorular şunlardır:

* "Kendi DRM lisans sunucularımı kullanabilir miyim?" (Bu durumda, müşteriler gömülü iş mantığı ile DRM lisans sunucusu çiftliğine yatırım yaptık).
* "AMS'de içerik barındırmadan Azure Medya Hizmetleri'ndeki yalnızca DRM lisans tesliminizi kullanabilir miyim?"

## <a name="modularity-of-the-ams-drm-platform"></a>AMS DRM platformunun modülerliği

Kapsamlı bir bulut video platformunun parçası olarak Azure Media Services DRM, esneklik ve modülerlik göz önünde bulundurularak bir tasarıma sahiptir. Azure Medya Hizmetleri'ni aşağıdaki tabloda açıklanan aşağıdaki farklı kombinasyonlardan herhangi biriyle kullanabilirsiniz (tabloda kullanılan notasyonun açıklaması aşağıdaki). 

|**& menşeini barındıran içerik**|**İçerik şifreleme**|**DRM lisansı verme**|
|---|---|---|
|AMS|AMS|AMS|
|AMS|AMS|Üçüncü taraf|
|AMS|Üçüncü taraf|AMS|
|AMS|Üçüncü taraf|Üçüncü taraf|
|Üçüncü taraf|Üçüncü taraf|AMS|

### <a name="content-hosting--origin"></a>& menşeini barındıran içerik

* AMS: video varlık AMS barındırılan ve akış AMS akışı uç noktaları (ancak mutlaka dinamik ambalaj) geçer.
* Üçüncü taraf: video AMS dışında bir üçüncü taraf akış platformunda barındırılır ve teslim edilir.

### <a name="content-encryption"></a>İçerik şifreleme

* AMS: içerik şifreleme, AMS dinamik şifreleme ile dinamik/isteğe bağlı olarak gerçekleştirilir.
* Üçüncü taraf: içerik şifreleme, ön işleme iş akışı kullanılarak AMS dışında gerçekleştirilir.

### <a name="drm-license-delivery"></a>DRM lisansı verme

* AMS: DRM lisansı AMS lisans teslim hizmeti tarafından teslim edilir.
* Üçüncü taraf: DRM lisansı AMS dışında bir üçüncü taraf DRM lisans sunucusu tarafından teslim edilir.

## <a name="configure-based-on-your-hybrid-scenario"></a>Karma senaryonuza göre yapılandırma

### <a name="content-key"></a>İçerik anahtarı

Bir içerik anahtarının yapılandırması sayesinde, hem AMS dinamik şifreleme hem de AMS lisans teslim hizmetinin aşağıdaki özniteliklerini denetleyebilirsiniz:

* Dinamik DRM şifrelemesi için kullanılan içerik anahtarı.
* DRM lisans içeriği lisans teslim hizmetleri tarafından teslim edilecek: haklar, içerik anahtarı ve kısıtlamalar.
* İçerik **türü anahtar yetkilendirme ilkesi kısıtlaması:** açık, IP veya belirteç kısıtlaması.
* **İçerik anahtar yetkilendirme ilkesi kısıtlamasının** **belirteç** türü kullanılıyorsa, bir lisans verilmeden önce **içerik anahtarı yetkilendirme ilkesi kısıtlamasının** karşılanması gerekir.

### <a name="asset-delivery-policy"></a>Varlık teslim ilkesi

Bir varlık teslim ilkesinin yapılandırması sayesinde, AMS dinamik paketleyici si ve AMS akış bitiş noktasının dinamik şifrelemesi tarafından kullanılan aşağıdaki öznitelikleri denetleyebilirsiniz:

* Streaming protokolü ve DRM şifreleme kombinasyonu, CENC altında DASH (PlayReady ve Widevine), PlayReady altında düzgün akış, Widevine veya PlayReady altında HLS.
* İlgili DRM'lerin her biri için varsayılan/katıştüre lisans teslim URL'leri.
* DASH MPD veya HLS çalma listesindeki lisans edinme URL'leri (LA_URLs) sırasıyla Widevine ve FairPlay için anahtar kimliği (KID) sorgu dizesini içerir.

## <a name="scenarios-and-samples"></a>Senaryolar ve örnekler

Önceki bölümdeki açıklamalara dayanarak, aşağıdaki beş karma senaryo da ilgili **İçerik anahtarı**-**Varlık teslim ilkesi** yapılandırma kombinasyonlarını kullanır (son sütunda belirtilen örnekler tabloyu izler):

|**& menşeini barındıran içerik**|**DRM şifreleme**|**DRM lisansı verme**|**İçerik anahtarını yapılandırma**|**Varlık teslim ilkesini yapılandırma**|**Örnek**|
|---|---|---|---|---|---|
|AMS|AMS|AMS|Evet|Evet|Örnek 1|
|AMS|AMS|Üçüncü taraf|Evet|Evet|Örnek 2|
|AMS|Üçüncü taraf|AMS|Evet|Hayır|Örnek 3|
|AMS|Üçüncü taraf|Dışında|Hayır|Hayır|Örnek 4|
|Üçüncü taraf|Üçüncü taraf|AMS|Evet|Hayır|    

Örneklerde, PlayReady koruması hem DASH hem de düzgün akış için çalışır. Aşağıdaki video URL'leri düzgün akış URL'leridir. Karşılık gelen DASH URL'lerini almak için "(format=mpd-time-csf)" ekle. Bir tarayıcıda test etmek için [azure medya test oynatıcısını](https://aka.ms/amtest) kullanabilirsiniz. Hangi akış protokolünü kullanacağınızı, hangi teknoloji altında kullanacağınızı yapılandırmanızı sağlar. IE11 ve Windows 10'daki Microsoft Edge, EME aracılığıyla PlayReady'yi destekler. Daha fazla bilgi için [test aracı yla ilgili ayrıntılara](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/)bakın.

### <a name="sample-1"></a>Örnek 1

* Kaynak (taban) URL:https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest 
* PlayReady LA_URL (DASH & pürüzsüz):https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 
* Widevine LA_URL (DASH):https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4 
* FairPlay LA_URL (HLS):https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8 

### <a name="sample-2"></a>Örnek 2

* Kaynak (taban) URL:https://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest 
* PlayReady LA_URL (DASH & pürüzsüz):http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx 

### <a name="sample-3"></a>Örnek 3

* Kaynak URL:https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & pürüzsüz):https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 

### <a name="sample-4"></a>Örnek 4

* Kaynak URL:https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & pürüzsüz):https://willzhan12.cloudapp.net/playready/rightsmanager.asmx 

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="summary"></a>Özet

Özetle, Azure Medya Hizmetleri DRM bileşenleri esnektir, bu konuda açıklandığı gibi içerik anahtarını ve varlık teslim ilkesini düzgün şekilde yapılandırarak bunları karma bir senaryoda kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Medya Hizmetleri öğrenme yollarını görüntüleyin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

