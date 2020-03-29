---
title: StorSimple 8000 serisi donanım bileşen değiştirme | Microsoft Dokümanlar
description: StorSimple cihazının PCM'leri, pil, denetleyici modülleri, EBOD denetleyicileri, disk sürücüleri ve şasisinin güvenli bir şekilde nasıl değiştirilebildiğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: e05a37122647d4979089f0ba00b1fc15f9b84b0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60321826"
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınızdaki donanım bileşenini değiştirme

## <a name="overview"></a>Genel Bakış
Donanım bileşeni değiştirme öğreticileri, Microsoft Azure StorSimple 8000 serisi cihazınızın donanım bileşenlerini ve bunları kaldırmak ve değiştirmek için gereken adımları açıklar. Bu makalede, güvenlik simgeleri açıklanır, ayrıntılı öğreticiler için işaretçiler sağlar ve değiştirilebilir bileşenleri listeler.

> [!IMPORTANT]
> Herhangi bir StorSimple bileşenini kaldırmaya veya değiştirmeye çalışmadan önce, [güvenlik simgesi kurallarını](#safety-icon-conventions) ve diğer güvenlik [önlemlerini](storsimple-safety.md)gözden aldığınızdan emin olun.


### <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları
Aşağıdaki tabloda bu öğreticilerde kullanılan güvenlik simgeleri açıklanmaktadır. Cihaz bileşenlerini kaldırmak ve değiştirmek için adımlardan geçerken bu güvenlik simgelerine dikkat edin.

| Simge | Metin | Ek bilgiler |
|:--- |:--- |:--- |
| ![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) |**Tehlike!** |Kaçınılmaması halinde ölümle veya ciddi yaralanmalara yol açabilecek tehlikeli bir durumu gösterir. Bu sinyal sözcüğü en uç durumlarla sınırlıdır. |
| ![Uyarı simgesi](./media/storsimple-hardware-component-replacement/Warning.png) |**Uyarı!** |Kaçınılmaması halinde ölüm veya ciddi yaralanmalara yol açabilecek tehlikeli bir durumu gösterir. |
| ![Dikkat simgesi](./media/storsimple-hardware-component-replacement/Caution.png) |**Dikkat!** |Kaçınılmaması halinde küçük veya orta derecede yaralanmaya neden olabilecek tehlikeli bir durumu gösterir. |
| ![Bildirim simgesi](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**Fark:** |Önemli kabul edilen, ancak tehlikeyle ilgili olmayan bilgileri gösterir. |
| ![Elektrik şoku simgesi](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektrik Çarpması Tehlikesi** |Yüksek voltajı gösterir. |
| ![Ağır ağırlık simgesi](./media/storsimple-hardware-component-replacement/Weight.png) |**Ağır Ağırlık** | |
| ![Kullanıcı yada servis edilebilen parçalar simgesi yok](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Kullanıcı ServisiNe Uygun Parça Yok** |Düzgün bir şekilde eğitilmedikçe erişmeyin. |
| ![Talimatları okuma simgesi](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Önce Tüm Talimatları Okuyun** | |
| ![İpucu tehlike simgesi](./media/storsimple-hardware-component-replacement/TipHazard.png) |**İpucu Tehlike** | |

### <a name="before-you-begin"></a>Başlamadan önce
Bu eğitimde kullanılan cihazınız ve güvenlik simgeleri hakkındaki güvenlik bilgilerini edin. Tam bilgi için [StorSimple cihazınızı güvenli bir şekilde yükleyin ve çalıştırın.](storsimple-safety.md) StorSimple cihazınızı işlemeden önce [Güvenlik önlemlerini](storsimple-safety.md#handling-precautions) gözden geçirdiğinizden emin olun.

Bir bileşeni değiştirmeyi denemeden önce aşağıdaki bilgileri göz önünde bulundurun.

![Uyarı](./media/storsimple-hardware-component-replacement/Warning.png) ![Simgesi Elektrik](./media/storsimple-hardware-component-replacement/Electric.png) Şok Ukonu **UYARI!**

* StorSimple cihazınızın modüllerini ve bileşenlerini kullanırken elektrostatik deşarj veya antistatik paspas kullanarak kendinizi düzgün bir şekilde topraklayın.
* Herhangi bir devreye dokunmayın. Devreleri açığa çıkaran bileşenleri kullanırken verilen tutamaçları ve kılavuzları kullanın.

![Uyarı](./media/storsimple-hardware-component-replacement/Warning.png) ![Simgesi](./media/storsimple-hardware-component-replacement/NoticeIcon.png) Bildirimi Simge **DİkKAT:**

Bir modülü değiştirdiğinizde, **kasanın arkasında boş bir yuva asla bırakmayın.** Sorun kısmını kaldırmadan önce yedek veya boş bir modül edinin.

## <a name="hardware-component-replacement-procedures"></a>Donanım bileşeni değiştirme yordamları
StorSimple 8000 serisi cihazınız birincil ve/veya EBOD muhafazalarında birkaç eklenti modülünden oluşur. 8100 tek bir birincil kasaya sahipken, 8600 birincil kasası ve EBOD muhafazası olan çift kasalı bir cihazdır.

Cihazınızdaki ana donanım bileşenleri aşağıdaki tablolarda özetlenmiştir. İlişkili öğreticiye gitmek için **Değiştirme yordamı** sütunundaki bağlantıyı tıklatın.

| Bileşenler | # Şimdiki Zaman | Eklenti modülü mü? | Değiştirme prosedürü |
|:--- |:--- |:--- |:--- |
| Kasa |1 |Hayır |[StorSimple cihazınızdaki şasiyi değiştirin](storsimple-8000-chassis-replacement.md) |
| Birincil denetleyiciler |2 |Evet |[StorSimple cihazınızda bir denetleyici modüldeğiştirme](storsimple-8000-controller-replacement.md) |
| 764W Güç ve Soğutma Modülleri (PCM' ler) |2 |Evet |[StorSimple cihazınızın güç ve soğutma modülünü değiştirme](storsimple-8000-power-cooling-module-replacement.md) |
| Yedek pil |2 |Evet |[StorSimple cihazınızın yedek pil modülünü değiştirme](storsimple-8000-battery-replacement.md) |
| Disk sürücüleri |12 |Evet |[StorSimple cihazınızda bir disk sürücüsüdeğiştirme](storsimple-8000-disk-drive-replacement.md) |

**Tablo 1** Birincil kasadaki donanım bileşenleri

Birincil kasa ve EBOD muhafazası G/Ç modüllerinde farklılık gösterir. Ayrıca, PCM'ler farklı watt var. Birincil kasadaki PC'ler 764 W, EBOD kasasındakiler ise 580 W'dır. Birincil kasadaki PCM'ler de yedek pil modülü içerir.

| Bileşenler | # Şimdiki Zaman | Eklenti modülü mü? | Değiştirme prosedürü |
|:--- |:--- |:--- |:--- |
| Kasa |1 |Hayır |[StorSimple cihazınızdaki şasiyi değiştirin](storsimple-8000-chassis-replacement.md) |
| EBOD denetleyicileri |2 |Evet |[StorSimple cihazınızda bir EBOD denetleyicisi değiştirme](storsimple-8000-ebod-controller-replacement.md) |
| 580W Güç ve Soğutma Modülleri (PCM' ler) |2 |Evet |[StorSimple cihazınızın güç ve soğutma modülünü değiştirme](storsimple-8000-power-cooling-module-replacement.md) |
| Disk sürücüleri |12 |Evet |[StorSimple cihazınızda bir disk sürücüsüdeğiştirme](storsimple-8000-disk-drive-replacement.md) |

**Tablo 2** EBOD kasasındaki donanım bileşenleri

Cihazdaki eklenti modülleri aşağıdaki ön ve arka diyagramlarda vurgulanır. Değiştirme gerekiyorsa, çeşitli eklenti modüllerinin konumunu belirlemek için bu diyagramları kullanabilirsiniz. Ön diyagram disk sürücülerini, EBOD kasasının ve birincil kasanın arka diyagramlarını gösterir ve eklenti modüllerini gösterir.

![Disk sürücüleri ile cihazın ön paneli](./media/storsimple-hardware-component-replacement/IC741028.png)

**Şekil 1** Cihazın ön

| Etiketle | Açıklama |
|:--- |:--- |
| 0 - 11 |Disk sürücüleri (toplam 12) |

Hem birincil kasa hem de EBOD muhafazası tahrik taşıyıcı modüllerine sahiptir. Şasi evler on iki 3.5 " disk sürücüler bir 3 tarafından 4 formatında düzenlenmiş.

![Cihaz birincil muhafaza modüllerinin arka düzlemi](./media/storsimple-hardware-component-replacement/IC740994.png)

**Şekil 2** Birincil muhafazanın arkası

| Etiketle | Açıklama |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |Denetleyici 0 |
| 4 |Denetleyici 1 |

![Cihaz EBOD muhafaza eklentisi modüllerinin arka düzlemi](./media/storsimple-hardware-component-replacement/IC769599.png)

**Şekil 3** EBOD kasasının arkası

| Etiketle | Açıklama |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD Denetleyici 0 |
| 4 |EBOD Denetleyici 1 |

## <a name="field-replaceable-units"></a>Alan değiştirilebilir birimler
StorSimple cihazınız için aşağıdaki alan değiştirilebilir birimler (FRUs) mevcuttur:

* Şasi (entegre operasyon paneli dahil)
* 764 W AC PCM
* 580 W AC PCM
* Sürücü taşıyıcı modülüne sahip sabit disk sürücüsü
* Denetleyici modülü
* EBOD denetleyici modülü
* Yedek pil modülü
* Raf montaj ray kiti

Bu değiştirme birimlerinden herhangi birini sipariş etmek için lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

## <a name="next-steps"></a>Sonraki adımlar
StorSimple donanım bileşenini değiştirmeye çalışmadan önce tüm [güvenlik bilgilerini](storsimple-safety.md) gözden geçirin.

