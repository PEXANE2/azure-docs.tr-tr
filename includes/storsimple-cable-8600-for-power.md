---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9b9922602218280d58331a755ed0dfed7df96f40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188407"
---
#### <a name="to-cable-your-device-for-power"></a>Cihazınızı güç için kablolamak için
> [!NOTE]
> StorSimple cihazınızdaki her iki kasada gereksiz PCM'ler bulunmaktadır. Her kasa için, pcm'lerin yüklü olması ve yüksek kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlanması gerekir.
> 
> 

1. Tüm PCM'lerin güç anahtarlarının KAPALı konumda olduğundan emin olun.
2. Birincil kasada güç kablolarını her iki PC'ye de bağlayın. Güç kabloları aşağıdaki güç kablolama diyagramında kırmızı ile tanımlanır.
3. Birincil kasadaki iki PCM'nin ayrı güç kaynakları kullandığından emin olun.
4. Güç kablolarını güç kablolama diyagramında gösterildiği gibi raf dağıtım ünitelerindeki güce takın.
5. EBOD muhafazası için 2'den 4'e kadar olan adımları tekrarlayın.
6. Her PCM'deki güç düğmesini ANDAKI konuma çevirerek EBOD kasasını açın.
7. EBOD denetleyicisinin arka sideki yeşil LED'lerin açık olup olmadığını kontrol ederek EBOD kasasının açık olduğunu doğrulayın.
8. Her PCM anahtarını AÇI KONUMUna çevirerek birincil muhafazayı açın.
9. Cihaz denetleyici LED'lerinin açık olduğundan emin olarak sistemin açık olduğunu doğrulayın.
10. EBOD denetleyicisi üzerindeki SAS bağlantı noktasının yanındaki dört LED'in yeşil olduğunu doğrulayarak EBOD denetleyicisi ile aygıt denetleyicisi arasındaki bağlantının etkin olduğundan emin olun.
    
    > [!IMPORTANT]
    > Sisteminiz için yüksek kullanılabilirlik sağlamak için, aşağıdaki diyagramda gösterilen güç kablolama düzenine kesinlikle uymanızı öneririz.
    > 
    > 
    
    ![Güç için 4U cihazınızı kablolayın](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Güç kablolama**
    
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
    | 9 |PD'ler |

