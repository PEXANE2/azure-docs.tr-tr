---
title: Azure Media Player Erişilebilirliği
description: Azure Media Player'ın erişilebilirlik ayarları hakkında daha fazla bilgi edinin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 2231c2969bbfce1668002ad4f5f719e0b8e13de5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726560"
---
# <a name="accessibility"></a>Erişilebilirlik #

Azure Media Player, Windows Ekran Okuyucusu ve Apple OSX/iOS VoiceOver gibi ekran okuyucu özellikleriyle çalışır. Kullanıcı Arabirimi düğmeleri için alternatif etiketler kullanılabilir ve ekran okuyucu kullanıcı bu düğmelere gittiğinde bu alternatif etiketleri okuyabilir. İşletim Sistemi düzeyinde ek yapılandırmalar ayarlanabilir.

## <a name="captions-and-subtitles"></a>Altyazılar ve altyazılar ##

Şu anda Azure Media Player, şu anda yalnızca WebVTT biçimine sahip Isteğe Bağlı etkinlikler ve CEA 708 kullanarak canlı etkinlikler için altyazıları desteklemektedir. TTML biçimi şu anda desteklenmiyor. Açıklamalı alt yazılar ve alt yazılar, oynatıcının işitme engelli kişiler veya farklı bir dilde okumak isteyen kişiler gibi daha geniş bir kitleye hitap etmesini sağlar. Açıklamalı alt yazılar ayrıca video etkileşimini artırır, içeriğin daha iyi kavranmasını sağlar ve videonun iş yeri gibi ses açısından hassas ortamlarda görüntülenmesini mümkün kılar.  

## <a name="high-contrast-mode"></a>Yüksek kontrast modu ##

Azure Media Player'daki varsayılan Kullanıcı Arabirimi, çoğu aygıt/tarayıcı yüksek kontrastlı görünüm moduyla uyumludur. Yapılandırmalar İşletim Sistemi düzeyinde ayarlanabilir.

## <a name="mobility-options"></a>Mobilite seçenekleri ##

### <a name="tabbing-focus"></a>Sekme odağı ###

Genel HTML standartlarına göre sağlanan sekme odağı Azure Media Player'da kullanılabilir. Sekme odaklamayı etkinleştirmek için `tabindex=0` HTML'ye `<video>` sekme sırasının nasıl etkilendiğini (veya başka bir `<video ... tabindex=0>...</video>`değer) eklemeniz gerekir: . Bazı platformlarda, denetimler için odak yalnızca denetimler görünürse ve platform bu yetenekleri destekliyorsa kullanılabilir.

Sekme odağı etkinleştirildikten sonra, son kullanıcı farelerine bağlı olmadan video oynatıcıda etkili bir şekilde gezinebilir ve denetleyebilir. Her bağlam menüsü veya denetlenebilir öğe sekme düğmesine basarak gezinilebilir ve enter veya spacebar ile seçilebilir. Bağlam menüsünde enter veya space bar'a vurmak, son kullanıcının bir menü öğesi seçmek için sekmeye devam edilebilsin diye genişletir. Seçmek istediğiniz öğenin bağlamına ulaştığınızda seçimi tamamlamak için yeniden Enter tuşuna veya ara çubuğuna basın.

### <a name="hotkeys"></a>Hotkeys ###

Azure Media Player klavye sıcak tuşu ile denetlemeyi destekler. Bir web tarayıcısında, altta yatan video öğesini kontrol etmenin tek yolu oynatıcıya odaklanmaktır. Bir kez oyuncu üzerinde duruluyor, sıcak anahtar oyuncu işlevselliğini kontrol edebilirsiniz.  Aşağıdaki tabloda, çeşitli kısayol tuşları ve bunlarla ilişkili davranışlar açıklanmıştır.

| Kısayol tuşu              | Davranış                                                                |
|----------------------|-------------------------------------------------------------------------|
| F/f                  | Oynatıcı Tam Ekran moduna girer veya bu moddan çıkar                                  |
| M/m                  | Oynatıcının sesi kapatılır/açılır                                          |
| Yukarı ve Aşağı Ok    | Oynatıcının sesi artar/azalır                                    |
| Sol ve Sağ Ok | Video ileri/geri alınır                                  |
| 0,1,2,3,4,5,6,7,8,9  | Video ilerlemesi tuşa basıldığında bağlı olarak %0%90\- olarak değiştirilecektir |
| Eylem'i tıklatın         | Video oynatılır/duraklatılır                                                   |

## <a name="next-steps"></a>Sonraki adımlar

<!---Some context for the following links goes here--->
- [Azure Media Player Quickstart](azure-media-player-quickstart.md)