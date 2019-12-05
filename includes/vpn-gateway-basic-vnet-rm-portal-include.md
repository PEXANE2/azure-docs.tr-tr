---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5bf93980a8be86c77240ab981eb812a738a96204
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74829130"
---
Aşağıdaki adımları izleyerek, Resource Manager dağıtım modelini ve Azure portalı ile bir sanal ağ oluşturabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Bir şirket içi konuma bağlanması sanal ağ için bu sanal ağ için özellikle kullanabileceğiniz bir IP adresi aralığı ayırma işlemini koordine şirket içi ağ yöneticinizle koordine edin. VPN bağlantısının her iki tarafında bir yinelenen adres aralığı varsa trafik beklenmedik bir şekilde yönlendirir. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak isterseniz adres alanı diğer sanal ağ ile örtüşemez. Ağ yapılandırmanızı uygun şekilde planlayın.
>
>

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.  Azure portal menüsünde veya **giriş** sayfasından **kaynak oluştur**' u seçin. **Yeni** sayfası açılır.

2. **Markette ara**' te *sanal ağ* girin ve sonuçlardan **sanal ağ** ' ı seçin.

   ![Sanal ağ kaynağını bul sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/search-marketplace-for-virtual-network.png "Sanal ağ kaynağını bul sayfası")

   **Sanal ağ** sayfası açılır.

3. Gelen **dağıtım modeli seçin** seçin sayfanın alt kısmındaki liste **Resource Manager**ve ardından **Oluştur**. **Sanal ağ oluştur** sayfası açılır.

   ![Sanal ağ oluştur sayfası](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet.png "Sanal ağ oluştur sayfası")

4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girdiğiniz karakterleri doğrulandığında kırmızı ünlem işareti yeşil onay işareti olur. Bazı değerleri kendi değerlerinizle değiştirebildiğiniz autofilled şunlardır:

   - **Ad**: Sanal ağınızın adını girin.

   - **Adres alanı**: Adres alanını girin. Birden fazla adres alanı eklemek için varsa, ilk adres alanınızı buraya girin. Sanal ağ oluşturduktan sonra daha sonra ek adres alanları ekleyebilirsiniz.

   - **Abonelik**: Listelenen aboneliğin doğru olduğunu onaylayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.

   - **Kaynak grubu**: mevcut bir kaynak grubunu seçin veya yeni kaynak grubunuz için bir ad girerek yeni bir tane oluşturun. Yeni bir grup oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).

   - **Konum**: Sanal ağınızın konumunu seçin. Bu sanal ağa dağıttığınız kaynakların nerede yaşayacaksa konumunu belirler.

   - **Alt ağ**: alt ağ Ekle **adı** ve alt ağ **adres aralığı**. Sanal ağ oluşturduktan sonra daha sonra ek alt ağlar ekleyebilirsiniz. 
     
5. **Oluştur**'u seçin.
