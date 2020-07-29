---
title: StorSimple 8000 serisi cihazında kasa değiştirme | Microsoft Docs
description: StorSimple birincil Kasası veya EBOD Kasası için kasanın nasıl kaldırılacağını ve değiştirileceğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: de0f6299f35f9d76fdade976bf70456426e5ec51
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513430"
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>StorSimple cihazınızda kasayı değiştirme
## <a name="overview"></a>Genel Bakış
Bu öğreticide, StorSimple 8000 serisi cihazındaki bir kasanın nasıl kaldırılacağı ve değiştirileceği açıklanmaktadır. StorSimple 8100 modeli tek bir muhafaza aygıtıdır (bir kasa), ancak 8600 bir çift muhafaza aygıtıdır (iki kasa). 8600 modeli için cihazda başarısız olabilecek büyük olasılıkla iki kasa vardır: birincil kasa için kasa veya EBOD Kasası Kasası.

Her iki durumda da, Microsoft tarafından gönderilen değiştirme Kasası boştur. Güç ve soğutma modülleri (PCMs), denetleyici modülleri, katı hal disk sürücüleri (SSD), sabit disk sürücüleri (HDD 'ler) veya EBOD modülleri dahil edilir.

> [!IMPORTANT]
> Kasası kaldırmadan ve değiştirmeden önce, [StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md)içindeki güvenlik bilgilerini gözden geçirin.


## <a name="remove-the-chassis"></a>Kasası kaldırma
StorSimple cihazınızda kasayı kaldırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-remove-a-chassis"></a>Bir Kasası kaldırmak için
1. StorSimple cihazının kapatıldığından ve tüm güç kaynaklarıyla bağlantısı kesildiğinizden emin olun.
2. Varsa tüm ağ ve SAS kablolarını kaldırın.
3. Birimi rafınızdan kaldırın.
4. Sürücülerin her birini kaldırın ve kaldırıldıkları yuvaları aklınızda edin. Daha fazla bilgi için bkz. [disk sürücüsünü kaldırma](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).
5. EBOD muhafazasında (başarısız olan kasa ise), EBOD denetleyici modüllerini kaldırın. Daha fazla bilgi için bkz. [EBOD denetleyiciyi kaldırma](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).
   
    Birincil kasada (başarısız olan kase), denetleyicileri kaldırın ve kaldırıldıkları yuvaları unutmayın. Daha fazla bilgi için bkz. [denetleyiciyi kaldırma](storsimple-8000-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Kasası 'nı yükler
Kasası StorSimple cihazınıza yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-a-chassis"></a>Kasa yüklemek için
1. Kasayı rafa bağlayın. Daha fazla bilgi için bkz. [storsimple 8100 cihazınızı raf bağlama](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) veya [StorSimple 8600 cihazınızı rafa bağlama](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Kasa rafa bağlandıktan sonra, denetleyici modüllerini daha önce yüklenmiş oldukları konumlara de yüklersiniz.
3. Sürücüleri, daha önce yüklenmiş oldukları konumlara ve yuvalara yükler.
   
   > [!NOTE]
   > SSD 'leri öncelikle yuvalara yüklemeniz ve sonra HDD 'leri yüklemeniz önerilir.
  
4. Cihaza ve yüklü bileşenlere bağlı olarak, cihazınızı uygun güç kaynaklarına bağlayın ve cihazı açın. Ayrıntılar için bkz. [storsimple 8100 cihazınızı](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) veya [StorSimple 8600 cihazınızı](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device)kablosunu inceleyin.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

