---
title: 'Sanal WAN: NVA için sanal hub rota tablosu oluşturma: Azure portalı'
description: Portalı kullanarak trafiği bir ağ sanal cihazına yönlendirmek için sanal WAN sanal hub rota tablosu.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to create a route table using the portal.
ms.openlocfilehash: 0807b535adc45093b439dba5ab8a0ea26b2a0721
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402934"
---
# <a name="create-a-virtual-wan-hub-route-table-for-nvas-azure-portal"></a>NV'ler için Sanal WAN hub rota tablosu oluşturma: Azure portalı

Bu makalede, Sanal WAN hub'ına bağlı bir şubeden (şirket içi site) Ağ Sanal Cihazı (NVA) aracılığıyla Spoke sanal ağına (VNet) nasıl trafik yönlendirilen gösterilmektedir.

![Sanal WAN diyagramı](./media/virtual-wan-route-table/vwanroute.png)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki ölçütleri karşıladığınızı doğrulayın:

*  Bir Ağ Sanal Cihaz (NVA) var. Ağ Sanal Cihaz, genellikle Azure Marketplace'ten sanal ağda sağlanan, seçtiğiniz üçüncü taraf bir yazılımdır.

    * NVA ağ arabirimine özel bir IP adresi atanmalıdır.

    * NVA sanal hub'da dağıtılan değil. Ayrı bir sanal ağda dağıtılmalıdır.

    *  NVA sanal ağının bir veya çok sayıda sanal ağı olabilir. Bu makalede, NVA sanal ağına 'dolaylı vnet' olarak atıfta bulunuyoruz. Bu sanal ağlar VNet eşleme kullanılarak NVA VNet'e bağlanabilir. VNet Peering bağlantıları VNet 1, VNet 2 ve NVA VNet arasındaki yukarıdaki şekilde siyah oklarla gösterilmiştir.
*  İki sanal ağ oluşturdunuz. Onlar vnets konuştu olarak kullanılacaktır.

    * VNet konuşan adres boşlukları şunlardır: VNet1: 10.0.2.0/24 ve VNet2: 10.0.3.0/24. Sanal ağ oluşturma hakkında bilgiye ihtiyacınız varsa, [bkz.](../virtual-network/quick-create-portal.md)

    * VNet'lerin hiçbirinde sanal ağ ağ geçidi olmadığından emin olun.

    * VNet'ler ağ geçidi alt ağı gerektirmez.

## <a name="1-sign-in"></a><a name="signin"></a>1. Oturum aç

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

## <a name="2-create-a-virtual-wan"></a><a name="vwan"></a>2. Sanal bir WAN oluşturma

Sanal bir WAN oluşturun. Aşağıdaki örnek değerleri kullanın:

* **Sanal WAN adı:** myVirtualWAN
* **Kaynak grubu:** testRG
* **Yer:** Batı ABD

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="3-create-a-hub"></a><a name="hub"></a>3. Bir hub oluşturma

Hub'ı oluşturun. Aşağıdaki örnek değerleri kullanın:

* **Yer:** Batı ABD
* **İsim:** westushub
* **Hub özel adres alanı:** 10.0.1.0/24

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="4-create-and-apply-a-hub-route-table"></a><a name="route"></a>4. Hub rota tablosu oluşturma ve uygulama

Hub rota tablosuyla hub'ı güncelleştirin. Aşağıdaki örnek değerleri kullanın:

* **Spoke VNet adres boşlukları:** (VNet1 ve VNet2) 10.0.2.0/24 ve 10.0.3.0/24
* **DMZ NVA ağ arayüzü özel IP adresi:** 10.0.4.5

1. Sanal WAN'ınıza gidin.
2. Rota tablosu oluşturmak istediğiniz hub'ı tıklatın.
3. **...**' u tıklatın ve ardından **sanal hub'ı edit'i**tıklatın.
4. Sanal **hub'ı edit** sayfasında aşağı kaydırın ve yönlendirme için onay kutusu **Kullanım tablosunu**seçin.
5. If **hedef öneki sütununda** adres boşluklarını ekleyin. Sonraki **hop sütununa** gönder'de, DMZ NVA ağ arabirimini özel IP adresi ekleyin.
6. Rota tablosu ayarlarıyla hub kaynağını güncelleştirmek için **Onayla'yı** tıklatın.

## <a name="5-create-the-vnet-connections"></a><a name="connections"></a>5. VNet bağlantılarını oluşturma

Her dolaylı kollu VNet'ten (VNet1 ve VNet2) hub'a sanal ağ bağlantısı oluşturun. Bu sanal ağ bağlantıları yukarıdaki şekilde mavi oklar ile gösterilmiştir. Ardından, NVA VNet'ten hub'a (şekildeki siyah ok) bir VNet bağlantısı oluşturun.

 Bu adım için aşağıdaki değerleri kullanabilirsiniz:

| Sanal ağın adı| Bağlantı adı|
| --- | --- |
| VNet1 | test bağlantısı1 |
| VNet2 | test bağlantısı2 |
| NVAVNet | testbağlantısı3 |

Bağlanmak istediğiniz her sanal ağ için aşağıdaki yordamı yineleyin.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantıları**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.
4. Bağlantıyı oluşturmak için **Tamam'ı** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
