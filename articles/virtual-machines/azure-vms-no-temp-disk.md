---
title: Yerel geçici disk olmadan SSS Azure VM boyutları
description: Bu makalede, yerel geçici diski olmayan Microsoft Azure VM boyutları hakkında sık sorulan soruların (SSS) yanıtları sağlanmaktadır.
author: brbell
ms.service: virtual-machines
ms.topic: article
ms.author: brbell
ms.reviewer: mimckitt
ms.date: 06/15/2020
ms.openlocfilehash: 502883a0c4eb36be9e89ba8693f4d32ba7f16ebd
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088878"
---
# <a name="azure-vm-sizes-with-no-local-temporary-disk"></a>Azure VM boyutları yerel geçici disk yok 
Bu makalede, yerel geçici disk (yerel geçici disk yok) olmayan Azure VM boyutları hakkında sık sorulan soruların (SSS) yanıtları sağlanmaktadır. Bu VM boyutları hakkında daha fazla bilgi için bkz. [dv4 and Dsv4-Series (genel amaçlı Iş yükleri) Için belirtimler](dv4-dsv4-series.md) veya [Ev4 ve Esv4 serisi (bellek Için iyileştirilmiş Iş yükleri) özellikleri](ev4-esv4-series.md).

> [!IMPORTANT]
> Dv4, Dsv4, Ev4 ve Esv4 serisi VM boyutları artık genel önizlemede. Genel önizlemeye kaydolmak için bu [formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u)doldurun. 

## <a name="what-does-no-local-temp-disk-mean"></a>Yerel geçici disk ne anlama geliyor? 
Geleneksel olarak, küçük bir yerel disk (örneğin, D: sürücü) içeren VM boyutları (örn. Standard_D2s_v3, Standard_E48_v3) vardı. Artık bu yeni VM boyutlarıyla, küçük yerel disk artık yok; Ancak, hala Standart HDD, Premium SSD veya Ultra SSD iliştirebilirsiniz.

## <a name="what-if-i-still-want-a-local-temp-disk"></a>Yine de yerel bir geçici disk istiyorum ne olursa?
İş yükünüz yerel bir geçici disk gerektiriyorsa, yeni [Ddv4 ve Ddsv4](ddv4-ddsv4-series.md) ya da [Edv4 ve Edsv4](edv4-edsv4-series.md) VM boyutlarıdır. Bu boyutlar, önceki v3 boyutları ile karşılaştırıldığında %50 daha büyük geçici disk sunmaktadır.

> [!NOTE]
> Yerel geçici disk kalıcı değil; verilerinizin kalıcı olduğundan emin olmak için lütfen Standart HDD, Premium SSD veya Ultra SSD seçeneklerini kullanın. 

## <a name="what-are-the-differences-between-these-new-vm-sizes-and-the-general-purpose-dv3dsv3-or-the-memory-optimized-ev3esv3-vm-sizes-that-i-am-used-to"></a>Bu yeni VM boyutları ve Genel Amaçlı Dv3/Dsv3 ya da kullandığım bellek için Iyileştirilmiş Ev3/Esv3 VM boyutları arasındaki farklar nelerdir? 
| Yerel geçici disk ile v3 VM aileleri   | Yerel geçici disk ile yeni v4 aileleri | Yerel geçici diski olmayan yeni v4 aileleri |
|---|---|---|
| Dv3   | Ddv4 | Dv4 |
| Dsv3 | Ddsv4  | Dsv4 |
| Ev3   | Edv4  | Ev4 |
| Esv3 | Edsv4 |    Esv4 | 

## <a name="can-i-resize-a-vm-size-that-has-a-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>Yerel bir geçici diskine sahip bir VM boyutunu yerel geçici disk olmadan bir VM boyutuna yeniden boyutlandırabilir miyim?  
Hayır. Yeniden boyutlandırma için izin verilen tek birleşimler şunlardır: 

1. VM (yerel geçici disk ile)-> VM (yerel geçici disk ile); ' 
2. VM (yerel geçici disk olmadan)-> VM (yerel geçici disk olmadan). 

> [!NOTE]
> Bir görüntü kaynak diskine bağımlıysa veya yerel geçici diskte bir disk belleği dosyası ya da takas dosyası varsa disksiz görüntüler çalışmaz; bunun yerine ' disk ' ile ' alternatif kullanın. 

## <a name="do-these-vm-sizes-support-both-linux-and-windows-operating-systems-os"></a>Bu sanal makine boyutları hem Linux hem de Windows Işletim sistemlerini (OS) destekler mi?
Evet.

## <a name="will-this-break-my-custom-scripts-custom-images-or-os-images-that-have-scratch-files-or-page-files-on-a-local-temp-disk"></a>Bu, özel betiklerimi, özel görüntüleri veya yerel bir geçici diskte bulunan dosya veya sayfa dosyalarını içeren işletim sistemi görüntülerini keser mı?
Özel işletim sistemi görüntüsü yerel geçici diske işaret ediyorsa, görüntü bu disksiz boyutla düzgün çalışmayabilir.

## <a name="have-questions-or-feedback"></a>Sorularınız veya geri bildiriminiz mi var?
[Geri bildirim formunu]( https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRUMzdCQkw0OVVRTldFUUtXSTlLQVBPUkVHSy4u)doldurun. 

## <a name="next-steps"></a>Sonraki adımlar 
Bu belgede, yerel geçici disk ile Azure VM 'lerle ilgili en sık sorulan sorular hakkında daha fazla bilgi edindiniz. Bu VM boyutları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Dv4 ve Dsv4 serisi için belirtimler (Genel Amaçlı Iş yükü)](dv4-dsv4-series.md)
- [Ev4 ve Esv4 serisi için belirtimler (bellek için Iyileştirilmiş Iş yükü)](ev4-esv4-series.md)
