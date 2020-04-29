---
title: 'Azure ExpressRoute: sanal ağa ağ geçidi ekleme: Portal'
description: Bu makalede, Azure portal kullanarak ExpressRoute için zaten oluşturulmuş bir Kaynak Yöneticisi VNet 'e bir sanal ağ geçidi ekleme işlemi adım adım açıklanmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 87b656f0ef999b3b15a89476f5cba4c4fcfc2b1e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79264836"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-azure-portal"></a>Azure portal kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi Azure portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik-PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede, önceden var olan VNet için sanal ağ geçidi ekleme adımlarında izlenecek yol gösterilmektedir. Bu makalede, önceden var olan VNet için bir sanal ağ (VNet) ağ geçidi ekleme, yeniden boyutlandırma ve kaldırma adımlarında adım adım gösterilmektedir. Bu yapılandırmaya yönelik adımlar, bir ExpressRoute yapılandırmasında kullanılacak Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan sanal ağlar içindir. ExpressRoute için sanal ağ geçitleri ve ağ geçidi yapılandırma ayarları hakkında daha fazla bilgi için bkz. [ExpressRoute için sanal ağ geçitleri hakkında](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Başlamadan önce

Bu görevin adımları aşağıdaki yapılandırma başvurusu listesindeki değerleri temel alarak bir sanal ağ kullanır. Bu listeyi örnek adımlarımızda kullanırız. Bir başvuru olarak kullanmak için listeyi kopyalayabilir ve değerleri kendi değerlerinizle değiştirin.

**Yapılandırma başvuru listesi**

* Sanal ağ adı = "TestVNet"
* Sanal ağ adres alanı = 192.168.0.0/16
* Alt ağ adı = "ön uç" 
    * Alt ağ adres alanı = "192.168.1.0/24"
* Kaynak grubu = "TestRG"
* Konum = "Doğu ABD"
* Ağ geçidi alt ağ adı: "GatewaySubnet" her zaman bir ağ geçidi alt ağı *gatewaysubnet*adını vermelisiniz.
    * Ağ geçidi alt ağ adres alanı = "192.168.200.0/26"
* Ağ geçidi adı = "ERGW"
* Ağ Geçidi genel IP adı = "Myergwvıp"
* Ağ Geçidi türü = "ExpressRoute" Bu tür bir ExpressRoute yapılandırması için gereklidir.

Yapılandırmanıza başlamadan önce bu adımların bir [videosunu](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) görüntüleyebilirsiniz.

## <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. [Portalda](https://portal.azure.com), sanal ağ geçidini oluşturmak istediğiniz kaynak yöneticisi sanal ağa gidin.
2. VNet dikey pencerenizin **Ayarlar** bölümünde, Alt Ağlar dikey penceresini genişletmek için **Alt Ağlar**'a tıklayın.
3. **Alt ağlar** dikey penceresinde, **+Alt ağ**’a tıklayarak **Alt ağ ekle** dikey penceresini açın. 
   
    ![Ağ geçidi alt ağı ekleme](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "Ağ geçidi alt ağı ekleme")


4. Alt ağınız için **Ad** alanı otomatik olarak ‘GatewaySubnet’ değeriyle doldurulur. Alt ağın Azure tarafından ağ geçidi alt ağı olarak tanınması için bu değer gereklidir. Otomatik olarak doldurulmuş **Adres aralığı** değerlerini, yapılandırma gereksinimlerinize uyacak şekilde ayarlayın. /27 veya daha büyük (/26,/25 vb.) bir ağ geçidi alt ağı oluşturmanızı öneririz. Ardından, değerleri kaydetmek ve ağ geçidi alt ağını oluşturmak için **Tamam** ' ı tıklatın.

    ![Alt ağ ekleme](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "Alt ağ ekleme")

## <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Portalda, sol tarafta, ara ' ya tıklayın **+** ve ' sanal ağ geçidi ' yazın. Arama sonuçlarında **Sanal ağ geçidi** seçeneğini bulun ve girişe tıklayın. **Sanal ağ geçidi** dikey penceresinin alt kısmındaki **Oluştur**’a tıklayın. Bu işlem **Sanal ağ geçidi oluştur** dikey penceresini açar.
2. **Sanal ağ geçidi oluştur** dikey penceresinde, sanal ağ geçidinize ait değerleri girin.

    ![Sanal ağ geçidi oluştur dikey penceresinin alanları](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Sanal ağ geçidi oluştur dikey penceresinin alanları")
3. **Ad**: Ağ geçidinizi adlandırın. Bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
4. **Ağ geçidi türü**: **ExpressRoute**' ı seçin.
5. **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin.
6. **Konum**: **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Konum, sanal ağınızın bulunduğu bölgeye işaret etmiyorsa, 'Sanal ağ seçin' açılır menüsünde sanal ağ görüntülenmez.
7. Bu ağ geçidini eklemek istediğiniz sanal ağı seçin. Sanal **ağ** ' **a tıklayarak sanal ağ seçin** dikey penceresini açın. VNet'i seçin. VNet 'iniz görmüyorsanız, **konum** alanının, sanal ağınızın bulunduğu bölgeye işaret ettiğinden emin olun.
9. Genel bir IP adresi seçin. **Genel IP adresi**'ne tıklayarak **Genel IP adresi seçin** dikey penceresini açın. **+Yeni Oluştur**'a tıklayarak **Genel IP adresi oluştur** dikey penceresini açın. Genel IP adresiniz için bir ad girin. Bu dikey pencere bir genel IP adresi nesnesi oluşturur ve daha sonra bu nesneye dinamik olarak bir genel IP adresi atanır. Bu dikey penceredeki değişiklikleri kaydetmek için **Tamam**’a tıklayın.
10. **Abonelik**: Doğru aboneliğin seçildiğini doğrulayın.
11. **Kaynak grubu**: Bu ayar, seçtiğiniz Sanal Ağ tarafından belirlenir.
12. Önceki ayarları belirttikten sonra **Konum**'u ayarlamayın.
13. Ayarları doğrulayın. Ağ geçidinizin panoda görünmesini istiyorsanız dikey pencerenin altında yer alan **Panoya sabitle** seçeneğini belirleyebilirsiniz.
14. Ağ geçidi oluşturmaya başlamak için **Oluştur**’a tıklayın. Ayarlar doğrulanır ve ağ geçidi dağıtılır. Sanal ağ geçidi oluşturma işleminin tamamlanması 45 dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra sanal ortamınızı bir ExpressRoute devresine bağlayabilirsiniz. Bkz. [sanal ağı bir ExpressRoute devresine bağlama](expressroute-howto-linkvnet-portal-resource-manager.md).
