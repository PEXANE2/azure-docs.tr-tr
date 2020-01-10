---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5975f334eae543ea0f6ddc182170ae185ac5397a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75468288"
---
Azure portalını kullanarak Resource Manager dağıtımında bir VNet oluşturmak için aşağıdaki adımları izleyin. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Virtual Network’e Genel Bakış](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Bu VNet’i şirket içi konuma bağlamak (P2S yapılandırması oluşturmaya ek olarak) istiyorsanız şirket içi ağ yöneticinizle bu ağ için özellikle kullanabileceğiniz bir IP adresi aralığı ayırma işlemini koordine etmeniz gerekir. VPN bağlantısının her iki tarafında bir yinelenen adres aralığı varsa, trafik beklediğiniz şekilde yönlendirilmez. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak isterseniz adres alanı diğer sanal ağ ile örtüşemez. Ağ yapılandırmanızı uygun şekilde planlamaya dikkat edin.
>
>

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.  Azure portal menüsünde veya **giriş** sayfasından **kaynak oluştur**' u seçin. **Yeni** sayfası açılır.

2. **Markette ara**' te *sanal ağ* girin ve sonuçlardan **sanal ağ** ' ı seçin.

   ![Sanal ağ kaynağını bul sayfası](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/search-marketplace-for-virtual-network.png "Sanal ağ kaynağını bul sayfası")

3. Sanal Ağ sayfasının en altına doğru, **Bir dağıtım modeli seçin** listesinden **Resource Manager**’ı seçip **Oluştur**’a tıklayın.

   ![Kaynak Yöneticisi seçin](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "Resource Manager’ı seçin")
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür. Otomatik olarak doldurulmuş değerler olabilir. Böyle değerler varsa, değerleri kendi değerlerinizle değiştirin. **Sanal ağ oluştur** sayfası aşağıdaki örneğe benzer şekilde görünür.

   ![Alan doğrulama](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/vnetp2s.png "Alan doğrulama")
5. **Ad**: Sanal Ağınızın adını girin.
6. **Adres alanı**: Adres alanını girin. Eklenecek birden fazla adres alanınız varsa birinci adres alanınızı ekleyin. Sanal ağı oluşturduktan sonra başka adres alanları ekleyebilirsiniz.
7. **Abonelik**: Listelenen aboneliğin doğru olduğunu onaylayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
8. **Kaynak grubu**: Var olan bir kaynak grubunu seçin ya da yeni kaynak grubunuz için bir ad yazarak yeni bir tane oluşturun. Yeni bir grup oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/azure-resource-manager/management/overview.md#resource-groups) sayfasını ziyaret edin.
9. **Konum**: Sanal ağınızın konumunu seçin. Konum bu sanal ağa dağıttığınız kaynakların nerede olacağını belirler.
10. **Alt ağ**: Alt ağ adını ve alt ağ adres aralığını ekleyin. Sanal ağı oluşturduktan sonra başka alt ağlar ekleyebilirsiniz.
11. VNet’inizi panoda kolay bulmak istiyorsanız **Panoya sabitle**’yi seçin ve ardından **Oluştur**’a tıklayın.

    ![Panoya sabitle](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "panoya sabitle")
12. **Oluştur**’a tıkladıktan sonra panonuzda sanal ağınızın ilerleme durumunu yansıtacak bir kutucuk göreceksiniz. Sanal ağ oluşturulurken kutucuk değişir.

    ![Sanal ağ kutucuğu oluşturuluyor](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Sanal ağ kutucuğu oluşturma")
