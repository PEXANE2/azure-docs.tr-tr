---
title: StorSimple 8000 serisi cihazında güncelleştirme 4 ' ü yükler | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 4 ' ün StorSimple 8000 serisi cihazınıza nasıl yükleneceğini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: ed6f9d58c5c54c88acf8e3a0e7fda7d2d65b8637
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85514326"
---
# <a name="install-update-4-on-your-storsimple-device"></a>StorSimple cihazınıza güncelleştirme 4 ' ü yükler

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Azure portal ve düzeltme yöntemi aracılığıyla önceki bir yazılım sürümünü çalıştıran bir StorSimple cihazına güncelleştirme 4 ' ün nasıl yükleneceği açıklanmaktadır. Düzeltme yöntemi, StorSimple cihazının 0 ' ından farklı bir ağ arabiriminde yapılandırıldığında ve güncelleştirme öncesi 1 yazılım sürümünden güncelleştirme yapmaya çalıştığınızda kullanılır.

Güncelleştirme 4, cihaz yazılımı, USMMı üretici yazılımı, LSI sürücü ve üretici yazılımı, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmeleri ve diğer işletim sistemi güncelleştirmelerinin bir ana bilgisayarı içerir.  Cihaz yazılımı, USı üretici yazılımı, Spaceport, Storport ve diğer işletim sistemi güncelleştirmeleri, kesintiye uğramamış güncelleştirmelerdir. Kesintiye uğramamış veya düzenli güncelleştirmeler Azure portal veya düzeltme yöntemi aracılığıyla uygulanabilir. Disk üretici yazılımı güncelleştirmeleri, kesintiye uğratan güncelleştirmelerdir ve yalnızca cihazın Windows PowerShell arabirimi kullanılarak düzeltme yöntemi aracılığıyla uygulanabilir.

> [!IMPORTANT]
> * Donanım durumu ve ağ bağlantısı açısından cihaz sistem durumunu tespit etmek için, yüklemeden önce bir el ile ve otomatik denetimler kümesi yapılır. Bu ön denetimler yalnızca Azure portal güncelleştirmeleri uyguladığınızda gerçekleştirilir.
> * Azure portal aracılığıyla yazılımı ve diğer normal güncelleştirmeleri yüklemenizi öneririz. Portalda güncelleştirme öncesi ağ geçidi denetimi başarısız olursa, yalnızca cihazın (güncelleştirmeleri yüklemek için) Windows PowerShell arabirimine gitmeniz gerekir. Güncelleştirme yaptığınız sürüme bağlı olarak, güncelleştirmelerin yüklenmesi 4 saat (veya daha fazla) sürebilir. Bakım modu güncelleştirmeleri de cihazın Windows PowerShell arabirimi aracılığıyla yüklenmelidir. Bakım modu güncelleştirmeleri kesintiye uğratan güncelleştirmeler olduğundan, bu, cihazınız için bir süre boyunca zaman kaybına neden olur.
> * İsteğe bağlı StorSimple Snapshot Manager çalıştırıyorsanız, cihazı güncelleştirmeden önce Snapshot Manager sürümünüzü güncelleştirme 4 ' e yükseltdiğinizden emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-the-azure-portal"></a>Azure portal aracılığıyla güncelleştirme 4 ' ü yükler
Cihazınızı [güncelleştirme 4](storsimple-update4-release-notes.md)' e güncelleştirmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Microsoft cihazdan ek tanılama bilgileri çeker. Sonuç olarak, işletim ekibimiz sorun yaşayan cihazları tanımlarsa, cihazdan bilgi toplamak ve sorunları tanılamak için daha iyi bir şekilde donatılmıştır. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Cihazınızın **StorSimple 8000 serisi güncelleştirme 4 (6.3.9600.17820)** çalıştığını doğrulayın. **Son güncelleme tarihi** de değiştirilmelidir.

* Artık bakım modu güncelleştirmelerinin kullanılabildiğini görürsünüz (Bu ileti, güncelleştirmeleri yükledikten sonra 24 saate kadar görüntülenmeye devam edebilir). Bakım modu güncelleştirmeleri, cihaz kapalı kalma süresi ile sonuçlanan ve yalnızca cihazınızın Windows PowerShell arabirimi aracılığıyla uygulanabilen, kesintiye uğratan güncelleştirmelerdir.

* Bakım modu güncelleştirmelerini, KB4011837 aramak ve indirmek üzere [düzeltmeleri indirmek](#to-download-hotfixes) için ' de listelenen adımları kullanarak, disk üretici yazılımı güncelleştirmelerini yükleyen (diğer güncelleştirmeler şu anda zaten yüklü olmalıdır). Bakım modu güncelleştirmelerini yüklemek için, [bakım modu düzeltmelerini yüklemek ve doğrulamak](#to-install-and-verify-maintenance-mode-hotfixes) için listelenen adımları izleyin.

## <a name="install-update-4-as-a-hotfix"></a>Güncelleştirme 4 ' ü bir düzeltme olarak yükler
Güncelleştirme 4 ' ü yüklemek için önerilen yöntem Azure portal aracılığıyla yapılır.

Azure portal aracılığıyla güncelleştirmeleri yüklemeye çalışırken ağ geçidi denetimi başarısız olursa bu yordamı kullanın. VERI olmayan 0 ağ arabirimine atanmış bir ağ geçidiniz varsa ve cihazınız güncelleştirme 1 ' den önceki bir yazılım sürümü çalıştırıyorsa, Denetim başarısız olur.

Düzeltme yöntemi kullanılarak yükseltilemeyen yazılım sürümleri şunlardır:

* Güncelleştirme 0,1, 0,2, 0,3
* Güncelleştirme 1, 1,1, 1,2
* Güncelleştirme 2, 2,1, 2,2
* Güncelleştirme 3, 3,1


Düzeltme yöntemi aşağıdaki üç adımdan oluşur:

1. Microsoft Update kataloğundan düzeltmeleri indirin.
2. Normal mod düzeltmelerini yükleyip doğrulayın.
3. Bakım modu düzeltmesini yükleyip doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncelleştirmeleri indirin

Aşağıdaki düzeltmeleri önceden belirlenmiş sıraya ve önerilen klasörlere indirmeniz ve kurmanız gerekir:

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati |Klasöre yüklensin|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Yazılım güncelleştirmesi |Normal <br></br>Kesintiye uğramamış |~ 25 dakika |FirstOrderUpdate|
| 2A. |KB4011841 <br> KB4011842 |LSI sürücü ve bellenim güncelleştirmeleri <br> USMBELLENIM Güncelleştirmesi (sürüm 3,38) |Normal <br></br>Kesintiye uğramamış |~ 3 saat <br> (2A 'yı içerir. + 2B. + 2C.)|SecondOrderUpdate|
| 7.. |KB3139398, KB3108381 <br> KB3205400, KB3142030 <br> KB3197873, KB3197873 <br> KB3192392, KB3153704 <br> KB3174644, KB3139914  |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 'yi indirin |Normal <br></br>Kesintiye uğramamış |- |SecondOrderUpdate|
| 2C. |KB3210083, KB3103616 <br> KB3146621, KB3121261 <br> KB3123538 |İşletim sistemi güncelleştirmeleri paketi <br> Windows Server 2012 R2 'yi indirin |Normal <br></br>Kesintiye uğramamış |- |SecondOrderUpdate|

Ayrıca, önceki tablolarda gösterilen tüm güncelleştirmelerin üzerine disk üretici yazılımı güncelleştirmelerini yüklemeniz gerekebilir. Cmdlet 'ini çalıştırarak, disk üretici yazılımı güncelleştirmelerine ihtiyacınız olup olmadığını doğrulayabilirsiniz `Get-HcsFirmwareVersion` . Bu bellenim sürümlerini çalıştırıyorsanız:,,,,,,,,,,,,,,,, `XMGJ` `XGEG` `KZ50` `F6C2` `VR08` `N002` `0106` Bu güncelleştirmeleri yüklemeniz gerekmez.

| Sipariş verme | KB | Açıklama | Güncelleştirme türü | Yükleme saati | Klasöre yüklensin|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Disk üretici yazılımı |Bakım <br></br>Kesintiye uğratan |~ 30 dakika | Üçüncüorderupdate |

<br></br>

> [!IMPORTANT]
> * Bu yordamın, güncelleştirme 4 ' ü uygulamak için yalnızca bir kez gerçekleştirilmesi gerekir. Sonraki güncelleştirmeleri uygulamak için Azure portal kullanabilirsiniz.
> * Güncelleştirme 3 veya 3,1 ' den güncelleştirme yaptıysanız toplam yüklemesi süresi 4 saate yakın olur.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce, her iki cihaz denetleyicisinin de çevrimiçi olduğundan ve tüm donanım bileşenlerinin sağlıklı olduğundan emin olun.

Düzeltmeleri indirmek ve yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
[Güncelleştirme 4 sürümü](storsimple-update4-release-notes.md)hakkında daha fazla bilgi edinin.

