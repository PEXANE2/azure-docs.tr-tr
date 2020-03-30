---
title: "Azure ExpressRoute: VNet'e ağ geçidi ekleme: portal"
description: Bu makale, Azure portalını kullanarak ExpressRoute için zaten oluşturulmuş bir Kaynak Yöneticisi VNet'e sanal ağ ağ geçidi eklemenize yardımcı olur.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 87b656f0ef999b3b15a89476f5cba4c4fcfc2b1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264836"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-azure-portal"></a>Azure portalını kullanarak ExpressRoute için sanal ağ ağ geçidini yapılandırma
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi - Azure portalı](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makale, önceden varolan bir VNet için sanal ağ ağ geçidi eklemek için adımlar boyunca size yol açar. Bu makale, önceden varolan bir VNet için sanal ağ (VNet) ağ geçidi ekleme, yeniden boyutlandırma ve kaldırma adımlarını size iletir. Bu yapılandırma için adımlar, expressroute yapılandırmasında kullanılacak Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan VNet'ler için özel olarak kullanılır. ExpressRoute için sanal ağ ağ geçitleri ve ağ geçidi yapılandırma ayarları hakkında daha fazla bilgi için [ExpressRoute için sanal ağ ağ geçitleri hakkında](expressroute-about-virtual-network-gateways.md)bilgi için bkz. 


## <a name="before-beginning"></a>Başlamadan önce

Bu görevin adımları, aşağıdaki yapılandırma başvuru listesindeki değerleri temel alan bir VNet kullanır. Bu listeyi örnek adımlarımızda kullanırız. Listeyi başvuru olarak kullanmak üzere kopyalayarak değerleri kendi değerlerinideğiştirebilirsiniz.

**Yapılandırma başvuru listesi**

* Sanal Ağ Adı = "TestVNet"
* Sanal Ağ adres alanı = 192.168.0.0/16
* Alt ağ adı = "FrontEnd" 
    * Alt net adres alanı = "192.168.1.0/24"
* Kaynak Grubu = "TestRG"
* Yer = "Doğu ABD"
* Ağ Geçidi Subnet adı: "GatewaySubnet" Her zaman bir ağ geçidi alt ağ *GatewaySubnet*adlandırmanız gerekir.
    * Ağ Geçidi Subnet adres alanı = "192.168.200.0/26"
* Ağ Geçidi Adı = "ERGW"
* Ağ Geçidi Genel IP Adı = "MyERGWVIP"
* Ağ geçidi türü = "ExpressRoute" Bu tür bir ExpressRoute yapılandırması için gereklidir.

Yapılandırmanıza başlamadan önce bu [adımların](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) videosunu görüntüleyebilirsiniz.

## <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. [Portalda,](https://portal.azure.com)sanal ağ ağ geçidi oluşturmak istediğiniz Kaynak Yöneticisi sanal ağına gidin.
2. VNet dikey pencerenizin **Ayarlar** bölümünde, Alt Ağlar dikey penceresini genişletmek için **Alt Ağlar**'a tıklayın.
3. **Alt ağlar** dikey penceresinde, **+Alt ağ**’a tıklayarak **Alt ağ ekle** dikey penceresini açın. 
   
    ![Ağ geçidi alt ağı ekleme](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "Ağ geçidi alt ağı ekleme")


4. Alt ağınız için **Ad** alanı otomatik olarak ‘GatewaySubnet’ değeriyle doldurulur. Alt ağın Azure tarafından ağ geçidi alt ağı olarak tanınması için bu değer gereklidir. Otomatik olarak doldurulmuş **Adres aralığı** değerlerini, yapılandırma gereksinimlerinize uyacak şekilde ayarlayın. /27 veya daha büyük (/26, /25, vb.) içeren bir ağ geçidi alt ağı oluşturmanızı öneririz. Ardından, değerleri kaydetmek ve ağ geçidi alt ağını oluşturmak için **Tamam'ı** tıklatın.

    ![Alt ağ ekleme](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "Alt ağ ekleme")

## <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Portalda, sol tarafta, aramada 'Sanal Ağ Ağ Ağ Geçidi'ni tıklatın **+** ve yazın. Arama sonuçlarında **Sanal ağ geçidi** seçeneğini bulun ve girişe tıklayın. **Sanal ağ geçidi** dikey penceresinin alt kısmındaki **Oluştur**’a tıklayın. Bu işlem **Sanal ağ geçidi oluştur** dikey penceresini açar.
2. **Sanal ağ geçidi oluştur** dikey penceresinde, sanal ağ geçidinize ait değerleri girin.

    ![Sanal ağ geçidi oluştur dikey penceresinin alanları](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Sanal ağ geçidi oluştur dikey penceresinin alanları")
3. **Ad**: Ağ geçidinizi adlandırın. Bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
4. **Ağ Geçidi türü**: **ExpressRoute'u**seçin.
5. **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin.
6. **Konum**: **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Konum, sanal ağınızın bulunduğu bölgeye işaret etmiyorsa, 'Sanal ağ seçin' açılır menüsünde sanal ağ görüntülenmez.
7. Bu ağ geçidini eklemek istediğiniz sanal ağı seçin. **Sanal ağ** bıçakını seçmek için Sanal **ağa** tıklayın. VNet'i seçin. VNet'inizi görmüyorsanız, **Konum** alanının sanal ağınızın bulunduğu bölgeyi işaret ettiğinden emin olun.
9. Genel bir IP adresi seçin. **Genel IP adresi**'ne tıklayarak **Genel IP adresi seçin** dikey penceresini açın. **+Yeni Oluştur**'a tıklayarak **Genel IP adresi oluştur** dikey penceresini açın. Genel IP adresiniz için bir ad girin. Bu dikey pencere bir genel IP adresi nesnesi oluşturur ve daha sonra bu nesneye dinamik olarak bir genel IP adresi atanır. Bu dikey penceredeki değişiklikleri kaydetmek için **Tamam**’a tıklayın.
10. **Abonelik**: Doğru aboneliğin seçildiğini doğrulayın.
11. **Kaynak grubu**: Bu ayar, seçtiğiniz Sanal Ağ tarafından belirlenir.
12. Önceki ayarları belirttikten sonra **Konum**'u ayarlamayın.
13. Ayarları doğrulayın. Ağ geçidinizin panoda görünmesini istiyorsanız dikey pencerenin altında yer alan **Panoya sabitle** seçeneğini belirleyebilirsiniz.
14. Ağ geçidi oluşturmaya başlamak için **Oluştur**’a tıklayın. Ayarlar doğrulanır ve ağ geçidi dağıtılır. Sanal ağ ağ geçidi oluşturmanın tamamlanması 45 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra, VNet'inizi bir ExpressRoute devresine bağlayabilirsiniz. Bkz. [Bir Sanal Ağı Bir ExpressRoute devresine bağla.](expressroute-howto-linkvnet-portal-resource-manager.md)
