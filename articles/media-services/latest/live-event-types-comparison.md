---
title: Azure Medya Hizmetleri LiveEvent türleri | Microsoft Dokümanlar
description: Azure Medya Hizmetleri'nde, canlı bir olay *geçiş* veya *canlı kodlama*olarak ayarlanabilir. Bu makalede, Canlı Olay türlerini karşılaştıran ayrıntılı bir tablo gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: a28d4d96f643c12eeb6aa542db2c6af06f4fd954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78244636"
---
# <a name="live-event-types-comparison"></a>Canlı Olay türleri karşılaştırması

Azure Medya Hizmetleri'nde, [Canlı Etkinlik](https://docs.microsoft.com/rest/api/media/liveevents) geçiş *(şirket* içi canlı kodlayıcı birden çok bit hızı akışı gönderir) veya *canlı kodlama* (şirket içi canlı kodlayıcı tek bir bit hızı akışı gönderir) olarak ayarlanabilir. 

Bu makale, canlı olay türlerinin özelliklerini karşılaştırır.

## <a name="types-comparison"></a>Tür karşılaştırması 

Aşağıdaki tablo, Canlı Etkinlik türlerinin özelliklerini karşılaştırın. Türleri [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype)kullanarak oluşturma sırasında ayarlanır:

* **LiveEventEncodingType.None** - Şirket içi canlı kodlayıcı birden çok bithızı akışı gönderir. Yutulan akışlar, başka bir işleme gerek kalmadan Live Event'den geçer. Ayrıca bir pass-through Live Event olarak anılacaktır.
* **LiveEventEncodingType.Standard** - Şirket içi canlı kodlayıcı, Live Event'e tek bir bithızı akışı gönderir ve Medya Hizmetleri birden çok bithızı akışı oluşturur. Katkı akışı 720p veya daha yüksek çözünürlükteyse, **Default720p** önceden ayarlanmış 6 çözünürlük/bitat çifti kümesini kodlar (ayrıntılar makalenin ilerleyen günlerini izler).
* **LiveEventEncodingType.Premium1080p** - Şirket içi canlı kodlayıcı, Live Event'e tek bir bithızı akışı gönderir ve Medya Hizmetleri birden çok bithızı akışı oluşturur. Default1080p ön kümesi çözünürlük/bithızı çiftleri çıkış kümesini belirtir (ayrıntılar makalenin ilerleyen saatlerinde izleyin). 

| Özellik | Pass-through Canlı Etkinlik | Standart veya Premium1080p Canlı Etkinlik |
| --- | --- | --- |
| Tek bit hızı girişi bulutta birden çok bit hızına kodlanır |Hayır |Evet |
| Katkı akışı için maksimum video çözünürlüğü |4K (4096x2160 60 kare/sn) |1080p (1920x1088 30 kare/sn)|
| Katkı beslemesinde önerilen maksimum katmanlar|12'ye kadar|Bir ses|
| Çıktıdaki maksimum katmanlar| Girişle aynı|En fazla 6 (aşağıdaki Sistem Hazır Ayarları'na bakın)|
| Katkı beslemesinin maksimum toplam bant genişliği|60 Mbps|Yok|
| Katkıdaki tek bir katman için maksimum bit hızı |20 Mbps|20 Mbps|
| Birden çok dil ses parçaları için destek|Evet|Hayır|
| Desteklenen giriş video codec'leri |H.264/AVC ve H.265/HEVC|H.264/AVC|
| Desteklenen çıkış video codec'leri|Girişle aynı|H.264/AVC|
| Desteklenen video biti derinliği, giriş ve çıktı|HDR 10/HLG dahil 10 bit'e kadar|8 bit|
| Desteklenen giriş ses codec'leri|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Desteklenen çıkış ses codec'leri|Girişle aynı|AAC-LC|
| Çıktı videosunun maksimum video çözünürlüğü|Girişle aynı|Standart - 720p, Premium1080p - 1080p|
| Giriş videosunun maksimum kare hızı|60 kare/saniye|Standart veya Premium1080p - 30 kare/saniye|
| Giriş protokolleri|RTMP, parçalanmış-MP4 (Düzgün Akış)|RTMP, parçalanmış-MP4 (Düzgün Akış)|
| Fiyat|Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/media-services/) bakın ve "Canlı Video" sekmesine tıklayın|Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/media-services/) bakın ve "Canlı Video" sekmesine tıklayın|
| Maksimum çalışma süresi| 24 saat x 365 gün, canlı doğrusal | 24 saat x 365 gün, canlı doğrusal (önizleme)|
| Gömülü CEA 608/708 altyazı verilerinden geçebilme|Evet|Evet|
| Canlı Transkripsiyon'u açma becerisi|Evet|Evet|
| Kayrak ekleme desteği|Hayır|Hayır|
| API üzerinden reklam sinyali desteği| Hayır|Hayır|
| SCTE-35 bant içi mesajlarla reklam sinyali desteği|Evet|Evet|
| Katkı beslemesindeki kısa duraklardan kurtulma becerisi|Evet|Kısmi|
| Tek tip olmayan giriş GOP'ları için destek|Evet|Hayır – giriş gop süresi sabit olmalıdır|
| Değişken kare hızı girişi desteği|Evet|Hayır – giriş sabit kare hızı olmalıdır. Küçük varyasyonlar, örneğin, yüksek hareket sahneleri sırasında tolere edilir. Ancak katkı akışı kare hızını düşüremez (örneğin, 15 kare/saniye).|
| Giriş akışı kaybolduğunda Canlı Olay'ın otomatik kapatması|Hayır|12 saat sonra, çalışan LiveOutput yoksa|

## <a name="system-presets"></a>Sistem ön ayarları

Canlı kodlayıcıdan çıktıda bulunan çözünürlükler ve bitrates [önceden ayarlanmış Name](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencoding)ile belirlenir. **Standart** canlı kodlayıcı (LiveEventEncodingType.Standard) kullanıyorsanız, *Default720p* ön kümesi aşağıda açıklanan 6 çözünürlük/bit hızı çifti kümesini belirtir. Aksi takdirde, **Premium1080p** canlı kodlayıcı (LiveEventEncodingType.Premium1080p) kullanıyorsanız, *Default1080p* ön kümesi çıkış çözüm kümesi/bithızı çiftleri belirtir.

> [!NOTE]
> Varsayılan 1080p ön ayarını, Standart canlı kodlama için ayarlanmışsa, Canlı Etkinlik'e uygulayamazsınız - bir hata alırsınız. Varsayılan720p önceden ayarlanmış ı premium1080p canlı kodlayıcıya uygulamayı denerseniz de bir hata alırsınız.

### <a name="output-video-streams-for-default720p"></a>Varsayılan720p için Çıktı Video Akışları

Katkı beslemesi 720p veya daha yüksek çözünürlükteyse, **Varsayılan720p** önceden ayarlanmış olan özet akışı aşağıdaki 6 katmana kodlar. Aşağıdaki tabloda, Bitrate kbps, MaxFPS maksimum izin verilen kare hızı (kare /saniye), Profil kullanılan H.264 Profili temsil eder.

| Bitrate | Genişlik | Height | MaxFPS | Profil |
| --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Yüksek |
| 2200 |960 |540 |30 |Yüksek |
| 1350 |704 |396 |30 |Yüksek |
| 850 |512 |288 |30 |Yüksek |
| 550 |384 |216 |30 |Yüksek |
| 200 |340 |192 |30 |Yüksek |

> [!NOTE]
> Canlı kodlama ön ayarını özelleştirmeniz gerekiyorsa, lütfen Azure Portalı üzerinden bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 720p çözünürlükte yalnızca bir katman ve toplamda en fazla 6 katman bulunduğundan emin olun. Ayrıca, Standart canlı kodlayıcı için önceden ayarlanmış istediğinizi belirtin.
> Bitrates ve çözünürlüklerin belirli değerleri zaman içinde ayarlanabilir

### <a name="output-video-streams-for-default1080p"></a>Varsayılan 1080p için Çıkış Video Akışları

Katkı akışı 1080p çözünürlüğe sahipse, **Varsayılan 1080p** önceden ayarlanmış olan özet akışı aşağıdaki 6 katmana kodlar.

| Bitrate | Genişlik | Height | MaxFPS | Profil |
| --- | --- | --- | --- | --- |
| 5500 |1920 |1080 |30 |Yüksek |
| 3000 |1280 |720 |30 |Yüksek |
| 1600 |960 |540 |30 |Yüksek |
| 800 |640 |360 |30 |Yüksek |
| 400 |480 |270 |30 |Yüksek |
| 200 |320 |180 |30 |Yüksek |

> [!NOTE]
> Canlı kodlama ön ayarını özelleştirmeniz gerekiyorsa, lütfen Azure Portalı üzerinden bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 1080p'de ve en fazla 6 katmanda yalnızca bir katman olduğunu doğrulayın. Ayrıca Premium1080p canlı kodlayıcı için önceden ayarlanmış talep ettiğinizi belirtin.
> Bitrates ve çözünürlüklerin belirli değerleri zaman içinde ayarlanabilir.

### <a name="output-audio-stream-for-default720p-and-default1080p"></a>Varsayılan720p ve Varsayılan1080p için Çıkış Ses Akışı

Hem *Varsayılan720p* hem de *Default1080p* ön ayarları için ses 128 kbps stereo AAC-LC'ye kodlanır. Örnekleme hızı, katkı beslemesindeki ses parçasının kini izler.

## <a name="implicit-properties-of-the-live-encoder"></a>Canlı kodlayıcının örtük özellikleri

Önceki bölümde, katman sayısı, çözünürlükler ve bitrates gibi önceden ayarlanmış aracılığıyla açıkça kontrol edilebilen canlı kodlayıcının özellikleri açıklanmaktadır. Bu bölümde örtük özellikleri açıklar.

### <a name="group-of-pictures-gop-duration"></a>Resim grubu (GOP) süresi

Canlı kodlayıcı katkı beslemesinin [GOP](https://en.wikipedia.org/wiki/Group_of_pictures) yapısını izler - bu da çıkış katmanlarının aynı GOP süresine sahip olacağı anlamına gelir. Bu nedenle, gop süresini sabitlemiş bir katkı beslemesi (genellikle 2 saniye) üretmek için şirket içi kodlayıcıyı yapılandırmanız önerilir. Bu, hizmetten giden HLS ve MPEG DASH akışlarının da GOP sürelerini sabitlemesini sağlayacaktır. GOP sürelerinde küçük varyasyonlar çoğu cihaz tarafından tolere edilmesi muhtemeldir.

### <a name="frame-rate"></a>Kare hızı

Canlı kodlayıcı aynı zamanda katkı akışındaki tek tek video karelerinin sürelerini de izler - bu da çıktı katmanlarının aynı sürelere sahip çerçevelere sahip olacağı anlamına gelir. Bu nedenle, şirket içi kodlayıcıyı sabit kare hızına (en fazla 30 kare/saniye) bir katkı beslemesi üretecek şekilde yapılandırmanız önerilir. Bu, hizmetten giden HLS ve MPEG DASH akışlarının sabit kare hızlarına sahip olmasını sağlar. Kare hızlarında küçük varyasyonlar çoğu aygıt tarafından tolere edilebilir, ancak canlı kodlayıcının doğru oynayacak bir çıktı üreteceğinin garantisi yoktur. Şirket içi canlı kodlayıcınız çerçeveleri düşürmemelidir (örn. düşük pil koşullarında) veya herhangi bir şekilde kare hızı değişen.

### <a name="resolution-of-contribution-feed-and-output-layers"></a>Katkı besleme ve çıkış katmanlarının çözünürlüğü

Canlı kodlayıcı, katkı beslemesinin dönüştürülmesini önlemek için yapılandırılır. Sonuç olarak, çıktı katmanlarının maksimum çözünürlüğü katkı beslemesini aşmaz.

Örneğin, Varsayılan 1080p canlı kodlama için yapılandırılmış bir Live Event'e 720p'de bir katkı akışı gönderirseniz, çıktı 3Mbps'de 720p ile başlayarak 200 kbps'de 1080p'ye inen yalnızca 5 katmana sahip olur. Veya Standart canlı kodlama için yapılandırılmış bir Live Event'e 360p'de katkı akışı gönderirseniz, çıktı 3 katman (288p, 216p ve 192p çözünürlükte) içerir. Dejenere durumda, örneğin, Standart canlı kodlayıcıya 160x90 piksel katkı beslemesi gönderirseniz, çıktı katkı beslemesi ile aynı bithızında 160x90 çözünürlükte bir katman içerir.

### <a name="bitrate-of-contribution-feed-and-output-layers"></a>Katkı besleme ve çıktı katmanlarının bithızı

Canlı kodlayıcı, katkı beslemesinin bit hızından bağımsız olarak önceden ayarlanmış bithızı ayarlarını onurlandıracak şekilde yapılandırılır. Sonuç olarak, çıktı katmanlarının bithızı katkı beslemesini aşabilir. Örneğin, 1 Mbps çözünürlükte 720p çözünürlükte bir katkı akışı gönderirseniz, çıkış katmanları yukarıdaki [tablodaki](live-event-types-comparison.md#output-video-streams-for-default720p) yle aynı kalır.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akışa genel bakış](live-streaming-overview.md)
