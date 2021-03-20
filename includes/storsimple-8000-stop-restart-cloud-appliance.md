---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: a01f91a81629800d3f03b907c65f05433b6163e6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93376274"
---
#### <a name="to-stop-and-start-a-cloud-appliance"></a>Bulut gerecini başlatmak ve durdurmak için

1. Bulut gerecini durdurmak için gerecinizin sanal makinesine gidin.
    ![StorSimple Cloud Appliance Sanal Makinesi](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)

2. Komut çubuğundan **Durdur**'a tıklayın.

    ![StorSimple Cloud Appliance sanal makine 2](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın.

    ![StorSimple Cloud Appliance sanal makine 3](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. Bir VM durdurulduğunda serbest bırakılır. Bulut gereci durdurulduğu sırada gerecin durumu **Serbest bırakılıyor** olur. Bulut gereci durdurulduktan sonra gerecin durumu **Durduruldu (serbest bırakıldı)** olur.

    ![StorSimple Cloud Appliance sanal makine 4](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. VM durdurulduktan sonra VM’yi başlatmak için **Başlat**’a (düğme kullanılabilir hale gelir) tıklayın. Bulut gereci başlatıldıktan sonra gerecin durumu **Başlatıldı** olur.

    ![StorSimple Cloud Appliance sanal makine 5](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

Bulut gerecini durdurmak ve başlatmak için aşağıdaki cmdlet'leri kullanın.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-cloud-appliance"></a>Bulut gerecini yeniden başlatmak için

Bulut gerecini yeniden başlatmak için gerecinizin sanal makinesine gidin. Komut çubuğundan **Yeniden Başlat**'a tıklayın. Sorulduğunda yeniden başlatma işlemini onaylayın. Bulut gereci kullanıma hazır olduğunda gerecin durumu **Çalışıyor** olur.

![StorSimple Cloud Appliance sanal makine 6](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

Bulut gerecini yeniden başlatmak için aşağıdaki cmdlet'i kullanın.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

