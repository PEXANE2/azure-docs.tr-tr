---
title: Azure Media Player Oynatma Teknolojisi
description: Videoyu veya sesi oynatmak için kullanılan oynatma teknolojisi hakkında daha fazla bilgi edinin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 85eaa04836774b838da67e073017f4af3d2fe179
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726490"
---
# <a name="playback-technology-tech"></a>Oynatma teknolojisi ("teknoloji") #

Oynatma Teknolojisi, videoyu veya sesi oynatmak için kullanılan belirli tarayıcı veya eklenti teknolojisini ifade eder.

- **azureHtml5JS**: Azure Medya Hizmetleri'nden AES-128 bit zarf şifreli içerik veya DRM ortak şifreli içerik (tarayıcı desteklediğinde PlayReady ve Widevine üzerinden) desteği yle DASH içeriğinin eklentisiz tabanlı oynatımı için video öğesi ile birlikte MSE ve EME standartlarını kullanır
- **flashSS**: Azure Medya Hizmetleri'nden AES-128 bit zarf şifre çözme desteği ile Düzgün içeriği oynatmak için flash player teknolojisini kullanır - 11.4 veya üzeri Flash sürümü gerektirir
- **html5FairPlayHLS**: video öğesi ile HLS üzerinden tarayıcı tabanlı oynatma teknolojisisafari özel kullanır. Bu teknoloji, Azure Medya Hizmetleri'nden FairPlay korumalı içeriği oynatmayı gerektirir ve 10/19/16 itibariyle techOrder'a eklendi
- **silverlightSS**: Azure Media Services'in PlayReady korumalı içeriği için destek le Pürüzsüz içeriği oynatmak için silverlight teknolojisini kullanır.
- **html5**: video öğesi ile tarayıcı tabanlı oynatma teknolojisikullanır.  Bir Apple iOS veya Android cihazda yken, bu teknoloji AES-128 bit zarf şifreleme veya DRM içeriği için bazı temel destekle HLS akışlarının oynatTırılmasına izin verir (tarayıcı desteklediğinde FairPlay üzerinden).

## <a name="tech-order"></a>Teknik Sipariş ##

Kıymetinizin çok çeşitli cihazlarda oynanabilir olduğundan emin olmak için, aşağıdaki teknik sipariş `techOrder: ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"]` önerilir ve aşağıdaki `<video>` durumlarda varsayılandır: ve seçeneklerde doğrudan veya programlı olarak ayarlanabilir:

`<video data-setup='{"techOrder": ["azureHtml5JS", "flashSS", "html5FairPlayHLS, "silverlightSS", "html5"]}`

or

```javascript
    amp("vid1", {
          techOrder: ["azureHtml5JS", "flashSS", "html5FairPlayHLS, "silverlightSS", "html5"]
    });
```

## <a name="compatibility-matrix"></a>Uyumluluk Matrisi ##

Azure Media Services'ten akış içeriği içeren önerilen teknik sipariş göz önüne alındığında, aşağıdaki uyumluluk oynatma matrisi beklenmektedir

| Tarayıcı        | İşletim Sistemi                                                       | Beklenen Teknoloji (Clear)  | Beklenen Teknoloji (AES)  | Beklenen Teknoloji (DRM)          |
|----------------|----------------------------------------------------------|------------------------|----------------------|------------------------------|
| Kenar 11      | Windows 10, Windows 8.1, Windows Phone 101               | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (PlayReady)     |
| Yani 11IE 9-101  | Windows 7, Windows Vista<sup>1</sup>                     | flashSS                | flashSS              | silverlightSS (PlayReady)    |
| IE 11          | Windows Phone 8.1                                        | azureHtml5JS           | azureHtml5JS         | desteklenmiyor                |
| Edge           | Xbox One<sup>1</sup> (Kasım 2015 güncellemesi)                   | azureHtml5JS           | azureHtml5JS         | desteklenmiyor                |
| Krom 37+     | Windows 10, Windows 8.1, macOS X Yosemite<sup>1</sup>   | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (Widevine)      |
| Firefox 47+    | Windows 10, Windows 8.1, macOS X Yosemite+<sup>1</sup>  | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (Widevine)      |
| Firefox 42-46  | Windows 10, Windows 8.1, macOS X Yosemite+<sup>1</sup>  | azureHtml5JS           | azureHtml5JS         | silverlightSS (PlayReady)    |
| Firefox 35-41  | Windows 10, Windows 8.1                                  | flashSS                | flashSS              | silverlightSS (PlayReady)    |
| Safari         | iOS 6+                                                   | html5                  | html5 (belirteç yok)3    | desteklenmiyor                |
| Safari 8+      | OS X Yosemite+                                           | azureHtml5JS           | azureHtml5JS         | html5FairPlayHLS (FairPlay)  |
| Safari 6       | OS X Dağ Aslanı<sup>1</sup>                           | flashSS                | flashSS              | silverlightSS (PlayReady)    |
| Krom 37+     | Android 4.4.4+<sup>2</sup>                               | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (Widevine)      |
| Krom 37+     | Android 4.02                                             | html5                  | html5 (belirteç yok)<sup>3</sup>    | desteklenmiyor                |
| Firefox 42+    | Android 5.0+<sup>2</sup>                                 | azureHtml5JS           | azureHtml5JS         | desteklenmiyor                |
| IE 8           | Windows                                                  | desteklenmiyor          | desteklenmiyor        | desteklenmiyor                |

<sup>1</sup> Yapılandırma desteklenmez veya test edilmez; tamamlanmak için referans olarak listelenir.

<sup>2</sup> Android cihazlarda başarılı oynatma, aygıt özellikleri, grafik desteği, codec oluşturma, işletim sistemi desteği ve daha fazlasının bir birleşimini gerektirir. Android telefon üreticileri Google tarafından sağlanan Vanilla Android işletim sistemi değiştirmek için izin veren bir açık kaynak platformu olduğundan, Bu Android alanında bazı parçalanma neden, ve bazı cihazlar özellikleri eksikliği nedeniyle desteklenmeyebilir. Ayrıca, bazı Android cihazlar tüm codec desteği yok.  

<sup>3</sup> Belirteç için destek olmadığı durumlarda, bu işlevselliği eklemek için bir proxy kullanılabilir. Bu çözüm hakkında daha fazla bilgi edinmek için bu [bloga](https://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) göz atın.

> [!NOTE]
> Beklenen teknoloji seçilen flash gibi bir eklenti yüklü gerektirir, ve kullanıcının makineyüklü değilse, AMP kaynak türleri ve koruma bilgileri ile birlikte, teknoloji listesinde bir sonraki teknolojinin yeteneklerini kontrol etmeye devam edecektir. Örneğin, OS X Yosemite'de Safari 8'de korumasız isteğe bağlı bir akış görüntülemeye çalışırken ve hem Flash hem de Silverlight yüklenmezse, AMP oynatma için yerel Html5 teknik ini seçer.<br/><br/>Yeni tarayıcı teknolojileri her gün ortaya çıkmaktadır ve bu nedenle bu matrisi etkileyebilir.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)