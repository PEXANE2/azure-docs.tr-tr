---
title: Azure Media Player Korumalı İçerik
description: Azure Media Player şu anda AES-128 bit zarf şifreli içeriği ve yaygın şifrelenmiş içeriği destekler.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 64414d3ec31e8763b7c576af93374bf514141fd4
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726497"
---
# <a name="protected-content"></a>Korumalı içerik #

Azure Media Player şu anda AES-128 bit zarf şifreli içeriği ve yaygın şifrelenmiş içeriği (PlayReady ve Widevine aracılığıyla) veya FairPlay üzerinden şifrelenmiş içeriği destekler. Korumalı içeriği doğru bir şekilde oynatmak için Azure `protectionInfo`Media Player'a . Bu bilgiler kaynak başına var ve `<source>` doğrudan etikete `data-setup`eklenebilir.  Kaynağı dinamik olarak `protectionInfo` ayarlıyorsanız doğrudan parametre olarak da ekleyebilirsiniz.

`protectionInfo`bir JSON nesnesi kabul eder ve şunları içerir:

- `type`: `AES` `PlayReady` veya `Widevine` ya da`FairPlay`
- `certificateUrl`: Bu ev sahipliği fairplay sertifika doğrudan bir bağlantı olmalıdır

- `authenticationToken`: bu, kodlanmamış kimlik doğrulama belirteci eklemek için bir seçenek alanıdır

> [!IMPORTANT]
> **SertifikaUrl** nesnesi yalnızca FairPlay DRM için gereklidir.***
>[!NOTE]
> Varsayılan techOrder, `html5FairPlayHLS` özellikle FairPlay içeriğini destekleyen tarayıcılarda (OSX 8+'da Safari) yerel olarak oynatmak için yeni teknolojiye uyacak şekilde değiştirilmiştir. Oynatmak için FairPlay içeriğiniz varsa **ve** varsayılan techOrder'ı uygulamanızda özel bir siparişle değiştirdiyseniz, bu yeni teknolojiyi techOrder nesnenize eklemeniz gerekir. İçeriğinizin PlayReady üzerinden oynatılması için silverlightSS'a önce eklemenizi öneririz.

## <a name="code-sample"></a>Kod örneği ##

```html
Ex:

    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source
            src="//example/path/to/myVideo.ism/manifest"
            type="application/vnd.ms-sstr+xml"
            data-setup='{"protectionInfo": [{"type": "AES", "authenticationToken": "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=8130520b-c116-45a9-824e-4a0082f3cb3c&Audience=urn%3atest&ExpiresOn=1450207516&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=eV7HDgZ9msp9H9bnEPGN91sBdU7XsZ9OyB6VgFhKBAU%3d"}]}'
        />
    </video>
or

```javascript
    var myPlayer = amp("vid1", /* Options */);
    myPlayer.src([{
        src: "//example/path/to/myVideo.ism/manifest",
        type: "application/vnd.ms-sstr+xml",
        protectionInfo: [{
            type: "PlayReady",
            authenticationToken: "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=d5646e95-63ee-4fbe-ba4e-295c8d9502e0&Audience=urn%3atest&ExpiresOn=1450222961&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=4Jop3kNJdzVI8L5IZLgFtPdImyE%2fHTRil0x%2bEikSdPs%3d"
        }] }, ]
    );
```

veya birden fazla DRM ile

```javascript
    var myPlayer = amp("vid1", /* Options */);
    myPlayer.src([{
        src: "//example/path/to/myVideo.ism/manifest",
        type: "application/vnd.ms-sstr+xml",
        protectionInfo: [{
                type: "PlayReady",
                authenticationToken: "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=d5646e95-63ee-4fbe-ba4e-295c8d9502e0&Audience=urn%3atest&ExpiresOn=1450222961&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=4Jop3kNJdzVI8L5IZLgFtPdImyE%2fHTRil0x%2bEikSdPs%3d"
            },
            {
                type: "Widevine",
                authenticationToken: "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=d5646e95-63ee-4fbe-ba4e-295c8d9502e0&Audience=urn%3atest&ExpiresOn=1450222961&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=4Jop3kNJdzVI8L5IZLgFtPdImyE%2fHTRil0x%2bEikSdPs%3d"
            },
            {
                   type: "FairPlay",
                  certificateUrl: "//example/path/to/myFairplay.der",
                   authenticationToken: "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1cm46bWljcm9zb2Z0OmF6dXJlOm1lZGlhc2VydmljZXM6Y29udGVudGtleWlkZW50aWZpZXIiOiIyMTI0M2Q2OC00Yjc4LTRlNzUtYTU5MS1jZWMzMDI0NDNhYWMiLCJpc3MiOiJodHRwOi8vY29udG9zbyIsImF1ZCI6InVybjp0ZXN0IiwiZXhwIjoxNDc0NTkyNDYzLCJuYmYiOjE0NzQ1ODg1NjN9.mE7UxgNhkieMMqtM_IiYQj-FK1KKIzB6lAptw4Mi67A"
        }] } ]
    );
```

> [!NOTE]
> Tüm tarayıcılar/platformlar korumalı içeriği oynatma yeteneğine sahip değildir. Desteklenenler hakkında daha fazla bilgi için [Oynatma Teknolojisi](azure-media-player-playback-technology.md) bölümüne bakın.
> [!IMPORTANT]
> Oynatıcıya geçirilen belirteç güvenli içerik içindir ve yalnızca kimlik doğrulaması yapılan kullanıcılar için kullanılır. Uygulamanın SSL veya başka bir güvenlik önlemi biçimi kullandığı varsayılır. Ayrıca, son kullanıcı belirteci kötüye değil güvenilir olarak kabul edilir; durum bu değilse, lütfen güvenlik uzmanlarınızı dahil edin.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)