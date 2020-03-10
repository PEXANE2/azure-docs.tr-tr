---
title: StorSimple 8000 serisi cihazına güncelleştirme 5 ' i yükler | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 5 ' in StorSimple 8000 serisi cihazınıza nasıl yükleneceğini açıklar.
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
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78394206"
---
# <a name="install-update-5-on-your-storsimple-device"></a>StorSimple cihazınıza güncelleştirme 5 ' i yükledikten sonra

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Azure portal aracılığıyla önceki bir yazılım sürümünü çalıştıran bir StorSimple cihazına güncelleştirme 5 ' in yüklenmesi ve düzeltme yönteminin kullanılması açıklanmaktadır. Güncelleştirme 5 ' i güncelleştirme 3 sürümlerini çalıştıran bir cihaza yüklemeye çalışırken düzeltme yöntemi kullanılır. Düzeltme Yöntemi ayrıca, StorSimple cihazının 0 ' ından farklı bir ağ arabiriminde yapılandırıldığında ve güncelleştirme öncesi 1 yazılım sürümünden güncelleştirme yapmaya çalıştığınızda da kullanılır.

Güncelleştirme 5 cihaz yazılımı, Storport ve Spaceport, işletim sistemi güvenlik güncelleştirmeleri ve işletim sistemi güncelleştirmeleri ve disk üretici yazılımı güncelleştirmeleri içerir.  Cihaz yazılımı, Spaceport, Storport, güvenlik ve diğer işletim sistemi güncelleştirmeleri, kesintiye uğramamış güncelleştirmelerdir. Kesintiye uğramamış veya düzenli güncelleştirmeler Azure portal veya düzeltme yöntemi aracılığıyla uygulanabilir. Disk üretici yazılımı güncelleştirmeleri, kesintiye uğramalardır ve cihaz bakım modundayken cihazın Windows PowerShell arabirimi kullanılarak düzeltme yöntemi aracılığıyla uygulanır.

> [!IMPORTANT]
> * Güncelleştirme 5, zorunlu bir güncelleştirmedir ve hemen yüklenmesi gerekir. Daha fazla bilgi için bkz. [güncelleştirme 5 sürüm notları](storsimple-update5-release-notes.md).
> * Donanım durumu ve ağ bağlantısı açısından cihaz sistem durumunu tespit etmek için, yüklemeden önce bir el ile ve otomatik denetimler kümesi yapılır. Bu ön denetimler yalnızca Azure portal güncelleştirmeleri uyguladığınızda gerçekleştirilir.
> * Güncelleştirme 3 ' ten önceki sürümleri çalıştıran bir cihazı güncelleştirirken, güncelleştirmeleri düzeltme yöntemini kullanarak yüklediğimizi önemle öneririz. Herhangi bir sorunla karşılaşırsanız, [bir destek bileti kaydedin](storsimple-8000-contact-microsoft-support.md).
> * Azure portal aracılığıyla yazılımı ve diğer normal güncelleştirmeleri yüklemenizi öneririz. Portalda güncelleştirme öncesi ağ geçidi denetimi başarısız olursa, yalnızca cihazın (güncelleştirmeleri yüklemek için) Windows PowerShell arabirimine gitmeniz gerekir. Güncelleştirme yaptığınız sürüme bağlı olarak, güncelleştirmelerin yüklenmesi 4 saat (veya daha fazla) sürebilir. Bakım modu güncelleştirmelerinin, cihazın Windows PowerShell arabirimi aracılığıyla yüklenmesi gerekir. Bakım modu güncelleştirmeleri kesintiye uğratan güncelleştirmeler olduğundan, bu, cihazınız için bir süre aşağı doğru sonuçlanır.
> * İsteğe bağlı StorSimple Snapshot Manager çalıştırıyorsanız, cihazı güncelleştirmeden önce Snapshot Manager sürümünüzü güncelleştirme 5 ' e yükseltdiğinizden emin olun.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-the-azure-portal"></a>Azure portal aracılığıyla güncelleştirme 5 ' i yükler
Cihazınızı [güncelleştirme 5](storsimple-update5-release-notes.md)' e güncelleştirmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Microsoft cihazdan ek tanılama bilgileri çeker. Sonuç olarak, işletim ekibimiz sorun yaşayan cihazları tanımlarsa, cihazdan bilgi toplamak ve sorunları tanılamak için daha iyi bir şekilde donatılmıştır.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Cihazınızın **StorSimple 8000 serisi güncelleştirme 5 (6.3.9600.17845)** çalıştığını doğrulayın. **Son güncelleme tarihi** değiştirilmelidir.

Artık bakım modu güncelleştirmelerinin kullanılabildiğini görürsünüz (Bu ileti, güncelleştirmeleri yükledikten sonra 24 saate kadar görüntülenmeye devam edebilir). Bakım modu güncelleştirmesini yüklemek için gereken adımlar, sonraki bölümde ayrıntılıdır.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## <a name="install-update-5-as-a-hotfix"></a>Güncelleştirme 5 ' i bir düzeltme olarak yükler

Düzeltme yöntemi kullanılarak yükseltilemeyen yazılım sürümleri şunlardır:

* Güncelleştirme 0,1, 0,2, 0,3
* Güncelleştirme 1, 1,1, 1,2
* Güncelleştirme 2, 2,1, 2,2
* Güncelleştirme 3, 3,1
* Güncelleştirme 4

> [!NOTE] 
> Güncelleştirme 5 ' ü yüklemek için önerilen yöntem, güncelleştirme 3 ' ten ve sonraki sürümden güncelleştirilmeye çalışıldığında Azure portal aracılığıyla yapılır. Güncelleştirme 3 ' ten önceki sürümleri çalıştıran bir cihaz güncelleştirilirken bu yordamı kullanın. Ayrıca, Azure portal aracılığıyla güncelleştirmeleri yüklemeye çalışırken ağ geçidi denetimi başarısız olursa da bu yordamı kullanabilirsiniz. VERI olmayan 0 ağ arabirimine atanmış bir ağ geçidiniz varsa ve cihazınız güncelleştirme 1 ' den önceki bir yazılım sürümünü çalıştırıyorsa, Denetim başarısız olur.

Düzeltme yöntemi aşağıdaki üç adımdan oluşur:

1. Microsoft Update kataloğundan düzeltmeleri indirin.
2. Normal mod düzeltmelerini yükleyip doğrulayın.
3. Bakım modu düzeltmesini yükleyip doğrulayın.

#### <a name="download-updates-for-your-device"></a>Cihazınız için güncelleştirmeleri indirin

Aşağıdaki düzeltmeleri önceden belirlenmiş sıraya ve önerilen klasörlere indirmeniz ve kurmanız gerekir:

| Sipariş verme | MAKALESININ | Açıklama | Güncelleştirme türü | Yüklemesi saati |Klasöre yüklensin|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Yazılım güncelleştirmesi<br> Hem _Hcssoftwareupdate. exe_ hem de _cımsdagent. exe_ ' yi indirin |Aralıklarla <br></br>Kesintiye uğramamış |~ 25 dakika |FirstOrderUpdate|

Güncelleştirme 4 çalıştıran bir cihazdan güncelleştirme yapıyorsanız, işletim sistemi toplu güncelleştirmelerini yalnızca ikinci sipariş güncelleştirmeleri olarak yüklemeniz gerekir.

| Sipariş verme | MAKALESININ | Açıklama | Güncelleştirme türü | Yüklemesi saati |Klasöre yüklensin|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |İşletim sistemi toplu güncelleştirmeler paketi <br> Windows Server 2012 R2 sürümünü indirin |Aralıklarla <br></br>Kesintiye uğramamış |- |SecondOrderUpdate|

Güncelleştirme 3 veya önceki bir sürümünü çalıştıran bir cihazdan yüklüyorsanız, toplu güncelleştirmelere ek olarak aşağıdakileri de yüklemeniz gerekir.

| Sipariş verme | MAKALESININ | Açıklama | Güncelleştirme türü | Yüklemesi saati |Klasöre yüklensin|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |LSI sürücü ve bellenim güncelleştirmeleri <br> USMBELLENIM Güncelleştirmesi (sürüm 3,38) |Aralıklarla <br></br>Kesintiye uğramamış |~ 3 saat <br> (2A 'yı içerir. + 2B. + 2C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |İşletim sistemi güvenlik güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü indirin |Aralıklarla <br></br>Kesintiye uğramamış |- |SecondOrderUpdate|
| 2B. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |İşletim sistemi güncelleştirmeleri paketi <br> Windows Server 2012 R2 sürümünü indirin |Aralıklarla <br></br>Kesintiye uğramamış |- |SecondOrderUpdate|


Ayrıca, önceki tablolarda gösterilen tüm güncelleştirmelerin üzerine disk üretici yazılımı güncelleştirmelerini yüklemeniz gerekebilir. `Get-HcsFirmwareVersion` cmdlet 'ini çalıştırarak disk üretici yazılımı güncelleştirmelerine ihtiyacınız olup olmadığını doğrulayabilirsiniz. Bu bellenim sürümlerini çalıştırıyorsanız: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, bu güncelleştirmeleri yüklemeniz gerekmez.

| Sipariş verme | MAKALESININ | Açıklama | Güncelleştirme türü | Yüklemesi saati | Klasöre yüklensin|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Disk üretici yazılımı |Bakım <br></br>Kesintiye uğratan |~ 30 dakika | Üçüncüorderupdate |

<br></br>

> [!IMPORTANT]
> * Güncelleştirme 4 ' ten güncelleştirme yapıyorsanız, toplam yüklemenin süresi 4 saate kadar yakındır.
> * Güncelleştirmeyi uygulamak için bu yordamı kullanmadan önce, her iki cihaz denetleyicisinin de çevrimiçi olduğundan ve tüm donanım bileşenlerinin sağlıklı olduğundan emin olun.

Düzeltmeleri indirmek ve yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar
[Güncelleştirme 5 sürümü](storsimple-update5-release-notes.md)hakkında daha fazla bilgi edinin.

