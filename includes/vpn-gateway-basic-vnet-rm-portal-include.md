---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/25/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 872ba86c9e43b1f6642331908eb829605f6c19bd
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77619660"
---
Aşağıdaki adımları izleyerek, Resource Manager dağıtım modelini ve Azure portalı ile bir sanal ağ oluşturabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Bir şirket içi konuma bağlanması sanal ağ için bu sanal ağ için özellikle kullanabileceğiniz bir IP adresi aralığı ayırma işlemini koordine şirket içi ağ yöneticinizle koordine edin. VPN bağlantısının her iki tarafında bir yinelenen adres aralığı varsa trafik beklenmedik bir şekilde yönlendirir. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak isterseniz adres alanı diğer sanal ağ ile örtüşemez. Ağ yapılandırmanızı uygun şekilde planlayın.
>
>

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Arama kaynakları, hizmet ve belgeler (G +/)** ' de *sanal ağ*yazın.

   ![Sanal ağ kaynağını bul sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace.png "Sanal ağ kaynağını bul sayfası")
1. **Market** sonuçlarından **sanal ağ** ' ı seçin.

   ![Sanal ağ seçin](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace-results.png "Sanal ağ kaynağını bul sayfası")
1. **Sanal ağ** sayfasında **Oluştur**' a tıklayın.

   ![sanal ağ sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet-click-create.png "Oluştur’a tıklayın")
1. Oluştur ' a tıkladığınızda, **sanal ağ oluştur** sayfası açılır.

   ![Sanal ağ oluştur sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/create-virtual-network-page.png "Sanal ağ oluştur sayfası")
1. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girdiğiniz karakterleri doğrulandığında kırmızı ünlem işareti yeşil onay işareti olur. Bazı değerleri kendi değerlerinizle değiştirebildiğiniz autofilled şunlardır:

   - **Ad**: Sanal ağınızın adını girin.
   - **Adres alanı**: Adres alanını girin. Birden fazla adres alanı eklemek için varsa, ilk adres alanınızı buraya girin. Sanal ağ oluşturduktan sonra daha sonra ek adres alanları ekleyebilirsiniz. Yapılandırmanız IPv6 adres alanı gerektiriyorsa, bu bilgileri girmek için onay kutusunu işaretleyin.
   - **Abonelik**: Listelenen aboneliğin doğru olduğunu onaylayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
   - **Kaynak grubu**: var olan bir kaynak grubunu seçin veya yeni kaynak grubunuz için bir ad girerek yeni bir tane oluşturun. Yeni bir grup oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../articles/azure-resource-manager/management/overview.md#resource-groups).
   - **Konum**: Sanal ağınızın konumunu seçin. Bu sanal ağa dağıttığınız kaynakların nerede yaşayacaksa konumunu belirler.
   - **Alt ağ**: alt ağ **adını** ve alt ağ **adres aralığını**ekleyin. Sanal ağ oluşturduktan sonra daha sonra ek alt ağlar ekleyebilirsiniz.
   - **DDoS koruması**: Standart hizmeti kullanmak Istemediğiniz müddetçe **temel**' yı seçin.
   - **Hizmet uç noktaları**: yapılandırmanız bu ayarı belirtmediği sürece bu ayarı **devre dışı**olarak bırakabilirsiniz.
   - **Güvenlik duvarı**: yapılandırmanız bu ayarı belirtmediği sürece bu ayarı **devre dışı**olarak bırakabilirsiniz.
1. Sanal ağ dağıtımına başlamak için **Oluştur** ' a tıklayın.
