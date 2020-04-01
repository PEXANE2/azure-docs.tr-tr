---
title: StorSimple cihazını güvenli bir şekilde kurun ve çalıştırın
description: Güvenlik kurallarını, yönergeleri ve dikkatleri açıklar ve StorSimple cihazınızı güvenli bir şekilde nasıl yükleyip çalıştırılacakaçıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: dae6d535-1ca2-4d2b-b221-6819043aa068
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: alkohli
ms.openlocfilehash: 480875c17d5ff5bb5c0d42d827b5477f45bf30f4
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396456"
---
# <a name="safely-install-and-operate-your-storsimple-device"></a>StorSimple cihazınızı güvenli bir şekilde kurun ve çalıştırın

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

![Uyarı](./media/storsimple-safety/IC740879.png)
![Simgesi Oku](./media/storsimple-safety/IC740885.png) Güvenlik Bildirimi Simgesi **OKU GÜVENLİk VE SAĞLIK BİlGİlerİ**

Bu makalede Microsoft Azure StorSimple aygıtınız için geçerli olan tüm güvenlik ve sistem durumu bilgilerini okuyun. İleride başvurmak için StorSimple cihazınızla birlikte gönderilen tüm basılı kılavuzları saklayın. Talimatlara uyulmaması ve bu ürünün düzgün bir şekilde ayarlanması, kullanılması ve bakımının yapılmaması, ciddi yaralanma veya ölüm veya cihaz veya cihazlara zarar verme riskini artırabilir. [Bu kılavuzun indirilebilir sürümü](https://www.microsoft.com/download/details.aspx?id=44233) de mevcuttur.

## <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları
Microsoft Azure StorSimple aygıtınızı kurarken ve çalıştırırken gözlenecek güvenlik önlemlerini gözden geçirdiğinizde bulacağınız simgeler aşağıda verilmiştir.

| Simge | Açıklama |
|:--- |:--- |
| ![Tehlike](./media/storsimple-safety/IC740879.png) Simgesi **DANGER!** |Kaçınılmaması halinde ölümle veya ciddi yaralanmalara yol açabilecek tehlikeli bir durumu gösterir. Bu sinyal kelimesi en uç durumlarla sınırlı olacaktır. |
| ![Uyarı](./media/storsimple-safety/IC740879.png) Simgesi **UYARI!** |Kaçınılmaması halinde ölüm veya ciddi yaralanmalara yol açabilecek tehlikeli bir durumu gösterir. |
| ![Uyarı](./media/storsimple-safety/IC740879.png) Simgesi **DİkKAT!** |Kaçınılmaması halinde küçük veya orta derecede yaralanmaya neden olabilecek tehlikeli bir durumu gösterir. |
| ![Bildirim](./media/storsimple-safety/IC740881.png) Simgesi **DİkKAT:** |Önemli kabul edilen, ancak tehlikeyle ilgili olmayan bilgileri gösterir. |
| ![Elektrik Şoku Simgesi](./media/storsimple-safety/IC740882.png) Elektrik **Şoku Tehlikesi** |Yüksek gerilim |
| ![Ağır Simge](./media/storsimple-safety/IC740883.png) **Ağır Ağırlık** | |
| ![Kullanıcı ServisiNe Tabi](./media/storsimple-safety/IC740879.png) Parçalar Simgesi **Yok Kullanıcı Servis Eki Parçalar** |Düzgün bir şekilde eğitilmedikçe erişmeyin. |
| ![Güvenlik Bildirimi Simgesini Oku](./media/storsimple-safety/IC740885.png)**Önce Tüm Talimatları Oku** | |
| ![İpucu Tehlike](./media/storsimple-safety/IC740886.png) Simgesi **İpucu Tehlike** | |

## <a name="handling-precautions"></a>Taşıma önlemleri
![Uyarı](./media/storsimple-safety/IC740879.png) ![Simgesi Ağır](./media/storsimple-safety/IC740883.png) Ağırlık Simgesi **UYARI!** 

Yaralanma riskini azaltmak için:

* Tamamen yapılandırılmış bir muhafaza 32 kg'a (70 lbs) kadar ağırolabilir; tek başınıza kaldırmaya çalışmayın.
* Muhafazayı hareket ettirmeden önce, her zaman ağırlığı işlemek için iki kişinin kullanılabilir olduğundan emin olun. Bu ağırlığı kaldırmaya çalışan bir kişinin yaralanmalara neden olabileceğini unutmayın.
* Ünitenin arkasında bulunan Güç ve Soğutma Modülleri (PCM) üzerindeki tutamaklarla muhafazayı kaldırmayın. Bunlar ağırlığı almak için tasarlanmaz.

## <a name="connection-precautions"></a>Bağlantı önlemleri
![Uyarı](./media/storsimple-safety/IC740879.png) ![Simgesi Elektrik](./media/storsimple-safety/IC740882.png) Şok Ukonu **UYARI!**

Yaralanma, elektrik çarpması veya ölüm olasılığını azaltmak için:

* Birden fazla AC kaynağı tarafından desteklendiğinde, tam yalıtım için tüm besleme gücünü niçin kes.
* Taşımadan önce veya herhangi bir şekilde hasar gördüğünü düşünüyorsanız, ünitenin fişini kalıcı olarak çıkarın.
* Güç kaynağı kablolarına güvenli bir elektrik dünya bağlantısı sağlayın. İktidaruygulamadan önce kasanın topraklamasının ulusal ve yerel gereksinimleri karşıladığını doğrulayın.
* PcM'nin kasadan çıkarılmasından önce güç bağlantısının her zaman kesildiğinden emin olun.
* Güç besleme kablosunun fişinin ana bağlantı kesme cihazı olduğu göz önüne alındığında, soket prizlerinin ekipmanın yakınında olduğundan ve kolayca erişilebilir olduğundan emin olun.

![Uyarı](./media/storsimple-safety/IC740879.png) ![Simgesi Elektrik](./media/storsimple-safety/IC740882.png) Şok Ukonu **UYARI!**

Elektrik bağlantılarından aşırı ısınma veya yangın olasılığını azaltmak için:

* Teknik şartnamede ayrıntılı olarak belirtilen gereksinimleri karşılamak için elektrik aşırı yük koruması ile uygun bir güç kaynağı sağlayın.
* Çatallı güç kabloları ("Y" müşteri adayları) kullanmayın.
* Geçerli güvenlik, emisyon ve termal gereksinimlere uymak için kapaklar çıkarılmamalı ve tüm bölmeler eklenti modülleri veya sürücü boşluklarıyla doldurulmalıdır.
* Ekipmanın üretici tarafından belirtilen şekilde kullanıldığından emin olun. Bu ekipman üretici tarafından belirtilmeyen bir şekilde kullanılırsa, ekipman tarafından sağlanan koruma bozulabilir.

![Bildirim](./media/storsimple-safety/IC740881.png) Simgesi **DİkKAT:**

Ekipmanınızın düzgün çalışması ve ürün hasarının önlenmesi için:

* Aygıtın arka sınırındaki RJ45 portları yalnızcümün Ethernet bağlantısı içindir. Bunlar bir telekomünikasyon ağına bağlı olmamalıdır.
* Cihazı önden arkaya soğutma tasarımına uygun bir rafa taktığından emin olun.
* Tüm eklenti modülleri ve boş plakalar sistem kasasının bir parçasıdır. Bunlar yalnızca bir yedek hemen eklenebileceğinde kaldırılmalıdır. Sistem, tüm modüller veya boşluklar yerinde olmadan çalıştırılmamalıdır.

## <a name="rack-system-precautions"></a>Raf sistemi önlemleri
Cihazı raf dolabına monte ederken aşağıdaki güvenlik gereksinimleri göz önünde bulundurulmalıdır.

![Uyarı](./media/storsimple-safety/IC740879.png) ![Simgesi İpucu](./media/storsimple-safety/IC740886.png) Tehlike Simgesi **UYARI!**

Bir ipucu üzerinde yaralanma olasılığını azaltmak için:

* Raf tasarımı, yüklenen muhafazaların toplam ağırlığını desteklemeli ve rafın kurulum veya normal kullanım sırasında devrilmesini veya itilmesine engel olacak stabilize özellikleri dahil etmelidir.
* Raf ı yüklerken, rafı aşağıdan yukarıya doldurun ve yukarıdan aşağıya boşlayın.
* Rafın devrilmesi tehlikesini önlemek için raftan birden fazla muhafazayı bir seferde çıkarmayın.

![Uyarı](./media/storsimple-safety/IC740879.png) ![Simgesi Elektrik](./media/storsimple-safety/IC740882.png) Şok Ukonu **UYARI!**

Yaralanma, elektrik çarpması veya ölüm olasılığını azaltmak için:

* Raf güvenli bir elektrik dağıtım sistemi olmalıdır. Kasa için aşırı akım koruması sağlamalı ve yüklenen toplam kasa sayısı tarafından aşırı yüklenmemelidir. İsim plakası üzerinde gösterilen elektrik enerjisi tüketim derecesine dikkat edilmelidir.
* Elektrik dağıtım sistemi raftaki her muhafaza için güvenilir bir zemin sağlamalıdır.
* Elektrik dağıtım sisteminin tasarımı, tüm muhafazalarda bulunan tüm güç kaynaklarından gelen toplam zemin kaçağı akımını dikkate almalıdır. Her kasadaki her güç kaynağının 60 Hz, 264 voltda maksimum 1,0 mA'lık bir toprak kaçağı akımı olduğunu unutmayın. Raf "YÜKSEK KAÇAK AKıMı" ile etiketleme gerektirebilir. Toprak (toprak) bağlantısı, bir kaynağı bağlamadan önce esastır."
* Raf, muhafazaları ile yapılandırıldığında, UL 60950-1 ve IEC 60950-1/EN 60950-1 güvenlik gereksinimlerini karşılamalıdır.

![Bildirim](./media/storsimple-safety/IC740881.png) Simgesi **DİkKAT:**

Raf sisteminizin doğru soğutması için:

* Raf tasarımının 35 santigrat derece (95 fahrenhayt) maksimum kasa çalışma ortamı sıcaklığını dikkate aldığından emin olun.
* Sistem düşük basınçlı, arka egzoz tesisatı (raf kapıları ve engellerin oluşturduğu arka basınç 5 Pascal [0,5 mm su göstergesi] geçmemek üzere) ile çalıştırılır.

## <a name="power-cooling-module-pcm-precautions"></a>Güç Soğutma Modülü (PCM) önlemleri
Cihaz iki PCMs ile çalışacak şekilde tasarlanmıştır. PCM'lerin her birinde güç kaynağı ve çift eksenli fan vardır. Kritik bir durum sırasında, sistem normal işlemleri devam ederken bir güç kaynağının arızalanmasına izin verir. İki PCM (ve dolayısıyla güç kaynakları) her zaman kurulmalıdır. Tek bir PCM yedek güç sağlamaz. Bu nedenle, bir PCM'nin bile arızalanması kapalı kalma süresine veya olası veri kaybına neden olabilir.

![Uyarı](./media/storsimple-safety/IC740879.png) ![Simgesi Elektrik](./media/storsimple-safety/IC740882.png) Şok Ukonu **UYARI!**

Yaralanma, elektrik çarpması veya ölüm olasılığını azaltmak için:

* Kapakları PCM'den çıkarmayın. İçeride elektrik çarpması tehlikesi var. PCM'yi döndürmek ve yenisini almak için [Microsoft Destek'e başvurun.](storsimple-contact-microsoft-support.md)

![Bildirim](./media/storsimple-safety/IC740881.png) Simgesi **DİkKAT:**

Ekipmanınızın düzgün çalışması ve ürün hasarının önlenmesi için:

* Başarısız PCM'yi 24 saat içinde değiştirmeniz gerekir. Bir PCM değiştirilme için kaldırıldıktan sonra, değiştirme çıkarıldıktan sonra 10 dakika içinde tamamlanmalıdır.
* Bir yedek hemen kurulamadıkça PCM'yi çıkarmayın. Kasa, tüm modüller yerinde olmadan çalıştırılmamalıdır.

## <a name="electrostatic-discharge-esd-precautions"></a>Elektrostatik deşarj (ESD) önlemleri
![Bildirim](./media/storsimple-safety/IC740881.png) Simgesi **DİkKAT:**

ESD ile ilgili aşağıdaki önlemleri gözlemleyin.

* Uygun bir antistatik bilek veya ayak bileği kayışı taktığınıza ve kontrol ettiğinizden emin olun.
* Modülleri ve bileşenleri işlerken tüm geleneksel ESD önlemlerine uyun.
* Arka düzlem bileşenleri ve modül konektörleri ile temasından kaçının.
* ESD hasarı garanti kapsamında değildir.

## <a name="battery-disposal-precautions"></a>Pil imha önlemleri
Güç kaynağı, geçici, kısa süreli elektrik kesintileri sırasında bellek içeriğini korumak için özel bir pil kullanır. Pil PCM'de yer alır. Pil hakkında aşağıdaki bilgileri aklınızda bulundurun.

![Uyarı](./media/storsimple-safety/IC740879.png) Simgesi **UYARI!**

Şort, yangın, patlama, yaralanma veya ölüm riskini azaltmak için:

* Kullanılmış pilleri ulusal/bölgesel yönetmeliklere uygun olarak atın.
* 60 santigrat derecenin (140 fahrenhayt) üzerinde sökmeyin, ezmeyin veya ısımayın veya yakmayın. PCM pilini yalnızca sağlanan bir pil ile değiştirin. Başka bir pilin kullanılması yangın veya patlama riski ne olabilir.
* Bunlar güç kaynağından kaldırılırsa pillerde koruyucu uç kapakları kullanın.

![Bildirim](./media/storsimple-safety/IC740881.png) Simgesi **DİkKAT:**

Pilleri hava yoluyla taşırken veya başka bir şekilde taşırken,[https://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx](https://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx)

Bu güvenlik bildirimlerini inceledikten sonra, sonraki adımlar cihazınızı açmak, rafa çıkarmak ve kablolamaktır.

## <a name="next-steps"></a>Sonraki adımlar
* 8100 aygıtı [için StorSimple 8100 cihazınızı yükleyin.](storsimple-8100-hardware-installation.md)
* 8600 [aygıtıiçin StorSimple 8600 cihazınızı yükleyin.](storsimple-8600-hardware-installation.md)

