---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/22/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c538c510eeafacd1596fdeb1b4593919e6236cf0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92487082"
---
1. [Azure Portal](https://portal.azure.com), **arama kaynakları, hizmetler ve docs (G +/)** ' da **yerel ağ geçidi** yazın. Arama sonuçlarında **Market** altında **yerel ağ geçidini** bulun ve seçin. Bu, **yerel ağ geçidi oluştur** sayfasını açar.
1. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

   :::image type="content" source="./media/vpn-gateway-add-local-network-gateway-portal-ip-empty/create-ip.png" alt-text="IP adresi ile yerel ağ geçidi oluşturma":::

   * **Ad:** Yerel ağ geçidi nesneniz için bir ad belirtin.
   * **Uç nokta:** Şirket içi VPN cihazı için uç nokta türünü seçin- **IP adresi** veya **FQDN (tam etki alanı adı)**.
      * **IP adresi**: VPN cihazınız için Internet hizmet sağlayıcınızdan ayrılan statik BIR genel IP ADRESINIZ varsa IP adresi seçeneğini BELIRLEYIN ve IP adresini örnekte gösterildiği gibi girin. Bu, Azure VPN ağ geçidinin bağlanmasını istediğiniz VPN cihazının genel IP adresidir. IP adresini şu anda bilmiyorsanız örnekte gösterilen değerleri kullanabilirsiniz ancak geri dönüp yer tutucu IP adresinizi VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Aksi halde Azure bağlantı kuramaz.
   * **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. *Şirket içi sitenize bağlanmak istiyorsanız, burada, örnekte gösterilen değerleri değil, kendi değerlerinizi kullanın*.
   * **BGP ayarları yapılandır:** Yalnızca BGP’yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
   * **Abonelik:** Doğru aboneliğin gösterildiğinden emin olun.
   * **Kaynak Grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
   * **Konum:** Konum, diğer ayarların **bölgesiyle** aynı olur. Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.

1. Değerleri belirtmeyi tamamladığınızda, sayfanın alt kısmındaki **Oluştur** düğmesini seçerek yerel ağ geçidini oluşturun.
