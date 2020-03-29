---
title: StorSimple cihaz modunu değiştir | Microsoft Dokümanlar
description: StorSimple aygıt modlarını açıklar ve aygıt modunu değiştirmek için StorSimple için Windows PowerShell'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e55964beff48df6ce24d99c01975d39b662f1612
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60576100"
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>StorSimple cihazınızdaki aygıt modunu değiştirme

Bu makalede, StorSimple cihazınızın çalışabileceği çeşitli modların kısa bir açıklaması yer alabilirsiniz. StorSimple aygıtınız üç modda çalışabilir: normal, bakım ve kurtarma.

Bu makaleyi okuduktan sonra, bilecek:

* StorSimple aygıt modları nedir
* StorSimple cihazının hangi modda olduğunu nasıl anlayabilirsiniz?
* Normalden bakım moduna nasıl *değiştirilir?*

Yukarıdaki yönetim görevleri yalnızca StorSimple cihazınızın Windows PowerShell arabirimi üzerinden gerçekleştirilebilir.

## <a name="about-storsimple-device-modes"></a>StorSimple cihaz modları hakkında

StorSimple aygıtınız normal, bakım veya kurtarma modunda çalışabilir. Bu modların her biri aşağıda kısaca açıklanmıştır.

### <a name="normal-mode"></a>Normal mod

Bu, tam olarak yapılandırılmış bir StorSimple aygıtı için normal çalışma modu olarak tanımlanır. Varsayılan olarak, cihazınız normal modda olmalıdır.

### <a name="maintenance-mode"></a>Bakım modu

Bazen StorSimple aygıtının bakım moduna alınması gerekebilir. Bu mod, aygıtta bakım yapmanızı ve disk firmware ile ilgili olanlar gibi yıkıcı güncelleştirmeler yüklemenize olanak tanır.

Sistemi yalnızca StorSimple için Windows PowerShell üzerinden bakım moduna geçirebilirsiniz. Tüm G/Ç istekleri bu modda duraklatıldı. Geçici olmayan rasgele erişim belleği (NVRAM) veya kümeleme hizmeti gibi hizmetler de durdurulur. Bu moda girdiğinizde veya çıktığınızda her iki denetleyici de yeniden başlatılır. Bakım modundan çıktığınızda, tüm hizmetler devam edecektir ve sağlıklı olmalıdır. Bu birkaç dakika sürebilir.

> [!NOTE]
> **Bakım modu yalnızca düzgün çalışan bir aygıtta desteklenir. Denetleyicilerden birinin veya her ikisinin çalışmadığı bir aygıtta desteklenmez.**


### <a name="recovery-mode"></a>Kurtarma modu

Kurtarma modu "Ağ destekli Windows Güvenli Modu" olarak tanımlanabilir. Kurtarma modu, Microsoft Destek ekibini meşgul eder ve sistem üzerinde tanılama gerçekleştirmelerine olanak tanır. Kurtarma modunun birincil amacı sistem günlüklerini almaktır.

Sisteminiz kurtarma moduna geçerse, sonraki adımlar için Microsoft Destek'e başvurmalısınız. Daha fazla bilgi için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

> [!NOTE]
> **Aygıtı kurtarma moduna yerleştiremezsiniz. Aygıt kötü durumdaysa, kurtarma modu aygıtı Microsoft Destek personelinin inceleyebileceği bir duruma sokmaya çalışır.**

## <a name="determine-storsimple-device-mode"></a>StorSimple cihaz modunu belirleme

#### <a name="to-determine-the-current-device-mode"></a>Geçerli aygıt modunu belirlemek için

1. Cihaz seri konsoluna bağlanmak için [PuTTY'yi kullan](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)adımlarını izleyerek aygıt seri konsoluna oturum açın.
2. Aygıtın seri konsol menüsündeki banner iletisine bakın. Bu ileti, aygıtın bakım veya kurtarma modunda olup olmadığını açıkça gösterir. İleti, sistem moduyla ilgili belirli bir bilgi içermiyorsa, aygıt normal moddadır.

## <a name="change-the-storsimple-device-mode"></a>StorSimple aygıt modunu değiştirme

Bakım yapmak veya bakım modu güncelleştirmelerini yüklemek için StorSimple cihazını bakım moduna (normal moddan) yerleştirebilirsiniz. Bakım moduna girmek veya çıkmak için aşağıdaki yordamları gerçekleştirin.

> [!IMPORTANT]
> Bakım moduna girmeden önce, Azure portalındaki **cihazınızın Aygıt ayarları na > Donanım sağlığına** erişerek her iki aygıt denetleyicisinin de sağlıklı olduğunu doğrulayın. Denetleyicilerden biri veya her ikisi de sağlıklı değilse, sonraki adımlar için Microsoft Destek'e başvurun. Daha fazla bilgi için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
 

#### <a name="to-enter-maintenance-mode"></a>Bakım moduna girmek için

1. Cihaz seri konsoluna bağlanmak için [PuTTY'yi kullan](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)adımlarını izleyerek aygıt seri konsoluna oturum açın.
2. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.** İstendiğinde, **aygıt yöneticisi parolasını**girin. Varsayılan parola: `Password1`.
3. Komut isteminde, 
   
    `Enter-HcsMaintenanceMode`
4. Bakım modunun tüm G/Ç isteklerini bozacağını ve Azure portalına olan bağlantıyı keseceğini belirten bir uyarı iletisi görürsünüz ve onay için sizden istenir. Bakım moduna girmek için **Y** yazın.
5. Her iki denetleyici de yeniden başlatılacaktır. Yeniden başlatma tamamlandığında, seri konsol başlığı aygıtın bakım modunda olduğunu gösterir. Örnek çıktı aşağıda gösterilmiştir.

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="to-exit-maintenance-mode"></a>Bakım modundan çıkmak için

1. Aygıt seri konsolunda oturum açın. Banner iletisinden cihazınızın bakım modunda olduğunu doğrulayın.
2. Komut istemine şunları yazın:
   
    `Exit-HcsMaintenanceMode`
3. Bir uyarı iletisi ve onay iletisi görüntülenir. Bakım modundan çıkmak için **Y** yazın.
4. Her iki denetleyici de yeniden başlatılacaktır. Yeniden başlatma tamamlandığında, seri konsol başlığı aygıtın normal modda olduğunu gösterir. Örnek çıktı aşağıda gösterilmiştir.

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure to install on each controller could result in data corruption. Exiting maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to exit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a>Sonraki adımlar

StorSimple cihazınızda [normal ve bakım modu güncelleştirmelerini](storsimple-update-device.md) nasıl uygulayacağınızı öğrenin.

