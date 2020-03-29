---
title: StorSimple 8000 serisi cihaza Güncelleme 4'e yükleme | Microsoft Dokümanlar
description: StorSimple 8000 serisi cihazınıza StorSimple 8000 Serisi Güncelleme 4'ün nasıl yüklenir olduğunu açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 5b48cbd1020cfd51fe989a9be33197f2735f21f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60860527"
---
# <a name="install-update-4-on-your-storsimple-device"></a>StorSimple cihazınıza Güncelleme 4'e yükleme

## <a name="overview"></a>Genel Bakış

Bu öğretici, Azure portalı üzerinden daha önceki bir yazılım sürümünü çalıştıran ve düzeltme yöntemini kullanan bir StorSimple cihazına Güncelleştirme 4'ün nasıl yüklenir olduğunu açıklar. Hotfix yöntemi, StorSimple aygıtının DATA 0 dışındaki bir ağ arabiriminde bir ağ geçidi yapılandırıldığında ve güncelleştirme öncesi 1 yazılım sürümünden güncelleştirmeye çalıştığınızda kullanılır.

Güncelleme 4 cihaz yazılımı, USM firmware, LSI sürücüsü ve firmware, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmeleri ve diğer işletim sistemi güncellemeleri bir dizi içerir.  Cihaz yazılımı, USM firmware, Spaceport, Storport ve diğer işletim sistemi güncelleştirmeleri kesintisiz güncelleştirmelerdir. Kesintisiz veya düzenli güncelleştirmeler Azure portalı üzerinden veya düzeltme yöntemi yle uygulanabilir. Disk firmware güncelleştirmeleri rahatsız edici güncelleştirmelerdir ve yalnızca aygıtın Windows PowerShell arabirimi kullanılarak hotfix yöntemi yle uygulanabilir.

> [!IMPORTANT]
> * Donanım durumu ve ağ bağlantısı açısından aygıt durumunu belirlemek için yüklemeden önce bir dizi el ile ve otomatik ön denetim yapılır. Bu ön denetimler yalnızca Azure portalındaki güncelleştirmeleri uygularsanız gerçekleştirilir.
> * Yazılımı ve diğer düzenli güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Yalnızca ön güncelleştirme ağ geçidi denetimi portalda başarısız olursa aygıtın Windows PowerShell arabirimine (güncelleştirmeleri yüklemek için) gitmeniz gerekir. Güncelleme yaptığınız sürüme bağlı olarak, güncelleştirmelerin yüklenmesi 4 saat (veya daha fazla) sürebilir. Bakım modu güncelleştirmeleri de cihazın Windows PowerShell arabirimi üzerinden yüklenmiş olmalıdır. Bakım modu güncelleştirmeleri rahatsız edici güncelleştirmeler olduğundan, bunlar cihazınıziçin bir kesinti süresine neden olur.
> * İsteğe bağlı StorSimple Snapshot Manager'ı çalıştırıyorsanız, aygıtı güncellemeden önce Snapshot Manager sürümünüzü Update 4'e yükselttiğinizden emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-the-azure-portal"></a>Azure portalı üzerinden Güncelleme 4'e yükleme
Cihazınızı [Güncelleştirme 4'e](storsimple-update4-release-notes.md)güncelleştirmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Microsoft aygıttan ek tanılama bilgileri çeker. Sonuç olarak, operasyon ekibimiz sorun olan aygıtları tanımladığında, cihazdan bilgi toplamak ve sorunları tanılamak için daha iyi donanıma sahip olacağız. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Cihazınızın **StorSimple 8000 Serisi Güncelleme 4 (6.3.9600.17820)** çalıştığını doğrulayın. **Son güncelleştirme tarihi** de değiştirilmelidir.

* Şimdi Bakım modu güncelleştirmelerinin kullanılabilir olduğunu göreceksiniz (bu ileti güncelleştirmeleri yükledikten sonra 24 saate kadar görüntülenmeye devam edebilir). Bakım modu güncelleştirmeleri, aygıtın kapalı kalma süresiyle sonuçlanan rahatsız edici güncelleştirmelerdir ve yalnızca cihazınızın Windows PowerShell arabirimi üzerinden uygulanabilir.

* Disk firmware güncelleştirmelerini yükleyen KB4011837'yi aramak ve indirmek için [hotfixes'i indirmek](#to-download-hotfixes) için listelenen adımları kullanarak bakım modu güncelleştirmelerini indirin (diğer güncelleştirmeler şimdiye kadar yüklenmiş olmalıdır). Yükleme de listelenen adımları izleyin ve bakım modu güncelleştirmelerini yüklemek için [bakım modu eklerini doğrulayın.](#to-install-and-verify-maintenance-mode-hotfixes)

## <a name="install-update-4-as-a-hotfix"></a>Güncelleştirme 4'e düzeltme olarak yükleme
Güncelleştirme 4'e yüklemek için önerilen yöntem Azure portalı üzerinden yapılır.

Güncelleştirmeleri Azure portalı üzerinden yüklemeye çalışırken ağ geçidi denetiminde başarısız olursanız bu yordamı kullanın. DATA 0 olmayan bir ağ arabirimine atanmış bir ağ geçidiniz olduğundan ve aygıtınız Güncelleştirme 1'den önce bir yazılım sürümünü çalıştırdığı için denetim başarısız olur.

Düzeltme yöntemi kullanılarak yükseltilebilen yazılım sürümleri şunlardır:

* 0.1, 0.2, 0.3'u güncelleştirin
* Güncelleştirme 1, 1.1, 1.2
* Güncelleştirme 2, 2.1, 2.2
* Güncelleme 3, 3.1


Hotfix yöntemi aşağıdaki üç adımı içerir:

1. Düzeltmeleri Microsoft Güncelleştirme Kataloğu'ndan indirin.
2. Normal mod düzeltmelerini yükleyin ve doğrulayın.
3. Bakım modu düzeltmesini yükleyin ve doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncellemeleri karşıdan yükleme

Aşağıdaki düzeltmeleri öngörülen sırada ve önerilen klasörlere indirmeli ve yüklemeniz gerekir:

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleme|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Yazılım güncelleştirmesi |Normal <br></br>Kesintisiz |~ 25 dakika |FirstOrderUpdate|
| 2a. |KB4011841 <br> KB4011842 |LSI sürücü ve firmware güncellemeleri <br> USM firmware güncellemesi (sürüm 3.38) |Normal <br></br>Kesintisiz |~ 3 saat <br> (2A içerir. + 2B. + 2C.)|SecondOrderUpdate|
| 2B' ye kadar. |KB3139398, KB3108381 <br> KB3205400, KB3142030 <br> KB3197873, KB3197873 <br> KB3192392, KB3153704 <br> KB3174644, KB3139914  |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 İndir |Normal <br></br>Kesintisiz |- |SecondOrderUpdate|
| 2C' ye kadar. |KB3210083, KB3103616 <br> KB3146621, KB3121261 <br> KB3123538 |İşletim sistemi güncellemeleri paketi <br> Windows Server 2012 R2 İndir |Normal <br></br>Kesintisiz |- |SecondOrderUpdate|

Ayrıca, önceki tablolarda gösterilen tüm güncelleştirmelerin üstüne disk firmware güncelleştirmeleri yüklemeniz gerekebilir. `Get-HcsFirmwareVersion` Cmdlet'i çalıştırarak disk firmware güncelleştirmesine ihtiyacınız olup olmadığını doğrulayabilirsiniz. Bu `XMGJ`firmware sürümlerini çalıştırıyorsanız: `VR08`, `N002` `0106` `XGEG`, `KZ50` `F6C2`, , , , , o zaman bu güncelleştirmeleri yüklemeniz gerekmez.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati | Klasöre yükleme|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Disk firmware |Bakım <br></br>Yıkıcı |~ 30 dakika | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Güncelleştirme 4'ün uygulanması için bu yordamın yalnızca bir kez gerçekleştirilmesi gerekir. Sonraki güncelleştirmeleri uygulamak için Azure portalını kullanabilirsiniz.
> * Güncelleştirme 3 veya 3.1'den güncelleniyorsa, toplam yükleme süresi 4 saate yakındır.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce, hem aygıt denetleyicilerinin çevrimiçi olduğundan hem de tüm donanım bileşenlerinin sağlıklı olduğundan emin olun.

Düzeltmeleri indirmek ve yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
Güncelleme 4 [sürümü](storsimple-update4-release-notes.md)hakkında daha fazla bilgi edinin.

