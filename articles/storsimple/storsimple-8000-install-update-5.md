---
title: StorSimple 8000 serisi cihaza Güncelleme 5 yüklenin | Microsoft Dokümanlar
description: StorSimple 8000 serisi güncelleme 5'in Nasıl Yüklenir, StorSimple 8000 serisi cihazınıza nasıl yüklenir.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: d86e77ef0148c0fac3dfa31153364de153b094ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267891"
---
# <a name="install-update-5-on-your-storsimple-device"></a>StorSimple cihazınıza Update 5'i yükleyin

## <a name="overview"></a>Genel Bakış

Bu öğretici, Azure portalı üzerinden daha önceki bir yazılım sürümünü çalıştıran ve düzeltme yöntemini kullanan bir StorSimple cihazına Güncelleştirme 5'in nasıl yüklenir olduğunu açıklar. Düzeltme yöntemi, Güncelleştirme 5'i güncelleştirme öncesi 3 sürümlerini çalıştıran bir aygıta yüklemeye çalışırken kullanılır. Hotfix yöntemi, StorSimple aygıtının DATA 0 dışındaki bir ağ arabiriminde bir ağ geçidi yapılandırıldığında ve güncelleştirme öncesi 1 yazılım sürümünden güncelleştirmeye çalıştığınızda da kullanılır.

Güncelleme 5 aygıt yazılımı, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmeleri ve işletim sistemi güncelleştirmeleri ve disk firmware güncelleştirmeleri içerir.  Aygıt yazılımı, Spaceport, Storport, güvenlik ve diğer işletim sistemi güncelleştirmeleri kesintisiz güncelleştirmelerdir. Kesintisiz veya düzenli güncelleştirmeler Azure portalı üzerinden veya düzeltme yöntemi yle uygulanabilir. Disk firmware güncelleştirmeleri rahatsız edici güncelleştirmelerdir ve aygıtın Windows PowerShell arabirimini kullanarak hotfix yöntemi yle aygıt bakım modundayken uygulanır.

> [!IMPORTANT]
> * Güncelleştirme 5 zorunlu bir güncelleştirmedir ve hemen yüklenmelidir. Daha fazla bilgi için [bkz.](storsimple-update5-release-notes.md)
> * Donanım durumu ve ağ bağlantısı açısından aygıt durumunu belirlemek için yüklemeden önce bir dizi el ile ve otomatik ön denetim yapılır. Bu ön denetimler yalnızca Azure portalındaki güncelleştirmeleri uygularsanız gerçekleştirilir.
> * Güncelleştirme 3'ten önce sürümleri çalıştıran bir aygıtı güncelleştirirken güncelleştirmeleri hotfix yöntemini kullanarak yüklemenizi şiddetle öneririz. Herhangi bir sorunla karşılaşırsanız, [bir destek bileti günlüğe](storsimple-8000-contact-microsoft-support.md)kaydedin.
> * Yazılımı ve diğer düzenli güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Yalnızca ön güncelleştirme ağ geçidi denetimi portalda başarısız olursa aygıtın Windows PowerShell arabirimine (güncelleştirmeleri yüklemek için) gitmeniz gerekir. Güncelleme yaptığınız sürüme bağlı olarak, güncelleştirmelerin yüklenmesi 4 saat (veya daha fazla) sürebilir. Bakım modu güncelleştirmeleri aygıtın Windows PowerShell arabirimi üzerinden yüklenmelidir. Bakım modu güncelleştirmeleri rahatsız edici güncelleştirmeler olduğundan, bunlar cihazınıziçin bir kesinti süresi ne olur.
> * İsteğe bağlı StorSimple Snapshot Manager'ı çalıştırıyorsanız, aygıtı güncellemeden önce Snapshot Manager sürümünüzü Update 5'e yükselttiğinizden emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-the-azure-portal"></a>Azure portalı üzerinden Güncelleştirme 5'i yükleme
Cihazınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin [5 güncelleştirin.](storsimple-update5-release-notes.md)

> [!NOTE]
> Microsoft aygıttan ek tanılama bilgileri çeker. Sonuç olarak, operasyon ekibimiz sorun olan aygıtları tanımladığında, cihazdan bilgi toplamak ve sorunları tanılamak için daha iyi donanıma sahip olacağız.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Cihazınızın **StorSimple 8000 Serisi Güncelleme 5 (6.3.9600.17845)** çalıştığını doğrulayın. **Son güncelleştirme tarihi** değiştirilmelidir.

Şimdi Bakım modu güncelleştirmelerinin kullanılabilir olduğunu göreceksiniz (bu ileti güncelleştirmeleri yükledikten sonra 24 saate kadar görüntülenmeye devam edebilir). Bakım modu güncelleştirmesini yüklemek için gereken adımlar sonraki bölümde ayrıntılı olarak açıklanır.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## <a name="install-update-5-as-a-hotfix"></a>Güncelleştirme 5'i düzeltme olarak yükleyin

Düzeltme yöntemi kullanılarak yükseltilebilen yazılım sürümleri şunlardır:

* 0.1, 0.2, 0.3'u güncelleştirin
* Güncelleştirme 1, 1.1, 1.2
* Güncelleştirme 2, 2.1, 2.2
* Güncelleme 3, 3.1
* Güncelleştirme 4

> [!NOTE] 
> Güncelleştirme 5'i yüklemek için önerilen yöntem, Güncelleştirme 3 ve sonraki sürümden güncelleştirmeye çalışırken Azure portalı üzerinden yapılır. Güncelleştirme 3'den önce sürümleri çalıştıran bir aygıtı güncellerken bu yordamı kullanın. Güncelleştirmeleri Azure portalı üzerinden yüklemeye çalışırken ağ geçidi denetiminde başarısız olursanız da bu yordamı kullanabilirsiniz. VERI 0 olmayan bir ağ arabirimine atanmış bir ağ geçidiniz olduğunda ve aygıtınız Güncelleştirme 1'den daha önce bir yazılım sürümü çalıştırdığında denetim başarısız olur.

Hotfix yöntemi aşağıdaki üç adımı içerir:

1. Düzeltmeleri Microsoft Güncelleştirme Kataloğu'ndan indirin.
2. Normal mod düzeltmelerini yükleyin ve doğrulayın.
3. Bakım modu düzeltmesini yükleyin ve doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncellemeleri karşıdan yükleme

Aşağıdaki düzeltmeleri öngörülen sırada ve önerilen klasörlere indirmeli ve yüklemeniz gerekir:

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleme|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Yazılım güncelleştirmesi<br> Hem _HcsSoftwareUpdate.exe_ ve _CisMSDAgent.exe_ indirin |Normal <br></br>Kesintisiz |~ 25 dakika |FirstOrderUpdate|

Güncelleştirme 4'i çalıştıran bir aygıttan güncelleniyorsa, işletim sistemi kümülatif güncelleştirmelerini yalnızca ikinci sipariş güncelleştirmeleri olarak yüklemeniz gerekir.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleme|
| --- | --- | --- | --- | --- | --- |
| 2a. |KB4025336 |İşletim sistemi kümülatif güncellemeler paketi <br> Windows Server 2012 R2 sürümünü indirin |Normal <br></br>Kesintisiz |- |SecondOrderUpdate|

Güncelleştirme 3 veya daha önceki bir aygıttan yükleme yle, kümülatif güncelleştirmelere ek olarak aşağıdakileri yükleyin.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yükleme|
| --- | --- | --- | --- | --- | --- |
| 2B' ye kadar. |KB4011841 <br> KB4011842 |LSI sürücü ve firmware güncellemeleri <br> USM firmware güncellemesi (sürüm 3.38) |Normal <br></br>Kesintisiz |~ 3 saat <br> (2A içerir. + 2B. + 2C.)|SecondOrderUpdate|
| 2C' ye kadar. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü indirin |Normal <br></br>Kesintisiz |- |SecondOrderUpdate|
| 2d. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |İşletim sistemi güncellemeleri paketi <br> Windows Server 2012 R2 sürümünü indirin |Normal <br></br>Kesintisiz |- |SecondOrderUpdate|


Ayrıca, önceki tablolarda gösterilen tüm güncelleştirmelerin üstüne disk firmware güncelleştirmeleri yüklemeniz gerekebilir. `Get-HcsFirmwareVersion` Cmdlet'i çalıştırarak disk firmware güncelleştirmesine ihtiyacınız olup olmadığını doğrulayabilirsiniz. Bu `XMGJ`firmware sürümlerini çalıştırıyorsanız: `VR08`, `N003` `0107` `XGEG`, `KZ50` `F6C2`, , , , , o zaman bu güncelleştirmeleri yüklemeniz gerekmez.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati | Klasöre yükleme|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Disk firmware |Bakım <br></br>Yıkıcı |~ 30 dakika | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Güncelleştirme 4'ten güncelleniyorsa, toplam yükleme süresi 4 saate yakındır.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce, hem aygıt denetleyicilerinin çevrimiçi olduğundan hem de tüm donanım bileşenlerinin sağlıklı olduğundan emin olun.

Düzeltmeleri indirmek ve yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
Güncelleme 5 [sürümü](storsimple-update5-release-notes.md)hakkında daha fazla bilgi edinin.

