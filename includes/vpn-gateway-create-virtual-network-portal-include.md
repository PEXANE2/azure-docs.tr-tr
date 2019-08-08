---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8b585a47d3950d232eb3e8047c12ee8949030c95
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780158"
---
Azure portalını kullanarak Resource Manager dağıtımında bir VNet oluşturmak için aşağıdaki adımları izleyin. Bu adımları öğretici olarak kullanıyorsanız **örnek değerleri** kullanın. Bu adımları öğretici olarak uygulamıyorsanız değerleri kendi değerlerinizle değiştirmeyi unutmayın. Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Virtual Network’e Genel Bakış](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Bu VNet’in bir şirket içi konuma bağlanması için şirket içi ağ yöneticinizle bu ağ için özellikle kullanabileceğiniz bir IP adresi aralığı ayırma işlemini koordine etmeniz gerekir. VPN bağlantısının her iki tarafında bir yinelenen adres aralığı varsa, trafik beklediğiniz şekilde yönlendirilmez. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak isterseniz adres alanı diğer sanal ağ ile örtüşemez. Ağ yapılandırmanızı uygun şekilde planlamaya dikkat edin.
>

1. Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.
2. **Kaynak oluştur**’a tıklayın. **Markette ara** alanına 'sanal ağ' yazın. Döndürülen listeden **Sanal ağ**’ı bulun ve tıklayarak **Sanal Ağ** sayfasını açın.
3.           **Oluştur**'a tıklayın. Bu, **sanal ağ oluştur** sayfasını açar.
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür. Aşağıdaki değerleri kullanın:

   - **Ad**: VNet1
   - **Adres alanı**: 10.1.0.0/16
   - **Abonelik**: Listelenen aboneliğin kullanmak istediğiniz bir abonelik olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
   - **Kaynak grubu**: TestRG1 (yeni bir grup oluşturmak için **Yeni oluştur** ' a tıklayın)
   - **Konum**: East US
   - **Alt ağ**: Ön uç
   - **Adres aralığı**: 10.1.0.0/24

   ![Sanal ağ oluştur sayfası](./media/vpn-gateway-create-virtual-network-portal-include/create-virtual-network1.png)
5. DDoS 'ı temel, hizmet uç noktaları devre dışı olarak bırakın ve güvenlik duvarını devre dışı bırakın.
6. VNet oluşturmak için **Oluştur**’a tıklayın.