---
title: StorSimple cihazınızı güncelleyin | Microsoft Dokümanlar
description: Düzenli ve bakım modu güncelleştirmelerini ve düzeltmeleri yüklemek için StorSimple güncelleştirme özelliğinin nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: carmonm
editor: ''
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/23/2018
ms.author: twooley
ms.openlocfilehash: c9451afaefdd220b5f87d4650c7844f06926b03a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933430"
---
# <a name="update-your-storsimple-8000-series-device"></a>StorSimple 8000 Serisi cihazınızı güncelleyin
> [!NOTE]
> StorSimple için klasik portal kullanım dışıdır. StorSimple Cihaz Yöneticileriniz, yeni Azure portalına kullanımdan kaldırma zamanlamasına göre otomatik olarak taşınacaktır. Bu taşımayla ilgili bir e-posta ve portal bildirimi alacaksınız. Bu belge de yakında kullanımdan kaldırılacaktır. Taşıma hakkında tüm sorularınız için bkz. [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
StorSimple güncellemeleri özellikleri, StorSimple cihazınızı kolayca güncel tutmanızı sağlar. Güncelleştirme türüne bağlı olarak, güncelleştirmeleri Azure klasik portalı üzerinden veya Windows PowerShell arabirimi üzerinden aygıta uygulayabilirsiniz. Bu öğretici, güncelleştirme türlerini ve bunların her birinin nasıl yüklenirlerini açıklar.

İki tür aygıt güncelleştirmesi uygulayabilirsiniz: 

* Normal (veya Normal mod) güncelleştirmeleri
* Bakım modu güncellemeleri

Azure klasik portalı veya Windows PowerShell üzerinden düzenli güncelleştirmeler yükleyebilirsiniz; ancak, Bakım modu güncelleştirmelerini yüklemek için Windows PowerShell'i kullanmanız gerekir. 

Her güncelleştirme türü aşağıda ayrı olarak açıklanmıştır.

### <a name="regular-updates"></a>Düzenli güncellemeler
Düzenli güncelleştirmeler, aygıt Normal moddayken yüklenebilecek kesintisiz güncelleştirmelerdir. Bu güncelleştirmeler, Microsoft Update web sitesi aracılığıyla her aygıt denetleyicisine uygulanır. 

> [!IMPORTANT]
> Güncelleştirme işlemi sırasında bir denetleyici nin başarısız oluşabilir. Ancak, bu sistem kullanılabilirliğini veya çalışmasını etkilemez.
> 
> 

* Azure klasik portalı üzerinden düzenli güncelleştirmelerin nasıl yüklenir hakkında ayrıntılı bilgi için Bkz. [Azure klasik portalı üzerinden düzenli güncelleştirmeleri yükle.](#install-regular-updates-via-the-azure-classic-portal)
* StorSimple için Windows PowerShell üzerinden düzenli güncelleştirmeler de yükleyebilirsiniz. Ayrıntılar için, [StorSimple için Windows PowerShell üzerinden düzenli güncelleştirmeleri yükle'ye](#install-regular-updates-via-windows-powershell-for-storsimple)bakın.

### <a name="maintenance-mode-updates"></a>Bakım modu güncellemeleri
Bakım Modu güncelleştirmeleri, disk firmware yükseltmeleri gibi rahatsız edici güncelleştirmelerdir. Bu güncelleştirmeler, aygıtın Bakım moduna alınmasını gerektirir. Ayrıntılar için Bkz. [Adım 2: Bakım modunu girin.](#step2) Bakım modu güncelleştirmelerini yüklemek için Azure klasik portalını kullanamazsınız. Bunun yerine, StorSimple için Windows PowerShell kullanmanız gerekir. 

Bakım modu güncelleştirmelerini nasıl yükleyeceklerine ilişkin ayrıntılar için, [StorSimple için Windows PowerShell üzerinden Bakım modu güncelleştirmelerini yükle'ye](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple)bakın.

> [!IMPORTANT]
> Bakım modu güncelleştirmeleri her denetleyiciye ayrı olarak uygulanmalıdır. 
> 
> 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Azure klasik portalı üzerinden düzenli güncelleştirmeler yükleme
StorSimple cihazınıza güncelleştirmeleri uygulamak için Azure klasik portalını kullanabilirsiniz.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden düzenli güncelleştirmeler yükleyin
Alternatif olarak, düzenli (Normal mod) güncelleştirmeleri uygulamak için StorSimple için Windows PowerShell'i kullanabilirsiniz.

> [!IMPORTANT]
> StorSimple için Windows PowerShell'i kullanarak düzenli güncelleştirmeler yükleyebiliyor olsanız da, Azure klasik portalı üzerinden düzenli güncelleştirmeler yüklemenizi şiddetle öneririz. Güncelleştirme 1'den başlayarak, portaldan güncelleştirmeleri yüklemeden önce ön denetimler yapılır. Bu ön kontroller hataları önleyecek ve daha sorunsuz bir deneyim sağlayacaktır. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden Bakım modu güncelleştirmelerini yükleyin
StorSimple cihazınıza Bakım modu güncelleştirmelerini uygulamak için StorSimple için Windows PowerShell'i kullanırsınız. Tüm G/Ç istekleri bu modda duraklatıldı. Geçici olmayan rasgele erişim belleği (NVRAM) veya kümeleme hizmeti gibi hizmetler de durdurulur. Bu moda girdiğinizde veya çıktığınızda her iki denetleyici de yeniden başlatılır. Bu moddan çıktığınızda, tüm hizmetler devam edecek ve sağlıklı olmalıdır. (Bu işlem birkaç dakika sürebilir.)

Bakım modu güncelleştirmelerini uygulamanız gerekiyorsa, Azure klasik portalı aracılığıyla yüklenmesi gereken güncelleştirmelere sahip olduğunuza dair bir uyarı alırsınız. Bu uyarı, güncelleştirmeleri yüklemek için StorSimple için Windows PowerShell'i kullanma yönergelerini içerir. Cihazınızı güncelledikten sonra, aygıtı Normal mod olarak değiştirmek için aynı yordamı kullanın. Adım adım talimatlar için [bkz.](#step4)

> [!IMPORTANT]
> * Bakım moduna girmeden önce, Azure klasik portalındaki **Bakım** sayfasındaki **Donanım Durumunu** kontrol ederek her iki aygıt denetleyicisinin de sağlıklı olduğunu doğrulayın. Denetleyici sağlıklı değilse, sonraki adımlar için Microsoft Destek'e başvurun. Daha fazla bilgi için Microsoft Destek'e başvurun. 
> * Bakım modundayken, güncelleştirmeyi önce bir denetleyiciye, sonra da diğer denetleyiciye uygulamanız gerekir.
> 
> 

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Adım 1: Seri konsoluna bağlanın<a name="step1">
İlk olarak, seri konsoluna erişmek için PuTTY gibi bir uygulama kullanın. Aşağıdaki yordam, seri konsoluna bağlanmak için PuTTY'nin nasıl kullanılacağını açıklar.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Adım 2: Bakım moduna girin<a name="step2">
Konsola bağlandıktan sonra, yüklenmesi gereken güncelleştirmeler olup olmadığını belirleyin ve bunları yüklemek için Bakım moduna girin.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Adım 3: Güncelleştirmelerinizi yükleyin<a name="step3">
Ardından, güncelleştirmelerinizi yükleyin.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Adım 4: Çıkış Bakım modu<a name="step4">
Son olarak, Çıkış Bakım modu.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden düzeltmeleri yükleyin
Microsoft Azure StorSimple güncelleştirmelerinin aksine, ortak bir klasörden düzeltmeler yüklenir. Güncelleştirmelerde olduğu gibi, iki tür düzeltme vardır: 

* Düzenli düzeltmeler 
* Bakım modu düzeltmeleri  

Aşağıdaki yordamlar, düzenli ve Bakım modu düzeltmeleri yüklemek için StorSimple için Windows PowerShell'in nasıl kullanılacağını açıklar.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Aygıtın fabrikada sıfırlamasını gerçekleştirirseniz güncelleştirmelere ne olur?
Bir aygıt fabrika ayarlarına sıfırlanırsa, tüm güncelleştirmeler kaybolur. Fabrika sıfırlama aygıtı kaydedildikten ve yapılandırıldıktan sonra, StorSimple için Azure klasik portalı ve/veya Windows PowerShell üzerinden güncelleştirmeleri el ile yüklemeniz gerekir. Fabrika sıfırlama hakkında daha fazla bilgi için [bkz.](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings)

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple cihazınızı yönetmek için StorSimple için Windows PowerShell'i kullanma](storsimple-windows-powershell-administration.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Manager hizmetini kullanma](storsimple-manager-service-administration.md)hakkında daha fazla bilgi edinin.

