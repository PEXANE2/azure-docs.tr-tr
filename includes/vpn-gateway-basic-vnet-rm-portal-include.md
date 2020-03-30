---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/03/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d2cf1a2e2ab9cf2d6e35aa12b5b0f8ddc04ad0e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301971"
---
Bu adımları izleyerek Kaynak Yöneticisi dağıtım modeli ve Azure portalı ile bir VNet oluşturabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için [Sanal Ağa genel bakış](../articles/virtual-network/virtual-networks-overview.md)bilgisine bakın.

>[!NOTE]
>Bir sanal ağı, bir bina içi mimarinin parçası olarak kullanırken, bu sanal ağ için özel olarak kullanabileceğiniz bir IP adresi aralığını oymak için şirket içi ağ yöneticinizle koordine ettiğinizden emin olun. VPN bağlantısının her iki tarafında da yinelenen bir adres aralığı varsa, trafik beklenmeyen bir şekilde yönlendirecektir. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak istiyorsanız, adres alanı diğer sanal ağla çakışmaz. Ağ yapılandırmanızı buna göre planlayın.
>
>

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Arama kaynaklarında, hizmette ve dokümanlarda (G+/)** *sanal ağ*yazın.

   ![Sanal Ağ kaynak sayfasını bulma](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace.png "Sanal ağ kaynak sayfasını bulma")
1. **Market** sonuçlarından **Sanal Ağ'ı** seçin.

   ![Sanal ağı seçin](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace-results.png "Sanal ağ kaynak sayfasını bulma")
1. Sanal **Ağ** sayfasında **Oluştur'u**seçin.

   ![sanal ağ sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet-click-create.png "Oluştur’u seçin")
1. **Oluştur'u**seçtiğinizde, **Sanal Ağ Oluştur** sayfası açılır.
1. Temel **Bilgiler** sekmesinde, **Proje ayrıntılarını** ve **Örnek ayrıntılarını** VNet ayarlarını yapılandırın.

   ![Temel bilgiler sekmesi](./media/vpn-gateway-basic-vnet-rm-portal-include/basics.png "Temel bilgiler sekmesi") Alanları doldurduğınızda, alana girdiğiniz karakterler doğrulandığında yeşil bir onay işareti görürsünüz. Bazı değerler otomatik olarak doldurulur ve bunları kendi değerlerinizle değiştirebilirsiniz:

   - **Abonelik**: Listelenen aboneliğin doğru olduğunu onaylayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
   - **Kaynak grubu**: Varolan bir kaynak grubu seçin veya yeni bir kaynak oluşturmak için **yeni oluştur'u** tıklatın. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/management/overview.md#resource-groups).
   - **Ad**: Sanal ağınızın adını girin.
   - **Bölge**: VNet'inizin konumunu seçin. Konum, bu VNet'e dağıttığınız kaynakların nerede yaşayacağını belirler.

1. IP **Adresleri** sekmesinde, değerleri yapılandırın. Aşağıdaki örneklerde gösterilen değerler gösteri amaçlıdır. Bu değerleri, gereksinim duyduğunuz ayarlara göre ayarlayın.

   ![IP adresleri sekmesi](./media/vpn-gateway-basic-vnet-rm-portal-include/addresses.png "IP adresleri sekmesi")  
   - **IPv4 adres alanı**: Varsayılan olarak, bir adres alanı otomatik olarak oluşturulur. Kendi değerlerinizi yansıtacak şekilde ayarlamak için adres alanını tıklatabilirsiniz. Ayrıca ek adres alanları da ekleyebilirsiniz.
   - **IPv6**: Yapılandırmanız IPv6 adres alanı gerektiriyorsa, bu bilgileri girmek için **IPv6 adres alanı ekle** kutusunu seçin.
   - **Alt Ağ**: Varsayılan adres alanını kullanırsanız, varsayılan bir alt ağ otomatik olarak oluşturulur. Adres alanını değiştirirseniz, bir alt ağ eklemeniz gerekir. **Alt ağ** ekle penceresini açmak için + Alt **ağ ekle'yi** seçin. Aşağıdaki ayarları yapılandırın ve değerleri eklemek için **Ekle'yi** seçin:
      - **Subnet adı**: Bu örnekte, alt net "FrontEnd" adını verdik.
      - **Subnet adres aralığı**: Bu alt netin adres aralığı.

1. **Güvenlik** sekmesinde, şu anda varsayılan değerleri bırakın:

   - **DDos koruması**: Temel
   - **Güvenlik Duvarı**: Devre Dışı
1. Sanal ağ ayarlarını doğrulamak için **Gözden Geçir + oluştur'u** seçin.
1. Ayarlar doğrulandıktan sonra **Oluştur'u**seçin.
