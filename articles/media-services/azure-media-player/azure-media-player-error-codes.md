---
title: Azure Media Player hata kodları
description: Azure Media Player için bir hata kodu başvurusu.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: error-reference
ms.date: 04/20/2020
ms.openlocfilehash: 13d804ec39c3d7753d4ee04962a88d4451fb04d9
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727261"
---
# <a name="error-codes"></a>Hata kodları #

Oynatma başlatılamadığında veya durdurulduğunda, bir hata olayı `error()` başlatılacaktır ve işlev, uygulama geliştiricisinin daha fazla ayrıntı elde edilmesine yardımcı olacak bir kod ve isteğe bağlı bir mesaj döndürür. `error().message`kullanıcıya görüntülenen ileti değildir.  Kullanıcıya görüntülenen ileti, aşağıdaki `error().code` tabloya bakın, 27-20 bitlerini temel almaktadır.

```javascript

    var myPlayer = amp('vid1');
    myPlayer.addEventListener('error', function() {
        var errorDetails = myPlayer.error();
        var code = errorDetails.code;
        var message = errorDetails.message;
    }
```

## <a name="error-codes-bits-31-28-4-bits"></a>Hata kodları, bit [31-28] (4 bit) ##

Hata alanını açıklayın.

- 0 - Bilinmeyen
- 1 - AMP
- 2 - AzureHtml5JS
- 3 - Yanıp söner
- 4 - SilverlightSS
- 5 - Html5
- 6 - Html5FairPlayHLS

## <a name="error-codes-bits-27-0-28-bits"></a>Hata kodları, bit [27-0] (28 bit) ##

Hatanın ayrıntılarını açıklayın, bit 27-20 yüksek düzeyde sağlamak, bit 19-0 varsa daha fazla ayrıntı sağlar.


| amp.errorCode. [isim] | Kodlar, Bitler [27-0] (28 bit) | Açıklama |
|---|---:|---|
| **MEDIA_ERR_ABORTED hataları aralığı (0x0100000 - 0x01FFFFF)** | | |
| abortedErrUnknown | 0x0100000 | Genel iptal hatası |
| iptalErrNotImplemented | 0x0100001 | İptal hatası, uygulanmadı |
| abortedErrHttpMixedContentBlocked | 0x0100002 | İptal hatası, karışık içerik engellendi - genellikle bir `http://` `https://` sayfadan akış yüklenirken oluşur |
| **MEDIA_ERR_NETWORK hataları başlangıç değeri (0x0200000 - 0x02FFFFF)** | | |
| ağErrBilinmeyen | 0x0200000 | Genel ağ hatası |
| networkErrHttpBadUrlFormat | 0x0200190 | Http 400 hata yanıtı |
| networkErrHttpUserAuthGerekli | 0x0200191 | Http 401 hata yanıtı |
| ağErrHttpUserYasak | 0x0200193 | Http 403 hata yanıtı |
| ağErrHttpUrlNotFound | 0x0200194 | Http 404 hata yanıtı |
| ağErrHttpNotAllowed | 0x0200195 | Http 405 hata yanıtı |
| ağErrHttpGone | 0x020019A | Http 410 hata yanıtı |
| networkErrHttpPreconditionFailed | 0x020019C | Http 412 hata yanıtı |
| networkErrHttpInternalServerFailure | 0x02001F4 | Http 500 hata yanıtı
| ağErrHttpBadGateway | 0x02001F6 | Http 502 hata yanıtı |
| networkErrHttpServiceKullanılamaz | 0x02001F7 | Http 503 hata yanıtı |
| ağErrHttpGatewayTimeout | 0x02001F8 | Http 504 hata yanıtı |
| ağErrTimeout | 0x0200258 | Ağ zaman hatası
| ağErrErr | 0x0200259 | Ağ bağlantısı hatası yanıtı |
| **MEDIA_ERR_DECODE hataları (0x0300000 - 0x03FFFFF)** | | |
| decodeErrUnknown | 0x0300000 | Genel kod çözme hatası |
| **MEDIA_ERR_SRC_NOT_SUPPORTED hataları (0x0400000 - 0x04FFFFF)** | | |
| srcErrBilinmeyen | 0x0400000 | Genel kaynak destekli hata değil |
| srcErrParseSunum | 0x0400001 | Sunu ayrışdırmama hatası |
| srcErrParseSegment | 0x0400002 | Segment ayrıştı hatası |
| srcErrDesteksizSunum | 0x0400003 | Sunu desteklenmiyor |
| srcErrGeçersizSegment | 0x0400004 | Geçersiz kesim |
| **MEDIA_ERR_ENCRYPTED hataları başlangıç değeri (0x0500000 - 0x05FFFFF)** | | |
| encryptErrUnknown | 0x0500000 | Genel şifreli hata | 
| şifreErrDecrypterNotFound | 0x0500001 | Şifre çözücü bulunamadı |
| şifreErrDecrypterInit | 0x0500002 | Şifre çözücü başlatma hatası |
| şifreErrDecrypterNotSupported | 0x0500003 | Şifre çözücü desteklenmiyor |
| encryptErrKeyAcquire | 0x0500004 | Anahtar edinme başarısız oldu |
| şifreErrŞifre çözme | 0x0500005 | Segmentin şifresi çözülemedi |
| encryptErrLicenseAcquire | 0x0500006 | Lisans edinme başarısız oldu |
| **SRC_PLAYER_MISMATCH hataları başlangıç değeri (0x0600000 - 0x06FFFFF)** | | |
| srcPlayerMismatchUnknown | 0x0600000 | Genel hiçbir eşleşen teknik oyuncu kaynak oynamak için |
| srcPlayerMismatchFlashNotInstalled | 0x0600001 |Flash eklentisi yüklü değil, yüklenirse kaynak çalabilir. *VEYA* Flash 30 yüklenir ve AES içeriğini oynatın.  Bu durumda, lütfen farklı bir tarayıcı deneyin. Flash 30 bugün 7 Haziran itibariyle desteklenmedi. Daha fazla ayrıntı için [bilinen sorunlara](azure-media-player-known-issues.md) bakın. Not: 0x00600003 ise, techOrder'da belirtildiği takdirde hem Flash hem de Silverlight yüklenmez.|
| srcPlayerMismatchSilverlightNotInstalled | 0x0600002 | Silverlight eklentisi yüklü değil, yüklenirse kaynak çalabilir. Not: 0x00600003 ise, techOrder'da belirtildiği takdirde hem Flash hem de Silverlight yüklenmez. |
| | 0x00600003 | TechOrder'da belirtildiği takdirde Hem Flash hem de Silverlight yüklü değildir. |
| **Bilinmeyen hatalar (0x0FF00000)** | | |
| errBilinmeyen | 0xFF00000 | Bilinmeyen hatalar |

## <a name="user-error-messages"></a>Kullanıcı hatası iletileri ##

Görüntülenen kullanıcı iletisi hata kodunun 27-20 bitlerine dayanır.

- MEDIA_ERR_ABORTED (1) -"Video oynatmayı iptal ettin"
- MEDIA_ERR_NETWORK (2) - "Bir ağ hatası video indirmenin yarı yolda başarısız olmasıyla sonuçlandı."
- MEDIA_ERR_DECODE (3) - "Video oynatma, bir yolsuzluk sorunu nedeniyle veya kullanılan video tarayıcınızın desteklemediği özellikler nedeniyle iptal edildi."
- MEDIA_ERR_SRC_NOT_SUPPORTED (4)-"Sunucu veya ağ başarısız olduğu için veya biçim desteklenmediği için video yüklenemedi."
- MEDIA_ERR_ENCRYPTED (5) - "Video şifrelenmiş ve şifresini çözecek tuşlarımız yok."
- SRC_PLAYER_MISMATCH (6) - "Bu video için uyumlu bir kaynak bulunamadı."
- MEDIA_ERR_UNKNOWN (0xFF) - "Bilinmeyen bir hata oluştu."

## <a name="examples"></a>Örnekler ##

### <a name="0x10600001"></a>0x10600001 ##

"Bu video için uyumlu bir kaynak bulunamadı." son kullanıcıya görüntülenir.

İstenen kaynakları oynatabilen bir teknik oyuncu yoktur, ancak Flash eklentisi yüklenirse, bir kaynağın çalınması olasıdır.  

### <a name="0x20200194"></a>0x20200194 ###

"Bir ağ hatası video indirmenin yarı yolda başarısız olmasıyla sonuçlandı." son kullanıcıya görüntülenir.

AzureHtml5JS http 404 yanıtından oynatmayı başaramadı.

### <a name="categorizing-errors"></a>Hataları kategorilere ayırma ###

```javascript
    if(myPlayer.error().code & amp.errorCode.abortedErrStart) {
        // MEDIA_ERR_ABORTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.networkErrStart) {
        // MEDIA_ERR_NETWORK errors
    }
    else if(myPlayer.error().code & amp.errorCode.decodeErrStart) {
        // MEDIA_ERR_DECODE errors
    }
    else if(myPlayer.error().code & amp.errorCode.srcErrStart) {
        // MEDIA_ERR_SRC_NOT_SUPPORTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.encryptErrStart) {
        // MEDIA_ERR_ENCRYPTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.srcPlayerMismatchStart) {
        // SRC_PLAYER_MISMATCH errors
    }
    else {
        // unknown errors
    }
```

### <a name="catching-a-specific-error"></a>Belirli bir hatayı yakalama ###

Aşağıdaki kod sadece 404 hata yakalar:

```javascript
    if(myPlayer.error().code & amp.errorCode.networkErrHttpUrlNotFound) {
        // all http 404 errors
    }
```

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)