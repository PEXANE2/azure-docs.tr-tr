---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: a5a286753e438b7d65f3d33a82669c4f7e79a282
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544148"
---
#### <a name="to-create-public-endpoints-on-the-cloud-appliance"></a>Bulut gerecinde ortak uç noktalar oluşturmak için

1. Azure portalında oturum açın.
2. **Sanal Makineler**’e gidin ve bulut gereciniz olarak kullanılan sanal makineyi seçip tıklayın.
    
3. Sanal makinenize gelen ve giden trafik akışını denetlemek için bir ağ güvenlik grubu (NSG) kuralı oluşturmanız gerekir. NSG kuralı oluşturmak için aşağıdaki adımları gerçekleştirin.
    1. **Ağ güvenlik grubu**’nu seçin.
        ![Sanal makine sayfasının ekran görüntüsü. Ayarlar bölümünde, ağ güvenlik grubu vurgulanır.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Gösterilen varsayılan ağ güvenlik grubunu seçin.
        ![Ağ güvenlik grubu sayfasının ekran görüntüsü. Varsayılan ağ güvenlik grubu vurgulanır.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. **Gelen güvenlik kuralları**’nı seçin.
        ![Varsayılan ağ güvenlik grubunun özelliklerini gösteren bir sayfanın ekran görüntüsü. Gezinti bölmesinde, gelen güvenlik kuralları vurgulanır.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Gelen güvenlik kuralı oluşturmak için **+ Ekle**’ye tıklayın.
        ![Gelen güvenlik kuralları sayfasının ekran görüntüsü. Artı işareti ve sözcük ekleme eklentileri birbirini izleyen ve vurgulanmıştır.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        Gelen güvenlik kuralı ekle dikey penceresinde:

        1. **Ad**için şu uç nokta için aşağıdaki adı yazın: WinRMHttps.
        
        2. **Öncelik** için 1000’den küçük bir sayı seçin (varsayılan kuralın önceliği). Değer ne kadar yüksek olursa öncelik o kadar düşük olur.

        3. **Kaynak** seçeneğini **Herhangi biri** olarak ayarlayın.

        4. **Hizmet** için **WinRM**’yi seçin. **Protokol** seçeneği otomatik olarak **TCP**; **Bağlantı noktası aralığı** ise **5986** olarak ayarlanır.

        5. Kuralı oluşturmak için **Tamam**'a tıklayın.

            ![Gelen güvenlik kuralı ekle dikey penceresinin ekran görüntüsü. Değerler yordamda açıklandığı gibi doldurulur ve Tamam düğmesi vurgulanır.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Gerçekleştirmeniz gereken son adım, ağ güvenlik grubunuzu bir alt ağ ile veya belirli bir ağ arabirimiyle ilişkilendirmektir. Ağ güvenlik grubunuzu bir alt ağla ilişkilendirmek için aşağıdaki adımları gerçekleştirin.
    1. **Alt ağlar**’a gidin.
    2. **+ İlişkilendir**’e tıklayın.
        ![Alt ağlar sayfasının ekran görüntüsü. Artı işareti ve Ilişkilendirme sözcüğü birbirini izleyen ve vurgulanır.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Sanal ağınızı ve ardından uygun alt ağı seçin.
    4. Kuralı oluşturmak için **Tamam**'a tıklayın.

        ![Alt ağı Ilişkilendir sayfasının ekran görüntüsü. Sanal ağ seçilir ve Tamam düğmesi vurgulanır.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

Kural oluşturulduktan sonra, Genel Sanal IP (VIP) adresini belirlemek için kuralın ayrıntılarını görüntüleyebilirsiniz. Bu adresini kaydedin.


