---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/03/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 059d15090204c2fce0fddd4b80f4954755ea8f65
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89449631"
---
1. [Azure Portal](https://portal.azure.com) menüsünde **kaynak oluştur**' u seçin. 

   ![Azure portalında kaynak oluşturma](./media/vpn-gateway-add-local-network-gateway-portal-include/azure-portal-create-resource.png)
2. Market 'te **Ara** alanına **yerel ağ geçidi**yazın ve aramak için **ENTER** tuşuna basın. Böylece sonuçların bir listesi döndürülür. **Yerel ağ geçidi**’ne ve sonra **Oluştur** düğmesine tıklayarak **Yerel ağ geçidi oluştur** sayfasını açın.

   ![Yerel ağ geçidini oluşturma](./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-network-gateway.png "Yerel ağ geçidini oluşturma")

3. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

   - **Ad:** Yerel ağ geçidi nesneniz için bir ad belirtin.
   - **IP adresi:** Bu değer, Azure'ın bağlanmasını istediğiniz VPN cihazının genel IP adresidir. Geçerli bir genel IP adresi belirtin. IP adresini şu anda bilmiyorsanız örnekte gösterilen değerleri kullanabilirsiniz ancak geri dönüp yer tutucu IP adresinizi VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Aksi halde Azure bağlantı kuramaz.
   - **Adres alanı** , yerel ağ nesnesinin temsil ettiği ağ (Şirket içi ağınız) için adres aralıklarını ifade eder. Şirket içi ağınıza yönlendirmek istediğiniz adres alanlarını eklersiniz. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. *Şirket içi sitenize bağlanmak istiyorsanız, burada, örnekte gösterilen değerleri değil, kendi değerlerinizi kullanın*.
   - **BGP ayarları yapılandır:** Yalnızca BGP’yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
   - **Abonelik:** Doğru aboneliğin gösterildiğinden emin olun.
   - **Kaynak Grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
   - **Konum:** Konum, diğer ayarların **bölgesiyle** aynı olur. Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.

4. Tüm değerleri belirttikten sonra yerel ağ geçidini oluşturmak için sayfanın alt tarafındaki **Oluştur** düğmesine tıklayın.
