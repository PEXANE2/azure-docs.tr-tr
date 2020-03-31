---
title: StorSimple 8000 serisi cihazda şasideğiştirin | Microsoft Dokümanlar
description: StorSimple birincil muhafazanız veya EBOD muhafazanız için şasinin nasıl kaldırılıp değiştirilmeye başlanması gerektiğini açıklar.
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 7dfc39f4d08c8a49d1564a0a5bd7e3ef4156e3fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61312454"
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>StorSimple cihazınızdaki şasiyi değiştirin
## <a name="overview"></a>Genel Bakış
Bu öğretici, StorSimple 8000 serisi bir cihazdaki bir şasinin nasıl kaldırılıp değiştirilmeye başladığını açıklar. StorSimple 8100 modeli tek bir kasa cihazıdır (tek kasa), 8600 ise çift kasa (iki kasa). 8600 model için cihazda arıza lanabilecek potansiyel iki şasi vardır: birincil kasa nın şasisi veya EBOD kasasının şasisi.

Her iki durumda da, Microsoft tarafından gönderilen yedek şasi boştur. Güç ve Soğutma Modülleri (PcM'ler), denetleyici modülleri, katı hal disk sürücüleri (SSD'ler), sabit disk sürücüleri (HDD'ler) veya EBOD modülleri dahil edilecektir.

> [!IMPORTANT]
> Şasiyi çıkarmadan ve değiştirmeden önce [StorSimple donanım bileşeni değişimindeki](storsimple-8000-hardware-component-replacement.md)güvenlik bilgilerini gözden geçirin.


## <a name="remove-the-chassis"></a>Şasiyi çıkarın
StorSimple cihazınızdaki şasiyi kaldırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-remove-a-chassis"></a>Bir şasiyi kaldırmak için
1. StorSimple aygıtının kapatıldığından ve tüm güç kaynaklarından bağlantısının kesildiğinden emin olun.
2. Varsa tüm ağ ve SAS kablolarını çıkarın.
3. Üniteyi raftan çıkarın.
4. Sürücülerin her birini kaldırın ve kaldırıldıkları yuvaları not alın. Daha fazla bilgi için [bkz.](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive)
5. EBOD kasası üzerinde (bu arızalı şasi ise), EBOD denetleyici modüllerini çıkarın. Daha fazla bilgi için [bkz.](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller)
   
    Birincil kasada (bu arızalanan şasi ise), denetleyicileri çıkarın ve çıkarıldıkları yuvalara dikkat edin. Daha fazla bilgi için [bkz.](storsimple-8000-controller-replacement.md#remove-a-controller)

## <a name="install-the-chassis"></a>Şasiyi töşekilde kurun
StorSimple cihazınıza şasiyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-a-chassis"></a>Bir şasi yüklemek için
1. Şasiyi rafa monte edin. Daha fazla bilgi için [StorSimple 8100 cihazınıza veya StorSimple](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) [8600 cihazınıza Rack-mount'a](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device)bakın.
2. Şasi rafa monte edildikten sonra, denetleyici modüllerini daha önce takıldığı konumlara yerleştirin.
3. Sürücüleri daha önce yüklendikleri konumlara ve yuvalara yükleyin.
   
   > [!NOTE]
   > SSD'leri önce yuvalara yüklemenizi, sonra da HDD'leri yüklemenizi öneririz.
  
4. Cihaz rafa monte edilmiş ve bileşenler takılı olduğunda, cihazınızı uygun güç kaynaklarına bağlayın ve cihazı açın. Ayrıntılar için [StorSimple 8100 cihazınızı kabloya](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) veya [StorSimple 8600 cihazınıza kablo](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device)lama ya da kablo lama.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşen değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

