---
title: StorSimple 8600 EBOD denetleyicisi değiştirin | Microsoft Dokümanlar
description: StorSimple 8600 aygıtındaki bir veya her iki EBOD denetleyicisini nasıl kaldırıp değiştirebilirsiniz açıklar.
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
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: b05d1f36d1e74b3d915e216676859654fbcbacf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254891"
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>StorSimple cihazınızda bir EBOD denetleyicisi değiştirme

## <a name="overview"></a>Genel Bakış
Bu öğretici, Microsoft Azure StorSimple aygıtınızdaki hatalı bir EBOD denetleyici modülünüzünasıl değiştirilen açıklar. Bir EBOD denetleyici modüllerini değiştirmek için şunları yapmanız gerekir:

* Hatalı EBOD denetleyicisini çıkarın
* Yeni bir EBOD denetleyicisi yükleme

Başlamadan önce aşağıdaki bilgileri göz önünde bulundurun:

* Boş EBOD modülleri kullanılmayan tüm yuvalara takılmalıdır. Bir yuva açık bırakılırsa kasa düzgün soğumaz.
* EBOD denetleyicisi sıcak değiştirilebilir ve kaldırılabilir veya değiştirilebilir. Bir yedek alana kadar başarısız bir modülü çıkarmayın. Değiştirme işlemini başlattığınızda, 10 dakika içinde tamamlamanız gerekir.

> [!IMPORTANT]
> Herhangi bir StorSimple bileşenini kaldırmaya veya değiştirmeye çalışmadan önce, [güvenlik simgesi kurallarını](storsimple-safety.md#safety-icon-conventions) ve diğer güvenlik [önlemlerini](storsimple-safety.md)gözden aldığınızdan emin olun.

## <a name="remove-an-ebod-controller"></a>EBOD denetleyicisi kaldırma
StorSimple cihazınızdaki başarısız EBOD denetleyici modüllerini değiştirmeden önce, diğer EBOD denetleyici modülünün etkin ve çalışır durumda olduğundan emin olun. Aşağıdaki yordam ve tablo, EBOD denetleyici modülünün nasıl kaldırılış edileceğini açıklar.

#### <a name="to-remove-an-ebod-module"></a>EBOD modüllerini kaldırmak için
1. Azure portalı açın.
2. Cihazınıza gidin ve **Ayarlar** > **Donanım ı durumuna**gidin ve etkin EBOD denetleyici modülü için LED durumunun yeşil ve başarısız EBOD denetleyici modülüiçin LED'in kırmızı olduğunu doğrulayın.
3. Başarısız EBOD denetleyici modülünün aygıtın arka sırasında bulun.
4. EBOD denetleyici modülünü sistemden çıkarmadan önce EBOD denetleyici modülünü kontrol örneğeten kabloları çıkarın.
5. Denetleyiciye bağlı Olan EBOD denetleyici modülünün tam SAS bağlantı noktasını not alın. EBOD modüllerini değiştirdikten sonra sistemi bu yapılandırmaya geri yüklemeniz gerekecektir.
   
   > [!NOTE]
   > Genellikle, bu Port A, aşağıdaki **diyagramda Ana bilgisayar** olarak etiketlenir olacaktır.
   
    ![EBOD denetleyicisinin arka düzlemi](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Şekil 1** EBOD modülünün arkası
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Hata LED |
   | 2 |Güç LED |
   | 3 |SAS konektörleri |
   | 4 |SAS LED'ler |
   | 5 |Yalnızca fabrika kullanımı için seri bağlantı noktaları |
   | 6 |Port A (Ana Bilgisayar girişte) |
   | 7 |Port B (Host out) |
   | 8 |Port C (Yalnızca fabrika kullanımı) |

## <a name="install-a-new-ebod-controller"></a>Yeni bir EBOD denetleyicisi yükleme
Aşağıdaki yordam ve tablo, StorSimple cihazınıza bir EBOD denetleyici modülünün nasıl yüklenir olduğunu açıklar.

#### <a name="to-install-an-ebod-controller"></a>EBOD denetleyicisi yüklemek için
1. EBOD aygıtının özellikle arayüz konektörüne zarar olup olup olduğunu kontrol edin. Herhangi bir pim bükülürse yeni EBOD denetleyicisini yüklemeyin.
2. Açık pozisyondaki mandallarla, mandallar devreye girene kadar modülü kasaya kaydırın.
   
    ![EBOD denetleyicisi yükleme](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Şekil 2**  EBOD denetleyici modülünün yüklenmesi
3. Mandalı kapatın. Mandal devreye girerken bir tıklama duymalısınız.
   
    ![EBOD mandalını serbest bırakma](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Şekil 3**  EBOD modül mandalının kapatılması
4. Kabloları yeniden bağlayın. Değiştirmeden önce mevcut olan tam yapılandırmayı kullanın. Kabloların nasıl bağlanılabildiğini öğrenmek için aşağıdaki diyagrama ve tabloya bakın.
   
    ![Güç için 4U cihazınızı kablolayın](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Şekil 4**. Kabloları yeniden bağlama
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Birincil muhafaza |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Denetleyici 0 |
   | 5 |Denetleyici 1 |
   | 6 |EBOD denetleyici 0 |
   | 7 |EBOD denetleyici si1 |
   | 8 |EBOD muhafazası |
   | 9 |Güç Dağıtım Üniteleri |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşen değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

