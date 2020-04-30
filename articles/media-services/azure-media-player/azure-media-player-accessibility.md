---
title: Azure Media Player erişilebilirliği
description: Azure Media Player erişilebilirlik ayarları hakkında daha fazla bilgi edinin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 2231c2969bbfce1668002ad4f5f719e0b8e13de5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81726560"
---
# <a name="accessibility"></a>Erişilebilirlik #

Azure Media Player, Windows ekran okuyucusu ve Apple OSX/iOS VoiceOver gibi ekran okuyucu özellikleri ile birlikte kullanılabilir. Alternatif Etiketler UI düğmeleri için kullanılabilir ve ekran okuyucu, Kullanıcı onlara gittiğinde bu alternatif etiketleri okuyabilir. Daha fazla yapılandırma Işletim sistemi düzeyinde ayarlanabilir.

## <a name="captions-and-subtitles"></a>Açıklamalı alt yazılar ve alt yazılar ##

Şu anda Azure Media Player şu anda, CEA 708 kullanarak yalnızca WebVTT biçimli ve canlı etkinliklerle Isteğe bağlı olaylara yönelik açıklamalı alt yazıları desteklemektedir. TTML biçimi şu anda desteklenmiyor. Açıklamalı alt yazılar ve alt yazılar, oynatıcının işitme engelli kişiler veya farklı bir dilde okumak isteyen kişiler gibi daha geniş bir kitleye hitap etmesini sağlar. Açıklamalı alt yazılar ayrıca video etkileşimini artırır, içeriğin daha iyi kavranmasını sağlar ve videonun iş yeri gibi ses açısından hassas ortamlarda görüntülenmesini mümkün kılar.  

## <a name="high-contrast-mode"></a>Yüksek karşıtlık modu ##

Azure Media Player ' deki varsayılan kullanıcı arabirimi, çoğu cihaz/tarayıcı Yüksek Karşıtlık görünüm modları ile uyumludur. Yapılandırma Işletim sistemi düzeyinde ayarlanabilir.

## <a name="mobility-options"></a>Mobility seçenekleri ##

### <a name="tabbing-focus"></a>Sekme odaklı ###

Genel HTML standartları tarafından sağlanan sekme odağı Azure Media Player kullanılabilir. Sekme odaklama özelliğini etkinleştirmek için, (veya sekme `tabindex=0` sırasının HTML 'de nasıl etkilendiğine ilişkin bir değer) eklemeniz gerekir, `<video>` örneğin: `<video ... tabindex=0>...</video>`. Bazı platformlarda denetimlerin odağı yalnızca denetimler görünür durumdaysa ve platform bu özellikleri destekliyorsa bulunabilir.

Sekme odağı etkinleştirildikten sonra, Son Kullanıcı, fareye bağlı kalmadan video yürütücüyü etkin bir şekilde gezabilir ve denetleyebilir. Her bağlam menüsü veya denetlenebilir öğe, sekme düğmesine vurarak ve ENTER ya da boşluk ile seçili bir şekilde gezinilebilir. Bağlam menüsünde ENTER veya boşluk çubuğuna basmak, son kullanıcının bir menü öğesi seçmek üzere sekmeye devam edebilmesi için onu genişletir. Seçmek istediğiniz öğenin bağlamına ulaştığınızda seçimi tamamlamak için yeniden Enter tuşuna veya ara çubuğuna basın.

### <a name="hotkeys"></a>Tuşlarının ###

Azure Media Player klavye kısayol tuşu üzerinden denetlemeyi destekler. Bir Web tarayıcısında, temel alınan video öğesini kontrol etmenin tek yolu Player 'a odaklanmalıdır. Oyuncu üzerinde odak olduktan sonra, kısayol tuşu Player işlevlerini denetleyebilir.  Aşağıdaki tabloda, çeşitli kısayol tuşları ve bunlarla ilişkili davranışlar açıklanmıştır.

| Kısayol tuşu              | Davranış                                                                |
|----------------------|-------------------------------------------------------------------------|
| F/f                  | Oynatıcı Tam Ekran moduna girer veya bu moddan çıkar                                  |
| M/m                  | Oynatıcının sesi kapatılır/açılır                                          |
| Yukarı ve Aşağı Ok    | Oynatıcının sesi artar/azalır                                    |
| Sol ve Sağ Ok | Video ileri/geri alınır                                  |
| 0,1,2,3,4,5,6,7,8,9  | Basılan anahtara bağlı olarak, video ilerleme durumu\- %90 %0 olarak değiştirilecek |
| Tıklama eylemi         | Video oynatılır/duraklatılır                                                   |

## <a name="next-steps"></a>Sonraki adımlar

<!---Some context for the following links goes here--->
- [Hızlı başlangıç Azure Media Player](azure-media-player-quickstart.md)