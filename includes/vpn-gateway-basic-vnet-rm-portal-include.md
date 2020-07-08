---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/03/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d2cf1a2e2ab9cf2d6e35aa12b5b0f8ddc04ad0e7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78301971"
---
Aşağıdaki adımları izleyerek Kaynak Yöneticisi dağıtım modeliyle ve Azure portal bir sanal ağ oluşturabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Bir şirket içi mimarinin parçası olarak bir sanal ağ kullanırken, bu sanal ağ için özel olarak kullanabileceğiniz bir IP adresi aralığı almak için şirket içi ağ yöneticinizle birlikte koordine ettiğinizden emin olun. VPN bağlantısının her iki tarafında da yinelenen adres aralığı varsa, trafik beklenmedik bir şekilde yönlendirme yapılır. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak istiyorsanız, adres alanı diğer sanal ağla çakışamaz. Ağ yapılandırmanızı uygun şekilde planlayın.
>
>

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Arama kaynakları, hizmet ve belgeler (G +/)**' de *sanal ağ*yazın.

   ![Sanal ağ kaynağını bul sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace.png "Sanal ağ kaynağını bul sayfası")
1. **Market** sonuçlarından **sanal ağ** ' ı seçin.

   ![Sanal ağ seçin](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace-results.png "Sanal ağ kaynağını bul sayfası")
1. **Sanal ağ** sayfasında **Oluştur**' u seçin.

   ![sanal ağ sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet-click-create.png "Oluştur’u seçin")
1. **Oluştur**' u seçtiğinizde, **sanal ağ oluştur** sayfası açılır.
1. **Temel bilgiler** sekmesinde, **proje ayrıntıları** ve **örnek ayrıntıları** VNET ayarları ' nı yapılandırın.

   ![Temel bilgiler sekmesi](./media/vpn-gateway-basic-vnet-rm-portal-include/basics.png "Temel bilgiler sekmesi") Alanları doldururken, alana girdiğiniz karakterler doğrulandığında yeşil bir onay işareti görürsünüz. Bazı değerler, kendi değerlerinizle değiştirmek için tekrar doldurulur.

   - **Abonelik**: Listelenen aboneliğin doğru olduğunu onaylayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
   - **Kaynak grubu**: var olan bir kaynak grubunu seçin veya yeni bir tane oluşturmak Için **Yeni oluştur** ' a tıklayın. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/management/overview.md#resource-groups).
   - **Ad**: Sanal ağınızın adını girin.
   - **Bölge**: VNET 'iniz için konumu seçin. Konum, bu sanal ağa dağıttığınız kaynakların canlı olacağını belirler.

1. **IP adresleri** sekmesinde değerlerini yapılandırın. Aşağıdaki örneklerde gösterilen değerler tanıtım amaçlıdır. Bu değerleri, ihtiyacınız olan ayarlara göre ayarlayın.

   ![IP adresleri sekmesi](./media/vpn-gateway-basic-vnet-rm-portal-include/addresses.png "IP adresleri sekmesi")  
   - **IPv4 adres alanı**: varsayılan olarak, bir adres alanı otomatik olarak oluşturulur. Adres alanına tıklayarak kendi değerlerinizi yansıtacak şekilde ayarlayabilirsiniz. Ayrıca, ek adres alanları da ekleyebilirsiniz.
   - **IPv6**: yapılandırmanız IPv6 adres alanı gerektiriyorsa, bu bilgileri girmek için **IPv6 adres alanı Ekle** kutusunu seçin.
   - **Alt ağ**: varsayılan adres alanını kullanırsanız otomatik olarak varsayılan bir alt ağ oluşturulur. Adres alanını değiştirirseniz, bir alt ağ eklemeniz gerekir. **Alt ağ ekle** penceresini açmak için **+ alt ağ ekle** ' yi seçin. Aşağıdaki ayarları yapılandırın ve ardından **Ekle** ' yi seçerek değerleri ekleyin:
      - **Alt ağ adı**: Bu örnekte "ön uç" alt ağını adlandırdık.
      - **Alt ağ adres aralığı**: Bu alt ağın adres aralığı.

1. **Güvenlik** sekmesinde, şu anda varsayılan değerleri bırakın:

   - **DDoS koruması**: temel
   - **Güvenlik duvarı**: devre dışı
1. Sanal ağ ayarlarını doğrulamak için **gözden geçir + oluştur** ' u seçin.
1. Ayarlar doğrulandıktan sonra **Oluştur**' u seçin.
