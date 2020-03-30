---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 51e1fd18b52d7e215ba43be540156199fb41778e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188405"
---
#### <a name="to-attach-the-sas-cables"></a>SAS kablolarını takmak için
1. Birincil ve EBOD muhafazalarını tanımlayın. İki muhafaza, kendi arka düzlemlerine bakarak tanımlanabilir. Rehberlik için aşağıdaki resme bakın. 
   
    ![Birincil ve EBOD muhafazalarının arka düzlemi](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Birincil ve EBOD muhafazalarının geri görünümü**
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Birincil muhafaza |
   | 2 |EBOD muhafazası |
2. Birincil ve EBOD muhafazalarında seri numaralarını bulun. Seri numarası etiketi her kasanın arka kulağına yapıştırılır. Seri numaraları her iki kasada da aynı olmalıdır. Seri numaraları eşleşmiyorsa hemen [Microsoft Destek'e başvurun.](../articles/storsimple/storsimple-contact-microsoft-support.md) Seri numaralarını bulmak için aşağıdaki resme bakın.
   
    ![Seri numarasını gösteren kasanın arka görünümü](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Seri numarası etiketinin konumu**
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Kasanın kulağı |
3. EBOD kasasını birincil kasaya aşağıdaki şekilde bağlamak için sağlanan SAS kablolarını kullanın:
   
   1. Birincil kasa ve EBOD muhafazası üzerindeki dört SAS bağlantı noktasını tanımlayın. SAS bağlantı noktaları birincil kasada EBOD olarak etiketlenir ve aşağıdaki SAS kablolama çiziminde gösterildiği gibi EBOD kasasındaki A bağlantı noktasına karşılık gelir.
   2. EBOD bağlantı noktasını A limanına bağlamak için sağlanan SAS kablolarını kullanın.
   3. Denetleyici 0'daki EBOD bağlantı noktası, EBOD denetleyicisi 0'daki A bağlantı noktasına bağlanmalıdır. Denetleyici 1'deki EBOD bağlantı noktası, EBOD denetleyici 1'deki A bağlantı noktasına bağlanmalıdır. Kılavuz için aşağıdaki resme bakın. 
      
      ![Cihazınız için SAS kablolama](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS kablolama**
      
      | Etiketle | Açıklama |
      |:--- |:--- |
      | A |Birincil muhafaza |
      | B |EBOD muhafazası |
      | 1 |Denetleyici 0 |
      | 2 |Denetleyici 1 |
      | 3 |EBOD Denetleyici 0 |
      | 4 |EBOD Denetleyici 1 |
      | 5, 6 |Birincil kasadaki SAS bağlantı noktaları (EBOD etiketli) |
      | 7, 8 |EBOD kasası üzerindeki SAS bağlantı noktaları (Port A) |

