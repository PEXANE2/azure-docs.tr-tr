---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9b9922602218280d58331a755ed0dfed7df96f40
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188407"
---
#### <a name="to-cable-your-device-for-power"></a>Cihazınızı güç kablosu için
> [!NOTE]
> StorSimple cihazınızdaki her iki muhafazada yedekli PCMs vardır. Her kutu için, PCMs 'nin yüklü olması ve yüksek kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlanması gerekir.
> 
> 

1. Tüm PCMs üzerindeki güç anahtarlarının kapalı konumda olduğundan emin olun.
2. Birincil kasada, güç bağıntılarını hem PCMs 'ye bağlayın. Güç kabloları, aşağıdaki güç kablosu diyagramında kırmızı renkle tanımlanır.
3. Birincil kasada iki PCMs 'nin ayrı güç kaynakları kullanmasını sağlayın.
4. Power Cord 'leri, güç kablosu diyagramında gösterildiği gibi raf dağıtım birimlerindeki güce bağlayın.
5. EBOD Kasası için 2 ile 4 arasındaki adımları tekrarlayın.
6. Her PCM üzerindeki güç anahtarını açık konuma getirerek EBOD Kasası 'nı açın.
7. EBOD denetleyicisinin arkasında bulunan yeşil LED 'ler açık olup olmadığını denetleyerek EBOD Kasası 'nın açık olduğunu doğrulayın.
8. Her PCM anahtarını Açık konumuna getirerek birincil Kasası açın.
9. Cihaz denetleyicisi LED 'Leri açık olduğundan emin olarak sistemin açık olduğunu doğrulayın.
10. Ebod denetleyicisi ile cihaz denetleyicisi arasındaki bağlantının, EBOD denetleyicisindeki SAS bağlantı noktasının yanındaki dört LED 'in yeşil olduğunu doğrulayarak etkin olduğundan emin olun.
    
    > [!IMPORTANT]
    > Sisteminiz için yüksek kullanılabilirlik sağlamak amacıyla, aşağıdaki diyagramda gösterilen güç kablolama şemasına kesinlikle uymanızı öneririz.
    > 
    > 
    
    ![4U cihazınızın güç bağlantısını yapın](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Güç kabloları**
    
    | Etiketle | Açıklama |
    |:--- |:--- |
    | 1 |Birincil kutu |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Denetleyici 0 |
    | 5 |Denetleyici 1 |
    | 6 |EBOD denetleyicisi 0 |
    | 7 |EBOD denetleyicisi 1 |
    | 8 |EBOD Kasası |
    | 9 |PDU |

