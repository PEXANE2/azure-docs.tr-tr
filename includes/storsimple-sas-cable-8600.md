---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 51e1fd18b52d7e215ba43be540156199fb41778e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188405"
---
#### <a name="to-attach-the-sas-cables"></a>SAS kablolarını eklemek için
1. Birincil ve EBOD kasaları belirler. İki kasa ilgili arka düzlemleri inceleyerek belirlenebilir. Rehberlik için aşağıdaki resme bakın. 
   
    ![Birincil ve EBOD kasaları arka düzlemi](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Birincil ve EBOD kasaları geri görünümü**
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Birincil kutu |
   | 2 |EBOD Kasası |
2. Birincil ve EBOD kasaları üzerindeki seri numaralarını bulun. Seri numarası etiketi her bir kutunun arkasındaki geri doğru bir şekilde yapılır. Seri numaralarının her iki muhafazada aynı olması gerekir. Seri numaraları eşleşmiyorsa hemen [Microsoft desteği başvurun](../articles/storsimple/storsimple-contact-microsoft-support.md) . Seri numaralarını bulmak için aşağıdaki çizime bakın.
   
    ![Kutunun seri numarasını gösteren arka görünümü](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Seri numarası etiketi konumu**
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Kutunun Ear |
3. Aşağıdaki gibi, EBOD Kasası 'nı birincil muhafaza ile bağlamak için, belirtilen SAS kablolarını kullanın:
   
   1. Birincil kasada ve EBOD kasasında dört SAS bağlantı noktasını belirler. SAS bağlantı noktaları, birincil kasadaki EBOD olarak etiketlidir ve aşağıdaki SAS kablolama çiziminde gösterildiği gibi EBOD muhafazasında A bağlantı noktasına karşılık gelir.
   2. EBOD bağlantı noktasını A bağlantı noktasına bağlamak için, belirtilen SAS kablolarını kullanın.
   3. Denetleyici 0 üzerindeki EBOD bağlantı noktası, EBOD denetleyicisi 0 ' A ait bağlantı noktasına bağlanmalıdır. Denetleyici 1 ' deki EBOD bağlantı noktası, EBOD denetleyici 1 ' e ait bağlantı noktasına bağlanmalıdır. Rehberlik için aşağıdaki çizime bakın. 
      
      ![Cihazınız için SAS kablolama](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS kablolama**
      
      | Etiketle | Açıklama |
      |:--- |:--- |
      | A |Birincil kutu |
      | B |EBOD Kasası |
      | 1 |Denetleyici 0 |
      | 2 |Denetleyici 1 |
      | 3 |EBOD denetleyicisi 0 |
      | 4 |EBOD denetleyicisi 1 |
      | 5, 6 |Birincil muhafaza üzerindeki SAS bağlantı noktaları (EBOD etiketli) |
      | 7, 8 |EBOD muhafazasında SAS bağlantı noktaları (bağlantı noktası A) |

